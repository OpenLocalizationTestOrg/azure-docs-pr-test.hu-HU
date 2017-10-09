---
title: "a helyszíni HDFS aaaMove adatait |} Microsoft Docs"
description: "További információk a hogyan toomove adatokat a helyszíni HDFS Azure Data Factory használatával."
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
ms.openlocfilehash: 96387e5dd089099fc2e983ab26d67c2044b973b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a><span data-ttu-id="512e7-103">Adatok áthelyezése az Azure Data Factory használatával a helyszíni HDFS</span><span class="sxs-lookup"><span data-stu-id="512e7-103">Move data from on-premises HDFS using Azure Data Factory</span></span>
<span data-ttu-id="512e7-104">Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatait egy helyszíni HDFS a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="512e7-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises HDFS.</span></span> <span data-ttu-id="512e7-105">-Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="512e7-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="512e7-106">HDFS támogatott tooany fogadó adattár adatainak másolhatja.</span><span class="sxs-lookup"><span data-stu-id="512e7-106">You can copy data from HDFS tooany supported sink data store.</span></span> <span data-ttu-id="512e7-107">Az adatok támogatott tárolja, a fogadók esetében hello másolási tevékenység, lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="512e7-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="512e7-108">Adat-előállító jelenleg csak áthelyezése adatait egy helyszíni HDFS tooother adattárolókhoz támogatja, de a nem az adatok áthelyezését más adatokat tároló tooan a helyszíni HDFS.</span><span class="sxs-lookup"><span data-stu-id="512e7-108">Data factory currently supports only moving data from an on-premises HDFS tooother data stores, but not for moving data from other data stores tooan on-premises HDFS.</span></span>

> [!NOTE]
> <span data-ttu-id="512e7-109">Másolási tevékenység nem hello forrásfájl törlése, miután sikeresen másolt toohello cél.</span><span class="sxs-lookup"><span data-stu-id="512e7-109">Copy Activity does not delete hello source file after it is successfully copied toohello destination.</span></span> <span data-ttu-id="512e7-110">Ha toodelete hello forrásfájl után sikeres másolatát, hozzon létre egy egyéni tevékenység toodelete hello fájlt, és a hello tevékenység hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="512e7-110">If you need toodelete hello source file after a successful copy, create a custom activity toodelete hello file and use hello activity in hello pipeline.</span></span> 

## <a name="enabling-connectivity"></a><span data-ttu-id="512e7-111">Kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="512e7-111">Enabling connectivity</span></span>
<span data-ttu-id="512e7-112">Data Factory szolgáltatásnak hello az adatkezelési átjáró használatával csatlakozó tooon helyszíni HDFS támogatja.</span><span class="sxs-lookup"><span data-stu-id="512e7-112">Data Factory service supports connecting tooon-premises HDFS using hello Data Management Gateway.</span></span> <span data-ttu-id="512e7-113">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk toolearn az adatkezelési átjáró és hello átjáró beállításával kapcsolatos részletes utasításokat.</span><span class="sxs-lookup"><span data-stu-id="512e7-113">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span> <span data-ttu-id="512e7-114">Hello átjáró tooconnect tooHDFS használja, akkor is, ha egy Azure IaaS virtuális gép helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="512e7-114">Use hello gateway tooconnect tooHDFS even if it is hosted in an Azure IaaS VM.</span></span>

> [!NOTE]
> <span data-ttu-id="512e7-115">Győződjön meg arról, hogy hello túl férhetnek hozzá az adatkezelési átjáró**összes** hello [névkiszolgáló csomópont]: [csomópont port name] és [adatok működő kiszolgálók]: [adatok csomópont port] hello Hadoop-fürt.</span><span class="sxs-lookup"><span data-stu-id="512e7-115">Make sure hello Data Management Gateway can access too**ALL** hello [name node server]:[name node port] and [data node servers]:[data node port] of hello Hadoop cluster.</span></span> <span data-ttu-id="512e7-116">Alapértelmezés szerint a [name csomópont port] 50070, és alapértelmezett [adatok csomópont port] 50075.</span><span class="sxs-lookup"><span data-stu-id="512e7-116">Default [name node port] is 50070, and default [data node port] is 50075.</span></span>

<span data-ttu-id="512e7-117">Amíg az átjáró telepíthető hello azonos helyszíni gépet vagy hello Azure virtuális gép hello HDFS, azt javasoljuk, hogy hello átjáró telepítése egy különálló számítógép vagy az Azure infrastruktúra-szolgáltatási virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="512e7-117">While you can install gateway on hello same on-premises machine or hello Azure VM as hello HDFS, we recommend that you install hello gateway on a separate machine/Azure IaaS VM.</span></span> <span data-ttu-id="512e7-118">Átjáró egy külön számítógépen Erőforrásverseny csökkenti, és javítja a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="512e7-118">Having gateway on a separate machine reduces resource contention and improves performance.</span></span> <span data-ttu-id="512e7-119">Hello átjáró egy külön számítógépen való telepítésekor hello gépnek képes tooaccess hello gép hello HDFS kell lennie.</span><span class="sxs-lookup"><span data-stu-id="512e7-119">When you install hello gateway on a separate machine, hello machine should be able tooaccess hello machine with hello HDFS.</span></span>

## <a name="getting-started"></a><span data-ttu-id="512e7-120">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="512e7-120">Getting started</span></span>
<span data-ttu-id="512e7-121">A másolási tevékenység, amely helyezi át az adatokat HDFS forrásból származó különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="512e7-121">You can create a pipeline with a copy activity that moves data from a HDFS source by using different tools/APIs.</span></span>

