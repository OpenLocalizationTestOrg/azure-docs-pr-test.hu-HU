---
title: "az Azure Data Factory használatával fájlrendszer aaaCopy adatok |} Microsoft Docs"
description: "Megtudhatja, hogyan toocopy adatok tooand egy helyszíni fájlrendszerből Azure Data Factory használatával."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: ce19f1ae-358e-4ffc-8a80-d802505c9c84
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jingwang
ms.openlocfilehash: 201b8bc3ffa639df781443aa0c3f95c975d280be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooand-from-an-on-premises-file-system-by-using-azure-data-factory"></a><span data-ttu-id="f30c7-103">Adatok tooand másolása egy helyszíni fájlrendszer Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="f30c7-103">Copy data tooand from an on-premises file system by using Azure Data Factory</span></span>
<span data-ttu-id="f30c7-104">Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toocopy adatok belőle egy helyszíni fájlrendszer a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="f30c7-104">This article explains how toouse hello Copy Activity in Azure Data Factory toocopy data to/from an on-premises file system.</span></span> <span data-ttu-id="f30c7-105">-Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="f30c7-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="f30c7-106">Támogatott helyzetek</span><span class="sxs-lookup"><span data-stu-id="f30c7-106">Supported scenarios</span></span>
<span data-ttu-id="f30c7-107">Adatokat másolhat **egy helyi fájl rendszerből** toohello a következő adatokat tárolja:</span><span class="sxs-lookup"><span data-stu-id="f30c7-107">You can copy data **from an on-premises file system** toohello following data stores:</span></span>

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="f30c7-108">Adatok másolása a következő adatokat tárolja hello **tooan helyszíni fájlrendszer**:</span><span class="sxs-lookup"><span data-stu-id="f30c7-108">You can copy data from hello following data stores **tooan on-premises file system**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> <span data-ttu-id="f30c7-109">Másolási tevékenység nem hello forrásfájl törlése, miután sikeresen másolt toohello cél.</span><span class="sxs-lookup"><span data-stu-id="f30c7-109">Copy Activity does not delete hello source file after it is successfully copied toohello destination.</span></span> <span data-ttu-id="f30c7-110">Ha toodelete hello forrásfájl után sikeres másolatát, hozzon létre egy egyéni tevékenység toodelete hello fájlt, és a hello tevékenység hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="f30c7-110">If you need toodelete hello source file after a successful copy, create a custom activity toodelete hello file and use hello activity in hello pipeline.</span></span> 

