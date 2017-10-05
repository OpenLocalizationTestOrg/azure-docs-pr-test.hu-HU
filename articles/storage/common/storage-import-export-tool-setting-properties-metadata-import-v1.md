---
title: "Tulajdonságok és a metaadatok Azure Import/Export - 1-es verzió beállítása |} Microsoft Docs"
description: "Megtudhatja, hogyan adhatja meg az tulajdonságait és a metaadatok állítható be a cél BLOB az Azure Import/Export eszköz futtatásakor a meghajtók előkészítéséhez. A v1 az Import/Export eszköz vonatkozik."
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
ms.openlocfilehash: 77bdaa5559de86cd1de9f30e70656e47fd5719e2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="setting-properties-and-metadata-during-the-import-process"></a><span data-ttu-id="e1ad6-104">Tulajdonságok és metaadatok beállítása az importálási folyamat során</span><span class="sxs-lookup"><span data-stu-id="e1ad6-104">Setting properties and metadata during the import process</span></span>
<span data-ttu-id="e1ad6-105">A meghajtók előkészítése a Microsoft Azure Import/Export eszköz futtatásakor tulajdonságok és állítható be a cél BLOB metaadatait is megadhat.</span><span class="sxs-lookup"><span data-stu-id="e1ad6-105">When you run the Microsoft Azure Import/Export Tool to prepare your drives, you can specify properties and metadata to be set on the destination blobs.</span></span> <span data-ttu-id="e1ad6-106">Kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="e1ad6-106">Follow these steps:</span></span>  
  
1.  <span data-ttu-id="e1ad6-107">Blob tulajdonságainak beállításához hozzon létre egy szövegfájlt, a helyi számítógépen, amely meghatározza a nevét és értékeit.</span><span class="sxs-lookup"><span data-stu-id="e1ad6-107">To set blob properties, create a text file on your local computer that specifies property names and values.</span></span>  
  
2.  <span data-ttu-id="e1ad6-108">Állítsa be a blob metaadatai, hozzon létre egy szövegfájlt, a helyi számítógépen, amely meghatározza a metaadatok neveit és értékeit.</span><span class="sxs-lookup"><span data-stu-id="e1ad6-108">To set blob metadata, create a text file on your local computer that specifies metadata names and values.</span></span>  
  
3.  <span data-ttu-id="e1ad6-109">A teljes elérési útja át egyik vagy mindkét ezeket a fájlokat az Azure Import/Export eszköz részeként a `PrepImport` műveletet.</span><span class="sxs-lookup"><span data-stu-id="e1ad6-109">Pass the full path to one or both of these files to the Azure Import/Export Tool as part of the `PrepImport` operation.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="e1ad6-110">Amikor egy tulajdonságok vagy metaadat-fájlt ad meg egy másolási munkamenet részeként, ezeket a tulajdonságokat vagy metaadatok vannak állítva minden BLOB másolási munkamenet részeként importált.</span><span class="sxs-lookup"><span data-stu-id="e1ad6-110">When you specify a properties or metadata file as part of a copy session, those properties or metadata are set for every blob that is imported as part of that copy session.</span></span> <span data-ttu-id="e1ad6-111">Adjon meg másik tulajdonságok vagy metaadatok egyes importált blobok szeretnénk, ha szüksége hozzon létre egy külön példányát munkamenetet más tulajdonságokkal vagy metaadat-fájlok.</span><span class="sxs-lookup"><span data-stu-id="e1ad6-111">If you want to specify a different set of properties or metadata for some of the blobs being imported, you'll need to create a separate copy session with different properties or metadata files.</span></span>  
  
