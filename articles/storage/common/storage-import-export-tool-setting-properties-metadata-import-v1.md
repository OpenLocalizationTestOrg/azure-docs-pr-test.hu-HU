---
title: "aaaSetting tulajdonságait és a metaadatok Azure Import/Export - 1-es verzió |} Microsoft Docs"
description: "Ismerje meg, hogyan toospecify tulajdonságait és a metaadatok toobe be hello cél blobok a meghajtók hello Azure Import/Export eszköz tooprepare futtatásakor. Az Import/Export eszköz hello toov1 utal."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: e8541695-bcfb-4bf0-84d9-72c9de32a0a8
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 5b7b1c346ecde8a26d985bd5de7efcf7d86eb9e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="setting-properties-and-metadata-during-hello-import-process"></a>A tulajdonságok és metaadatok során hello importálási folyamat
Hello Microsoft Azure Import/Export eszköz tooprepare futtatásakor a meghajtók tulajdonságait és a metaadatok toobe hello cél blobok beállítása is megadhat. Kövesse az alábbi lépéseket:  
  
1.  tooset blob tulajdonságai, hozzon létre egy szövegfájlt, a helyi számítógépen, amely meghatározza a nevét és értékeit.  
  
2.  tooset metaadatok blob, hozzon létre egy szövegfájlt, a helyi számítógépen, amely meghatározza a metaadatok neveit és értékeit.  
  
3.  Hello teljes elérési útja tooone vagy mindkét ezen fájlok toohello Azure Import/Export eszköz átadni hello részeként `PrepImport` műveletet.  
  
> [!NOTE]
>  Amikor egy tulajdonságok vagy metaadat-fájlt ad meg egy másolási munkamenet részeként, ezeket a tulajdonságokat vagy metaadatok vannak állítva minden BLOB másolási munkamenet részeként importált. Ha egyes importált hello blobok toospecify tulajdonságok vagy metaadatok különböző készlete, külön munkamenetet más tulajdonságokkal vagy metaadat-fájlok másolása toocreate lesz szüksége.  
  
## <a name="specify-blob-properties-in-a-text-file"></a>Adja meg a Blob tulajdonságai szövegfájl  
toospecify blob tulajdonságai, hozzon létre egy helyi szövegfájlt, és tartalmazza, amely meghatározza a tulajdonság nevét, és tulajdonságértékeket értékként XML. Íme egy példa, amely meghatározza az egyes tulajdonságértékeket:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
Mentés hello fájl tooa helyi helyen, például `C:\WAImportExport\ImportProperties.txt`.  
  
## <a name="specify-blob-metadata-in-a-text-file"></a>Adja meg a Blob metaadatai szövegfájl  
Ehhez hasonlóan toospecify metaadatok blob, hozzon létre egy helyi szövegfájlt, amely meghatározza a metaadatok nevét, és metaadatok értékek értékként. Íme egy példa, amely néhány metaadatok értékeket adja meg:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>  
    <DataSetName>SampleData</DataSetName>  
    <CreationDate>10/1/2013</CreationDate>  
</Metadata>  
```
  
Mentés hello fájl tooa helyi helyen, például `C:\WAImportExport\ImportMetadata.txt`.  
  
## <a name="create-a-copy-session-including-hello-properties-or-metadata-files"></a>Hozzon létre egy másolatot munkamenet beleértve hello tulajdonságok vagy metaadat-fájlok  
Hello Azure Import/Export eszköz tooprepare hello importálási feladat futtatásakor adja meg a hello tulajdonságok fájl használatával hello hello parancssorban `PropertyFile` paraméter. Adja meg a hello metaadatfájl hello segítségével hello parancssorban `/MetadataFile` paraméter. Íme egy példa, amely mindkét fájlokat:  
  
```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```
  
## <a name="next-steps"></a>Következő lépések

* [Az Import/Export szolgáltatás metaadat- és tulajdonságfájljainak formátuma](../storage-import-export-file-format-metadata-and-properties.md)
