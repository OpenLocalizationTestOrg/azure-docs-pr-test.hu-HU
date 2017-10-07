---
title: "aaaSample munkafolyamat tooprep merevlemez-meghajtók az Azure Import/Export importálási feladat |} Microsoft Docs"
description: "Tekintse meg a teljes folyamat hello meghajtók előkészítése az importálási feladat hello Azure Import/Export szolgáltatás egy bemutató."
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
ms.date: 04/07/2017
ms.author: muralikk
ms.openlocfilehash: 560220b7dc9f87416f1fec1ff30fa5cd65812ce5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sample-workflow-tooprepare-hard-drives-for-an-import-job"></a><span data-ttu-id="f0e18-103">Minta munkafolyamat tooprepare merevlemezeit, az importálási feladat</span><span class="sxs-lookup"><span data-stu-id="f0e18-103">Sample workflow tooprepare hard drives for an import job</span></span>

<span data-ttu-id="f0e18-104">Ez a cikk bemutatja, hogyan hello teljes folyamata meghajtók előkészítése az importálási feladat.</span><span class="sxs-lookup"><span data-stu-id="f0e18-104">This article walks you through hello complete process of preparing drives for an import job.</span></span>

## <a name="sample-data"></a><span data-ttu-id="f0e18-105">Mintaadatok</span><span class="sxs-lookup"><span data-stu-id="f0e18-105">Sample data</span></span>

<span data-ttu-id="f0e18-106">Ez a példa importál adatokat az Azure-tárfiók neve a következő hello `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="f0e18-106">This example imports hello following data into an Azure storage account named `mystorageaccount`:</span></span>

|<span data-ttu-id="f0e18-107">Hely</span><span class="sxs-lookup"><span data-stu-id="f0e18-107">Location</span></span>|<span data-ttu-id="f0e18-108">Leírás</span><span class="sxs-lookup"><span data-stu-id="f0e18-108">Description</span></span>|<span data-ttu-id="f0e18-109">Adatok mérete</span><span class="sxs-lookup"><span data-stu-id="f0e18-109">Data size</span></span>|
|--------------|-----------------|-----|
|<span data-ttu-id="f0e18-110">H:\Video\\</span><span class="sxs-lookup"><span data-stu-id="f0e18-110">H:\Video\\</span></span> |<span data-ttu-id="f0e18-111">Videók gyűjteménye</span><span class="sxs-lookup"><span data-stu-id="f0e18-111">A collection of videos</span></span>|<span data-ttu-id="f0e18-112">12 TB</span><span class="sxs-lookup"><span data-stu-id="f0e18-112">12 TB</span></span>|
|<span data-ttu-id="f0e18-113">H:\Photo\\</span><span class="sxs-lookup"><span data-stu-id="f0e18-113">H:\Photo\\</span></span> |<span data-ttu-id="f0e18-114">Fotók</span><span class="sxs-lookup"><span data-stu-id="f0e18-114">A collection of photos</span></span>|<span data-ttu-id="f0e18-115">30 GB</span><span class="sxs-lookup"><span data-stu-id="f0e18-115">30 GB</span></span>|
|<span data-ttu-id="f0e18-116">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="f0e18-116">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="f0e18-117">A Blu-ray™ lemezképe</span><span class="sxs-lookup"><span data-stu-id="f0e18-117">A Blu-Ray™ disk image</span></span>|<span data-ttu-id="f0e18-118">25 GB</span><span class="sxs-lookup"><span data-stu-id="f0e18-118">25 GB</span></span>|
|<span data-ttu-id="f0e18-119">\\\bigshare\john\music\\</span><span class="sxs-lookup"><span data-stu-id="f0e18-119">\\\bigshare\john\music\\</span></span>|<span data-ttu-id="f0e18-120">Letilthatja a zenei fájlok egy hálózati megosztáson</span><span class="sxs-lookup"><span data-stu-id="f0e18-120">A collection of music files on a network share</span></span>|<span data-ttu-id="f0e18-121">10 GB</span><span class="sxs-lookup"><span data-stu-id="f0e18-121">10 GB</span></span>|

## <a name="storage-account-destinations"></a><span data-ttu-id="f0e18-122">Tárolási fiók célok</span><span class="sxs-lookup"><span data-stu-id="f0e18-122">Storage account destinations</span></span>

<span data-ttu-id="f0e18-123">hello importálási feladat hello adatok importálása a következő célhoz hello tárfiók hello:</span><span class="sxs-lookup"><span data-stu-id="f0e18-123">hello import job will import hello data into hello following destinations in hello storage account:</span></span>

|<span data-ttu-id="f0e18-124">Forrás</span><span class="sxs-lookup"><span data-stu-id="f0e18-124">Source</span></span>|<span data-ttu-id="f0e18-125">Cél virtuális könyvtárat vagy blob</span><span class="sxs-lookup"><span data-stu-id="f0e18-125">Destination virtual directory or blob</span></span>|
|------------|-------------------------------------------|
|<span data-ttu-id="f0e18-126">H:\Video\\</span><span class="sxs-lookup"><span data-stu-id="f0e18-126">H:\Video\\</span></span> |<span data-ttu-id="f0e18-127">videó /</span><span class="sxs-lookup"><span data-stu-id="f0e18-127">video/</span></span>|
|<span data-ttu-id="f0e18-128">H:\Photo\\</span><span class="sxs-lookup"><span data-stu-id="f0e18-128">H:\Photo\\</span></span> |<span data-ttu-id="f0e18-129">fénykép vagy</span><span class="sxs-lookup"><span data-stu-id="f0e18-129">photo/</span></span>|
|<span data-ttu-id="f0e18-130">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="f0e18-130">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="f0e18-131">favorite/FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="f0e18-131">favorite/FavoriteMovies.ISO</span></span>|
|<span data-ttu-id="f0e18-132">\\\bigshare\john\music\\</span><span class="sxs-lookup"><span data-stu-id="f0e18-132">\\\bigshare\john\music\\</span></span> |<span data-ttu-id="f0e18-133">Zene</span><span class="sxs-lookup"><span data-stu-id="f0e18-133">music</span></span>|

<span data-ttu-id="f0e18-134">Ez a leképezés a hello fájl `H:\Video\Drama\GreatMovie.mov` importált toohello blob lesz `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span><span class="sxs-lookup"><span data-stu-id="f0e18-134">With this mapping, hello file `H:\Video\Drama\GreatMovie.mov` will be imported toohello blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span></span>

