---
title: "Azure Storage-minták .NET használatával |} Microsoft Docs"
description: "Megtekintése, töltse le és futtassa a mintakódot és az alkalmazások az Azure Storage. Felderíteni a bevezetés minták BLOB, üzenetsorok, táblák és fájlok, a .NET-storage ügyfélkódtáraival használatával."
services: storage
documentationcenter: na
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 01/12/2017
ms.author: seguler
ms.openlocfilehash: d2b6b3d9483f230ad25ae47255a4f28c1a67e064
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-storage-samples-using-net"></a><span data-ttu-id="ba7c4-104">Azure Storage-minták .NET használatával</span><span class="sxs-lookup"><span data-stu-id="ba7c4-104">Azure Storage samples using .NET</span></span>

## <a name="net-sample-index"></a><span data-ttu-id="ba7c4-105">.NET minta index</span><span class="sxs-lookup"><span data-stu-id="ba7c4-105">.NET sample index</span></span>

<span data-ttu-id="ba7c4-106">A következő táblázat a minták tárház és minden egyes minta az ismertetett forgatókönyvek áttekintése.</span><span class="sxs-lookup"><span data-stu-id="ba7c4-106">The following table provides an overview of our samples repository and the scenarios covered in each sample.</span></span> <span data-ttu-id="ba7c4-107">Kattintson a hivatkozásra kattintva a megfelelő mintakód a Githubon.</span><span class="sxs-lookup"><span data-stu-id="ba7c4-107">Click on the links to view the corresponding sample code in GitHub.</span></span>

