---
title: "Adatok előkészítése az Azure Import/Export importálási feladatnak merevlemezek minta munkafolyamat |} Microsoft Docs"
description: "Lásd: a forgatókönyv a teljes folyamat meghajtók előkészítése az Azure Import/Export szolgáltatás egy importálási feladat."
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
ms.openlocfilehash: 78d7ce3bbd3205fd995ba331af08d830097c8156
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="sample-workflow-to-prepare-hard-drives-for-an-import-job"></a><span data-ttu-id="116ec-103">Munkafolyamat-minta a merevlemezek importálási feladatokhoz való előkészítésére</span><span class="sxs-lookup"><span data-stu-id="116ec-103">Sample workflow to prepare hard drives for an import job</span></span>

<span data-ttu-id="116ec-104">Ez a cikk végigvezeti a teljes folyamat meghajtók előkészítése az importálási feladat.</span><span class="sxs-lookup"><span data-stu-id="116ec-104">This article walks you through the complete process of preparing drives for an import job.</span></span>

## <a name="sample-data"></a><span data-ttu-id="116ec-105">Mintaadatok</span><span class="sxs-lookup"><span data-stu-id="116ec-105">Sample data</span></span>

<span data-ttu-id="116ec-106">Ebben a példában a következő adatokat importálja az Azure-tárfiók nevű `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="116ec-106">This example imports the following data into an Azure storage account named `mystorageaccount`:</span></span>

|<span data-ttu-id="116ec-107">Hely</span><span class="sxs-lookup"><span data-stu-id="116ec-107">Location</span></span>|<span data-ttu-id="116ec-108">Leírás</span><span class="sxs-lookup"><span data-stu-id="116ec-108">Description</span></span>|<span data-ttu-id="116ec-109">Adatok mérete</span><span class="sxs-lookup"><span data-stu-id="116ec-109">Data size</span></span>|
|--------------|-----------------|-----|
|<span data-ttu-id="116ec-110">H:\Video\\</span><span class="sxs-lookup"><span data-stu-id="116ec-110">H:\Video\\</span></span> |<span data-ttu-id="116ec-111">Videók gyűjteménye</span><span class="sxs-lookup"><span data-stu-id="116ec-111">A collection of videos</span></span>|<span data-ttu-id="116ec-112">12 TB</span><span class="sxs-lookup"><span data-stu-id="116ec-112">12 TB</span></span>|
|<span data-ttu-id="116ec-113">H:\Photo\\</span><span class="sxs-lookup"><span data-stu-id="116ec-113">H:\Photo\\</span></span> |<span data-ttu-id="116ec-114">Fotók</span><span class="sxs-lookup"><span data-stu-id="116ec-114">A collection of photos</span></span>|<span data-ttu-id="116ec-115">30 GB</span><span class="sxs-lookup"><span data-stu-id="116ec-115">30 GB</span></span>|
|<span data-ttu-id="116ec-116">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="116ec-116">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="116ec-117">A Blu-ray™ lemezképe</span><span class="sxs-lookup"><span data-stu-id="116ec-117">A Blu-Ray™ disk image</span></span>|<span data-ttu-id="116ec-118">25 GB</span><span class="sxs-lookup"><span data-stu-id="116ec-118">25 GB</span></span>|
|<span data-ttu-id="116ec-119">\\\bigshare\john\music\\</span><span class="sxs-lookup"><span data-stu-id="116ec-119">\\\bigshare\john\music\\</span></span>|<span data-ttu-id="116ec-120">Letilthatja a zenei fájlok egy hálózati megosztáson</span><span class="sxs-lookup"><span data-stu-id="116ec-120">A collection of music files on a network share</span></span>|<span data-ttu-id="116ec-121">10 GB</span><span class="sxs-lookup"><span data-stu-id="116ec-121">10 GB</span></span>|

## <a name="storage-account-destinations"></a><span data-ttu-id="116ec-122">Tárolási fiók célok</span><span class="sxs-lookup"><span data-stu-id="116ec-122">Storage account destinations</span></span>

<span data-ttu-id="116ec-123">Az importálási feladat az adatok importálása a storage-fiókot a következő célhoz:</span><span class="sxs-lookup"><span data-stu-id="116ec-123">The import job will import the data into the following destinations in the storage account:</span></span>

|<span data-ttu-id="116ec-124">Forrás</span><span class="sxs-lookup"><span data-stu-id="116ec-124">Source</span></span>|<span data-ttu-id="116ec-125">Cél virtuális könyvtárat vagy blob</span><span class="sxs-lookup"><span data-stu-id="116ec-125">Destination virtual directory or blob</span></span>|
|------------|-------------------------------------------|
|<span data-ttu-id="116ec-126">H:\Video\\</span><span class="sxs-lookup"><span data-stu-id="116ec-126">H:\Video\\</span></span> |<span data-ttu-id="116ec-127">videó /</span><span class="sxs-lookup"><span data-stu-id="116ec-127">video/</span></span>|
|<span data-ttu-id="116ec-128">H:\Photo\\</span><span class="sxs-lookup"><span data-stu-id="116ec-128">H:\Photo\\</span></span> |<span data-ttu-id="116ec-129">fénykép vagy</span><span class="sxs-lookup"><span data-stu-id="116ec-129">photo/</span></span>|
|<span data-ttu-id="116ec-130">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="116ec-130">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="116ec-131">favorite/FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="116ec-131">favorite/FavoriteMovies.ISO</span></span>|
|<span data-ttu-id="116ec-132">\\\bigshare\john\music\\</span><span class="sxs-lookup"><span data-stu-id="116ec-132">\\\bigshare\john\music\\</span></span> |<span data-ttu-id="116ec-133">Zene</span><span class="sxs-lookup"><span data-stu-id="116ec-133">music</span></span>|

<span data-ttu-id="116ec-134">A hozzárendelést, a fájl a `H:\Video\Drama\GreatMovie.mov` importálhatja, hogy a blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span><span class="sxs-lookup"><span data-stu-id="116ec-134">With this mapping, the file `H:\Video\Drama\GreatMovie.mov` will be imported to the blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span></span>

## <a name="determine-hard-drive-requirements"></a><span data-ttu-id="116ec-135">Merevlemez-meghajtóról követelmények meghatározása</span><span class="sxs-lookup"><span data-stu-id="116ec-135">Determine hard drive requirements</span></span>

<span data-ttu-id="116ec-136">Ezt követően annak megállapításához, hogy hány merevlemezek szükségesek, számítási az adatok mérete:</span><span class="sxs-lookup"><span data-stu-id="116ec-136">Next, to determine how many hard drives are needed, compute the size of the data:</span></span>

`12TB + 30GB + 25GB + 10GB = 12TB + 65GB`

<span data-ttu-id="116ec-137">Az ebben a példában két 8 TB-os merevlemezeket elegendőnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="116ec-137">For this example, two 8TB hard drives should be sufficient.</span></span> <span data-ttu-id="116ec-138">Mivel azonban a forráskönyvtár `H:\Video` 12TB adatot, és az egyetlen merevlemez-területtel csak 8TB, adja meg a következő módon a ez lesz a **driveset.csv** fájlt:</span><span class="sxs-lookup"><span data-stu-id="116ec-138">However, since the source directory `H:\Video` has 12TB of data and your single hard drive's capacity is only 8TB, you will be able to specify this in the following way in the **driveset.csv** file:</span></span>

```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
X,Format,SilentMode,Encrypt,
Y,Format,SilentMode,Encrypt,
```
<span data-ttu-id="116ec-139">Az eszköz fogja el az adatokat két merevlemez-meghajtók optimalizált módon.</span><span class="sxs-lookup"><span data-stu-id="116ec-139">The tool will distribute data across two hard drives in an optimized way.</span></span>

## <a name="attach-drives-and-configure-the-job"></a><span data-ttu-id="116ec-140">Meghajtók csatolja, és a feladat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="116ec-140">Attach drives and configure the job</span></span>
<span data-ttu-id="116ec-141">Ön mindkét lemez csatolása a géphez, és hozzon létre köteteket.</span><span class="sxs-lookup"><span data-stu-id="116ec-141">You will attach both disks to the machine and create volumes.</span></span> <span data-ttu-id="116ec-142">Majd szerzői **dataset.csv** fájlt:</span><span class="sxs-lookup"><span data-stu-id="116ec-142">Then author **dataset.csv** file:</span></span>
```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
H:\Video\,video/,BlockBlob,rename,None,H:\mydirectory\properties.xml
H:\Photo\,photo/,BlockBlob,rename,None,H:\mydirectory\properties.xml
K:\Temp\FavoriteVideo.ISO,favorite/FavoriteVideo.ISO,BlockBlob,rename,None,H:\mydirectory\properties.xml
\\myshare\john\music\,music/,BlockBlob,rename,None,H:\mydirectory\properties.xml
```

<span data-ttu-id="116ec-143">Ezenkívül a következő metaadatokat az összes fájl állíthatja be:</span><span class="sxs-lookup"><span data-stu-id="116ec-143">In addition, you can set the following metadata for all files:</span></span>

* <span data-ttu-id="116ec-144">**UploadMethod:** Windows Azure Import/Export szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="116ec-144">**UploadMethod:** Windows Azure Import/Export service</span></span>
* <span data-ttu-id="116ec-145">**DataSetName:** SampleData</span><span class="sxs-lookup"><span data-stu-id="116ec-145">**DataSetName:** SampleData</span></span>
* <span data-ttu-id="116ec-146">**CreationDate:** 10/1/2013</span><span class="sxs-lookup"><span data-stu-id="116ec-146">**CreationDate:** 10/1/2013</span></span>

<span data-ttu-id="116ec-147">Ha szeretné beállítani az importált fájlok metaadatait, hozzon létre egy szövegfájlt, `c:\WAImportExport\SampleMetadata.txt`, a következő tartalommal:</span><span class="sxs-lookup"><span data-stu-id="116ec-147">To set metadata for the imported files, create a text file, `c:\WAImportExport\SampleMetadata.txt`, with the following content:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Metadata>
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>
    <DataSetName>SampleData</DataSetName>
    <CreationDate>10/1/2013</CreationDate>
</Metadata>
```

