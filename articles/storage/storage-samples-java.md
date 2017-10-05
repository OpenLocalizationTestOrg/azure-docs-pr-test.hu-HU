---
title: "Azure Storage-minták Java használatával |} Microsoft Docs"
description: "Megtekintése, töltse le és futtassa a mintakódot és az alkalmazások az Azure Storage. Fedezze fel bevezetés minták BLOB, üzenetsorok, táblák és fájlok, a Java storage ügyfélkódtáraival használatával."
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
ms.openlocfilehash: 98e6022062b4ef5b5c71b54a0e94775b925d216b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-storage-samples-using-java"></a><span data-ttu-id="ecc45-104">Azure Storage-minták javás környezetekben</span><span class="sxs-lookup"><span data-stu-id="ecc45-104">Azure Storage samples using Java</span></span>

## <a name="java-sample-index"></a><span data-ttu-id="ecc45-105">Java-sample index</span><span class="sxs-lookup"><span data-stu-id="ecc45-105">Java sample index</span></span>

<span data-ttu-id="ecc45-106">A következő táblázat a minták tárház és minden egyes minta az ismertetett forgatókönyvek áttekintése.</span><span class="sxs-lookup"><span data-stu-id="ecc45-106">The following table provides an overview of our samples repository and the scenarios covered in each sample.</span></span> <span data-ttu-id="ecc45-107">Kattintson a hivatkozásra kattintva a megfelelő mintakód a Githubon.</span><span class="sxs-lookup"><span data-stu-id="ecc45-107">Click on the links to view the corresponding sample code in GitHub.</span></span>

