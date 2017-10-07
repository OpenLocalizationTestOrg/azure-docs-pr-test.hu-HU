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
# <a name="azure-importexport-service-metadata-and-properties-file-format"></a>Az Azure Import/Export szolgáltatás metaadatok és a Tulajdonságok fájlformátum
Megadhat egy vagy több blobot tulajdonságai és metaadatok egy importálási feladat vagy exportálási feladat részeként. tooset metaadatok vagy egy importálási feladat részeként létrehozott BLOB tulajdonságai, meg kell adnia egy metaadatok vagy tulajdonságai-fájlt tartalmazó hello adatok toobe importált hello merevlemezre. Exportálási feladat metaadatok és a Tulajdonságok írja tooa metaadatok vagy tulajdonságok fájl merevlemez-meghajtón hello tooyou adott vissza.  
  
## <a name="metadata-file-format"></a>Metaadatok fájlformátum  
hello a metaadatfájl formátuma a következő:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
[<metadata-name-1>metadata-value-1</metadata-name-1>]  
[<metadata-name-2>metadata-value-2</metadata-name-2>]  
. . .  
</Metadata>  
```
  
|XML-elem.|Típus|Leírás|  
|-----------------|----------|-----------------|  
|`Metadata`|Legfelső szintű elem|hello hello metaadatait tartalmazó fájl gyökérelemének.|  
|`metadata-name`|Karakterlánc|Választható. hello XML-elem hello blob hello metaadatok hello nevét határozza meg, és az értékét megadja hello hello metaadatok beállítás értékét.|  
  
## <a name="properties-file-format"></a>Tulajdonságok fájlformátum  
hello tulajdonságok fájl formátuma a következő:  
  
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
  
|XML-elem.|Típus|Leírás|  
|-----------------|----------|-----------------|  
|`Properties`|Legfelső szintű elem|hello hello tulajdonságok fájl gyökérelemének.|  
|`Last-Modified`|Karakterlánc|Választható. hello utolsó módosításának ideje hello blob. Az exportálási feladatok csak.|  
|`Etag`|Karakterlánc|Választható. hello blob ETag érték. Az exportálási feladatok csak.|  
|`Content-Length`|Karakterlánc|Választható. hello mérete bájtban hello blob. Az exportálási feladatok csak.|  
|`Content-Type`|Karakterlánc|Választható. hello tartalomtípus hello BLOB.|  
|`Content-MD5`|Karakterlánc|Választható. blob MD5 kivonatoló hello.|  
|`Content-Encoding`|Karakterlánc|Választható. hello blob tartalom kódolása.|  
|`Content-Language`|Karakterlánc|Választható. hello blob a tartalom kívánt nyelvét.|  
|`Cache-Control`|Karakterlánc|Választható. hello gyorsítótár vezérlő karakterlánc hello BLOB.|  

## <a name="next-steps"></a>Következő lépések

Lásd: [blob tulajdonságainak beállítása](/rest/api/storageservices/set-blob-properties), [beállítása Blob metaadatai](/rest/api/storageservices/set-blob-metadata), és [beállítás és lekérése közben tulajdonságait és a metaadatok blob-erőforrások](/rest/api/storageservices/setting-and-retrieving-properties-and-metadata-for-blob-resources) a blob metaadatai és tulajdonságok beállításával kapcsolatos részletes szabályok.
