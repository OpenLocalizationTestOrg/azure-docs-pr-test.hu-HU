---
title: "Adatok másolása az Azure Data Factory használatával fájlrendszer |} Microsoft Docs"
description: "Ismerje meg az adatok másolása és egy helyszíni fájlrendszer az Azure Data Factory használatával."
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
ms.openlocfilehash: 52305e54f539de6aba2ba9cc856a09e04d608ded
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="copy-data-to-and-from-an-on-premises-file-system-by-using-azure-data-factory"></a><span data-ttu-id="2c0e2-103">Adatok másolása és egy helyszíni fájlrendszer az Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="2c0e2-103">Copy data to and from an on-premises file system by using Azure Data Factory</span></span>
<span data-ttu-id="2c0e2-104">Ez a cikk ismerteti, hogyan használható a másolási tevékenység során az Azure Data Factory belőle egy helyszíni fájlrendszer adatok másolása.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-104">This article explains how to use the Copy Activity in Azure Data Factory to copy data to/from an on-premises file system.</span></span> <span data-ttu-id="2c0e2-105">Buildekről nyújtanak a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amelynek során adatátvitel a másolási tevékenység az általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="2c0e2-106">Támogatott helyzetek</span><span class="sxs-lookup"><span data-stu-id="2c0e2-106">Supported scenarios</span></span>
<span data-ttu-id="2c0e2-107">Adatokat másolhat **egy helyi fájl rendszerből** tárolja a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="2c0e2-107">You can copy data **from an on-premises file system** to the following data stores:</span></span>

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="2c0e2-108">Adatok másolása a következő adatokat tárolja **egy helyszíni fájlrendszerhez**:</span><span class="sxs-lookup"><span data-stu-id="2c0e2-108">You can copy data from the following data stores **to an on-premises file system**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> <span data-ttu-id="2c0e2-109">Másolási tevékenység nem törli a forrásfájl, miután sikerült átmásolni a cél.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-109">Copy Activity does not delete the source file after it is successfully copied to the destination.</span></span> <span data-ttu-id="2c0e2-110">Ha a forrásfájl törlése után sikeres másolatot van szüksége, létrehozhat egy egyéni törölje a fájlt, és az adatcsatorna használja a tevékenységet.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-110">If you need to delete the source file after a successful copy, create a custom activity to delete the file and use the activity in the pipeline.</span></span> 

