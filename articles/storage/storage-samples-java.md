---
title: "aaaAzure tárolási minták Java használatával |} Microsoft Docs"
description: "Megtekintése, töltse le és futtassa a mintakódot és az alkalmazások az Azure Storage. Fedezze fel hello Java storage ügyfélkódtáraival használó kódmintát BLOB, üzenetsorok, táblák és fájlok, az első lépések."
services: storage
documentationcenter: na
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 01/12/2017
ms.author: seguler
ms.openlocfilehash: e3b8fbe86e82dd58c2a13a3c68760cbf6e9a6e4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-samples-using-java"></a><span data-ttu-id="6f834-104">Azure Storage-minták javás környezetekben</span><span class="sxs-lookup"><span data-stu-id="6f834-104">Azure Storage samples using Java</span></span>

## <a name="java-sample-index"></a><span data-ttu-id="6f834-105">Java-sample index</span><span class="sxs-lookup"><span data-stu-id="6f834-105">Java sample index</span></span>

<span data-ttu-id="6f834-106">hello alábbi táblázat áttekintést nyújt az általunk biztosított mintákat tárház és hello forgatókönyvben szereplő minden egyes minta.</span><span class="sxs-lookup"><span data-stu-id="6f834-106">hello following table provides an overview of our samples repository and hello scenarios covered in each sample.</span></span> <span data-ttu-id="6f834-107">Kattintson a hello hivatkozások tooview hello megfelelő mintakód a Githubon.</span><span class="sxs-lookup"><span data-stu-id="6f834-107">Click on hello links tooview hello corresponding sample code in GitHub.</span></span>

