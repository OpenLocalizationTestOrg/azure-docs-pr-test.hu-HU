---
title: "aaaMove adatokat OData forrásból |} Microsoft Docs"
description: "További információk a hogyan toomove adatokat OData adatforrásokat, Azure Data Factory használatával."
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
ms.openlocfilehash: 328efe4fd274fb3e54c1d2f209e4614c77c1ff37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-a-odata-source-using-azure-data-factory"></a><span data-ttu-id="81374-103">Helyezi át az adatokat a egy OData-forrásra Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="81374-103">Move data From a OData source using Azure Data Factory</span></span>
<span data-ttu-id="81374-104">Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatokat OData forrásból származó a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="81374-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an OData source.</span></span> <span data-ttu-id="81374-105">-Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="81374-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="81374-106">Egy OData forrás támogatott tooany fogadó adattároló adatainak másolhatja.</span><span class="sxs-lookup"><span data-stu-id="81374-106">You can copy data from an OData source tooany supported sink data store.</span></span> <span data-ttu-id="81374-107">Az adatok támogatott tárolja, a fogadók esetében hello másolási tevékenység, lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="81374-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="81374-108">Adat-előállító jelenleg csak áthelyezése egy OData forrásadatok tooother adatait tárolja, de nem támogatja az adatok áthelyezését más adatok tooan OData-forrásra tárolja.</span><span class="sxs-lookup"><span data-stu-id="81374-108">Data factory currently supports only moving data from an OData source tooother data stores, but not for moving data from other data stores tooan OData source.</span></span> 

## <a name="supported-versions-and-authentication-types"></a><span data-ttu-id="81374-109">Támogatott verziók és hitelesítési típusok</span><span class="sxs-lookup"><span data-stu-id="81374-109">Supported versions and authentication types</span></span>
<span data-ttu-id="81374-110">Az OData-összekötő 3.0 és 4.0-s verzióját, és mindkét felhőbeli OData adatait és a helyszíni OData források másolhatja OData-verziót támogatja.</span><span class="sxs-lookup"><span data-stu-id="81374-110">This OData connector support OData version 3.0 and 4.0, and you can copy data from both cloud OData and on-premises OData sources.</span></span> <span data-ttu-id="81374-111">Ez utóbbi hello kell tooinstall hello az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="81374-111">For hello latter, you need tooinstall hello Data Management Gateway.</span></span> <span data-ttu-id="81374-112">Lásd: [helyezze át az adatokat a helyszíni és a felhő között](data-factory-move-data-between-onprem-and-cloud.md) szóló cikkben olvashat az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="81374-112">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for details about Data Management Gateway.</span></span>

<span data-ttu-id="81374-113">Alább hitelesítési típusok támogatottak:</span><span class="sxs-lookup"><span data-stu-id="81374-113">Below authentication types are supported:</span></span>

* <span data-ttu-id="81374-114">tooaccess **felhő** OData-adatcsatorna, használhatja, hogy a névtelen, alapszintű (felhasználónév és jelszó), vagy az Azure Active Directory-alapú OAuth-hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="81374-114">tooaccess **cloud** OData feed, you can use anonymous, basic (user name and password), or Azure Active Directory based OAuth authentication.</span></span>
* <span data-ttu-id="81374-115">tooaccess **helyszíni** OData-adatcsatorna, használja a névtelen, alapszintű (felhasználónév és jelszó), vagy a Windows-hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="81374-115">tooaccess **on-premises** OData feed, you can use anonymous, basic (user name and password), or Windows authentication.</span></span>

## <a name="getting-started"></a><span data-ttu-id="81374-116">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="81374-116">Getting started</span></span>
<span data-ttu-id="81374-117">A másolási tevékenység, amely helyezi át az adatokat OData forrásból származó különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="81374-117">You can create a pipeline with a copy activity that moves data from an OData source by using different tools/APIs.</span></span>

