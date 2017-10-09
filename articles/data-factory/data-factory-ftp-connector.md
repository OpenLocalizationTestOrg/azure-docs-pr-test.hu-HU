---
title: "Azure Data Factory használatával az FTP-kiszolgáló adatait aaaMove |} Microsoft Docs"
description: "Megtudhatja, hogyan toomove adatokat az Azure Data Factory használatával FTP-kiszolgálóhoz."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: eea3bab0-a6e4-4045-ad44-9ce06229c718
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: jingwang
ms.openlocfilehash: c707e29532b2a8a870603948cb6150ab857bd6ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-ftp-server-by-using-azure-data-factory"></a><span data-ttu-id="8717f-103">Adatok áthelyezése az FTP-kiszolgáló Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="8717f-103">Move data from an FTP server by using Azure Data Factory</span></span>
<span data-ttu-id="8717f-104">Ez a cikk azt ismerteti, hogyan toouse hello másolási tevékenység az Azure Data Factory toomove adatok az FTP-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="8717f-104">This article explains how toouse hello copy activity in Azure Data Factory toomove data from an FTP server.</span></span> <span data-ttu-id="8717f-105">-Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="8717f-105">It builds on hello [Data movement activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="8717f-106">Az FTP kiszolgáló támogatott tooany fogadó adattár adatainak másolhatja.</span><span class="sxs-lookup"><span data-stu-id="8717f-106">You can copy data from an FTP server tooany supported sink data store.</span></span> <span data-ttu-id="8717f-107">Az adatok támogatott tárolja, a fogadók esetében hello másolási tevékenység, lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="8717f-107">For a list of data stores supported as sinks by hello copy activity, see hello [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="8717f-108">Adat-előállító jelenleg csak egy FTP-kiszolgáló tooother adatok áthelyezése adatait tárolja, de nem az adatok áthelyezése más adatok tooan FTP-kiszolgáló tárolja.</span><span class="sxs-lookup"><span data-stu-id="8717f-108">Data Factory currently supports only moving data from an FTP server tooother data stores, but not moving data from other data stores tooan FTP server.</span></span> <span data-ttu-id="8717f-109">Az támogatja-e mind a helyszíni és felhőalapú FTP-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="8717f-109">It supports both on-premises and cloud FTP servers.</span></span>

> [!NOTE]
> <span data-ttu-id="8717f-110">hello másolási tevékenység nem hello forrásfájl törlése, miután sikeresen másolt toohello cél.</span><span class="sxs-lookup"><span data-stu-id="8717f-110">hello copy activity does not delete hello source file after it is successfully copied toohello destination.</span></span> <span data-ttu-id="8717f-111">Ha egy sikeres másolása után toodelete hello forrásfájl van szüksége, hozzon létre egy egyéni tevékenység toodelete hello fájlt, és hello tevékenység hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="8717f-111">If you need toodelete hello source file after a successful copy, create a custom activity toodelete hello file, and use hello activity in hello pipeline.</span></span> 

## <a name="enable-connectivity"></a><span data-ttu-id="8717f-112">Kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="8717f-112">Enable connectivity</span></span>
<span data-ttu-id="8717f-113">Ha áthelyezi adatait egy **helyszíni** FTP server tooa felhőalapú adattároló (már például tooAzure Blob Storage tárolóban van hátra), telepítéséhez, és az adatkezelési átjáró használatához.</span><span class="sxs-lookup"><span data-stu-id="8717f-113">If you are moving data from an **on-premises** FTP server tooa cloud data store (for example, tooAzure Blob storage), install and use Data Management Gateway.</span></span> <span data-ttu-id="8717f-114">Az adatkezelési átjáró hello olyan ügyfélügynök, amelynek a helyszíni gépre van telepítve, és lehetővé teszi a felhőalapú szolgáltatások tooconnect tooan helyszíni erőforrás.</span><span class="sxs-lookup"><span data-stu-id="8717f-114">hello Data Management Gateway is a client agent that is installed on your on-premises machine, and it allows cloud services tooconnect tooan on-premises resource.</span></span> <span data-ttu-id="8717f-115">További információkért lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="8717f-115">For details, see [Data Management Gateway](data-factory-data-management-gateway.md).</span></span> <span data-ttu-id="8717f-116">Hello átjáró beállításával és használatával, akkor a részletes útmutatót lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="8717f-116">For step-by-step instructions on setting up hello gateway and using it, see [Moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md).</span></span> <span data-ttu-id="8717f-117">Akkor is, ha hello server az Azure-infrastruktúra (IaaS) szolgáltatás virtuális gépként (VM) hello átjáró tooconnect tooan FTP-kiszolgáló használ.</span><span class="sxs-lookup"><span data-stu-id="8717f-117">You use hello gateway tooconnect tooan FTP server, even if hello server is on an Azure infrastructure as a service (IaaS) virtual machine (VM).</span></span>

<span data-ttu-id="8717f-118">Már lehetséges tooinstall hello átjáró hello megegyezik a helyszíni gépen vagy infrastruktúra-szolgáltatási virtuális gép hello FTP-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="8717f-118">It is possible tooinstall hello gateway on hello same on-premises machine or IaaS VM as hello FTP server.</span></span> <span data-ttu-id="8717f-119">Azt javasoljuk azonban hello átjáró telepítése egy másik számítógépre, vagy az infrastruktúra-szolgáltatási virtuális gép tooavoid Erőforrásverseny, és a jobb teljesítmény érdekében.</span><span class="sxs-lookup"><span data-stu-id="8717f-119">However, we recommend that you install hello gateway on a separate machine or IaaS VM tooavoid resource contention, and for better performance.</span></span> <span data-ttu-id="8717f-120">Hello átjáró egy külön számítógépen való telepítésekor hello gépnek képes tooaccess hello FTP-kiszolgálón kell lennie.</span><span class="sxs-lookup"><span data-stu-id="8717f-120">When you install hello gateway on a separate machine, hello machine should be able tooaccess hello FTP server.</span></span>

## <a name="get-started"></a><span data-ttu-id="8717f-121">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="8717f-121">Get started</span></span>
<span data-ttu-id="8717f-122">A másolási tevékenység, amely FTP forrásból származó adatokat a különböző eszközök vagy API-k használatával helyezi át a feldolgozási sor hozhatja létre.</span><span class="sxs-lookup"><span data-stu-id="8717f-122">You can create a pipeline with a copy activity that moves data from an FTP source by using different tools or APIs.</span></span>

<span data-ttu-id="8717f-123">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **Data Factory másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="8717f-123">hello easiest way toocreate a pipeline is toouse hello **Data Factory Copy Wizard**.</span></span> <span data-ttu-id="8717f-124">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="8717f-124">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough.</span></span>

<span data-ttu-id="8717f-125">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="8717f-125">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="8717f-126">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="8717f-126">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span>

<span data-ttu-id="8717f-127">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="8717f-127">Whether you use hello tools or APIs, perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="8717f-128">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="8717f-128">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="8717f-129">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="8717f-129">Create **datasets** toorepresent input and output data for hello copy operation.</span></span>
3. <span data-ttu-id="8717f-130">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="8717f-130">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span>

<span data-ttu-id="8717f-131">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="8717f-131">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="8717f-132">Eszközök vagy API-k (kivéve a .NET API-t) használ, akkor határozzák meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="8717f-132">When you use tools or APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span> <span data-ttu-id="8717f-133">Az adat-előállító entitások, amelyek egy FTP-adattároló használt toocopy adatait JSON-definíciók minta, lásd: hello [JSON-példa: adatok másolása az FTP-kiszolgáló tooAzure blobból](#json-example-copy-data-from-ftp-server-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="8717f-133">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an FTP data store, see hello [JSON example: Copy data from FTP server tooAzure blob](#json-example-copy-data-from-ftp-server-to-azure-blob) section of this article.</span></span>

> [!NOTE]
> <span data-ttu-id="8717f-134">Fájl- és tömörítési formátum támogatott toouse kapcsolatos részletekért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md).</span><span class="sxs-lookup"><span data-stu-id="8717f-134">For details about supported file and compression formats toouse, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md).</span></span>

<span data-ttu-id="8717f-135">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooFTP részleteit tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="8717f-135">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooFTP.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="8717f-136">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="8717f-136">Linked service properties</span></span>
<span data-ttu-id="8717f-137">hello a következő táblázat ismerteti a JSON elemek adott tooan kapcsolódó FTP-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8717f-137">hello following table describes JSON elements specific tooan FTP linked service.</span></span>

| <span data-ttu-id="8717f-138">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="8717f-138">Property</span></span> | <span data-ttu-id="8717f-139">Leírás</span><span class="sxs-lookup"><span data-stu-id="8717f-139">Description</span></span> | <span data-ttu-id="8717f-140">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8717f-140">Required</span></span> | <span data-ttu-id="8717f-141">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="8717f-141">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="8717f-142">type</span><span class="sxs-lookup"><span data-stu-id="8717f-142">type</span></span> |<span data-ttu-id="8717f-143">Állítsa be a tooFtpServer.</span><span class="sxs-lookup"><span data-stu-id="8717f-143">Set this tooFtpServer.</span></span> |<span data-ttu-id="8717f-144">Igen</span><span class="sxs-lookup"><span data-stu-id="8717f-144">Yes</span></span> |&nbsp; |
| <span data-ttu-id="8717f-145">állomás</span><span class="sxs-lookup"><span data-stu-id="8717f-145">host</span></span> |<span data-ttu-id="8717f-146">Adja meg a hello nevét vagy IP-cím hello FTP-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="8717f-146">Specify hello name or IP address of hello FTP server.</span></span> |<span data-ttu-id="8717f-147">Igen</span><span class="sxs-lookup"><span data-stu-id="8717f-147">Yes</span></span> |&nbsp; |
| <span data-ttu-id="8717f-148">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="8717f-148">authenticationType</span></span> |<span data-ttu-id="8717f-149">Adja meg a hello hitelesítési típus.</span><span class="sxs-lookup"><span data-stu-id="8717f-149">Specify hello authentication type.</span></span> |<span data-ttu-id="8717f-150">Igen</span><span class="sxs-lookup"><span data-stu-id="8717f-150">Yes</span></span> |<span data-ttu-id="8717f-151">Alapszintű, a névtelen</span><span class="sxs-lookup"><span data-stu-id="8717f-151">Basic, Anonymous</span></span> |
| <span data-ttu-id="8717f-152">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="8717f-152">username</span></span> |<span data-ttu-id="8717f-153">Adja meg a hello felhasználó, aki rendelkezik hozzáférési toohello FTP-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="8717f-153">Specify hello user who has access toohello FTP server.</span></span> |<span data-ttu-id="8717f-154">Nem</span><span class="sxs-lookup"><span data-stu-id="8717f-154">No</span></span> |&nbsp; |
| <span data-ttu-id="8717f-155">jelszó</span><span class="sxs-lookup"><span data-stu-id="8717f-155">password</span></span> |<span data-ttu-id="8717f-156">Adja meg (felhasználónév) hello felhasználó hello jelszavát.</span><span class="sxs-lookup"><span data-stu-id="8717f-156">Specify hello password for hello user (username).</span></span> |<span data-ttu-id="8717f-157">Nem</span><span class="sxs-lookup"><span data-stu-id="8717f-157">No</span></span> |&nbsp; |
| <span data-ttu-id="8717f-158">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="8717f-158">encryptedCredential</span></span> |<span data-ttu-id="8717f-159">Adja meg a hello titkosított hitelesítő adat tooaccess hello FTP-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="8717f-159">Specify hello encrypted credential tooaccess hello FTP server.</span></span> |<span data-ttu-id="8717f-160">Nem</span><span class="sxs-lookup"><span data-stu-id="8717f-160">No</span></span> |&nbsp; |
| <span data-ttu-id="8717f-161">gatewayName</span><span class="sxs-lookup"><span data-stu-id="8717f-161">gatewayName</span></span> |<span data-ttu-id="8717f-162">Adja meg hello hello átjárót az adatkezelési átjáró tooconnect tooan helyszíni FTP-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="8717f-162">Specify hello name of hello gateway in Data Management Gateway tooconnect tooan on-premises FTP server.</span></span> |<span data-ttu-id="8717f-163">Nem</span><span class="sxs-lookup"><span data-stu-id="8717f-163">No</span></span> |&nbsp; |
| <span data-ttu-id="8717f-164">port</span><span class="sxs-lookup"><span data-stu-id="8717f-164">port</span></span> |<span data-ttu-id="8717f-165">Adja meg, melyik hello FTP-kiszolgáló figyel hello port.</span><span class="sxs-lookup"><span data-stu-id="8717f-165">Specify hello port on which hello FTP server is listening.</span></span> |<span data-ttu-id="8717f-166">Nem</span><span class="sxs-lookup"><span data-stu-id="8717f-166">No</span></span> |<span data-ttu-id="8717f-167">21</span><span class="sxs-lookup"><span data-stu-id="8717f-167">21</span></span> |
| <span data-ttu-id="8717f-168">enableSsl</span><span class="sxs-lookup"><span data-stu-id="8717f-168">enableSsl</span></span> |<span data-ttu-id="8717f-169">Adja meg, hogy toouse FTP SSL/TLS-csatornán keresztül.</span><span class="sxs-lookup"><span data-stu-id="8717f-169">Specify whether toouse FTP over an SSL/TLS channel.</span></span> |<span data-ttu-id="8717f-170">Nem</span><span class="sxs-lookup"><span data-stu-id="8717f-170">No</span></span> |<span data-ttu-id="8717f-171">Igaz</span><span class="sxs-lookup"><span data-stu-id="8717f-171">true</span></span> |
| <span data-ttu-id="8717f-172">enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="8717f-172">enableServerCertificateValidation</span></span> |<span data-ttu-id="8717f-173">Adja meg, hogy tooenable server SSL tanúsítvány érvényesítése a TLS/SSL csatornán keresztül FTP használata esetén.</span><span class="sxs-lookup"><span data-stu-id="8717f-173">Specify whether tooenable server SSL certificate validation when you are using FTP over SSL/TLS channel.</span></span> |<span data-ttu-id="8717f-174">Nem</span><span class="sxs-lookup"><span data-stu-id="8717f-174">No</span></span> |<span data-ttu-id="8717f-175">Igaz</span><span class="sxs-lookup"><span data-stu-id="8717f-175">true</span></span> |

### <a name="use-anonymous-authentication"></a><span data-ttu-id="8717f-176">Névtelen hitelesítés</span><span class="sxs-lookup"><span data-stu-id="8717f-176">Use Anonymous authentication</span></span>

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {        
            "authenticationType": "Anonymous",
              "host": "myftpserver.com"
        }
    }
}
```

### <a name="use-username-and-password-in-plain-text-for-basic-authentication"></a><span data-ttu-id="8717f-177">Felhasználónevet és jelszót egyszerű szövegként használja az egyszerű hitelesítés</span><span class="sxs-lookup"><span data-stu-id="8717f-177">Use username and password in plain text for basic authentication</span></span>

```JSON
{
    "name": "FTPLinkedService",
      "properties": {
    "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
      }
}
```

### <a name="use-port-enablessl-enableservercertificatevalidation"></a><span data-ttu-id="8717f-178">Használja a portot, enableSsl, enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="8717f-178">Use port, enableSsl, enableServerCertificateValidation</span></span>

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",    
            "username": "Admin",
            "password": "123456",
            "port": "21",
            "enableSsl": true,
            "enableServerCertificateValidation": true
        }
    }
}
```