## <a name="specify-blob-properties-in-a-text-file"></a><span data-ttu-id="e1ad6-112">Adja meg a Blob tulajdonságai szövegfájl</span><span class="sxs-lookup"><span data-stu-id="e1ad6-112">Specify Blob Properties in a Text File</span></span>  
<span data-ttu-id="e1ad6-113">Adja meg a blob tulajdonságai, hozzon létre egy helyi szövegfájlt, és tartalmazza, amely meghatározza a tulajdonság nevét, és tulajdonságértékeket értékként XML.</span><span class="sxs-lookup"><span data-stu-id="e1ad6-113">To specify blob properties, create a local text file, and include XML that specifies property names as elements, and property values as values.</span></span> <span data-ttu-id="e1ad6-114">Íme egy példa, amely meghatározza az egyes tulajdonságértékeket:</span><span class="sxs-lookup"><span data-stu-id="e1ad6-114">Here's an example that specifies some property values:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
<span data-ttu-id="e1ad6-115">Mentse a fájlt egy helyi helyre, például a `C:\WAImportExport\ImportProperties.txt`.</span><span class="sxs-lookup"><span data-stu-id="e1ad6-115">Save the file to a local location like `C:\WAImportExport\ImportProperties.txt`.</span></span>  
  
## <a name="specify-blob-metadata-in-a-text-file"></a><span data-ttu-id="e1ad6-116">Adja meg a Blob metaadatai szövegfájl</span><span class="sxs-lookup"><span data-stu-id="e1ad6-116">Specify Blob Metadata in a Text File</span></span>  
<span data-ttu-id="e1ad6-117">Hasonlóképpen adja meg a blob metaadatai, hozzon létre a helyi szövegfájl, amely meghatározza a metaadatok nevét, és metaadatok értékek értékként.</span><span class="sxs-lookup"><span data-stu-id="e1ad6-117">Similarly, to specify blob metadata, create a local text file that specifies metadata names as elements, and metadata values as values.</span></span> <span data-ttu-id="e1ad6-118">Íme egy példa, amely néhány metaadatok értékeket adja meg:</span><span class="sxs-lookup"><span data-stu-id="e1ad6-118">Here's an example that specifies some metadata values:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>  
    <DataSetName>SampleData</DataSetName>  
    <CreationDate>10/1/2013</CreationDate>  
</Metadata>  
```
  
<span data-ttu-id="e1ad6-119">Mentse a fájlt egy helyi helyre, például a `C:\WAImportExport\ImportMetadata.txt`.</span><span class="sxs-lookup"><span data-stu-id="e1ad6-119">Save the file to a local location like `C:\WAImportExport\ImportMetadata.txt`.</span></span>  
  
## <a name="create-a-copy-session-including-the-properties-or-metadata-files"></a><span data-ttu-id="e1ad6-120">Beleértve a Tulajdonságok vagy metaadat-fájlok másolása munkamenet létrehozása</span><span class="sxs-lookup"><span data-stu-id="e1ad6-120">Create a Copy Session Including the Properties or Metadata Files</span></span>  
<span data-ttu-id="e1ad6-121">Az Azure Import/Export eszköz előkészítése az importálási feladat futtatásakor adja meg a parancssor használatával tulajdonságok fájlt a `PropertyFile` paraméter.</span><span class="sxs-lookup"><span data-stu-id="e1ad6-121">When you run the Azure Import/Export Tool to prepare the import job, specify the properties file on the command line using the `PropertyFile` parameter.</span></span> <span data-ttu-id="e1ad6-122">Adja meg a metaadat-fájlt a parancssor használatával a `/MetadataFile` paraméter.</span><span class="sxs-lookup"><span data-stu-id="e1ad6-122">Specify the metadata file on the command line using the `/MetadataFile` parameter.</span></span> <span data-ttu-id="e1ad6-123">Íme egy példa, amely mindkét fájlokat:</span><span class="sxs-lookup"><span data-stu-id="e1ad6-123">Here's an example that specifies both files:</span></span>  
  
```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```
  
## <a name="next-steps"></a><span data-ttu-id="e1ad6-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e1ad6-124">Next steps</span></span>

* [<span data-ttu-id="e1ad6-125">Az Import/Export szolgáltatás metaadat- és tulajdonságfájljainak formátuma</span><span class="sxs-lookup"><span data-stu-id="e1ad6-125">Import/Export service metadata and properties file format</span></span>](../storage-import-export-file-format-metadata-and-properties.md)
