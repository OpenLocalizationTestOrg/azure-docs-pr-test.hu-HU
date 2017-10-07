---
title: "aaaAzure tárolási minták .NET használatával |} Microsoft Docs"
description: "Megtekintése, töltse le és futtassa a mintakódot és az alkalmazások az Azure Storage. Fedezze fel hello .NET storage ügyfélkódtáraival használó kódmintát BLOB, üzenetsorok, táblák és fájlok, az első lépések."
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
ms.openlocfilehash: 9a7055645b0f0658b850f024b8b19ab19840330e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-samples-using-net"></a><span data-ttu-id="1d0ab-104">Azure Storage-minták .NET használatával</span><span class="sxs-lookup"><span data-stu-id="1d0ab-104">Azure Storage samples using .NET</span></span>

## <a name="net-sample-index"></a><span data-ttu-id="1d0ab-105">.NET minta index</span><span class="sxs-lookup"><span data-stu-id="1d0ab-105">.NET sample index</span></span>

<span data-ttu-id="1d0ab-106">hello alábbi táblázat áttekintést nyújt az általunk biztosított mintákat tárház és hello forgatókönyvben szereplő minden egyes minta.</span><span class="sxs-lookup"><span data-stu-id="1d0ab-106">hello following table provides an overview of our samples repository and hello scenarios covered in each sample.</span></span> <span data-ttu-id="1d0ab-107">Kattintson a hello hivatkozások tooview hello megfelelő mintakód a Githubon.</span><span class="sxs-lookup"><span data-stu-id="1d0ab-107">Click on hello links tooview hello corresponding sample code in GitHub.</span></span>

