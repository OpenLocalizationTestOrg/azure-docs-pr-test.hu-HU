---
title: "Adatok áthelyezése a helyszíni HDFS |} Microsoft Docs"
description: "További tudnivalók az Azure Data Factory használatával a helyszíni HDFS áthelyezni az adatokat."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 3215b82d-291a-46db-8478-eac1a3219614
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 9a8f3156a62a1a7aa49377349e8a85454efeda50
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a><span data-ttu-id="272ad-103">Adatok áthelyezése az Azure Data Factory használatával a helyszíni HDFS</span><span class="sxs-lookup"><span data-stu-id="272ad-103">Move data from on-premises HDFS using Azure Data Factory</span></span>
<span data-ttu-id="272ad-104">Ez a cikk ismerteti, hogyan a másolási tevékenység során az Azure Data Factoryben az adatok mozgatása egy helyszíni HDFS.</span><span class="sxs-lookup"><span data-stu-id="272ad-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises HDFS.</span></span> <span data-ttu-id="272ad-105">Buildekről nyújtanak a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amelynek során adatátvitel a másolási tevékenység az általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="272ad-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="272ad-106">HDFS adatok bármely támogatott fogadó adattárolóhoz másolhatja.</span><span class="sxs-lookup"><span data-stu-id="272ad-106">You can copy data from HDFS to any supported sink data store.</span></span> <span data-ttu-id="272ad-107">A másolási tevékenység által támogatott mosdók adattárolókhoz listájáért lásd: a [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="272ad-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="272ad-108">Adat-előállító jelenleg csak áthelyezése adatait egy helyszíni HDFS egyéb adattárakhoz, de nem az egyéb adattárakhoz adatok áthelyezése egy helyszíni HDFS.</span><span class="sxs-lookup"><span data-stu-id="272ad-108">Data factory currently supports only moving data from an on-premises HDFS to other data stores, but not for moving data from other data stores to an on-premises HDFS.</span></span>

> [!NOTE]
> <span data-ttu-id="272ad-109">Másolási tevékenység nem törli a forrásfájl, miután sikerült átmásolni a cél.</span><span class="sxs-lookup"><span data-stu-id="272ad-109">Copy Activity does not delete the source file after it is successfully copied to the destination.</span></span> <span data-ttu-id="272ad-110">Ha a forrásfájl törlése után sikeres másolatot van szüksége, létrehozhat egy egyéni törölje a fájlt, és az adatcsatorna használja a tevékenységet.</span><span class="sxs-lookup"><span data-stu-id="272ad-110">If you need to delete the source file after a successful copy, create a custom activity to delete the file and use the activity in the pipeline.</span></span> 

## <a name="enabling-connectivity"></a><span data-ttu-id="272ad-111">Kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="272ad-111">Enabling connectivity</span></span>
<span data-ttu-id="272ad-112">Data Factory szolgáltatásnak a helyszíni HDFS az adatkezelési átjáró használatával történő csatlakozást támogatja.</span><span class="sxs-lookup"><span data-stu-id="272ad-112">Data Factory service supports connecting to on-premises HDFS using the Data Management Gateway.</span></span> <span data-ttu-id="272ad-113">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikkben tájékozódhat az adatkezelési átjáró és az átjáró beállításával kapcsolatos részletes útmutatás.</span><span class="sxs-lookup"><span data-stu-id="272ad-113">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step-by-step instructions on setting up the gateway.</span></span> <span data-ttu-id="272ad-114">Az átjáró használatával kapcsolódhat HDFS, még akkor is, ha egy Azure IaaS virtuális gép helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="272ad-114">Use the gateway to connect to HDFS even if it is hosted in an Azure IaaS VM.</span></span>

> [!NOTE]
> <span data-ttu-id="272ad-115">Győződjön meg arról, hogy az adatkezelési átjáró férhetnek hozzá **összes** [csomópont névkiszolgálót]: [csomópont port name] és [adatok működő kiszolgálók]: [adatok csomópont port] a Hadoop-fürt.</span><span class="sxs-lookup"><span data-stu-id="272ad-115">Make sure the Data Management Gateway can access to **ALL** the [name node server]:[name node port] and [data node servers]:[data node port] of the Hadoop cluster.</span></span> <span data-ttu-id="272ad-116">Alapértelmezés szerint a [name csomópont port] 50070, és alapértelmezett [adatok csomópont port] 50075.</span><span class="sxs-lookup"><span data-stu-id="272ad-116">Default [name node port] is 50070, and default [data node port] is 50075.</span></span>

<span data-ttu-id="272ad-117">Amíg az átjáró telepíthető az ugyanabban a helyi számítógépen, vagy az Azure virtuális Gépen, mint a HDFS, azt javasoljuk, hogy az átjáró telepítése egy különálló számítógép vagy az Azure infrastruktúra-szolgáltatási virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="272ad-117">While you can install gateway on the same on-premises machine or the Azure VM as the HDFS, we recommend that you install the gateway on a separate machine/Azure IaaS VM.</span></span> <span data-ttu-id="272ad-118">Átjáró egy külön számítógépen Erőforrásverseny csökkenti, és javítja a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="272ad-118">Having gateway on a separate machine reduces resource contention and improves performance.</span></span> <span data-ttu-id="272ad-119">Az átjáró egy külön számítógépen való telepítésekor a gép kell tudni hozzáférni a gépet, amelynek a HDFS.</span><span class="sxs-lookup"><span data-stu-id="272ad-119">When you install the gateway on a separate machine, the machine should be able to access the machine with the HDFS.</span></span>

## <a name="getting-started"></a><span data-ttu-id="272ad-120">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="272ad-120">Getting started</span></span>
<span data-ttu-id="272ad-121">A másolási tevékenység, amely helyezi át az adatokat HDFS forrásból származó különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="272ad-121">You can create a pipeline with a copy activity that moves data from a HDFS source by using different tools/APIs.</span></span>

<span data-ttu-id="272ad-122">Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="272ad-122">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="272ad-123">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) létrehozásával egy folyamatot, az adatok másolása varázsló segítségével gyorsan útmutatást.</span><span class="sxs-lookup"><span data-stu-id="272ad-123">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="272ad-124">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="272ad-124">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="272ad-125">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="272ad-125">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span>

