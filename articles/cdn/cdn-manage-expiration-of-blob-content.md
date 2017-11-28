---
title: "Azure Storage blobs szolgáltatásban az Azure CDN lejáratának kezelése |} Microsoft Docs"
description: "További tudnivalók a beállításait élő idő a blobok az Azure CDN gyorsítótárazását."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: ad4801e9-d09a-49bf-b35c-efdc4e6034e8
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: d4741921806e443d92c385a04b781cec296c2ae8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="manage-expiration-of-azure-storage-blobs-in-azure-cdn"></a><span data-ttu-id="47ecc-103">Azure Storage blobs szolgáltatásban az Azure CDN lejáratának kezelése</span><span class="sxs-lookup"><span data-stu-id="47ecc-103">Manage expiration of Azure Storage blobs in Azure CDN</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="47ecc-104">Az Azure Web Apps/Felhőszolgáltatások, ASP.NET, vagy az IIS</span><span class="sxs-lookup"><span data-stu-id="47ecc-104">Azure Web Apps/Cloud Services, ASP.NET, or IIS</span></span>](cdn-manage-expiration-of-cloud-service-content.md)
> * [<span data-ttu-id="47ecc-105">Azure Storage-blob szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="47ecc-105">Azure Storage blob service</span></span>](cdn-manage-expiration-of-blob-content.md)
> 
> 

