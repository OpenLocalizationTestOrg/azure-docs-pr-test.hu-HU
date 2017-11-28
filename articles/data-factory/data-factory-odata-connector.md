---
title: "Az OData forrásból származó adatok áthelyezése |} Microsoft Docs"
description: "További tudnivalók az Azure Data Factory használatával OData forrásból származó áthelyezni az adatokat."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: de28fa56-3204-4546-a4df-21a21de43ed7
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: 624b6c8f317477d83539392c6c2f15c2dc69d401
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-from-a-odata-source-using-azure-data-factory"></a><span data-ttu-id="e8b46-103">Helyezi át az adatokat a egy OData-forrásra Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="e8b46-103">Move data From a OData source using Azure Data Factory</span></span>
<span data-ttu-id="e8b46-104">Ez a cikk ismerteti, hogyan a másolási tevékenység során az Azure Data Factoryben az adatok mozgatása egy OData-forrásra.</span><span class="sxs-lookup"><span data-stu-id="e8b46-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an OData source.</span></span> <span data-ttu-id="e8b46-105">Buildekről nyújtanak a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amelynek során adatátvitel a másolási tevékenység az általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="e8b46-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="e8b46-106">Adatok bármely támogatott fogadó adattárolóhoz másolhat egy OData-forrásra.</span><span class="sxs-lookup"><span data-stu-id="e8b46-106">You can copy data from an OData source to any supported sink data store.</span></span> <span data-ttu-id="e8b46-107">A másolási tevékenység által támogatott mosdók adattárolókhoz listájáért lásd: a [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="e8b46-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="e8b46-108">Adat-előállító jelenleg csak áthelyezése adatokat OData forrásból származó más adattárolókhoz, de nem az egyéb adattárakhoz adatok áthelyezése egy OData-forrásra.</span><span class="sxs-lookup"><span data-stu-id="e8b46-108">Data factory currently supports only moving data from an OData source to other data stores, but not for moving data from other data stores to an OData source.</span></span> 

## <a name="supported-versions-and-authentication-types"></a><span data-ttu-id="e8b46-109">Támogatott verziók és hitelesítési típusok</span><span class="sxs-lookup"><span data-stu-id="e8b46-109">Supported versions and authentication types</span></span>
<span data-ttu-id="e8b46-110">Az OData-összekötő 3.0 és 4.0-s verzióját, és mindkét felhőbeli OData adatait és a helyszíni OData források másolhatja OData-verziót támogatja.</span><span class="sxs-lookup"><span data-stu-id="e8b46-110">This OData connector support OData version 3.0 and 4.0, and you can copy data from both cloud OData and on-premises OData sources.</span></span> <span data-ttu-id="e8b46-111">Az utóbbit telepíteni szeretné az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="e8b46-111">For the latter, you need to install the Data Management Gateway.</span></span> <span data-ttu-id="e8b46-112">Lásd: [helyezze át az adatokat a helyszíni és a felhő között](data-factory-move-data-between-onprem-and-cloud.md) szóló cikkben olvashat az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="e8b46-112">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for details about Data Management Gateway.</span></span>

<span data-ttu-id="e8b46-113">Alább hitelesítési típusok támogatottak:</span><span class="sxs-lookup"><span data-stu-id="e8b46-113">Below authentication types are supported:</span></span>

* <span data-ttu-id="e8b46-114">Hozzáférés **felhő** OData-adatcsatorna, használhatja, hogy a névtelen, alapszintű (felhasználónév és jelszó), vagy az Azure Active Directory-alapú OAuth-hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="e8b46-114">To access **cloud** OData feed, you can use anonymous, basic (user name and password), or Azure Active Directory based OAuth authentication.</span></span>
* <span data-ttu-id="e8b46-115">Hozzáférés **helyszíni** OData-adatcsatorna, használja a névtelen, alapszintű (felhasználónév és jelszó), vagy a Windows-hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="e8b46-115">To access **on-premises** OData feed, you can use anonymous, basic (user name and password), or Windows authentication.</span></span>

## <a name="getting-started"></a><span data-ttu-id="e8b46-116">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="e8b46-116">Getting started</span></span>
<span data-ttu-id="e8b46-117">A másolási tevékenység, amely helyezi át az adatokat OData forrásból származó különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="e8b46-117">You can create a pipeline with a copy activity that moves data from an OData source by using different tools/APIs.</span></span>

<span data-ttu-id="e8b46-118">Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="e8b46-118">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="e8b46-119">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) létrehozásával egy folyamatot, az adatok másolása varázsló segítségével gyorsan útmutatást.</span><span class="sxs-lookup"><span data-stu-id="e8b46-119">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="e8b46-120">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="e8b46-120">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="e8b46-121">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="e8b46-121">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="e8b46-122">Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban:</span><span class="sxs-lookup"><span data-stu-id="e8b46-122">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="e8b46-123">Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.</span><span class="sxs-lookup"><span data-stu-id="e8b46-123">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="e8b46-124">Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="e8b46-124">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="e8b46-125">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="e8b46-125">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="e8b46-126">A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="e8b46-126">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="e8b46-127">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások a JSON formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="e8b46-127">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="e8b46-128">Egy minta, amely segítségével másolja az adatokat OData forrásból származó adat-előállító entitások JSON-definíciók, lásd: [JSON-példa: adatok másolása az OData-forrásra az Azure Blob](#json-example-copy-data-from-odata-source-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="e8b46-128">For a sample with JSON definitions for Data Factory entities that are used to copy data from an OData source, see [JSON example: Copy data from OData source to Azure Blob](#json-example-copy-data-from-odata-source-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="e8b46-129">A következő szakaszok részletesen bemutatják az OData-forrásra megadása a Data Factory tartozó entitások használt JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="e8b46-129">The following sections provide details about JSON properties that are used to define Data Factory entities specific to OData source:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="e8b46-130">Csatolt szolgáltatás tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="e8b46-130">Linked Service properties</span></span>
<span data-ttu-id="e8b46-131">A következő táblázat a JSON-elemek szerepelnek kapcsolódó OData-szolgáltatásra vonatkozó leírást.</span><span class="sxs-lookup"><span data-stu-id="e8b46-131">The following table provides description for JSON elements specific to OData linked service.</span></span>

| <span data-ttu-id="e8b46-132">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e8b46-132">Property</span></span> | <span data-ttu-id="e8b46-133">Leírás</span><span class="sxs-lookup"><span data-stu-id="e8b46-133">Description</span></span> | <span data-ttu-id="e8b46-134">Szükséges</span><span class="sxs-lookup"><span data-stu-id="e8b46-134">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e8b46-135">type</span><span class="sxs-lookup"><span data-stu-id="e8b46-135">type</span></span> |<span data-ttu-id="e8b46-136">A type tulajdonságot kell beállítani: **OData**</span><span class="sxs-lookup"><span data-stu-id="e8b46-136">The type property must be set to: **OData**</span></span> |<span data-ttu-id="e8b46-137">Igen</span><span class="sxs-lookup"><span data-stu-id="e8b46-137">Yes</span></span> |
| <span data-ttu-id="e8b46-138">URL-címe</span><span class="sxs-lookup"><span data-stu-id="e8b46-138">url</span></span> |<span data-ttu-id="e8b46-139">Az OData szolgáltatási URL-címét.</span><span class="sxs-lookup"><span data-stu-id="e8b46-139">Url of the OData service.</span></span> |<span data-ttu-id="e8b46-140">Igen</span><span class="sxs-lookup"><span data-stu-id="e8b46-140">Yes</span></span> |
| <span data-ttu-id="e8b46-141">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="e8b46-141">authenticationType</span></span> |<span data-ttu-id="e8b46-142">Az OData-forrásra való kapcsolódáshoz használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="e8b46-142">Type of authentication used to connect to the OData source.</span></span> <br/><br/> <span data-ttu-id="e8b46-143">A felhőbeli OData a lehetséges értékek: névtelen, alapszintű és OAuth (Megjegyzés: jelenleg csak Azure Data Factory támogatási Azure Active Directory-alapú OAuth).</span><span class="sxs-lookup"><span data-stu-id="e8b46-143">For cloud OData, possible values are Anonymous, Basic, and OAuth (note Azure Data Factory currently only support Azure Active Directory based OAuth).</span></span> <br/><br/> <span data-ttu-id="e8b46-144">A helyszíni OData lehetséges értékei a névtelen, alapszintű és a Windows.</span><span class="sxs-lookup"><span data-stu-id="e8b46-144">For on-premises OData, possible values are Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="e8b46-145">Igen</span><span class="sxs-lookup"><span data-stu-id="e8b46-145">Yes</span></span> |
| <span data-ttu-id="e8b46-146">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="e8b46-146">username</span></span> |<span data-ttu-id="e8b46-147">Ha egyszerű hitelesítést használ, adja meg a felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="e8b46-147">Specify user name if you are using Basic authentication.</span></span> |<span data-ttu-id="e8b46-148">Igen (Ha csak az egyszerű hitelesítés használata esetén)</span><span class="sxs-lookup"><span data-stu-id="e8b46-148">Yes (only if you are using Basic authentication)</span></span> |
| <span data-ttu-id="e8b46-149">jelszó</span><span class="sxs-lookup"><span data-stu-id="e8b46-149">password</span></span> |<span data-ttu-id="e8b46-150">Adja meg a felhasználónévhez megadott felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="e8b46-150">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="e8b46-151">Igen (Ha csak az egyszerű hitelesítés használata esetén)</span><span class="sxs-lookup"><span data-stu-id="e8b46-151">Yes (only if you are using Basic authentication)</span></span> |
| <span data-ttu-id="e8b46-152">authorizedCredential</span><span class="sxs-lookup"><span data-stu-id="e8b46-152">authorizedCredential</span></span> |<span data-ttu-id="e8b46-153">Ha OAuth használ, kattintson a **engedélyezés** gombra a Data Factory másolása varázsló vagy a szerkesztőben, majd adja meg a hitelesítő adatok, akkor ez a tulajdonság értékének lesz automatikusan generált.</span><span class="sxs-lookup"><span data-stu-id="e8b46-153">If you are using OAuth, click **Authorize** button in the Data Factory Copy Wizard or Editor and enter your credential, then the value of this property will be auto-generated.</span></span> |<span data-ttu-id="e8b46-154">Igen (csak ha OAuth-hitelesítés használata esetén)</span><span class="sxs-lookup"><span data-stu-id="e8b46-154">Yes (only if you are using OAuth authentication)</span></span> |
| <span data-ttu-id="e8b46-155">gatewayName</span><span class="sxs-lookup"><span data-stu-id="e8b46-155">gatewayName</span></span> |<span data-ttu-id="e8b46-156">Az átjáró, amely a Data Factory szolgáltatásnak csatlakoznia a helyszíni OData-szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="e8b46-156">Name of the gateway that the Data Factory service should use to connect to the on-premises OData service.</span></span> <span data-ttu-id="e8b46-157">Csak adja meg, ha a másolt adatokat a helyszíni OData-forrásra.</span><span class="sxs-lookup"><span data-stu-id="e8b46-157">Specify only if you are copying data from on-prem OData source.</span></span> |<span data-ttu-id="e8b46-158">Nem</span><span class="sxs-lookup"><span data-stu-id="e8b46-158">No</span></span> |

### <a name="using-basic-authentication"></a><span data-ttu-id="e8b46-159">Alapszintű hitelesítést használó</span><span class="sxs-lookup"><span data-stu-id="e8b46-159">Using Basic authentication</span></span>
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Basic",
            "username": "username",
            "password": "password"
        }
    }
}
```

### <a name="using-anonymous-authentication"></a><span data-ttu-id="e8b46-160">A névtelen hitelesítés segítségével</span><span class="sxs-lookup"><span data-stu-id="e8b46-160">Using Anonymous authentication</span></span>
```json
{
    "name": "ODataLinkedService",
        "properties":
    {
        "type": "OData",
        "typeProperties":
        {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        }
    }
}
```

### <a name="using-windows-authentication-accessing-on-premises-odata-source"></a><span data-ttu-id="e8b46-161">Windows-hitelesítéssel fér hozzá a helyszíni OData-forrásra</span><span class="sxs-lookup"><span data-stu-id="e8b46-161">Using Windows authentication accessing on-premises OData source</span></span>
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "<endpoint of on-premises OData source e.g. Dynamics CRM>",
            "authenticationType": "Windows",
            "username": "domain\\user",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-oauth-authentication-accessing-cloud-odata-source"></a><span data-ttu-id="e8b46-162">OAuth hitelesítési elérése során a felhővel OData-forrásra</span><span class="sxs-lookup"><span data-stu-id="e8b46-162">Using OAuth authentication accessing cloud OData source</span></span>
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "<endpoint of cloud OData source e.g. https://<tenant>.crm.dynamics.com/XRMServices/2011/OrganizationData.svc>",
            "authenticationType": "OAuth",
            "authorizedCredential": "<auto generated by clicking the Authorize button on UI>"
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="e8b46-163">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="e8b46-163">Dataset properties</span></span>
<span data-ttu-id="e8b46-164">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="e8b46-164">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="e8b46-165">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="e8b46-165">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="e8b46-166">A **typeProperties** szakasz eltérő adatkészlet egyes típusai és információkat nyújt azokról az adattárban adatok helyét.</span><span class="sxs-lookup"><span data-stu-id="e8b46-166">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="e8b46-167">A typeProperties szakasz típusú adatkészlet **ODataResource** (amely tartalmazza a OData dataset) a következő tulajdonságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="e8b46-167">The typeProperties section for dataset of type **ODataResource** (which includes OData dataset) has the following properties</span></span>

| <span data-ttu-id="e8b46-168">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e8b46-168">Property</span></span> | <span data-ttu-id="e8b46-169">Leírás</span><span class="sxs-lookup"><span data-stu-id="e8b46-169">Description</span></span> | <span data-ttu-id="e8b46-170">Szükséges</span><span class="sxs-lookup"><span data-stu-id="e8b46-170">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e8b46-171">Elérési út</span><span class="sxs-lookup"><span data-stu-id="e8b46-171">path</span></span> |<span data-ttu-id="e8b46-172">Az OData-erőforrás elérési útja</span><span class="sxs-lookup"><span data-stu-id="e8b46-172">Path to the OData resource</span></span> |<span data-ttu-id="e8b46-173">Nem</span><span class="sxs-lookup"><span data-stu-id="e8b46-173">No</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="e8b46-174">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="e8b46-174">Copy activity properties</span></span>
<span data-ttu-id="e8b46-175">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="e8b46-175">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="e8b46-176">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="e8b46-176">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="e8b46-177">A typeProperties szakaszban a tevékenység tulajdonságai a tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="e8b46-177">Properties available in the typeProperties section of the activity on the other hand vary with each activity type.</span></span> <span data-ttu-id="e8b46-178">A másolási tevékenység során két érték források és mosdók típusától függően.</span><span class="sxs-lookup"><span data-stu-id="e8b46-178">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="e8b46-179">Ha a forrás típusa van **RelationalSource** (amely magában foglalja az OData) a következő tulajdonságok érhetők el typeProperties szakaszában:</span><span class="sxs-lookup"><span data-stu-id="e8b46-179">When source is of type **RelationalSource** (which includes OData) the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="e8b46-180">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e8b46-180">Property</span></span> | <span data-ttu-id="e8b46-181">Leírás</span><span class="sxs-lookup"><span data-stu-id="e8b46-181">Description</span></span> | <span data-ttu-id="e8b46-182">Példa</span><span class="sxs-lookup"><span data-stu-id="e8b46-182">Example</span></span> | <span data-ttu-id="e8b46-183">Szükséges</span><span class="sxs-lookup"><span data-stu-id="e8b46-183">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="e8b46-184">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="e8b46-184">query</span></span> |<span data-ttu-id="e8b46-185">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="e8b46-185">Use the custom query to read data.</span></span> |<span data-ttu-id="e8b46-186">"? $select neve, leírása és $top = = 5"</span><span class="sxs-lookup"><span data-stu-id="e8b46-186">"?$select=Name, Description&$top=5"</span></span> |<span data-ttu-id="e8b46-187">Nem</span><span class="sxs-lookup"><span data-stu-id="e8b46-187">No</span></span> |

## <a name="type-mapping-for-odata"></a><span data-ttu-id="e8b46-188">Az OData-leképezésének</span><span class="sxs-lookup"><span data-stu-id="e8b46-188">Type Mapping for OData</span></span>
<span data-ttu-id="e8b46-189">Ahogyan az a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, a másolási tevékenység az eseményforrás-típusnak a következő kétlépéses módszert típusok gyűjtése automatikus típuskonverziók hajt végre.</span><span class="sxs-lookup"><span data-stu-id="e8b46-189">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following two-step approach.</span></span>

1. <span data-ttu-id="e8b46-190">A natív eseményforrás-típusnak átalakítása .NET-típusa</span><span class="sxs-lookup"><span data-stu-id="e8b46-190">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="e8b46-191">.NET-típus konvertálása natív a fogadó típusa</span><span class="sxs-lookup"><span data-stu-id="e8b46-191">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="e8b46-192">Amikor adatokat OData, a következő megfeleltetéseket szolgálnak az OData-típusok .NET-típusa.</span><span class="sxs-lookup"><span data-stu-id="e8b46-192">When moving data from OData, the following mappings are used from OData types to .NET type.</span></span>

| <span data-ttu-id="e8b46-193">Az OData-adattípus</span><span class="sxs-lookup"><span data-stu-id="e8b46-193">OData Data Type</span></span> | <span data-ttu-id="e8b46-194">.NET-típusa</span><span class="sxs-lookup"><span data-stu-id="e8b46-194">.NET Type</span></span> |
| --- | --- |
| <span data-ttu-id="e8b46-195">Edm.Binary</span><span class="sxs-lookup"><span data-stu-id="e8b46-195">Edm.Binary</span></span> |<span data-ttu-id="e8b46-196">Byte]</span><span class="sxs-lookup"><span data-stu-id="e8b46-196">Byte[]</span></span> |
| <span data-ttu-id="e8b46-197">Edm.Boolean</span><span class="sxs-lookup"><span data-stu-id="e8b46-197">Edm.Boolean</span></span> |<span data-ttu-id="e8b46-198">logikai érték</span><span class="sxs-lookup"><span data-stu-id="e8b46-198">Bool</span></span> |
| <span data-ttu-id="e8b46-199">Edm.Byte</span><span class="sxs-lookup"><span data-stu-id="e8b46-199">Edm.Byte</span></span> |<span data-ttu-id="e8b46-200">Byte]</span><span class="sxs-lookup"><span data-stu-id="e8b46-200">Byte[]</span></span> |
| <span data-ttu-id="e8b46-201">Edm.DateTime</span><span class="sxs-lookup"><span data-stu-id="e8b46-201">Edm.DateTime</span></span> |<span data-ttu-id="e8b46-202">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="e8b46-202">DateTime</span></span> |
| <span data-ttu-id="e8b46-203">Edm.Decimal</span><span class="sxs-lookup"><span data-stu-id="e8b46-203">Edm.Decimal</span></span> |<span data-ttu-id="e8b46-204">Decimális</span><span class="sxs-lookup"><span data-stu-id="e8b46-204">Decimal</span></span> |
| <span data-ttu-id="e8b46-205">Edm.Double</span><span class="sxs-lookup"><span data-stu-id="e8b46-205">Edm.Double</span></span> |<span data-ttu-id="e8b46-206">Dupla</span><span class="sxs-lookup"><span data-stu-id="e8b46-206">Double</span></span> |
| <span data-ttu-id="e8b46-207">Edm.Single</span><span class="sxs-lookup"><span data-stu-id="e8b46-207">Edm.Single</span></span> |<span data-ttu-id="e8b46-208">Egyetlen</span><span class="sxs-lookup"><span data-stu-id="e8b46-208">Single</span></span> |
| <span data-ttu-id="e8b46-209">Edm.Guid</span><span class="sxs-lookup"><span data-stu-id="e8b46-209">Edm.Guid</span></span> |<span data-ttu-id="e8b46-210">GUID</span><span class="sxs-lookup"><span data-stu-id="e8b46-210">Guid</span></span> |
| <span data-ttu-id="e8b46-211">Edm.Int16</span><span class="sxs-lookup"><span data-stu-id="e8b46-211">Edm.Int16</span></span> |<span data-ttu-id="e8b46-212">Int16</span><span class="sxs-lookup"><span data-stu-id="e8b46-212">Int16</span></span> |
| <span data-ttu-id="e8b46-213">Edm.Int32</span><span class="sxs-lookup"><span data-stu-id="e8b46-213">Edm.Int32</span></span> |<span data-ttu-id="e8b46-214">Int32</span><span class="sxs-lookup"><span data-stu-id="e8b46-214">Int32</span></span> |
| <span data-ttu-id="e8b46-215">Edm.Int64</span><span class="sxs-lookup"><span data-stu-id="e8b46-215">Edm.Int64</span></span> |<span data-ttu-id="e8b46-216">Int64</span><span class="sxs-lookup"><span data-stu-id="e8b46-216">Int64</span></span> |
| <span data-ttu-id="e8b46-217">Edm.SByte</span><span class="sxs-lookup"><span data-stu-id="e8b46-217">Edm.SByte</span></span> |<span data-ttu-id="e8b46-218">Int16</span><span class="sxs-lookup"><span data-stu-id="e8b46-218">Int16</span></span> |
| <span data-ttu-id="e8b46-219">Edm.String</span><span class="sxs-lookup"><span data-stu-id="e8b46-219">Edm.String</span></span> |<span data-ttu-id="e8b46-220">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e8b46-220">String</span></span> |
| <span data-ttu-id="e8b46-221">Edm.Time</span><span class="sxs-lookup"><span data-stu-id="e8b46-221">Edm.Time</span></span> |<span data-ttu-id="e8b46-222">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="e8b46-222">TimeSpan</span></span> |
| <span data-ttu-id="e8b46-223">Edm.DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="e8b46-223">Edm.DateTimeOffset</span></span> |<span data-ttu-id="e8b46-224">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="e8b46-224">DateTimeOffset</span></span> |

> [!Note]
> <span data-ttu-id="e8b46-225">OData összetett adattípusok pl. objektum nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="e8b46-225">OData complex data types e.g. Object are not supported.</span></span>

## <a name="json-example-copy-data-from-odata-source-to-azure-blob"></a><span data-ttu-id="e8b46-226">JSON-példa: adatok másolása az OData-forrásra az Azure-Blobba</span><span class="sxs-lookup"><span data-stu-id="e8b46-226">JSON example: Copy data from OData source to Azure Blob</span></span>
<span data-ttu-id="e8b46-227">Ebben a példában a minta JSON-definíciókat tartalmazzon, segítségével hozzon létre egy folyamatot biztosít [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="e8b46-227">This example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="e8b46-228">OData forrásból származó adatok másolása az Azure Blob Storage mutatnak.</span><span class="sxs-lookup"><span data-stu-id="e8b46-228">They show how to copy data from an OData source to an Azure Blob Storage.</span></span> <span data-ttu-id="e8b46-229">Azonban adatok átmásolhatók a megadott mosdók bármelyikét [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a másolási tevékenység során az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="e8b46-229">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span> <span data-ttu-id="e8b46-230">A minta a következő adat-előállító entitások rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="e8b46-230">The sample has the following Data Factory entities:</span></span>

1. <span data-ttu-id="e8b46-231">A társított szolgáltatás típusa [OData](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="e8b46-231">A linked service of type [OData](#linked-service-properties).</span></span>
2. <span data-ttu-id="e8b46-232">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="e8b46-232">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="e8b46-233">Bemeneti [dataset](data-factory-create-datasets.md) típusú [ODataResource](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="e8b46-233">An input [dataset](data-factory-create-datasets.md) of type [ODataResource](#dataset-properties).</span></span>
4. <span data-ttu-id="e8b46-234">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="e8b46-234">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="e8b46-235">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="e8b46-235">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="e8b46-236">A minta másol adatokat óránként egy OData-forrásra egy Azure-blobba lekérdezését.</span><span class="sxs-lookup"><span data-stu-id="e8b46-236">The sample copies data from querying against an OData source to an Azure blob every hour.</span></span> <span data-ttu-id="e8b46-237">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="e8b46-237">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="e8b46-238">**OData társított szolgáltatás:** ebben a példában a névtelen hitelesítést használ.</span><span class="sxs-lookup"><span data-stu-id="e8b46-238">**OData linked service:** This example uses the Anonymous authentication.</span></span> <span data-ttu-id="e8b46-239">Lásd: [OData társított szolgáltatás](#linked-service-properties) szakasz a különböző típusú hitelesítés használható.</span><span class="sxs-lookup"><span data-stu-id="e8b46-239">See [OData linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

```json
{
    "name": "ODataLinkedService",
        "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
            }
        }
}
```

<span data-ttu-id="e8b46-240">**Az Azure tárolás társított szolgáltatásának:**</span><span class="sxs-lookup"><span data-stu-id="e8b46-240">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="e8b46-241">**OData bemeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="e8b46-241">**OData input dataset:**</span></span>

<span data-ttu-id="e8b46-242">"External" beállítása: "true" arról tájékoztatja a Data Factory szolgáltatásnak, hogy az adatkészlet külső data factoryval való és adat-előállító tevékenység nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="e8b46-242">Setting “external”: ”true” informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```json
{
    "name": "ODataDataset",
    "properties":
    {
        "type": "ODataResource",
        "typeProperties":
        {
                "path": "Products"
        },
        "linkedServiceName": "ODataLinkedService",
        "structure": [],
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3                
        }
    }
}
```

<span data-ttu-id="e8b46-243">Adja meg **elérési** adatkészlet definíciója nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="e8b46-243">Specifying **path** in the dataset definition is optional.</span></span>

<span data-ttu-id="e8b46-244">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="e8b46-244">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="e8b46-245">Adatot ír egy új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="e8b46-245">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="e8b46-246">A mappa elérési útját a BLOB a szelet által feldolgozott kezdési ideje alapján dinamikusan történik.</span><span class="sxs-lookup"><span data-stu-id="e8b46-246">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="e8b46-247">A mappa elérési útját használja, év, hónap, nap és a kezdési idő órában részeit.</span><span class="sxs-lookup"><span data-stu-id="e8b46-247">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```json
{
    "name": "AzureBlobODataDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/odata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


<span data-ttu-id="e8b46-248">**Másolási tevékenység az OData-forrásra és Blob fogadó egy folyamaton belül:**</span><span class="sxs-lookup"><span data-stu-id="e8b46-248">**Copy activity in a pipeline with OData source and Blob sink:**</span></span>

<span data-ttu-id="e8b46-249">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="e8b46-249">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="e8b46-250">Az adatcsatorna JSON-definícióból a **forrás** típusúra **RelationalSource** és **fogadó** típusúra **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="e8b46-250">In the pipeline JSON definition, the **source** type is set to **RelationalSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="e8b46-251">A megadott SQL-lekérdezést a **lekérdezés** tulajdonság kiválasztja azokat a legújabb (legújabb) adatokat az OData-forrásra.</span><span class="sxs-lookup"><span data-stu-id="e8b46-251">The SQL query specified for the **query** property selects the latest (newest) data from the OData source.</span></span>

```json
{
    "name": "CopyODataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "?$select=Name, Description&$top=5",
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "ODataDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobODataDataSet"
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
                "name": "ODataToBlob"
            }
        ],
        "start": "2017-02-01T18:00:00Z",
        "end": "2017-02-03T19:00:00Z"
    }
}
```

<span data-ttu-id="e8b46-252">Adja meg **lekérdezés** az adatcsatorna definíciója nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="e8b46-252">Specifying **query** in the pipeline definition is optional.</span></span> <span data-ttu-id="e8b46-253">A **URL-cím** van, hogy a Data Factory szolgáltatásnak adatok lekéréséhez használja: a társított szolgáltatás (kötelező) a megadott URL-cím + adatkészletet (nem kötelező) + (nem kötelező) a feldolgozási lekérdezés megadott elérési út.</span><span class="sxs-lookup"><span data-stu-id="e8b46-253">The **URL** that the Data Factory service uses to retrieve data is: URL specified in the linked service (required) + path specified in the dataset (optional) + query in the pipeline (optional).</span></span>


### <a name="type-mapping-for-odata"></a><span data-ttu-id="e8b46-254">Az OData-leképezésének</span><span class="sxs-lookup"><span data-stu-id="e8b46-254">Type mapping for OData</span></span>
<span data-ttu-id="e8b46-255">Ahogyan az a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, a másolási tevékenység eseményforrás-típusnak gyűjtése típusa a következő 2. lépés – a módszert használja az automatikus típuskonverziók hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="e8b46-255">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following 2-step approach:</span></span>

1. <span data-ttu-id="e8b46-256">A natív eseményforrás-típusnak átalakítása .NET-típusa</span><span class="sxs-lookup"><span data-stu-id="e8b46-256">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="e8b46-257">.NET-típus konvertálása natív a fogadó típusa</span><span class="sxs-lookup"><span data-stu-id="e8b46-257">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="e8b46-258">OData adatok áthelyezése adatait tárolja, amikor az OData-adattípusok .NET típusú képezi le.</span><span class="sxs-lookup"><span data-stu-id="e8b46-258">When moving data from OData data stores, OData data types are mapped to .NET types.</span></span>

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="e8b46-259">Térkép forrás oszlopok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="e8b46-259">Map source to sink columns</span></span>
<span data-ttu-id="e8b46-260">A forrás oszlop szerepel a fogadó dataset adatkészlet leképezési oszlopok, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="e8b46-260">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="e8b46-261">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="e8b46-261">Repeatable read from relational sources</span></span>
<span data-ttu-id="e8b46-262">Ha az adatok másolását a relációs adatokat tárol, ismételhetőség tartsa szem előtt, nem kívánt eredmények elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="e8b46-262">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="e8b46-263">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="e8b46-263">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="e8b46-264">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="e8b46-264">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="e8b46-265">A szelet akkor fut újra, vagy módon, ha győződjön meg arról, hogy ugyanazokat az adatokat olvasható függetlenül attól, hogy a szelet futtatása hány alkalommal kell.</span><span class="sxs-lookup"><span data-stu-id="e8b46-265">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="e8b46-266">Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="e8b46-266">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="e8b46-267">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="e8b46-267">Performance and Tuning</span></span>
<span data-ttu-id="e8b46-268">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) tájékozódhat az kulcsfontosságú szerepet játszik adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon optimalizálhatja azt, hogy hatás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="e8b46-268">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