<span data-ttu-id="272ad-126">Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban:</span><span class="sxs-lookup"><span data-stu-id="272ad-126">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="272ad-127">Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.</span><span class="sxs-lookup"><span data-stu-id="272ad-127">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="272ad-128">Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="272ad-128">Create **datasets** to represent input and output data for the copy operation.</span></span>
3. <span data-ttu-id="272ad-129">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="272ad-129">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span>

<span data-ttu-id="272ad-130">A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="272ad-130">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="272ad-131">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások a JSON formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="272ad-131">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="272ad-132">Adatok másolása a HDFS-tárolóban használt adat-előállító entitások JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az helyszíni HDFS az Azure Blob](#json-example-copy-data-from-on-premises-hdfs-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="272ad-132">For a sample with JSON definitions for Data Factory entities that are used to copy data from a HDFS data store, see [JSON example: Copy data from on-premises HDFS to Azure Blob](#json-example-copy-data-from-on-premises-hdfs-to-azure-blob) section of this article.</span></span>

<span data-ttu-id="272ad-133">A következő szakaszok részletesen bemutatják való HDFS adat-előállító tartozó entitások meghatározásához használt JSON tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="272ad-133">The following sections provide details about JSON properties that are used to define Data Factory entities specific to HDFS:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="272ad-134">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="272ad-134">Linked service properties</span></span>
<span data-ttu-id="272ad-135">A társított szolgáltatás adattárat egy adat-előállító hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="272ad-135">A linked service links a data store to a data factory.</span></span> <span data-ttu-id="272ad-136">Típusú társított szolgáltatás létrehozása **Hdfs** egy helyszíni HDFS összekapcsolása a data factory.</span><span class="sxs-lookup"><span data-stu-id="272ad-136">You create a linked service of type **Hdfs** to link an on-premises HDFS to your data factory.</span></span> <span data-ttu-id="272ad-137">A következő táblázat a társított szolgáltatás JSON-elemek szerepelnek HDFS jellemző leírást.</span><span class="sxs-lookup"><span data-stu-id="272ad-137">The following table provides description for JSON elements specific to HDFS linked service.</span></span>

| <span data-ttu-id="272ad-138">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="272ad-138">Property</span></span> | <span data-ttu-id="272ad-139">Leírás</span><span class="sxs-lookup"><span data-stu-id="272ad-139">Description</span></span> | <span data-ttu-id="272ad-140">Szükséges</span><span class="sxs-lookup"><span data-stu-id="272ad-140">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="272ad-141">type</span><span class="sxs-lookup"><span data-stu-id="272ad-141">type</span></span> |<span data-ttu-id="272ad-142">A type tulajdonságot kell beállítani: **Hdfs**</span><span class="sxs-lookup"><span data-stu-id="272ad-142">The type property must be set to: **Hdfs**</span></span> |<span data-ttu-id="272ad-143">Igen</span><span class="sxs-lookup"><span data-stu-id="272ad-143">Yes</span></span> |
| <span data-ttu-id="272ad-144">URL-cím</span><span class="sxs-lookup"><span data-stu-id="272ad-144">Url</span></span> |<span data-ttu-id="272ad-145">A HDFS URL-címe</span><span class="sxs-lookup"><span data-stu-id="272ad-145">URL to the HDFS</span></span> |<span data-ttu-id="272ad-146">Igen</span><span class="sxs-lookup"><span data-stu-id="272ad-146">Yes</span></span> |
| <span data-ttu-id="272ad-147">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="272ad-147">authenticationType</span></span> |<span data-ttu-id="272ad-148">Névtelen, vagy a Windows.</span><span class="sxs-lookup"><span data-stu-id="272ad-148">Anonymous, or Windows.</span></span> <br><br> <span data-ttu-id="272ad-149">Használandó **Kerberos-hitelesítés** HDFS-összekötőhöz, tekintse meg [ebben a szakaszban](#use-kerberos-authentication-for-hdfs-connector) ennek megfelelően a helyszíni környezet beállítása.</span><span class="sxs-lookup"><span data-stu-id="272ad-149">To use **Kerberos authentication** for HDFS connector, refer to [this section](#use-kerberos-authentication-for-hdfs-connector) to set up your on-premises environment accordingly.</span></span> |<span data-ttu-id="272ad-150">Igen</span><span class="sxs-lookup"><span data-stu-id="272ad-150">Yes</span></span> |
| <span data-ttu-id="272ad-151">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="272ad-151">userName</span></span> |<span data-ttu-id="272ad-152">Felhasználónév a Windows-hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="272ad-152">Username for Windows authentication.</span></span> |<span data-ttu-id="272ad-153">Igen (a Windows-hitelesítés)</span><span class="sxs-lookup"><span data-stu-id="272ad-153">Yes (for Windows Authentication)</span></span> |
| <span data-ttu-id="272ad-154">jelszó</span><span class="sxs-lookup"><span data-stu-id="272ad-154">password</span></span> |<span data-ttu-id="272ad-155">A Windows-hitelesítés jelszót.</span><span class="sxs-lookup"><span data-stu-id="272ad-155">Password for Windows authentication.</span></span> |<span data-ttu-id="272ad-156">Igen (a Windows-hitelesítés)</span><span class="sxs-lookup"><span data-stu-id="272ad-156">Yes (for Windows Authentication)</span></span> |
| <span data-ttu-id="272ad-157">gatewayName</span><span class="sxs-lookup"><span data-stu-id="272ad-157">gatewayName</span></span> |<span data-ttu-id="272ad-158">Az átjáró, amely a Data Factory szolgáltatásnak csatlakoznia a HDFS a neve.</span><span class="sxs-lookup"><span data-stu-id="272ad-158">Name of the gateway that the Data Factory service should use to connect to the HDFS.</span></span> |<span data-ttu-id="272ad-159">Igen</span><span class="sxs-lookup"><span data-stu-id="272ad-159">Yes</span></span> |
| <span data-ttu-id="272ad-160">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="272ad-160">encryptedCredential</span></span> |<span data-ttu-id="272ad-161">[Új AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) kimenetét a hozzáférési hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="272ad-161">[New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) output of the access credential.</span></span> |<span data-ttu-id="272ad-162">Nem</span><span class="sxs-lookup"><span data-stu-id="272ad-162">No</span></span> |

### <a name="using-anonymous-authentication"></a><span data-ttu-id="272ad-163">A névtelen hitelesítés segítségével</span><span class="sxs-lookup"><span data-stu-id="272ad-163">Using Anonymous authentication</span></span>

```JSON
{
    "name": "hdfs",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "userName": "hadoop",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-windows-authentication"></a><span data-ttu-id="272ad-164">Windows-hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="272ad-164">Using Windows authentication</span></span>

```JSON
{
    "name": "hdfs",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```
## <a name="dataset-properties"></a><span data-ttu-id="272ad-165">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="272ad-165">Dataset properties</span></span>
<span data-ttu-id="272ad-166">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="272ad-166">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="272ad-167">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="272ad-167">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="272ad-168">A **typeProperties** szakasz eltérő adatkészlet egyes típusai és információkat nyújt azokról az adattárban adatok helyét.</span><span class="sxs-lookup"><span data-stu-id="272ad-168">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="272ad-169">A typeProperties szakasz típusú adatkészlet **fájlmegosztási** (amely tartalmazza a HDFS dataset) a következő tulajdonságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="272ad-169">The typeProperties section for dataset of type **FileShare** (which includes HDFS dataset) has the following properties</span></span>

| <span data-ttu-id="272ad-170">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="272ad-170">Property</span></span> | <span data-ttu-id="272ad-171">Leírás</span><span class="sxs-lookup"><span data-stu-id="272ad-171">Description</span></span> | <span data-ttu-id="272ad-172">Szükséges</span><span class="sxs-lookup"><span data-stu-id="272ad-172">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="272ad-173">folderPath</span><span class="sxs-lookup"><span data-stu-id="272ad-173">folderPath</span></span> |<span data-ttu-id="272ad-174">A mappa elérési útját.</span><span class="sxs-lookup"><span data-stu-id="272ad-174">Path to the folder.</span></span> <span data-ttu-id="272ad-175">Példa:`myfolder`</span><span class="sxs-lookup"><span data-stu-id="272ad-175">Example: `myfolder`</span></span><br/><br/><span data-ttu-id="272ad-176">Használja az escape-karakter "\" a speciális karakterek a karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="272ad-176">Use escape character ‘ \ ’ for special characters in the string.</span></span> <span data-ttu-id="272ad-177">Például: folder\subfolder, adja meg a mappa\\\\almappa és d:\samplefolder, adja meg a d:\\\\mappába.</span><span class="sxs-lookup"><span data-stu-id="272ad-177">For example: for folder\subfolder, specify folder\\\\subfolder and for d:\samplefolder, specify d:\\\\samplefolder.</span></span><br/><br/><span data-ttu-id="272ad-178">Ez a tulajdonság a kombinálhatja **partitionBy** szeretné, hogy a mappa elérési utak alapján szelet kezdő és záró dátum-idő.</span><span class="sxs-lookup"><span data-stu-id="272ad-178">You can combine this property with **partitionBy** to have folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="272ad-179">Igen</span><span class="sxs-lookup"><span data-stu-id="272ad-179">Yes</span></span> |
| <span data-ttu-id="272ad-180">fileName</span><span class="sxs-lookup"><span data-stu-id="272ad-180">fileName</span></span> |<span data-ttu-id="272ad-181">Adja meg a fájl nevét a **folderPath** Ha azt szeretné, hogy a tábla egy adott fájlra a mappában.</span><span class="sxs-lookup"><span data-stu-id="272ad-181">Specify the name of the file in the **folderPath** if you want the table to refer to a specific file in the folder.</span></span> <span data-ttu-id="272ad-182">Ha nem ad meg ehhez a tulajdonsághoz értéket, a tábla a mappában lévő összes fájlt mutat.</span><span class="sxs-lookup"><span data-stu-id="272ad-182">If you do not specify any value for this property, the table points to all files in the folder.</span></span><br/><br/><span data-ttu-id="272ad-183">Ha nincs megadva fájlnév egy kimeneti adatkészletet, a létrehozott fájl nevét a következő lenne ebben a formátumban:</span><span class="sxs-lookup"><span data-stu-id="272ad-183">When fileName is not specified for an output dataset, the name of the generated file would be in the following this format:</span></span> <br/><br/><span data-ttu-id="272ad-184">Adatok. <Guid>.txt (például:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="272ad-184">Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="272ad-185">Nem</span><span class="sxs-lookup"><span data-stu-id="272ad-185">No</span></span> |
| <span data-ttu-id="272ad-186">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="272ad-186">partitionedBy</span></span> |<span data-ttu-id="272ad-187">Adjon meg egy dinamikus folderPath idő adatsor fájlnevét partitionedBy használható.</span><span class="sxs-lookup"><span data-stu-id="272ad-187">partitionedBy can be used to specify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="272ad-188">Példa: folderPath adatok óránkénti paraméteres.</span><span class="sxs-lookup"><span data-stu-id="272ad-188">Example: folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="272ad-189">Nem</span><span class="sxs-lookup"><span data-stu-id="272ad-189">No</span></span> |
| <span data-ttu-id="272ad-190">Formátumban</span><span class="sxs-lookup"><span data-stu-id="272ad-190">format</span></span> | <span data-ttu-id="272ad-191">A következő formátumban típusok támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="272ad-191">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="272ad-192">Állítsa be a **típus** tulajdonság a formátuma a következő értékek egyikét.</span><span class="sxs-lookup"><span data-stu-id="272ad-192">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="272ad-193">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="272ad-193">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="272ad-194">Ha azt szeretné, hogy **másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a Formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="272ad-194">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="272ad-195">Nem</span><span class="sxs-lookup"><span data-stu-id="272ad-195">No</span></span> |
| <span data-ttu-id="272ad-196">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="272ad-196">compression</span></span> | <span data-ttu-id="272ad-197">Adja meg a típus és az adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="272ad-197">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="272ad-198">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="272ad-198">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="272ad-199">Támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="272ad-199">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="272ad-200">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="272ad-200">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="272ad-201">Nem</span><span class="sxs-lookup"><span data-stu-id="272ad-201">No</span></span> |

> [!NOTE]
> <span data-ttu-id="272ad-202">fájlnév és fileFilter nem használható egyszerre.</span><span class="sxs-lookup"><span data-stu-id="272ad-202">filename and fileFilter cannot be used simultaneously.</span></span>

### <a name="using-partionedby-property"></a><span data-ttu-id="272ad-203">PartionedBy tulajdonság használatával</span><span class="sxs-lookup"><span data-stu-id="272ad-203">Using partionedBy property</span></span>
<span data-ttu-id="272ad-204">Az előző szakaszban említett, megadhat egy dinamikus folderPath és a fájlnév idő adatsorozat adatokhoz a **partitionedBy** tulajdonság, [adat-előállító funkciók és a rendszer változók](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="272ad-204">As mentioned in the previous section, you can specify a dynamic folderPath and filename for time series data with the **partitionedBy** property, [Data Factory functions, and the system variables](data-factory-functions-variables.md).</span></span>

<span data-ttu-id="272ad-205">Idő adatsorozat adatkészleteket, az ütemezés és a szeletek kapcsolatos további információkért lásd: [létrehozása adatkészletek](data-factory-create-datasets.md), [ütemezés & végrehajtási](data-factory-scheduling-and-execution.md), és [létrehozása folyamatok](data-factory-create-pipelines.md) cikkeket.</span><span class="sxs-lookup"><span data-stu-id="272ad-205">To learn more about time series datasets, scheduling, and slices, see [Creating Datasets](data-factory-create-datasets.md), [Scheduling & Execution](data-factory-scheduling-and-execution.md), and [Creating Pipelines](data-factory-create-pipelines.md) articles.</span></span>

#### <a name="sample-1"></a><span data-ttu-id="272ad-206">1. példa:</span><span class="sxs-lookup"><span data-stu-id="272ad-206">Sample 1:</span></span>

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
<span data-ttu-id="272ad-207">Ebben a példában {szelet} adat-előállító rendszer változó SliceStart (YYYYMMDDHH) formátumban megadott érték helyére.</span><span class="sxs-lookup"><span data-stu-id="272ad-207">In this example {Slice} is replaced with the value of Data Factory system variable SliceStart in the format (YYYYMMDDHH) specified.</span></span> <span data-ttu-id="272ad-208">A szelet kezdete a SliceStart hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="272ad-208">The SliceStart refers to start time of the slice.</span></span> <span data-ttu-id="272ad-209">A folderPath nem azonos az egyes szeletek.</span><span class="sxs-lookup"><span data-stu-id="272ad-209">The folderPath is different for each slice.</span></span> <span data-ttu-id="272ad-210">Például: wikidatagateway/wikisampledataout/2014100103 vagy wikidatagateway/wikisampledataout/2014100104.</span><span class="sxs-lookup"><span data-stu-id="272ad-210">For example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.</span></span>

#### <a name="sample-2"></a><span data-ttu-id="272ad-211">2. példa:</span><span class="sxs-lookup"><span data-stu-id="272ad-211">Sample 2:</span></span>

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
<span data-ttu-id="272ad-212">Ebben a példában év, hónap, nap és SliceStart idején ki kell olvasni a külön változókat, amelyek folderPath és a fájlnév tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="272ad-212">In this example, year, month, day, and time of SliceStart are extracted into separate variables that are used by folderPath and fileName properties.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="272ad-213">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="272ad-213">Copy activity properties</span></span>
<span data-ttu-id="272ad-214">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="272ad-214">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="272ad-215">Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti táblák és házirendek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="272ad-215">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="272ad-216">Mivel a tevékenység typeProperties szakaszában elérhető tulajdonságok tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="272ad-216">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="272ad-217">A másolási tevékenység során két érték források és mosdók típusától függően.</span><span class="sxs-lookup"><span data-stu-id="272ad-217">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="272ad-218">A másolási tevékenység, ha a forrás típusa nem **FileSystemSource** typeProperties szakaszában érhetők a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="272ad-218">For Copy Activity, when source is of type **FileSystemSource** the following properties are available in typeProperties section:</span></span>

<span data-ttu-id="272ad-219">**FileSystemSource** támogatja a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="272ad-219">**FileSystemSource** supports the following properties:</span></span>

| <span data-ttu-id="272ad-220">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="272ad-220">Property</span></span> | <span data-ttu-id="272ad-221">Leírás</span><span class="sxs-lookup"><span data-stu-id="272ad-221">Description</span></span> | <span data-ttu-id="272ad-222">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="272ad-222">Allowed values</span></span> | <span data-ttu-id="272ad-223">Szükséges</span><span class="sxs-lookup"><span data-stu-id="272ad-223">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="272ad-224">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="272ad-224">recursive</span></span> |<span data-ttu-id="272ad-225">Azt jelzi, hogy az adatok olvasható rekurzív módon az almappák vagy csak a megadott mappát.</span><span class="sxs-lookup"><span data-stu-id="272ad-225">Indicates whether the data is read recursively from the sub folders or only from the specified folder.</span></span> |<span data-ttu-id="272ad-226">IGAZ, hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="272ad-226">True, False (default)</span></span> |<span data-ttu-id="272ad-227">Nem</span><span class="sxs-lookup"><span data-stu-id="272ad-227">No</span></span> |

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="272ad-228">Támogatott formátumú és tömörítés</span><span class="sxs-lookup"><span data-stu-id="272ad-228">Supported file and compression formats</span></span>
<span data-ttu-id="272ad-229">Lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md) cikk részletei.</span><span class="sxs-lookup"><span data-stu-id="272ad-229">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-example-copy-data-from-on-premises-hdfs-to-azure-blob"></a><span data-ttu-id="272ad-230">JSON-példa: adatok másolása az helyszíni HDFS az Azure-Blobba</span><span class="sxs-lookup"><span data-stu-id="272ad-230">JSON example: Copy data from on-premises HDFS to Azure Blob</span></span>
<span data-ttu-id="272ad-231">Ez a példa bemutatja, hogyan egy helyszíni HDFS adatok másolása az Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="272ad-231">This sample shows how to copy data from an on-premises HDFS to Azure Blob Storage.</span></span> <span data-ttu-id="272ad-232">Azonban az adatok átmásolhatók **közvetlenül** bármely, a megadott nyelő [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a másolási tevékenység során az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="272ad-232">However, data can be copied **directly** to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>  

<span data-ttu-id="272ad-233">A minta a következő Data Factory-entitásokhoz JSON-definíciókat biztosítja.</span><span class="sxs-lookup"><span data-stu-id="272ad-233">The sample provides JSON definitions for the following Data Factory entities.</span></span> <span data-ttu-id="272ad-234">E definíciókat hozhat létre egy folyamatot, az adatok másolása az HDFS az Azure Blob Storage használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="272ad-234">You can use these definitions to create a pipeline to copy data from HDFS to Azure Blob Storage by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span>

1. <span data-ttu-id="272ad-235">A társított szolgáltatás típusa [OnPremisesHdfs](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="272ad-235">A linked service of type [OnPremisesHdfs](#linked-service-properties).</span></span>
2. <span data-ttu-id="272ad-236">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="272ad-236">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="272ad-237">Bemeneti [dataset](data-factory-create-datasets.md) típusú [fájlmegosztási](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="272ad-237">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties).</span></span>
4. <span data-ttu-id="272ad-238">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="272ad-238">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="272ad-239">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [FileSystemSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="272ad-239">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="272ad-240">A minta másol adatokat egy helyszíni HDFS egy Azure blob minden órában.</span><span class="sxs-lookup"><span data-stu-id="272ad-240">The sample copies data from an on-premises HDFS to an Azure blob every hour.</span></span> <span data-ttu-id="272ad-241">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="272ad-241">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="272ad-242">Első lépésként, állítsa be az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="272ad-242">As a first step, set up the data management gateway.</span></span> <span data-ttu-id="272ad-243">Utasításait a [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="272ad-243">The instructions in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="272ad-244">**HDFS társított szolgáltatás:** ebben a példában a Windows-hitelesítést használ.</span><span class="sxs-lookup"><span data-stu-id="272ad-244">**HDFS linked service:** This example uses the Windows authentication.</span></span> <span data-ttu-id="272ad-245">Lásd: [HDFS társított szolgáltatás](#linked-service-properties) szakasz a különböző típusú hitelesítés használható.</span><span class="sxs-lookup"><span data-stu-id="272ad-245">See [HDFS linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

```JSON
{
    "name": "HDFSLinkedService",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```

<span data-ttu-id="272ad-246">**Az Azure tárolás társított szolgáltatásának:**</span><span class="sxs-lookup"><span data-stu-id="272ad-246">**Azure Storage linked service:**</span></span>

```JSON
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

<span data-ttu-id="272ad-247">**HDFS bemeneti adatkészletéből:** Ez az adatkészlet hivatkozik a HDFS mappa DataTransfer/UnitTest /.</span><span class="sxs-lookup"><span data-stu-id="272ad-247">**HDFS input dataset:** This dataset refers to the HDFS folder DataTransfer/UnitTest/.</span></span> <span data-ttu-id="272ad-248">A feldolgozási sor másolja a fájlokat ebben a mappában a cél.</span><span class="sxs-lookup"><span data-stu-id="272ad-248">The pipeline copies all the files in this folder to the destination.</span></span>

<span data-ttu-id="272ad-249">"External" beállítása: "true" arról tájékoztatja a Data Factory szolgáltatásnak, hogy az adatkészlet külső data factoryval való és adat-előállító tevékenység nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="272ad-249">Setting “external”: ”true” informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```JSON
{
    "name": "InputDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "HDFSLinkedService",
        "typeProperties": {
            "folderPath": "DataTransfer/UnitTest/"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

<span data-ttu-id="272ad-250">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="272ad-250">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="272ad-251">Adatot ír egy új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="272ad-251">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="272ad-252">A mappa elérési útját a BLOB a szelet által feldolgozott kezdési ideje alapján dinamikusan történik.</span><span class="sxs-lookup"><span data-stu-id="272ad-252">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="272ad-253">A mappa elérési útját használja, év, hónap, nap és a kezdési idő órában részeit.</span><span class="sxs-lookup"><span data-stu-id="272ad-253">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```JSON
{
    "name": "OutputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

<span data-ttu-id="272ad-254">**A másolási tevékenység során a fájlrendszer és a Blob fogadó folyamat:**</span><span class="sxs-lookup"><span data-stu-id="272ad-254">**A copy activity in a pipeline with File System source and Blob sink:**</span></span>

<span data-ttu-id="272ad-255">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="272ad-255">The pipeline contains a Copy Activity that is configured to use these input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="272ad-256">Az adatcsatorna JSON-definícióból a **forrás** típusúra **FileSystemSource** és **fogadó** típusúra **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="272ad-256">In the pipeline JSON definition, the **source** type is set to **FileSystemSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="272ad-257">A megadott SQL-lekérdezést a **lekérdezés** tulajdonság kiválasztása az adatok másolása az elmúlt órában.</span><span class="sxs-lookup"><span data-stu-id="272ad-257">The SQL query specified for the **query** property selects the data in the past hour to copy.</span></span>

```JSON
{
    "name": "pipeline",
    "properties":
    {
        "activities":
        [
            {
                "name": "HdfsToBlobCopy",
                "inputs": [ {"name": "InputDataset"} ],
                "outputs": [ {"name": "OutputDataset"} ],
                "type": "Copy",
                "typeProperties":
                {
                    "source":
                    {
                        "type": "FileSystemSource"
                    },
                    "sink":
                    {
                        "type": "BlobSink"
                    }
                },
                "policy":
                {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "00:05:00"
                }
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="use-kerberos-authentication-for-hdfs-connector"></a><span data-ttu-id="272ad-258">A HDFS-összekötő Kerberos-hitelesítés használata</span><span class="sxs-lookup"><span data-stu-id="272ad-258">Use Kerberos authentication for HDFS connector</span></span>
<span data-ttu-id="272ad-259">A helyszíni környezet beállítása úgy, hogy a Kerberos-hitelesítés használatát a HDFS connector két lehetőség áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="272ad-259">There are two options to set up the on-premises environment so as to use Kerberos Authentication in HDFS connector.</span></span> <span data-ttu-id="272ad-260">Kiválaszthatja a legjobban az esethez.</span><span class="sxs-lookup"><span data-stu-id="272ad-260">You can choose the one better fits your case.</span></span>
* <span data-ttu-id="272ad-261">1. lehetőség: [illesztési átjárót működtető gépen, a Kerberos-tartomány](#kerberos-join-realm)</span><span class="sxs-lookup"><span data-stu-id="272ad-261">Option 1: [Join gateway machine in Kerberos realm](#kerberos-join-realm)</span></span>
* <span data-ttu-id="272ad-262">2. lehetőség: [kölcsönös, a Windows-tartomány és a Kerberos-tartomány közötti megbízhatósági kapcsolat engedélyezése](#kerberos-mutual-trust)</span><span class="sxs-lookup"><span data-stu-id="272ad-262">Option 2: [Enable mutual trust between Windows domain and Kerberos realm](#kerberos-mutual-trust)</span></span>

### <span data-ttu-id="272ad-263"><a name="kerberos-join-realm"></a>1. lehetőség: Illesztés átjárót működtető gépen, Kerberos-tartomány</span><span class="sxs-lookup"><span data-stu-id="272ad-263"><a name="kerberos-join-realm"></a>Option 1: Join gateway machine in Kerberos realm</span></span>

#### <a name="requirement"></a><span data-ttu-id="272ad-264">Követelmény:</span><span class="sxs-lookup"><span data-stu-id="272ad-264">Requirement:</span></span>

* <span data-ttu-id="272ad-265">Az átjáró számítógépe a Kerberos-tartomány csatlakozni kell, és nem tud csatlakozni a Windows-tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="272ad-265">The gateway machine needs to join the Kerberos realm and can’t join any Windows domain.</span></span>

#### <a name="how-to-configure"></a><span data-ttu-id="272ad-266">Hogyan kell konfigurálni:</span><span class="sxs-lookup"><span data-stu-id="272ad-266">How to configure:</span></span>

<span data-ttu-id="272ad-267">**Az átjáró számítógépén:**</span><span class="sxs-lookup"><span data-stu-id="272ad-267">**On gateway machine:**</span></span>

1.  <span data-ttu-id="272ad-268">Futtassa a **Ksetup** segédprogram a Kerberos Kulcsszolgáltató kiszolgáló és a tartomány beállításához.</span><span class="sxs-lookup"><span data-stu-id="272ad-268">Run the **Ksetup** utility to configure the Kerberos KDC server and realm.</span></span>

    <span data-ttu-id="272ad-269">A számítógép egy munkacsoport tagjaként kell konfigurálni óta egy Kerberos-tartomány eltér a Windows-tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="272ad-269">The machine must be configured as a member of a workgroup since a Kerberos realm is different from a Windows domain.</span></span> <span data-ttu-id="272ad-270">Ez a Kerberos-tartomány és a KDC-kiszolgáló hozzáadása az alábbiak szerint elérhető.</span><span class="sxs-lookup"><span data-stu-id="272ad-270">This can be achieved by setting the Kerberos realm and adding a KDC server as follows.</span></span> <span data-ttu-id="272ad-271">Cserélje le *REALM.COM* a saját megfelelő terület szükséges.</span><span class="sxs-lookup"><span data-stu-id="272ad-271">Replace *REALM.COM* with your own respective realm as needed.</span></span>

            C:> Ksetup /setdomain REALM.COM
            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>

    <span data-ttu-id="272ad-272">**Indítsa újra a** 2 parancsok végrehajtása után a gép.</span><span class="sxs-lookup"><span data-stu-id="272ad-272">**Restart** the machine after executing these 2 commands.</span></span>

2.  <span data-ttu-id="272ad-273">A konfiguráció ellenőrzése a **Ksetup** parancsot.</span><span class="sxs-lookup"><span data-stu-id="272ad-273">Verify the configuration with **Ksetup** command.</span></span> <span data-ttu-id="272ad-274">A kimeneti kell lennie, például:</span><span class="sxs-lookup"><span data-stu-id="272ad-274">The output should be like:</span></span>

            C:> Ksetup
            default realm = REALM.COM (external)
            REALM.com:
                kdc = <your_kdc_server_address>

<span data-ttu-id="272ad-275">**Az Azure Data Factoryben:**</span><span class="sxs-lookup"><span data-stu-id="272ad-275">**In Azure Data Factory:**</span></span>

* <span data-ttu-id="272ad-276">Konfigurálhatja a HDFS összekötő segítségével **Windows-hitelesítés** együtt a Kerberos egyszerű neve és a jelszót a HDFS-adatforráshoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="272ad-276">Configure the HDFS connector using **Windows authentication** together with your Kerberos principal name and password to connect to the HDFS data source.</span></span> <span data-ttu-id="272ad-277">Ellenőrizze [HDFS társított szolgáltatás Tulajdonságok](#linked-service-properties) konfiguráció részletei című szakaszban.</span><span class="sxs-lookup"><span data-stu-id="272ad-277">Check [HDFS Linked Service properties](#linked-service-properties) section on configuration details.</span></span>

### <span data-ttu-id="272ad-278"><a name="kerberos-mutual-trust"></a>2. lehetőség: A Windows-tartomány és a Kerberos-tartomány közötti kölcsönös megbízhatósági engedélyezése</span><span class="sxs-lookup"><span data-stu-id="272ad-278"><a name="kerberos-mutual-trust"></a>Option 2: Enable mutual trust between Windows domain and Kerberos realm</span></span>

#### <a name="requirement"></a><span data-ttu-id="272ad-279">Követelmény:</span><span class="sxs-lookup"><span data-stu-id="272ad-279">Requirement:</span></span>
*   <span data-ttu-id="272ad-280">Az átjáró számítógépe Windows-tartományhoz kell csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="272ad-280">The gateway machine must join a Windows domain.</span></span>
*   <span data-ttu-id="272ad-281">A tartományvezérlő-beállítások frissítése engedéllyel kell rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="272ad-281">You need permission to update the domain controller's settings.</span></span>

#### <a name="how-to-configure"></a><span data-ttu-id="272ad-282">Hogyan kell konfigurálni:</span><span class="sxs-lookup"><span data-stu-id="272ad-282">How to configure:</span></span>

> [!NOTE]
> <span data-ttu-id="272ad-283">Cserélje le REALM.COM és AD.COM az alábbi oktatóanyag saját megfelelő tartomány és a tartományvezérlő igény szerint.</span><span class="sxs-lookup"><span data-stu-id="272ad-283">Replace REALM.COM and AD.COM in the following tutorial with your own respective realm and domain controller as needed.</span></span>

<span data-ttu-id="272ad-284">**KDC-kiszolgálón:**</span><span class="sxs-lookup"><span data-stu-id="272ad-284">**On KDC server:**</span></span>

1.  <span data-ttu-id="272ad-285">A KDC konfigurációjának szerkesztése **krb5.conf** fájlt, hogy a KDC se hivatkozzon a következő konfigurációs Windows-tartományt.</span><span class="sxs-lookup"><span data-stu-id="272ad-285">Edit the KDC configuration in **krb5.conf** file to let KDC trust Windows Domain referring to the following configuration template.</span></span> <span data-ttu-id="272ad-286">Alapértelmezés szerint a konfigurációja itt található: **/etc/krb5.conf**.</span><span class="sxs-lookup"><span data-stu-id="272ad-286">By default, the configuration is located at **/etc/krb5.conf**.</span></span>

            [logging]
             default = FILE:/var/log/krb5libs.log
             kdc = FILE:/var/log/krb5kdc.log
             admin_server = FILE:/var/log/kadmind.log

            [libdefaults]
             default_realm = REALM.COM
             dns_lookup_realm = false
             dns_lookup_kdc = false
             ticket_lifetime = 24h
             renew_lifetime = 7d
             forwardable = true

            [realms]
             REALM.COM = {
              kdc = node.REALM.COM
              admin_server = node.REALM.COM
             }
            AD.COM = {
             kdc = windc.ad.com
             admin_server = windc.ad.com
            }

            [domain_realm]
             .REALM.COM = REALM.COM
             REALM.COM = REALM.COM
             .ad.com = AD.COM
             ad.com = AD.COM

            [capaths]
             AD.COM = {
              REALM.COM = .
             }

  <span data-ttu-id="272ad-287">**Indítsa újra a** a KDC-szolgáltatás konfigurálása után.</span><span class="sxs-lookup"><span data-stu-id="272ad-287">**Restart** the KDC service after configuration.</span></span>

2.  <span data-ttu-id="272ad-288">Készítse elő a rendszerbiztonsági tag nevű  **krbtgt/REALM.COM@AD.COM**  a KDC-kiszolgáló a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="272ad-288">Prepare a principal named **krbtgt/REALM.COM@AD.COM** in KDC server with the following command:</span></span>

            Kadmin> addprinc krbtgt/REALM.COM@AD.COM

3.  <span data-ttu-id="272ad-289">A **hadoop.security.auth_to_local** HDFS-szolgáltatás konfigurációs fájlt, adja hozzá `RULE:[1:$1@$0](.*@AD.COM)s/@.*//`.</span><span class="sxs-lookup"><span data-stu-id="272ad-289">In **hadoop.security.auth_to_local** HDFS service configuration file, add `RULE:[1:$1@$0](.*@AD.COM)s/@.*//`.</span></span>

<span data-ttu-id="272ad-290">**A tartományvezérlőn:**</span><span class="sxs-lookup"><span data-stu-id="272ad-290">**On domain controller:**</span></span>

1.  <span data-ttu-id="272ad-291">Futtassa a következő **Ksetup** parancsok tartomány bejegyzés hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="272ad-291">Run the following **Ksetup** commands to add a realm entry:</span></span>

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

2.  <span data-ttu-id="272ad-292">Windows-tartomány Kerberos-tartomány bizalmi kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="272ad-292">Establish trust from Windows Domain to Kerberos Realm.</span></span> <span data-ttu-id="272ad-293">[jelszó] pedig a jelszót a rendszerbiztonsági tag  **krbtgt/REALM.COM@AD.COM** .</span><span class="sxs-lookup"><span data-stu-id="272ad-293">[password] is the password for the principal **krbtgt/REALM.COM@AD.COM**.</span></span>

            C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /passwordt:[password]

3.  <span data-ttu-id="272ad-294">Válassza ki a Kerberos használt titkosítási algoritmus.</span><span class="sxs-lookup"><span data-stu-id="272ad-294">Select encryption algorithm used in Kerberos.</span></span>

    1. <span data-ttu-id="272ad-295">Nyissa meg a Kiszolgálókezelő > csoportházirend-kezelés > tartomány > csoportházirend-objektumok > alapértelmezett vagy az aktív tartományi házirend és a Szerkesztés.</span><span class="sxs-lookup"><span data-stu-id="272ad-295">Go to Server Manager > Group Policy Management > Domain > Group Policy Objects > Default or Active Domain Policy, and Edit.</span></span>

    2. <span data-ttu-id="272ad-296">A a **Csoportházirendkezelés-szerkesztő** előugró ablakban, keresse fel a számítógép konfigurációja > házirendek > Windows-beállítások > biztonsági beállítások > helyi házirend > biztonsági beállítások, és konfigurálja **hálózati biztonság: konfigurálása titkosítási típusok engedélyezett Kerberos**.</span><span class="sxs-lookup"><span data-stu-id="272ad-296">In the **Group Policy Management Editor** popup window, go to Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options, and configure **Network security: Configure Encryption types allowed for Kerberos**.</span></span>

    3. <span data-ttu-id="272ad-297">Válassza ki a titkosítási algoritmust szeretné használni, ha a KDC csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="272ad-297">Select the encryption algorithm you want to use when connect to KDC.</span></span> <span data-ttu-id="272ad-298">Gyakran egyszerűen kiválaszthatja a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="272ad-298">Commonly, you can simply select all the options.</span></span>

        ![A Kerberos config titkosítási típusok](media/data-factory-hdfs-connector/config-encryption-types-for-kerberos.png)

    4. <span data-ttu-id="272ad-300">Használjon **Ksetup** parancs használatával adja meg a titkosítási algoritmus az adott tartomány kell használni.</span><span class="sxs-lookup"><span data-stu-id="272ad-300">Use **Ksetup** command to specify the encryption algorithm to be used on the specific REALM.</span></span>

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  <span data-ttu-id="272ad-301">A tartományi fiókot és a Kerberos egyszerű közötti leképezést létrehozása a Windows-tartomány Kerberos egyszerű használatához.</span><span class="sxs-lookup"><span data-stu-id="272ad-301">Create the mapping between the domain account and Kerberos principal, in order to use Kerberos principal in Windows Domain.</span></span>

    1. <span data-ttu-id="272ad-302">Indítsa el a felügyeleti eszközök > **Active Directory – felhasználók és számítógépek**.</span><span class="sxs-lookup"><span data-stu-id="272ad-302">Start the Administrative tools > **Active Directory Users and Computers**.</span></span>

    2. <span data-ttu-id="272ad-303">Kattintson a speciális szolgáltatások konfigurálása **nézet** > **speciális funkciók**.</span><span class="sxs-lookup"><span data-stu-id="272ad-303">Configure advanced features by clicking **View** > **Advanced Features**.</span></span>

    3. <span data-ttu-id="272ad-304">Keresse meg a fiókot, amelyhez hozzárendelések létrehozásához, és a jobb gombbal kattintva **a felhasználónév-leképezések** > kattintson **Kerberos-nevek** fülre.</span><span class="sxs-lookup"><span data-stu-id="272ad-304">Locate the account to which you want to create mappings, and right-click to view **Name Mappings** > click **Kerberos Names** tab.</span></span>

    4. <span data-ttu-id="272ad-305">Adja hozzá a tartományi rendszerbiztonsági tag.</span><span class="sxs-lookup"><span data-stu-id="272ad-305">Add a principal from the realm.</span></span>

        ![Térkép biztonsági azonosító](media/data-factory-hdfs-connector/map-security-identity.png)

<span data-ttu-id="272ad-307">**Az átjáró számítógépén:**</span><span class="sxs-lookup"><span data-stu-id="272ad-307">**On gateway machine:**</span></span>

* <span data-ttu-id="272ad-308">Futtassa a következő **Ksetup** parancs használatával adja hozzá egy tartományi bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="272ad-308">Run the following **Ksetup** commands to add a realm entry.</span></span>

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

<span data-ttu-id="272ad-309">**Az Azure Data Factoryben:**</span><span class="sxs-lookup"><span data-stu-id="272ad-309">**In Azure Data Factory:**</span></span>

* <span data-ttu-id="272ad-310">Konfigurálhatja a HDFS összekötő segítségével **Windows-hitelesítés** együtt tartományi fiók vagy a Kerberos egyszerű a HDFS-adatforráshoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="272ad-310">Configure the HDFS connector using **Windows authentication** together with either your Domain Account or Kerberos Principal to connect to the HDFS data source.</span></span> <span data-ttu-id="272ad-311">Ellenőrizze [HDFS társított szolgáltatás Tulajdonságok](#linked-service-properties) konfiguráció részletei című szakaszban.</span><span class="sxs-lookup"><span data-stu-id="272ad-311">Check [HDFS Linked Service properties](#linked-service-properties) section on configuration details.</span></span>

> [!NOTE]
> <span data-ttu-id="272ad-312">Képezze le a fogadó adatkészletből oszlopok forrás adatkészletből oszlopokat, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="272ad-312">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>


## <a name="performance-and-tuning"></a><span data-ttu-id="272ad-313">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="272ad-313">Performance and Tuning</span></span>
<span data-ttu-id="272ad-314">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) tájékozódhat az kulcsfontosságú szerepet játszik adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon optimalizálhatja azt, hogy hatás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="272ad-314">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
