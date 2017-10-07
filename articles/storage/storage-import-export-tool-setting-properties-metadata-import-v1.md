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
ms.openlocfilehash: 66e55c2076fbcda9b78302f17b5ff2cf96bb24e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="setting-properties-and-metadata-during-hello-import-process"></a><span data-ttu-id="03a4a-104">A tulajdonságok és metaadatok során hello importálási folyamat</span><span class="sxs-lookup"><span data-stu-id="03a4a-104">Setting properties and metadata during hello import process</span></span>
<span data-ttu-id="03a4a-105">Hello Microsoft Azure Import/Export eszköz tooprepare futtatásakor a meghajtók tulajdonságait és a metaadatok toobe hello cél blobok beállítása is megadhat.</span><span class="sxs-lookup"><span data-stu-id="03a4a-105">When you run hello Microsoft Azure Import/Export Tool tooprepare your drives, you can specify properties and metadata toobe set on hello destination blobs.</span></span> <span data-ttu-id="03a4a-106">Kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="03a4a-106">Follow these steps:</span></span>  
  
1.  <span data-ttu-id="03a4a-107">tooset blob tulajdonságai, hozzon létre egy szövegfájlt, a helyi számítógépen, amely meghatározza a nevét és értékeit.</span><span class="sxs-lookup"><span data-stu-id="03a4a-107">tooset blob properties, create a text file on your local computer that specifies property names and values.</span></span>  
  
2.  <span data-ttu-id="03a4a-108">tooset metaadatok blob, hozzon létre egy szövegfájlt, a helyi számítógépen, amely meghatározza a metaadatok neveit és értékeit.</span><span class="sxs-lookup"><span data-stu-id="03a4a-108">tooset blob metadata, create a text file on your local computer that specifies metadata names and values.</span></span>  
  
3.  <span data-ttu-id="03a4a-109">Hello teljes elérési útja tooone vagy mindkét ezen fájlok toohello Azure Import/Export eszköz átadni hello részeként `PrepImport` műveletet.</span><span class="sxs-lookup"><span data-stu-id="03a4a-109">Pass hello full path tooone or both of these files toohello Azure Import/Export Tool as part of hello `PrepImport` operation.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="03a4a-110">Amikor egy tulajdonságok vagy metaadat-fájlt ad meg egy másolási munkamenet részeként, ezeket a tulajdonságokat vagy metaadatok vannak állítva minden BLOB másolási munkamenet részeként importált.</span><span class="sxs-lookup"><span data-stu-id="03a4a-110">When you specify a properties or metadata file as part of a copy session, those properties or metadata are set for every blob that is imported as part of that copy session.</span></span> <span data-ttu-id="03a4a-111">Ha egyes importált hello blobok toospecify tulajdonságok vagy metaadatok különböző készlete, külön munkamenetet más tulajdonságokkal vagy metaadat-fájlok másolása toocreate lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="03a4a-111">If you want toospecify a different set of properties or metadata for some of hello blobs being imported, you'll need toocreate a separate copy session with different properties or metadata files.</span></span>  
  
## <a name="specify-blob-properties-in-a-text-file"></a><span data-ttu-id="03a4a-112">Adja meg a Blob tulajdonságai szövegfájl</span><span class="sxs-lookup"><span data-stu-id="03a4a-112">Specify Blob Properties in a Text File</span></span>  
<span data-ttu-id="03a4a-113">toospecify blob tulajdonságai, hozzon létre egy helyi szövegfájlt, és tartalmazza, amely meghatározza a tulajdonság nevét, és tulajdonságértékeket értékként XML.</span><span class="sxs-lookup"><span data-stu-id="03a4a-113">toospecify blob properties, create a local text file, and include XML that specifies property names as elements, and property values as values.</span></span> <span data-ttu-id="03a4a-114">Íme egy példa, amely meghatározza az egyes tulajdonságértékeket:</span><span class="sxs-lookup"><span data-stu-id="03a4a-114">Here's an example that specifies some property values:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
<span data-ttu-id="03a4a-115">Mentés hello fájl tooa helyi helyen, például `C:\WAImportExport\ImportProperties.txt`.</span><span class="sxs-lookup"><span data-stu-id="03a4a-115">Save hello file tooa local location like `C:\WAImportExport\ImportProperties.txt`.</span></span>  
  
## <a name="specify-blob-metadata-in-a-text-file"></a><span data-ttu-id="03a4a-116">Adja meg a Blob metaadatai szövegfájl</span><span class="sxs-lookup"><span data-stu-id="03a4a-116">Specify Blob Metadata in a Text File</span></span>  
<span data-ttu-id="03a4a-117">Ehhez hasonlóan toospecify metaadatok blob, hozzon létre egy helyi szövegfájlt, amely meghatározza a metaadatok nevét, és metaadatok értékek értékként.</span><span class="sxs-lookup"><span data-stu-id="03a4a-117">Similarly, toospecify blob metadata, create a local text file that specifies metadata names as elements, and metadata values as values.</span></span> <span data-ttu-id="03a4a-118">Íme egy példa, amely néhány metaadatok értékeket adja meg:</span><span class="sxs-lookup"><span data-stu-id="03a4a-118">Here's an example that specifies some metadata values:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>  
    <DataSetName>SampleData</DataSetName>  
    <CreationDate>10/1/2013</CreationDate>  
</Metadata>  
```
  
<span data-ttu-id="03a4a-119">Mentés hello fájl tooa helyi helyen, például `C:\WAImportExport\ImportMetadata.txt`.</span><span class="sxs-lookup"><span data-stu-id="03a4a-119">Save hello file tooa local location like `C:\WAImportExport\ImportMetadata.txt`.</span></span>  
  
## <a name="create-a-copy-session-including-hello-properties-or-metadata-files"></a><span data-ttu-id="03a4a-120">Hozzon létre egy másolatot munkamenet beleértve hello tulajdonságok vagy metaadat-fájlok</span><span class="sxs-lookup"><span data-stu-id="03a4a-120">Create a Copy Session Including hello Properties or Metadata Files</span></span>  
<span data-ttu-id="03a4a-121">Hello Azure Import/Export eszköz tooprepare hello importálási feladat futtatásakor adja meg a hello tulajdonságok fájl használatával hello hello parancssorban `PropertyFile` paraméter.</span><span class="sxs-lookup"><span data-stu-id="03a4a-121">When you run hello Azure Import/Export Tool tooprepare hello import job, specify hello properties file on hello command line using hello `PropertyFile` parameter.</span></span> <span data-ttu-id="03a4a-122">Adja meg a hello metaadatfájl hello segítségével hello parancssorban `/MetadataFile` paraméter.</span><span class="sxs-lookup"><span data-stu-id="03a4a-122">Specify hello metadata file on hello command line using hello `/MetadataFile` parameter.</span></span> <span data-ttu-id="03a4a-123">Íme egy példa, amely mindkét fájlokat:</span><span class="sxs-lookup"><span data-stu-id="03a4a-123">Here's an example that specifies both files:</span></span>  
  
```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```
  
## <a name="next-steps"></a><span data-ttu-id="03a4a-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="03a4a-124">Next steps</span></span>

* [<span data-ttu-id="03a4a-125">Az Import/Export szolgáltatás metaadat- és tulajdonságfájljainak formátuma</span><span class="sxs-lookup"><span data-stu-id="03a4a-125">Import/Export service metadata and properties file format</span></span>](storage-import-export-file-format-metadata-and-properties.md)