<span data-ttu-id="512e7-122">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="512e7-122">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="512e7-123">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="512e7-123">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="512e7-124">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="512e7-124">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="512e7-125">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="512e7-125">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span>

<span data-ttu-id="512e7-126">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="512e7-126">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="512e7-127">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="512e7-127">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="512e7-128">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="512e7-128">Create **datasets** toorepresent input and output data for hello copy operation.</span></span>
3. <span data-ttu-id="512e7-129">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="512e7-129">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span>

<span data-ttu-id="512e7-130">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="512e7-130">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="512e7-131">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="512e7-131">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="512e7-132">Egy minta JSON-definíciók az adat-előállító entitások, amelyek a HDFS-tárolóban használt toocopy adatait, tekintse meg [JSON-példa: adatok másolása a helyszíni HDFS tooAzure Blob](#json-example-copy-data-from-on-premises-hdfs-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="512e7-132">For a sample with JSON definitions for Data Factory entities that are used toocopy data from a HDFS data store, see [JSON example: Copy data from on-premises HDFS tooAzure Blob](#json-example-copy-data-from-on-premises-hdfs-to-azure-blob) section of this article.</span></span>

<span data-ttu-id="512e7-133">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooHDFS részleteit tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="512e7-133">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooHDFS:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="512e7-134">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="512e7-134">Linked service properties</span></span>
<span data-ttu-id="512e7-135">A társított szolgáltatás adatokat tároló tooa adat-előállító hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="512e7-135">A linked service links a data store tooa data factory.</span></span> <span data-ttu-id="512e7-136">Típusú társított szolgáltatás létrehozása **Hdfs** toolink egy helyszíni HDFS tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="512e7-136">You create a linked service of type **Hdfs** toolink an on-premises HDFS tooyour data factory.</span></span> <span data-ttu-id="512e7-137">a következő táblázat hello biztosít JSON elemek adott tooHDFS kapcsolódó szolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="512e7-137">hello following table provides description for JSON elements specific tooHDFS linked service.</span></span>

| <span data-ttu-id="512e7-138">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="512e7-138">Property</span></span> | <span data-ttu-id="512e7-139">Leírás</span><span class="sxs-lookup"><span data-stu-id="512e7-139">Description</span></span> | <span data-ttu-id="512e7-140">Szükséges</span><span class="sxs-lookup"><span data-stu-id="512e7-140">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="512e7-141">type</span><span class="sxs-lookup"><span data-stu-id="512e7-141">type</span></span> |<span data-ttu-id="512e7-142">hello type tulajdonságot kell beállítani: **Hdfs**</span><span class="sxs-lookup"><span data-stu-id="512e7-142">hello type property must be set to: **Hdfs**</span></span> |<span data-ttu-id="512e7-143">Igen</span><span class="sxs-lookup"><span data-stu-id="512e7-143">Yes</span></span> |
| <span data-ttu-id="512e7-144">URL-cím</span><span class="sxs-lookup"><span data-stu-id="512e7-144">Url</span></span> |<span data-ttu-id="512e7-145">URL-cím toohello HDFS</span><span class="sxs-lookup"><span data-stu-id="512e7-145">URL toohello HDFS</span></span> |<span data-ttu-id="512e7-146">Igen</span><span class="sxs-lookup"><span data-stu-id="512e7-146">Yes</span></span> |
| <span data-ttu-id="512e7-147">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="512e7-147">authenticationType</span></span> |<span data-ttu-id="512e7-148">Névtelen, vagy a Windows.</span><span class="sxs-lookup"><span data-stu-id="512e7-148">Anonymous, or Windows.</span></span> <br><br> <span data-ttu-id="512e7-149">toouse **Kerberos-hitelesítés** HDFS-összekötőhöz, tekintse meg túl[ebben a szakaszban](#use-kerberos-authentication-for-hdfs-connector) a helyszíni környezet tooset ennek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="512e7-149">toouse **Kerberos authentication** for HDFS connector, refer too[this section](#use-kerberos-authentication-for-hdfs-connector) tooset up your on-premises environment accordingly.</span></span> |<span data-ttu-id="512e7-150">Igen</span><span class="sxs-lookup"><span data-stu-id="512e7-150">Yes</span></span> |
| <span data-ttu-id="512e7-151">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="512e7-151">userName</span></span> |<span data-ttu-id="512e7-152">Felhasználónév a Windows-hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="512e7-152">Username for Windows authentication.</span></span> |<span data-ttu-id="512e7-153">Igen (a Windows-hitelesítés)</span><span class="sxs-lookup"><span data-stu-id="512e7-153">Yes (for Windows Authentication)</span></span> |
| <span data-ttu-id="512e7-154">jelszó</span><span class="sxs-lookup"><span data-stu-id="512e7-154">password</span></span> |<span data-ttu-id="512e7-155">A Windows-hitelesítés jelszót.</span><span class="sxs-lookup"><span data-stu-id="512e7-155">Password for Windows authentication.</span></span> |<span data-ttu-id="512e7-156">Igen (a Windows-hitelesítés)</span><span class="sxs-lookup"><span data-stu-id="512e7-156">Yes (for Windows Authentication)</span></span> |
| <span data-ttu-id="512e7-157">gatewayName</span><span class="sxs-lookup"><span data-stu-id="512e7-157">gatewayName</span></span> |<span data-ttu-id="512e7-158">Hello átjáró, amely a Data Factory szolgáltatásnak hello neve tooconnect toohello HDFS kell használnia.</span><span class="sxs-lookup"><span data-stu-id="512e7-158">Name of hello gateway that hello Data Factory service should use tooconnect toohello HDFS.</span></span> |<span data-ttu-id="512e7-159">Igen</span><span class="sxs-lookup"><span data-stu-id="512e7-159">Yes</span></span> |
| <span data-ttu-id="512e7-160">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="512e7-160">encryptedCredential</span></span> |<span data-ttu-id="512e7-161">[Új AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) hello hozzáférési hitelesítő adatok kimenetét.</span><span class="sxs-lookup"><span data-stu-id="512e7-161">[New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) output of hello access credential.</span></span> |<span data-ttu-id="512e7-162">Nem</span><span class="sxs-lookup"><span data-stu-id="512e7-162">No</span></span> |

### <a name="using-anonymous-authentication"></a><span data-ttu-id="512e7-163">A névtelen hitelesítés segítségével</span><span class="sxs-lookup"><span data-stu-id="512e7-163">Using Anonymous authentication</span></span>

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

### <a name="using-windows-authentication"></a><span data-ttu-id="512e7-164">Windows-hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="512e7-164">Using Windows authentication</span></span>

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
## <a name="dataset-properties"></a><span data-ttu-id="512e7-165">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="512e7-165">Dataset properties</span></span>
<span data-ttu-id="512e7-166">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="512e7-166">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="512e7-167">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="512e7-167">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="512e7-168">Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="512e7-168">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="512e7-169">hello typeProperties szakasz típusú adatkészlet **fájlmegosztási** következő tulajdonságai hello (amely tartalmazza a HDFS dataset) rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="512e7-169">hello typeProperties section for dataset of type **FileShare** (which includes HDFS dataset) has hello following properties</span></span>

| <span data-ttu-id="512e7-170">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="512e7-170">Property</span></span> | <span data-ttu-id="512e7-171">Leírás</span><span class="sxs-lookup"><span data-stu-id="512e7-171">Description</span></span> | <span data-ttu-id="512e7-172">Szükséges</span><span class="sxs-lookup"><span data-stu-id="512e7-172">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="512e7-173">folderPath</span><span class="sxs-lookup"><span data-stu-id="512e7-173">folderPath</span></span> |<span data-ttu-id="512e7-174">Toohello mappa elérési útja.</span><span class="sxs-lookup"><span data-stu-id="512e7-174">Path toohello folder.</span></span> <span data-ttu-id="512e7-175">Példa:`myfolder`</span><span class="sxs-lookup"><span data-stu-id="512e7-175">Example: `myfolder`</span></span><br/><br/><span data-ttu-id="512e7-176">Használja az escape-karakter "\" hello karakterlánc speciális karakter.</span><span class="sxs-lookup"><span data-stu-id="512e7-176">Use escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="512e7-177">Például: folder\subfolder, adja meg a mappa\\\\almappa és d:\samplefolder, adja meg a d:\\\\mappába.</span><span class="sxs-lookup"><span data-stu-id="512e7-177">For example: for folder\subfolder, specify folder\\\\subfolder and for d:\samplefolder, specify d:\\\\samplefolder.</span></span><br/><br/><span data-ttu-id="512e7-178">Ez a tulajdonság a kombinálhatja **partitionBy** toohave mappák elérési útjaiban szelet alapján kezdő és záró dátum-idő.</span><span class="sxs-lookup"><span data-stu-id="512e7-178">You can combine this property with **partitionBy** toohave folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="512e7-179">Igen</span><span class="sxs-lookup"><span data-stu-id="512e7-179">Yes</span></span> |
| <span data-ttu-id="512e7-180">fileName</span><span class="sxs-lookup"><span data-stu-id="512e7-180">fileName</span></span> |<span data-ttu-id="512e7-181">Meg kell adnia hello fájl hello nevet hello **folderPath** Ha azt szeretné, hogy hello tábla toorefer tooa adott fájl hello mappában.</span><span class="sxs-lookup"><span data-stu-id="512e7-181">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="512e7-182">Ha nem ad meg semmilyen értéket ehhez a tulajdonsághoz, hello tábla mutat tooall fájlok hello mappában.</span><span class="sxs-lookup"><span data-stu-id="512e7-182">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="512e7-183">Ha nincs megadva fájlnév egy kimeneti adatkészletet, hello hello létrehozott fájl neve a következő formátumban hello lenne:</span><span class="sxs-lookup"><span data-stu-id="512e7-183">When fileName is not specified for an output dataset, hello name of hello generated file would be in hello following this format:</span></span> <br/><br/><span data-ttu-id="512e7-184">Adatok. <Guid>.txt (például:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="512e7-184">Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="512e7-185">Nem</span><span class="sxs-lookup"><span data-stu-id="512e7-185">No</span></span> |
| <span data-ttu-id="512e7-186">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="512e7-186">partitionedBy</span></span> |<span data-ttu-id="512e7-187">partitionedBy lehet használt toospecify egy dinamikus folderPath időadatok adatsorozat nevét.</span><span class="sxs-lookup"><span data-stu-id="512e7-187">partitionedBy can be used toospecify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="512e7-188">Példa: folderPath adatok óránkénti paraméteres.</span><span class="sxs-lookup"><span data-stu-id="512e7-188">Example: folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="512e7-189">Nem</span><span class="sxs-lookup"><span data-stu-id="512e7-189">No</span></span> |
| <span data-ttu-id="512e7-190">Formátumban</span><span class="sxs-lookup"><span data-stu-id="512e7-190">format</span></span> | <span data-ttu-id="512e7-191">a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="512e7-191">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="512e7-192">Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="512e7-192">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="512e7-193">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="512e7-193">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="512e7-194">Ha azt szeretné, túl**másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="512e7-194">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="512e7-195">Nem</span><span class="sxs-lookup"><span data-stu-id="512e7-195">No</span></span> |
| <span data-ttu-id="512e7-196">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="512e7-196">compression</span></span> | <span data-ttu-id="512e7-197">Adja meg a hello típusát és hello adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="512e7-197">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="512e7-198">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="512e7-198">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="512e7-199">Támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="512e7-199">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="512e7-200">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="512e7-200">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="512e7-201">Nem</span><span class="sxs-lookup"><span data-stu-id="512e7-201">No</span></span> |

> [!NOTE]
> <span data-ttu-id="512e7-202">fájlnév és fileFilter nem használható egyszerre.</span><span class="sxs-lookup"><span data-stu-id="512e7-202">filename and fileFilter cannot be used simultaneously.</span></span>

### <a name="using-partionedby-property"></a><span data-ttu-id="512e7-203">PartionedBy tulajdonság használatával</span><span class="sxs-lookup"><span data-stu-id="512e7-203">Using partionedBy property</span></span>
<span data-ttu-id="512e7-204">Hello előző szakaszban említett, megadhatja a dinamikus folderPath és a sorozat időadatok fájlnevét hello **partitionedBy** tulajdonság, [adat-előállító funkciók és hello rendszerváltozók](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="512e7-204">As mentioned in hello previous section, you can specify a dynamic folderPath and filename for time series data with hello **partitionedBy** property, [Data Factory functions, and hello system variables](data-factory-functions-variables.md).</span></span>

<span data-ttu-id="512e7-205">toolearn bővebben idő adatsorozat adatkészleteket, az ütemezés és a szeletek, lásd: [létrehozása adatkészletek](data-factory-create-datasets.md), [ütemezés & végrehajtási](data-factory-scheduling-and-execution.md), és [létrehozása folyamatok](data-factory-create-pipelines.md) cikkeket.</span><span class="sxs-lookup"><span data-stu-id="512e7-205">toolearn more about time series datasets, scheduling, and slices, see [Creating Datasets](data-factory-create-datasets.md), [Scheduling & Execution](data-factory-scheduling-and-execution.md), and [Creating Pipelines](data-factory-create-pipelines.md) articles.</span></span>

#### <a name="sample-1"></a><span data-ttu-id="512e7-206">1. példa:</span><span class="sxs-lookup"><span data-stu-id="512e7-206">Sample 1:</span></span>

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
<span data-ttu-id="512e7-207">Ebben a példában {szelet} hello adat-előállító rendszer változó SliceStart hello formátumban (YYYYMMDDHH) megadott érték helyére.</span><span class="sxs-lookup"><span data-stu-id="512e7-207">In this example {Slice} is replaced with hello value of Data Factory system variable SliceStart in hello format (YYYYMMDDHH) specified.</span></span> <span data-ttu-id="512e7-208">hello SliceStart toostart idő hello szelet hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="512e7-208">hello SliceStart refers toostart time of hello slice.</span></span> <span data-ttu-id="512e7-209">hello folderPath nem azonos az egyes szeletek.</span><span class="sxs-lookup"><span data-stu-id="512e7-209">hello folderPath is different for each slice.</span></span> <span data-ttu-id="512e7-210">Például: wikidatagateway/wikisampledataout/2014100103 vagy wikidatagateway/wikisampledataout/2014100104.</span><span class="sxs-lookup"><span data-stu-id="512e7-210">For example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.</span></span>

#### <a name="sample-2"></a><span data-ttu-id="512e7-211">2. példa:</span><span class="sxs-lookup"><span data-stu-id="512e7-211">Sample 2:</span></span>

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
<span data-ttu-id="512e7-212">Ebben a példában év, hónap, nap és SliceStart idején ki kell olvasni a külön változókat, amelyek folderPath és a fájlnév tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="512e7-212">In this example, year, month, day, and time of SliceStart are extracted into separate variables that are used by folderPath and fileName properties.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="512e7-213">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="512e7-213">Copy activity properties</span></span>
<span data-ttu-id="512e7-214">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="512e7-214">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="512e7-215">Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti táblák és házirendek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="512e7-215">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="512e7-216">Mivel a hello hello tevékenység részében typeProperties rendelkezésre álló tulajdonságok tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="512e7-216">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="512e7-217">A másolási tevékenység során két érték források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="512e7-217">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="512e7-218">A másolási tevékenység, ha a forrás típusa nem **FileSystemSource** typeProperties szakaszában érhetők hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="512e7-218">For Copy Activity, when source is of type **FileSystemSource** hello following properties are available in typeProperties section:</span></span>

<span data-ttu-id="512e7-219">**FileSystemSource** következő tulajdonságai hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="512e7-219">**FileSystemSource** supports hello following properties:</span></span>

| <span data-ttu-id="512e7-220">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="512e7-220">Property</span></span> | <span data-ttu-id="512e7-221">Leírás</span><span class="sxs-lookup"><span data-stu-id="512e7-221">Description</span></span> | <span data-ttu-id="512e7-222">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="512e7-222">Allowed values</span></span> | <span data-ttu-id="512e7-223">Szükséges</span><span class="sxs-lookup"><span data-stu-id="512e7-223">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="512e7-224">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="512e7-224">recursive</span></span> |<span data-ttu-id="512e7-225">Azt jelzi, hogy hello adatolvasás rekurzív módon hello almappák vagy csak a megadott mappa hello.</span><span class="sxs-lookup"><span data-stu-id="512e7-225">Indicates whether hello data is read recursively from hello sub folders or only from hello specified folder.</span></span> |<span data-ttu-id="512e7-226">IGAZ, hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="512e7-226">True, False (default)</span></span> |<span data-ttu-id="512e7-227">Nem</span><span class="sxs-lookup"><span data-stu-id="512e7-227">No</span></span> |

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="512e7-228">Támogatott formátumú és tömörítés</span><span class="sxs-lookup"><span data-stu-id="512e7-228">Supported file and compression formats</span></span>
<span data-ttu-id="512e7-229">Lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md) cikk részletei.</span><span class="sxs-lookup"><span data-stu-id="512e7-229">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-example-copy-data-from-on-premises-hdfs-tooazure-blob"></a><span data-ttu-id="512e7-230">JSON-példa: adatok másolása a helyszíni HDFS tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="512e7-230">JSON example: Copy data from on-premises HDFS tooAzure Blob</span></span>
<span data-ttu-id="512e7-231">Ez a példa bemutatja, hogyan egy helyszíni HDFS tooAzure Blob Storage toocopy adatait.</span><span class="sxs-lookup"><span data-stu-id="512e7-231">This sample shows how toocopy data from an on-premises HDFS tooAzure Blob Storage.</span></span> <span data-ttu-id="512e7-232">Azonban az adatok átmásolhatók **közvetlenül** közölt hello nyelő tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.</span><span class="sxs-lookup"><span data-stu-id="512e7-232">However, data can be copied **directly** tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>  

<span data-ttu-id="512e7-233">hello minta biztosít a következő adat-előállító entitások hello JSON jelentésdefiníciókat.</span><span class="sxs-lookup"><span data-stu-id="512e7-233">hello sample provides JSON definitions for hello following Data Factory entities.</span></span> <span data-ttu-id="512e7-234">A definíciók toocreate egy folyamat toocopy adatokat HDFS tooAzure Blob Storage segítségével is használhatók [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="512e7-234">You can use these definitions toocreate a pipeline toocopy data from HDFS tooAzure Blob Storage by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span>

1. <span data-ttu-id="512e7-235">A társított szolgáltatás típusa [OnPremisesHdfs](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="512e7-235">A linked service of type [OnPremisesHdfs](#linked-service-properties).</span></span>
2. <span data-ttu-id="512e7-236">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="512e7-236">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="512e7-237">Bemeneti [dataset](data-factory-create-datasets.md) típusú [fájlmegosztási](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="512e7-237">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties).</span></span>
4. <span data-ttu-id="512e7-238">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="512e7-238">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="512e7-239">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [FileSystemSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="512e7-239">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="512e7-240">hello minta másol adatokat egy helyszíni HDFS tooan Azure blob minden órában.</span><span class="sxs-lookup"><span data-stu-id="512e7-240">hello sample copies data from an on-premises HDFS tooan Azure blob every hour.</span></span> <span data-ttu-id="512e7-241">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="512e7-241">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="512e7-242">Első lépésként hello az adatkezelési átjáró beállítása.</span><span class="sxs-lookup"><span data-stu-id="512e7-242">As a first step, set up hello data management gateway.</span></span> <span data-ttu-id="512e7-243">hello utasításait hello [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="512e7-243">hello instructions in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="512e7-244">**HDFS társított szolgáltatás:** ebben a példában használt hello Windows-hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="512e7-244">**HDFS linked service:** This example uses hello Windows authentication.</span></span> <span data-ttu-id="512e7-245">Lásd: [HDFS társított szolgáltatás](#linked-service-properties) szakasz a különböző típusú hitelesítés használható.</span><span class="sxs-lookup"><span data-stu-id="512e7-245">See [HDFS linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

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

<span data-ttu-id="512e7-246">**Az Azure tárolás társított szolgáltatásának:**</span><span class="sxs-lookup"><span data-stu-id="512e7-246">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="512e7-247">**HDFS bemeneti adatkészletéből:** Ez az adatkészlet hivatkozik toohello HDFS mappa DataTransfer/UnitTest /.</span><span class="sxs-lookup"><span data-stu-id="512e7-247">**HDFS input dataset:** This dataset refers toohello HDFS folder DataTransfer/UnitTest/.</span></span> <span data-ttu-id="512e7-248">hello folyamat összes hello fájlt másolja a következő mappa toohello célra.</span><span class="sxs-lookup"><span data-stu-id="512e7-248">hello pipeline copies all hello files in this folder toohello destination.</span></span>

<span data-ttu-id="512e7-249">"External" beállítása: "true" tájékoztatja hello Data Factory szolgáltatásnak, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="512e7-249">Setting “external”: ”true” informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="512e7-250">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="512e7-250">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="512e7-251">Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="512e7-251">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="512e7-252">hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="512e7-252">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="512e7-253">hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.</span><span class="sxs-lookup"><span data-stu-id="512e7-253">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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

<span data-ttu-id="512e7-254">**A másolási tevékenység során a fájlrendszer és a Blob fogadó folyamat:**</span><span class="sxs-lookup"><span data-stu-id="512e7-254">**A copy activity in a pipeline with File System source and Blob sink:**</span></span>

<span data-ttu-id="512e7-255">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse ezen bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="512e7-255">hello pipeline contains a Copy Activity that is configured toouse these input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="512e7-256">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**FileSystemSource** és **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="512e7-256">In hello pipeline JSON definition, hello **source** type is set too**FileSystemSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="512e7-257">hello SQL-lekérdezésben megadott hello **lekérdezés** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.</span><span class="sxs-lookup"><span data-stu-id="512e7-257">hello SQL query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

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

## <a name="use-kerberos-authentication-for-hdfs-connector"></a><span data-ttu-id="512e7-258">A HDFS-összekötő Kerberos-hitelesítés használata</span><span class="sxs-lookup"><span data-stu-id="512e7-258">Use Kerberos authentication for HDFS connector</span></span>
<span data-ttu-id="512e7-259">Nincsenek a HDFS-összekötő Kerberos-hitelesítés toouse, így a két beállítások tooset hello a helyszíni környezet létrehozása.</span><span class="sxs-lookup"><span data-stu-id="512e7-259">There are two options tooset up hello on-premises environment so as toouse Kerberos Authentication in HDFS connector.</span></span> <span data-ttu-id="512e7-260">Választhat egy hello legjobban az esethez.</span><span class="sxs-lookup"><span data-stu-id="512e7-260">You can choose hello one better fits your case.</span></span>
* <span data-ttu-id="512e7-261">1. lehetőség: [illesztési átjárót működtető gépen, a Kerberos-tartomány](#kerberos-join-realm)</span><span class="sxs-lookup"><span data-stu-id="512e7-261">Option 1: [Join gateway machine in Kerberos realm](#kerberos-join-realm)</span></span>
* <span data-ttu-id="512e7-262">2. lehetőség: [kölcsönös, a Windows-tartomány és a Kerberos-tartomány közötti megbízhatósági kapcsolat engedélyezése](#kerberos-mutual-trust)</span><span class="sxs-lookup"><span data-stu-id="512e7-262">Option 2: [Enable mutual trust between Windows domain and Kerberos realm](#kerberos-mutual-trust)</span></span>

### <span data-ttu-id="512e7-263"><a name="kerberos-join-realm"></a>1. lehetőség: Illesztés átjárót működtető gépen, Kerberos-tartomány</span><span class="sxs-lookup"><span data-stu-id="512e7-263"><a name="kerberos-join-realm"></a>Option 1: Join gateway machine in Kerberos realm</span></span>

#### <a name="requirement"></a><span data-ttu-id="512e7-264">Követelmény:</span><span class="sxs-lookup"><span data-stu-id="512e7-264">Requirement:</span></span>

* <span data-ttu-id="512e7-265">hello átjárót futtató gép toojoin hello Kerberos-tartomány van szüksége, és nem tud csatlakozni a Windows-tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="512e7-265">hello gateway machine needs toojoin hello Kerberos realm and can’t join any Windows domain.</span></span>

#### <a name="how-tooconfigure"></a><span data-ttu-id="512e7-266">Hogyan tooconfigure:</span><span class="sxs-lookup"><span data-stu-id="512e7-266">How tooconfigure:</span></span>

<span data-ttu-id="512e7-267">**Az átjáró számítógépén:**</span><span class="sxs-lookup"><span data-stu-id="512e7-267">**On gateway machine:**</span></span>

1.  <span data-ttu-id="512e7-268">Futtassa a hello **Ksetup** segédprogram tooconfigure hello Kerberos KDC-kiszolgáló és a tartomány.</span><span class="sxs-lookup"><span data-stu-id="512e7-268">Run hello **Ksetup** utility tooconfigure hello Kerberos KDC server and realm.</span></span>

    <span data-ttu-id="512e7-269">hello gép be kell állítani egy munkacsoport tagjaként óta egy Kerberos-tartomány eltér a Windows-tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="512e7-269">hello machine must be configured as a member of a workgroup since a Kerberos realm is different from a Windows domain.</span></span> <span data-ttu-id="512e7-270">Ez megvalósítható hello Kerberos-tartomány és a KDC-kiszolgáló hozzáadása az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="512e7-270">This can be achieved by setting hello Kerberos realm and adding a KDC server as follows.</span></span> <span data-ttu-id="512e7-271">Cserélje le *REALM.COM* a saját megfelelő terület szükséges.</span><span class="sxs-lookup"><span data-stu-id="512e7-271">Replace *REALM.COM* with your own respective realm as needed.</span></span>

            C:> Ksetup /setdomain REALM.COM
            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>

    <span data-ttu-id="512e7-272">**Indítsa újra a** hello gép 2 parancsok végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="512e7-272">**Restart** hello machine after executing these 2 commands.</span></span>

2.  <span data-ttu-id="512e7-273">Ellenőrizze a hello-konfiguráció **Ksetup** parancsot.</span><span class="sxs-lookup"><span data-stu-id="512e7-273">Verify hello configuration with **Ksetup** command.</span></span> <span data-ttu-id="512e7-274">hello kimeneti kell lennie, mint:</span><span class="sxs-lookup"><span data-stu-id="512e7-274">hello output should be like:</span></span>

            C:> Ksetup
            default realm = REALM.COM (external)
            REALM.com:
                kdc = <your_kdc_server_address>

<span data-ttu-id="512e7-275">**Az Azure Data Factoryben:**</span><span class="sxs-lookup"><span data-stu-id="512e7-275">**In Azure Data Factory:**</span></span>

* <span data-ttu-id="512e7-276">Konfigurálja a hello HDFS-összekötő használatával **Windows-hitelesítés** a Kerberos egyszerű neve és jelszava tooconnect toohello HDFS adatforrás együtt.</span><span class="sxs-lookup"><span data-stu-id="512e7-276">Configure hello HDFS connector using **Windows authentication** together with your Kerberos principal name and password tooconnect toohello HDFS data source.</span></span> <span data-ttu-id="512e7-277">Ellenőrizze [HDFS társított szolgáltatás Tulajdonságok](#linked-service-properties) konfiguráció részletei című szakaszban.</span><span class="sxs-lookup"><span data-stu-id="512e7-277">Check [HDFS Linked Service properties](#linked-service-properties) section on configuration details.</span></span>

### <span data-ttu-id="512e7-278"><a name="kerberos-mutual-trust"></a>2. lehetőség: A Windows-tartomány és a Kerberos-tartomány közötti kölcsönös megbízhatósági engedélyezése</span><span class="sxs-lookup"><span data-stu-id="512e7-278"><a name="kerberos-mutual-trust"></a>Option 2: Enable mutual trust between Windows domain and Kerberos realm</span></span>

#### <a name="requirement"></a><span data-ttu-id="512e7-279">Követelmény:</span><span class="sxs-lookup"><span data-stu-id="512e7-279">Requirement:</span></span>
*   <span data-ttu-id="512e7-280">hello átjárót működtető gépen Windows-tartományhoz kell csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="512e7-280">hello gateway machine must join a Windows domain.</span></span>
*   <span data-ttu-id="512e7-281">Engedély tooupdate hello tartományvezérlő-beállítások van szüksége.</span><span class="sxs-lookup"><span data-stu-id="512e7-281">You need permission tooupdate hello domain controller's settings.</span></span>

#### <a name="how-tooconfigure"></a><span data-ttu-id="512e7-282">Hogyan tooconfigure:</span><span class="sxs-lookup"><span data-stu-id="512e7-282">How tooconfigure:</span></span>

> [!NOTE]
> <span data-ttu-id="512e7-283">Igény szerint a saját megfelelő tartomány és a tartományvezérlő oktatóanyag következő hello REALM.COM és AD.COM cserélni.</span><span class="sxs-lookup"><span data-stu-id="512e7-283">Replace REALM.COM and AD.COM in hello following tutorial with your own respective realm and domain controller as needed.</span></span>

<span data-ttu-id="512e7-284">**KDC-kiszolgálón:**</span><span class="sxs-lookup"><span data-stu-id="512e7-284">**On KDC server:**</span></span>

1.  <span data-ttu-id="512e7-285">Hello KDC konfigurációjának szerkesztése **krb5.conf** fájl toolet KDC megbízható hivatkozik a következő konfigurációs sablon toohello Windows-tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="512e7-285">Edit hello KDC configuration in **krb5.conf** file toolet KDC trust Windows Domain referring toohello following configuration template.</span></span> <span data-ttu-id="512e7-286">Alapértelmezés szerint hello konfigurációs itt található: **/etc/krb5.conf**.</span><span class="sxs-lookup"><span data-stu-id="512e7-286">By default, hello configuration is located at **/etc/krb5.conf**.</span></span>

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

  <span data-ttu-id="512e7-287">**Indítsa újra a** KDC-szolgáltatás hello konfigurálása után.</span><span class="sxs-lookup"><span data-stu-id="512e7-287">**Restart** hello KDC service after configuration.</span></span>

2.  <span data-ttu-id="512e7-288">Készítse elő a rendszerbiztonsági tag nevű  **krbtgt/REALM.COM@AD.COM**  a KDC kiszolgáló hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="512e7-288">Prepare a principal named **krbtgt/REALM.COM@AD.COM** in KDC server with hello following command:</span></span>

            Kadmin> addprinc krbtgt/REALM.COM@AD.COM

3.  <span data-ttu-id="512e7-289">A **hadoop.security.auth_to_local** HDFS-szolgáltatás konfigurációs fájlt, adja hozzá `RULE:[1:$1@$0](.*@AD.COM)s/@.*//`.</span><span class="sxs-lookup"><span data-stu-id="512e7-289">In **hadoop.security.auth_to_local** HDFS service configuration file, add `RULE:[1:$1@$0](.*@AD.COM)s/@.*//`.</span></span>

<span data-ttu-id="512e7-290">**A tartományvezérlőn:**</span><span class="sxs-lookup"><span data-stu-id="512e7-290">**On domain controller:**</span></span>

1.  <span data-ttu-id="512e7-291">Futtassa a következő hello **Ksetup** tooadd egy tartomány bejegyzés parancsokat:</span><span class="sxs-lookup"><span data-stu-id="512e7-291">Run hello following **Ksetup** commands tooadd a realm entry:</span></span>

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

2.  <span data-ttu-id="512e7-292">A Windows-tartomány tooKerberos tartomány megbízhatósági kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="512e7-292">Establish trust from Windows Domain tooKerberos Realm.</span></span> <span data-ttu-id="512e7-293">[jelszava] hello egyszerű hello jelszavát  **krbtgt/REALM.COM@AD.COM** .</span><span class="sxs-lookup"><span data-stu-id="512e7-293">[password] is hello password for hello principal **krbtgt/REALM.COM@AD.COM**.</span></span>

            C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /passwordt:[password]

3.  <span data-ttu-id="512e7-294">Válassza ki a Kerberos használt titkosítási algoritmus.</span><span class="sxs-lookup"><span data-stu-id="512e7-294">Select encryption algorithm used in Kerberos.</span></span>

    1. <span data-ttu-id="512e7-295">Nyissa meg tooServer Manager > csoportházirend-kezelés > tartomány > csoportházirend-objektumok > alapértelmezett vagy az aktív tartományi házirend és a Szerkesztés.</span><span class="sxs-lookup"><span data-stu-id="512e7-295">Go tooServer Manager > Group Policy Management > Domain > Group Policy Objects > Default or Active Domain Policy, and Edit.</span></span>

    2. <span data-ttu-id="512e7-296">A hello **Csoportházirendkezelés-szerkesztő** előugró ablakban, keresse tooComputer konfigurációs > házirendek > Windows-beállítások > biztonsági beállítások > helyi házirend > biztonsági beállítások, és konfigurálja **hálózati biztonsági: konfigurálja a Kerberos engedélyezett titkosítási típusok**.</span><span class="sxs-lookup"><span data-stu-id="512e7-296">In hello **Group Policy Management Editor** popup window, go tooComputer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options, and configure **Network security: Configure Encryption types allowed for Kerberos**.</span></span>

    3. <span data-ttu-id="512e7-297">Jelölje be hello titkosítási algoritmus toouse kívánt tooKDC amikor kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="512e7-297">Select hello encryption algorithm you want toouse when connect tooKDC.</span></span> <span data-ttu-id="512e7-298">Gyakran, egyszerűen minden hello lehetőségek közül választhat.</span><span class="sxs-lookup"><span data-stu-id="512e7-298">Commonly, you can simply select all hello options.</span></span>

        ![A Kerberos config titkosítási típusok](media/data-factory-hdfs-connector/config-encryption-types-for-kerberos.png)

    4. <span data-ttu-id="512e7-300">Használjon **Ksetup** parancs toospecify hello titkosítási algoritmus toobe hello használt adott tartomány.</span><span class="sxs-lookup"><span data-stu-id="512e7-300">Use **Ksetup** command toospecify hello encryption algorithm toobe used on hello specific REALM.</span></span>

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  <span data-ttu-id="512e7-301">Hello leképezés közötti hello tartományi fiók és a Kerberos egyszerű, a rendelés toouse Kerberos egyszerű a Windows-tartomány létrehozása.</span><span class="sxs-lookup"><span data-stu-id="512e7-301">Create hello mapping between hello domain account and Kerberos principal, in order toouse Kerberos principal in Windows Domain.</span></span>

    1. <span data-ttu-id="512e7-302">Indítsa el a felügyeleti eszközök hello > **Active Directory – felhasználók és számítógépek**.</span><span class="sxs-lookup"><span data-stu-id="512e7-302">Start hello Administrative tools > **Active Directory Users and Computers**.</span></span>

    2. <span data-ttu-id="512e7-303">Kattintson a speciális szolgáltatások konfigurálása **nézet** > **speciális funkciók**.</span><span class="sxs-lookup"><span data-stu-id="512e7-303">Configure advanced features by clicking **View** > **Advanced Features**.</span></span>

    3. <span data-ttu-id="512e7-304">Keresse meg a hello fiók toowhich szeretné, hogy toocreate hozzárendeléseket, és kattintson a jobb gombbal a tooview **a felhasználónév-leképezések** > kattintson **Kerberos-nevek** fülre.</span><span class="sxs-lookup"><span data-stu-id="512e7-304">Locate hello account toowhich you want toocreate mappings, and right-click tooview **Name Mappings** > click **Kerberos Names** tab.</span></span>

    4. <span data-ttu-id="512e7-305">Adjon hozzá egy egyszerű hello tartomány.</span><span class="sxs-lookup"><span data-stu-id="512e7-305">Add a principal from hello realm.</span></span>

        ![Térkép biztonsági azonosító](media/data-factory-hdfs-connector/map-security-identity.png)

<span data-ttu-id="512e7-307">**Az átjáró számítógépén:**</span><span class="sxs-lookup"><span data-stu-id="512e7-307">**On gateway machine:**</span></span>

* <span data-ttu-id="512e7-308">Futtassa a következő hello **Ksetup** tooadd egy tartomány bejegyzést a parancsokat.</span><span class="sxs-lookup"><span data-stu-id="512e7-308">Run hello following **Ksetup** commands tooadd a realm entry.</span></span>

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

<span data-ttu-id="512e7-309">**Az Azure Data Factoryben:**</span><span class="sxs-lookup"><span data-stu-id="512e7-309">**In Azure Data Factory:**</span></span>

* <span data-ttu-id="512e7-310">Konfigurálja a hello HDFS-összekötő használatával **Windows-hitelesítés** tartományi fiók vagy a Kerberos egyszerű tooconnect toohello HDFS adatforrás együtt.</span><span class="sxs-lookup"><span data-stu-id="512e7-310">Configure hello HDFS connector using **Windows authentication** together with either your Domain Account or Kerberos Principal tooconnect toohello HDFS data source.</span></span> <span data-ttu-id="512e7-311">Ellenőrizze [HDFS társított szolgáltatás Tulajdonságok](#linked-service-properties) konfiguráció részletei című szakaszban.</span><span class="sxs-lookup"><span data-stu-id="512e7-311">Check [HDFS Linked Service properties](#linked-service-properties) section on configuration details.</span></span>

> [!NOTE]
> <span data-ttu-id="512e7-312">Tekintse meg a forrás adatkészlet toocolumns fogadó adatkészletből toomap oszlopokat [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="512e7-312">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>


## <a name="performance-and-tuning"></a><span data-ttu-id="512e7-313">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="512e7-313">Performance and Tuning</span></span>
<span data-ttu-id="512e7-314">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.</span><span class="sxs-lookup"><span data-stu-id="512e7-314">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