### <a name="use-encryptedcredential-for-authentication-and-gateway"></a><span data-ttu-id="8717f-179">A hitelesítés és az átjáró encryptedCredential használata</span><span class="sxs-lookup"><span data-stu-id="8717f-179">Use encryptedCredential for authentication and gateway</span></span>

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "gatewayName": "mygateway"
        }
      }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="8717f-180">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="8717f-180">Dataset properties</span></span>
<span data-ttu-id="8717f-181">Szakaszok és meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: [adatkészletek létrehozása](data-factory-create-datasets.md).</span><span class="sxs-lookup"><span data-stu-id="8717f-181">For a full list of sections and properties available for defining datasets, see [Creating datasets](data-factory-create-datasets.md).</span></span> <span data-ttu-id="8717f-182">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében.</span><span class="sxs-lookup"><span data-stu-id="8717f-182">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="8717f-183">Hello **typeProperties** szakaszban nem egyezik az adatkészlet egyes típusú.</span><span class="sxs-lookup"><span data-stu-id="8717f-183">hello **typeProperties** section is different for each type of dataset.</span></span> <span data-ttu-id="8717f-184">Adott toohello adathalmaztípushoz információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="8717f-184">It provides information that is specific toohello dataset type.</span></span> <span data-ttu-id="8717f-185">Hello **typeProperties** szakasz egy adatkészlet típusú **fájlmegosztási** rendelkezik hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="8717f-185">hello **typeProperties** section for a dataset of type **FileShare** has hello following properties:</span></span>

