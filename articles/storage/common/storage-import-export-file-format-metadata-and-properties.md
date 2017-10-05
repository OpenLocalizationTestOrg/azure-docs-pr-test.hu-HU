---
title: "Az Azure Import/Export metaadatok és a Tulajdonságok fájlformátum |} Microsoft Docs"
description: "Megtudhatja, hogyan adhatja meg a metaadatok és egy vagy több blobot, az importálás során vagy exportálja a feladat tulajdonságait."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 840364c6-d9a8-4b43-a9f3-f7441c625069
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 3f728ad94cdcbd32092b677f11a737ae91376720
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-importexport-service-metadata-and-properties-file-format"></a><span data-ttu-id="7d6e4-103">Az Azure Import/Export szolgáltatás metaadatok és a Tulajdonságok fájlformátum</span><span class="sxs-lookup"><span data-stu-id="7d6e4-103">Azure Import/Export service metadata and properties file format</span></span>
<span data-ttu-id="7d6e4-104">Megadhat egy vagy több blobot tulajdonságai és metaadatok egy importálási feladat vagy exportálási feladat részeként.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-104">You can specify metadata and properties for one or more blobs as part of an import job or an export job.</span></span> <span data-ttu-id="7d6e4-105">Metaadatok és az importálási feladat részeként létrehozott BLOB tulajdonságainak beállításához adja meg az importálandó adatokat tartalmazó merevlemez-meghajtóról metaadatok vagy tulajdonságok fájlba.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-105">To set metadata or properties for blobs being created as part of an import job, you provide a metadata or properties file on the hard drive containing the data to be imported.</span></span> <span data-ttu-id="7d6e4-106">Exportálási feladat metaadatok és a Tulajdonságok írja metaadatok vagy tulajdonságok fájlba, amely tartalmazza a visszaküldött merevlemez-meghajtóra.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-106">For an export job, metadata and properties are written to a metadata or properties file that is included on the hard drive returned to you.</span></span>  
  
## <a name="metadata-file-format"></a><span data-ttu-id="7d6e4-107">Metaadatok fájlformátum</span><span class="sxs-lookup"><span data-stu-id="7d6e4-107">Metadata file format</span></span>  
<span data-ttu-id="7d6e4-108">A metaadatfájl formátuma a következő:</span><span class="sxs-lookup"><span data-stu-id="7d6e4-108">The format of a metadata file is as follows:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
[<metadata-name-1>metadata-value-1</metadata-name-1>]  
[<metadata-name-2>metadata-value-2</metadata-name-2>]  
. . .  
</Metadata>  
```
  
|<span data-ttu-id="7d6e4-109">XML-elem.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-109">XML Element</span></span>|<span data-ttu-id="7d6e4-110">Típus</span><span class="sxs-lookup"><span data-stu-id="7d6e4-110">Type</span></span>|<span data-ttu-id="7d6e4-111">Leírás</span><span class="sxs-lookup"><span data-stu-id="7d6e4-111">Description</span></span>|  
|-----------------|----------|-----------------|  
|`Metadata`|<span data-ttu-id="7d6e4-112">Legfelső szintű elem</span><span class="sxs-lookup"><span data-stu-id="7d6e4-112">Root element</span></span>|<span data-ttu-id="7d6e4-113">A metaadat-fájl gyökérelemének.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-113">The root element of the metadata file.</span></span>|  
|`metadata-name`|<span data-ttu-id="7d6e4-114">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d6e4-114">String</span></span>|<span data-ttu-id="7d6e4-115">Választható.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-115">Optional.</span></span> <span data-ttu-id="7d6e4-116">Az XML-elem a BLOB metaadatait nevét adja meg, és az értékét a metadata beállítás értékét adja meg.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-116">The XML element specifies the name of the metadata for the blob, and its value specifies the value of the metadata setting.</span></span>|  
  
## <a name="properties-file-format"></a><span data-ttu-id="7d6e4-117">Tulajdonságok fájlformátum</span><span class="sxs-lookup"><span data-stu-id="7d6e4-117">Properties file format</span></span>  
<span data-ttu-id="7d6e4-118">A tulajdonságok fájl formátuma a következő:</span><span class="sxs-lookup"><span data-stu-id="7d6e4-118">The format of a properties file is as follows:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
[<Last-Modified>date-time-value</Last-Modified>]  
[<Etag>etag</Etag>]  
[<Content-Length>size-in-bytes<Content-Length>]  
[<Content-Type>content-type</Content-Type>]  
[<Content-MD5>content-md5</Content-MD5>]  
[<Content-Encoding>content-encoding</Content-Encoding>]  
[<Content-Language>content-language</Content-Language>]  
[<Cache-Control>cache-control</Cache-Control>]  
</Properties>  
```
  