<span data-ttu-id="116ec-148">Bizonyos tulajdonságait is beállíthat a `FavoriteMovie.ISO` blob:</span><span class="sxs-lookup"><span data-stu-id="116ec-148">You can also set some properties for the `FavoriteMovie.ISO` blob:</span></span>

* <span data-ttu-id="116ec-149">**Content-Type:** application/octet-stream</span><span class="sxs-lookup"><span data-stu-id="116ec-149">**Content-Type:** application/octet-stream</span></span>
* <span data-ttu-id="116ec-150">**Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==</span><span class="sxs-lookup"><span data-stu-id="116ec-150">**Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ==</span></span>
* <span data-ttu-id="116ec-151">**A Cache-Control:** no-cache</span><span class="sxs-lookup"><span data-stu-id="116ec-151">**Cache-Control:** no-cache</span></span>

<span data-ttu-id="116ec-152">A tulajdonságok beállításáról, hozzon létre egy szövegfájlt `c:\WAImportExport\SampleProperties.txt`:</span><span class="sxs-lookup"><span data-stu-id="116ec-152">To set these properties, create a text file, `c:\WAImportExport\SampleProperties.txt`:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Properties>
    <Content-Type>application/octet-stream</Content-Type>
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>
    <Cache-Control>no-cache</Cache-Control>
