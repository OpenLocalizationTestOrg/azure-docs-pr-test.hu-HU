---
title: "aaaAzure-importálási/exportálási metaadatok és a Tulajdonságok fájlformátum |} Microsoft Docs"
description: "Megtudhatja, hogyan toospecify metaadatok és egy vagy több tulajdonságok blobok, amelyek az importálás során vagy exportálja a feladatot."
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
ms.openlocfilehash: bb13c1f1a27baea77298cb224970cd521d02d8c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-importexport-service-metadata-and-properties-file-format"></a><span data-ttu-id="7c84c-103">Az Azure Import/Export szolgáltatás metaadatok és a Tulajdonságok fájlformátum</span><span class="sxs-lookup"><span data-stu-id="7c84c-103">Azure Import/Export service metadata and properties file format</span></span>
<span data-ttu-id="7c84c-104">Megadhat egy vagy több blobot tulajdonságai és metaadatok egy importálási feladat vagy exportálási feladat részeként.</span><span class="sxs-lookup"><span data-stu-id="7c84c-104">You can specify metadata and properties for one or more blobs as part of an import job or an export job.</span></span> <span data-ttu-id="7c84c-105">tooset metaadatok vagy egy importálási feladat részeként létrehozott BLOB tulajdonságai, meg kell adnia egy metaadatok vagy tulajdonságai-fájlt tartalmazó hello adatok toobe importált hello merevlemezre.</span><span class="sxs-lookup"><span data-stu-id="7c84c-105">tooset metadata or properties for blobs being created as part of an import job, you provide a metadata or properties file on hello hard drive containing hello data toobe imported.</span></span> <span data-ttu-id="7c84c-106">Exportálási feladat metaadatok és a Tulajdonságok írja tooa metaadatok vagy tulajdonságok fájl merevlemez-meghajtón hello tooyou adott vissza.</span><span class="sxs-lookup"><span data-stu-id="7c84c-106">For an export job, metadata and properties are written tooa metadata or properties file that is included on hello hard drive returned tooyou.</span></span>  
  