<span data-ttu-id="47ecc-106">A [blob szolgáltatás](../storage/common/storage-introduction.md#blob-storage) a [Azure Storage](../storage/common/storage-introduction.md) integrálva van a több Azure-alapú források egyik Azure CDN szolgáltatás használata.</span><span class="sxs-lookup"><span data-stu-id="47ecc-106">The [blob service](../storage/common/storage-introduction.md#blob-storage) in [Azure Storage](../storage/common/storage-introduction.md) is one of several Azure-based origins integrated with Azure CDN.</span></span>  <span data-ttu-id="47ecc-107">Bármely nyilvánosan elérhető blobtartalom Azure CDN mindaddig, amíg az idő-live (TTL) lejárta gyorsítótárazható.</span><span class="sxs-lookup"><span data-stu-id="47ecc-107">Any publicly accessible blob content can be cached in Azure CDN until its time-to-live (TTL) elapses.</span></span>  <span data-ttu-id="47ecc-108">Az élettartam határozza meg a [ *Cache-Control* fejléc](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) az Azure Storage-ból a HTTP-válasz.</span><span class="sxs-lookup"><span data-stu-id="47ecc-108">The TTL is determined by the [*Cache-Control* header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in the HTTP response from Azure Storage.</span></span>

> [!TIP]
> <span data-ttu-id="47ecc-109">Választhatja a blob nem TTL beállítása.</span><span class="sxs-lookup"><span data-stu-id="47ecc-109">You may choose to set no TTL on a blob.</span></span>  <span data-ttu-id="47ecc-110">Ebben az esetben Azure CDN automatikusan érvényes alapértelmezett élettartam 7 nap.</span><span class="sxs-lookup"><span data-stu-id="47ecc-110">In this case, Azure CDN automatically applies a default TTL of seven days.</span></span>
> 
> <span data-ttu-id="47ecc-111">Azure CDN gyorsabb hozzáférés blobok és egyéb fájlok működésével kapcsolatos további információkért lásd: a [Azure CDN áttekintése](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="47ecc-111">For more information about how Azure CDN works to speed up access to blobs and other files, see the [Azure CDN Overview](cdn-overview.md).</span></span>
> 
> <span data-ttu-id="47ecc-112">Az Azure Storage-blob szolgáltatásban további részletekért lásd: [Blob szolgáltatással kapcsolatos fogalmak](https://msdn.microsoft.com/library/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="47ecc-112">For more details on the Azure Storage blob service, see [Blob Service Concepts](https://msdn.microsoft.com/library/dd179376.aspx).</span></span> 
> 
> 

<span data-ttu-id="47ecc-113">Ez az oktatóanyag bemutatja a TTL-t állíthat be az Azure Storage blob számos módja.</span><span class="sxs-lookup"><span data-stu-id="47ecc-113">This tutorial demonstrates several ways that you can set the TTL on a blob in Azure Storage.</span></span>  

## <a name="azure-powershell"></a><span data-ttu-id="47ecc-114">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="47ecc-114">Azure PowerShell</span></span>
<span data-ttu-id="47ecc-115">[Az Azure PowerShell](/powershell/azure/overview) egyik a leggyorsabb, a leghatékonyabb módszer az Azure-szolgáltatások felügyeletéhez.</span><span class="sxs-lookup"><span data-stu-id="47ecc-115">[Azure PowerShell](/powershell/azure/overview) is one of the quickest, most powerful ways to administer your Azure services.</span></span>  <span data-ttu-id="47ecc-116">Használja a `Get-AzureStorageBlob` parancsmagot, hogy megkapja a blobra mutató hivatkozást, majd állítsa be a `.ICloudBlob.Properties.CacheControl` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="47ecc-116">Use the `Get-AzureStorageBlob` cmdlet to get a reference to the blob, then set the `.ICloudBlob.Properties.CacheControl` property.</span></span> 

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "public, max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

> [!TIP]
> <span data-ttu-id="47ecc-117">Használhatja a PowerShell [a CDN-profil és a végpontok kezelése](cdn-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="47ecc-117">You can also use PowerShell to [manage your CDN profiles and endpoints](cdn-manage-powershell.md).</span></span>
> 
> 

## <a name="azure-storage-client-library-for-net"></a><span data-ttu-id="47ecc-118">Az Azure Storage .NET-hez készült ügyféloldali kódtára</span><span class="sxs-lookup"><span data-stu-id="47ecc-118">Azure Storage Client Library for .NET</span></span>
<span data-ttu-id="47ecc-119">Egy blob TTL-t használó .NET beállításához használja a [Azure Storage ügyféloldali kódtára a .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) beállítása a [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="47ecc-119">To set a blob's TTL using .NET, use the [Azure Storage Client Library for .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) to set the [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) property.</span></span>

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);

        // Create a blob client for interacting with the blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Create a reference to the container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "public, max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

> [!TIP]
> <span data-ttu-id="47ecc-120">Nincsenek elérhető számos további .NET Kódminták a [Azure Blob Storage-példák .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="47ecc-120">There are many more .NET code samples available in the [Azure Blob Storage Samples for .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).</span></span>
> 
> 

## <a name="other-methods"></a><span data-ttu-id="47ecc-121">Más módszerrel</span><span class="sxs-lookup"><span data-stu-id="47ecc-121">Other methods</span></span>
* [<span data-ttu-id="47ecc-122">Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="47ecc-122">Azure Command-Line Interface</span></span>](../cli-install-nodejs.md)
  
    <span data-ttu-id="47ecc-123">Amikor feltöltése a blob, állítsa be a *cacheControl* tulajdonság használatával a `-p` váltani.</span><span class="sxs-lookup"><span data-stu-id="47ecc-123">When uploading the blob, set the *cacheControl* property using the `-p` switch.</span></span>  <span data-ttu-id="47ecc-124">Ebben a példában az élettartam beállítja egy óra (3600 másodperc).</span><span class="sxs-lookup"><span data-stu-id="47ecc-124">This example sets the TTL to one hour (3600 seconds).</span></span>
  
    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```
* [<span data-ttu-id="47ecc-125">Az Azure Storage-szolgáltatások REST API-ja</span><span class="sxs-lookup"><span data-stu-id="47ecc-125">Azure Storage Services REST API</span></span>](https://msdn.microsoft.com/library/azure/dd179355.aspx)
  
    <span data-ttu-id="47ecc-126">Explicit módon beállítva a *x-ms-blob-cache-control* tulajdonságának egy [Put Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [Put tiltólista](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx), vagy [Blob tulajdonságai](https://msdn.microsoft.com/library/azure/ee691966.aspx) kérelmet.</span><span class="sxs-lookup"><span data-stu-id="47ecc-126">Explicitly set the *x-ms-blob-cache-control* property on a [Put Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [Put Block List](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx), or [Set Blob Properties](https://msdn.microsoft.com/library/azure/ee691966.aspx) request.</span></span>
* <span data-ttu-id="47ecc-127">Külső tárolófelügyeleti eszközök</span><span class="sxs-lookup"><span data-stu-id="47ecc-127">Third-party storage management tools</span></span>
  
    <span data-ttu-id="47ecc-128">Egyes külső Azure Tárolófelügyeleti eszközök lehetővé teszik a *CacheControl* blobok tulajdonságát.</span><span class="sxs-lookup"><span data-stu-id="47ecc-128">Some third-party Azure Storage management tools allow you to set the *CacheControl* property on blobs.</span></span> 

## <a name="testing-the-cache-control-header"></a><span data-ttu-id="47ecc-129">Tesztelés a *Cache-Control* fejléc</span><span class="sxs-lookup"><span data-stu-id="47ecc-129">Testing the *Cache-Control* header</span></span>
<span data-ttu-id="47ecc-130">Az élettartam a blobok könnyen ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="47ecc-130">You can easily verify the TTL of your blobs.</span></span>  <span data-ttu-id="47ecc-131">A böngésző használatával [fejlesztői eszközök](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), tesztelje, hogy a blob közé tartoznak a következők a *Cache-Control* válaszfejlécet.</span><span class="sxs-lookup"><span data-stu-id="47ecc-131">Using your browser's [developer tools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), test that your blob is including the *Cache-Control* response header.</span></span>  <span data-ttu-id="47ecc-132">Egy eszköz, például is használható **wget**, [Postman](https://www.getpostman.com/), vagy [Fiddler](http://www.telerik.com/fiddler) megvizsgálhatja a response fejlécekkel együtt.</span><span class="sxs-lookup"><span data-stu-id="47ecc-132">You can also use a tool like **wget**, [Postman](https://www.getpostman.com/), or [Fiddler](http://www.telerik.com/fiddler) to examine the response headers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="47ecc-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="47ecc-133">Next Steps</span></span>
* [<span data-ttu-id="47ecc-134">Olvassa el a *Cache-Control* fejléc</span><span class="sxs-lookup"><span data-stu-id="47ecc-134">Read about the *Cache-Control* header</span></span>](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [<span data-ttu-id="47ecc-135">Útmutató: Azure CDN felhő Felhőszolgáltatásbeli tartalom lejáratának kezelése</span><span class="sxs-lookup"><span data-stu-id="47ecc-135">Learn how to manage expiration of Cloud Service content in Azure CDN</span></span>](cdn-manage-expiration-of-cloud-service-content.md)