<table style="font-size:90%"><thead><tr><th style="font-size:110%"><span data-ttu-id="6f834-108">Végpont</span><span class="sxs-lookup"><span data-stu-id="6f834-108">Endpoint</span></span></th><th style="font-size:110%"><span data-ttu-id="6f834-109">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="6f834-109">Scenario</span></span></th><th style="font-size:110%"><span data-ttu-id="6f834-110">Mintakód</span><span class="sxs-lookup"><span data-stu-id="6f834-110">Sample Code</span></span></th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><span data-ttu-id="6f834-111"><b>A BLOB</b></span><span class="sxs-lookup"><span data-stu-id="6f834-111"><b>Blob</b></span></span></td>
<td><span data-ttu-id="6f834-112">Hozzáfűző Blob</span><span class="sxs-lookup"><span data-stu-id="6f834-112">Append Blob</span></span></td> 
<td><span data-ttu-id="6f834-113"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-113"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="6f834-114">Blokkblob</span><span class="sxs-lookup"><span data-stu-id="6f834-114">Block Blob</span></span></td>
<td><span data-ttu-id="6f834-115"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-115"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="6f834-116">Ügyféloldali titkosítás</span><span class="sxs-lookup"><span data-stu-id="6f834-116">Client-Side Encryption</span></span></td>
<td><span data-ttu-id="6f834-117"><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Ismerkedés az Azure ügyféloldali titkosítása Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-117"><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Getting Started with Azure Client Side Encryption in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="6f834-118">A Blob másolása</span><span class="sxs-lookup"><span data-stu-id="6f834-118">Copy Blob</span></span></td>
<td><span data-ttu-id="6f834-119"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-119"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="6f834-120">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="6f834-120">Create Container</span></span></td>
<td><span data-ttu-id="6f834-121"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-121"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="6f834-122">A Blob törlése</span><span class="sxs-lookup"><span data-stu-id="6f834-122">Delete Blob</span></span></td>
<td><span data-ttu-id="6f834-123"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-123"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="6f834-124">Törli a tárolót</span><span class="sxs-lookup"><span data-stu-id="6f834-124">Delete Container</span></span></td>
<td><span data-ttu-id="6f834-125"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-125"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="6f834-126">Metaadatok/tulajdonságok/Stats BLOB</span><span class="sxs-lookup"><span data-stu-id="6f834-126">Blob Metadata/Properties/Stats</span></span></td>
<td><span data-ttu-id="6f834-127"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-127"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="6f834-128">A tároló hozzáférés-vezérlési lista/metaadatok/tulajdonságainak</span><span class="sxs-lookup"><span data-stu-id="6f834-128">Container ACL/Metadata/Properties</span></span></td>
<td><span data-ttu-id="6f834-129"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-129"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="6f834-130">Get tartományokat</span><span class="sxs-lookup"><span data-stu-id="6f834-130">Get Page Ranges</span></span></td>
<td><span data-ttu-id="6f834-131"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/CloudPageBlobTests.java">Az oldalakra vonatkozó Blob tesztek minta</a></span><span class="sxs-lookup"><span data-stu-id="6f834-131"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/CloudPageBlobTests.java">Page Blob Tests Sample</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="6f834-132">Bérbe Blobtárolóban /</span><span class="sxs-lookup"><span data-stu-id="6f834-132">Lease Blob/Container</span></span></td>
<td><span data-ttu-id="6f834-133"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-133"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="6f834-134">Lista Blobtárolóban</span><span class="sxs-lookup"><span data-stu-id="6f834-134">List Blob/Container</span></span></td>
<td><span data-ttu-id="6f834-135"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-135"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="6f834-136">Az oldalakra vonatkozó Blob</span><span class="sxs-lookup"><span data-stu-id="6f834-136">Page Blob</span></span></td>
<td><span data-ttu-id="6f834-137"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-137"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr>
<tr> 
<td><span data-ttu-id="6f834-138">SAS</span><span class="sxs-lookup"><span data-stu-id="6f834-138">SAS</span></span></td>
<td><span data-ttu-id="6f834-139"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/SasTests.java">SAS-vizsgálatok minta</a></span><span class="sxs-lookup"><span data-stu-id="6f834-139"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/SasTests.java">SAS Tests Sample</a></span></span></td>
</tr>   
<tr> 
<td><span data-ttu-id="6f834-140">Szolgáltatás tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="6f834-140">Service Properties</span></span></td>
<td><span data-ttu-id="6f834-141"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-141"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr>           
<tr> 
<td><span data-ttu-id="6f834-142">Pillanatkép Blob</span><span class="sxs-lookup"><span data-stu-id="6f834-142">Snapshot Blob</span></span></td>
<td><span data-ttu-id="6f834-143"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-143"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td rowspan="9"><span data-ttu-id="6f834-144"><b>Fájl</b></span><span class="sxs-lookup"><span data-stu-id="6f834-144"><b>File</b></span></span></td>
<td><span data-ttu-id="6f834-145">Megosztás-könyvtárak-fájlok létrehozása</span><span class="sxs-lookup"><span data-stu-id="6f834-145">Create Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="6f834-146"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Ismerkedés az Azure File szolgáltatással Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-146"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="6f834-147">Megosztások/könyvtárak/fájlok törlése</span><span class="sxs-lookup"><span data-stu-id="6f834-147">Delete Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="6f834-148"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Ismerkedés az Azure File szolgáltatással Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-148"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="6f834-149">Directory tulajdonságok/metaadatok</span><span class="sxs-lookup"><span data-stu-id="6f834-149">Directory Properties/Metadata</span></span></td> 
<td><span data-ttu-id="6f834-150"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Ismerkedés az Azure File szolgáltatással Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-150"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="6f834-151">Fájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="6f834-151">Download Files</span></span></td> 
<td><span data-ttu-id="6f834-152"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Ismerkedés az Azure File szolgáltatással Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-152"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="6f834-153">A fájl tulajdonságok/metaadatok/metrikák</span><span class="sxs-lookup"><span data-stu-id="6f834-153">File Properties/Metadata/Metrics</span></span></td> 
<td><span data-ttu-id="6f834-154"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Ismerkedés az Azure File szolgáltatással Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-154"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="6f834-155">Fájlok szolgáltatás tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="6f834-155">File Service Properties</span></span></td> 
<td><span data-ttu-id="6f834-156"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Ismerkedés az Azure File szolgáltatással Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-156"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="6f834-157">Fájlok és könyvtárak listája</span><span class="sxs-lookup"><span data-stu-id="6f834-157">List Directories and Files</span></span></td> 
<td><span data-ttu-id="6f834-158"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Ismerkedés az Azure File szolgáltatással Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-158"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="6f834-159">Lista megosztások</span><span class="sxs-lookup"><span data-stu-id="6f834-159">List Shares</span></span></td> 
<td><span data-ttu-id="6f834-160"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Ismerkedés az Azure File szolgáltatással Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-160"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="6f834-161">Tulajdonságok/metaadatok/Stats megosztás</span><span class="sxs-lookup"><span data-stu-id="6f834-161">Share Properties/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="6f834-162"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Ismerkedés az Azure File szolgáltatással Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-162"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td rowspan="8"><span data-ttu-id="6f834-163"><b>Várólista</b></span><span class="sxs-lookup"><span data-stu-id="6f834-163"><b>Queue</b></span></span></td>
<td><span data-ttu-id="6f834-164">Üzenet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6f834-164">Add Message</span></span></td> 
<td><span data-ttu-id="6f834-165"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/queue/gettingstarted/QueueBasics.java">Tárolási Java ügyfél függvénytár – minták</a></span><span class="sxs-lookup"><span data-stu-id="6f834-165"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/queue/gettingstarted/QueueBasics.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="6f834-166">Ügyféloldali titkosítás</span><span class="sxs-lookup"><span data-stu-id="6f834-166">Client-Side Encryption</span></span></td> 
<td><span data-ttu-id="6f834-167"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/encryption/queue/gettingstarted/QueueGettingStarted.java">Tárolási Java ügyfél függvénytár – minták</a></span><span class="sxs-lookup"><span data-stu-id="6f834-167"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/encryption/queue/gettingstarted/QueueGettingStarted.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="6f834-168">Várólista létrehozása</span><span class="sxs-lookup"><span data-stu-id="6f834-168">Create Queues</span></span></td> 
<td><span data-ttu-id="6f834-169"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Ismerkedés az Azure Queue szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-169"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="6f834-170">Az üzenet-várólista törlése</span><span class="sxs-lookup"><span data-stu-id="6f834-170">Delete Message/Queue</span></span></td> 
<td><span data-ttu-id="6f834-171"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Ismerkedés az Azure Queue szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-171"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="6f834-172">Üzenet megtekintése</span><span class="sxs-lookup"><span data-stu-id="6f834-172">Peek Message</span></span></td> 
<td><span data-ttu-id="6f834-173"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Ismerkedés az Azure Queue szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-173"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="6f834-174">Hozzáférés-vezérlési lista/metaadatok/Stats várólista</span><span class="sxs-lookup"><span data-stu-id="6f834-174">Queue ACL/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="6f834-175"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Ismerkedés az Azure Queue szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-175"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="6f834-176">Várólista szolgáltatás tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="6f834-176">Queue Service Properties</span></span></td> 
<td><span data-ttu-id="6f834-177"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Ismerkedés az Azure Queue szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-177"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="6f834-178">Üzenet frissítése</span><span class="sxs-lookup"><span data-stu-id="6f834-178">Update Message</span></span></td> 
<td><span data-ttu-id="6f834-179"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Ismerkedés az Azure Queue szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-179"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td rowspan="7"><span data-ttu-id="6f834-180"><b>Tábla</b></span><span class="sxs-lookup"><span data-stu-id="6f834-180"><b>Table</b></span></span></td>
<td><span data-ttu-id="6f834-181">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="6f834-181">Create Table</span></span></td> 
<td><span data-ttu-id="6f834-182"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Ismerkedés az Azure Table szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-182"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="6f834-183">Entitás vagy a tábla törlése</span><span class="sxs-lookup"><span data-stu-id="6f834-183">Delete Entity/Table</span></span></td> 
<td><span data-ttu-id="6f834-184"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Ismerkedés az Azure Table szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-184"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="6f834-185">Entitás beszúrása és egyesítési/cseréje</span><span class="sxs-lookup"><span data-stu-id="6f834-185">Insert/Merge/Replace Entity</span></span></td> 
<td><span data-ttu-id="6f834-186"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Tárolási Java ügyfél függvénytár – minták</a></span><span class="sxs-lookup"><span data-stu-id="6f834-186"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="6f834-187">Lekérdezés entitások</span><span class="sxs-lookup"><span data-stu-id="6f834-187">Query Entities</span></span></td> 
<td><span data-ttu-id="6f834-188"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Ismerkedés az Azure Table szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-188"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="6f834-189">Lekérdezés táblák</span><span class="sxs-lookup"><span data-stu-id="6f834-189">Query Tables</span></span></td> 
<td><span data-ttu-id="6f834-190"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Ismerkedés az Azure Table szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-190"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="6f834-191">Táblázat ACL/tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="6f834-191">Table ACL/Properties</span></span></td> 
<td><span data-ttu-id="6f834-192"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableAdvanced.java">Ismerkedés az Azure Table szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="6f834-192"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableAdvanced.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="6f834-193">Entitás frissítése</span><span class="sxs-lookup"><span data-stu-id="6f834-193">Update Entity</span></span></td> 
<td><span data-ttu-id="6f834-194"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Tárolási Java ügyfél függvénytár – minták</a></span><span class="sxs-lookup"><span data-stu-id="6f834-194"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a><span data-ttu-id="6f834-195">Az Azure mintakódok könyvtár</span><span class="sxs-lookup"><span data-stu-id="6f834-195">Azure Code Samples library</span></span>