<table style="font-size:90%"><thead><tr><th style="font-size:110%"><span data-ttu-id="ba7c4-108">Végpont</span><span class="sxs-lookup"><span data-stu-id="ba7c4-108">Endpoint</span></span></th><th style="font-size:110%"><span data-ttu-id="ba7c4-109">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="ba7c4-109">Scenario</span></span></th><th style="font-size:110%"><span data-ttu-id="ba7c4-110">Mintakód</span><span class="sxs-lookup"><span data-stu-id="ba7c4-110">Sample Code</span></span></th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><span data-ttu-id="ba7c4-111"><b>A BLOB</b></span><span class="sxs-lookup"><span data-stu-id="ba7c4-111"><b>Blob</b></span></span></td>
<td><span data-ttu-id="ba7c4-112">Hozzáfűző Blob</span><span class="sxs-lookup"><span data-stu-id="ba7c4-112">Append Blob</span></span></td> 
<td><span data-ttu-id="ba7c4-113"><a href="https://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.getappendblobreference.aspx">CloudBlobContainer.GetAppendBlobReference módszer – példa</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-113"><a href="https://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.getappendblobreference.aspx">CloudBlobContainer.GetAppendBlobReference Method Example</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-114">Blokkblob</span><span class="sxs-lookup"><span data-stu-id="ba7c4-114">Block Blob</span></span></td>
<td><span data-ttu-id="ba7c4-115"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage fotótár webalkalmazás</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-115"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-116">Ügyféloldali titkosítás</span><span class="sxs-lookup"><span data-stu-id="ba7c4-116">Client-Side Encryption</span></span></td>
<td><span data-ttu-id="ba7c4-117"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/BlobGettingStarted/Program.cs">A BLOB titkosítási minták</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-117"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/BlobGettingStarted/Program.cs">Blob Encryption Samples</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-118">A Blob másolása</span><span class="sxs-lookup"><span data-stu-id="ba7c4-118">Copy Blob</span></span></td>
<td><span data-ttu-id="ba7c4-119"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">A BLOB első lépések</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-119"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-120">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="ba7c4-120">Create Container</span></span></td>
<td><span data-ttu-id="ba7c4-121"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage fotótár webalkalmazás</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-121"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-122">A Blob törlése</span><span class="sxs-lookup"><span data-stu-id="ba7c4-122">Delete Blob</span></span></td>
<td><span data-ttu-id="ba7c4-123"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage fotótár webalkalmazás</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-123"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-124">Törli a tárolót</span><span class="sxs-lookup"><span data-stu-id="ba7c4-124">Delete Container</span></span></td>
<td><span data-ttu-id="ba7c4-125"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">A BLOB első lépések</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-125"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-126">Metaadatok/tulajdonságok/Stats BLOB</span><span class="sxs-lookup"><span data-stu-id="ba7c4-126">Blob Metadata/Properties/Stats</span></span></td>
<td><span data-ttu-id="ba7c4-127"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">A BLOB első lépések</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-127"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-128">A tároló hozzáférés-vezérlési lista/metaadatok/tulajdonságainak</span><span class="sxs-lookup"><span data-stu-id="ba7c4-128">Container ACL/Metadata/Properties</span></span></td>
<td><span data-ttu-id="ba7c4-129"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage fotótár webalkalmazás</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-129"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-130">Get tartományokat</span><span class="sxs-lookup"><span data-stu-id="ba7c4-130">Get Page Ranges</span></span></td>
<td><span data-ttu-id="ba7c4-131"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">A BLOB első lépések</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-131"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-132">Bérbe Blobtárolóban /</span><span class="sxs-lookup"><span data-stu-id="ba7c4-132">Lease Blob/Container</span></span></td>
<td><span data-ttu-id="ba7c4-133"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">A BLOB első lépések</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-133"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-134">Lista Blobtárolóban</span><span class="sxs-lookup"><span data-stu-id="ba7c4-134">List Blob/Container</span></span></td>
<td><span data-ttu-id="ba7c4-135"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">A BLOB első lépések</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-135"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-136">Az oldalakra vonatkozó Blob</span><span class="sxs-lookup"><span data-stu-id="ba7c4-136">Page Blob</span></span></td>
<td><span data-ttu-id="ba7c4-137"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">A BLOB első lépések</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-137"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Getting Started with Blobs</a></span></span></td>
</tr>
<tr> 
<td><span data-ttu-id="ba7c4-138">SAS</span><span class="sxs-lookup"><span data-stu-id="ba7c4-138">SAS</span></span></td>
<td><span data-ttu-id="ba7c4-139"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">A BLOB első lépések</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-139"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr>   
<tr> 
<td><span data-ttu-id="ba7c4-140">Szolgáltatás tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="ba7c4-140">Service Properties</span></span></td>
<td><span data-ttu-id="ba7c4-141"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">A BLOB első lépések</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-141"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr>           
<tr> 
<td><span data-ttu-id="ba7c4-142">Pillanatkép Blob</span><span class="sxs-lookup"><span data-stu-id="ba7c4-142">Snapshot Blob</span></span></td>
<td><span data-ttu-id="ba7c4-143"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-back-up-with-incremental-snapshots/blob/master/Program.cs">Biztonsági mentési Azure virtuális gépek lemezeit növekményes pillanatképek</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-143"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-back-up-with-incremental-snapshots/blob/master/Program.cs">Backup Azure Virtual Machine Disks with Incremental Snapshots</a></span></span></td>
</tr> 
<tr> 
<td rowspan="9"><span data-ttu-id="ba7c4-144"><b>Fájl</b></span><span class="sxs-lookup"><span data-stu-id="ba7c4-144"><b>File</b></span></span></td>
<td><span data-ttu-id="ba7c4-145">Megosztás-könyvtárak-fájlok létrehozása</span><span class="sxs-lookup"><span data-stu-id="ba7c4-145">Create Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="ba7c4-146"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Az Azure Storage .NET fájl tárolási minta</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-146"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="ba7c4-147">Megosztások/könyvtárak/fájlok törlése</span><span class="sxs-lookup"><span data-stu-id="ba7c4-147">Delete Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="ba7c4-148"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/master/FileStorage/GettingStarted.cs">Bevezetés az Azure File szolgáltatás a .NET használatába</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-148"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/master/FileStorage/GettingStarted.cs">Getting Started with Azure File Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-149">Directory tulajdonságok/metaadatok</span><span class="sxs-lookup"><span data-stu-id="ba7c4-149">Directory Properties/Metadata</span></span></td> 
<td><span data-ttu-id="ba7c4-150"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Az Azure Storage .NET fájl tárolási minta</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-150"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-151">Fájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="ba7c4-151">Download Files</span></span></td> 
<td><span data-ttu-id="ba7c4-152"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Az Azure Storage .NET fájl tárolási minta</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-152"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-153">A fájl tulajdonságok/metaadatok/metrikák</span><span class="sxs-lookup"><span data-stu-id="ba7c4-153">File Properties/Metadata/Metrics</span></span></td> 
<td><span data-ttu-id="ba7c4-154"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Az Azure Storage .NET fájl tárolási minta</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-154"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-155">Fájlok szolgáltatás tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="ba7c4-155">File Service Properties</span></span></td> 
<td><span data-ttu-id="ba7c4-156"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Az Azure Storage .NET fájl tárolási minta</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-156"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-157">Fájlok és könyvtárak listája</span><span class="sxs-lookup"><span data-stu-id="ba7c4-157">List Directories and Files</span></span></td> 
<td><span data-ttu-id="ba7c4-158"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Az Azure Storage .NET fájl tárolási minta</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-158"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="ba7c4-159">Lista megosztások</span><span class="sxs-lookup"><span data-stu-id="ba7c4-159">List Shares</span></span></td> 
<td><span data-ttu-id="ba7c4-160"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Az Azure Storage .NET fájl tárolási minta</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-160"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="ba7c4-161">Tulajdonságok/metaadatok/Stats megosztás</span><span class="sxs-lookup"><span data-stu-id="ba7c4-161">Share Properties/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="ba7c4-162"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Az Azure Storage .NET fájl tárolási minta</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-162"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td rowspan="8"><span data-ttu-id="ba7c4-163"><b>Várólista</b></span><span class="sxs-lookup"><span data-stu-id="ba7c4-163"><b>Queue</b></span></span></td>
<td><span data-ttu-id="ba7c4-164">Üzenet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ba7c4-164">Add Message</span></span></td> 
<td><span data-ttu-id="ba7c4-165"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Ismerkedés az Azure Queue szolgáltatás a .NET</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-165"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-166">Ügyféloldali titkosítás</span><span class="sxs-lookup"><span data-stu-id="ba7c4-166">Client-Side Encryption</span></span></td> 
<td><span data-ttu-id="ba7c4-167"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/QueueGettingStarted/Program.cs">Az Azure Storage .NET várólista ügyféloldali titkosítás</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-167"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/QueueGettingStarted/Program.cs">Azure Storage .NET Queue Client-Side Encryption</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-168">Várólista létrehozása</span><span class="sxs-lookup"><span data-stu-id="ba7c4-168">Create Queues</span></span></td> 
<td><span data-ttu-id="ba7c4-169"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Ismerkedés az Azure Queue szolgáltatás a .NET</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-169"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-170">Az üzenet-várólista törlése</span><span class="sxs-lookup"><span data-stu-id="ba7c4-170">Delete Message/Queue</span></span></td> 
<td><span data-ttu-id="ba7c4-171"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Ismerkedés az Azure Queue szolgáltatás a .NET</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-171"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-172">Üzenet megtekintése</span><span class="sxs-lookup"><span data-stu-id="ba7c4-172">Peek Message</span></span></td> 
<td><span data-ttu-id="ba7c4-173"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Ismerkedés az Azure Queue szolgáltatás a .NET</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-173"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-174">Hozzáférés-vezérlési lista/metaadatok/Stats várólista</span><span class="sxs-lookup"><span data-stu-id="ba7c4-174">Queue ACL/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="ba7c4-175"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Ismerkedés az Azure Queue szolgáltatás a .NET</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-175"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-176">Várólista szolgáltatás tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="ba7c4-176">Queue Service Properties</span></span></td> 
<td><span data-ttu-id="ba7c4-177"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Ismerkedés az Azure Queue szolgáltatás a .NET</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-177"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-178">Üzenet frissítése</span><span class="sxs-lookup"><span data-stu-id="ba7c4-178">Update Message</span></span></td> 
<td><span data-ttu-id="ba7c4-179"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Ismerkedés az Azure Queue szolgáltatás a .NET</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-179"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td rowspan="7"><span data-ttu-id="ba7c4-180"><b>Tábla</b></span><span class="sxs-lookup"><span data-stu-id="ba7c4-180"><b>Table</b></span></span></td>
<td><span data-ttu-id="ba7c4-181">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="ba7c4-181">Create Table</span></span></td> 
<td><span data-ttu-id="ba7c4-182"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Azure Storage - mintaalkalmazás használatával párhuzamossági kezelése</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-182"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Managing Concurrency using Azure Storage - Sample Application</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-183">Entitás vagy a tábla törlése</span><span class="sxs-lookup"><span data-stu-id="ba7c4-183">Delete Entity/Table</span></span></td> 
<td><span data-ttu-id="ba7c4-184"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-184"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-185">Entitás beszúrása és egyesítési/cseréje</span><span class="sxs-lookup"><span data-stu-id="ba7c4-185">Insert/Merge/Replace Entity</span></span></td> 
<td><span data-ttu-id="ba7c4-186"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Azure Storage - mintaalkalmazás használatával párhuzamossági kezelése</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-186"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Managing Concurrency using Azure Storage - Sample Application</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-187">Lekérdezés entitások</span><span class="sxs-lookup"><span data-stu-id="ba7c4-187">Query Entities</span></span></td> 
<td><span data-ttu-id="ba7c4-188"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-188"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-189">Lekérdezés táblák</span><span class="sxs-lookup"><span data-stu-id="ba7c4-189">Query Tables</span></span></td> 
<td><span data-ttu-id="ba7c4-190"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-190"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-191">Táblázat ACL/tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="ba7c4-191">Table ACL/Properties</span></span></td> 
<td><span data-ttu-id="ba7c4-192"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/AdvancedSamples.cs">Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-192"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/AdvancedSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="ba7c4-193">Entitás frissítése</span><span class="sxs-lookup"><span data-stu-id="ba7c4-193">Update Entity</span></span></td> 
<td><span data-ttu-id="ba7c4-194"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Azure Storage - mintaalkalmazás használatával párhuzamossági kezelése</a></span><span class="sxs-lookup"><span data-stu-id="ba7c4-194"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Managing Concurrency using Azure Storage - Sample Application</a></span></span></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a><span data-ttu-id="ba7c4-195">Az Azure mintakódok könyvtár</span><span class="sxs-lookup"><span data-stu-id="ba7c4-195">Azure Code Samples library</span></span>

