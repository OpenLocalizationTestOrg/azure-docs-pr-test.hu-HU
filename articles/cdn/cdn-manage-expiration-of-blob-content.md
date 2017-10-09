---
title: "Azure Storage blobs szolgáltatásban az Azure CDN aaaManage lejáratának |} Microsoft Docs"
description: "További tudnivalók hello beállításait élő idő a blobok az Azure CDN gyorsítótárazását."
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
ms.openlocfilehash: 9fecae9639deb28977da7f851e1da4a823ddc4e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-expiration-of-azure-storage-blobs-in-azure-cdn"></a><span data-ttu-id="73fa2-103">Azure Storage blobs szolgáltatásban az Azure CDN lejáratának kezelése</span><span class="sxs-lookup"><span data-stu-id="73fa2-103">Manage expiration of Azure Storage blobs in Azure CDN</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="73fa2-104">Az Azure Web Apps/Felhőszolgáltatások, ASP.NET, vagy az IIS</span><span class="sxs-lookup"><span data-stu-id="73fa2-104">Azure Web Apps/Cloud Services, ASP.NET, or IIS</span></span>](cdn-manage-expiration-of-cloud-service-content.md)
> * [<span data-ttu-id="73fa2-105">Azure Storage-blob szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="73fa2-105">Azure Storage blob service</span></span>](cdn-manage-expiration-of-blob-content.md)
> 
> 