## <a name="determine-hard-drive-requirements"></a><span data-ttu-id="f0e18-135">Merevlemez-meghajtóról követelmények meghatározása</span><span class="sxs-lookup"><span data-stu-id="f0e18-135">Determine hard drive requirements</span></span>

<span data-ttu-id="f0e18-136">Ezt követően toodetermine hány merevlemezek szükségesek, számítási hello hello adatok mérete:</span><span class="sxs-lookup"><span data-stu-id="f0e18-136">Next, toodetermine how many hard drives are needed, compute hello size of hello data:</span></span>

`12TB + 30GB + 25GB + 10GB = 12TB + 65GB`

<span data-ttu-id="f0e18-137">Az ebben a példában két 8 TB-os merevlemezeket elegendőnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f0e18-137">For this example, two 8TB hard drives should be sufficient.</span></span> <span data-ttu-id="f0e18-138">Hello forráskönyvtár mivel azonban `H:\Video` 12TB adatot, és az egyetlen merevlemez-területtel csak 8TB, fogja tudni toospecify ezt a következő módon hello hello **driveset.csv** fájlt:</span><span class="sxs-lookup"><span data-stu-id="f0e18-138">However, since hello source directory `H:\Video` has 12TB of data and your single hard drive's capacity is only 8TB, you will be able toospecify this in hello following way in hello **driveset.csv** file:</span></span>

```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
X,Format,SilentMode,Encrypt,
Y,Format,SilentMode,Encrypt,
```
<span data-ttu-id="f0e18-139">hello eszköz lesz el az adatokat két merevlemez-meghajtók optimalizált módon.</span><span class="sxs-lookup"><span data-stu-id="f0e18-139">hello tool will distribute data across two hard drives in an optimized way.</span></span>

## <a name="attach-drives-and-configure-hello-job"></a><span data-ttu-id="f0e18-140">Meghajtó csatlakoztatása és hello feladat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f0e18-140">Attach drives and configure hello job</span></span>
<span data-ttu-id="f0e18-141">A rendszer Csatlakoztassa mindkét lemezek toohello gép, és hozzon létre köteteket.</span><span class="sxs-lookup"><span data-stu-id="f0e18-141">You will attach both disks toohello machine and create volumes.</span></span> <span data-ttu-id="f0e18-142">Majd szerzői **dataset.csv** fájlt:</span><span class="sxs-lookup"><span data-stu-id="f0e18-142">Then author **dataset.csv** file:</span></span>
```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
H:\Video\,video/,BlockBlob,rename,None,H:\mydirectory\properties.xml
H:\Photo\,photo/,BlockBlob,rename,None,H:\mydirectory\properties.xml
K:\Temp\FavoriteVideo.ISO,favorite/FavoriteVideo.ISO,BlockBlob,rename,None,H:\mydirectory\properties.xml
\\myshare\john\music\,music/,BlockBlob,rename,None,H:\mydirectory\properties.xml
```

<span data-ttu-id="f0e18-143">Ezenkívül a következő összes fájlok metaadatait hello állíthatja be:</span><span class="sxs-lookup"><span data-stu-id="f0e18-143">In addition, you can set hello following metadata for all files:</span></span>

* <span data-ttu-id="f0e18-144">**UploadMethod:** Windows Azure Import/Export szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="f0e18-144">**UploadMethod:** Windows Azure Import/Export service</span></span>
* <span data-ttu-id="f0e18-145">**DataSetName:** SampleData</span><span class="sxs-lookup"><span data-stu-id="f0e18-145">**DataSetName:** SampleData</span></span>
* <span data-ttu-id="f0e18-146">**CreationDate:** 10/1/2013</span><span class="sxs-lookup"><span data-stu-id="f0e18-146">**CreationDate:** 10/1/2013</span></span>