<span data-ttu-id="ba7c4-196">A teljes minta könyvtár megtekintéséhez keresse fel a [Azure mintakódok](https://azure.microsoft.com/resources/samples/?service=storage) könyvtárban, és azt az Azure Storage töltse le és futtassa helyileg minták tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ba7c4-196">To view the complete sample library, go to the [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=storage) library, which includes samples for Azure Storage that you can download and run locally.</span></span> <span data-ttu-id="ba7c4-197">A kód a minta könyvtár .zip formátumban példakódot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ba7c4-197">The Code Sample Library provides sample code in .zip format.</span></span> <span data-ttu-id="ba7c4-198">Azt is megteheti keresse meg és a GitHub-tárházban, minden egyes minta klónozását.</span><span class="sxs-lookup"><span data-stu-id="ba7c4-198">Alternatively, you can browse and clone the GitHub repository for each sample.</span></span>

[!INCLUDE [storage-dotnet-samples-include](../../includes/storage-dotnet-samples-include.md)]

## <a name="getting-started-guides"></a><span data-ttu-id="ba7c4-199">Első lépéseket ismertető útmutatók</span><span class="sxs-lookup"><span data-stu-id="ba7c4-199">Getting started guides</span></span>

<span data-ttu-id="ba7c4-200">Tekintse meg az alábbi útmutatókban, ha a telepítését, és ismerkedjen meg az Azure Storage Ügyfélkódtáraival útmutatást.</span><span class="sxs-lookup"><span data-stu-id="ba7c4-200">Check out the following guides if you are looking for instructions on how to install and get started with the Azure Storage Client Libraries.</span></span>

* [<span data-ttu-id="ba7c4-201">Ismerkedés az Azure Blob szolgáltatás a .NET</span><span class="sxs-lookup"><span data-stu-id="ba7c4-201">Getting Started with Azure Blob Service in .NET</span></span>](storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="ba7c4-202">Ismerkedés az Azure Queue szolgáltatás a .NET</span><span class="sxs-lookup"><span data-stu-id="ba7c4-202">Getting Started with Azure Queue Service in .NET</span></span>](storage-dotnet-how-to-use-queues.md)
* [<span data-ttu-id="ba7c4-203">Ismerkedés az Azure Table szolgáltatás a .NET</span><span class="sxs-lookup"><span data-stu-id="ba7c4-203">Getting Started with Azure Table Service in .NET</span></span>](storage-dotnet-how-to-use-tables.md)
* [<span data-ttu-id="ba7c4-204">Bevezetés az Azure File szolgáltatás a .NET használatába</span><span class="sxs-lookup"><span data-stu-id="ba7c4-204">Getting Started with Azure File Service in .NET</span></span>](storage-dotnet-how-to-use-files.md)

## <a name="next-steps"></a><span data-ttu-id="ba7c4-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ba7c4-205">Next steps</span></span>

<span data-ttu-id="ba7c4-206">Egyéb nyelvek minták olvashat:</span><span class="sxs-lookup"><span data-stu-id="ba7c4-206">For information on samples for other languages:</span></span>

* <span data-ttu-id="ba7c4-207">Java: [Java használatával az Azure Storage-minták](storage-samples-java.md)</span><span class="sxs-lookup"><span data-stu-id="ba7c4-207">Java: [Azure Storage samples using Java](storage-samples-java.md)</span></span>
* <span data-ttu-id="ba7c4-208">Minden más nyelv: [Azure Storage-minták](storage-samples.md)</span><span class="sxs-lookup"><span data-stu-id="ba7c4-208">All other languages: [Azure Storage samples](storage-samples.md)</span></span>