<span data-ttu-id="81374-118">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="81374-118">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="81374-119">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="81374-119">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="81374-120">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="81374-120">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="81374-121">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="81374-121">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="81374-122">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="81374-122">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="81374-123">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="81374-123">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="81374-124">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="81374-124">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="81374-125">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="81374-125">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="81374-126">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="81374-126">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="81374-127">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="81374-127">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="81374-128">Egy minta JSON-definíciók, amelyek használt toocopy adatokat OData forrásból származó adat-előállító entitások, lásd: [JSON-példa: adatok másolása az OData-forrás tooAzure Blob](#json-example-copy-data-from-odata-source-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="81374-128">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an OData source, see [JSON example: Copy data from OData source tooAzure Blob](#json-example-copy-data-from-odata-source-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="81374-129">a következő szakaszok hello használt toodefine adat-előállító entitások adott tooOData forrás JSON-tulajdonághoz részleteit tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="81374-129">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooOData source:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="81374-130">Csatolt szolgáltatás tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="81374-130">Linked Service properties</span></span>
<span data-ttu-id="81374-131">a következő táblázat hello biztosít JSON elemek adott tooOData kapcsolódó szolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="81374-131">hello following table provides description for JSON elements specific tooOData linked service.</span></span>

| <span data-ttu-id="81374-132">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="81374-132">Property</span></span> | <span data-ttu-id="81374-133">Leírás</span><span class="sxs-lookup"><span data-stu-id="81374-133">Description</span></span> | <span data-ttu-id="81374-134">Szükséges</span><span class="sxs-lookup"><span data-stu-id="81374-134">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="81374-135">type</span><span class="sxs-lookup"><span data-stu-id="81374-135">type</span></span> |<span data-ttu-id="81374-136">hello type tulajdonságot kell beállítani: **OData**</span><span class="sxs-lookup"><span data-stu-id="81374-136">hello type property must be set to: **OData**</span></span> |<span data-ttu-id="81374-137">Igen</span><span class="sxs-lookup"><span data-stu-id="81374-137">Yes</span></span> |
| <span data-ttu-id="81374-138">URL-címe</span><span class="sxs-lookup"><span data-stu-id="81374-138">url</span></span> |<span data-ttu-id="81374-139">Hello OData-szolgáltatás URL-címét.</span><span class="sxs-lookup"><span data-stu-id="81374-139">Url of hello OData service.</span></span> |<span data-ttu-id="81374-140">Igen</span><span class="sxs-lookup"><span data-stu-id="81374-140">Yes</span></span> |
| <span data-ttu-id="81374-141">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="81374-141">authenticationType</span></span> |<span data-ttu-id="81374-142">Tooconnect toohello OData-forrásra használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="81374-142">Type of authentication used tooconnect toohello OData source.</span></span> <br/><br/> <span data-ttu-id="81374-143">A felhőbeli OData a lehetséges értékek: névtelen, alapszintű és OAuth (Megjegyzés: jelenleg csak Azure Data Factory támogatási Azure Active Directory-alapú OAuth).</span><span class="sxs-lookup"><span data-stu-id="81374-143">For cloud OData, possible values are Anonymous, Basic, and OAuth (note Azure Data Factory currently only support Azure Active Directory based OAuth).</span></span> <br/><br/> <span data-ttu-id="81374-144">A helyszíni OData lehetséges értékei a névtelen, alapszintű és a Windows.</span><span class="sxs-lookup"><span data-stu-id="81374-144">For on-premises OData, possible values are Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="81374-145">Igen</span><span class="sxs-lookup"><span data-stu-id="81374-145">Yes</span></span> |
| <span data-ttu-id="81374-146">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="81374-146">username</span></span> |<span data-ttu-id="81374-147">Ha egyszerű hitelesítést használ, adja meg a felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="81374-147">Specify user name if you are using Basic authentication.</span></span> |<span data-ttu-id="81374-148">Igen (Ha csak az egyszerű hitelesítés használata esetén)</span><span class="sxs-lookup"><span data-stu-id="81374-148">Yes (only if you are using Basic authentication)</span></span> |
| <span data-ttu-id="81374-149">jelszó</span><span class="sxs-lookup"><span data-stu-id="81374-149">password</span></span> |<span data-ttu-id="81374-150">Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="81374-150">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="81374-151">Igen (Ha csak az egyszerű hitelesítés használata esetén)</span><span class="sxs-lookup"><span data-stu-id="81374-151">Yes (only if you are using Basic authentication)</span></span> |
| <span data-ttu-id="81374-152">authorizedCredential</span><span class="sxs-lookup"><span data-stu-id="81374-152">authorizedCredential</span></span> |<span data-ttu-id="81374-153">Ha OAuth használ, kattintson a **engedélyezés** hello Data Factory másolása varázsló vagy a szerkesztő gombra, és adja meg a hitelesítő adatok, akkor a tulajdonság értékének hello lesz automatikusan generált.</span><span class="sxs-lookup"><span data-stu-id="81374-153">If you are using OAuth, click **Authorize** button in hello Data Factory Copy Wizard or Editor and enter your credential, then hello value of this property will be auto-generated.</span></span> |<span data-ttu-id="81374-154">Igen (csak ha OAuth-hitelesítés használata esetén)</span><span class="sxs-lookup"><span data-stu-id="81374-154">Yes (only if you are using OAuth authentication)</span></span> |
| <span data-ttu-id="81374-155">gatewayName</span><span class="sxs-lookup"><span data-stu-id="81374-155">gatewayName</span></span> |<span data-ttu-id="81374-156">Hello átjáró, amely a Data Factory szolgáltatásnak hello nevét használja tooconnect toohello helyszíni OData-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="81374-156">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises OData service.</span></span> <span data-ttu-id="81374-157">Csak adja meg, ha a másolt adatokat a helyszíni OData-forrásra.</span><span class="sxs-lookup"><span data-stu-id="81374-157">Specify only if you are copying data from on-prem OData source.</span></span> |<span data-ttu-id="81374-158">Nem</span><span class="sxs-lookup"><span data-stu-id="81374-158">No</span></span> |

### <a name="using-basic-authentication"></a><span data-ttu-id="81374-159">Alapszintű hitelesítést használó</span><span class="sxs-lookup"><span data-stu-id="81374-159">Using Basic authentication</span></span>
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

### <a name="using-anonymous-authentication"></a><span data-ttu-id="81374-160">A névtelen hitelesítés segítségével</span><span class="sxs-lookup"><span data-stu-id="81374-160">Using Anonymous authentication</span></span>
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

### <a name="using-windows-authentication-accessing-on-premises-odata-source"></a><span data-ttu-id="81374-161">Windows-hitelesítéssel fér hozzá a helyszíni OData-forrásra</span><span class="sxs-lookup"><span data-stu-id="81374-161">Using Windows authentication accessing on-premises OData source</span></span>
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

### <a name="using-oauth-authentication-accessing-cloud-odata-source"></a><span data-ttu-id="81374-162">OAuth hitelesítési elérése során a felhővel OData-forrásra</span><span class="sxs-lookup"><span data-stu-id="81374-162">Using OAuth authentication accessing cloud OData source</span></span>
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
            "authorizedCredential": "<auto generated by clicking hello Authorize button on UI>"
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="81374-163">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="81374-163">Dataset properties</span></span>
<span data-ttu-id="81374-164">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="81374-164">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="81374-165">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="81374-165">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="81374-166">Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="81374-166">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="81374-167">hello typeProperties szakasz típusú adatkészlet **ODataResource** következő tulajdonságai hello (amely tartalmazza a OData dataset) rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="81374-167">hello typeProperties section for dataset of type **ODataResource** (which includes OData dataset) has hello following properties</span></span>

| <span data-ttu-id="81374-168">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="81374-168">Property</span></span> | <span data-ttu-id="81374-169">Leírás</span><span class="sxs-lookup"><span data-stu-id="81374-169">Description</span></span> | <span data-ttu-id="81374-170">Szükséges</span><span class="sxs-lookup"><span data-stu-id="81374-170">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="81374-171">Elérési út</span><span class="sxs-lookup"><span data-stu-id="81374-171">path</span></span> |<span data-ttu-id="81374-172">Elérési út toohello OData-erőforrás</span><span class="sxs-lookup"><span data-stu-id="81374-172">Path toohello OData resource</span></span> |<span data-ttu-id="81374-173">Nem</span><span class="sxs-lookup"><span data-stu-id="81374-173">No</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="81374-174">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="81374-174">Copy activity properties</span></span>
<span data-ttu-id="81374-175">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="81374-175">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="81374-176">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="81374-176">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

<span data-ttu-id="81374-177">Tulajdonságok tevékenységek minden típusának hello hello tevékenységekre hello typeProperties szakaszában érhető el ugyanakkor függenek.</span><span class="sxs-lookup"><span data-stu-id="81374-177">Properties available in hello typeProperties section of hello activity on hello other hand vary with each activity type.</span></span> <span data-ttu-id="81374-178">A másolási tevékenység során két érték források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="81374-178">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="81374-179">Ha forrás típusa nem **RelationalSource** (amely magában foglalja az OData) typeProperties szakaszában érhetők hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="81374-179">When source is of type **RelationalSource** (which includes OData) hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="81374-180">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="81374-180">Property</span></span> | <span data-ttu-id="81374-181">Leírás</span><span class="sxs-lookup"><span data-stu-id="81374-181">Description</span></span> | <span data-ttu-id="81374-182">Példa</span><span class="sxs-lookup"><span data-stu-id="81374-182">Example</span></span> | <span data-ttu-id="81374-183">Szükséges</span><span class="sxs-lookup"><span data-stu-id="81374-183">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="81374-184">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="81374-184">query</span></span> |<span data-ttu-id="81374-185">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="81374-185">Use hello custom query tooread data.</span></span> |<span data-ttu-id="81374-186">"? $select neve, leírása és $top = = 5"</span><span class="sxs-lookup"><span data-stu-id="81374-186">"?$select=Name, Description&$top=5"</span></span> |<span data-ttu-id="81374-187">Nem</span><span class="sxs-lookup"><span data-stu-id="81374-187">No</span></span> |

## <a name="type-mapping-for-odata"></a><span data-ttu-id="81374-188">Az OData-leképezésének</span><span class="sxs-lookup"><span data-stu-id="81374-188">Type Mapping for OData</span></span>
<span data-ttu-id="81374-189">A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység hajt végre típusok toosink típusát Automatikus típusú konverzió a következő kétlépcsős megközelítést hello.</span><span class="sxs-lookup"><span data-stu-id="81374-189">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following two-step approach.</span></span>

1. <span data-ttu-id="81374-190">Natív típusok too.NET forrástípus konvertálása</span><span class="sxs-lookup"><span data-stu-id="81374-190">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="81374-191">.NET típusú toonative a fogadó típusa konvertálása</span><span class="sxs-lookup"><span data-stu-id="81374-191">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="81374-192">Amikor adatokat OData, hello következő megfeleltetéseket használt OData típusok too.NET típusból.</span><span class="sxs-lookup"><span data-stu-id="81374-192">When moving data from OData, hello following mappings are used from OData types too.NET type.</span></span>

| <span data-ttu-id="81374-193">Az OData-adattípus</span><span class="sxs-lookup"><span data-stu-id="81374-193">OData Data Type</span></span> | <span data-ttu-id="81374-194">.NET-típusa</span><span class="sxs-lookup"><span data-stu-id="81374-194">.NET Type</span></span> |
| --- | --- |
| <span data-ttu-id="81374-195">Edm.Binary</span><span class="sxs-lookup"><span data-stu-id="81374-195">Edm.Binary</span></span> |<span data-ttu-id="81374-196">Byte]</span><span class="sxs-lookup"><span data-stu-id="81374-196">Byte[]</span></span> |
| <span data-ttu-id="81374-197">Edm.Boolean</span><span class="sxs-lookup"><span data-stu-id="81374-197">Edm.Boolean</span></span> |<span data-ttu-id="81374-198">logikai érték</span><span class="sxs-lookup"><span data-stu-id="81374-198">Bool</span></span> |
| <span data-ttu-id="81374-199">Edm.Byte</span><span class="sxs-lookup"><span data-stu-id="81374-199">Edm.Byte</span></span> |<span data-ttu-id="81374-200">Byte]</span><span class="sxs-lookup"><span data-stu-id="81374-200">Byte[]</span></span> |
| <span data-ttu-id="81374-201">Edm.DateTime</span><span class="sxs-lookup"><span data-stu-id="81374-201">Edm.DateTime</span></span> |<span data-ttu-id="81374-202">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="81374-202">DateTime</span></span> |
| <span data-ttu-id="81374-203">Edm.Decimal</span><span class="sxs-lookup"><span data-stu-id="81374-203">Edm.Decimal</span></span> |<span data-ttu-id="81374-204">Decimális</span><span class="sxs-lookup"><span data-stu-id="81374-204">Decimal</span></span> |
| <span data-ttu-id="81374-205">Edm.Double</span><span class="sxs-lookup"><span data-stu-id="81374-205">Edm.Double</span></span> |<span data-ttu-id="81374-206">Dupla</span><span class="sxs-lookup"><span data-stu-id="81374-206">Double</span></span> |
| <span data-ttu-id="81374-207">Edm.Single</span><span class="sxs-lookup"><span data-stu-id="81374-207">Edm.Single</span></span> |<span data-ttu-id="81374-208">Egyetlen</span><span class="sxs-lookup"><span data-stu-id="81374-208">Single</span></span> |
| <span data-ttu-id="81374-209">Edm.Guid</span><span class="sxs-lookup"><span data-stu-id="81374-209">Edm.Guid</span></span> |<span data-ttu-id="81374-210">GUID</span><span class="sxs-lookup"><span data-stu-id="81374-210">Guid</span></span> |
| <span data-ttu-id="81374-211">Edm.Int16</span><span class="sxs-lookup"><span data-stu-id="81374-211">Edm.Int16</span></span> |<span data-ttu-id="81374-212">Int16</span><span class="sxs-lookup"><span data-stu-id="81374-212">Int16</span></span> |
| <span data-ttu-id="81374-213">Edm.Int32</span><span class="sxs-lookup"><span data-stu-id="81374-213">Edm.Int32</span></span> |<span data-ttu-id="81374-214">Int32</span><span class="sxs-lookup"><span data-stu-id="81374-214">Int32</span></span> |
| <span data-ttu-id="81374-215">Edm.Int64</span><span class="sxs-lookup"><span data-stu-id="81374-215">Edm.Int64</span></span> |<span data-ttu-id="81374-216">Int64</span><span class="sxs-lookup"><span data-stu-id="81374-216">Int64</span></span> |
| <span data-ttu-id="81374-217">Edm.SByte</span><span class="sxs-lookup"><span data-stu-id="81374-217">Edm.SByte</span></span> |<span data-ttu-id="81374-218">Int16</span><span class="sxs-lookup"><span data-stu-id="81374-218">Int16</span></span> |
| <span data-ttu-id="81374-219">Edm.String</span><span class="sxs-lookup"><span data-stu-id="81374-219">Edm.String</span></span> |<span data-ttu-id="81374-220">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="81374-220">String</span></span> |
| <span data-ttu-id="81374-221">Edm.Time</span><span class="sxs-lookup"><span data-stu-id="81374-221">Edm.Time</span></span> |<span data-ttu-id="81374-222">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="81374-222">TimeSpan</span></span> |
| <span data-ttu-id="81374-223">Edm.DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="81374-223">Edm.DateTimeOffset</span></span> |<span data-ttu-id="81374-224">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="81374-224">DateTimeOffset</span></span> |