<span data-ttu-id="f0e18-147">importált hello fájlok metaadatait tooset hozzon létre egy szövegfájlt, `c:\WAImportExport\SampleMetadata.txt`, a tartalom a következő hello:</span><span class="sxs-lookup"><span data-stu-id="f0e18-147">tooset metadata for hello imported files, create a text file, `c:\WAImportExport\SampleMetadata.txt`, with hello following content:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Metadata>
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>
    <DataSetName>SampleData</DataSetName>
    <CreationDate>10/1/2013</CreationDate>
</Metadata>
```

<span data-ttu-id="f0e18-148">Hello bizonyos tulajdonságait is beállíthat `FavoriteMovie.ISO` blob:</span><span class="sxs-lookup"><span data-stu-id="f0e18-148">You can also set some properties for hello `FavoriteMovie.ISO` blob:</span></span>

* <span data-ttu-id="f0e18-149">**Content-Type:** application/octet-stream</span><span class="sxs-lookup"><span data-stu-id="f0e18-149">**Content-Type:** application/octet-stream</span></span>
* <span data-ttu-id="f0e18-150">**Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==</span><span class="sxs-lookup"><span data-stu-id="f0e18-150">**Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ==</span></span>
* <span data-ttu-id="f0e18-151">**A Cache-Control:** no-cache</span><span class="sxs-lookup"><span data-stu-id="f0e18-151">**Cache-Control:** no-cache</span></span>

<span data-ttu-id="f0e18-152">tooset ezeket a tulajdonságokat, hozzon létre egy szövegfájlt `c:\WAImportExport\SampleProperties.txt`:</span><span class="sxs-lookup"><span data-stu-id="f0e18-152">tooset these properties, create a text file, `c:\WAImportExport\SampleProperties.txt`:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Properties>
    <Content-Type>application/octet-stream</Content-Type>
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>
    <Cache-Control>no-cache</Cache-Control>
</Properties>
```

## <a name="run-hello-azure-importexport-tool-waimportexportexe"></a><span data-ttu-id="f0e18-153">Futtatási hello Azure Import/Export eszköz (WAImportExport.exe)</span><span class="sxs-lookup"><span data-stu-id="f0e18-153">Run hello Azure Import/Export Tool (WAImportExport.exe)</span></span>

<span data-ttu-id="f0e18-154">Most már készen áll a toorun hello Azure Import/Export eszköz tooprepare hello két merevlemez-meghajtók áll.</span><span class="sxs-lookup"><span data-stu-id="f0e18-154">Now you are ready toorun hello Azure Import/Export Tool tooprepare hello two hard drives.</span></span>

<span data-ttu-id="f0e18-155">**Az első munkamenet hello:**</span><span class="sxs-lookup"><span data-stu-id="f0e18-155">**For hello first session:**</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

<span data-ttu-id="f0e18-156">Ha több adat hozzáadott toobe, hozzon létre egy másik dataset fájlt (Initialdataset megegyező formátumban).</span><span class="sxs-lookup"><span data-stu-id="f0e18-156">If any more data needs toobe added, create another dataset file (same format as Initialdataset).</span></span>

<span data-ttu-id="f0e18-157">**A második munkamenet hello:**</span><span class="sxs-lookup"><span data-stu-id="f0e18-157">**For hello second session:**</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

<span data-ttu-id="f0e18-158">Hello másolási munkamenetek befejezése után hello két meghajtók leválasztása hello másolási számítógépről, és toohello megfelelő Azure-adatközpont küldje el.</span><span class="sxs-lookup"><span data-stu-id="f0e18-158">Once hello copy sessions have completed, you can disconnect hello two drives from hello copy computer and ship them toohello appropriate Azure data center.</span></span> <span data-ttu-id="f0e18-159">Hello két Adatbázisnapló-fájlok feltöltése lesz `<FirstDriveSerialNumber>.xml` és `<SecondDriveSerialNumber>.xml`, hello Azure-portálon hello importálási feladat létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="f0e18-159">You'll upload hello two journal files, `<FirstDriveSerialNumber>.xml` and `<SecondDriveSerialNumber>.xml`, when you create hello import job in hello Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f0e18-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f0e18-160">Next steps</span></span>

* [<span data-ttu-id="f0e18-161">Merevlemezek előkészítése importálási feladatokhoz</span><span class="sxs-lookup"><span data-stu-id="f0e18-161">Preparing hard drives for an import job</span></span>](storage-import-export-tool-preparing-hard-drives-import.md)
* [<span data-ttu-id="f0e18-162">A gyakran használt parancsok rövid összefoglalása</span><span class="sxs-lookup"><span data-stu-id="f0e18-162">Quick reference for frequently used commands</span></span>](storage-import-export-tool-quick-reference.md)