## <a name="enabling-connectivity"></a><span data-ttu-id="2c0e2-111">Kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2c0e2-111">Enabling connectivity</span></span>
<span data-ttu-id="2c0e2-112">Adat-előállító támogatja a csatlakozást egy helyszíni fájlrendszer keresztül érkező vagy oda irányuló **az adatkezelési átjáró**.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-112">Data Factory supports connecting to and from an on-premises file system via **Data Management Gateway**.</span></span> <span data-ttu-id="2c0e2-113">A helyszíni környezetben a Data Factory szolgáltatásnak bármely támogatott helyszíni adattár, beleértve a fájlrendszer való kapcsolódáshoz telepítenie kell az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-113">You must install the Data Management Gateway in your on-premises environment for the Data Factory service to connect to any supported on-premises data store including file system.</span></span> <span data-ttu-id="2c0e2-114">Az adatkezelési átjáró és az átjáró beállításának lépéseit, lásd: [helyezze át az adatokat a helyszíni adatforrások és az adatkezelési átjáró a felhő közötti](data-factory-move-data-between-onprem-and-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="2c0e2-114">To learn about Data Management Gateway and for step-by-step instructions on setting up the gateway, see [Move data between on-premises sources and the cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md).</span></span> <span data-ttu-id="2c0e2-115">Az adatkezelési átjáró kívül más bináris fájlokat telepítve kell lennie egy helyszíni fájlrendszer érkező vagy oda irányuló kommunikációra.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-115">Apart from Data Management Gateway, no other binary files need to be installed to communicate to and from an on-premises file system.</span></span> <span data-ttu-id="2c0e2-116">Telepítse, és az adatkezelési átjáró használja, akkor is, ha a fájlrendszer Azure IaaS virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-116">You must install and use the Data Management Gateway even if the file system is in Azure IaaS VM.</span></span> <span data-ttu-id="2c0e2-117">Az átjáró kapcsolatos részletes információkért lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="2c0e2-117">For detailed information about the gateway, see [Data Management Gateway](data-factory-data-management-gateway.md).</span></span>

<span data-ttu-id="2c0e2-118">Linux fájlmegosztás használatához telepítenie [Samba](https://www.samba.org/) a Linux-kiszolgálón, telepítse az adatkezelési átjáró a Windows server.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-118">To use a Linux file share, install [Samba](https://www.samba.org/) on your Linux server, and install Data Management Gateway on a Windows server.</span></span> <span data-ttu-id="2c0e2-119">Az adatkezelési átjáró telepítése egy Linux-kiszolgálón nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-119">Installing Data Management Gateway on a Linux server is not supported.</span></span>

## <a name="getting-started"></a><span data-ttu-id="2c0e2-120">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="2c0e2-120">Getting started</span></span>
<span data-ttu-id="2c0e2-121">A másolási tevékenység, amely helyezi át az adatokat, vagy az operációs rendszer különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-121">You can create a pipeline with a copy activity that moves data to/from a file system by using different tools/APIs.</span></span>

<span data-ttu-id="2c0e2-122">Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-122">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="2c0e2-123">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) létrehozásával egy folyamatot, az adatok másolása varázsló segítségével gyorsan útmutatást.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-123">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="2c0e2-124">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-124">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="2c0e2-125">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-125">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span>

<span data-ttu-id="2c0e2-126">Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban:</span><span class="sxs-lookup"><span data-stu-id="2c0e2-126">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="2c0e2-127">Hozzon létre egy **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-127">Create a **data factory**.</span></span> <span data-ttu-id="2c0e2-128">Egy adat-előállító tartalmazhat egy vagy több folyamatok.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-128">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="2c0e2-129">Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-129">Create **linked services** to link input and output data stores to your data factory.</span></span> <span data-ttu-id="2c0e2-130">Például ha a másolt adatok az az Azure blob storage egy helyszíni fájlrendszerhez, hoz létre a helyszíni fájlrendszerben és az Azure storage-fiók összekapcsolása a data factory két társított szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-130">For example, if you are copying data from an Azure blob storage to an on-premises file system, you create two linked services to link your on-premises file system and Azure storage account to your data factory.</span></span> <span data-ttu-id="2c0e2-131">Egy helyszíni fájlrendszer jellemző csatolt szolgáltatás tulajdonságait, lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-131">For linked service properties that are specific to an on-premises file system, see [linked service properties](#linked-service-properties) section.</span></span>
3. <span data-ttu-id="2c0e2-132">Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-132">Create **datasets** to represent input and output data for the copy operation.</span></span> <span data-ttu-id="2c0e2-133">A példában az előző lépésben említett hozzon létre egy adatkészlet adja meg a blob-tároló és a bemeneti adatokat tartalmazó mappát.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-133">In the example mentioned in the last step, you create a dataset to specify the blob container and folder that contains the input data.</span></span> <span data-ttu-id="2c0e2-134">És hoz létre egy másik dataset adja meg a mappát és a fájl nevét a fájlrendszert (nem kötelező).</span><span class="sxs-lookup"><span data-stu-id="2c0e2-134">And, you create another dataset to specify the folder and file name (optional) in your file system.</span></span> <span data-ttu-id="2c0e2-135">A helyszíni fájlrendszer adott adatkészlet tulajdonságai, lásd: [adatkészlet tulajdonságai](#dataset-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-135">For dataset properties that are specific to on-premises file system, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="2c0e2-136">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-136">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="2c0e2-137">A korábban említett példában BlobSource forrás-és FileSystemSink akár használhatja a fogadó a másolási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-137">In the example mentioned earlier, you use BlobSource as a source and FileSystemSink as a sink for the copy activity.</span></span> <span data-ttu-id="2c0e2-138">Hasonlóképpen a helyszíni fájlrendszer az Azure Blob Storage másolása, használható FileSystemSource és BlobSink a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-138">Similarly, if you are copying from on-premises file system to Azure Blob Storage, you use FileSystemSource and BlobSink in the copy activity.</span></span> <span data-ttu-id="2c0e2-139">A másolási tevékenység tulajdonságai a helyszíni fájlrendszer jellemző, lásd: [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-139">For copy activity properties that are specific to on-premises file system, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="2c0e2-140">További részletek a tárolóban használatáról a forrás vagy a fogadó a hivatkozásra a adattároló az előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-140">For details on how to use a data store as a source or a sink, click the link in the previous section for your data store.</span></span>

<span data-ttu-id="2c0e2-141">A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-141">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="2c0e2-142">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások a JSON formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-142">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="2c0e2-143">Másolja az adatokat, az operációs rendszer használt adat-előállító entitások JSON-definíciók minták, lásd: [JSON példák](#json-examples-for-copying-data-to-and-from-file-system) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-143">For samples with JSON definitions for Data Factory entities that are used to copy data to/from a file system, see [JSON examples](#json-examples-for-copying-data-to-and-from-file-system) section of this article.</span></span>

<span data-ttu-id="2c0e2-144">A következő szakaszok részletesen bemutatják, amely segítségével határozza meg a Data Factory tartozó entitások fájlrendszerre JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="2c0e2-144">The following sections provide details about JSON properties that are used to define Data Factory entities specific to file system:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="2c0e2-145">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="2c0e2-145">Linked service properties</span></span>
<span data-ttu-id="2c0e2-146">Egy helyszíni fájlrendszer hozzákapcsolhatja egy az Azure data factory, a **a helyi fájlkiszolgáló** társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-146">You can link an on-premises file system to an Azure data factory with the **On-Premises File Server** linked service.</span></span> <span data-ttu-id="2c0e2-147">A következő táblázat ismerteti, amelyek a helyszíni fájl kiszolgálóhoz kapcsolódó szolgáltatásra vonatkozó JSON-elemek szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-147">The following table provides descriptions for JSON elements that are specific to the On-Premises File Server linked service.</span></span>

| <span data-ttu-id="2c0e2-148">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="2c0e2-148">Property</span></span> | <span data-ttu-id="2c0e2-149">Leírás</span><span class="sxs-lookup"><span data-stu-id="2c0e2-149">Description</span></span> | <span data-ttu-id="2c0e2-150">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2c0e2-150">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2c0e2-151">type</span><span class="sxs-lookup"><span data-stu-id="2c0e2-151">type</span></span> |<span data-ttu-id="2c0e2-152">Győződjön meg arról, hogy a type tulajdonság értéke **OnPremisesFileServer**.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-152">Ensure that the type property is set to **OnPremisesFileServer**.</span></span> |<span data-ttu-id="2c0e2-153">Igen</span><span class="sxs-lookup"><span data-stu-id="2c0e2-153">Yes</span></span> |
| <span data-ttu-id="2c0e2-154">állomás</span><span class="sxs-lookup"><span data-stu-id="2c0e2-154">host</span></span> |<span data-ttu-id="2c0e2-155">Adja meg a legfelső szintű mappa elérési útját, amelyet szeretne másolni.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-155">Specifies the root path of the folder that you want to copy.</span></span> <span data-ttu-id="2c0e2-156">Az escape-karakter használata "\" a speciális karakterek a karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-156">Use the escape character ‘ \ ’ for special characters in the string.</span></span> <span data-ttu-id="2c0e2-157">Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-157">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span> |<span data-ttu-id="2c0e2-158">Igen</span><span class="sxs-lookup"><span data-stu-id="2c0e2-158">Yes</span></span> |
| <span data-ttu-id="2c0e2-159">felhasználói azonosítóját</span><span class="sxs-lookup"><span data-stu-id="2c0e2-159">userid</span></span> |<span data-ttu-id="2c0e2-160">Adja meg a felhasználó, aki hozzáfér a kiszolgáló Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-160">Specify the ID of the user who has access to the server.</span></span> |<span data-ttu-id="2c0e2-161">Nem (Ha úgy dönt, hogy encryptedCredential)</span><span class="sxs-lookup"><span data-stu-id="2c0e2-161">No (if you choose encryptedCredential)</span></span> |
| <span data-ttu-id="2c0e2-162">jelszó</span><span class="sxs-lookup"><span data-stu-id="2c0e2-162">password</span></span> |<span data-ttu-id="2c0e2-163">Adja meg a felhasználó (userid) jelszavát.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-163">Specify the password for the user (userid).</span></span> |<span data-ttu-id="2c0e2-164">Nem (Ha úgy dönt, hogy encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="2c0e2-164">No (if you choose encryptedCredential</span></span> |
| <span data-ttu-id="2c0e2-165">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="2c0e2-165">encryptedCredential</span></span> |<span data-ttu-id="2c0e2-166">Adja meg a titkosított hitelesítő adatokat kaphat a New-AzureRmDataFactoryEncryptValue parancsmag futtatásával.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-166">Specify the encrypted credentials that you can get by running the New-AzureRmDataFactoryEncryptValue cmdlet.</span></span> |<span data-ttu-id="2c0e2-167">Nem (Ha úgy dönt, hogy adja meg a felhasználói azonosítót és jelszót a szövegként)</span><span class="sxs-lookup"><span data-stu-id="2c0e2-167">No (if you choose to specify userid and password in plain text)</span></span> |
| <span data-ttu-id="2c0e2-168">gatewayName</span><span class="sxs-lookup"><span data-stu-id="2c0e2-168">gatewayName</span></span> |<span data-ttu-id="2c0e2-169">Megadja a Data Factory kell csatlakozni a helyi fájlkiszolgálón használó átjáró nevét.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-169">Specifies the name of the gateway that Data Factory should use to connect to the on-premises file server.</span></span> |<span data-ttu-id="2c0e2-170">Igen</span><span class="sxs-lookup"><span data-stu-id="2c0e2-170">Yes</span></span> |


### <a name="sample-linked-service-and-dataset-definitions"></a><span data-ttu-id="2c0e2-171">Példa társított szolgáltatás és a dataset definíciók</span><span class="sxs-lookup"><span data-stu-id="2c0e2-171">Sample linked service and dataset definitions</span></span>
| <span data-ttu-id="2c0e2-172">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="2c0e2-172">Scenario</span></span> | <span data-ttu-id="2c0e2-173">A társított szolgáltatás definíciójának üzemeltetéséhez</span><span class="sxs-lookup"><span data-stu-id="2c0e2-173">Host in linked service definition</span></span> | <span data-ttu-id="2c0e2-174">Az adatkészlet-definícióban folderPath</span><span class="sxs-lookup"><span data-stu-id="2c0e2-174">folderPath in dataset definition</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2c0e2-175">Az adatkezelési átjáró gépen helyi mappában:</span><span class="sxs-lookup"><span data-stu-id="2c0e2-175">Local folder on Data Management Gateway machine:</span></span> <br/><br/><span data-ttu-id="2c0e2-176">Példák: D:\\ \* vagy D:\folder\subfolder\\*</span><span class="sxs-lookup"><span data-stu-id="2c0e2-176">Examples: D:\\\* or D:\folder\subfolder\\*</span></span> |<span data-ttu-id="2c0e2-177">D:\\ \\ (az adatok felügyeleti átjáró 2.0-s és újabb verziók)</span><span class="sxs-lookup"><span data-stu-id="2c0e2-177">D:\\\\ (for Data Management Gateway 2.0 and later versions)</span></span> <br/><br/> <span data-ttu-id="2c0e2-178">a localhost (korábbi verzióihoz mint adatok felügyeleti átjáró 2.0-s)</span><span class="sxs-lookup"><span data-stu-id="2c0e2-178">localhost (for earlier versions than Data Management Gateway 2.0)</span></span> |<span data-ttu-id="2c0e2-179">. \\ \\ vagy mappa\\\\almappa (az adatok felügyeleti átjáró 2.0-s és újabb verziók)</span><span class="sxs-lookup"><span data-stu-id="2c0e2-179">.\\\\ or folder\\\\subfolder (for Data Management Gateway 2.0 and later versions)</span></span> <br/><br/><span data-ttu-id="2c0e2-180">D:\\ \\ vagy D:\\\\mappa\\\\almappa (az átjáró verziója alatt 2.0-s)</span><span class="sxs-lookup"><span data-stu-id="2c0e2-180">D:\\\\ or D:\\\\folder\\\\subfolder (for gateway version below 2.0)</span></span> |
| <span data-ttu-id="2c0e2-181">Távoli megosztott mappa:</span><span class="sxs-lookup"><span data-stu-id="2c0e2-181">Remote shared folder:</span></span> <br/><br/><span data-ttu-id="2c0e2-182">Példák: \\ \\myserver\\megosztása\\ \* vagy \\ \\myserver\\megosztása\\mappa\\almappa\\*</span><span class="sxs-lookup"><span data-stu-id="2c0e2-182">Examples: \\\\myserver\\share\\\* or \\\\myserver\\share\\folder\\subfolder\\*</span></span> |<span data-ttu-id="2c0e2-183">\\\\\\\\myserver\\\\megosztása</span><span class="sxs-lookup"><span data-stu-id="2c0e2-183">\\\\\\\\myserver\\\\share</span></span> |<span data-ttu-id="2c0e2-184">. \\ \\ vagy mappa\\\\almappa</span><span class="sxs-lookup"><span data-stu-id="2c0e2-184">.\\\\ or folder\\\\subfolder</span></span> |


### <a name="example-using-username-and-password-in-plain-text"></a><span data-ttu-id="2c0e2-185">Példa: Felhasználónév és jelszó használatával egyszerű szöveges</span><span class="sxs-lookup"><span data-stu-id="2c0e2-185">Example: Using username and password in plain text</span></span>

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

### <a name="example-using-encryptedcredential"></a><span data-ttu-id="2c0e2-186">Példa: Encryptedcredential használatával</span><span class="sxs-lookup"><span data-stu-id="2c0e2-186">Example: Using encryptedcredential</span></span>

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

## <a name="dataset-properties"></a><span data-ttu-id="2c0e2-187">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="2c0e2-187">Dataset properties</span></span>
<span data-ttu-id="2c0e2-188">Illetve meghatározásához adatkészletek rendelkezésre álló tulajdonságok teljes listáját lásd: [adatkészletek létrehozása](data-factory-create-datasets.md).</span><span class="sxs-lookup"><span data-stu-id="2c0e2-188">For a full list of sections and properties that are available for defining datasets, see [Creating datasets](data-factory-create-datasets.md).</span></span> <span data-ttu-id="2c0e2-189">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-189">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="2c0e2-190">A typeProperties szakaszban nem egyezik az adatkészlet egyes típusú.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-190">The typeProperties section is different for each type of dataset.</span></span> <span data-ttu-id="2c0e2-191">Például a hely és az adattár adatok formátuma adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-191">It provides information such as the location and format of the data in the data store.</span></span> <span data-ttu-id="2c0e2-192">A typeProperties szakasz az adatkészlet típusú **fájlmegosztási** tulajdonságai a következők:</span><span class="sxs-lookup"><span data-stu-id="2c0e2-192">The typeProperties section for the dataset of type **FileShare** has the following properties:</span></span>

| <span data-ttu-id="2c0e2-193">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="2c0e2-193">Property</span></span> | <span data-ttu-id="2c0e2-194">Leírás</span><span class="sxs-lookup"><span data-stu-id="2c0e2-194">Description</span></span> | <span data-ttu-id="2c0e2-195">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2c0e2-195">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2c0e2-196">folderPath</span><span class="sxs-lookup"><span data-stu-id="2c0e2-196">folderPath</span></span> |<span data-ttu-id="2c0e2-197">Adja meg a részleges elérési útja a mappához.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-197">Specifies the subpath to the folder.</span></span> <span data-ttu-id="2c0e2-198">Az escape-karakter használata "\" a speciális karakterek a karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-198">Use the escape character ‘\’ for special characters in the string.</span></span> <span data-ttu-id="2c0e2-199">Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-199">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="2c0e2-200">Ez a tulajdonság a kombinálhatja **partitionBy** szeretné, hogy a mappa elérési utak alapján szelet kezdő és záró dátum-idő.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-200">You can combine this property with **partitionBy** to have folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="2c0e2-201">Igen</span><span class="sxs-lookup"><span data-stu-id="2c0e2-201">Yes</span></span> |
| <span data-ttu-id="2c0e2-202">fileName</span><span class="sxs-lookup"><span data-stu-id="2c0e2-202">fileName</span></span> |<span data-ttu-id="2c0e2-203">Adja meg a fájl nevét a **folderPath** Ha azt szeretné, hogy a tábla egy adott fájlra a mappában.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-203">Specify the name of the file in the **folderPath** if you want the table to refer to a specific file in the folder.</span></span> <span data-ttu-id="2c0e2-204">Ha nem ad meg ehhez a tulajdonsághoz értéket, a tábla a mappában lévő összes fájlt mutat.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-204">If you do not specify any value for this property, the table points to all files in the folder.</span></span><br/><br/><span data-ttu-id="2c0e2-205">Ha **Fájlnév** nincs megadva egy kimeneti adatkészlet és **preserveHierarchy** nincs megadva a tevékenység fogadó, a létrehozott fájl neve nem a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="2c0e2-205">When **fileName** is not specified for an output dataset and **preserveHierarchy** is not specified in activity sink, the name of the generated file is in the following format:</span></span> <br/><br/><span data-ttu-id="2c0e2-206">`Data.<Guid>.txt`(Példa: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span><span class="sxs-lookup"><span data-stu-id="2c0e2-206">`Data.<Guid>.txt` (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span></span> |<span data-ttu-id="2c0e2-207">Nem</span><span class="sxs-lookup"><span data-stu-id="2c0e2-207">No</span></span> |
| <span data-ttu-id="2c0e2-208">fileFilter</span><span class="sxs-lookup"><span data-stu-id="2c0e2-208">fileFilter</span></span> |<span data-ttu-id="2c0e2-209">Adjon meg egy szűrőt, amely minden fájl helyett a fájlok Tárolónév részhalmazának kiválasztására szolgál.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-209">Specify a filter to be used to select a subset of files in the folderPath rather than all files.</span></span> <br/><br/><span data-ttu-id="2c0e2-210">Két érték engedélyezett: `*` (több karaktert) és `?` (egyetlen karakter).</span><span class="sxs-lookup"><span data-stu-id="2c0e2-210">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="2c0e2-211">1. példa: "fileFilter": "* .log"</span><span class="sxs-lookup"><span data-stu-id="2c0e2-211">Example 1: "fileFilter": "*.log"</span></span><br/><span data-ttu-id="2c0e2-212">2. példa: "fileFilter": 2014 - 1-?. txt"</span><span class="sxs-lookup"><span data-stu-id="2c0e2-212">Example 2: "fileFilter": 2014-1-?.txt"</span></span><br/><br/><span data-ttu-id="2c0e2-213">Vegye figyelembe, hogy fileFilter egy bemeneti fájlmegosztási adatkészlet esetében alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-213">Note that fileFilter is applicable for an input FileShare dataset.</span></span> |<span data-ttu-id="2c0e2-214">Nem</span><span class="sxs-lookup"><span data-stu-id="2c0e2-214">No</span></span> |
| <span data-ttu-id="2c0e2-215">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="2c0e2-215">partitionedBy</span></span> |<span data-ttu-id="2c0e2-216">PartitionedBy segítségével adjon meg egy dinamikus folderPath/fájlnevet idősorozat adatok.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-216">You can use partitionedBy to specify a dynamic folderPath/fileName for time-series data.</span></span> <span data-ttu-id="2c0e2-217">Példa: az adatok óránkénti paraméteres folderPath.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-217">An example is folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="2c0e2-218">Nem</span><span class="sxs-lookup"><span data-stu-id="2c0e2-218">No</span></span> |
| <span data-ttu-id="2c0e2-219">Formátumban</span><span class="sxs-lookup"><span data-stu-id="2c0e2-219">format</span></span> | <span data-ttu-id="2c0e2-220">A következő formátumban típusok támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-220">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="2c0e2-221">Állítsa be a **típus** tulajdonság a formátuma a következő értékek egyikét.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-221">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="2c0e2-222">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-222">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="2c0e2-223">Ha azt szeretné, hogy **másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a Formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-223">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="2c0e2-224">Nem</span><span class="sxs-lookup"><span data-stu-id="2c0e2-224">No</span></span> |
| <span data-ttu-id="2c0e2-225">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="2c0e2-225">compression</span></span> | <span data-ttu-id="2c0e2-226">Adja meg a típus és az adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-226">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="2c0e2-227">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-227">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="2c0e2-228">Támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-228">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="2c0e2-229">Lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="2c0e2-229">see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="2c0e2-230">Nem</span><span class="sxs-lookup"><span data-stu-id="2c0e2-230">No</span></span> |

> [!NOTE]
> <span data-ttu-id="2c0e2-231">Nem használható egyszerre fájlnév és fileFilter.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-231">You cannot use fileName and fileFilter simultaneously.</span></span>

### <a name="using-partitionedby-property"></a><span data-ttu-id="2c0e2-232">PartitionedBy tulajdonság használatával</span><span class="sxs-lookup"><span data-stu-id="2c0e2-232">Using partitionedBy property</span></span>
<span data-ttu-id="2c0e2-233">Az előző szakaszban említett, megadhat egy dinamikus folderPath és a fájlnév idő adatsorozat adatokhoz a **partitionedBy** tulajdonság, [adat-előállító funkciók és a rendszer változók](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="2c0e2-233">As mentioned in the previous section, you can specify a dynamic folderPath and filename for time series data with the **partitionedBy** property, [Data Factory functions, and the system variables](data-factory-functions-variables.md).</span></span>

<span data-ttu-id="2c0e2-234">További részleteket a idősorozat adatkészleteket, az ütemezés és a szeletek ismertetése: [adatkészletek létrehozása](data-factory-create-datasets.md), [ütemezés és a végrehajtás](data-factory-scheduling-and-execution.md), és [folyamatok létrehozása](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="2c0e2-234">To understand more details on time-series datasets, scheduling, and slices, see [Creating datasets](data-factory-create-datasets.md), [Scheduling and execution](data-factory-scheduling-and-execution.md), and [Creating pipelines](data-factory-create-pipelines.md).</span></span>

#### <a name="sample-1"></a><span data-ttu-id="2c0e2-235">1. példa:</span><span class="sxs-lookup"><span data-stu-id="2c0e2-235">Sample 1:</span></span>

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

<span data-ttu-id="2c0e2-236">Ebben a példában {szelet} váltja fel a Data Factory rendszerváltozó SliceStart formátumban (YYYYMMDDHH) értékét.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-236">In this example, {Slice} is replaced with the value of the Data Factory system variable SliceStart in the format (YYYYMMDDHH).</span></span> <span data-ttu-id="2c0e2-237">A szelet kezdete SliceStart hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-237">SliceStart refers to start time of the slice.</span></span> <span data-ttu-id="2c0e2-238">A folderPath nem azonos az egyes szeletek.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-238">The folderPath is different for each slice.</span></span> <span data-ttu-id="2c0e2-239">Például: wikidatagateway/wikisampledataout/2014100103 vagy wikidatagateway/wikisampledataout/2014100104.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-239">For example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.</span></span>

#### <a name="sample-2"></a><span data-ttu-id="2c0e2-240">2. példa:</span><span class="sxs-lookup"><span data-stu-id="2c0e2-240">Sample 2:</span></span>

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

<span data-ttu-id="2c0e2-241">Ebben a példában év, hónap, nap és SliceStart idején ki kell olvasni a külön változókat, amelyek a fájlnév és a folderPath tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-241">In this example, year, month, day, and time of SliceStart are extracted into separate variables that the folderPath and fileName properties use.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="2c0e2-242">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="2c0e2-242">Copy activity properties</span></span>
<span data-ttu-id="2c0e2-243">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-243">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="2c0e2-244">Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti adatkészletek és házirendek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-244">Properties such as name, description, input and output datasets, and policies are available for all types of activities.</span></span> <span data-ttu-id="2c0e2-245">Mivel a tulajdonságok érhetők el a **typeProperties** szakasz a tevékenység tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-245">Whereas, properties available in the **typeProperties** section of the activity vary with each activity type.</span></span>

<span data-ttu-id="2c0e2-246">A másolási tevékenység során két érték források és mosdók típusától függően.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-246">For Copy activity, they vary depending on the types of sources and sinks.</span></span> <span data-ttu-id="2c0e2-247">Egy helyszíni fájlrendszerből helyez át adatokat, ha a forrás típusa beállítása a másolási tevékenység **FileSystemSource**.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-247">If you are moving data from an on-premises file system, you set the source type in the copy activity to **FileSystemSource**.</span></span> <span data-ttu-id="2c0e2-248">Hasonlóképpen, ha egy helyszíni fájlrendszerhez helyez át adatokat, beállítása a fogadó típusa a másolási tevékenység **FileSystemSink**.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-248">Similarly, if you are moving data to an on-premises file system, you set the sink type in the copy activity to **FileSystemSink**.</span></span> <span data-ttu-id="2c0e2-249">Ez a témakör FileSystemSource és FileSystemSink által támogatott tulajdonságokról.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-249">This section provides a list of properties supported by FileSystemSource and FileSystemSink.</span></span>

<span data-ttu-id="2c0e2-250">**FileSystemSource** támogatja a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="2c0e2-250">**FileSystemSource** supports the following properties:</span></span>

| <span data-ttu-id="2c0e2-251">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="2c0e2-251">Property</span></span> | <span data-ttu-id="2c0e2-252">Leírás</span><span class="sxs-lookup"><span data-stu-id="2c0e2-252">Description</span></span> | <span data-ttu-id="2c0e2-253">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="2c0e2-253">Allowed values</span></span> | <span data-ttu-id="2c0e2-254">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2c0e2-254">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="2c0e2-255">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="2c0e2-255">recursive</span></span> |<span data-ttu-id="2c0e2-256">Azt jelzi, hogy az adatok olvasható rekurzív módon az almappákat, illetve csak a megadott mappát.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-256">Indicates whether the data is read recursively from the subfolders or only from the specified folder.</span></span> |<span data-ttu-id="2c0e2-257">IGAZ, hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="2c0e2-257">True, False (default)</span></span> |<span data-ttu-id="2c0e2-258">Nem</span><span class="sxs-lookup"><span data-stu-id="2c0e2-258">No</span></span> |

<span data-ttu-id="2c0e2-259">**FileSystemSink** támogatja a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="2c0e2-259">**FileSystemSink** supports the following properties:</span></span>

| <span data-ttu-id="2c0e2-260">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="2c0e2-260">Property</span></span> | <span data-ttu-id="2c0e2-261">Leírás</span><span class="sxs-lookup"><span data-stu-id="2c0e2-261">Description</span></span> | <span data-ttu-id="2c0e2-262">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="2c0e2-262">Allowed values</span></span> | <span data-ttu-id="2c0e2-263">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2c0e2-263">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="2c0e2-264">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="2c0e2-264">copyBehavior</span></span> |<span data-ttu-id="2c0e2-265">Másolás viselkedését határozza meg, ha az adatforrás BlobSource vagy a fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-265">Defines the copy behavior when the source is BlobSource or FileSystem.</span></span> |<span data-ttu-id="2c0e2-266">**PreserveHierarchy:** őrzi meg a fájl hierarchia a célmappában.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-266">**PreserveHierarchy:** Preserves the file hierarchy in the target folder.</span></span> <span data-ttu-id="2c0e2-267">Ez azt jelenti, hogy a forrásfájl, a forrásmappához relatív elérési út ugyanaz, mint a relatív a cél elérési útja a célként megadott mappába.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-267">That is, the relative path of the source file to the source folder is the same as the relative path of the target file to the target folder.</span></span><br/><br/><span data-ttu-id="2c0e2-268">**FlattenHierarchy:** minden fájl a forrásmappából az első szintű célmappában jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-268">**FlattenHierarchy:** All files from the source folder are created in the first level of target folder.</span></span> <span data-ttu-id="2c0e2-269">A cél fájlok jönnek létre automatikusan létrehozott névvel.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-269">The target files are created with an autogenerated name.</span></span><br/><br/><span data-ttu-id="2c0e2-270">**Mergefiles típusú:** egyesíti a forrásmappából egy fájl összes fájlt.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-270">**MergeFiles:** Merges all files from the source folder to one file.</span></span> <span data-ttu-id="2c0e2-271">Ha a fájl neve/blob neve meg van adva, az egyesített fájlnév a megadott név.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-271">If the file name/blob name is specified, the merged file name is the specified name.</span></span> <span data-ttu-id="2c0e2-272">Ellenkező esetben egy automatikusan létrehozott nevét.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-272">Otherwise, it is an auto-generated file name.</span></span> |<span data-ttu-id="2c0e2-273">Nem</span><span class="sxs-lookup"><span data-stu-id="2c0e2-273">No</span></span> |

### <a name="recursive-and-copybehavior-examples"></a><span data-ttu-id="2c0e2-274">rekurzív és copyBehavior példák</span><span class="sxs-lookup"><span data-stu-id="2c0e2-274">recursive and copyBehavior examples</span></span>
<span data-ttu-id="2c0e2-275">Ez a szakasz ismerteti az eredményül kapott viselkedéstől a másolási művelet kombinációk a rekurzív és a copyBehavior tulajdonság értéktartománya.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-275">This section describes the resulting behavior of the Copy operation for different combinations of values for the recursive and copyBehavior properties.</span></span>

| <span data-ttu-id="2c0e2-276">rekurzív érték</span><span class="sxs-lookup"><span data-stu-id="2c0e2-276">recursive value</span></span> | <span data-ttu-id="2c0e2-277">copyBehavior érték</span><span class="sxs-lookup"><span data-stu-id="2c0e2-277">copyBehavior value</span></span> | <span data-ttu-id="2c0e2-278">Viselkedésről</span><span class="sxs-lookup"><span data-stu-id="2c0e2-278">Resulting behavior</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2c0e2-279">Igaz</span><span class="sxs-lookup"><span data-stu-id="2c0e2-279">true</span></span> |<span data-ttu-id="2c0e2-280">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="2c0e2-280">preserveHierarchy</span></span> |<span data-ttu-id="2c0e2-281">A forrásmappa mappa1 az alábbi struktúrával,</span><span class="sxs-lookup"><span data-stu-id="2c0e2-281">For a source folder Folder1 with the following structure,</span></span><br/><br/><span data-ttu-id="2c0e2-282">Mappa1</span><span class="sxs-lookup"><span data-stu-id="2c0e2-282">Folder1</span></span><br/><span data-ttu-id="2c0e2-283">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="2c0e2-283">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="2c0e2-284">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="2c0e2-284">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="2c0e2-285">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="2c0e2-285">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="2c0e2-286">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="2c0e2-286">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="2c0e2-287">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="2c0e2-287">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="2c0e2-288">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="2c0e2-288">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="2c0e2-289">a célmappa mappa1 forrásaként azonos struktúrájú jön létre:</span><span class="sxs-lookup"><span data-stu-id="2c0e2-289">the target folder Folder1 is created with the same structure as the source:</span></span><br/><br/><span data-ttu-id="2c0e2-290">Mappa1</span><span class="sxs-lookup"><span data-stu-id="2c0e2-290">Folder1</span></span><br/><span data-ttu-id="2c0e2-291">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="2c0e2-291">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="2c0e2-292">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="2c0e2-292">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="2c0e2-293">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="2c0e2-293">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="2c0e2-294">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="2c0e2-294">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="2c0e2-295">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="2c0e2-295">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="2c0e2-296">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="2c0e2-296">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span> |
| <span data-ttu-id="2c0e2-297">Igaz</span><span class="sxs-lookup"><span data-stu-id="2c0e2-297">true</span></span> |<span data-ttu-id="2c0e2-298">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="2c0e2-298">flattenHierarchy</span></span> |<span data-ttu-id="2c0e2-299">A forrásmappa mappa1 az alábbi struktúrával,</span><span class="sxs-lookup"><span data-stu-id="2c0e2-299">For a source folder Folder1 with the following structure,</span></span><br/><br/><span data-ttu-id="2c0e2-300">Mappa1</span><span class="sxs-lookup"><span data-stu-id="2c0e2-300">Folder1</span></span><br/><span data-ttu-id="2c0e2-301">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="2c0e2-301">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="2c0e2-302">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="2c0e2-302">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="2c0e2-303">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="2c0e2-303">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="2c0e2-304">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="2c0e2-304">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="2c0e2-305">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="2c0e2-305">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="2c0e2-306">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="2c0e2-306">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="2c0e2-307">a cél az alábbi szerkezettel mappa1 jön létre:</span><span class="sxs-lookup"><span data-stu-id="2c0e2-307">the target Folder1 is created with the following structure:</span></span> <br/><br/><span data-ttu-id="2c0e2-308">Mappa1</span><span class="sxs-lookup"><span data-stu-id="2c0e2-308">Folder1</span></span><br/><span data-ttu-id="2c0e2-309">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet a file1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="2c0e2-309">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="2c0e2-310">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File2</span><span class="sxs-lookup"><span data-stu-id="2c0e2-310">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><span data-ttu-id="2c0e2-311">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet fájl3</span><span class="sxs-lookup"><span data-stu-id="2c0e2-311">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File3</span></span><br/><span data-ttu-id="2c0e2-312">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File4</span><span class="sxs-lookup"><span data-stu-id="2c0e2-312">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File4</span></span><br/><span data-ttu-id="2c0e2-313">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File5</span><span class="sxs-lookup"><span data-stu-id="2c0e2-313">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File5</span></span> |
| <span data-ttu-id="2c0e2-314">Igaz</span><span class="sxs-lookup"><span data-stu-id="2c0e2-314">true</span></span> |<span data-ttu-id="2c0e2-315">mergefiles típusú</span><span class="sxs-lookup"><span data-stu-id="2c0e2-315">mergeFiles</span></span> |<span data-ttu-id="2c0e2-316">A forrásmappa mappa1 az alábbi struktúrával,</span><span class="sxs-lookup"><span data-stu-id="2c0e2-316">For a source folder Folder1 with the following structure,</span></span><br/><br/><span data-ttu-id="2c0e2-317">Mappa1</span><span class="sxs-lookup"><span data-stu-id="2c0e2-317">Folder1</span></span><br/><span data-ttu-id="2c0e2-318">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="2c0e2-318">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="2c0e2-319">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="2c0e2-319">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="2c0e2-320">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="2c0e2-320">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="2c0e2-321">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="2c0e2-321">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="2c0e2-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="2c0e2-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="2c0e2-323">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="2c0e2-323">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="2c0e2-324">a cél az alábbi szerkezettel mappa1 jön létre:</span><span class="sxs-lookup"><span data-stu-id="2c0e2-324">the target Folder1 is created with the following structure:</span></span> <br/><br/><span data-ttu-id="2c0e2-325">Mappa1</span><span class="sxs-lookup"><span data-stu-id="2c0e2-325">Folder1</span></span><br/><span data-ttu-id="2c0e2-326">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + fájl3 + File4 + 5 fájl tartalmát egy fájlba, egy fájl automatikusan létrehozott névvel egyesítve lesznek.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-326">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + File 5 contents are merged into one file with an auto-generated file name.</span></span> |
| <span data-ttu-id="2c0e2-327">hamis</span><span class="sxs-lookup"><span data-stu-id="2c0e2-327">false</span></span> |<span data-ttu-id="2c0e2-328">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="2c0e2-328">preserveHierarchy</span></span> |<span data-ttu-id="2c0e2-329">A forrásmappa mappa1 az alábbi struktúrával,</span><span class="sxs-lookup"><span data-stu-id="2c0e2-329">For a source folder Folder1 with the following structure,</span></span><br/><br/><span data-ttu-id="2c0e2-330">Mappa1</span><span class="sxs-lookup"><span data-stu-id="2c0e2-330">Folder1</span></span><br/><span data-ttu-id="2c0e2-331">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="2c0e2-331">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="2c0e2-332">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="2c0e2-332">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="2c0e2-333">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="2c0e2-333">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="2c0e2-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="2c0e2-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="2c0e2-335">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="2c0e2-335">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="2c0e2-336">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="2c0e2-336">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="2c0e2-337">a tároló mappa mappa1 jön létre az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="2c0e2-337">the target folder Folder1 is created with the following structure:</span></span><br/><br/><span data-ttu-id="2c0e2-338">Mappa1</span><span class="sxs-lookup"><span data-stu-id="2c0e2-338">Folder1</span></span><br/><span data-ttu-id="2c0e2-339">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="2c0e2-339">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="2c0e2-340">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="2c0e2-340">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><br/><span data-ttu-id="2c0e2-341">Fájl3, File4 és File5 Subfolder1 nem felvételre.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-341">Subfolder1 with File3, File4, and File5 is not picked up.</span></span> |
| <span data-ttu-id="2c0e2-342">hamis</span><span class="sxs-lookup"><span data-stu-id="2c0e2-342">false</span></span> |<span data-ttu-id="2c0e2-343">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="2c0e2-343">flattenHierarchy</span></span> |<span data-ttu-id="2c0e2-344">A forrásmappa mappa1 az alábbi struktúrával,</span><span class="sxs-lookup"><span data-stu-id="2c0e2-344">For a source folder Folder1 with the following structure,</span></span><br/><br/><span data-ttu-id="2c0e2-345">Mappa1</span><span class="sxs-lookup"><span data-stu-id="2c0e2-345">Folder1</span></span><br/><span data-ttu-id="2c0e2-346">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="2c0e2-346">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="2c0e2-347">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="2c0e2-347">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="2c0e2-348">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="2c0e2-348">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="2c0e2-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="2c0e2-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="2c0e2-350">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="2c0e2-350">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="2c0e2-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="2c0e2-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="2c0e2-352">a tároló mappa mappa1 jön létre az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="2c0e2-352">the target folder Folder1 is created with the following structure:</span></span><br/><br/><span data-ttu-id="2c0e2-353">Mappa1</span><span class="sxs-lookup"><span data-stu-id="2c0e2-353">Folder1</span></span><br/><span data-ttu-id="2c0e2-354">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet a file1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="2c0e2-354">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="2c0e2-355">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File2</span><span class="sxs-lookup"><span data-stu-id="2c0e2-355">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><br/><span data-ttu-id="2c0e2-356">Fájl3, File4 és File5 Subfolder1 nem felvételre.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-356">Subfolder1 with File3, File4, and File5 is not picked up.</span></span> |
| <span data-ttu-id="2c0e2-357">hamis</span><span class="sxs-lookup"><span data-stu-id="2c0e2-357">false</span></span> |<span data-ttu-id="2c0e2-358">mergefiles típusú</span><span class="sxs-lookup"><span data-stu-id="2c0e2-358">mergeFiles</span></span> |<span data-ttu-id="2c0e2-359">A forrásmappa mappa1 az alábbi struktúrával,</span><span class="sxs-lookup"><span data-stu-id="2c0e2-359">For a source folder Folder1 with the following structure,</span></span><br/><br/><span data-ttu-id="2c0e2-360">Mappa1</span><span class="sxs-lookup"><span data-stu-id="2c0e2-360">Folder1</span></span><br/><span data-ttu-id="2c0e2-361">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="2c0e2-361">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="2c0e2-362">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="2c0e2-362">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="2c0e2-363">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="2c0e2-363">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="2c0e2-364">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="2c0e2-364">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="2c0e2-365">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="2c0e2-365">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="2c0e2-366">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="2c0e2-366">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="2c0e2-367">a tároló mappa mappa1 jön létre az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="2c0e2-367">the target folder Folder1 is created with the following structure:</span></span><br/><br/><span data-ttu-id="2c0e2-368">Mappa1</span><span class="sxs-lookup"><span data-stu-id="2c0e2-368">Folder1</span></span><br/><span data-ttu-id="2c0e2-369">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 tartalma egyesítődnek fájl automatikusan létrehozott névvel egy fájlba.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-369">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 contents are merged into one file with an auto-generated file name.</span></span><br/><span data-ttu-id="2c0e2-370">&nbsp;&nbsp;&nbsp;&nbsp;Automatikusan létrehozott nevet a file1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="2c0e2-370">&nbsp;&nbsp;&nbsp;&nbsp;Auto-generated name for File1</span></span><br/><br/><span data-ttu-id="2c0e2-371">Fájl3, File4 és File5 Subfolder1 nem felvételre.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-371">Subfolder1 with File3, File4, and File5 is not picked up.</span></span> |

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="2c0e2-372">Támogatott formátumú és tömörítés</span><span class="sxs-lookup"><span data-stu-id="2c0e2-372">Supported file and compression formats</span></span>
<span data-ttu-id="2c0e2-373">Lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md) cikk részletei.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-373">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-examples-for-copying-data-to-and-from-file-system"></a><span data-ttu-id="2c0e2-374">Az adatok másolása a JSON-példák fájlrendszer</span><span class="sxs-lookup"><span data-stu-id="2c0e2-374">JSON examples for copying data to and from file system</span></span>
<span data-ttu-id="2c0e2-375">Az alábbi példák megadják minta JSON-definíciókat tartalmazzon, segítségével hozzon létre egy folyamatot a [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="2c0e2-375">The following examples provide sample JSON definitions that you can use to create a pipeline by using the [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="2c0e2-376">Adatok másolása egy a helyszíni fájlrendszerben és az Azure Blob Storage tárolóban mutatnak.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-376">They show how to copy data to and from an on-premises file system and Azure Blob storage.</span></span> <span data-ttu-id="2c0e2-377">Adatok másolása azonban *közvetlenül* bármelyik sem a felsorolt nyelő források [támogatott források és mosdók](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-377">However, you can copy data *directly* from any of the sources to any of the sinks listed in [Supported sources and sinks](data-factory-data-movement-activities.md#supported-data-stores-and-formats) by using Copy Activity in Azure Data Factory.</span></span>

### <a name="example-copy-data-from-an-on-premises-file-system-to-azure-blob-storage"></a><span data-ttu-id="2c0e2-378">Példa: Adatok másolása az egy helyszíni fájlrendszer az Azure Blob-tároló</span><span class="sxs-lookup"><span data-stu-id="2c0e2-378">Example: Copy data from an on-premises file system to Azure Blob storage</span></span>
<span data-ttu-id="2c0e2-379">Ez a példa bemutatja, hogyan egy helyszíni fájlrendszer adatok másolása az Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-379">This sample shows how to copy data from an on-premises file system to Azure Blob storage.</span></span> <span data-ttu-id="2c0e2-380">A minta a következő adat-előállító entitások rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="2c0e2-380">The sample has the following Data Factory entities:</span></span>

* <span data-ttu-id="2c0e2-381">A társított szolgáltatás típusa [OnPremisesFileServer](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="2c0e2-381">A linked service of type [OnPremisesFileServer](#linked-service-properties).</span></span>
* <span data-ttu-id="2c0e2-382">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="2c0e2-382">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="2c0e2-383">Bemeneti [dataset](data-factory-create-datasets.md) típusú [fájlmegosztási](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="2c0e2-383">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties).</span></span>
* <span data-ttu-id="2c0e2-384">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="2c0e2-384">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="2c0e2-385">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [FileSystemSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="2c0e2-385">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="2c0e2-386">Az alábbi minta másol idősorozat adatokat egy helyszíni fájlrendszer az Azure Blob storage óránként.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-386">The following sample copies time-series data from an on-premises file system to Azure Blob storage every hour.</span></span> <span data-ttu-id="2c0e2-387">A JSON-tulajdonságok ezeket a mintákat a használt a szakaszok ismertetik a minta után.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-387">The JSON properties that are used in these samples are described in the sections after the samples.</span></span>

<span data-ttu-id="2c0e2-388">Első lépésként állítsa be az adatkezelési átjáró szerint utasításait [helyezze át az adatokat a helyszíni adatforrások és az adatkezelési átjáró a felhő közötti](data-factory-move-data-between-onprem-and-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="2c0e2-388">As a first step, set up Data Management Gateway as per the instructions in [Move data between on-premises sources and the cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md).</span></span>

<span data-ttu-id="2c0e2-389">**A helyi fájlkiszolgáló társított szolgáltatáshoz:**</span><span class="sxs-lookup"><span data-stu-id="2c0e2-389">**On-Premises File Server linked service:**</span></span>

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

<span data-ttu-id="2c0e2-390">Azt javasoljuk, a **encryptedCredential** tulajdonság inkább a **userid** és **jelszó** tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-390">We recommend using the **encryptedCredential** property instead the **userid** and **password** properties.</span></span> <span data-ttu-id="2c0e2-391">Lásd: [fájlkiszolgáló társított szolgáltatás](#linked-service-properties) részleteiről a társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-391">See [File Server linked service](#linked-service-properties) for details about this linked service.</span></span>

<span data-ttu-id="2c0e2-392">**Az Azure tárolás társított szolgáltatásának:**</span><span class="sxs-lookup"><span data-stu-id="2c0e2-392">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="2c0e2-393">**A helyi rendszer bemeneti adatkészlet fájl:**</span><span class="sxs-lookup"><span data-stu-id="2c0e2-393">**On-premises file system input dataset:**</span></span>

<span data-ttu-id="2c0e2-394">Adatok van felvett egy új fájl óránként.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-394">Data is picked up from a new file every hour.</span></span> <span data-ttu-id="2c0e2-395">A fájlnév és a folderPath tulajdonság alapján a szelet kezdési idejét határozza meg.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-395">The folderPath and fileName properties are determined based on the start time of the slice.</span></span>  

<span data-ttu-id="2c0e2-396">Beállítás `"external": "true"` adat-előállító tájékoztatja, hogy az adatkészlet data factoryval való külső, és egy tevékenység adat-előállító nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-396">Setting `"external": "true"` informs Data Factory that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

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

<span data-ttu-id="2c0e2-397">**Az Azure Blob storage kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="2c0e2-397">**Azure Blob storage output dataset:**</span></span>

<span data-ttu-id="2c0e2-398">Adatot ír egy új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="2c0e2-398">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="2c0e2-399">A mappa elérési útját a BLOB a szelet által feldolgozott kezdési ideje alapján dinamikusan történik.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-399">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="2c0e2-400">A mappa elérési útját használja, év, hónap, nap, és a kezdési idő órában részeit.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-400">The folder path uses the year, month, day, and hour parts of the start time.</span></span>

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

<span data-ttu-id="2c0e2-401">**A másolási tevékenység során a fájlrendszer és a Blob fogadó folyamat:**</span><span class="sxs-lookup"><span data-stu-id="2c0e2-401">**A copy activity in a pipeline with File System source and Blob sink:**</span></span>

<span data-ttu-id="2c0e2-402">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-402">The pipeline contains a copy activity that is configured to use the input and output datasets, and is scheduled to run every hour.</span></span> <span data-ttu-id="2c0e2-403">Az adatcsatorna JSON-definícióból a **forrás** típusúra **FileSystemSource**, és **fogadó** típusúra **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-403">In the pipeline JSON definition, the **source** type is set to **FileSystemSource**, and **sink** type is set to **BlobSink**.</span></span>

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

### <a name="example-copy-data-from-azure-sql-database-to-an-on-premises-file-system"></a><span data-ttu-id="2c0e2-404">Példa: Adatok másolása az Azure SQL-adatbázis egy helyszíni fájlrendszerhez</span><span class="sxs-lookup"><span data-stu-id="2c0e2-404">Example: Copy data from Azure SQL Database to an on-premises file system</span></span>
<span data-ttu-id="2c0e2-405">A következő példában:</span><span class="sxs-lookup"><span data-stu-id="2c0e2-405">The following sample shows:</span></span>

* <span data-ttu-id="2c0e2-406">A társított szolgáltatás típusa [AzureSqlDatabase.](data-factory-azure-sql-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="2c0e2-406">A linked service of type [AzureSqlDatabase.](data-factory-azure-sql-connector.md#linked-service-properties)</span></span>
* <span data-ttu-id="2c0e2-407">A társított szolgáltatás típusa [OnPremisesFileServer](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="2c0e2-407">A linked service of type [OnPremisesFileServer](#linked-service-properties).</span></span>
* <span data-ttu-id="2c0e2-408">Egy bemeneti adatkészlet típusú [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="2c0e2-408">An input dataset of type [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="2c0e2-409">Egy kimeneti adatkészlet típusú [fájlmegosztási](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="2c0e2-409">An output dataset of type [FileShare](#dataset-properties).</span></span>
* <span data-ttu-id="2c0e2-410">A másolási tevékenység által használt az adatcsatorna [SqlSource](data-factory-azure-sql-connector.md##copy-activity-properties) és [FileSystemSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="2c0e2-410">A pipeline with a copy activity that uses [SqlSource](data-factory-azure-sql-connector.md##copy-activity-properties) and [FileSystemSink](#copy-activity-properties).</span></span>

<span data-ttu-id="2c0e2-411">A minta-idősoros adatok egy Azure SQL tábla másolja egy helyi fájlrendszer minden órában.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-411">The sample copies time-series data from an Azure SQL table to an on-premises file system every hour.</span></span> <span data-ttu-id="2c0e2-412">A JSON-tulajdonságok ezeket a mintákat a használt szakaszok ismertetik a minta után.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-412">The JSON properties that are used in these samples are described in sections after the samples.</span></span>

<span data-ttu-id="2c0e2-413">**A társított szolgáltatásnak Azure SQL Database:**</span><span class="sxs-lookup"><span data-stu-id="2c0e2-413">**Azure SQL Database linked service:**</span></span>

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

<span data-ttu-id="2c0e2-414">**A helyi fájlkiszolgáló társított szolgáltatáshoz:**</span><span class="sxs-lookup"><span data-stu-id="2c0e2-414">**On-Premises File Server linked service:**</span></span>

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

<span data-ttu-id="2c0e2-415">Azt javasoljuk, a **encryptedCredential** tulajdonság használata helyett a **userid** és **jelszó** tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-415">We recommend using the **encryptedCredential** property instead of using the **userid** and **password** properties.</span></span> <span data-ttu-id="2c0e2-416">Lásd: [fájlrendszer társított szolgáltatás](#linked-service-properties) részleteiről a társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-416">See [File System linked service](#linked-service-properties) for details about this linked service.</span></span>

<span data-ttu-id="2c0e2-417">**Az Azure SQL bemeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="2c0e2-417">**Azure SQL input dataset:**</span></span>

<span data-ttu-id="2c0e2-418">A minta feltételezi, hogy létrehozott egy tábla "MyTable" Azure SQL-ben, és egy idősorozat adatok "timestampcolumn" nevű oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-418">The sample assumes that you've created a table “MyTable” in Azure SQL, and it contains a column called “timestampcolumn” for time-series data.</span></span>

<span data-ttu-id="2c0e2-419">Beállítás ``“external”: ”true”`` adat-előállító tájékoztatja, hogy az adatkészlet data factoryval való külső, és egy tevékenység adat-előállító nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-419">Setting ``“external”: ”true”`` informs Data Factory that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

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

<span data-ttu-id="2c0e2-420">**A helyszíni rendszer kimeneti adatkészlet fájl:**</span><span class="sxs-lookup"><span data-stu-id="2c0e2-420">**On-premises file system output dataset:**</span></span>

<span data-ttu-id="2c0e2-421">Adatokat egy új fájlt másolja minden órában.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-421">Data is copied to a new file every hour.</span></span> <span data-ttu-id="2c0e2-422">A folderPath és a blob fájlnevét határozza meg a kezdési időt a szelet alapján.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-422">The folderPath and fileName for the blob are determined based on the start time of the slice.</span></span>

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

<span data-ttu-id="2c0e2-423">**A másolási tevékenység során egy SQL-forrás és fogadó fájlrendszer folyamaton belül:**</span><span class="sxs-lookup"><span data-stu-id="2c0e2-423">**A copy activity in a pipeline with SQL source and File System sink:**</span></span>

<span data-ttu-id="2c0e2-424">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-424">The pipeline contains a copy activity that is configured to use the input and output datasets, and is scheduled to run every hour.</span></span> <span data-ttu-id="2c0e2-425">Az adatcsatorna JSON-definícióból a **forrás** típusúra **SqlSource**, és a **fogadó** típusúra **FileSystemSink**.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-425">In the pipeline JSON definition, the **source** type is set to **SqlSource**, and the **sink** type is set to **FileSystemSink**.</span></span> <span data-ttu-id="2c0e2-426">A megadott SQL-lekérdezést a **SqlReaderQuery** tulajdonság kiválasztása az adatok másolása az elmúlt órában.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-426">The SQL query that is specified for the **SqlReaderQuery** property selects the data in the past hour to copy.</span></span>

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


<span data-ttu-id="2c0e2-427">A másolási tevékenység definíciójának fogadó adatkészletből oszlopok forrás adatkészletből oszlopokat is leképezheti.</span><span class="sxs-lookup"><span data-stu-id="2c0e2-427">You can also map columns from source dataset to columns from sink dataset in the copy activity definition.</span></span> <span data-ttu-id="2c0e2-428">További információkért lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="2c0e2-428">For details, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="2c0e2-429">Teljesítmény és finomhangolás</span><span class="sxs-lookup"><span data-stu-id="2c0e2-429">Performance and tuning</span></span>
 <span data-ttu-id="2c0e2-430">Az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon optimalizálása azt a teljesítményt befolyásoló legfontosabb tényezők kapcsolatos további tudnivalókért lásd: a [másolási tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="2c0e2-430">To learn about key factors that impact the performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it, see the [Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md).</span></span>