<table style="font-size:90%"><thead><tr><th style="font-size:110%"><span data-ttu-id="ecc45-108">Végpont</span><span class="sxs-lookup"><span data-stu-id="ecc45-108">Endpoint</span></span></th><th style="font-size:110%"><span data-ttu-id="ecc45-109">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="ecc45-109">Scenario</span></span></th><th style="font-size:110%"><span data-ttu-id="ecc45-110">Mintakód</span><span class="sxs-lookup"><span data-stu-id="ecc45-110">Sample Code</span></span></th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><span data-ttu-id="ecc45-111"><b>A BLOB</b></span><span class="sxs-lookup"><span data-stu-id="ecc45-111"><b>Blob</b></span></span></td>
<td><span data-ttu-id="ecc45-112">Hozzáfűző Blob</span><span class="sxs-lookup"><span data-stu-id="ecc45-112">Append Blob</span></span></td> 
<td><span data-ttu-id="ecc45-113"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-113"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-114">Blokkblob</span><span class="sxs-lookup"><span data-stu-id="ecc45-114">Block Blob</span></span></td>
<td><span data-ttu-id="ecc45-115"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-115"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-116">Ügyféloldali titkosítás</span><span class="sxs-lookup"><span data-stu-id="ecc45-116">Client-Side Encryption</span></span></td>
<td><span data-ttu-id="ecc45-117"><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Ismerkedés az Azure ügyféloldali titkosítása Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-117"><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Getting Started with Azure Client Side Encryption in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-118">A Blob másolása</span><span class="sxs-lookup"><span data-stu-id="ecc45-118">Copy Blob</span></span></td>
<td><span data-ttu-id="ecc45-119"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-119"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-120">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="ecc45-120">Create Container</span></span></td>
<td><span data-ttu-id="ecc45-121"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-121"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-122">A Blob törlése</span><span class="sxs-lookup"><span data-stu-id="ecc45-122">Delete Blob</span></span></td>
<td><span data-ttu-id="ecc45-123"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-123"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-124">Törli a tárolót</span><span class="sxs-lookup"><span data-stu-id="ecc45-124">Delete Container</span></span></td>
<td><span data-ttu-id="ecc45-125"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-125"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-126">Metaadatok/tulajdonságok/Stats BLOB</span><span class="sxs-lookup"><span data-stu-id="ecc45-126">Blob Metadata/Properties/Stats</span></span></td>
<td><span data-ttu-id="ecc45-127"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-127"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-128">A tároló hozzáférés-vezérlési lista/metaadatok/tulajdonságainak</span><span class="sxs-lookup"><span data-stu-id="ecc45-128">Container ACL/Metadata/Properties</span></span></td>
<td><span data-ttu-id="ecc45-129"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-129"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-130">Get tartományokat</span><span class="sxs-lookup"><span data-stu-id="ecc45-130">Get Page Ranges</span></span></td>
<td><span data-ttu-id="ecc45-131"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/CloudPageBlobTests.java">Az oldalakra vonatkozó Blob tesztek minta</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-131"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/CloudPageBlobTests.java">Page Blob Tests Sample</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-132">Bérbe Blobtárolóban /</span><span class="sxs-lookup"><span data-stu-id="ecc45-132">Lease Blob/Container</span></span></td>
<td><span data-ttu-id="ecc45-133"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-133"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-134">Lista Blobtárolóban</span><span class="sxs-lookup"><span data-stu-id="ecc45-134">List Blob/Container</span></span></td>
<td><span data-ttu-id="ecc45-135"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-135"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-136">Az oldalakra vonatkozó Blob</span><span class="sxs-lookup"><span data-stu-id="ecc45-136">Page Blob</span></span></td>
<td><span data-ttu-id="ecc45-137"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-137"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr>
<tr> 
<td><span data-ttu-id="ecc45-138">SAS</span><span class="sxs-lookup"><span data-stu-id="ecc45-138">SAS</span></span></td>
<td><span data-ttu-id="ecc45-139"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/SasTests.java">SAS-vizsgálatok minta</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-139"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/SasTests.java">SAS Tests Sample</a></span></span></td>
</tr>   
<tr> 
<td><span data-ttu-id="ecc45-140">Szolgáltatás tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="ecc45-140">Service Properties</span></span></td>
<td><span data-ttu-id="ecc45-141"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-141"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr>           
<tr> 
<td><span data-ttu-id="ecc45-142">Pillanatkép Blob</span><span class="sxs-lookup"><span data-stu-id="ecc45-142">Snapshot Blob</span></span></td>
<td><span data-ttu-id="ecc45-143"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Ismerkedés az Azure Blob szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-143"><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Getting Started with Azure Blob Service in Java</a></span></span></td>
</tr> 
<tr> 
<td rowspan="9"><span data-ttu-id="ecc45-144"><b>Fájl</b></span><span class="sxs-lookup"><span data-stu-id="ecc45-144"><b>File</b></span></span></td>
<td><span data-ttu-id="ecc45-145">Megosztás-könyvtárak-fájlok létrehozása</span><span class="sxs-lookup"><span data-stu-id="ecc45-145">Create Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="ecc45-146"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Ismerkedés az Azure File szolgáltatással Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-146"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="ecc45-147">Megosztások/könyvtárak/fájlok törlése</span><span class="sxs-lookup"><span data-stu-id="ecc45-147">Delete Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="ecc45-148"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Ismerkedés az Azure File szolgáltatással Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-148"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-149">Directory tulajdonságok/metaadatok</span><span class="sxs-lookup"><span data-stu-id="ecc45-149">Directory Properties/Metadata</span></span></td> 
<td><span data-ttu-id="ecc45-150"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Ismerkedés az Azure File szolgáltatással Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-150"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-151">Fájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="ecc45-151">Download Files</span></span></td> 
<td><span data-ttu-id="ecc45-152"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Ismerkedés az Azure File szolgáltatással Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-152"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-153">A fájl tulajdonságok/metaadatok/metrikák</span><span class="sxs-lookup"><span data-stu-id="ecc45-153">File Properties/Metadata/Metrics</span></span></td> 
<td><span data-ttu-id="ecc45-154"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Ismerkedés az Azure File szolgáltatással Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-154"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-155">Fájlok szolgáltatás tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="ecc45-155">File Service Properties</span></span></td> 
<td><span data-ttu-id="ecc45-156"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Ismerkedés az Azure File szolgáltatással Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-156"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-157">Fájlok és könyvtárak listája</span><span class="sxs-lookup"><span data-stu-id="ecc45-157">List Directories and Files</span></span></td> 
<td><span data-ttu-id="ecc45-158"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Ismerkedés az Azure File szolgáltatással Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-158"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="ecc45-159">Lista megosztások</span><span class="sxs-lookup"><span data-stu-id="ecc45-159">List Shares</span></span></td> 
<td><span data-ttu-id="ecc45-160"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Ismerkedés az Azure File szolgáltatással Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-160"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="ecc45-161">Tulajdonságok/metaadatok/Stats megosztás</span><span class="sxs-lookup"><span data-stu-id="ecc45-161">Share Properties/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="ecc45-162"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Ismerkedés az Azure File szolgáltatással Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-162"><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Getting Started with Azure File Service in Java</a></span></span></td> 
</tr>
<tr> 
<td rowspan="8"><span data-ttu-id="ecc45-163"><b>Várólista</b></span><span class="sxs-lookup"><span data-stu-id="ecc45-163"><b>Queue</b></span></span></td>
<td><span data-ttu-id="ecc45-164">Üzenet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ecc45-164">Add Message</span></span></td> 
<td><span data-ttu-id="ecc45-165"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/queue/gettingstarted/QueueBasics.java">Tárolási Java ügyfél függvénytár – minták</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-165"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/queue/gettingstarted/QueueBasics.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-166">Ügyféloldali titkosítás</span><span class="sxs-lookup"><span data-stu-id="ecc45-166">Client-Side Encryption</span></span></td> 
<td><span data-ttu-id="ecc45-167"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/encryption/queue/gettingstarted/QueueGettingStarted.java">Tárolási Java ügyfél függvénytár – minták</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-167"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/encryption/queue/gettingstarted/QueueGettingStarted.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-168">Várólista létrehozása</span><span class="sxs-lookup"><span data-stu-id="ecc45-168">Create Queues</span></span></td> 
<td><span data-ttu-id="ecc45-169"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Ismerkedés az Azure Queue szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-169"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-170">Az üzenet-várólista törlése</span><span class="sxs-lookup"><span data-stu-id="ecc45-170">Delete Message/Queue</span></span></td> 
<td><span data-ttu-id="ecc45-171"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Ismerkedés az Azure Queue szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-171"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-172">Üzenet megtekintése</span><span class="sxs-lookup"><span data-stu-id="ecc45-172">Peek Message</span></span></td> 
<td><span data-ttu-id="ecc45-173"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Ismerkedés az Azure Queue szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-173"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-174">Hozzáférés-vezérlési lista/metaadatok/Stats várólista</span><span class="sxs-lookup"><span data-stu-id="ecc45-174">Queue ACL/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="ecc45-175"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Ismerkedés az Azure Queue szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-175"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-176">Várólista szolgáltatás tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="ecc45-176">Queue Service Properties</span></span></td> 
<td><span data-ttu-id="ecc45-177"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Ismerkedés az Azure Queue szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-177"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-178">Üzenet frissítése</span><span class="sxs-lookup"><span data-stu-id="ecc45-178">Update Message</span></span></td> 
<td><span data-ttu-id="ecc45-179"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Ismerkedés az Azure Queue szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-179"><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Getting Started with Azure Queue Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td rowspan="7"><span data-ttu-id="ecc45-180"><b>Tábla</b></span><span class="sxs-lookup"><span data-stu-id="ecc45-180"><b>Table</b></span></span></td>
<td><span data-ttu-id="ecc45-181">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="ecc45-181">Create Table</span></span></td> 
<td><span data-ttu-id="ecc45-182"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Ismerkedés az Azure Table szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-182"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-183">Entitás vagy a tábla törlése</span><span class="sxs-lookup"><span data-stu-id="ecc45-183">Delete Entity/Table</span></span></td> 
<td><span data-ttu-id="ecc45-184"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Ismerkedés az Azure Table szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-184"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-185">Entitás beszúrása és egyesítési/cseréje</span><span class="sxs-lookup"><span data-stu-id="ecc45-185">Insert/Merge/Replace Entity</span></span></td> 
<td><span data-ttu-id="ecc45-186"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Tárolási Java ügyfél függvénytár – minták</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-186"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-187">Lekérdezés entitások</span><span class="sxs-lookup"><span data-stu-id="ecc45-187">Query Entities</span></span></td> 
<td><span data-ttu-id="ecc45-188"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Ismerkedés az Azure Table szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-188"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-189">Lekérdezés táblák</span><span class="sxs-lookup"><span data-stu-id="ecc45-189">Query Tables</span></span></td> 
<td><span data-ttu-id="ecc45-190"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Ismerkedés az Azure Table szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-190"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-191">Táblázat ACL/tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="ecc45-191">Table ACL/Properties</span></span></td> 
<td><span data-ttu-id="ecc45-192"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableAdvanced.java">Ismerkedés az Azure Table szolgáltatás Java nyelven</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-192"><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableAdvanced.java">Getting Started with Azure Table Service in Java</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ecc45-193">Entitás frissítése</span><span class="sxs-lookup"><span data-stu-id="ecc45-193">Update Entity</span></span></td> 
<td><span data-ttu-id="ecc45-194"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Tárolási Java ügyfél függvénytár – minták</a></span><span class="sxs-lookup"><span data-stu-id="ecc45-194"><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Storage Java Client Library Samples</a></span></span></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a><span data-ttu-id="ecc45-195">Az Azure mintakódok könyvtár</span><span class="sxs-lookup"><span data-stu-id="ecc45-195">Azure Code Samples library</span></span>