<span data-ttu-id="73fa2-106">Hello [blob szolgáltatás](../storage/common/storage-introduction.md#blob-storage) a [Azure Storage](../storage/common/storage-introduction.md) integrálva van a több Azure-alapú források egyik Azure CDN szolgáltatás használata.</span><span class="sxs-lookup"><span data-stu-id="73fa2-106">hello [blob service](../storage/common/storage-introduction.md#blob-storage) in [Azure Storage](../storage/common/storage-introduction.md) is one of several Azure-based origins integrated with Azure CDN.</span></span>  <span data-ttu-id="73fa2-107">Bármely nyilvánosan elérhető blobtartalom Azure CDN mindaddig, amíg az idő-live (TTL) lejárta gyorsítótárazható.</span><span class="sxs-lookup"><span data-stu-id="73fa2-107">Any publicly accessible blob content can be cached in Azure CDN until its time-to-live (TTL) elapses.</span></span>  <span data-ttu-id="73fa2-108">hello TTL határozza meg hello [ *Cache-Control* fejléc](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) hello HTTP válaszként az Azure Storage-ból.</span><span class="sxs-lookup"><span data-stu-id="73fa2-108">hello TTL is determined by hello [*Cache-Control* header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in hello HTTP response from Azure Storage.</span></span>

> [!TIP]
> <span data-ttu-id="73fa2-109">Úgy is dönthet, tooset nincs blob a TTL-t.</span><span class="sxs-lookup"><span data-stu-id="73fa2-109">You may choose tooset no TTL on a blob.</span></span>  <span data-ttu-id="73fa2-110">Ebben az esetben Azure CDN automatikusan érvényes alapértelmezett élettartam 7 nap.</span><span class="sxs-lookup"><span data-stu-id="73fa2-110">In this case, Azure CDN automatically applies a default TTL of seven days.</span></span>
> 
> <span data-ttu-id="73fa2-111">Hozzáférés tooblobs és egyéb fájlok toospeed Azure CDN szolgáltatás működésével kapcsolatos további információkért lásd: hello [Azure CDN áttekintése](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="73fa2-111">For more information about how Azure CDN works toospeed up access tooblobs and other files, see hello [Azure CDN Overview](cdn-overview.md).</span></span>
> 
> <span data-ttu-id="73fa2-112">Az Azure Storage-blob szolgáltatás hello további részletekért lásd: [Blob szolgáltatással kapcsolatos fogalmak](https://msdn.microsoft.com/library/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="73fa2-112">For more details on hello Azure Storage blob service, see [Blob Service Concepts](https://msdn.microsoft.com/library/dd179376.aspx).</span></span> 
> 
> 

<span data-ttu-id="73fa2-113">Ez az oktatóanyag bemutatja, többféle módon, hogy hello TTL-t állíthat be az Azure Storage blob.</span><span class="sxs-lookup"><span data-stu-id="73fa2-113">This tutorial demonstrates several ways that you can set hello TTL on a blob in Azure Storage.</span></span>  

## <a name="azure-powershell"></a><span data-ttu-id="73fa2-114">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="73fa2-114">Azure PowerShell</span></span>
<span data-ttu-id="73fa2-115">[Az Azure PowerShell](/powershell/azure/overview) egyike hello leggyorsabb, leghatékonyabb módon tooadminister az Azure-szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="73fa2-115">[Azure PowerShell](/powershell/azure/overview) is one of hello quickest, most powerful ways tooadminister your Azure services.</span></span>  <span data-ttu-id="73fa2-116">Használjon hello `Get-AzureStorageBlob` parancsmag tooget hivatkozás toohello blob, majd állítsa be a hello `.ICloudBlob.Properties.CacheControl` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="73fa2-116">Use hello `Get-AzureStorageBlob` cmdlet tooget a reference toohello blob, then set hello `.ICloudBlob.Properties.CacheControl` property.</span></span> 

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference toohello blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set hello CacheControl property tooexpire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "public, max-age=3600"

# Send hello update toohello cloud
$blob.ICloudBlob.SetProperties()
```

> [!TIP]
> <span data-ttu-id="73fa2-117">Túl PowerShell is használható[a CDN-profil és a végpontok kezelése](cdn-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="73fa2-117">You can also use PowerShell too[manage your CDN profiles and endpoints](cdn-manage-powershell.md).</span></span>
> 
> 

## <a name="azure-storage-client-library-for-net"></a><span data-ttu-id="73fa2-118">Az Azure Storage .NET-hez készült ügyféloldali kódtára</span><span class="sxs-lookup"><span data-stu-id="73fa2-118">Azure Storage Client Library for .NET</span></span>
<span data-ttu-id="73fa2-119">tooset blob a TTL-t használja a .NET-keretrendszerből használja hello [Azure Storage ügyféloldali kódtára a .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) tooset hello [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="73fa2-119">tooset a blob's TTL using .NET, use hello [Azure Storage Client Library for .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) tooset hello [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) property.</span></span>

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);

        // Create a blob client for interacting with hello blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Create a reference toohello container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference toohello blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set hello CacheControl property tooexpire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "public, max-age=3600";

        // Update hello blob's properties in hello cloud
        blob.SetProperties();
    }
}
```

> [!TIP]
> <span data-ttu-id="73fa2-120">Nincsenek elérhető hello számos további .NET mintakódok [Azure Blob Storage-példák .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).</span><span class="sxs-lookup"><span data-stu-id="73fa2-120">There are many more .NET code samples available in hello [Azure Blob Storage Samples for .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).</span></span>
> 
> 

## <a name="other-methods"></a><span data-ttu-id="73fa2-121">Más módszerrel</span><span class="sxs-lookup"><span data-stu-id="73fa2-121">Other methods</span></span>
* [<span data-ttu-id="73fa2-122">Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="73fa2-122">Azure Command-Line Interface</span></span>](../cli-install-nodejs.md)
  
    <span data-ttu-id="73fa2-123">Amikor hello blob feltöltése, állítsa be a hello *cacheControl* tulajdonságon keresztül hello `-p` váltani.</span><span class="sxs-lookup"><span data-stu-id="73fa2-123">When uploading hello blob, set hello *cacheControl* property using hello `-p` switch.</span></span>  <span data-ttu-id="73fa2-124">Ez a példa beállítja hello TTL tooone óra (3600 másodperc).</span><span class="sxs-lookup"><span data-stu-id="73fa2-124">This example sets hello TTL tooone hour (3600 seconds).</span></span>
  
    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```
* [<span data-ttu-id="73fa2-125">Az Azure Storage-szolgáltatások REST API-ja</span><span class="sxs-lookup"><span data-stu-id="73fa2-125">Azure Storage Services REST API</span></span>](https://msdn.microsoft.com/library/azure/dd179355.aspx)
  
    <span data-ttu-id="73fa2-126">Explicit módon beállítva hello *x-ms-blob-cache-control* tulajdonságának egy [Put Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [Put tiltólista](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx), vagy [Blob tulajdonságai](https://msdn.microsoft.com/library/azure/ee691966.aspx) kérelmet.</span><span class="sxs-lookup"><span data-stu-id="73fa2-126">Explicitly set hello *x-ms-blob-cache-control* property on a [Put Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [Put Block List](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx), or [Set Blob Properties](https://msdn.microsoft.com/library/azure/ee691966.aspx) request.</span></span>
* <span data-ttu-id="73fa2-127">Külső tárolófelügyeleti eszközök</span><span class="sxs-lookup"><span data-stu-id="73fa2-127">Third-party storage management tools</span></span>
  
    <span data-ttu-id="73fa2-128">Egyes külső Azure Tárolófelügyeleti eszközök lehetővé teszik tooset hello *CacheControl* blobok tulajdonságát.</span><span class="sxs-lookup"><span data-stu-id="73fa2-128">Some third-party Azure Storage management tools allow you tooset hello *CacheControl* property on blobs.</span></span> 

## <a name="testing-hello-cache-control-header"></a><span data-ttu-id="73fa2-129">Tesztelési hello *Cache-Control* fejléc</span><span class="sxs-lookup"><span data-stu-id="73fa2-129">Testing hello *Cache-Control* header</span></span>
<span data-ttu-id="73fa2-130">A blobok Élettartammal hello könnyen ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="73fa2-130">You can easily verify hello TTL of your blobs.</span></span>  <span data-ttu-id="73fa2-131">A böngésző használatával [fejlesztői eszközök](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), tesztelheti, hogy a blob közé tartoznak a következők hello *Cache-Control* válaszfejlécet.</span><span class="sxs-lookup"><span data-stu-id="73fa2-131">Using your browser's [developer tools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), test that your blob is including hello *Cache-Control* response header.</span></span>  <span data-ttu-id="73fa2-132">Egy eszköz, például is használható **wget**, [Postman](https://www.getpostman.com/), vagy [Fiddler](http://www.telerik.com/fiddler) tooexamine hello response fejlécekkel együtt.</span><span class="sxs-lookup"><span data-stu-id="73fa2-132">You can also use a tool like **wget**, [Postman](https://www.getpostman.com/), or [Fiddler](http://www.telerik.com/fiddler) tooexamine hello response headers.</span></span>

## <a name="next-steps"></a><span data-ttu-id="73fa2-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="73fa2-133">Next Steps</span></span>
* [<span data-ttu-id="73fa2-134">További információ a hello *Cache-Control* fejléc</span><span class="sxs-lookup"><span data-stu-id="73fa2-134">Read about hello *Cache-Control* header</span></span>](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [<span data-ttu-id="73fa2-135">Megtudhatja, hogyan Azure CDN felhő Felhőszolgáltatásbeli tartalom lejáratának toomanage</span><span class="sxs-lookup"><span data-stu-id="73fa2-135">Learn how toomanage expiration of Cloud Service content in Azure CDN</span></span>](cdn-manage-expiration-of-cloud-service-content.md)