## <a name="metadata-file-format"></a><span data-ttu-id="7c84c-107">Metaadatok fájlformátum</span><span class="sxs-lookup"><span data-stu-id="7c84c-107">Metadata file format</span></span>  
<span data-ttu-id="7c84c-108">hello a metaadatfájl formátuma a következő:</span><span class="sxs-lookup"><span data-stu-id="7c84c-108">hello format of a metadata file is as follows:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
[<metadata-name-1>metadata-value-1</metadata-name-1>]  
[<metadata-name-2>metadata-value-2</metadata-name-2>]  
. . .  
</Metadata>  
```
  
|<span data-ttu-id="7c84c-109">XML-elem.</span><span class="sxs-lookup"><span data-stu-id="7c84c-109">XML Element</span></span>|<span data-ttu-id="7c84c-110">Típus</span><span class="sxs-lookup"><span data-stu-id="7c84c-110">Type</span></span>|<span data-ttu-id="7c84c-111">Leírás</span><span class="sxs-lookup"><span data-stu-id="7c84c-111">Description</span></span>|  
|-----------------|----------|-----------------|  
|`Metadata`|<span data-ttu-id="7c84c-112">Legfelső szintű elem</span><span class="sxs-lookup"><span data-stu-id="7c84c-112">Root element</span></span>|<span data-ttu-id="7c84c-113">hello hello metaadatait tartalmazó fájl gyökérelemének.</span><span class="sxs-lookup"><span data-stu-id="7c84c-113">hello root element of hello metadata file.</span></span>|  
|`metadata-name`|<span data-ttu-id="7c84c-114">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7c84c-114">String</span></span>|<span data-ttu-id="7c84c-115">Választható.</span><span class="sxs-lookup"><span data-stu-id="7c84c-115">Optional.</span></span> <span data-ttu-id="7c84c-116">hello XML-elem hello blob hello metaadatok hello nevét határozza meg, és az értékét megadja hello hello metaadatok beállítás értékét.</span><span class="sxs-lookup"><span data-stu-id="7c84c-116">hello XML element specifies hello name of hello metadata for hello blob, and its value specifies hello value of hello metadata setting.</span></span>|  
  
## <a name="properties-file-format"></a><span data-ttu-id="7c84c-117">Tulajdonságok fájlformátum</span><span class="sxs-lookup"><span data-stu-id="7c84c-117">Properties file format</span></span>  
<span data-ttu-id="7c84c-118">hello tulajdonságok fájl formátuma a következő:</span><span class="sxs-lookup"><span data-stu-id="7c84c-118">hello format of a properties file is as follows:</span></span>  
  
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
  
|<span data-ttu-id="7c84c-119">XML-elem.</span><span class="sxs-lookup"><span data-stu-id="7c84c-119">XML Element</span></span>|<span data-ttu-id="7c84c-120">Típus</span><span class="sxs-lookup"><span data-stu-id="7c84c-120">Type</span></span>|<span data-ttu-id="7c84c-121">Leírás</span><span class="sxs-lookup"><span data-stu-id="7c84c-121">Description</span></span>|  
|-----------------|----------|-----------------|  
|`Properties`|<span data-ttu-id="7c84c-122">Legfelső szintű elem</span><span class="sxs-lookup"><span data-stu-id="7c84c-122">Root element</span></span>|<span data-ttu-id="7c84c-123">hello hello tulajdonságok fájl gyökérelemének.</span><span class="sxs-lookup"><span data-stu-id="7c84c-123">hello root element of hello properties file.</span></span>|  
|`Last-Modified`|<span data-ttu-id="7c84c-124">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7c84c-124">String</span></span>|<span data-ttu-id="7c84c-125">Választható.</span><span class="sxs-lookup"><span data-stu-id="7c84c-125">Optional.</span></span> <span data-ttu-id="7c84c-126">hello utolsó módosításának ideje hello blob.</span><span class="sxs-lookup"><span data-stu-id="7c84c-126">hello last-modified time for hello blob.</span></span> <span data-ttu-id="7c84c-127">Az exportálási feladatok csak.</span><span class="sxs-lookup"><span data-stu-id="7c84c-127">For export jobs only.</span></span>|  
|`Etag`|<span data-ttu-id="7c84c-128">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7c84c-128">String</span></span>|<span data-ttu-id="7c84c-129">Választható.</span><span class="sxs-lookup"><span data-stu-id="7c84c-129">Optional.</span></span> <span data-ttu-id="7c84c-130">hello blob ETag érték.</span><span class="sxs-lookup"><span data-stu-id="7c84c-130">hello blob's ETag value.</span></span> <span data-ttu-id="7c84c-131">Az exportálási feladatok csak.</span><span class="sxs-lookup"><span data-stu-id="7c84c-131">For export jobs only.</span></span>|  
|`Content-Length`|<span data-ttu-id="7c84c-132">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7c84c-132">String</span></span>|<span data-ttu-id="7c84c-133">Választható.</span><span class="sxs-lookup"><span data-stu-id="7c84c-133">Optional.</span></span> <span data-ttu-id="7c84c-134">hello mérete bájtban hello blob.</span><span class="sxs-lookup"><span data-stu-id="7c84c-134">hello size of hello blob in bytes.</span></span> <span data-ttu-id="7c84c-135">Az exportálási feladatok csak.</span><span class="sxs-lookup"><span data-stu-id="7c84c-135">For export jobs only.</span></span>|  
|`Content-Type`|<span data-ttu-id="7c84c-136">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7c84c-136">String</span></span>|<span data-ttu-id="7c84c-137">Választható.</span><span class="sxs-lookup"><span data-stu-id="7c84c-137">Optional.</span></span> <span data-ttu-id="7c84c-138">hello tartalomtípus hello BLOB.</span><span class="sxs-lookup"><span data-stu-id="7c84c-138">hello content type of hello blob.</span></span>|  
|`Content-MD5`|<span data-ttu-id="7c84c-139">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7c84c-139">String</span></span>|<span data-ttu-id="7c84c-140">Választható.</span><span class="sxs-lookup"><span data-stu-id="7c84c-140">Optional.</span></span> <span data-ttu-id="7c84c-141">blob MD5 kivonatoló hello.</span><span class="sxs-lookup"><span data-stu-id="7c84c-141">hello blob's MD5 hash.</span></span>|  
|`Content-Encoding`|<span data-ttu-id="7c84c-142">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7c84c-142">String</span></span>|<span data-ttu-id="7c84c-143">Választható.</span><span class="sxs-lookup"><span data-stu-id="7c84c-143">Optional.</span></span> <span data-ttu-id="7c84c-144">hello blob tartalom kódolása.</span><span class="sxs-lookup"><span data-stu-id="7c84c-144">hello blob's content encoding.</span></span>|  
|`Content-Language`|<span data-ttu-id="7c84c-145">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7c84c-145">String</span></span>|<span data-ttu-id="7c84c-146">Választható.</span><span class="sxs-lookup"><span data-stu-id="7c84c-146">Optional.</span></span> <span data-ttu-id="7c84c-147">hello blob a tartalom kívánt nyelvét.</span><span class="sxs-lookup"><span data-stu-id="7c84c-147">hello blob's content language.</span></span>|  
|`Cache-Control`|<span data-ttu-id="7c84c-148">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7c84c-148">String</span></span>|<span data-ttu-id="7c84c-149">Választható.</span><span class="sxs-lookup"><span data-stu-id="7c84c-149">Optional.</span></span> <span data-ttu-id="7c84c-150">hello gyorsítótár vezérlő karakterlánc hello BLOB.</span><span class="sxs-lookup"><span data-stu-id="7c84c-150">hello cache control string for hello blob.</span></span>|  

## <a name="next-steps"></a><span data-ttu-id="7c84c-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7c84c-151">Next steps</span></span>

<span data-ttu-id="7c84c-152">Lásd: [blob tulajdonságainak beállítása](/rest/api/storageservices/set-blob-properties), [beállítása Blob metaadatai](/rest/api/storageservices/set-blob-metadata), és [beállítás és lekérése közben tulajdonságait és a metaadatok blob-erőforrások](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources) a blob metaadatai és tulajdonságok beállításával kapcsolatos részletes szabályok.</span><span class="sxs-lookup"><span data-stu-id="7c84c-152">See [Set blob properties](/rest/api/storageservices/set-blob-properties), [Set Blob Metadata](/rest/api/storageservices/set-blob-metadata), and [Setting and retrieving properties and metadata for blob resources](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources) for detailed rules about setting blob metadata and properties.</span></span>