</Properties>
```

## <a name="run-the-azure-importexport-tool-waimportexportexe"></a><span data-ttu-id="116ec-153">Futtassa az Azure Import/Export eszköz (WAImportExport.exe)</span><span class="sxs-lookup"><span data-stu-id="116ec-153">Run the Azure Import/Export Tool (WAImportExport.exe)</span></span>

<span data-ttu-id="116ec-154">Most már készen áll a két merevlemez-meghajtók előkészítése az Azure Import/Export eszköz futtatásához.</span><span class="sxs-lookup"><span data-stu-id="116ec-154">Now you are ready to run the Azure Import/Export Tool to prepare the two hard drives.</span></span>

<span data-ttu-id="116ec-155">**Az első munkamenet:**</span><span class="sxs-lookup"><span data-stu-id="116ec-155">**For the first session:**</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

<span data-ttu-id="116ec-156">További adatokat hozzá kell adni, ha hozzon létre egy másik dataset fájlt (Initialdataset megegyező formátumban).</span><span class="sxs-lookup"><span data-stu-id="116ec-156">If any more data needs to be added, create another dataset file (same format as Initialdataset).</span></span>

<span data-ttu-id="116ec-157">**A második munkamenethez:**</span><span class="sxs-lookup"><span data-stu-id="116ec-157">**For the second session:**</span></span>

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

<span data-ttu-id="116ec-158">Miután végzett a másolat munkamenetek, a két meghajtók leválasztása a másolási számítógépről, és küldje el azokat a megfelelő Azure adatközpontba.</span><span class="sxs-lookup"><span data-stu-id="116ec-158">Once the copy sessions have completed, you can disconnect the two drives from the copy computer and ship them to the appropriate Azure data center.</span></span> <span data-ttu-id="116ec-159">A két Adatbázisnapló-fájlok feltöltése lesz `<FirstDriveSerialNumber>.xml` és `<SecondDriveSerialNumber>.xml`, az importálási feladat létrehozásakor az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="116ec-159">You'll upload the two journal files, `<FirstDriveSerialNumber>.xml` and `<SecondDriveSerialNumber>.xml`, when you create the import job in the Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="116ec-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="116ec-160">Next steps</span></span>

* [<span data-ttu-id="116ec-161">Merevlemezek előkészítése importálási feladatokhoz</span><span class="sxs-lookup"><span data-stu-id="116ec-161">Preparing hard drives for an import job</span></span>](storage-import-export-tool-preparing-hard-drives-import.md)
* [<span data-ttu-id="116ec-162">A gyakran használt parancsok rövid összefoglalása</span><span class="sxs-lookup"><span data-stu-id="116ec-162">Quick reference for frequently used commands</span></span>](storage-import-export-tool-quick-reference.md)
