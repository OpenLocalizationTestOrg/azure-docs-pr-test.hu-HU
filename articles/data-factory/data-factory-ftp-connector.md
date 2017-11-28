---
title: "Adatok áthelyezése az FTP-kiszolgáló Azure Data Factory használatával |} Microsoft Docs"
description: "Tudnivalók az adatok áthelyezése az Azure Data Factory használatával FTP-kiszolgálóhoz."
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
ms.openlocfilehash: f8f31f3a2ee02c964737dd32145499f3dcfd0624
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-an-ftp-server-by-using-azure-data-factory"></a><span data-ttu-id="ae522-103">Adatok áthelyezése az FTP-kiszolgáló Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="ae522-103">Move data from an FTP server by using Azure Data Factory</span></span>
<span data-ttu-id="ae522-104">Ez a cikk ismerteti, hogyan a másolási tevékenység során az Azure Data Factoryben az adatok mozgatása az FTP-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="ae522-104">This article explains how to use the copy activity in Azure Data Factory to move data from an FTP server.</span></span> <span data-ttu-id="ae522-105">Buildekről nyújtanak a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amelynek során adatátvitel a másolási tevékenység az általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="ae522-105">It builds on the [Data movement activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="ae522-106">Adatok átmásolhatja az FTP-kiszolgáló bármely támogatott fogadó adattárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="ae522-106">You can copy data from an FTP server to any supported sink data store.</span></span> <span data-ttu-id="ae522-107">A másolási tevékenység által támogatott mosdók adattárolókhoz listájáért lásd: a [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="ae522-107">For a list of data stores supported as sinks by the copy activity, see the [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="ae522-108">Adat-előállító jelenleg támogatja áthelyezése adatok kizárólag az FTP-kiszolgáló az egyéb adattárakhoz, de nem adatok áthelyezését más adatokat tárolja, FTP-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="ae522-108">Data Factory currently supports only moving data from an FTP server to other data stores, but not moving data from other data stores to an FTP server.</span></span> <span data-ttu-id="ae522-109">Az támogatja-e mind a helyszíni és felhőalapú FTP-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="ae522-109">It supports both on-premises and cloud FTP servers.</span></span>

> [!NOTE]
> <span data-ttu-id="ae522-110">A másolási tevékenység nem törli a forrásfájl, miután sikerült átmásolni a cél.</span><span class="sxs-lookup"><span data-stu-id="ae522-110">The copy activity does not delete the source file after it is successfully copied to the destination.</span></span> <span data-ttu-id="ae522-111">Ha a forrásfájl törlése után sikeres másolatot van szüksége, létrehozhat egy egyéni a fájl törlésére, és használja a tevékenységet a feldolgozási.</span><span class="sxs-lookup"><span data-stu-id="ae522-111">If you need to delete the source file after a successful copy, create a custom activity to delete the file, and use the activity in the pipeline.</span></span> 

## <a name="enable-connectivity"></a><span data-ttu-id="ae522-112">Kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="ae522-112">Enable connectivity</span></span>
<span data-ttu-id="ae522-113">Ha áthelyezi adatait egy **helyszíni** adatokat (például az Azure Blob storage) tárolására, telepítése és használata az adatkezelési átjáró felhő FTP-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="ae522-113">If you are moving data from an **on-premises** FTP server to a cloud data store (for example, to Azure Blob storage), install and use Data Management Gateway.</span></span> <span data-ttu-id="ae522-114">Az adatkezelési átjáró egy olyan ügyfélügynök, a helyszíni számítógépre telepített, és lehetővé teszi a felhőszolgáltatások csatlakozás egy helyszíni erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="ae522-114">The Data Management Gateway is a client agent that is installed on your on-premises machine, and it allows cloud services to connect to an on-premises resource.</span></span> <span data-ttu-id="ae522-115">További információkért lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="ae522-115">For details, see [Data Management Gateway](data-factory-data-management-gateway.md).</span></span> <span data-ttu-id="ae522-116">A beállítás részletes utasításokat az átjáró össze, és használata, lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="ae522-116">For step-by-step instructions on setting up the gateway and using it, see [Moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md).</span></span> <span data-ttu-id="ae522-117">Az átjáró, FTP-kiszolgálóhoz való kapcsolódáshoz használja, akkor is, ha a kiszolgáló az az Azure-infrastruktúrák (IaaS) szolgáltatás virtuális gépként (VM).</span><span class="sxs-lookup"><span data-stu-id="ae522-117">You use the gateway to connect to an FTP server, even if the server is on an Azure infrastructure as a service (IaaS) virtual machine (VM).</span></span>

<span data-ttu-id="ae522-118">Az átjáró telepíthető ugyanarra a helyi számítógépen vagy infrastruktúra-szolgáltatási virtuális gép az FTP-kiszolgálóként is.</span><span class="sxs-lookup"><span data-stu-id="ae522-118">It is possible to install the gateway on the same on-premises machine or IaaS VM as the FTP server.</span></span> <span data-ttu-id="ae522-119">Azt javasoljuk azonban, hogy telepítse az átjáró, egy másik számítógépre, vagy az infrastruktúra-szolgáltatási virtuális gép az Erőforrásverseny elkerülése érdekében, és a jobb teljesítmény érdekében.</span><span class="sxs-lookup"><span data-stu-id="ae522-119">However, we recommend that you install the gateway on a separate machine or IaaS VM to avoid resource contention, and for better performance.</span></span> <span data-ttu-id="ae522-120">Az átjáró egy külön számítógépen való telepítésekor a gép érhessék el az FTP-kiszolgálón kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ae522-120">When you install the gateway on a separate machine, the machine should be able to access the FTP server.</span></span>

## <a name="get-started"></a><span data-ttu-id="ae522-121">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="ae522-121">Get started</span></span>
<span data-ttu-id="ae522-122">A másolási tevékenység, amely FTP forrásból származó adatokat a különböző eszközök vagy API-k használatával helyezi át a feldolgozási sor hozhatja létre.</span><span class="sxs-lookup"><span data-stu-id="ae522-122">You can create a pipeline with a copy activity that moves data from an FTP source by using different tools or APIs.</span></span>

<span data-ttu-id="ae522-123">Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **Data Factory másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="ae522-123">The easiest way to create a pipeline is to use the **Data Factory Copy Wizard**.</span></span> <span data-ttu-id="ae522-124">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="ae522-124">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough.</span></span>

<span data-ttu-id="ae522-125">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="ae522-125">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="ae522-126">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="ae522-126">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span>

<span data-ttu-id="ae522-127">Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban:</span><span class="sxs-lookup"><span data-stu-id="ae522-127">Whether you use the tools or APIs, perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="ae522-128">Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.</span><span class="sxs-lookup"><span data-stu-id="ae522-128">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="ae522-129">Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="ae522-129">Create **datasets** to represent input and output data for the copy operation.</span></span>
3. <span data-ttu-id="ae522-130">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="ae522-130">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span>

<span data-ttu-id="ae522-131">A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="ae522-131">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="ae522-132">Eszközök vagy API-k (kivéve a .NET API-t) használ, amikor az a JSON formátum használatával adja meg a Data Factory entitások.</span><span class="sxs-lookup"><span data-stu-id="ae522-132">When you use tools or APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span> <span data-ttu-id="ae522-133">Adatok másolása egy FTP-adattároló használt adat-előállító entitások JSON-definíciók minta, tekintse meg a [JSON-példa: adatok másolása az FTP-kiszolgáló az Azure blob](#json-example-copy-data-from-ftp-server-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="ae522-133">For a sample with JSON definitions for Data Factory entities that are used to copy data from an FTP data store, see the [JSON example: Copy data from FTP server to Azure blob](#json-example-copy-data-from-ftp-server-to-azure-blob) section of this article.</span></span>

> [!NOTE]
> <span data-ttu-id="ae522-134">Támogatott tömörítési formátumú és használatával kapcsolatos részletekért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md).</span><span class="sxs-lookup"><span data-stu-id="ae522-134">For details about supported file and compression formats to use, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md).</span></span>

<span data-ttu-id="ae522-135">A következő szakaszok részletesen bemutatják, amely segítségével határozza meg a Data Factory entitások adott FTP-JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="ae522-135">The following sections provide details about JSON properties that are used to define Data Factory entities specific to FTP.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="ae522-136">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="ae522-136">Linked service properties</span></span>
<span data-ttu-id="ae522-137">A következő táblázat ismerteti a JSON-elemek szerepelnek az FTP-kapcsolódó szolgáltatásra vonatkozó.</span><span class="sxs-lookup"><span data-stu-id="ae522-137">The following table describes JSON elements specific to an FTP linked service.</span></span>

| <span data-ttu-id="ae522-138">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ae522-138">Property</span></span> | <span data-ttu-id="ae522-139">Leírás</span><span class="sxs-lookup"><span data-stu-id="ae522-139">Description</span></span> | <span data-ttu-id="ae522-140">Szükséges</span><span class="sxs-lookup"><span data-stu-id="ae522-140">Required</span></span> | <span data-ttu-id="ae522-141">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="ae522-141">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ae522-142">type</span><span class="sxs-lookup"><span data-stu-id="ae522-142">type</span></span> |<span data-ttu-id="ae522-143">Válassza az FTP-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="ae522-143">Set this to FtpServer.</span></span> |<span data-ttu-id="ae522-144">Igen</span><span class="sxs-lookup"><span data-stu-id="ae522-144">Yes</span></span> |&nbsp; |
| <span data-ttu-id="ae522-145">állomás</span><span class="sxs-lookup"><span data-stu-id="ae522-145">host</span></span> |<span data-ttu-id="ae522-146">Adja meg a nevét vagy az FTP-kiszolgáló IP-címét.</span><span class="sxs-lookup"><span data-stu-id="ae522-146">Specify the name or IP address of the FTP server.</span></span> |<span data-ttu-id="ae522-147">Igen</span><span class="sxs-lookup"><span data-stu-id="ae522-147">Yes</span></span> |&nbsp; |
| <span data-ttu-id="ae522-148">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="ae522-148">authenticationType</span></span> |<span data-ttu-id="ae522-149">Adja meg a hitelesítés típusát.</span><span class="sxs-lookup"><span data-stu-id="ae522-149">Specify the authentication type.</span></span> |<span data-ttu-id="ae522-150">Igen</span><span class="sxs-lookup"><span data-stu-id="ae522-150">Yes</span></span> |<span data-ttu-id="ae522-151">Alapszintű, a névtelen</span><span class="sxs-lookup"><span data-stu-id="ae522-151">Basic, Anonymous</span></span> |
| <span data-ttu-id="ae522-152">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="ae522-152">username</span></span> |<span data-ttu-id="ae522-153">Adja meg a felhasználót, aki hozzáfér az FTP-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="ae522-153">Specify the user who has access to the FTP server.</span></span> |<span data-ttu-id="ae522-154">Nem</span><span class="sxs-lookup"><span data-stu-id="ae522-154">No</span></span> |&nbsp; |
| <span data-ttu-id="ae522-155">jelszó</span><span class="sxs-lookup"><span data-stu-id="ae522-155">password</span></span> |<span data-ttu-id="ae522-156">Adja meg a felhasználó (felhasználónév) jelszavát.</span><span class="sxs-lookup"><span data-stu-id="ae522-156">Specify the password for the user (username).</span></span> |<span data-ttu-id="ae522-157">Nem</span><span class="sxs-lookup"><span data-stu-id="ae522-157">No</span></span> |&nbsp; |
| <span data-ttu-id="ae522-158">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="ae522-158">encryptedCredential</span></span> |<span data-ttu-id="ae522-159">Adja meg a titkosított hitelesítő adatokat, az FTP-kiszolgáló eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="ae522-159">Specify the encrypted credential to access the FTP server.</span></span> |<span data-ttu-id="ae522-160">Nem</span><span class="sxs-lookup"><span data-stu-id="ae522-160">No</span></span> |&nbsp; |
| <span data-ttu-id="ae522-161">gatewayName</span><span class="sxs-lookup"><span data-stu-id="ae522-161">gatewayName</span></span> |<span data-ttu-id="ae522-162">Adja meg az átjáró nevét az adatkezelési átjáró helyszíni FTP-kiszolgálóhoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="ae522-162">Specify the name of the gateway in Data Management Gateway to connect to an on-premises FTP server.</span></span> |<span data-ttu-id="ae522-163">Nem</span><span class="sxs-lookup"><span data-stu-id="ae522-163">No</span></span> |&nbsp; |
| <span data-ttu-id="ae522-164">port</span><span class="sxs-lookup"><span data-stu-id="ae522-164">port</span></span> |<span data-ttu-id="ae522-165">Adja meg a portot, amelyet az FTP-kiszolgáló figyel.</span><span class="sxs-lookup"><span data-stu-id="ae522-165">Specify the port on which the FTP server is listening.</span></span> |<span data-ttu-id="ae522-166">Nem</span><span class="sxs-lookup"><span data-stu-id="ae522-166">No</span></span> |<span data-ttu-id="ae522-167">21</span><span class="sxs-lookup"><span data-stu-id="ae522-167">21</span></span> |
| <span data-ttu-id="ae522-168">enableSsl</span><span class="sxs-lookup"><span data-stu-id="ae522-168">enableSsl</span></span> |<span data-ttu-id="ae522-169">Adja meg, hogy a TLS/SSL csatornán FTP használata.</span><span class="sxs-lookup"><span data-stu-id="ae522-169">Specify whether to use FTP over an SSL/TLS channel.</span></span> |<span data-ttu-id="ae522-170">Nem</span><span class="sxs-lookup"><span data-stu-id="ae522-170">No</span></span> |<span data-ttu-id="ae522-171">Igaz</span><span class="sxs-lookup"><span data-stu-id="ae522-171">true</span></span> |
| <span data-ttu-id="ae522-172">enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="ae522-172">enableServerCertificateValidation</span></span> |<span data-ttu-id="ae522-173">Adja meg, hogy engedélyezze a kiszolgálói SSL-tanúsítvány hitelesítése a TLS/SSL csatornán keresztül FTP használata esetén.</span><span class="sxs-lookup"><span data-stu-id="ae522-173">Specify whether to enable server SSL certificate validation when you are using FTP over SSL/TLS channel.</span></span> |<span data-ttu-id="ae522-174">Nem</span><span class="sxs-lookup"><span data-stu-id="ae522-174">No</span></span> |<span data-ttu-id="ae522-175">Igaz</span><span class="sxs-lookup"><span data-stu-id="ae522-175">true</span></span> |

### <a name="use-anonymous-authentication"></a><span data-ttu-id="ae522-176">Névtelen hitelesítés</span><span class="sxs-lookup"><span data-stu-id="ae522-176">Use Anonymous authentication</span></span>

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

### <a name="use-username-and-password-in-plain-text-for-basic-authentication"></a><span data-ttu-id="ae522-177">Felhasználónevet és jelszót egyszerű szövegként használja az egyszerű hitelesítés</span><span class="sxs-lookup"><span data-stu-id="ae522-177">Use username and password in plain text for basic authentication</span></span>

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

### <a name="use-port-enablessl-enableservercertificatevalidation"></a><span data-ttu-id="ae522-178">Használja a portot, enableSsl, enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="ae522-178">Use port, enableSsl, enableServerCertificateValidation</span></span>

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

### <a name="use-encryptedcredential-for-authentication-and-gateway"></a><span data-ttu-id="ae522-179">A hitelesítés és az átjáró encryptedCredential használata</span><span class="sxs-lookup"><span data-stu-id="ae522-179">Use encryptedCredential for authentication and gateway</span></span>

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

## <a name="dataset-properties"></a><span data-ttu-id="ae522-180">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="ae522-180">Dataset properties</span></span>
<span data-ttu-id="ae522-181">Szakaszok és meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: [adatkészletek létrehozása](data-factory-create-datasets.md).</span><span class="sxs-lookup"><span data-stu-id="ae522-181">For a full list of sections and properties available for defining datasets, see [Creating datasets](data-factory-create-datasets.md).</span></span> <span data-ttu-id="ae522-182">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében.</span><span class="sxs-lookup"><span data-stu-id="ae522-182">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="ae522-183">A **typeProperties** szakaszban nem egyezik az adatkészlet egyes típusú.</span><span class="sxs-lookup"><span data-stu-id="ae522-183">The **typeProperties** section is different for each type of dataset.</span></span> <span data-ttu-id="ae522-184">A dataset típusra vonatkozó adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ae522-184">It provides information that is specific to the dataset type.</span></span> <span data-ttu-id="ae522-185">A **typeProperties** szakasz egy adatkészlet típusú **fájlmegosztási** tulajdonságai a következők:</span><span class="sxs-lookup"><span data-stu-id="ae522-185">The **typeProperties** section for a dataset of type **FileShare** has the following properties:</span></span>

| <span data-ttu-id="ae522-186">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ae522-186">Property</span></span> | <span data-ttu-id="ae522-187">Leírás</span><span class="sxs-lookup"><span data-stu-id="ae522-187">Description</span></span> | <span data-ttu-id="ae522-188">Szükséges</span><span class="sxs-lookup"><span data-stu-id="ae522-188">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ae522-189">folderPath</span><span class="sxs-lookup"><span data-stu-id="ae522-189">folderPath</span></span> |<span data-ttu-id="ae522-190">Részleges azt a mappát.</span><span class="sxs-lookup"><span data-stu-id="ae522-190">Subpath to the folder.</span></span> <span data-ttu-id="ae522-191">Használja az escape-karakter "\" a speciális karakterek a karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="ae522-191">Use escape character ‘ \ ’ for special characters in the string.</span></span> <span data-ttu-id="ae522-192">Lásd: [minta kapcsolódó szolgáltatás és az adatkészlet-definíciók](#sample-linked-service-and-dataset-definitions) példákat.</span><span class="sxs-lookup"><span data-stu-id="ae522-192">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="ae522-193">Ez a tulajdonság a kombinálhatja **partitionBy** mappa elérési utak alapján szelet kezdési és befejezési dátum-idő.</span><span class="sxs-lookup"><span data-stu-id="ae522-193">You can combine this property with **partitionBy** to have folder paths based on slice start and end date-times.</span></span> |<span data-ttu-id="ae522-194">Igen</span><span class="sxs-lookup"><span data-stu-id="ae522-194">Yes</span></span> |
| <span data-ttu-id="ae522-195">fileName</span><span class="sxs-lookup"><span data-stu-id="ae522-195">fileName</span></span> |<span data-ttu-id="ae522-196">Adja meg a fájl nevét a **folderPath** Ha azt szeretné, hogy a tábla egy adott fájlra a mappában.</span><span class="sxs-lookup"><span data-stu-id="ae522-196">Specify the name of the file in the **folderPath** if you want the table to refer to a specific file in the folder.</span></span> <span data-ttu-id="ae522-197">Ha nem ad meg ehhez a tulajdonsághoz értéket, a tábla a mappában lévő összes fájlt mutat.</span><span class="sxs-lookup"><span data-stu-id="ae522-197">If you do not specify any value for this property, the table points to all files in the folder.</span></span><br/><br/><span data-ttu-id="ae522-198">Ha **Fájlnév** nincs megadva egy kimeneti adatkészletet, a létrehozott fájl neve nem a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="ae522-198">When **fileName** is not specified for an output dataset, the name of the generated file is in the following format:</span></span> <br/><br/><span data-ttu-id="ae522-199">Adatok. <Guid>.txt (Példa: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span><span class="sxs-lookup"><span data-stu-id="ae522-199">Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span></span> |<span data-ttu-id="ae522-200">Nem</span><span class="sxs-lookup"><span data-stu-id="ae522-200">No</span></span> |
| <span data-ttu-id="ae522-201">fileFilter</span><span class="sxs-lookup"><span data-stu-id="ae522-201">fileFilter</span></span> |<span data-ttu-id="ae522-202">Adjon meg egy szűrőt, amely használatával a fájlok egy részét jelölje ki a **folderPath**, ahelyett, hogy minden fájl.</span><span class="sxs-lookup"><span data-stu-id="ae522-202">Specify a filter to be used to select a subset of files in the **folderPath**, rather than all files.</span></span><br/><br/><span data-ttu-id="ae522-203">Két érték engedélyezett: `*` (több karaktert) és `?` (egyetlen karakter).</span><span class="sxs-lookup"><span data-stu-id="ae522-203">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="ae522-204">1. példa:`"fileFilter": "*.log"`</span><span class="sxs-lookup"><span data-stu-id="ae522-204">Example 1: `"fileFilter": "*.log"`</span></span><br/><span data-ttu-id="ae522-205">2. példa:`"fileFilter": 2014-1-?.txt"`</span><span class="sxs-lookup"><span data-stu-id="ae522-205">Example 2: `"fileFilter": 2014-1-?.txt"`</span></span><br/><br/> <span data-ttu-id="ae522-206">**fileFilter** egy bemeneti fájlmegosztási adatkészlet esetében alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="ae522-206">**fileFilter** is applicable for an input FileShare dataset.</span></span> <span data-ttu-id="ae522-207">Ez a tulajdonság nem támogatott a Hadoop elosztott fájlrendszerrel (HDFS).</span><span class="sxs-lookup"><span data-stu-id="ae522-207">This property is not supported with Hadoop Distributed File System (HDFS).</span></span> |<span data-ttu-id="ae522-208">Nem</span><span class="sxs-lookup"><span data-stu-id="ae522-208">No</span></span> |
| <span data-ttu-id="ae522-209">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="ae522-209">partitionedBy</span></span> |<span data-ttu-id="ae522-210">Használatával adja meg a dinamikus **folderPath** és **Fájlnév** idő adatsorozat adatok.</span><span class="sxs-lookup"><span data-stu-id="ae522-210">Used to specify a dynamic **folderPath** and **fileName** for time series data.</span></span> <span data-ttu-id="ae522-211">Megadhat például egy **folderPath** , amely az adatok óránkénti paraméteres.</span><span class="sxs-lookup"><span data-stu-id="ae522-211">For example, you can specify a **folderPath** that is parameterized for every hour of data.</span></span> |<span data-ttu-id="ae522-212">Nem</span><span class="sxs-lookup"><span data-stu-id="ae522-212">No</span></span> |
| <span data-ttu-id="ae522-213">Formátumban</span><span class="sxs-lookup"><span data-stu-id="ae522-213">format</span></span> | <span data-ttu-id="ae522-214">A következő formátumban típusok támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="ae522-214">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="ae522-215">Állítsa be a **típus** tulajdonság a formátuma a következő értékek egyikét.</span><span class="sxs-lookup"><span data-stu-id="ae522-215">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="ae522-216">További információkért lásd: a [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="ae522-216">For more information, see the [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="ae522-217">Ha szeretné átmásolni a fájlokat, mivel ezek között a fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a Formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="ae522-217">If you want to copy files as they are between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="ae522-218">Nem</span><span class="sxs-lookup"><span data-stu-id="ae522-218">No</span></span> |
| <span data-ttu-id="ae522-219">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="ae522-219">compression</span></span> | <span data-ttu-id="ae522-220">Adja meg a típus és az adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="ae522-220">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="ae522-221">Támogatott típusok a következők **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**, és a támogatott szintek a következők **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="ae522-221">Supported types are **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**, and supported levels are **Optimal** and **Fastest**.</span></span> <span data-ttu-id="ae522-222">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="ae522-222">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="ae522-223">Nem</span><span class="sxs-lookup"><span data-stu-id="ae522-223">No</span></span> |
| <span data-ttu-id="ae522-224">useBinaryTransfer</span><span class="sxs-lookup"><span data-stu-id="ae522-224">useBinaryTransfer</span></span> |<span data-ttu-id="ae522-225">Adja meg, hogy a bináris átviteli mód használatára.</span><span class="sxs-lookup"><span data-stu-id="ae522-225">Specify whether to use the binary transfer mode.</span></span> <span data-ttu-id="ae522-226">Az értékek a következők igaz a bináris mód (Ez az alapértelmezett érték), és hamis értéket ASCII.</span><span class="sxs-lookup"><span data-stu-id="ae522-226">The values are true for binary mode (this is the default value), and false for ASCII.</span></span> <span data-ttu-id="ae522-227">A tulajdonság csak akkor használható, típusú a társított kapcsolódószolgáltatás-típus esetén: FTP-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="ae522-227">This property can only be used when the associated linked service type is of type: FtpServer.</span></span> |<span data-ttu-id="ae522-228">Nem</span><span class="sxs-lookup"><span data-stu-id="ae522-228">No</span></span> |

> [!NOTE]
> <span data-ttu-id="ae522-229">**Fájlnév** és **fileFilter** nem használható egyszerre.</span><span class="sxs-lookup"><span data-stu-id="ae522-229">**fileName** and **fileFilter** cannot be used simultaneously.</span></span>

### <a name="use-the-partionedby-property"></a><span data-ttu-id="ae522-230">A partionedBy tulajdonsággal</span><span class="sxs-lookup"><span data-stu-id="ae522-230">Use the partionedBy property</span></span>
<span data-ttu-id="ae522-231">Az előző szakaszban említett, megadhat egy dinamikus **folderPath** és **Fájlnév** idő adatsorozat adatokhoz a **partitionedBy** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="ae522-231">As mentioned in the previous section, you can specify a dynamic **folderPath** and **fileName** for time series data with the **partitionedBy** property.</span></span>

<span data-ttu-id="ae522-232">Idő adatsorozat adatkészleteket, az ütemezés és a szeletek kapcsolatos további tudnivalókért lásd: [adatkészletek létrehozása](data-factory-create-datasets.md), [ütemezés és a végrehajtás](data-factory-scheduling-and-execution.md), és [folyamatok létrehozása](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="ae522-232">To learn about time series datasets, scheduling, and slices, see [Creating datasets](data-factory-create-datasets.md), [Scheduling and execution](data-factory-scheduling-and-execution.md), and [Creating pipelines](data-factory-create-pipelines.md).</span></span>

#### <a name="sample-1"></a><span data-ttu-id="ae522-233">1. példa</span><span class="sxs-lookup"><span data-stu-id="ae522-233">Sample 1</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
<span data-ttu-id="ae522-234">Ebben a példában {szelet} cseréli a Data Factory rendszer változó SliceStart, megadott formátumban (YYYYMMDDHH) értékét.</span><span class="sxs-lookup"><span data-stu-id="ae522-234">In this example, {Slice} is replaced with the value of Data Factory system variable SliceStart, in the format specified (YYYYMMDDHH).</span></span> <span data-ttu-id="ae522-235">A szelet kezdete a SliceStart hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="ae522-235">The SliceStart refers to start time of the slice.</span></span> <span data-ttu-id="ae522-236">A mappa elérési útja eltér az egyes szeletek.</span><span class="sxs-lookup"><span data-stu-id="ae522-236">The folder path is different for each slice.</span></span> <span data-ttu-id="ae522-237">(Például wikidatagateway/wikisampledataout/2014100103 vagy wikidatagateway/wikisampledataout/2014100104.)</span><span class="sxs-lookup"><span data-stu-id="ae522-237">(For example, wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.)</span></span>

#### <a name="sample-2"></a><span data-ttu-id="ae522-238">2. példa</span><span class="sxs-lookup"><span data-stu-id="ae522-238">Sample 2</span></span>

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
<span data-ttu-id="ae522-239">Ebben a példában év, hónap, nap, és SliceStart idején ki kell olvasni a által használt külön változók a **folderPath** és **Fájlnév** tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="ae522-239">In this example, the year, month, day, and time of SliceStart are extracted into separate variables that are used by the **folderPath** and **fileName** properties.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="ae522-240">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="ae522-240">Copy activity properties</span></span>
<span data-ttu-id="ae522-241">Szakaszok és a rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: [folyamatok létrehozása](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="ae522-241">For a full list of sections and properties available for defining activities, see [Creating pipelines](data-factory-create-pipelines.md).</span></span> <span data-ttu-id="ae522-242">Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti táblák és házirendek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="ae522-242">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="ae522-243">Tulajdonságok érhetők el a **typeProperties** szakasz a tevékenység, másrészt a tevékenységek minden típusának eltérők lehetnek.</span><span class="sxs-lookup"><span data-stu-id="ae522-243">Properties available in the **typeProperties** section of the activity, on the other hand, vary with each activity type.</span></span> <span data-ttu-id="ae522-244">A másolási tevékenységhez a típus tulajdonságokat. az adatforrások és mosdók függenek.</span><span class="sxs-lookup"><span data-stu-id="ae522-244">For the copy activity, the type properties vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="ae522-245">A másolási tevékenység, ha az adatforrás típusú **FileSystemSource**, a következő tulajdonság érhető el **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="ae522-245">In copy activity, when the source is of type **FileSystemSource**, the following property is available in **typeProperties** section:</span></span>

| <span data-ttu-id="ae522-246">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ae522-246">Property</span></span> | <span data-ttu-id="ae522-247">Leírás</span><span class="sxs-lookup"><span data-stu-id="ae522-247">Description</span></span> | <span data-ttu-id="ae522-248">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="ae522-248">Allowed values</span></span> | <span data-ttu-id="ae522-249">Szükséges</span><span class="sxs-lookup"><span data-stu-id="ae522-249">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ae522-250">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="ae522-250">recursive</span></span> |<span data-ttu-id="ae522-251">Azt jelzi, hogy az adatok olvasható rekurzív módon az almappákat, vagy csak a megadott mappát.</span><span class="sxs-lookup"><span data-stu-id="ae522-251">Indicates whether the data is read recursively from the subfolders, or only from the specified folder.</span></span> |<span data-ttu-id="ae522-252">IGAZ, hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="ae522-252">True, False (default)</span></span> |<span data-ttu-id="ae522-253">Nem</span><span class="sxs-lookup"><span data-stu-id="ae522-253">No</span></span> |

## <a name="json-example-copy-data-from-ftp-server-to-azure-blob"></a><span data-ttu-id="ae522-254">JSON-példa: adatok másolása az FTP-kiszolgáló az Azure-Blobba</span><span class="sxs-lookup"><span data-stu-id="ae522-254">JSON example: Copy data from FTP server to Azure Blob</span></span>
<span data-ttu-id="ae522-255">Ez a példa bemutatja, hogyan adatok másolása az FTP-kiszolgálóhoz az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="ae522-255">This sample shows how to copy data from an FTP server to Azure Blob storage.</span></span> <span data-ttu-id="ae522-256">Azonban adatok átmásolhatók közvetlenül a megadott mosdók bármelyikét a [adatokról és formátumok támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats), a másolási tevékenység során a Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="ae522-256">However, data can be copied directly to any of the sinks stated in the [supported data stores and formats](data-factory-data-movement-activities.md#supported-data-stores-and-formats), by using the copy activity in Data Factory.</span></span>  

<span data-ttu-id="ae522-257">Az alábbi példák megadják minta JSON-definíciókat tartalmazzon, segítségével hozzon létre egy folyamatot [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), vagy [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md):</span><span class="sxs-lookup"><span data-stu-id="ae522-257">The following examples provide sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md):</span></span>

* <span data-ttu-id="ae522-258">A társított szolgáltatás típusa [FTP-kiszolgáló](#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="ae522-258">A linked service of type [FtpServer](#linked-service-properties)</span></span>
* <span data-ttu-id="ae522-259">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="ae522-259">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)</span></span>
* <span data-ttu-id="ae522-260">Bemeneti [dataset](data-factory-create-datasets.md) típusú [fájlmegosztás](#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="ae522-260">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties)</span></span>
* <span data-ttu-id="ae522-261">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="ae522-261">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)</span></span>
* <span data-ttu-id="ae522-262">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [FileSystemSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="ae522-262">A [pipeline](data-factory-create-pipelines.md) with copy activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)</span></span>

<span data-ttu-id="ae522-263">A minta másol adatokat az FTP-kiszolgáló egy Azure blob minden órában.</span><span class="sxs-lookup"><span data-stu-id="ae522-263">The sample copies data from an FTP server to an Azure blob every hour.</span></span> <span data-ttu-id="ae522-264">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="ae522-264">The JSON properties used in these samples are described in sections following the samples.</span></span>

### <a name="ftp-linked-service"></a><span data-ttu-id="ae522-265">Kapcsolódó FTP-szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="ae522-265">FTP linked service</span></span>

<span data-ttu-id="ae522-266">Ebben a példában a felhasználói nevet és jelszót egyszerű szövegként egyszerű hitelesítést használ.</span><span class="sxs-lookup"><span data-stu-id="ae522-266">This example uses basic authentication, with the user name and password in plain text.</span></span> <span data-ttu-id="ae522-267">Használhatja a következő módszerek valamelyikével:</span><span class="sxs-lookup"><span data-stu-id="ae522-267">You can also use one of the following ways:</span></span>

* <span data-ttu-id="ae522-268">A névtelen hitelesítés</span><span class="sxs-lookup"><span data-stu-id="ae522-268">Anonymous authentication</span></span>
* <span data-ttu-id="ae522-269">Egyszerű hitelesítés titkosított hitelesítő adatokkal</span><span class="sxs-lookup"><span data-stu-id="ae522-269">Basic authentication with encrypted credentials</span></span>
* <span data-ttu-id="ae522-270">FTP-keresztül SSL/TLS (ftps-t)</span><span class="sxs-lookup"><span data-stu-id="ae522-270">FTP over SSL/TLS (FTPS)</span></span>

<span data-ttu-id="ae522-271">Tekintse meg a [FTP társított szolgáltatás](#linked-service-properties) szakasz a különböző típusú hitelesítés használható.</span><span class="sxs-lookup"><span data-stu-id="ae522-271">See the [FTP linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

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
### <a name="azure-storage-linked-service"></a><span data-ttu-id="ae522-272">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="ae522-272">Azure Storage linked service</span></span>

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
### <a name="ftp-input-dataset"></a><span data-ttu-id="ae522-273">FTP-bemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="ae522-273">FTP input dataset</span></span>

<span data-ttu-id="ae522-274">Ez az adatkészlet hivatkozik az FTP-mappa `mysharedfolder` és fájl `test.csv`.</span><span class="sxs-lookup"><span data-stu-id="ae522-274">This dataset refers to the FTP folder `mysharedfolder` and file `test.csv`.</span></span> <span data-ttu-id="ae522-275">A feldolgozási sor átmásolja a fájlt a cél.</span><span class="sxs-lookup"><span data-stu-id="ae522-275">The pipeline copies the file to the destination.</span></span>

<span data-ttu-id="ae522-276">Beállítás **külső** való **igaz** tájékoztatja a Data Factory szolgáltatásnak, hogy az adatkészlet data factoryval való külső, és egy adat-előállító tevékenység nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="ae522-276">Setting **external** to **true** informs the Data Factory service that the dataset is external to the data factory, and is not produced by an activity in the data factory.</span></span>

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

### <a name="azure-blob-output-dataset"></a><span data-ttu-id="ae522-277">Azure Blob kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="ae522-277">Azure Blob output dataset</span></span>

<span data-ttu-id="ae522-278">Adatot ír egy új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="ae522-278">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="ae522-279">A mappa elérési útját a BLOB dinamikusan értékeli ki, az időpontnak a szelet által feldolgozott alapján.</span><span class="sxs-lookup"><span data-stu-id="ae522-279">The folder path for the blob is dynamically evaluated, based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="ae522-280">A mappa elérési útját használja, év, hónap, nap, és a kezdési idő órában részeit.</span><span class="sxs-lookup"><span data-stu-id="ae522-280">The folder path uses the year, month, day, and hours parts of the start time.</span></span>

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


### <a name="a-copy-activity-in-a-pipeline-with-file-system-source-and-blob-sink"></a><span data-ttu-id="ae522-281">A másolási tevékenység során a rendszer forrás- és a blob fájlgyűjtő egy folyamaton belül</span><span class="sxs-lookup"><span data-stu-id="ae522-281">A copy activity in a pipeline with file system source and blob sink</span></span>

<span data-ttu-id="ae522-282">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="ae522-282">The pipeline contains a copy activity that is configured to use the input and output datasets, and is scheduled to run every hour.</span></span> <span data-ttu-id="ae522-283">Az adatcsatorna JSON-definícióból a **forrás** típusúra **FileSystemSource**, és a **fogadó** típusúra **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="ae522-283">In the pipeline JSON definition, the **source** type is set to **FileSystemSource**, and the **sink** type is set to **BlobSink**.</span></span>

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
> <span data-ttu-id="ae522-284">Képezze le a fogadó adatkészletből oszlopok forrás adatkészletből oszlopokat, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="ae522-284">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ae522-285">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ae522-285">Next steps</span></span>
<span data-ttu-id="ae522-286">Lásd az alábbi cikkeket:</span><span class="sxs-lookup"><span data-stu-id="ae522-286">See the following articles:</span></span>

* <span data-ttu-id="ae522-287">Című témakörben olvashat kulcsfontosságú szerepet játszik az adatátvitelt jelölik a (másolási tevékenység) a Data Factory és különböző módokon optimalizálása azt hatás teljesítmény, a [másolása tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="ae522-287">To learn about key factors that impact performance of data movement (copy activity) in Data Factory, and various ways to optimize it, see the [Copy activity performance and tuning guide](data-factory-copy-activity-performance.md).</span></span>

* <span data-ttu-id="ae522-288">A másolási tevékenység során a folyamat létrehozásának részletes leírása, tekintse meg a [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="ae522-288">For step-by-step instructions for creating a pipeline with a copy activity, see the [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
