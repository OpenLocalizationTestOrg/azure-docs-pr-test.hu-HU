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
# <a name="manage-expiration-of-azure-storage-blobs-in-azure-cdn"></a>Azure Storage blobs szolgáltatásban az Azure CDN lejáratának kezelése
> [!div class="op_single_selector"]
> * [Az Azure Web Apps/Felhőszolgáltatások, ASP.NET, vagy az IIS](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Storage-blob szolgáltatás](cdn-manage-expiration-of-blob-content.md)
> 
> 

Hello [blob szolgáltatás](../storage/common/storage-introduction.md#blob-storage) a [Azure Storage](../storage/common/storage-introduction.md) integrálva van a több Azure-alapú források egyik Azure CDN szolgáltatás használata.  Bármely nyilvánosan elérhető blobtartalom Azure CDN mindaddig, amíg az idő-live (TTL) lejárta gyorsítótárazható.  hello TTL határozza meg hello [ *Cache-Control* fejléc](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) hello HTTP válaszként az Azure Storage-ból.

> [!TIP]
> Úgy is dönthet, tooset nincs blob a TTL-t.  Ebben az esetben Azure CDN automatikusan érvényes alapértelmezett élettartam 7 nap.
> 
> Hozzáférés tooblobs és egyéb fájlok toospeed Azure CDN szolgáltatás működésével kapcsolatos további információkért lásd: hello [Azure CDN áttekintése](cdn-overview.md).
> 
> Az Azure Storage-blob szolgáltatás hello további részletekért lásd: [Blob szolgáltatással kapcsolatos fogalmak](https://msdn.microsoft.com/library/dd179376.aspx). 
> 
> 

Ez az oktatóanyag bemutatja, többféle módon, hogy hello TTL-t állíthat be az Azure Storage blob.  

## <a name="azure-powershell"></a>Azure PowerShell
[Az Azure PowerShell](/powershell/azure/overview) egyike hello leggyorsabb, leghatékonyabb módon tooadminister az Azure-szolgáltatások.  Használjon hello `Get-AzureStorageBlob` parancsmag tooget hivatkozás toohello blob, majd állítsa be a hello `.ICloudBlob.Properties.CacheControl` tulajdonság. 

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
> Túl PowerShell is használható[a CDN-profil és a végpontok kezelése](cdn-manage-powershell.md).
> 
> 

## <a name="azure-storage-client-library-for-net"></a>Az Azure Storage .NET-hez készült ügyféloldali kódtára
tooset blob a TTL-t használja a .NET-keretrendszerből használja hello [Azure Storage ügyféloldali kódtára a .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) tooset hello [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) tulajdonság.

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
> Nincsenek elérhető hello számos további .NET mintakódok [Azure Blob Storage-példák .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).
> 
> 

## <a name="other-methods"></a>Más módszerrel
* [Azure parancssori felület](../cli-install-nodejs.md)
  
    Amikor hello blob feltöltése, állítsa be a hello *cacheControl* tulajdonságon keresztül hello `-p` váltani.  Ez a példa beállítja hello TTL tooone óra (3600 másodperc).
  
    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```
* [Az Azure Storage-szolgáltatások REST API-ja](https://msdn.microsoft.com/library/azure/dd179355.aspx)
  
    Explicit módon beállítva hello *x-ms-blob-cache-control* tulajdonságának egy [Put Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [Put tiltólista](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx), vagy [Blob tulajdonságai](https://msdn.microsoft.com/library/azure/ee691966.aspx) kérelmet.
* Külső tárolófelügyeleti eszközök
  
    Egyes külső Azure Tárolófelügyeleti eszközök lehetővé teszik tooset hello *CacheControl* blobok tulajdonságát. 

## <a name="testing-hello-cache-control-header"></a>Tesztelési hello *Cache-Control* fejléc
A blobok Élettartammal hello könnyen ellenőrizheti.  A böngésző használatával [fejlesztői eszközök](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), tesztelheti, hogy a blob közé tartoznak a következők hello *Cache-Control* válaszfejlécet.  Egy eszköz, például is használható **wget**, [Postman](https://www.getpostman.com/), vagy [Fiddler](http://www.telerik.com/fiddler) tooexamine hello response fejlécekkel együtt.

## <a name="next-steps"></a>Következő lépések
* [További információ a hello *Cache-Control* fejléc](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [Megtudhatja, hogyan Azure CDN felhő Felhőszolgáltatásbeli tartalom lejáratának toomanage](cdn-manage-expiration-of-cloud-service-content.md)