<table style="font-size:90%"><thead><tr><th style="font-size:110%"><span data-ttu-id="1d0ab-108">Végpont</span><span class="sxs-lookup"><span data-stu-id="1d0ab-108">Endpoint</span></span></th><th style="font-size:110%"><span data-ttu-id="1d0ab-109">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="1d0ab-109">Scenario</span></span></th><th style="font-size:110%"><span data-ttu-id="1d0ab-110">Mintakód</span><span class="sxs-lookup"><span data-stu-id="1d0ab-110">Sample Code</span></span></th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><span data-ttu-id="1d0ab-111"><b>A BLOB</b></span><span class="sxs-lookup"><span data-stu-id="1d0ab-111"><b>Blob</b></span></span></td>
<td><span data-ttu-id="1d0ab-112">Hozzáfűző Blob</span><span class="sxs-lookup"><span data-stu-id="1d0ab-112">Append Blob</span></span></td> 
<td><span data-ttu-id="1d0ab-113"><a href="https://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.getappendblobreference.aspx">CloudBlobContainer.GetAppendBlobReference módszer – példa</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-113"><a href="https://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.getappendblobreference.aspx">CloudBlobContainer.GetAppendBlobReference Method Example</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-114">Blokkblob</span><span class="sxs-lookup"><span data-stu-id="1d0ab-114">Block Blob</span></span></td>
<td><span data-ttu-id="1d0ab-115"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage fotótár webalkalmazás</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-115"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-116">Ügyféloldali titkosítás</span><span class="sxs-lookup"><span data-stu-id="1d0ab-116">Client-Side Encryption</span></span></td>
<td><span data-ttu-id="1d0ab-117"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/BlobGettingStarted/Program.cs">A BLOB titkosítási minták</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-117"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/BlobGettingStarted/Program.cs">Blob Encryption Samples</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-118">A Blob másolása</span><span class="sxs-lookup"><span data-stu-id="1d0ab-118">Copy Blob</span></span></td>
<td><span data-ttu-id="1d0ab-119"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">A BLOB első lépések</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-119"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-120">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d0ab-120">Create Container</span></span></td>
<td><span data-ttu-id="1d0ab-121"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage fotótár webalkalmazás</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-121"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-122">A Blob törlése</span><span class="sxs-lookup"><span data-stu-id="1d0ab-122">Delete Blob</span></span></td>
<td><span data-ttu-id="1d0ab-123"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage fotótár webalkalmazás</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-123"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-124">Törli a tárolót</span><span class="sxs-lookup"><span data-stu-id="1d0ab-124">Delete Container</span></span></td>
<td><span data-ttu-id="1d0ab-125"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">A BLOB első lépések</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-125"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-126">Metaadatok/tulajdonságok/Stats BLOB</span><span class="sxs-lookup"><span data-stu-id="1d0ab-126">Blob Metadata/Properties/Stats</span></span></td>
<td><span data-ttu-id="1d0ab-127"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">A BLOB első lépések</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-127"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-128">A tároló hozzáférés-vezérlési lista/metaadatok/tulajdonságainak</span><span class="sxs-lookup"><span data-stu-id="1d0ab-128">Container ACL/Metadata/Properties</span></span></td>
<td><span data-ttu-id="1d0ab-129"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage fotótár webalkalmazás</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-129"><a href="https://github.com/Azure-Samples/storage-blobs-dotnet-webapp/blob/master/WebApp-Storage-DotNet/Controllers/HomeController.cs">Azure Blob Storage Photo Gallery Web Application</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-130">Get tartományokat</span><span class="sxs-lookup"><span data-stu-id="1d0ab-130">Get Page Ranges</span></span></td>
<td><span data-ttu-id="1d0ab-131"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">A BLOB első lépések</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-131"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-132">Bérbe Blobtárolóban /</span><span class="sxs-lookup"><span data-stu-id="1d0ab-132">Lease Blob/Container</span></span></td>
<td><span data-ttu-id="1d0ab-133"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">A BLOB első lépések</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-133"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-134">Lista Blobtárolóban</span><span class="sxs-lookup"><span data-stu-id="1d0ab-134">List Blob/Container</span></span></td>
<td><span data-ttu-id="1d0ab-135"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">A BLOB első lépések</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-135"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Getting Started with Blobs</a></span></span></td>
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-136">Az oldalakra vonatkozó Blob</span><span class="sxs-lookup"><span data-stu-id="1d0ab-136">Page Blob</span></span></td>
<td><span data-ttu-id="1d0ab-137"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">A BLOB első lépések</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-137"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/GettingStarted.cs">Getting Started with Blobs</a></span></span></td>
</tr>
<tr> 
<td><span data-ttu-id="1d0ab-138">SAS</span><span class="sxs-lookup"><span data-stu-id="1d0ab-138">SAS</span></span></td>
<td><span data-ttu-id="1d0ab-139"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">A BLOB első lépések</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-139"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr>   
<tr> 
<td><span data-ttu-id="1d0ab-140">Szolgáltatás tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="1d0ab-140">Service Properties</span></span></td>
<td><span data-ttu-id="1d0ab-141"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">A BLOB első lépések</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-141"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-getting-started/blob/master/BlobStorage/Advanced.cs">Getting Started with Blobs</a></span></span></td>
</tr>           
<tr> 
<td><span data-ttu-id="1d0ab-142">Pillanatkép Blob</span><span class="sxs-lookup"><span data-stu-id="1d0ab-142">Snapshot Blob</span></span></td>
<td><span data-ttu-id="1d0ab-143"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-back-up-with-incremental-snapshots/blob/master/Program.cs">Biztonsági mentési Azure virtuális gépek lemezeit növekményes pillanatképek</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-143"><a href="https://github.com/Azure-Samples/storage-blob-dotnet-back-up-with-incremental-snapshots/blob/master/Program.cs">Backup Azure Virtual Machine Disks with Incremental Snapshots</a></span></span></td>
</tr> 
<tr> 
<td rowspan="9"><span data-ttu-id="1d0ab-144"><b>Fájl</b></span><span class="sxs-lookup"><span data-stu-id="1d0ab-144"><b>File</b></span></span></td>
<td><span data-ttu-id="1d0ab-145">Megosztás-könyvtárak-fájlok létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d0ab-145">Create Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="1d0ab-146"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Az Azure Storage .NET fájl tárolási minta</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-146"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="1d0ab-147">Megosztások/könyvtárak/fájlok törlése</span><span class="sxs-lookup"><span data-stu-id="1d0ab-147">Delete Shares/Directories/Files</span></span></td> 
<td><span data-ttu-id="1d0ab-148"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/master/FileStorage/GettingStarted.cs">Bevezetés az Azure File szolgáltatás a .NET használatába</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-148"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/master/FileStorage/GettingStarted.cs">Getting Started with Azure File Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-149">Directory tulajdonságok/metaadatok</span><span class="sxs-lookup"><span data-stu-id="1d0ab-149">Directory Properties/Metadata</span></span></td> 
<td><span data-ttu-id="1d0ab-150"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Az Azure Storage .NET fájl tárolási minta</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-150"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-151">Fájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="1d0ab-151">Download Files</span></span></td> 
<td><span data-ttu-id="1d0ab-152"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Az Azure Storage .NET fájl tárolási minta</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-152"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-153">A fájl tulajdonságok/metaadatok/metrikák</span><span class="sxs-lookup"><span data-stu-id="1d0ab-153">File Properties/Metadata/Metrics</span></span></td> 
<td><span data-ttu-id="1d0ab-154"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Az Azure Storage .NET fájl tárolási minta</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-154"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-155">Fájlok szolgáltatás tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="1d0ab-155">File Service Properties</span></span></td> 
<td><span data-ttu-id="1d0ab-156"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Az Azure Storage .NET fájl tárolási minta</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-156"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-157">Fájlok és könyvtárak listája</span><span class="sxs-lookup"><span data-stu-id="1d0ab-157">List Directories and Files</span></span></td> 
<td><span data-ttu-id="1d0ab-158"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Az Azure Storage .NET fájl tárolási minta</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-158"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/VisualStudioQuickStarts/DataFileStorage/Program.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="1d0ab-159">Lista megosztások</span><span class="sxs-lookup"><span data-stu-id="1d0ab-159">List Shares</span></span></td> 
<td><span data-ttu-id="1d0ab-160"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Az Azure Storage .NET fájl tárolási minta</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-160"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td><span data-ttu-id="1d0ab-161">Tulajdonságok/metaadatok/Stats megosztás</span><span class="sxs-lookup"><span data-stu-id="1d0ab-161">Share Properties/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="1d0ab-162"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Az Azure Storage .NET fájl tárolási minta</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-162"><a href="https://github.com/Azure-Samples/storage-file-dotnet-getting-started/blob/9f12304b2f5f5472a1c87c1e21be4af5661ac043/FileStorage/Advanced.cs">Azure Storage .NET File Storage Sample</a></span></span></td> 
</tr>
<tr> 
<td rowspan="8"><span data-ttu-id="1d0ab-163"><b>Várólista</b></span><span class="sxs-lookup"><span data-stu-id="1d0ab-163"><b>Queue</b></span></span></td>
<td><span data-ttu-id="1d0ab-164">Üzenet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1d0ab-164">Add Message</span></span></td> 
<td><span data-ttu-id="1d0ab-165"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Ismerkedés az Azure Queue szolgáltatás a .NET</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-165"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-166">Ügyféloldali titkosítás</span><span class="sxs-lookup"><span data-stu-id="1d0ab-166">Client-Side Encryption</span></span></td> 
<td><span data-ttu-id="1d0ab-167"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/QueueGettingStarted/Program.cs">Az Azure Storage .NET várólista ügyféloldali titkosítás</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-167"><a href="https://github.com/Azure/azure-storage-net/blob/master/Samples/GettingStarted/EncryptionSamples/QueueGettingStarted/Program.cs">Azure Storage .NET Queue Client-Side Encryption</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-168">Várólista létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d0ab-168">Create Queues</span></span></td> 
<td><span data-ttu-id="1d0ab-169"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Ismerkedés az Azure Queue szolgáltatás a .NET</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-169"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-170">Az üzenet-várólista törlése</span><span class="sxs-lookup"><span data-stu-id="1d0ab-170">Delete Message/Queue</span></span></td> 
<td><span data-ttu-id="1d0ab-171"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Ismerkedés az Azure Queue szolgáltatás a .NET</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-171"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-172">Üzenet megtekintése</span><span class="sxs-lookup"><span data-stu-id="1d0ab-172">Peek Message</span></span></td> 
<td><span data-ttu-id="1d0ab-173"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Ismerkedés az Azure Queue szolgáltatás a .NET</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-173"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-174">Hozzáférés-vezérlési lista/metaadatok/Stats várólista</span><span class="sxs-lookup"><span data-stu-id="1d0ab-174">Queue ACL/Metadata/Stats</span></span></td> 
<td><span data-ttu-id="1d0ab-175"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Ismerkedés az Azure Queue szolgáltatás a .NET</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-175"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-176">Várólista szolgáltatás tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="1d0ab-176">Queue Service Properties</span></span></td> 
<td><span data-ttu-id="1d0ab-177"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Ismerkedés az Azure Queue szolgáltatás a .NET</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-177"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/Advanced.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-178">Üzenet frissítése</span><span class="sxs-lookup"><span data-stu-id="1d0ab-178">Update Message</span></span></td> 
<td><span data-ttu-id="1d0ab-179"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Ismerkedés az Azure Queue szolgáltatás a .NET</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-179"><a href="https://github.com/Azure-Samples/storage-queue-dotnet-getting-started/blob/master/QueueStorage/GettingStarted.cs">Getting Started with Azure Queue Service in .NET</a></span></span></td> 
</tr> 
<tr> 
<td rowspan="7"><span data-ttu-id="1d0ab-180"><b>Tábla</b></span><span class="sxs-lookup"><span data-stu-id="1d0ab-180"><b>Table</b></span></span></td>
<td><span data-ttu-id="1d0ab-181">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d0ab-181">Create Table</span></span></td> 
<td><span data-ttu-id="1d0ab-182"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Azure Storage - mintaalkalmazás használatával párhuzamossági kezelése</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-182"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Managing Concurrency using Azure Storage - Sample Application</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-183">Entitás vagy a tábla törlése</span><span class="sxs-lookup"><span data-stu-id="1d0ab-183">Delete Entity/Table</span></span></td> 
<td><span data-ttu-id="1d0ab-184"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-184"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-185">Entitás beszúrása és egyesítési/cseréje</span><span class="sxs-lookup"><span data-stu-id="1d0ab-185">Insert/Merge/Replace Entity</span></span></td> 
<td><span data-ttu-id="1d0ab-186"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Azure Storage - mintaalkalmazás használatával párhuzamossági kezelése</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-186"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Managing Concurrency using Azure Storage - Sample Application</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-187">Lekérdezés entitások</span><span class="sxs-lookup"><span data-stu-id="1d0ab-187">Query Entities</span></span></td> 
<td><span data-ttu-id="1d0ab-188"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-188"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-189">Lekérdezés táblák</span><span class="sxs-lookup"><span data-stu-id="1d0ab-189">Query Tables</span></span></td> 
<td><span data-ttu-id="1d0ab-190"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-190"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/BasicSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-191">Táblázat ACL/tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="1d0ab-191">Table ACL/Properties</span></span></td> 
<td><span data-ttu-id="1d0ab-192"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/AdvancedSamples.cs">Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-192"><a href="https://github.com/Azure-Samples/storage-table-dotnet-getting-started/blob/master/TableStorage/AdvancedSamples.cs">Getting Started with Azure Table Storage in .NET</a></span></span></td> 
</tr> 
<tr> 
<td><span data-ttu-id="1d0ab-193">Entitás frissítése</span><span class="sxs-lookup"><span data-stu-id="1d0ab-193">Update Entity</span></span></td> 
<td><span data-ttu-id="1d0ab-194"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Azure Storage - mintaalkalmazás használatával párhuzamossági kezelése</a></span><span class="sxs-lookup"><span data-stu-id="1d0ab-194"><a href="https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114/sourcecode?fileId=123913&pathId=50196262">Managing Concurrency using Azure Storage - Sample Application</a></span></span></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a><span data-ttu-id="1d0ab-195">Az Azure mintakódok könyvtár</span><span class="sxs-lookup"><span data-stu-id="1d0ab-195">Azure Code Samples library</span></span>