## <a name="enabling-connectivity"></a><span data-ttu-id="f30c7-111">Kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f30c7-111">Enabling connectivity</span></span>
<span data-ttu-id="f30c7-112">Adat-előállító támogat egy helyszíni fájlrendszerből keresztül csatlakozó tooand **az adatkezelési átjáró**.</span><span class="sxs-lookup"><span data-stu-id="f30c7-112">Data Factory supports connecting tooand from an on-premises file system via **Data Management Gateway**.</span></span> <span data-ttu-id="f30c7-113">Telepítenie kell az adatkezelési átjáró hello hello adat-előállító szolgáltatás tooconnect tooany támogatott helyszíni adattároló többek között a fájlrendszer a helyszíni környezetben.</span><span class="sxs-lookup"><span data-stu-id="f30c7-113">You must install hello Data Management Gateway in your on-premises environment for hello Data Factory service tooconnect tooany supported on-premises data store including file system.</span></span> <span data-ttu-id="f30c7-114">toolearn az adatkezelési átjáró és hello átjáró beállításának lépéseit lásd: [helyezze át az adatokat a helyszíni adatforrások és az adatkezelési átjáró hello felhő között](data-factory-move-data-between-onprem-and-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="f30c7-114">toolearn about Data Management Gateway and for step-by-step instructions on setting up hello gateway, see [Move data between on-premises sources and hello cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md).</span></span> <span data-ttu-id="f30c7-115">Az adatkezelési átjáró, leszámítva más bináris fájlokat kell telepített toobe toocommunicate tooand egy helyi fájl rendszerből.</span><span class="sxs-lookup"><span data-stu-id="f30c7-115">Apart from Data Management Gateway, no other binary files need toobe installed toocommunicate tooand from an on-premises file system.</span></span> <span data-ttu-id="f30c7-116">Telepítse, és az adatkezelési átjáró hello használja, akkor is, ha fájlrendszer hello Azure IaaS virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="f30c7-116">You must install and use hello Data Management Gateway even if hello file system is in Azure IaaS VM.</span></span> <span data-ttu-id="f30c7-117">Hello átjáró kapcsolatos részletes információkért lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="f30c7-117">For detailed information about hello gateway, see [Data Management Gateway](data-factory-data-management-gateway.md).</span></span>

<span data-ttu-id="f30c7-118">Linux fájlmegosztást, toouse telepítése [Samba](https://www.samba.org/) a Linux-kiszolgálón, telepítse az adatkezelési átjáró a Windows server.</span><span class="sxs-lookup"><span data-stu-id="f30c7-118">toouse a Linux file share, install [Samba](https://www.samba.org/) on your Linux server, and install Data Management Gateway on a Windows server.</span></span> <span data-ttu-id="f30c7-119">Az adatkezelési átjáró telepítése egy Linux-kiszolgálón nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="f30c7-119">Installing Data Management Gateway on a Linux server is not supported.</span></span>

## <a name="getting-started"></a><span data-ttu-id="f30c7-120">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="f30c7-120">Getting started</span></span>
<span data-ttu-id="f30c7-121">A másolási tevékenység, amely helyezi át az adatokat, vagy az operációs rendszer különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="f30c7-121">You can create a pipeline with a copy activity that moves data to/from a file system by using different tools/APIs.</span></span>

<span data-ttu-id="f30c7-122">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="f30c7-122">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="f30c7-123">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="f30c7-123">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="f30c7-124">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon **, **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="f30c7-124">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="f30c7-125">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="f30c7-125">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span>

<span data-ttu-id="f30c7-126">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="f30c7-126">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="f30c7-127">Hozzon létre egy **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="f30c7-127">Create a **data factory**.</span></span> <span data-ttu-id="f30c7-128">Egy adat-előállító tartalmazhat egy vagy több folyamatok.</span><span class="sxs-lookup"><span data-stu-id="f30c7-128">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="f30c7-129">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="f30c7-129">Create **linked services** toolink input and output data stores tooyour data factory.</span></span> <span data-ttu-id="f30c7-130">Adatok másolása az Azure blob storage tooan helyszíni fájlrendszer a, akkor hozzon létre például két összekapcsolt szolgáltatások toolink a helyszíni fájlrendszerben és az Azure storage tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="f30c7-130">For example, if you are copying data from an Azure blob storage tooan on-premises file system, you create two linked services toolink your on-premises file system and Azure storage account tooyour data factory.</span></span> <span data-ttu-id="f30c7-131">Csatolt szolgáltatás tulajdonságait, amelyek adott tooan helyszíni fájlrendszer, lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="f30c7-131">For linked service properties that are specific tooan on-premises file system, see [linked service properties](#linked-service-properties) section.</span></span>
3. <span data-ttu-id="f30c7-132">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="f30c7-132">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> <span data-ttu-id="f30c7-133">Hello utolsó lépésében említett hello például létrehoz egy adatkészlet toospecify hello blobtárolót és hello bemeneti adatokat tartalmazó mappát.</span><span class="sxs-lookup"><span data-stu-id="f30c7-133">In hello example mentioned in hello last step, you create a dataset toospecify hello blob container and folder that contains hello input data.</span></span> <span data-ttu-id="f30c7-134">És egy másik dataset toospecify hello mappa nevét és a fájlrendszert (nem kötelező) létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f30c7-134">And, you create another dataset toospecify hello folder and file name (optional) in your file system.</span></span> <span data-ttu-id="f30c7-135">Adatkészlet tulajdonságai adott tooon helyszíni fájlrendszer, a következő témakörben: [adatkészlet tulajdonságai](#dataset-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="f30c7-135">For dataset properties that are specific tooon-premises file system, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="f30c7-136">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="f30c7-136">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="f30c7-137">A korábban említett hello példában BlobSource forrás-és FileSystemSink akár használhatja a fogadó hello másolási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="f30c7-137">In hello example mentioned earlier, you use BlobSource as a source and FileSystemSink as a sink for hello copy activity.</span></span> <span data-ttu-id="f30c7-138">Hasonlóképpen a helyi fájl rendszer tooAzure Blob Storage másolása, használható FileSystemSource és BlobSink hello másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="f30c7-138">Similarly, if you are copying from on-premises file system tooAzure Blob Storage, you use FileSystemSource and BlobSink in hello copy activity.</span></span> <span data-ttu-id="f30c7-139">A másolási tevékenység tulajdonságai adott tooon helyszíni fájlrendszer, lásd: [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="f30c7-139">For copy activity properties that are specific tooon-premises file system, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="f30c7-140">További információkért hogyan toouse egy adatok tárolót, mint a forrás- és a fogadó hivatkozásra hello az adattároló hello előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="f30c7-140">For details on how toouse a data store as a source or a sink, click hello link in hello previous section for your data store.</span></span>

<span data-ttu-id="f30c7-141">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="f30c7-141">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="f30c7-142">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="f30c7-142">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="f30c7-143">Az adat-előállító entitások, amelyek az operációs rendszer használt toocopy adatok JSON-definíciók minták, lásd: [JSON példák](#json-examples-for-copying-data-to-and-from-file-system) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="f30c7-143">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from a file system, see [JSON examples](#json-examples-for-copying-data-to-and-from-file-system) section of this article.</span></span>

<span data-ttu-id="f30c7-144">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott toofile rendszer részleteit tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="f30c7-144">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific toofile system:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="f30c7-145">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="f30c7-145">Linked service properties</span></span>
<span data-ttu-id="f30c7-146">Egy helyi fájl rendszer tooan az Azure data factory hello társíthatja **a helyi fájlkiszolgáló** társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f30c7-146">You can link an on-premises file system tooan Azure data factory with hello **On-Premises File Server** linked service.</span></span> <span data-ttu-id="f30c7-147">hello a következő táblázat ismerteti, amelyek adott toohello a helyi fájlkiszolgáló társított szolgáltatás JSON-elemek szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="f30c7-147">hello following table provides descriptions for JSON elements that are specific toohello On-Premises File Server linked service.</span></span>

| <span data-ttu-id="f30c7-148">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="f30c7-148">Property</span></span> | <span data-ttu-id="f30c7-149">Leírás</span><span class="sxs-lookup"><span data-stu-id="f30c7-149">Description</span></span> | <span data-ttu-id="f30c7-150">Szükséges</span><span class="sxs-lookup"><span data-stu-id="f30c7-150">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f30c7-151">type</span><span class="sxs-lookup"><span data-stu-id="f30c7-151">type</span></span> |<span data-ttu-id="f30c7-152">Győződjön meg arról, hogy hello típusú tulajdonsága túl**OnPremisesFileServer**.</span><span class="sxs-lookup"><span data-stu-id="f30c7-152">Ensure that hello type property is set too**OnPremisesFileServer**.</span></span> |<span data-ttu-id="f30c7-153">Igen</span><span class="sxs-lookup"><span data-stu-id="f30c7-153">Yes</span></span> |
| <span data-ttu-id="f30c7-154">állomás</span><span class="sxs-lookup"><span data-stu-id="f30c7-154">host</span></span> |<span data-ttu-id="f30c7-155">Hello legfelső szintű megjeleníteni kívánt toocopy hello mappa elérési útját adja meg.</span><span class="sxs-lookup"><span data-stu-id="f30c7-155">Specifies hello root path of hello folder that you want toocopy.</span></span> <span data-ttu-id="f30c7-156">Hello escape-karakter használata "\" hello karakterlánc speciális karakter.</span><span class="sxs-lookup"><span data-stu-id="f30c7-156">Use hello escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="f30c7-157">Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat.</span><span class="sxs-lookup"><span data-stu-id="f30c7-157">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span> |<span data-ttu-id="f30c7-158">Igen</span><span class="sxs-lookup"><span data-stu-id="f30c7-158">Yes</span></span> |
| <span data-ttu-id="f30c7-159">felhasználói azonosítóját</span><span class="sxs-lookup"><span data-stu-id="f30c7-159">userid</span></span> |<span data-ttu-id="f30c7-160">Adja meg a toohello kiszolgáló hello felhasználó hello Azonosítót.</span><span class="sxs-lookup"><span data-stu-id="f30c7-160">Specify hello ID of hello user who has access toohello server.</span></span> |<span data-ttu-id="f30c7-161">Nem (Ha úgy dönt, hogy encryptedCredential)</span><span class="sxs-lookup"><span data-stu-id="f30c7-161">No (if you choose encryptedCredential)</span></span> |
| <span data-ttu-id="f30c7-162">jelszó</span><span class="sxs-lookup"><span data-stu-id="f30c7-162">password</span></span> |<span data-ttu-id="f30c7-163">Adja meg a hello jelszó hello (userid).</span><span class="sxs-lookup"><span data-stu-id="f30c7-163">Specify hello password for hello user (userid).</span></span> |<span data-ttu-id="f30c7-164">Nem (Ha úgy dönt, hogy encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="f30c7-164">No (if you choose encryptedCredential</span></span> |
| <span data-ttu-id="f30c7-165">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="f30c7-165">encryptedCredential</span></span> |<span data-ttu-id="f30c7-166">Adja meg a hello titkosított hitelesítő adatokat, amelyek a parancsmagot a New-AzureRmDataFactoryEncryptValue hello kaphat.</span><span class="sxs-lookup"><span data-stu-id="f30c7-166">Specify hello encrypted credentials that you can get by running hello New-AzureRmDataFactoryEncryptValue cmdlet.</span></span> |<span data-ttu-id="f30c7-167">Nem (Ha úgy dönt, toospecify felhasználói azonosítót és jelszót a szövegként)</span><span class="sxs-lookup"><span data-stu-id="f30c7-167">No (if you choose toospecify userid and password in plain text)</span></span> |
| <span data-ttu-id="f30c7-168">gatewayName</span><span class="sxs-lookup"><span data-stu-id="f30c7-168">gatewayName</span></span> |<span data-ttu-id="f30c7-169">Megadja, hogy a Data Factory kell használnia tooconnect toohello a helyi fájlkiszolgáló hello átjáró hello nevét.</span><span class="sxs-lookup"><span data-stu-id="f30c7-169">Specifies hello name of hello gateway that Data Factory should use tooconnect toohello on-premises file server.</span></span> |<span data-ttu-id="f30c7-170">Igen</span><span class="sxs-lookup"><span data-stu-id="f30c7-170">Yes</span></span> |


### <a name="sample-linked-service-and-dataset-definitions"></a><span data-ttu-id="f30c7-171">Példa társított szolgáltatás és a dataset definíciók</span><span class="sxs-lookup"><span data-stu-id="f30c7-171">Sample linked service and dataset definitions</span></span>
| <span data-ttu-id="f30c7-172">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="f30c7-172">Scenario</span></span> | <span data-ttu-id="f30c7-173">A társított szolgáltatás definíciójának üzemeltetéséhez</span><span class="sxs-lookup"><span data-stu-id="f30c7-173">Host in linked service definition</span></span> | <span data-ttu-id="f30c7-174">Az adatkészlet-definícióban folderPath</span><span class="sxs-lookup"><span data-stu-id="f30c7-174">folderPath in dataset definition</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f30c7-175">Az adatkezelési átjáró gépen helyi mappában:</span><span class="sxs-lookup"><span data-stu-id="f30c7-175">Local folder on Data Management Gateway machine:</span></span> <br/><br/><span data-ttu-id="f30c7-176">Példák: D:\\ \* vagy D:\folder\subfolder\\*</span><span class="sxs-lookup"><span data-stu-id="f30c7-176">Examples: D:\\\* or D:\folder\subfolder\\*</span></span> |<span data-ttu-id="f30c7-177">D:\\ \\ (az adatok felügyeleti átjáró 2.0-s és újabb verziók)</span><span class="sxs-lookup"><span data-stu-id="f30c7-177">D:\\\\ (for Data Management Gateway 2.0 and later versions)</span></span> <br/><br/> <span data-ttu-id="f30c7-178">a localhost (korábbi verzióihoz mint adatok felügyeleti átjáró 2.0-s)</span><span class="sxs-lookup"><span data-stu-id="f30c7-178">localhost (for earlier versions than Data Management Gateway 2.0)</span></span> |<span data-ttu-id="f30c7-179">. \\ \\ vagy mappa\\\\almappa (az adatok felügyeleti átjáró 2.0-s és újabb verziók)</span><span class="sxs-lookup"><span data-stu-id="f30c7-179">.\\\\ or folder\\\\subfolder (for Data Management Gateway 2.0 and later versions)</span></span> <br/><br/><span data-ttu-id="f30c7-180">D:\\ \\ vagy D:\\\\mappa\\\\almappa (az átjáró verziója alatt 2.0-s)</span><span class="sxs-lookup"><span data-stu-id="f30c7-180">D:\\\\ or D:\\\\folder\\\\subfolder (for gateway version below 2.0)</span></span> |
| <span data-ttu-id="f30c7-181">Távoli megosztott mappa:</span><span class="sxs-lookup"><span data-stu-id="f30c7-181">Remote shared folder:</span></span> <br/><br/><span data-ttu-id="f30c7-182">Példák: \\ \\myserver\\megosztása\\ \* vagy \\ \\myserver\\megosztása\\mappa\\almappa\\*</span><span class="sxs-lookup"><span data-stu-id="f30c7-182">Examples: \\\\myserver\\share\\\* or \\\\myserver\\share\\folder\\subfolder\\*</span></span> |<span data-ttu-id="f30c7-183">\\\\\\\\myserver\\\\megosztása</span><span class="sxs-lookup"><span data-stu-id="f30c7-183">\\\\\\\\myserver\\\\share</span></span> |<span data-ttu-id="f30c7-184">. \\ \\ vagy mappa\\\\almappa</span><span class="sxs-lookup"><span data-stu-id="f30c7-184">.\\\\ or folder\\\\subfolder</span></span> |


### <a name="example-using-username-and-password-in-plain-text"></a><span data-ttu-id="f30c7-185">Példa: Felhasználónév és jelszó használatával egyszerű szöveges</span><span class="sxs-lookup"><span data-stu-id="f30c7-185">Example: Using username and password in plain text</span></span>

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

### <a name="example-using-encryptedcredential"></a><span data-ttu-id="f30c7-186">Példa: Encryptedcredential használatával</span><span class="sxs-lookup"><span data-stu-id="f30c7-186">Example: Using encryptedcredential</span></span>

```JSON
{
  "Name": " OnPremisesFileServerLinkedService ",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "D:\\",
      "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
      "gatewayName": "mygateway"
    }
  }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="f30c7-187">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="f30c7-187">Dataset properties</span></span>
<span data-ttu-id="f30c7-188">Illetve meghatározásához adatkészletek rendelkezésre álló tulajdonságok teljes listáját lásd: [adatkészletek létrehozása](data-factory-create-datasets.md).</span><span class="sxs-lookup"><span data-stu-id="f30c7-188">For a full list of sections and properties that are available for defining datasets, see [Creating datasets](data-factory-create-datasets.md).</span></span> <span data-ttu-id="f30c7-189">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében.</span><span class="sxs-lookup"><span data-stu-id="f30c7-189">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="f30c7-190">hello typeProperties szakaszban nem egyezik az adatkészlet egyes típusú.</span><span class="sxs-lookup"><span data-stu-id="f30c7-190">hello typeProperties section is different for each type of dataset.</span></span> <span data-ttu-id="f30c7-191">Például a hello helyét és hello adattár hello adatok formátuma adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f30c7-191">It provides information such as hello location and format of hello data in hello data store.</span></span> <span data-ttu-id="f30c7-192">hello typeProperties szakasz hello adatkészlet típusú **fájlmegosztási** rendelkezik hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="f30c7-192">hello typeProperties section for hello dataset of type **FileShare** has hello following properties:</span></span>

| <span data-ttu-id="f30c7-193">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="f30c7-193">Property</span></span> | <span data-ttu-id="f30c7-194">Leírás</span><span class="sxs-lookup"><span data-stu-id="f30c7-194">Description</span></span> | <span data-ttu-id="f30c7-195">Szükséges</span><span class="sxs-lookup"><span data-stu-id="f30c7-195">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f30c7-196">folderPath</span><span class="sxs-lookup"><span data-stu-id="f30c7-196">folderPath</span></span> |<span data-ttu-id="f30c7-197">Hello részleges toohello mappáját adja meg.</span><span class="sxs-lookup"><span data-stu-id="f30c7-197">Specifies hello subpath toohello folder.</span></span> <span data-ttu-id="f30c7-198">Hello escape-karakter használata "\" hello karakterlánc speciális karakter.</span><span class="sxs-lookup"><span data-stu-id="f30c7-198">Use hello escape character ‘\’ for special characters in hello string.</span></span> <span data-ttu-id="f30c7-199">Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat.</span><span class="sxs-lookup"><span data-stu-id="f30c7-199">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="f30c7-200">Ez a tulajdonság a kombinálhatja **partitionBy** toohave mappák elérési útjaiban szelet alapján kezdő és záró dátum-idő.</span><span class="sxs-lookup"><span data-stu-id="f30c7-200">You can combine this property with **partitionBy** toohave folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="f30c7-201">Igen</span><span class="sxs-lookup"><span data-stu-id="f30c7-201">Yes</span></span> |
| <span data-ttu-id="f30c7-202">fileName</span><span class="sxs-lookup"><span data-stu-id="f30c7-202">fileName</span></span> |<span data-ttu-id="f30c7-203">Meg kell adnia hello fájl hello nevet hello **folderPath** Ha azt szeretné, hogy hello tábla toorefer tooa adott fájl hello mappában.</span><span class="sxs-lookup"><span data-stu-id="f30c7-203">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="f30c7-204">Ha nem ad meg semmilyen értéket ehhez a tulajdonsághoz, hello tábla mutat tooall fájlok hello mappában.</span><span class="sxs-lookup"><span data-stu-id="f30c7-204">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="f30c7-205">Ha **Fájlnév** nincs megadva egy kimeneti adatkészlet és **preserveHierarchy** nincs megadva a tevékenység fogadó hello név hello létrehozott fájl formátuma a következő hello van:</span><span class="sxs-lookup"><span data-stu-id="f30c7-205">When **fileName** is not specified for an output dataset and **preserveHierarchy** is not specified in activity sink, hello name of hello generated file is in hello following format:</span></span> <br/><br/><span data-ttu-id="f30c7-206">`Data.<Guid>.txt`(Példa: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span><span class="sxs-lookup"><span data-stu-id="f30c7-206">`Data.<Guid>.txt` (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span></span> |<span data-ttu-id="f30c7-207">Nem</span><span class="sxs-lookup"><span data-stu-id="f30c7-207">No</span></span> |
| <span data-ttu-id="f30c7-208">fileFilter</span><span class="sxs-lookup"><span data-stu-id="f30c7-208">fileFilter</span></span> |<span data-ttu-id="f30c7-209">Adja meg a szűrő toobe tooselect használja, minden fájl helyett a hello folderPath fájlok egy részét.</span><span class="sxs-lookup"><span data-stu-id="f30c7-209">Specify a filter toobe used tooselect a subset of files in hello folderPath rather than all files.</span></span> <br/><br/><span data-ttu-id="f30c7-210">Két érték engedélyezett: `*` (több karaktert) és `?` (egyetlen karakter).</span><span class="sxs-lookup"><span data-stu-id="f30c7-210">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="f30c7-211">1. példa: "fileFilter": "* .log"</span><span class="sxs-lookup"><span data-stu-id="f30c7-211">Example 1: "fileFilter": "*.log"</span></span><br/><span data-ttu-id="f30c7-212">2. példa: "fileFilter": 2014 - 1-?. txt"</span><span class="sxs-lookup"><span data-stu-id="f30c7-212">Example 2: "fileFilter": 2014-1-?.txt"</span></span><br/><br/><span data-ttu-id="f30c7-213">Vegye figyelembe, hogy fileFilter egy bemeneti fájlmegosztási adatkészlet esetében alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="f30c7-213">Note that fileFilter is applicable for an input FileShare dataset.</span></span> |<span data-ttu-id="f30c7-214">Nem</span><span class="sxs-lookup"><span data-stu-id="f30c7-214">No</span></span> |
| <span data-ttu-id="f30c7-215">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="f30c7-215">partitionedBy</span></span> |<span data-ttu-id="f30c7-216">Az idősorozat adatok partitionedBy toospecify dinamikus folderPath/fileName is használhatja.</span><span class="sxs-lookup"><span data-stu-id="f30c7-216">You can use partitionedBy toospecify a dynamic folderPath/fileName for time-series data.</span></span> <span data-ttu-id="f30c7-217">Példa: az adatok óránkénti paraméteres folderPath.</span><span class="sxs-lookup"><span data-stu-id="f30c7-217">An example is folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="f30c7-218">Nem</span><span class="sxs-lookup"><span data-stu-id="f30c7-218">No</span></span> |
| <span data-ttu-id="f30c7-219">Formátumban</span><span class="sxs-lookup"><span data-stu-id="f30c7-219">format</span></span> | <span data-ttu-id="f30c7-220">a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, ** ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="f30c7-220">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="f30c7-221">Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="f30c7-221">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="f30c7-222">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="f30c7-222">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="f30c7-223">Ha azt szeretné, túl**másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="f30c7-223">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="f30c7-224">Nem</span><span class="sxs-lookup"><span data-stu-id="f30c7-224">No</span></span> |
| <span data-ttu-id="f30c7-225">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="f30c7-225">compression</span></span> | <span data-ttu-id="f30c7-226">Adja meg a hello típusát és hello adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="f30c7-226">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="f30c7-227">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="f30c7-227">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="f30c7-228">Támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="f30c7-228">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="f30c7-229">Lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="f30c7-229">see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="f30c7-230">Nem</span><span class="sxs-lookup"><span data-stu-id="f30c7-230">No</span></span> |

> [!NOTE]
> <span data-ttu-id="f30c7-231">Nem használható egyszerre fájlnév és fileFilter.</span><span class="sxs-lookup"><span data-stu-id="f30c7-231">You cannot use fileName and fileFilter simultaneously.</span></span>

### <a name="using-partitionedby-property"></a><span data-ttu-id="f30c7-232">PartitionedBy tulajdonság használatával</span><span class="sxs-lookup"><span data-stu-id="f30c7-232">Using partitionedBy property</span></span>
<span data-ttu-id="f30c7-233">Hello előző szakaszban említett, megadhatja a dinamikus folderPath és a sorozat időadatok fájlnevét hello **partitionedBy** tulajdonság, [adat-előállító funkciók és hello rendszerváltozók](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="f30c7-233">As mentioned in hello previous section, you can specify a dynamic folderPath and filename for time series data with hello **partitionedBy** property, [Data Factory functions, and hello system variables](data-factory-functions-variables.md).</span></span>

<span data-ttu-id="f30c7-234">toounderstand idősorozat adatkészleteket, az ütemezés és a szeletek, a további részletek: [adatkészletek létrehozása](data-factory-create-datasets.md), [ütemezés és a végrehajtás](data-factory-scheduling-and-execution.md), és [folyamatok létrehozása](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="f30c7-234">toounderstand more details on time-series datasets, scheduling, and slices, see [Creating datasets](data-factory-create-datasets.md), [Scheduling and execution](data-factory-scheduling-and-execution.md), and [Creating pipelines](data-factory-create-pipelines.md).</span></span>

#### <a name="sample-1"></a><span data-ttu-id="f30c7-235">1. példa:</span><span class="sxs-lookup"><span data-stu-id="f30c7-235">Sample 1:</span></span>

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

<span data-ttu-id="f30c7-236">Ebben a példában {szelet} hello értékű hello adat-előállító rendszer változó SliceStart hello formátumban (YYYYMMDDHH) váltja fel.</span><span class="sxs-lookup"><span data-stu-id="f30c7-236">In this example, {Slice} is replaced with hello value of hello Data Factory system variable SliceStart in hello format (YYYYMMDDHH).</span></span> <span data-ttu-id="f30c7-237">SliceStart toostart idő hello szelet hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="f30c7-237">SliceStart refers toostart time of hello slice.</span></span> <span data-ttu-id="f30c7-238">hello folderPath nem azonos az egyes szeletek.</span><span class="sxs-lookup"><span data-stu-id="f30c7-238">hello folderPath is different for each slice.</span></span> <span data-ttu-id="f30c7-239">Például: wikidatagateway/wikisampledataout/2014100103 vagy wikidatagateway/wikisampledataout/2014100104.</span><span class="sxs-lookup"><span data-stu-id="f30c7-239">For example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.</span></span>

#### <a name="sample-2"></a><span data-ttu-id="f30c7-240">2. példa:</span><span class="sxs-lookup"><span data-stu-id="f30c7-240">Sample 2:</span></span>

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```

<span data-ttu-id="f30c7-241">Ebben a példában év, hónap, nap és SliceStart idején ki kell olvasni a hello folderPath és a fájlnév tulajdonságot használó külön változók.</span><span class="sxs-lookup"><span data-stu-id="f30c7-241">In this example, year, month, day, and time of SliceStart are extracted into separate variables that hello folderPath and fileName properties use.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="f30c7-242">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="f30c7-242">Copy activity properties</span></span>
<span data-ttu-id="f30c7-243">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="f30c7-243">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="f30c7-244">Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti adatkészletek és házirendek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="f30c7-244">Properties such as name, description, input and output datasets, and policies are available for all types of activities.</span></span> <span data-ttu-id="f30c7-245">Mivel tulajdonságok érhetők el hello **typeProperties** hello tevékenység szakasza tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="f30c7-245">Whereas, properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span>

<span data-ttu-id="f30c7-246">A másolási tevékenység során két érték források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="f30c7-246">For Copy activity, they vary depending on hello types of sources and sinks.</span></span> <span data-ttu-id="f30c7-247">Ha adatok egy helyi fájl rendszerből, beállítása hello forrástípus hello másolási tevékenység túl**FileSystemSource**.</span><span class="sxs-lookup"><span data-stu-id="f30c7-247">If you are moving data from an on-premises file system, you set hello source type in hello copy activity too**FileSystemSource**.</span></span> <span data-ttu-id="f30c7-248">Hasonlóképpen, ha adatokat tooan helyszíni fájlrendszer, beállítása hello a fogadó típusa másolási tevékenység hello túl**FileSystemSink**.</span><span class="sxs-lookup"><span data-stu-id="f30c7-248">Similarly, if you are moving data tooan on-premises file system, you set hello sink type in hello copy activity too**FileSystemSink**.</span></span> <span data-ttu-id="f30c7-249">Ez a témakör FileSystemSource és FileSystemSink által támogatott tulajdonságokról.</span><span class="sxs-lookup"><span data-stu-id="f30c7-249">This section provides a list of properties supported by FileSystemSource and FileSystemSink.</span></span>

<span data-ttu-id="f30c7-250">**FileSystemSource** következő tulajdonságai hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="f30c7-250">**FileSystemSource** supports hello following properties:</span></span>

| <span data-ttu-id="f30c7-251">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="f30c7-251">Property</span></span> | <span data-ttu-id="f30c7-252">Leírás</span><span class="sxs-lookup"><span data-stu-id="f30c7-252">Description</span></span> | <span data-ttu-id="f30c7-253">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="f30c7-253">Allowed values</span></span> | <span data-ttu-id="f30c7-254">Szükséges</span><span class="sxs-lookup"><span data-stu-id="f30c7-254">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="f30c7-255">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="f30c7-255">recursive</span></span> |<span data-ttu-id="f30c7-256">Azt jelzi, hogy hello adatolvasás rekurzív módon hello almappák vagy csak a megadott mappa hello.</span><span class="sxs-lookup"><span data-stu-id="f30c7-256">Indicates whether hello data is read recursively from hello subfolders or only from hello specified folder.</span></span> |<span data-ttu-id="f30c7-257">IGAZ, hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="f30c7-257">True, False (default)</span></span> |<span data-ttu-id="f30c7-258">Nem</span><span class="sxs-lookup"><span data-stu-id="f30c7-258">No</span></span> |

<span data-ttu-id="f30c7-259">**FileSystemSink** következő tulajdonságai hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="f30c7-259">**FileSystemSink** supports hello following properties:</span></span>

| <span data-ttu-id="f30c7-260">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="f30c7-260">Property</span></span> | <span data-ttu-id="f30c7-261">Leírás</span><span class="sxs-lookup"><span data-stu-id="f30c7-261">Description</span></span> | <span data-ttu-id="f30c7-262">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="f30c7-262">Allowed values</span></span> | <span data-ttu-id="f30c7-263">Szükséges</span><span class="sxs-lookup"><span data-stu-id="f30c7-263">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="f30c7-264">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="f30c7-264">copyBehavior</span></span> |<span data-ttu-id="f30c7-265">Hello másolási viselkedését határozza meg, ha hello adatforrás BlobSource vagy a fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="f30c7-265">Defines hello copy behavior when hello source is BlobSource or FileSystem.</span></span> |<span data-ttu-id="f30c7-266">**PreserveHierarchy:** hello fájl hierarchia hello célmappában megőrzi.</span><span class="sxs-lookup"><span data-stu-id="f30c7-266">**PreserveHierarchy:** Preserves hello file hierarchy in hello target folder.</span></span> <span data-ttu-id="f30c7-267">Ez azt jelenti, hogy van hello ugyanaz, mint a hello relatív elérési útja hello cél fájl toohello célmappa hello hello forrás fájl toohello forrásmappa relatív elérési út.</span><span class="sxs-lookup"><span data-stu-id="f30c7-267">That is, hello relative path of hello source file toohello source folder is hello same as hello relative path of hello target file toohello target folder.</span></span><br/><br/><span data-ttu-id="f30c7-268">**FlattenHierarchy:** hello forrásmappából minden fájl első szintű hello célmappában jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="f30c7-268">**FlattenHierarchy:** All files from hello source folder are created in hello first level of target folder.</span></span> <span data-ttu-id="f30c7-269">hello fájljaira jönnek létre automatikusan létrehozott névvel.</span><span class="sxs-lookup"><span data-stu-id="f30c7-269">hello target files are created with an autogenerated name.</span></span><br/><br/><span data-ttu-id="f30c7-270">**Mergefiles típusú:** forrásfájlból hello mappa tooone minden fájl egyesíti.</span><span class="sxs-lookup"><span data-stu-id="f30c7-270">**MergeFiles:** Merges all files from hello source folder tooone file.</span></span> <span data-ttu-id="f30c7-271">Ha hello fájl neve/blob neve meg van adva, a hello egyesített fájl neve az hello megadott név.</span><span class="sxs-lookup"><span data-stu-id="f30c7-271">If hello file name/blob name is specified, hello merged file name is hello specified name.</span></span> <span data-ttu-id="f30c7-272">Ellenkező esetben egy automatikusan létrehozott nevét.</span><span class="sxs-lookup"><span data-stu-id="f30c7-272">Otherwise, it is an auto-generated file name.</span></span> |<span data-ttu-id="f30c7-273">Nem</span><span class="sxs-lookup"><span data-stu-id="f30c7-273">No</span></span> |

### <a name="recursive-and-copybehavior-examples"></a><span data-ttu-id="f30c7-274">rekurzív és copyBehavior példák</span><span class="sxs-lookup"><span data-stu-id="f30c7-274">recursive and copyBehavior examples</span></span>
<span data-ttu-id="f30c7-275">Ez a szakasz ismerteti a hello viselkedésről hello másolási műveletek kombinációk hello rekurzív és a copyBehavior tulajdonság értéktartománya.</span><span class="sxs-lookup"><span data-stu-id="f30c7-275">This section describes hello resulting behavior of hello Copy operation for different combinations of values for hello recursive and copyBehavior properties.</span></span>

| <span data-ttu-id="f30c7-276">rekurzív érték</span><span class="sxs-lookup"><span data-stu-id="f30c7-276">recursive value</span></span> | <span data-ttu-id="f30c7-277">copyBehavior érték</span><span class="sxs-lookup"><span data-stu-id="f30c7-277">copyBehavior value</span></span> | <span data-ttu-id="f30c7-278">Viselkedésről</span><span class="sxs-lookup"><span data-stu-id="f30c7-278">Resulting behavior</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f30c7-279">Igaz</span><span class="sxs-lookup"><span data-stu-id="f30c7-279">true</span></span> |<span data-ttu-id="f30c7-280">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="f30c7-280">preserveHierarchy</span></span> |<span data-ttu-id="f30c7-281">A forrásmappa mappa1 a struktúra, a következő hello</span><span class="sxs-lookup"><span data-stu-id="f30c7-281">For a source folder Folder1 with hello following structure,</span></span><br/><br/><span data-ttu-id="f30c7-282">Mappa1</span><span class="sxs-lookup"><span data-stu-id="f30c7-282">Folder1</span></span><br/><span data-ttu-id="f30c7-283">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="f30c7-283">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="f30c7-284">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="f30c7-284">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="f30c7-285">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="f30c7-285">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="f30c7-286">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="f30c7-286">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="f30c7-287">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="f30c7-287">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="f30c7-288">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="f30c7-288">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="f30c7-289">hello tároló mappa mappa1 azonos szerkezeti hello forrásaként hello jön létre:</span><span class="sxs-lookup"><span data-stu-id="f30c7-289">hello target folder Folder1 is created with hello same structure as hello source:</span></span><br/><br/><span data-ttu-id="f30c7-290">Mappa1</span><span class="sxs-lookup"><span data-stu-id="f30c7-290">Folder1</span></span><br/><span data-ttu-id="f30c7-291">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="f30c7-291">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="f30c7-292">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="f30c7-292">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="f30c7-293">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="f30c7-293">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="f30c7-294">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="f30c7-294">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="f30c7-295">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="f30c7-295">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="f30c7-296">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="f30c7-296">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span> |
| <span data-ttu-id="f30c7-297">Igaz</span><span class="sxs-lookup"><span data-stu-id="f30c7-297">true</span></span> |<span data-ttu-id="f30c7-298">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="f30c7-298">flattenHierarchy</span></span> |<span data-ttu-id="f30c7-299">A forrásmappa mappa1 a struktúra, a következő hello</span><span class="sxs-lookup"><span data-stu-id="f30c7-299">For a source folder Folder1 with hello following structure,</span></span><br/><br/><span data-ttu-id="f30c7-300">Mappa1</span><span class="sxs-lookup"><span data-stu-id="f30c7-300">Folder1</span></span><br/><span data-ttu-id="f30c7-301">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="f30c7-301">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="f30c7-302">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="f30c7-302">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="f30c7-303">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="f30c7-303">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="f30c7-304">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="f30c7-304">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="f30c7-305">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="f30c7-305">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="f30c7-306">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="f30c7-306">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="f30c7-307">a következő struktúra hello hello cél mappa1 jön létre:</span><span class="sxs-lookup"><span data-stu-id="f30c7-307">hello target Folder1 is created with hello following structure:</span></span> <br/><br/><span data-ttu-id="f30c7-308">Mappa1</span><span class="sxs-lookup"><span data-stu-id="f30c7-308">Folder1</span></span><br/><span data-ttu-id="f30c7-309">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet a file1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="f30c7-309">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="f30c7-310">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File2</span><span class="sxs-lookup"><span data-stu-id="f30c7-310">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><span data-ttu-id="f30c7-311">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet fájl3</span><span class="sxs-lookup"><span data-stu-id="f30c7-311">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File3</span></span><br/><span data-ttu-id="f30c7-312">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File4</span><span class="sxs-lookup"><span data-stu-id="f30c7-312">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File4</span></span><br/><span data-ttu-id="f30c7-313">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File5</span><span class="sxs-lookup"><span data-stu-id="f30c7-313">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File5</span></span> |
| <span data-ttu-id="f30c7-314">Igaz</span><span class="sxs-lookup"><span data-stu-id="f30c7-314">true</span></span> |<span data-ttu-id="f30c7-315">mergefiles típusú</span><span class="sxs-lookup"><span data-stu-id="f30c7-315">mergeFiles</span></span> |<span data-ttu-id="f30c7-316">A forrásmappa mappa1 a struktúra, a következő hello</span><span class="sxs-lookup"><span data-stu-id="f30c7-316">For a source folder Folder1 with hello following structure,</span></span><br/><br/><span data-ttu-id="f30c7-317">Mappa1</span><span class="sxs-lookup"><span data-stu-id="f30c7-317">Folder1</span></span><br/><span data-ttu-id="f30c7-318">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="f30c7-318">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="f30c7-319">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="f30c7-319">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="f30c7-320">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="f30c7-320">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="f30c7-321">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="f30c7-321">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="f30c7-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="f30c7-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="f30c7-323">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="f30c7-323">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="f30c7-324">a következő struktúra hello hello cél mappa1 jön létre:</span><span class="sxs-lookup"><span data-stu-id="f30c7-324">hello target Folder1 is created with hello following structure:</span></span> <br/><br/><span data-ttu-id="f30c7-325">Mappa1</span><span class="sxs-lookup"><span data-stu-id="f30c7-325">Folder1</span></span><br/><span data-ttu-id="f30c7-326">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + fájl3 + File4 + 5 fájl tartalmát egy fájlba, egy fájl automatikusan létrehozott névvel egyesítve lesznek.</span><span class="sxs-lookup"><span data-stu-id="f30c7-326">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + File 5 contents are merged into one file with an auto-generated file name.</span></span> |
| <span data-ttu-id="f30c7-327">hamis</span><span class="sxs-lookup"><span data-stu-id="f30c7-327">false</span></span> |<span data-ttu-id="f30c7-328">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="f30c7-328">preserveHierarchy</span></span> |<span data-ttu-id="f30c7-329">A forrásmappa mappa1 a struktúra, a következő hello</span><span class="sxs-lookup"><span data-stu-id="f30c7-329">For a source folder Folder1 with hello following structure,</span></span><br/><br/><span data-ttu-id="f30c7-330">Mappa1</span><span class="sxs-lookup"><span data-stu-id="f30c7-330">Folder1</span></span><br/><span data-ttu-id="f30c7-331">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="f30c7-331">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="f30c7-332">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="f30c7-332">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="f30c7-333">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="f30c7-333">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="f30c7-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="f30c7-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="f30c7-335">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="f30c7-335">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="f30c7-336">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="f30c7-336">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="f30c7-337">a következő struktúra hello hello tároló mappa mappa1 jön létre:</span><span class="sxs-lookup"><span data-stu-id="f30c7-337">hello target folder Folder1 is created with hello following structure:</span></span><br/><br/><span data-ttu-id="f30c7-338">Mappa1</span><span class="sxs-lookup"><span data-stu-id="f30c7-338">Folder1</span></span><br/><span data-ttu-id="f30c7-339">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="f30c7-339">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="f30c7-340">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="f30c7-340">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><br/><span data-ttu-id="f30c7-341">Fájl3, File4 és File5 Subfolder1 nem felvételre.</span><span class="sxs-lookup"><span data-stu-id="f30c7-341">Subfolder1 with File3, File4, and File5 is not picked up.</span></span> |
| <span data-ttu-id="f30c7-342">hamis</span><span class="sxs-lookup"><span data-stu-id="f30c7-342">false</span></span> |<span data-ttu-id="f30c7-343">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="f30c7-343">flattenHierarchy</span></span> |<span data-ttu-id="f30c7-344">A forrásmappa mappa1 a struktúra, a következő hello</span><span class="sxs-lookup"><span data-stu-id="f30c7-344">For a source folder Folder1 with hello following structure,</span></span><br/><br/><span data-ttu-id="f30c7-345">Mappa1</span><span class="sxs-lookup"><span data-stu-id="f30c7-345">Folder1</span></span><br/><span data-ttu-id="f30c7-346">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="f30c7-346">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="f30c7-347">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="f30c7-347">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="f30c7-348">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="f30c7-348">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="f30c7-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="f30c7-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="f30c7-350">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="f30c7-350">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="f30c7-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="f30c7-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="f30c7-352">a következő struktúra hello hello tároló mappa mappa1 jön létre:</span><span class="sxs-lookup"><span data-stu-id="f30c7-352">hello target folder Folder1 is created with hello following structure:</span></span><br/><br/><span data-ttu-id="f30c7-353">Mappa1</span><span class="sxs-lookup"><span data-stu-id="f30c7-353">Folder1</span></span><br/><span data-ttu-id="f30c7-354">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet a file1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="f30c7-354">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="f30c7-355">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File2</span><span class="sxs-lookup"><span data-stu-id="f30c7-355">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><br/><span data-ttu-id="f30c7-356">Fájl3, File4 és File5 Subfolder1 nem felvételre.</span><span class="sxs-lookup"><span data-stu-id="f30c7-356">Subfolder1 with File3, File4, and File5 is not picked up.</span></span> |
| <span data-ttu-id="f30c7-357">hamis</span><span class="sxs-lookup"><span data-stu-id="f30c7-357">false</span></span> |<span data-ttu-id="f30c7-358">mergefiles típusú</span><span class="sxs-lookup"><span data-stu-id="f30c7-358">mergeFiles</span></span> |<span data-ttu-id="f30c7-359">A forrásmappa mappa1 a struktúra, a következő hello</span><span class="sxs-lookup"><span data-stu-id="f30c7-359">For a source folder Folder1 with hello following structure,</span></span><br/><br/><span data-ttu-id="f30c7-360">Mappa1</span><span class="sxs-lookup"><span data-stu-id="f30c7-360">Folder1</span></span><br/><span data-ttu-id="f30c7-361">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="f30c7-361">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="f30c7-362">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="f30c7-362">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="f30c7-363">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="f30c7-363">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="f30c7-364">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="f30c7-364">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="f30c7-365">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="f30c7-365">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="f30c7-366">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="f30c7-366">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="f30c7-367">a következő struktúra hello hello tároló mappa mappa1 jön létre:</span><span class="sxs-lookup"><span data-stu-id="f30c7-367">hello target folder Folder1 is created with hello following structure:</span></span><br/><br/><span data-ttu-id="f30c7-368">Mappa1</span><span class="sxs-lookup"><span data-stu-id="f30c7-368">Folder1</span></span><br/><span data-ttu-id="f30c7-369">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 tartalma egyesítődnek fájl automatikusan létrehozott névvel egy fájlba.</span><span class="sxs-lookup"><span data-stu-id="f30c7-369">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 contents are merged into one file with an auto-generated file name.</span></span><br/><span data-ttu-id="f30c7-370">&nbsp;&nbsp;&nbsp;&nbsp;Automatikusan létrehozott nevet a file1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="f30c7-370">&nbsp;&nbsp;&nbsp;&nbsp;Auto-generated name for File1</span></span><br/><br/><span data-ttu-id="f30c7-371">Fájl3, File4 és File5 Subfolder1 nem felvételre.</span><span class="sxs-lookup"><span data-stu-id="f30c7-371">Subfolder1 with File3, File4, and File5 is not picked up.</span></span> |

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="f30c7-372">Támogatott formátumú és tömörítés</span><span class="sxs-lookup"><span data-stu-id="f30c7-372">Supported file and compression formats</span></span>
<span data-ttu-id="f30c7-373">Lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md) cikk részletei.</span><span class="sxs-lookup"><span data-stu-id="f30c7-373">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-examples-for-copying-data-tooand-from-file-system"></a><span data-ttu-id="f30c7-374">Az adatok tooand Másolás a fájlrendszerből JSON példák</span><span class="sxs-lookup"><span data-stu-id="f30c7-374">JSON examples for copying data tooand from file system</span></span>
<span data-ttu-id="f30c7-375">hello alábbi példák megadják minta JSON-definíciók használható toocreate adatcsatorna hello segítségével [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="f30c7-375">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using hello [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="f30c7-376">Azok hogyan toocopy adatok tooand egy a helyszíni fájlrendszerben és az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="f30c7-376">They show how toocopy data tooand from an on-premises file system and Azure Blob storage.</span></span> <span data-ttu-id="f30c7-377">Adatok másolása azonban *közvetlenül* bármelyik hello források tooany felsorolt hello nyelő [támogatott források és mosdók](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="f30c7-377">However, you can copy data *directly* from any of hello sources tooany of hello sinks listed in [Supported sources and sinks](data-factory-data-movement-activities.md#supported-data-stores-and-formats) by using Copy Activity in Azure Data Factory.</span></span>

### <a name="example-copy-data-from-an-on-premises-file-system-tooazure-blob-storage"></a><span data-ttu-id="f30c7-378">Példa: Adatok másolása egy helyi fájl rendszer tooAzure Blob-tároló</span><span class="sxs-lookup"><span data-stu-id="f30c7-378">Example: Copy data from an on-premises file system tooAzure Blob storage</span></span>
<span data-ttu-id="f30c7-379">Ez a példa bemutatja, hogyan egy helyi fájl rendszer tooAzure Blob-tároló toocopy adatait.</span><span class="sxs-lookup"><span data-stu-id="f30c7-379">This sample shows how toocopy data from an on-premises file system tooAzure Blob storage.</span></span> <span data-ttu-id="f30c7-380">hello minta a következő adat-előállító entitások hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="f30c7-380">hello sample has hello following Data Factory entities:</span></span>

* <span data-ttu-id="f30c7-381">A társított szolgáltatás típusa [OnPremisesFileServer](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="f30c7-381">A linked service of type [OnPremisesFileServer](#linked-service-properties).</span></span>
* <span data-ttu-id="f30c7-382">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="f30c7-382">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="f30c7-383">Bemeneti [dataset](data-factory-create-datasets.md) típusú [fájlmegosztási](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="f30c7-383">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties).</span></span>
* <span data-ttu-id="f30c7-384">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="f30c7-384">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="f30c7-385">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [FileSystemSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="f30c7-385">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="f30c7-386">a következő minta hello átmásolja-idősoros adatok egy helyi fájl rendszer tooAzure Blob-tároló minden órában.</span><span class="sxs-lookup"><span data-stu-id="f30c7-386">hello following sample copies time-series data from an on-premises file system tooAzure Blob storage every hour.</span></span> <span data-ttu-id="f30c7-387">Ezeket a mintákat a használt hello JSON-tulajdonságok hello minta után hello szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="f30c7-387">hello JSON properties that are used in these samples are described in hello sections after hello samples.</span></span>

<span data-ttu-id="f30c7-388">Első lépésként állítsa be az adatkezelési átjáró szerint hello utasításait [helyezze át az adatokat a helyszíni adatforrások és az adatkezelési átjáró hello felhő között](data-factory-move-data-between-onprem-and-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="f30c7-388">As a first step, set up Data Management Gateway as per hello instructions in [Move data between on-premises sources and hello cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md).</span></span>

<span data-ttu-id="f30c7-389">**A helyi fájlkiszolgáló társított szolgáltatáshoz:**</span><span class="sxs-lookup"><span data-stu-id="f30c7-389">**On-Premises File Server linked service:**</span></span>

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

<span data-ttu-id="f30c7-390">Azt javasoljuk, hello **encryptedCredential** tulajdonság helyette hello **userid** és **jelszó** tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="f30c7-390">We recommend using hello **encryptedCredential** property instead hello **userid** and **password** properties.</span></span> <span data-ttu-id="f30c7-391">Lásd: [fájlkiszolgáló társított szolgáltatás](#linked-service-properties) részleteiről a társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f30c7-391">See [File Server linked service](#linked-service-properties) for details about this linked service.</span></span>

<span data-ttu-id="f30c7-392">**Az Azure tárolás társított szolgáltatásának:**</span><span class="sxs-lookup"><span data-stu-id="f30c7-392">**Azure Storage linked service:**</span></span>

```JSON
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

<span data-ttu-id="f30c7-393">**A helyi rendszer bemeneti adatkészlet fájl:**</span><span class="sxs-lookup"><span data-stu-id="f30c7-393">**On-premises file system input dataset:**</span></span>

<span data-ttu-id="f30c7-394">Adatok van felvett egy új fájl óránként.</span><span class="sxs-lookup"><span data-stu-id="f30c7-394">Data is picked up from a new file every hour.</span></span> <span data-ttu-id="f30c7-395">hello folderPath, és a fájlnév tulajdonságok alapján hello szelet hello kezdési idejét határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f30c7-395">hello folderPath and fileName properties are determined based on hello start time of hello slice.</span></span>  

<span data-ttu-id="f30c7-396">Beállítás `"external": "true"` adat-előállító tájékoztatja, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="f30c7-396">Setting `"external": "true"` informs Data Factory that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```JSON
{
  "name": "OnpremisesFileSystemInput",
  "properties": {
    "type": " FileShare",
    "linkedServiceName": " OnPremisesFileServerLinkedService ",
    "typeProperties": {
      "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
      ]
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

<span data-ttu-id="f30c7-397">**Az Azure Blob storage kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="f30c7-397">**Azure Blob storage output dataset:**</span></span>

<span data-ttu-id="f30c7-398">Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="f30c7-398">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="f30c7-399">hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="f30c7-399">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="f30c7-400">hello mappa elérési útját használja hello év, hónap, nap és óra részei hello kezdési ideje.</span><span class="sxs-lookup"><span data-stu-id="f30c7-400">hello folder path uses hello year, month, day, and hour parts of hello start time.</span></span>

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

<span data-ttu-id="f30c7-401">**A másolási tevékenység során a fájlrendszer és a Blob fogadó folyamat:**</span><span class="sxs-lookup"><span data-stu-id="f30c7-401">**A copy activity in a pipeline with File System source and Blob sink:**</span></span>

<span data-ttu-id="f30c7-402">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek, és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="f30c7-402">hello pipeline contains a copy activity that is configured toouse hello input and output datasets, and is scheduled toorun every hour.</span></span> <span data-ttu-id="f30c7-403">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**FileSystemSource**, és **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="f30c7-403">In hello pipeline JSON definition, hello **source** type is set too**FileSystemSource**, and **sink** type is set too**BlobSink**.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-06-01T18:00:00",
    "end":"2015-06-01T19:00:00",
    "description":"Pipeline for copy activity",
    "activities":[  
      {
        "name": "OnpremisesFileSystemtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "OnpremisesFileSystemInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "FileSystemSource"
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

### <a name="example-copy-data-from-azure-sql-database-tooan-on-premises-file-system"></a><span data-ttu-id="f30c7-404">Példa: Adatok másolása az Azure SQL Database tooan helyszíni fájlrendszer</span><span class="sxs-lookup"><span data-stu-id="f30c7-404">Example: Copy data from Azure SQL Database tooan on-premises file system</span></span>
<span data-ttu-id="f30c7-405">a következő példa azt mutatja be hello:</span><span class="sxs-lookup"><span data-stu-id="f30c7-405">hello following sample shows:</span></span>

* <span data-ttu-id="f30c7-406">A társított szolgáltatás típusa [AzureSqlDatabase.](data-factory-azure-sql-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="f30c7-406">A linked service of type [AzureSqlDatabase.](data-factory-azure-sql-connector.md#linked-service-properties)</span></span>
* <span data-ttu-id="f30c7-407">A társított szolgáltatás típusa [OnPremisesFileServer](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="f30c7-407">A linked service of type [OnPremisesFileServer](#linked-service-properties).</span></span>
* <span data-ttu-id="f30c7-408">Egy bemeneti adatkészlet típusú [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="f30c7-408">An input dataset of type [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="f30c7-409">Egy kimeneti adatkészlet típusú [fájlmegosztási](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="f30c7-409">An output dataset of type [FileShare](#dataset-properties).</span></span>
* <span data-ttu-id="f30c7-410">A másolási tevékenység által használt az adatcsatorna [SqlSource](data-factory-azure-sql-connector.md##copy-activity-properties) és [FileSystemSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="f30c7-410">A pipeline with a copy activity that uses [SqlSource](data-factory-azure-sql-connector.md##copy-activity-properties) and [FileSystemSink](#copy-activity-properties).</span></span>

<span data-ttu-id="f30c7-411">hello minta másol idősorozat adatokat az Azure SQL táblázat tooan helyszíni fájlrendszer minden órában.</span><span class="sxs-lookup"><span data-stu-id="f30c7-411">hello sample copies time-series data from an Azure SQL table tooan on-premises file system every hour.</span></span> <span data-ttu-id="f30c7-412">Ezeket a mintákat a használt hello JSON-tulajdonságok hello minta után szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="f30c7-412">hello JSON properties that are used in these samples are described in sections after hello samples.</span></span>

<span data-ttu-id="f30c7-413">**A társított szolgáltatásnak Azure SQL Database:**</span><span class="sxs-lookup"><span data-stu-id="f30c7-413">**Azure SQL Database linked service:**</span></span>

```JSON
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```

<span data-ttu-id="f30c7-414">**A helyi fájlkiszolgáló társított szolgáltatáshoz:**</span><span class="sxs-lookup"><span data-stu-id="f30c7-414">**On-Premises File Server linked service:**</span></span>

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

<span data-ttu-id="f30c7-415">Azt javasoljuk, hello **encryptedCredential** hello használata helyett tulajdonság **userid** és **jelszó** tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="f30c7-415">We recommend using hello **encryptedCredential** property instead of using hello **userid** and **password** properties.</span></span> <span data-ttu-id="f30c7-416">Lásd: [fájlrendszer társított szolgáltatás](#linked-service-properties) részleteiről a társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f30c7-416">See [File System linked service](#linked-service-properties) for details about this linked service.</span></span>

<span data-ttu-id="f30c7-417">**Az Azure SQL bemeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="f30c7-417">**Azure SQL input dataset:**</span></span>

<span data-ttu-id="f30c7-418">hello minta feltételezi, hogy létrehozott egy tábla "MyTable" Azure SQL-ben, és egy idősorozat adatok "timestampcolumn" nevű oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f30c7-418">hello sample assumes that you've created a table “MyTable” in Azure SQL, and it contains a column called “timestampcolumn” for time-series data.</span></span>

<span data-ttu-id="f30c7-419">Beállítás ``“external”: ”true”`` adat-előállító tájékoztatja, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="f30c7-419">Setting ``“external”: ”true”`` informs Data Factory that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```JSON
{
  "name": "AzureSqlInput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
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

<span data-ttu-id="f30c7-420">**A helyszíni rendszer kimeneti adatkészlet fájl:**</span><span class="sxs-lookup"><span data-stu-id="f30c7-420">**On-premises file system output dataset:**</span></span>

<span data-ttu-id="f30c7-421">Adata másolt tooa új fájl óránként.</span><span class="sxs-lookup"><span data-stu-id="f30c7-421">Data is copied tooa new file every hour.</span></span> <span data-ttu-id="f30c7-422">hello folderPath és hello blob fájlnevét határozza meg hello szelet hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="f30c7-422">hello folderPath and fileName for hello blob are determined based on hello start time of hello slice.</span></span>

```JSON
{
  "name": "OnpremisesFileSystemOutput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": " OnPremisesFileServerLinkedService ",
    "typeProperties": {
      "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
      ]
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

<span data-ttu-id="f30c7-423">**A másolási tevékenység során egy SQL-forrás és fogadó fájlrendszer folyamaton belül:**</span><span class="sxs-lookup"><span data-stu-id="f30c7-423">**A copy activity in a pipeline with SQL source and File System sink:**</span></span>

<span data-ttu-id="f30c7-424">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek, és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="f30c7-424">hello pipeline contains a copy activity that is configured toouse hello input and output datasets, and is scheduled toorun every hour.</span></span> <span data-ttu-id="f30c7-425">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**SqlSource**, és hello **fogadó** típusuk értéke túl**FileSystemSink**.</span><span class="sxs-lookup"><span data-stu-id="f30c7-425">In hello pipeline JSON definition, hello **source** type is set too**SqlSource**, and hello **sink** type is set too**FileSystemSink**.</span></span> <span data-ttu-id="f30c7-426">hello SQL-lekérdezésben megadott hello **SqlReaderQuery** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.</span><span class="sxs-lookup"><span data-stu-id="f30c7-426">hello SQL query that is specified for hello **SqlReaderQuery** property selects hello data in hello past hour toocopy.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-06-01T18:00:00",
    "end":"2015-06-01T20:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "AzureSQLtoOnPremisesFile",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSQLInput"
          }
        ],
        "outputs": [
          {
            "name": "OnpremisesFileSystemOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "FileSystemSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 3,
          "timeout": "01:00:00"
        }
      }
     ]
   }
}
```


<span data-ttu-id="f30c7-427">Forrás adatkészlet toocolumns hello másolási tevékenységdefinícióban fogadó adatkészletből oszlopokat is leképezheti.</span><span class="sxs-lookup"><span data-stu-id="f30c7-427">You can also map columns from source dataset toocolumns from sink dataset in hello copy activity definition.</span></span> <span data-ttu-id="f30c7-428">További információkért lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="f30c7-428">For details, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="f30c7-429">Teljesítmény és finomhangolás</span><span class="sxs-lookup"><span data-stu-id="f30c7-429">Performance and tuning</span></span>
 <span data-ttu-id="f30c7-430">toolearn kulccsal kapcsolatos tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás hello teljesítmény, lásd: hello [másolási tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="f30c7-430">toolearn about key factors that impact hello performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it, see hello [Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md).</span></span>
