---
title: "ODBC-adattároló aaaMove adatait |} Microsoft Docs"
description: "További információk a hogyan ODBC adatok toomove adatait tárolja az Azure Data Factory használatával."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: ad70a598-c031-4339-a883-c6125403cb76
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: jingwang
ms.openlocfilehash: bf96e71da449313b6144bb194205c572d2ca2030
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-odbc-data-stores-using-azure-data-factory"></a><span data-ttu-id="52a80-103">Helyezze át a ODBC adattárolókhoz Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="52a80-103">Move data From ODBC data stores using Azure Data Factory</span></span>
<span data-ttu-id="52a80-104">Ez a cikk azt ismerteti, hogyan toouse hello másolási tevékenység az Azure Data Factory toomove adatait egy helyszíni ODBC adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="52a80-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises ODBC data store.</span></span> <span data-ttu-id="52a80-105">-Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="52a80-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="52a80-106">Egy ODBC adatokat tároló támogatott tooany fogadó adatokat tároló adatainak másolhatja.</span><span class="sxs-lookup"><span data-stu-id="52a80-106">You can copy data from an ODBC data store tooany supported sink data store.</span></span> <span data-ttu-id="52a80-107">Az adatok támogatott tárolja, a fogadók esetében hello másolási tevékenység, lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="52a80-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="52a80-108">Adat-előállító jelenleg csak egy ODBC-adatokból adattároló tooother adattárolókhoz áthelyezése, de nem adatok áthelyezését más adatok tárolók tooan ODBC adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="52a80-108">Data factory currently supports only moving data from an ODBC data store tooother data stores, but not for moving data from other data stores tooan ODBC data store.</span></span> 

## <a name="enabling-connectivity"></a><span data-ttu-id="52a80-109">Kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="52a80-109">Enabling connectivity</span></span>
<span data-ttu-id="52a80-110">Data Factory szolgáltatásnak csatlakozó tooon helyszíni ODBC adatforrások az adatkezelési átjáró hello segítségével támogatja.</span><span class="sxs-lookup"><span data-stu-id="52a80-110">Data Factory service supports connecting tooon-premises ODBC sources using hello Data Management Gateway.</span></span> <span data-ttu-id="52a80-111">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk toolearn az adatkezelési átjáró és hello átjáró beállításával kapcsolatos részletes utasításokat.</span><span class="sxs-lookup"><span data-stu-id="52a80-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span> <span data-ttu-id="52a80-112">Hello átjáró tooconnect tooan ODBC adattár használata akkor is, ha egy Azure IaaS virtuális gép helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="52a80-112">Use hello gateway tooconnect tooan ODBC data store even if it is hosted in an Azure IaaS VM.</span></span>

<span data-ttu-id="52a80-113">Hello ugyanabban a helyi számítógép- vagy hello hello ODBC adatokat tároló Azure VM hello átjáró telepíthető.</span><span class="sxs-lookup"><span data-stu-id="52a80-113">You can install hello gateway on hello same on-premises machine or hello Azure VM as hello ODBC data store.</span></span> <span data-ttu-id="52a80-114">Azonban azt javasoljuk, hogy hello átjáró telepítése egy különálló számítógép vagy az Azure infrastruktúra-szolgáltatási virtuális gép tooavoid Erőforrásverseny és a jobb teljesítmény érdekében.</span><span class="sxs-lookup"><span data-stu-id="52a80-114">However, we recommend that you install hello gateway on a separate machine/Azure IaaS VM tooavoid resource contention and for better performance.</span></span> <span data-ttu-id="52a80-115">Hello átjáró egy külön számítógépen való telepítésekor hello gépnek képes tooaccess hello gép hello ODBC adattár kell lennie.</span><span class="sxs-lookup"><span data-stu-id="52a80-115">When you install hello gateway on a separate machine, hello machine should be able tooaccess hello machine with hello ODBC data store.</span></span>