|<span data-ttu-id="7d6e4-119">XML-elem.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-119">XML Element</span></span>|<span data-ttu-id="7d6e4-120">Típus</span><span class="sxs-lookup"><span data-stu-id="7d6e4-120">Type</span></span>|<span data-ttu-id="7d6e4-121">Leírás</span><span class="sxs-lookup"><span data-stu-id="7d6e4-121">Description</span></span>|  
|-----------------|----------|-----------------|  
|`Properties`|<span data-ttu-id="7d6e4-122">Legfelső szintű elem</span><span class="sxs-lookup"><span data-stu-id="7d6e4-122">Root element</span></span>|<span data-ttu-id="7d6e4-123">A tulajdonságok fájl gyökérelemének.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-123">The root element of the properties file.</span></span>|  
|`Last-Modified`|<span data-ttu-id="7d6e4-124">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d6e4-124">String</span></span>|<span data-ttu-id="7d6e4-125">Választható.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-125">Optional.</span></span> <span data-ttu-id="7d6e4-126">Az utolsó módosítás dátuma a BLOB.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-126">The last-modified time for the blob.</span></span> <span data-ttu-id="7d6e4-127">Az exportálási feladatok csak.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-127">For export jobs only.</span></span>|  
|`Etag`|<span data-ttu-id="7d6e4-128">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d6e4-128">String</span></span>|<span data-ttu-id="7d6e4-129">Választható.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-129">Optional.</span></span> <span data-ttu-id="7d6e4-130">A blob ETag érték.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-130">The blob's ETag value.</span></span> <span data-ttu-id="7d6e4-131">Az exportálási feladatok csak.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-131">For export jobs only.</span></span>|  
|`Content-Length`|<span data-ttu-id="7d6e4-132">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d6e4-132">String</span></span>|<span data-ttu-id="7d6e4-133">Választható.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-133">Optional.</span></span> <span data-ttu-id="7d6e4-134">A blob bájtban kifejezett mérete.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-134">The size of the blob in bytes.</span></span> <span data-ttu-id="7d6e4-135">Az exportálási feladatok csak.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-135">For export jobs only.</span></span>|  
|`Content-Type`|<span data-ttu-id="7d6e4-136">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d6e4-136">String</span></span>|<span data-ttu-id="7d6e4-137">Választható.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-137">Optional.</span></span> <span data-ttu-id="7d6e4-138">A blob tartalomtípusa.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-138">The content type of the blob.</span></span>|  
|`Content-MD5`|<span data-ttu-id="7d6e4-139">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d6e4-139">String</span></span>|<span data-ttu-id="7d6e4-140">Választható.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-140">Optional.</span></span> <span data-ttu-id="7d6e4-141">A blob MD5 kivonatoló.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-141">The blob's MD5 hash.</span></span>|  
|`Content-Encoding`|<span data-ttu-id="7d6e4-142">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d6e4-142">String</span></span>|<span data-ttu-id="7d6e4-143">Választható.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-143">Optional.</span></span> <span data-ttu-id="7d6e4-144">A blob tartalma kódolást.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-144">The blob's content encoding.</span></span>|  
|`Content-Language`|<span data-ttu-id="7d6e4-145">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d6e4-145">String</span></span>|<span data-ttu-id="7d6e4-146">Választható.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-146">Optional.</span></span> <span data-ttu-id="7d6e4-147">A blob tartalom kívánt nyelvét.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-147">The blob's content language.</span></span>|  
|`Cache-Control`|<span data-ttu-id="7d6e4-148">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d6e4-148">String</span></span>|<span data-ttu-id="7d6e4-149">Választható.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-149">Optional.</span></span> <span data-ttu-id="7d6e4-150">A blob vezérlő gyorsítótár-karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-150">The cache control string for the blob.</span></span>|  

## <a name="next-steps"></a><span data-ttu-id="7d6e4-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7d6e4-151">Next steps</span></span>

<span data-ttu-id="7d6e4-152">Lásd: [blob tulajdonságainak beállítása](/rest/api/storageservices/set-blob-properties), [beállítása Blob metaadatai](/rest/api/storageservices/set-blob-metadata), és [beállítás és lekérése közben tulajdonságait és a metaadatok blob-erőforrások](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources) a blob metaadatai és tulajdonságok beállításával kapcsolatos részletes szabályok.</span><span class="sxs-lookup"><span data-stu-id="7d6e4-152">See [Set blob properties](/rest/api/storageservices/set-blob-properties), [Set Blob Metadata](/rest/api/storageservices/set-blob-metadata), and [Setting and retrieving properties and metadata for blob resources](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources) for detailed rules about setting blob metadata and properties.</span></span>