| <span data-ttu-id="8717f-186">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="8717f-186">Property</span></span> | <span data-ttu-id="8717f-187">Leírás</span><span class="sxs-lookup"><span data-stu-id="8717f-187">Description</span></span> | <span data-ttu-id="8717f-188">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8717f-188">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8717f-189">folderPath</span><span class="sxs-lookup"><span data-stu-id="8717f-189">folderPath</span></span> |<span data-ttu-id="8717f-190">Részleges toohello mappát.</span><span class="sxs-lookup"><span data-stu-id="8717f-190">Subpath toohello folder.</span></span> <span data-ttu-id="8717f-191">Használja az escape-karakter "\" hello karakterlánc speciális karakter.</span><span class="sxs-lookup"><span data-stu-id="8717f-191">Use escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="8717f-192">Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat.</span><span class="sxs-lookup"><span data-stu-id="8717f-192">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="8717f-193">Ez a tulajdonság a kombinálhatja **partitionBy** toohave mappák elérési útjaiban szelet alapján kezdési és befejezési dátum-idő.</span><span class="sxs-lookup"><span data-stu-id="8717f-193">You can combine this property with **partitionBy** toohave folder paths based on slice start and end date-times.</span></span> |<span data-ttu-id="8717f-194">Igen</span><span class="sxs-lookup"><span data-stu-id="8717f-194">Yes</span></span> |
| <span data-ttu-id="8717f-195">fileName</span><span class="sxs-lookup"><span data-stu-id="8717f-195">fileName</span></span> |<span data-ttu-id="8717f-196">Meg kell adnia hello fájl hello nevet hello **folderPath** Ha azt szeretné, hogy hello tábla toorefer tooa adott fájl hello mappában.</span><span class="sxs-lookup"><span data-stu-id="8717f-196">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="8717f-197">Ha nem ad meg semmilyen értéket ehhez a tulajdonsághoz, hello tábla mutat tooall fájlok hello mappában.</span><span class="sxs-lookup"><span data-stu-id="8717f-197">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="8717f-198">Ha **Fájlnév** nincs megadva egy kimeneti adatkészlet hello név hello létrehozott fájl formátuma a következő hello van:</span><span class="sxs-lookup"><span data-stu-id="8717f-198">When **fileName** is not specified for an output dataset, hello name of hello generated file is in hello following format:</span></span> <br/><br/><span data-ttu-id="8717f-199">Adatok. <Guid>.txt (Példa: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span><span class="sxs-lookup"><span data-stu-id="8717f-199">Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span></span> |<span data-ttu-id="8717f-200">Nem</span><span class="sxs-lookup"><span data-stu-id="8717f-200">No</span></span> |
| <span data-ttu-id="8717f-201">fileFilter</span><span class="sxs-lookup"><span data-stu-id="8717f-201">fileFilter</span></span> |<span data-ttu-id="8717f-202">Adja meg a szűrő használt toobe tooselect fájlok egy részének hello **folderPath**, ahelyett, hogy minden fájl.</span><span class="sxs-lookup"><span data-stu-id="8717f-202">Specify a filter toobe used tooselect a subset of files in hello **folderPath**, rather than all files.</span></span><br/><br/><span data-ttu-id="8717f-203">Két érték engedélyezett: `*` (több karaktert) és `?` (egyetlen karakter).</span><span class="sxs-lookup"><span data-stu-id="8717f-203">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="8717f-204">1. példa:`"fileFilter": "*.log"`</span><span class="sxs-lookup"><span data-stu-id="8717f-204">Example 1: `"fileFilter": "*.log"`</span></span><br/><span data-ttu-id="8717f-205">2. példa:`"fileFilter": 2014-1-?.txt"`</span><span class="sxs-lookup"><span data-stu-id="8717f-205">Example 2: `"fileFilter": 2014-1-?.txt"`</span></span><br/><br/> <span data-ttu-id="8717f-206">**fileFilter** egy bemeneti fájlmegosztási adatkészlet esetében alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="8717f-206">**fileFilter** is applicable for an input FileShare dataset.</span></span> <span data-ttu-id="8717f-207">Ez a tulajdonság nem támogatott a Hadoop elosztott fájlrendszerrel (HDFS).</span><span class="sxs-lookup"><span data-stu-id="8717f-207">This property is not supported with Hadoop Distributed File System (HDFS).</span></span> |<span data-ttu-id="8717f-208">Nem</span><span class="sxs-lookup"><span data-stu-id="8717f-208">No</span></span> |
| <span data-ttu-id="8717f-209">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="8717f-209">partitionedBy</span></span> |<span data-ttu-id="8717f-210">Dinamikus toospecify használt **folderPath** és **Fájlnév** idő adatsorozat adatok.</span><span class="sxs-lookup"><span data-stu-id="8717f-210">Used toospecify a dynamic **folderPath** and **fileName** for time series data.</span></span> <span data-ttu-id="8717f-211">Megadhat például egy **folderPath** , amely az adatok óránkénti paraméteres.</span><span class="sxs-lookup"><span data-stu-id="8717f-211">For example, you can specify a **folderPath** that is parameterized for every hour of data.</span></span> |<span data-ttu-id="8717f-212">Nem</span><span class="sxs-lookup"><span data-stu-id="8717f-212">No</span></span> |
| <span data-ttu-id="8717f-213">Formátumban</span><span class="sxs-lookup"><span data-stu-id="8717f-213">format</span></span> | <span data-ttu-id="8717f-214">a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="8717f-214">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="8717f-215">Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="8717f-215">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="8717f-216">További információkért lásd: hello [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátumban ](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="8717f-216">For more information, see hello [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="8717f-217">Ha azt szeretné toocopy fájlok, mivel ezek között a fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="8717f-217">If you want toocopy files as they are between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="8717f-218">Nem</span><span class="sxs-lookup"><span data-stu-id="8717f-218">No</span></span> |
| <span data-ttu-id="8717f-219">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="8717f-219">compression</span></span> | <span data-ttu-id="8717f-220">Adja meg a hello típusát és hello adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="8717f-220">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="8717f-221">Támogatott típusok a következők **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**, és a támogatott szintek a következők **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="8717f-221">Supported types are **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**, and supported levels are **Optimal** and **Fastest**.</span></span> <span data-ttu-id="8717f-222">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="8717f-222">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="8717f-223">Nem</span><span class="sxs-lookup"><span data-stu-id="8717f-223">No</span></span> |
| <span data-ttu-id="8717f-224">useBinaryTransfer</span><span class="sxs-lookup"><span data-stu-id="8717f-224">useBinaryTransfer</span></span> |<span data-ttu-id="8717f-225">Adja meg, hogy toouse hello bináris átviteli módot.</span><span class="sxs-lookup"><span data-stu-id="8717f-225">Specify whether toouse hello binary transfer mode.</span></span> <span data-ttu-id="8717f-226">hello értékek: true bináris mód (Ez az alapértelmezett érték hello), és hamis értéket ASCII.</span><span class="sxs-lookup"><span data-stu-id="8717f-226">hello values are true for binary mode (this is hello default value), and false for ASCII.</span></span> <span data-ttu-id="8717f-227">Ez a tulajdonság csak használható, ha hello társított társított szolgáltatás típusa típusú: FTP-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="8717f-227">This property can only be used when hello associated linked service type is of type: FtpServer.</span></span> |<span data-ttu-id="8717f-228">Nem</span><span class="sxs-lookup"><span data-stu-id="8717f-228">No</span></span> |

> [!NOTE]
> <span data-ttu-id="8717f-229">**Fájlnév** és **fileFilter** nem használható egyszerre.</span><span class="sxs-lookup"><span data-stu-id="8717f-229">**fileName** and **fileFilter** cannot be used simultaneously.</span></span>

### <a name="use-hello-partionedby-property"></a><span data-ttu-id="8717f-230">Hello partionedBy tulajdonság</span><span class="sxs-lookup"><span data-stu-id="8717f-230">Use hello partionedBy property</span></span>
<span data-ttu-id="8717f-231">Hello előző szakaszban említett, megadhat egy dinamikus **folderPath** és **Fájlnév** idő adatsorozat adatokhoz hello **partitionedBy** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="8717f-231">As mentioned in hello previous section, you can specify a dynamic **folderPath** and **fileName** for time series data with hello **partitionedBy** property.</span></span>

<span data-ttu-id="8717f-232">toolearn idő adatsorozat adatkészleteket, ütemezés és szeletek, lásd: [adatkészletek létrehozása](data-factory-create-datasets.md), [ütemezés és a végrehajtás](data-factory-scheduling-and-execution.md), és [folyamatok létrehozása](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="8717f-232">toolearn about time series datasets, scheduling, and slices, see [Creating datasets](data-factory-create-datasets.md), [Scheduling and execution](data-factory-scheduling-and-execution.md), and [Creating pipelines](data-factory-create-pipelines.md).</span></span>

#### <a name="sample-1"></a><span data-ttu-id="8717f-233">1. példa</span><span class="sxs-lookup"><span data-stu-id="8717f-233">Sample 1</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
<span data-ttu-id="8717f-234">Ebben a példában {szelet} adat-előállító rendszer változó SliceStart hello érték helyére, a hello formázza a megadott (YYYYMMDDHH).</span><span class="sxs-lookup"><span data-stu-id="8717f-234">In this example, {Slice} is replaced with hello value of Data Factory system variable SliceStart, in hello format specified (YYYYMMDDHH).</span></span> <span data-ttu-id="8717f-235">hello SliceStart toostart idő hello szelet hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="8717f-235">hello SliceStart refers toostart time of hello slice.</span></span> <span data-ttu-id="8717f-236">hello mappa elérési útja eltér az egyes szeletek.</span><span class="sxs-lookup"><span data-stu-id="8717f-236">hello folder path is different for each slice.</span></span> <span data-ttu-id="8717f-237">(Például wikidatagateway/wikisampledataout/2014100103 vagy wikidatagateway/wikisampledataout/2014100104.)</span><span class="sxs-lookup"><span data-stu-id="8717f-237">(For example, wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.)</span></span>

#### <a name="sample-2"></a><span data-ttu-id="8717f-238">2. példa</span><span class="sxs-lookup"><span data-stu-id="8717f-238">Sample 2</span></span>

```json
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
<span data-ttu-id="8717f-239">Ebben a példában hello év, hónap, nap és SliceStart idején ki kell olvasni a külön változók hello által használt **folderPath** és **Fájlnév** tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="8717f-239">In this example, hello year, month, day, and time of SliceStart are extracted into separate variables that are used by hello **folderPath** and **fileName** properties.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="8717f-240">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="8717f-240">Copy activity properties</span></span>
<span data-ttu-id="8717f-241">Szakaszok és a rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: [folyamatok létrehozása](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="8717f-241">For a full list of sections and properties available for defining activities, see [Creating pipelines](data-factory-create-pipelines.md).</span></span> <span data-ttu-id="8717f-242">Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti táblák és házirendek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="8717f-242">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="8717f-243">Tulajdonságok érhetők el hello **typeProperties** szakasza hello tevékenységet, hello ugyanakkor, tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="8717f-243">Properties available in hello **typeProperties** section of hello activity, on hello other hand, vary with each activity type.</span></span> <span data-ttu-id="8717f-244">Hello másolási tevékenységhez hello típustulajdonságokat hello típusú források és mosdók függenek.</span><span class="sxs-lookup"><span data-stu-id="8717f-244">For hello copy activity, hello type properties vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="8717f-245">A másolási tevékenység, ha hello adatforrás típusú **FileSystemSource**, a következő tulajdonság hello érhető el **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="8717f-245">In copy activity, when hello source is of type **FileSystemSource**, hello following property is available in **typeProperties** section:</span></span>

| <span data-ttu-id="8717f-246">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="8717f-246">Property</span></span> | <span data-ttu-id="8717f-247">Leírás</span><span class="sxs-lookup"><span data-stu-id="8717f-247">Description</span></span> | <span data-ttu-id="8717f-248">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="8717f-248">Allowed values</span></span> | <span data-ttu-id="8717f-249">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8717f-249">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="8717f-250">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="8717f-250">recursive</span></span> |<span data-ttu-id="8717f-251">Azt jelzi, hogy hello adatolvasás rekurzív módon hello almappák, vagy csak a megadott mappa hello.</span><span class="sxs-lookup"><span data-stu-id="8717f-251">Indicates whether hello data is read recursively from hello subfolders, or only from hello specified folder.</span></span> |<span data-ttu-id="8717f-252">IGAZ, hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="8717f-252">True, False (default)</span></span> |<span data-ttu-id="8717f-253">Nem</span><span class="sxs-lookup"><span data-stu-id="8717f-253">No</span></span> |

## <a name="json-example-copy-data-from-ftp-server-tooazure-blob"></a><span data-ttu-id="8717f-254">JSON-példa: adatok másolása az FTP-kiszolgáló tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="8717f-254">JSON example: Copy data from FTP server tooAzure Blob</span></span>
<span data-ttu-id="8717f-255">Ez a példa bemutatja, hogyan egy FTP-kiszolgáló tooAzure Blob-tároló toocopy adatait.</span><span class="sxs-lookup"><span data-stu-id="8717f-255">This sample shows how toocopy data from an FTP server tooAzure Blob storage.</span></span> <span data-ttu-id="8717f-256">Azonban adatok átmásolhatók, közvetlenül a hello tooany fogadók esetében a megadott hello [adatokról és formátumok támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats), adat-előállítóban hello másolási tevékenység használatával.</span><span class="sxs-lookup"><span data-stu-id="8717f-256">However, data can be copied directly tooany of hello sinks stated in hello [supported data stores and formats](data-factory-data-movement-activities.md#supported-data-stores-and-formats), by using hello copy activity in Data Factory.</span></span>  

<span data-ttu-id="8717f-257">hello alábbi példák megadják minta JSON-definíciók használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), vagy [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="8717f-257">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md):</span></span>

* <span data-ttu-id="8717f-258">A társított szolgáltatás típusa [FTP-kiszolgáló](#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="8717f-258">A linked service of type [FtpServer](#linked-service-properties)</span></span>
* <span data-ttu-id="8717f-259">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="8717f-259">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span></span>
* <span data-ttu-id="8717f-260">Bemeneti [dataset](data-factory-create-datasets.md) típusú [fájlmegosztás](#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="8717f-260">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties)</span></span>
* <span data-ttu-id="8717f-261">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="8717f-261">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span></span>
* <span data-ttu-id="8717f-262">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [FileSystemSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="8717f-262">A [pipeline](data-factory-create-pipelines.md) with copy activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span></span>

<span data-ttu-id="8717f-263">hello minta másol adatokat az FTP-kiszolgáló tooan Azure blob minden órában.</span><span class="sxs-lookup"><span data-stu-id="8717f-263">hello sample copies data from an FTP server tooan Azure blob every hour.</span></span> <span data-ttu-id="8717f-264">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="8717f-264">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

### <a name="ftp-linked-service"></a><span data-ttu-id="8717f-265">Kapcsolódó FTP-szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="8717f-265">FTP linked service</span></span>

<span data-ttu-id="8717f-266">Ebben a példában hello felhasználónevet és jelszót a szövegként egyszerű hitelesítést használ.</span><span class="sxs-lookup"><span data-stu-id="8717f-266">This example uses basic authentication, with hello user name and password in plain text.</span></span> <span data-ttu-id="8717f-267">A következő módokon hello egyikét is használhatja:</span><span class="sxs-lookup"><span data-stu-id="8717f-267">You can also use one of hello following ways:</span></span>

* <span data-ttu-id="8717f-268">A névtelen hitelesítés</span><span class="sxs-lookup"><span data-stu-id="8717f-268">Anonymous authentication</span></span>
* <span data-ttu-id="8717f-269">Egyszerű hitelesítés titkosított hitelesítő adatokkal</span><span class="sxs-lookup"><span data-stu-id="8717f-269">Basic authentication with encrypted credentials</span></span>
* <span data-ttu-id="8717f-270">FTP-keresztül SSL/TLS (ftps-t)</span><span class="sxs-lookup"><span data-stu-id="8717f-270">FTP over SSL/TLS (FTPS)</span></span>

<span data-ttu-id="8717f-271">Lásd: hello [FTP társított szolgáltatás](#linked-service-properties) szakasz a különböző típusú hitelesítés használható.</span><span class="sxs-lookup"><span data-stu-id="8717f-271">See hello [FTP linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

```JSON
{
    "name": "FTPLinkedService",
    "properties": {
    "type": "FtpServer",
    "typeProperties": {
        "host": "myftpserver.com",           
        "authenticationType": "Basic",
        "username": "Admin",
        "password": "123456"
    }
  }
}
```
### <a name="azure-storage-linked-service"></a><span data-ttu-id="8717f-272">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="8717f-272">Azure Storage linked service</span></span>

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
### <a name="ftp-input-dataset"></a><span data-ttu-id="8717f-273">FTP-bemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="8717f-273">FTP input dataset</span></span>

<span data-ttu-id="8717f-274">Ez az adatkészlet hivatkozik toohello FTP mappa `mysharedfolder` és fájl `test.csv`.</span><span class="sxs-lookup"><span data-stu-id="8717f-274">This dataset refers toohello FTP folder `mysharedfolder` and file `test.csv`.</span></span> <span data-ttu-id="8717f-275">hello csővezeték hello fájl toohello cél másolja.</span><span class="sxs-lookup"><span data-stu-id="8717f-275">hello pipeline copies hello file toohello destination.</span></span>

<span data-ttu-id="8717f-276">Beállítás **külső** túl**igaz** hello Data Factory szolgáltatásnak tájékoztatja, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="8717f-276">Setting **external** too**true** informs hello Data Factory service that hello dataset is external toohello data factory, and is not produced by an activity in hello data factory.</span></span>

```JSON
{
  "name": "FTPFileInput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": "FTPLinkedService",
    "typeProperties": {
      "folderPath": "mysharedfolder",
      "fileName": "test.csv",
      "useBinaryTransfer": true
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

### <a name="azure-blob-output-dataset"></a><span data-ttu-id="8717f-277">Azure Blob kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="8717f-277">Azure Blob output dataset</span></span>

<span data-ttu-id="8717f-278">Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="8717f-278">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="8717f-279">hello mappa elérési útja hello BLOB dinamikusan értékeli ki, hello szelet által feldolgozott hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="8717f-279">hello folder path for hello blob is dynamically evaluated, based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="8717f-280">hello mappa elérési útját használja hello év, hónap, nap és óra részei hello kezdési ideje.</span><span class="sxs-lookup"><span data-stu-id="8717f-280">hello folder path uses hello year, month, day, and hours parts of hello start time.</span></span>

```JSON
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/ftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


### <a name="a-copy-activity-in-a-pipeline-with-file-system-source-and-blob-sink"></a><span data-ttu-id="8717f-281">A másolási tevékenység során a rendszer forrás- és a blob fájlgyűjtő egy folyamaton belül</span><span class="sxs-lookup"><span data-stu-id="8717f-281">A copy activity in a pipeline with file system source and blob sink</span></span>

<span data-ttu-id="8717f-282">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek, és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="8717f-282">hello pipeline contains a copy activity that is configured toouse hello input and output datasets, and is scheduled toorun every hour.</span></span> <span data-ttu-id="8717f-283">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**FileSystemSource**, és hello **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="8717f-283">In hello pipeline JSON definition, hello **source** type is set too**FileSystemSource**, and hello **sink** type is set too**BlobSink**.</span></span>

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "FTPToBlobCopy",
            "inputs": [{
                "name": "FtpFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
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
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-08-24T18:00:00Z",
        "end": "2016-08-24T19:00:00Z"
    }
}
```
> [!NOTE]
> <span data-ttu-id="8717f-284">Tekintse meg a forrás adatkészlet toocolumns fogadó adatkészletből toomap oszlopokat [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="8717f-284">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8717f-285">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8717f-285">Next steps</span></span>
<span data-ttu-id="8717f-286">Tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="8717f-286">See hello following articles:</span></span>

* <span data-ttu-id="8717f-287">toolearn kulccsal kapcsolatos tényezők adatátvitelt jelölik a (másolási tevékenység) az adat-előállítót, és különböző módokon toooptimize hatás teljesítmény, lásd: hello [másolása tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="8717f-287">toolearn about key factors that impact performance of data movement (copy activity) in Data Factory, and various ways toooptimize it, see hello [Copy activity performance and tuning guide](data-factory-copy-activity-performance.md).</span></span>

* <span data-ttu-id="8717f-288">A másolási tevékenység során a folyamat létrehozása részletes ismertetését lásd: hello [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="8717f-288">For step-by-step instructions for creating a pipeline with a copy activity, see hello [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
