---
title: "aaaSetting tulajdonságok és az Azure Import/Export metaadatok |} Microsoft Docs"
description: "Ismerje meg, hogyan toospecify tulajdonságait és a metaadatok toobe be hello cél blobok a meghajtók hello Azure Import/Export eszköz tooprepare futtatásakor."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 05c2b13bead793c8ab5aac6ce25816be97fffb14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="setting-properties-and-metadata-during-hello-import-process"></a><span data-ttu-id="7017a-103">A tulajdonságok és metaadatok során hello importálási folyamat</span><span class="sxs-lookup"><span data-stu-id="7017a-103">Setting properties and metadata during hello import process</span></span>

<span data-ttu-id="7017a-104">Hello Microsoft Azure Import/Export eszköz tooprepare futtatásakor a meghajtók tulajdonságait és a metaadatok toobe hello cél blobok beállítása is megadhat.</span><span class="sxs-lookup"><span data-stu-id="7017a-104">When you run hello Microsoft Azure Import/Export Tool tooprepare your drives, you can specify properties and metadata toobe set on hello destination blobs.</span></span> <span data-ttu-id="7017a-105">Kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="7017a-105">Follow these steps:</span></span>

1.  <span data-ttu-id="7017a-106">tooset blob tulajdonságai, hozzon létre egy szövegfájlt, a helyi számítógépen, amely meghatározza a nevét és értékeit.</span><span class="sxs-lookup"><span data-stu-id="7017a-106">tooset blob properties, create a text file on your local computer that specifies property names and values.</span></span>
2.  <span data-ttu-id="7017a-107">tooset metaadatok blob, hozzon létre egy szövegfájlt, a helyi számítógépen, amely meghatározza a metaadatok neveit és értékeit.</span><span class="sxs-lookup"><span data-stu-id="7017a-107">tooset blob metadata, create a text file on your local computer that specifies metadata names and values.</span></span>
3.  <span data-ttu-id="7017a-108">Hello teljes elérési útja tooone vagy mindkét ezen fájlok toohello Azure Import/Export eszköz átadni hello részeként `PrepImport` műveletet.</span><span class="sxs-lookup"><span data-stu-id="7017a-108">Pass hello full path tooone or both of these files toohello Azure Import/Export Tool as part of hello `PrepImport` operation.</span></span>

> [!NOTE]
>  <span data-ttu-id="7017a-109">Amikor egy tulajdonságok vagy metaadat-fájlt ad meg egy másolási munkamenet részeként, ezeket a tulajdonságokat vagy metaadatok vannak állítva minden BLOB másolási munkamenet részeként importált.</span><span class="sxs-lookup"><span data-stu-id="7017a-109">When you specify a properties or metadata file as part of a copy session, those properties or metadata are set for every blob that is imported as part of that copy session.</span></span> <span data-ttu-id="7017a-110">Ha egyes importált hello blobok toospecify tulajdonságok vagy metaadatok különböző készlete, külön munkamenetet más tulajdonságokkal vagy metaadat-fájlok másolása toocreate lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="7017a-110">If you want toospecify a different set of properties or metadata for some of hello blobs being imported, you'll need toocreate a separate copy session with different properties or metadata files.</span></span>

## <a name="specify-blob-properties-in-a-text-file"></a><span data-ttu-id="7017a-111">Adja meg a blob tulajdonságai szövegfájl</span><span class="sxs-lookup"><span data-stu-id="7017a-111">Specify blob properties in a text file</span></span>

<span data-ttu-id="7017a-112">toospecify blob tulajdonságai, hozzon létre egy helyi szövegfájlt, és tartalmazza, amely meghatározza a tulajdonság nevét, és tulajdonságértékeket értékként XML.</span><span class="sxs-lookup"><span data-stu-id="7017a-112">toospecify blob properties, create a local text file, and include XML that specifies property names as elements, and property values as values.</span></span> <span data-ttu-id="7017a-113">Íme egy példa, amely meghatározza az egyes tulajdonságértékeket:</span><span class="sxs-lookup"><span data-stu-id="7017a-113">Here's an example that specifies some property values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Properties>
    <Content-Type>application/octet-stream</Content-Type>
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>
    <Cache-Control>no-cache</Cache-Control>
</Properties>
```

<span data-ttu-id="7017a-114">Mentés hello fájl tooa helyi helyen, például `C:\WAImportExport\ImportProperties.txt`.</span><span class="sxs-lookup"><span data-stu-id="7017a-114">Save hello file tooa local location like `C:\WAImportExport\ImportProperties.txt`.</span></span>

## <a name="specify-blob-metadata-in-a-text-file"></a><span data-ttu-id="7017a-115">Adja meg a blob metaadatai szövegfájl</span><span class="sxs-lookup"><span data-stu-id="7017a-115">Specify blob metadata in a text file</span></span>

<span data-ttu-id="7017a-116">Ehhez hasonlóan toospecify metaadatok blob, hozzon létre egy helyi szövegfájlt, amely meghatározza a metaadatok nevét, és metaadatok értékek értékként.</span><span class="sxs-lookup"><span data-stu-id="7017a-116">Similarly, toospecify blob metadata, create a local text file that specifies metadata names as elements, and metadata values as values.</span></span> <span data-ttu-id="7017a-117">Íme egy példa, amely néhány metaadatok értékeket adja meg:</span><span class="sxs-lookup"><span data-stu-id="7017a-117">Here's an example that specifies some metadata values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Metadata>
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>
    <DataSetName>SampleData</DataSetName>
    <CreationDate>10/1/2013</CreationDate>
</Metadata>
```

<span data-ttu-id="7017a-118">Mentés hello fájl tooa helyi helyen, például `C:\WAImportExport\ImportMetadata.txt`.</span><span class="sxs-lookup"><span data-stu-id="7017a-118">Save hello file tooa local location like `C:\WAImportExport\ImportMetadata.txt`.</span></span>

## <a name="add-hello-path-tooproperties-and-metadata-files-in-datasetcsv"></a><span data-ttu-id="7017a-119">A dataset.csv hello elérési tooproperties és metaadat-fájlok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7017a-119">Add hello path tooproperties and metadata files in dataset.csv</span></span>

```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
H:\Video\,https://mystorageaccount.blob.core.windows.net/video/,BlockBlob,rename,None,H:\mydirectory\properties.xml
H:\Photo\,https://mystorageaccount.blob.core.windows.net/photo/,BlockBlob,rename,None,H:\mydirectory\properties.xml
K:\Temp\FavoriteVideo.ISO,https://mystorageaccount.blob.core.windows.net/favorite/FavoriteVideo.ISO,BlockBlob,rename,None,H:\mydirectory\properties.xml
\\myshare\john\music\,https://mystorageaccount.blob.core.windows.net/music/,BlockBlob,rename,None,H:\mydirectory\properties.xml
```

## <a name="next-steps"></a><span data-ttu-id="7017a-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7017a-120">Next steps</span></span>

* [<span data-ttu-id="7017a-121">Az Import/Export szolgáltatás metaadat- és tulajdonságfájljainak formátuma</span><span class="sxs-lookup"><span data-stu-id="7017a-121">Import/Export service metadata and properties file format</span></span>](storage-import-export-file-format-metadata-and-properties.md)