<span data-ttu-id="52a80-116">Az adatkezelési átjáró hello, leszámítva szükség tooinstall hello ODBC-illesztőprogram hello adattároló hello-átjáró számítógépén.</span><span class="sxs-lookup"><span data-stu-id="52a80-116">Apart from hello Data Management Gateway, you also need tooinstall hello ODBC driver for hello data store on hello gateway machine.</span></span>

> [!NOTE]
> <span data-ttu-id="52a80-117">Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.</span><span class="sxs-lookup"><span data-stu-id="52a80-117">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="getting-started"></a><span data-ttu-id="52a80-118">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="52a80-118">Getting started</span></span>
<span data-ttu-id="52a80-119">A másolási tevékenység, mely az adatok egy ODBC-adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="52a80-119">You can create a pipeline with a copy activity that moves data from an ODBC data store by using different tools/APIs.</span></span>

<span data-ttu-id="52a80-120">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="52a80-120">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="52a80-121">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="52a80-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="52a80-122">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="52a80-122">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="52a80-123">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="52a80-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="52a80-124">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="52a80-124">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="52a80-125">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="52a80-125">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="52a80-126">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="52a80-126">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="52a80-127">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="52a80-127">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="52a80-128">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="52a80-128">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="52a80-129">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="52a80-129">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="52a80-130">Az adat-előállító entitások, amelyek egy ODBC-adattároló használt toocopy adatait JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az ODBC-adatok tárolására tooAzure Blob](#json-example-copy-data-from-odbc-data-store-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="52a80-130">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an ODBC data store, see [JSON example: Copy data from ODBC data store tooAzure Blob](#json-example-copy-data-from-odbc-data-store-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="52a80-131">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooODBC adattár részleteit tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="52a80-131">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooODBC data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="52a80-132">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="52a80-132">Linked service properties</span></span>
<span data-ttu-id="52a80-133">a következő táblázat hello biztosít JSON elemek adott tooODBC kapcsolódó szolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="52a80-133">hello following table provides description for JSON elements specific tooODBC linked service.</span></span>

| <span data-ttu-id="52a80-134">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="52a80-134">Property</span></span> | <span data-ttu-id="52a80-135">Leírás</span><span class="sxs-lookup"><span data-stu-id="52a80-135">Description</span></span> | <span data-ttu-id="52a80-136">Szükséges</span><span class="sxs-lookup"><span data-stu-id="52a80-136">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="52a80-137">type</span><span class="sxs-lookup"><span data-stu-id="52a80-137">type</span></span> |<span data-ttu-id="52a80-138">hello type tulajdonságot kell beállítani: **OnPremisesOdbc**</span><span class="sxs-lookup"><span data-stu-id="52a80-138">hello type property must be set to: **OnPremisesOdbc**</span></span> |<span data-ttu-id="52a80-139">Igen</span><span class="sxs-lookup"><span data-stu-id="52a80-139">Yes</span></span> |
| <span data-ttu-id="52a80-140">connectionString</span><span class="sxs-lookup"><span data-stu-id="52a80-140">connectionString</span></span> |<span data-ttu-id="52a80-141">hello nem hozzáférési hitelesítő adatok része hello kapcsolati karakterláncot, és egy nem kötelező hitelesítő adat titkosítva.</span><span class="sxs-lookup"><span data-stu-id="52a80-141">hello non-access credential portion of hello connection string and an optional encrypted credential.</span></span> <span data-ttu-id="52a80-142">Példák a következő részekben hello.</span><span class="sxs-lookup"><span data-stu-id="52a80-142">See examples in hello following sections.</span></span> |<span data-ttu-id="52a80-143">Igen</span><span class="sxs-lookup"><span data-stu-id="52a80-143">Yes</span></span> |
| <span data-ttu-id="52a80-144">hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="52a80-144">credential</span></span> |<span data-ttu-id="52a80-145">hello hozzáférési hitelesítő adatok része, illesztőprogram-specifikus tulajdonság-érték formátumban megadott hello kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="52a80-145">hello access credential portion of hello connection string specified in driver-specific property-value format.</span></span> <span data-ttu-id="52a80-146">Példa: "Uid =<user ID>; Pwd =<password>; RefreshToken =<secret refresh token>; ".</span><span class="sxs-lookup"><span data-stu-id="52a80-146">Example: “Uid=<user ID>;Pwd=<password>;RefreshToken=<secret refresh token>;”.</span></span> |<span data-ttu-id="52a80-147">Nem</span><span class="sxs-lookup"><span data-stu-id="52a80-147">No</span></span> |
| <span data-ttu-id="52a80-148">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="52a80-148">authenticationType</span></span> |<span data-ttu-id="52a80-149">Tooconnect toohello ODBC adattár használt hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="52a80-149">Type of authentication used tooconnect toohello ODBC data store.</span></span> <span data-ttu-id="52a80-150">Lehetséges értékek a következők: névtelen és alapvető.</span><span class="sxs-lookup"><span data-stu-id="52a80-150">Possible values are: Anonymous and Basic.</span></span> |<span data-ttu-id="52a80-151">Igen</span><span class="sxs-lookup"><span data-stu-id="52a80-151">Yes</span></span> |
| <span data-ttu-id="52a80-152">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="52a80-152">username</span></span> |<span data-ttu-id="52a80-153">Ha egyszerű hitelesítést használ, adja meg a felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="52a80-153">Specify user name if you are using Basic authentication.</span></span> |<span data-ttu-id="52a80-154">Nem</span><span class="sxs-lookup"><span data-stu-id="52a80-154">No</span></span> |
| <span data-ttu-id="52a80-155">jelszó</span><span class="sxs-lookup"><span data-stu-id="52a80-155">password</span></span> |<span data-ttu-id="52a80-156">Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="52a80-156">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="52a80-157">Nem</span><span class="sxs-lookup"><span data-stu-id="52a80-157">No</span></span> |
| <span data-ttu-id="52a80-158">gatewayName</span><span class="sxs-lookup"><span data-stu-id="52a80-158">gatewayName</span></span> |<span data-ttu-id="52a80-159">Hello átjáró, amely a Data Factory szolgáltatásnak hello neve tooconnect toohello ODBC adattárat kell használnia.</span><span class="sxs-lookup"><span data-stu-id="52a80-159">Name of hello gateway that hello Data Factory service should use tooconnect toohello ODBC data store.</span></span> |<span data-ttu-id="52a80-160">Igen</span><span class="sxs-lookup"><span data-stu-id="52a80-160">Yes</span></span> |

### <a name="using-basic-authentication"></a><span data-ttu-id="52a80-161">Alapszintű hitelesítést használó</span><span class="sxs-lookup"><span data-stu-id="52a80-161">Using Basic authentication</span></span>

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```
### <a name="using-basic-authentication-with-encrypted-credentials"></a><span data-ttu-id="52a80-162">Alapszintű hitelesítést használ, a titkosított hitelesítő adatokkal</span><span class="sxs-lookup"><span data-stu-id="52a80-162">Using Basic authentication with encrypted credentials</span></span>
<span data-ttu-id="52a80-163">Hello hitelesítő adatok hello segítségével titkosíthatja [New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) (Azure PowerShell 1.0-ás verziója) parancsmag vagy [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0.9 vagy korábbi verziójú hello Az Azure PowerShell).</span><span class="sxs-lookup"><span data-stu-id="52a80-163">You can encrypt hello credentials using hello [New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) (1.0 version of Azure PowerShell) cmdlet or [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0.9 or earlier version of hello Azure PowerShell).</span></span>  

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=myserver.database.windows.net; Database=TestDatabase;;EncryptedCredential=eyJDb25uZWN0...........................",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-anonymous-authentication"></a><span data-ttu-id="52a80-164">A névtelen hitelesítés segítségével</span><span class="sxs-lookup"><span data-stu-id="52a80-164">Using Anonymous authentication</span></span>

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "connectionString": "Driver={SQL Server};Server={servername}.database.windows.net; Database=TestDatabase;",
            "credential": "UID={uid};PWD={pwd}",
            "gatewayName": "mygateway"
        }
    }
}
```


## <a name="dataset-properties"></a><span data-ttu-id="52a80-165">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="52a80-165">Dataset properties</span></span>
<span data-ttu-id="52a80-166">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="52a80-166">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="52a80-167">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="52a80-167">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="52a80-168">Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="52a80-168">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="52a80-169">hello typeProperties szakasz típusú adatkészlet **RelationalTable** következő tulajdonságai hello (amely tartalmazza a ODBC dataset) rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="52a80-169">hello typeProperties section for dataset of type **RelationalTable** (which includes ODBC dataset) has hello following properties</span></span>

| <span data-ttu-id="52a80-170">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="52a80-170">Property</span></span> | <span data-ttu-id="52a80-171">Leírás</span><span class="sxs-lookup"><span data-stu-id="52a80-171">Description</span></span> | <span data-ttu-id="52a80-172">Szükséges</span><span class="sxs-lookup"><span data-stu-id="52a80-172">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="52a80-173">tableName</span><span class="sxs-lookup"><span data-stu-id="52a80-173">tableName</span></span> |<span data-ttu-id="52a80-174">Hello ODBC adattár hello tábla neve.</span><span class="sxs-lookup"><span data-stu-id="52a80-174">Name of hello table in hello ODBC data store.</span></span> |<span data-ttu-id="52a80-175">Igen</span><span class="sxs-lookup"><span data-stu-id="52a80-175">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="52a80-176">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="52a80-176">Copy activity properties</span></span>
<span data-ttu-id="52a80-177">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="52a80-177">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="52a80-178">Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti táblák és házirendek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="52a80-178">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="52a80-179">Tulajdonságok érhetők el hello **typeProperties** szakasz hello hello tevékenységekre ugyanakkor tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="52a80-179">Properties available in hello **typeProperties** section of hello activity on hello other hand vary with each activity type.</span></span> <span data-ttu-id="52a80-180">A másolási tevékenység során két érték források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="52a80-180">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="52a80-181">A másolási tevékenység során típusú forrás esetén **RelationalSource** (amely tartalmazza a ODBC), typeProperties szakaszában érhetők hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="52a80-181">In copy activity, when source is of type **RelationalSource** (which includes ODBC), hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="52a80-182">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="52a80-182">Property</span></span> | <span data-ttu-id="52a80-183">Leírás</span><span class="sxs-lookup"><span data-stu-id="52a80-183">Description</span></span> | <span data-ttu-id="52a80-184">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="52a80-184">Allowed values</span></span> | <span data-ttu-id="52a80-185">Szükséges</span><span class="sxs-lookup"><span data-stu-id="52a80-185">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="52a80-186">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="52a80-186">query</span></span> |<span data-ttu-id="52a80-187">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="52a80-187">Use hello custom query tooread data.</span></span> |<span data-ttu-id="52a80-188">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="52a80-188">SQL query string.</span></span> <span data-ttu-id="52a80-189">Például: Válasszon * from tábla.</span><span class="sxs-lookup"><span data-stu-id="52a80-189">For example: select * from MyTable.</span></span> |<span data-ttu-id="52a80-190">Igen</span><span class="sxs-lookup"><span data-stu-id="52a80-190">Yes</span></span> |


## <a name="json-example-copy-data-from-odbc-data-store-tooazure-blob"></a><span data-ttu-id="52a80-191">JSON-példa: adatok másolása az ODBC-adatok tárolására tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="52a80-191">JSON example: Copy data from ODBC data store tooAzure Blob</span></span>
<span data-ttu-id="52a80-192">Ez a példa biztosít JSON-definíciók használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="52a80-192">This example provides JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="52a80-193">Azt illusztrálja, hogyan toocopy egy ODBC az adatforrás-tooan Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="52a80-193">It shows how toocopy data from an ODBC source tooan Azure Blob Storage.</span></span> <span data-ttu-id="52a80-194">Adatok azonban nem közölt hello nyelő másolt tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.</span><span class="sxs-lookup"><span data-stu-id="52a80-194">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

<span data-ttu-id="52a80-195">hello minta a következő data factory entitások hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="52a80-195">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="52a80-196">A társított szolgáltatás típusa [OnPremisesOdbc](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="52a80-196">A linked service of type [OnPremisesOdbc](#linked-service-properties).</span></span>
2. <span data-ttu-id="52a80-197">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="52a80-197">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="52a80-198">Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="52a80-198">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
4. <span data-ttu-id="52a80-199">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="52a80-199">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="52a80-200">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="52a80-200">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="52a80-201">hello minta másol adatokat egy lekérdezés eredményét, az ODBC adatokat tároló tooa BLOB minden órában.</span><span class="sxs-lookup"><span data-stu-id="52a80-201">hello sample copies data from a query result in an ODBC data store tooa blob every hour.</span></span> <span data-ttu-id="52a80-202">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="52a80-202">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="52a80-203">Első lépésként hello az adatkezelési átjáró beállítása.</span><span class="sxs-lookup"><span data-stu-id="52a80-203">As a first step, set up hello data management gateway.</span></span> <span data-ttu-id="52a80-204">hello utasítások szerepelnek hello [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="52a80-204">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="52a80-205">**ODBC társított szolgáltatás** ebben a példában, használja az egyszerű hitelesítés hello.</span><span class="sxs-lookup"><span data-stu-id="52a80-205">**ODBC linked service** This example uses hello Basic authentication.</span></span> <span data-ttu-id="52a80-206">Lásd: [ODBC társított szolgáltatás](#linked-service-properties) szakasz a különböző típusú hitelesítés használható.</span><span class="sxs-lookup"><span data-stu-id="52a80-206">See [ODBC linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

```json
{
    "name": "OnPremOdbcLinkedService",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```

<span data-ttu-id="52a80-207">**Azure Storage társított szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="52a80-207">**Azure Storage linked service**</span></span>

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

<span data-ttu-id="52a80-208">**ODBC bemeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="52a80-208">**ODBC input dataset**</span></span>

<span data-ttu-id="52a80-209">hello minta azt feltételezi, hogy létrehozott egy "MyTable" tábla egy ODBC-adatbázis és a "timestampcolumn" nevű adatsorozat időadatok oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="52a80-209">hello sample assumes you have created a table “MyTable” in an ODBC database and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="52a80-210">"External" beállítása: "true" tájékoztatja hello Data Factory szolgáltatásnak, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="52a80-210">Setting “external”: ”true” informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```json
{
    "name": "ODBCDataSet",
    "properties": {
        "published": false,
        "type": "RelationalTable",
        "linkedServiceName": "OnPremOdbcLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
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

<span data-ttu-id="52a80-211">**Az Azure Blob kimeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="52a80-211">**Azure Blob output dataset**</span></span>

<span data-ttu-id="52a80-212">Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="52a80-212">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="52a80-213">hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="52a80-213">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="52a80-214">hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.</span><span class="sxs-lookup"><span data-stu-id="52a80-214">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```json
{
    "name": "AzureBlobOdbcDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/odbc/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


<span data-ttu-id="52a80-215">**Másolási tevékenység során a folyamat az ODBC-adatforrás (RelationalSource) és a Blob fogadó (BlobSink)**</span><span class="sxs-lookup"><span data-stu-id="52a80-215">**Copy activity in a pipeline with ODBC source (RelationalSource) and Blob sink (BlobSink)**</span></span>

<span data-ttu-id="52a80-216">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse ezen bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="52a80-216">hello pipeline contains a Copy Activity that is configured toouse these input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="52a80-217">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**RelationalSource** és **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="52a80-217">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="52a80-218">hello SQL-lekérdezésben megadott hello **lekérdezés** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.</span><span class="sxs-lookup"><span data-stu-id="52a80-218">hello SQL query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

```json
{
    "name": "CopyODBCToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "OdbcDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOdbcDataSet"
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
                "name": "OdbcToBlob"
            }
        ],
        "start": "2016-06-01T18:00:00Z",
        "end": "2016-06-01T19:00:00Z"
    }
}
```
### <a name="type-mapping-for-odbc"></a><span data-ttu-id="52a80-219">Az ODBC leképezésének</span><span class="sxs-lookup"><span data-stu-id="52a80-219">Type mapping for ODBC</span></span>
<span data-ttu-id="52a80-220">A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység hajt végre típusok toosink típusát Automatikus típusú konverzió a következő kétlépcsős megközelítést hello:</span><span class="sxs-lookup"><span data-stu-id="52a80-220">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following two-step approach:</span></span>

1. <span data-ttu-id="52a80-221">Natív típusok too.NET forrástípus konvertálása</span><span class="sxs-lookup"><span data-stu-id="52a80-221">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="52a80-222">.NET típusú toonative a fogadó típusa konvertálása</span><span class="sxs-lookup"><span data-stu-id="52a80-222">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="52a80-223">Adatok ODBC adatok áruházakból áthelyezésekor ODBC adattípusok-e a csatlakoztatott too.NET típusok, a hello [ODBC adattípus-leképezések alkalmazását](https://msdn.microsoft.com/library/cc668763.aspx) témakör.</span><span class="sxs-lookup"><span data-stu-id="52a80-223">When moving data from ODBC data stores, ODBC data types are mapped too.NET types as mentioned in hello [ODBC Data Type Mappings](https://msdn.microsoft.com/library/cc668763.aspx) topic.</span></span>

## <a name="map-source-toosink-columns"></a><span data-ttu-id="52a80-224">A forrásoszlopokat toosink leképezése</span><span class="sxs-lookup"><span data-stu-id="52a80-224">Map source toosink columns</span></span>
<span data-ttu-id="52a80-225">toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="52a80-225">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="52a80-226">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="52a80-226">Repeatable read from relational sources</span></span>
<span data-ttu-id="52a80-227">Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények.</span><span class="sxs-lookup"><span data-stu-id="52a80-227">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="52a80-228">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="52a80-228">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="52a80-229">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="52a80-229">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="52a80-230">A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása.</span><span class="sxs-lookup"><span data-stu-id="52a80-230">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="52a80-231">Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="52a80-231">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="ge-historian-store"></a><span data-ttu-id="52a80-232">GE történész tároló</span><span class="sxs-lookup"><span data-stu-id="52a80-232">GE Historian store</span></span>
<span data-ttu-id="52a80-233">Az ODBC kapcsolódó szolgáltatás toolink létrehozhat egy [GE Proficy történész (most GE történész)](http://www.geautomation.com/products/proficy-historian) az adattároló tooan az Azure data factory, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="52a80-233">You create an ODBC linked service toolink a [GE Proficy Historian (now GE Historian)](http://www.geautomation.com/products/proficy-historian) data store tooan Azure data factory as shown in hello following example:</span></span>

```json
{
    "name": "HistorianLinkedService",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "connectionString": "DSN=<name of hello GE Historian store>",
            "gatewayName": "<gateway name>",
            "authenticationType": "Basic",
            "userName": "<user name>",
            "password": "<password>"
        }
    }
}
```

<span data-ttu-id="52a80-234">Az adatkezelési átjáró telepíthető egy helyszíni gépre, és hello átjáró regisztrálása hello portálon.</span><span class="sxs-lookup"><span data-stu-id="52a80-234">Install Data Management Gateway on an on-premises machine and register hello gateway with hello portal.</span></span> <span data-ttu-id="52a80-235">a helyszíni számítógépre telepített hello átjáró GE történész tooconnect toohello GE történész adattár hello ODBC-illesztőprogram használ.</span><span class="sxs-lookup"><span data-stu-id="52a80-235">hello gateway installed on your on-premises computer uses hello ODBC driver for GE Historian tooconnect toohello GE Historian data store.</span></span> <span data-ttu-id="52a80-236">Ezért telepítenie hello illesztőprogram, ha még nincs telepítve hello-átjáró számítógépén.</span><span class="sxs-lookup"><span data-stu-id="52a80-236">Therefore, install hello driver if it is not already installed on hello gateway machine.</span></span> <span data-ttu-id="52a80-237">Lásd: [kapcsolat engedélyezése](#enabling-connectivity) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="52a80-237">See [Enabling connectivity](#enabling-connectivity) section for details.</span></span>

<span data-ttu-id="52a80-238">A Data Factory-megoldás tárolja GE történész hello használata előtt győződjön meg arról, hogy hello-átjáró képes kapcsolódni a útmutatást a következő szakaszban hello toohello adattár.</span><span class="sxs-lookup"><span data-stu-id="52a80-238">Before you use hello GE Historian store in a Data Factory solution, verify whether hello gateway can connect toohello data store using instructions in hello next section.</span></span>

<span data-ttu-id="52a80-239">A másolási műveletek a forrás-adattároló hello a cikk elolvasása részletes áttekintés hello elejétől ODBC adatok használatával tárolja.</span><span class="sxs-lookup"><span data-stu-id="52a80-239">Read hello article from hello beginning for a detailed overview of using ODBC data stores as source data stores in a copy operation.</span></span>  

## <a name="troubleshoot-connectivity-issues"></a><span data-ttu-id="52a80-240">Csatlakozási problémák</span><span class="sxs-lookup"><span data-stu-id="52a80-240">Troubleshoot connectivity issues</span></span>
<span data-ttu-id="52a80-241">tootroubleshoot csatlakozási problémáit, használja a hello **diagnosztika** lapján **az adatkezelési átjáró konfigurációkezelőjének**.</span><span class="sxs-lookup"><span data-stu-id="52a80-241">tootroubleshoot connection issues, use hello **Diagnostics** tab of **Data Management Gateway Configuration Manager**.</span></span>

1. <span data-ttu-id="52a80-242">Indítsa el **az adatkezelési átjáró konfigurációkezelőjének**.</span><span class="sxs-lookup"><span data-stu-id="52a80-242">Launch **Data Management Gateway Configuration Manager**.</span></span> <span data-ttu-id="52a80-243">Futtatásával "C:\Program Files\Microsoft Data felügyeleti Gateway\1.0\Shared\ConfigManager.exe" közvetlenül (vagy) keresés a **átjáró** toofind hivatkozás túl**Microsoft adatkezelési átjáró** ahogy az a következő kép hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="52a80-243">You can either run "C:\Program Files\Microsoft Data Management Gateway\1.0\Shared\ConfigManager.exe" directly (or) search for **Gateway** toofind a link too**Microsoft Data Management Gateway** application as shown in hello following image.</span></span>

    ![Keresési átjáró](./media/data-factory-odbc-connector/search-gateway.png)
2. <span data-ttu-id="52a80-245">Váltás toohello **diagnosztika** fülre.</span><span class="sxs-lookup"><span data-stu-id="52a80-245">Switch toohello **Diagnostics** tab.</span></span>

    ![Átjáró diagnosztika](./media/data-factory-odbc-connector/data-factory-gateway-diagnostics.png)
3. <span data-ttu-id="52a80-247">Jelölje be hello **típus** adatok tárolására (társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="52a80-247">Select hello **type** of data store (linked service).</span></span>
4. <span data-ttu-id="52a80-248">Adja meg **hitelesítési** , és írja be **hitelesítő adatok** (vagy) megadása **kapcsolati karakterlánc** használt tooconnect toohello adattár ez.</span><span class="sxs-lookup"><span data-stu-id="52a80-248">Specify **authentication** and enter **credentials** (or) enter **connection string** that is used tooconnect toohello data store.</span></span>
5. <span data-ttu-id="52a80-249">Kattintson a **tesztkapcsolat** tootest hello kapcsolat toohello adattár.</span><span class="sxs-lookup"><span data-stu-id="52a80-249">Click **Test connection** tootest hello connection toohello data store.</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="52a80-250">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="52a80-250">Performance and Tuning</span></span>
<span data-ttu-id="52a80-251">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.</span><span class="sxs-lookup"><span data-stu-id="52a80-251">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