<span data-ttu-id="6f834-196">tooview hello teljes mintát könyvtár, nyissa meg toohello [Azure mintakódok](https://azure.microsoft.com/resources/samples/?service=storage) könyvtárban, és azt az Azure Storage töltse le és futtassa helyileg minták tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6f834-196">tooview hello complete sample library, go toohello [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=storage) library, which includes samples for Azure Storage that you can download and run locally.</span></span> <span data-ttu-id="6f834-197">hello kód a minta könyvtár .zip formátumban példakódot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6f834-197">hello Code Sample Library provides sample code in .zip format.</span></span> <span data-ttu-id="6f834-198">Azt is megteheti keresse meg, és minden egyes minta hello GitHub tárház klónozása.</span><span class="sxs-lookup"><span data-stu-id="6f834-198">Alternatively, you can browse and clone hello GitHub repository for each sample.</span></span>

[!INCLUDE [storage-java-samples-include](../../includes/storage-java-samples-include.md)]

## <a name="getting-started-guides"></a><span data-ttu-id="6f834-199">Első lépéseket ismertető útmutatók</span><span class="sxs-lookup"><span data-stu-id="6f834-199">Getting started guides</span></span>

<span data-ttu-id="6f834-200">Tekintse meg a következő útmutatók, ha olyan eszközökre vonatkozóan törlésének hello tooinstall és Ismerkedés az Azure Storage Ügyfélkódtáraival hello.</span><span class="sxs-lookup"><span data-stu-id="6f834-200">Check out hello following guides if you are looking for instructions on how tooinstall and get started with hello Azure Storage Client Libraries.</span></span>

* [<span data-ttu-id="6f834-201">Ismerkedés az Azure Blob szolgáltatás Java nyelven</span><span class="sxs-lookup"><span data-stu-id="6f834-201">Getting Started with Azure Blob Service in Java</span></span>](storage-java-how-to-use-blob-storage.md)
* [<span data-ttu-id="6f834-202">Ismerkedés az Azure Queue szolgáltatás Java nyelven</span><span class="sxs-lookup"><span data-stu-id="6f834-202">Getting Started with Azure Queue Service in Java</span></span>](storage-java-how-to-use-queue-storage.md)
* [<span data-ttu-id="6f834-203">Ismerkedés az Azure Table szolgáltatás Java nyelven</span><span class="sxs-lookup"><span data-stu-id="6f834-203">Getting Started with Azure Table Service in Java</span></span>](storage-java-how-to-use-table-storage.md)
* [<span data-ttu-id="6f834-204">Ismerkedés az Azure File szolgáltatással Java nyelven</span><span class="sxs-lookup"><span data-stu-id="6f834-204">Getting Started with Azure File Service in Java</span></span>](storage-java-how-to-use-file-storage.md)

## <a name="next-steps"></a><span data-ttu-id="6f834-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6f834-205">Next steps</span></span>

<span data-ttu-id="6f834-206">Egyéb nyelvek minták olvashat:</span><span class="sxs-lookup"><span data-stu-id="6f834-206">For information on samples for other languages:</span></span>

* <span data-ttu-id="6f834-207">.NET: [.NET használatával az azure Storage-minták](storage-samples-dotnet.md)</span><span class="sxs-lookup"><span data-stu-id="6f834-207">.NET: [Azure Storage samples using .NET](storage-samples-dotnet.md)</span></span>
* <span data-ttu-id="6f834-208">Minden más nyelv: [Azure Storage-minták](storage-samples.md)</span><span class="sxs-lookup"><span data-stu-id="6f834-208">All other languages: [Azure Storage samples](storage-samples.md)</span></span>