<span data-ttu-id="1d0ab-196">tooview hello teljes mintát könyvtár, nyissa meg toohello [Azure mintakódok](https://azure.microsoft.com/resources/samples/?service=storage) könyvtárban, és azt az Azure Storage töltse le és futtassa helyileg minták tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1d0ab-196">tooview hello complete sample library, go toohello [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=storage) library, which includes samples for Azure Storage that you can download and run locally.</span></span> <span data-ttu-id="1d0ab-197">hello kód a minta könyvtár .zip formátumban példakódot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="1d0ab-197">hello Code Sample Library provides sample code in .zip format.</span></span> <span data-ttu-id="1d0ab-198">Azt is megteheti keresse meg, és minden egyes minta hello GitHub tárház klónozása.</span><span class="sxs-lookup"><span data-stu-id="1d0ab-198">Alternatively, you can browse and clone hello GitHub repository for each sample.</span></span>

[!INCLUDE [storage-dotnet-samples-include](../../includes/storage-dotnet-samples-include.md)]

## <a name="getting-started-guides"></a><span data-ttu-id="1d0ab-199">Első lépéseket ismertető útmutatók</span><span class="sxs-lookup"><span data-stu-id="1d0ab-199">Getting started guides</span></span>

<span data-ttu-id="1d0ab-200">Tekintse meg a következő útmutatók, ha olyan eszközökre vonatkozóan törlésének hello tooinstall és Ismerkedés az Azure Storage Ügyfélkódtáraival hello.</span><span class="sxs-lookup"><span data-stu-id="1d0ab-200">Check out hello following guides if you are looking for instructions on how tooinstall and get started with hello Azure Storage Client Libraries.</span></span>

* [<span data-ttu-id="1d0ab-201">Ismerkedés az Azure Blob szolgáltatás a .NET</span><span class="sxs-lookup"><span data-stu-id="1d0ab-201">Getting Started with Azure Blob Service in .NET</span></span>](storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="1d0ab-202">Ismerkedés az Azure Queue szolgáltatás a .NET</span><span class="sxs-lookup"><span data-stu-id="1d0ab-202">Getting Started with Azure Queue Service in .NET</span></span>](storage-dotnet-how-to-use-queues.md)
* [<span data-ttu-id="1d0ab-203">Ismerkedés az Azure Table szolgáltatás a .NET</span><span class="sxs-lookup"><span data-stu-id="1d0ab-203">Getting Started with Azure Table Service in .NET</span></span>](storage-dotnet-how-to-use-tables.md)
* [<span data-ttu-id="1d0ab-204">Bevezetés az Azure File szolgáltatás a .NET használatába</span><span class="sxs-lookup"><span data-stu-id="1d0ab-204">Getting Started with Azure File Service in .NET</span></span>](storage-dotnet-how-to-use-files.md)

## <a name="next-steps"></a><span data-ttu-id="1d0ab-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1d0ab-205">Next steps</span></span>

<span data-ttu-id="1d0ab-206">Egyéb nyelvek minták olvashat:</span><span class="sxs-lookup"><span data-stu-id="1d0ab-206">For information on samples for other languages:</span></span>

* <span data-ttu-id="1d0ab-207">Java: [Java használatával az Azure Storage-minták](storage-samples-java.md)</span><span class="sxs-lookup"><span data-stu-id="1d0ab-207">Java: [Azure Storage samples using Java](storage-samples-java.md)</span></span>
* <span data-ttu-id="1d0ab-208">Minden más nyelv: [Azure Storage-minták](storage-samples.md)</span><span class="sxs-lookup"><span data-stu-id="1d0ab-208">All other languages: [Azure Storage samples](storage-samples.md)</span></span>