> [!Note]
> <span data-ttu-id="81374-225">OData összetett adattípusok pl. objektum nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="81374-225">OData complex data types e.g. Object are not supported.</span></span>

## <a name="json-example-copy-data-from-odata-source-tooazure-blob"></a><span data-ttu-id="81374-226">JSON-példa: adatok másolása az OData-forrás tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="81374-226">JSON example: Copy data from OData source tooAzure Blob</span></span>
<span data-ttu-id="81374-227">Ebben a példában a minta JSON definícióit tartalmazza használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="81374-227">This example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="81374-228">Ezek bemutatják, hogyan toocopy egy OData az adatforrás-tooan Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="81374-228">They show how toocopy data from an OData source tooan Azure Blob Storage.</span></span> <span data-ttu-id="81374-229">Adatok azonban nem közölt hello nyelő másolt tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.</span><span class="sxs-lookup"><span data-stu-id="81374-229">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span> <span data-ttu-id="81374-230">hello minta a következő adat-előállító entitások hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="81374-230">hello sample has hello following Data Factory entities:</span></span>

1. <span data-ttu-id="81374-231">A társított szolgáltatás típusa [OData](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="81374-231">A linked service of type [OData](#linked-service-properties).</span></span>
2. <span data-ttu-id="81374-232">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="81374-232">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="81374-233">Bemeneti [dataset](data-factory-create-datasets.md) típusú [ODataResource](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="81374-233">An input [dataset](data-factory-create-datasets.md) of type [ODataResource](#dataset-properties).</span></span>
4. <span data-ttu-id="81374-234">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="81374-234">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="81374-235">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="81374-235">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="81374-236">hello minta másol adatokat kérdez le egy OData-forrás tooan szemben Azure blob minden órában.</span><span class="sxs-lookup"><span data-stu-id="81374-236">hello sample copies data from querying against an OData source tooan Azure blob every hour.</span></span> <span data-ttu-id="81374-237">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="81374-237">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="81374-238">**OData társított szolgáltatás:** ebben a példában használt hello névtelen hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="81374-238">**OData linked service:** This example uses hello Anonymous authentication.</span></span> <span data-ttu-id="81374-239">Lásd: [OData társított szolgáltatás](#linked-service-properties) szakasz a különböző típusú hitelesítés használható.</span><span class="sxs-lookup"><span data-stu-id="81374-239">See [OData linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

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

<span data-ttu-id="81374-240">**Az Azure tárolás társított szolgáltatásának:**</span><span class="sxs-lookup"><span data-stu-id="81374-240">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="81374-241">**OData bemeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="81374-241">**OData input dataset:**</span></span>

<span data-ttu-id="81374-242">"External" beállítása: "true" tájékoztatja hello Data Factory szolgáltatásnak, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="81374-242">Setting “external”: ”true” informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="81374-243">Adja meg **elérési** hello adatkészlet definíciója nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="81374-243">Specifying **path** in hello dataset definition is optional.</span></span>

<span data-ttu-id="81374-244">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="81374-244">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="81374-245">Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="81374-245">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="81374-246">hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="81374-246">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="81374-247">hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.</span><span class="sxs-lookup"><span data-stu-id="81374-247">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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


<span data-ttu-id="81374-248">**Másolási tevékenység az OData-forrásra és Blob fogadó egy folyamaton belül:**</span><span class="sxs-lookup"><span data-stu-id="81374-248">**Copy activity in a pipeline with OData source and Blob sink:**</span></span>

<span data-ttu-id="81374-249">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="81374-249">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="81374-250">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**RelationalSource** és **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="81374-250">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="81374-251">hello SQL-lekérdezésben megadott hello **lekérdezés** tulajdonság hello legújabb (legújabb) adatokat kiválaszt hello OData-forrásra.</span><span class="sxs-lookup"><span data-stu-id="81374-251">hello SQL query specified for hello **query** property selects hello latest (newest) data from hello OData source.</span></span>

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

<span data-ttu-id="81374-252">Adja meg **lekérdezés** hello folyamat definíciója nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="81374-252">Specifying **query** in hello pipeline definition is optional.</span></span> <span data-ttu-id="81374-253">Hello **URL-cím** van, hogy a hello Data Factory szolgáltatásnak tooretrieve adatokat használja: társított szolgáltatás (kötelező) + hello adatkészletet (nem kötelező) a megadott elérési út hello + hello folyamat (nem kötelező) lekérdezés megadott URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="81374-253">hello **URL** that hello Data Factory service uses tooretrieve data is: URL specified in hello linked service (required) + path specified in hello dataset (optional) + query in hello pipeline (optional).</span></span>


### <a name="type-mapping-for-odata"></a><span data-ttu-id="81374-254">Az OData-leképezésének</span><span class="sxs-lookup"><span data-stu-id="81374-254">Type mapping for OData</span></span>
<span data-ttu-id="81374-255">A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység hajt végre automatikus típuskonverziók származó típusok toosink típusait a 2. lépés – a módszert követve hello:</span><span class="sxs-lookup"><span data-stu-id="81374-255">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="81374-256">Natív típusok too.NET forrástípus konvertálása</span><span class="sxs-lookup"><span data-stu-id="81374-256">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="81374-257">.NET típusú toonative a fogadó típusa konvertálása</span><span class="sxs-lookup"><span data-stu-id="81374-257">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="81374-258">Adatok áthelyezése az OData-adattároló, amikor a OData adattípusok olyan csatlakoztatott too.NET típusok.</span><span class="sxs-lookup"><span data-stu-id="81374-258">When moving data from OData data stores, OData data types are mapped too.NET types.</span></span>

## <a name="map-source-toosink-columns"></a><span data-ttu-id="81374-259">A forrásoszlopokat toosink leképezése</span><span class="sxs-lookup"><span data-stu-id="81374-259">Map source toosink columns</span></span>
<span data-ttu-id="81374-260">toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="81374-260">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="81374-261">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="81374-261">Repeatable read from relational sources</span></span>
<span data-ttu-id="81374-262">Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények.</span><span class="sxs-lookup"><span data-stu-id="81374-262">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="81374-263">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="81374-263">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="81374-264">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="81374-264">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="81374-265">A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása.</span><span class="sxs-lookup"><span data-stu-id="81374-265">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="81374-266">Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="81374-266">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="81374-267">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="81374-267">Performance and Tuning</span></span>
<span data-ttu-id="81374-268">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.</span><span class="sxs-lookup"><span data-stu-id="81374-268">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