<span data-ttu-id="ecc45-196">A teljes minta könyvtár megtekintéséhez keresse fel a [Azure mintakódok](https://azure.microsoft.com/resources/samples/?service=storage) könyvtárban, és azt az Azure Storage töltse le és futtassa helyileg minták tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ecc45-196">To view the complete sample library, go to the [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=storage) library, which includes samples for Azure Storage that you can download and run locally.</span></span> <span data-ttu-id="ecc45-197">A kód a minta könyvtár .zip formátumban példakódot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ecc45-197">The Code Sample Library provides sample code in .zip format.</span></span> <span data-ttu-id="ecc45-198">Azt is megteheti keresse meg és a GitHub-tárházban, minden egyes minta klónozását.</span><span class="sxs-lookup"><span data-stu-id="ecc45-198">Alternatively, you can browse and clone the GitHub repository for each sample.</span></span>

[!INCLUDE [storage-java-samples-include](../../includes/storage-java-samples-include.md)]

## <a name="getting-started-guides"></a><span data-ttu-id="ecc45-199">Első lépéseket ismertető útmutatók</span><span class="sxs-lookup"><span data-stu-id="ecc45-199">Getting started guides</span></span>

<span data-ttu-id="ecc45-200">Tekintse meg az alábbi útmutatókban, ha a telepítését, és ismerkedjen meg az Azure Storage Ügyfélkódtáraival útmutatást.</span><span class="sxs-lookup"><span data-stu-id="ecc45-200">Check out the following guides if you are looking for instructions on how to install and get started with the Azure Storage Client Libraries.</span></span>

* [<span data-ttu-id="ecc45-201">Ismerkedés az Azure Blob szolgáltatás Java nyelven</span><span class="sxs-lookup"><span data-stu-id="ecc45-201">Getting Started with Azure Blob Service in Java</span></span>](storage-java-how-to-use-blob-storage.md)
* [<span data-ttu-id="ecc45-202">Ismerkedés az Azure Queue szolgáltatás Java nyelven</span><span class="sxs-lookup"><span data-stu-id="ecc45-202">Getting Started with Azure Queue Service in Java</span></span>](storage-java-how-to-use-queue-storage.md)
* [<span data-ttu-id="ecc45-203">Ismerkedés az Azure Table szolgáltatás Java nyelven</span><span class="sxs-lookup"><span data-stu-id="ecc45-203">Getting Started with Azure Table Service in Java</span></span>](storage-java-how-to-use-table-storage.md)
* [<span data-ttu-id="ecc45-204">Ismerkedés az Azure File szolgáltatással Java nyelven</span><span class="sxs-lookup"><span data-stu-id="ecc45-204">Getting Started with Azure File Service in Java</span></span>](storage-java-how-to-use-file-storage.md)

## <a name="next-steps"></a><span data-ttu-id="ecc45-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ecc45-205">Next steps</span></span>

<span data-ttu-id="ecc45-206">Egyéb nyelvek minták olvashat:</span><span class="sxs-lookup"><span data-stu-id="ecc45-206">For information on samples for other languages:</span></span>

* <span data-ttu-id="ecc45-207">.NET: [.NET használatával az azure Storage-minták](storage-samples-dotnet.md)</span><span class="sxs-lookup"><span data-stu-id="ecc45-207">.NET: [Azure Storage samples using .NET](storage-samples-dotnet.md)</span></span>
* <span data-ttu-id="ecc45-208">Minden más nyelv: [Azure Storage-minták](storage-samples.md)</span><span class="sxs-lookup"><span data-stu-id="ecc45-208">All other languages: [Azure Storage samples](storage-samples.md)</span></span>
