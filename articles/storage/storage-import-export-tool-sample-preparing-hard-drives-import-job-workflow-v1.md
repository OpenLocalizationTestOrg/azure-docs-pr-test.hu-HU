---
title: "Adatok előkészítése az Azure Import/Export importálási feladat - v1 merevlemezeit, a minta-munkafolyamat |} Microsoft Docs"
description: "Lásd: a forgatókönyv a teljes folyamat meghajtók előkészítése az Azure Import/Export szolgáltatás egy importálási feladat."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 6eb1b1b7-c69f-4365-b5ef-3cd5e05eb72a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 313f8c1f3962a943b4c98c530c324ff28aa84c10
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="sample-workflow-to-prepare-hard-drives-for-an-import-job"></a><span data-ttu-id="ad46b-103">Munkafolyamat-minta a merevlemezek importálási feladatokhoz való előkészítésére</span><span class="sxs-lookup"><span data-stu-id="ad46b-103">Sample workflow to prepare hard drives for an import job</span></span>
<span data-ttu-id="ad46b-104">Ez a témakör végigvezeti a teljes folyamat meghajtók előkészítése az importálási feladat.</span><span class="sxs-lookup"><span data-stu-id="ad46b-104">This topic walks you through the complete process of preparing drives for an import job.</span></span>  
  
<span data-ttu-id="ad46b-105">Ebben a példában a következő adatok importálása a Windows Azure storage-fiók nevű `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="ad46b-105">This example imports the following data into a Window Azure storage account named `mystorageaccount`:</span></span>  
  
|<span data-ttu-id="ad46b-106">Hely</span><span class="sxs-lookup"><span data-stu-id="ad46b-106">Location</span></span>|<span data-ttu-id="ad46b-107">Leírás</span><span class="sxs-lookup"><span data-stu-id="ad46b-107">Description</span></span>|  
|--------------|-----------------|  
|<span data-ttu-id="ad46b-108">H:\Video</span><span class="sxs-lookup"><span data-stu-id="ad46b-108">H:\Video</span></span>|<span data-ttu-id="ad46b-109">Gyűjteménye videók, összesen 5 TB.</span><span class="sxs-lookup"><span data-stu-id="ad46b-109">A collection of videos, 5 TB in total.</span></span>|  
|<span data-ttu-id="ad46b-110">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="ad46b-110">H:\Photo</span></span>|<span data-ttu-id="ad46b-111">Egy gyűjtemény fényképek, összesen 30 GB.</span><span class="sxs-lookup"><span data-stu-id="ad46b-111">A collection of photos, 30 GB in total.</span></span>|  
|<span data-ttu-id="ad46b-112">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="ad46b-112">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="ad46b-113">A Blu-ray™ lemezképét, 25 GB.</span><span class="sxs-lookup"><span data-stu-id="ad46b-113">A Blu-Ray™ disk image, 25 GB.</span></span>|  
|<span data-ttu-id="ad46b-114">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="ad46b-114">\\\bigshare\john\music</span></span>|<span data-ttu-id="ad46b-115">Letilthatja a zenei fájlok egy hálózati megosztáson, 10 GB-os teljes gyűjteményét.</span><span class="sxs-lookup"><span data-stu-id="ad46b-115">A collection of music files on a network share, 10 GB in total.</span></span>|  
  
<span data-ttu-id="ad46b-116">Az importálási feladat ezek az adatok importálása a storage-fiókot a következő célhoz:</span><span class="sxs-lookup"><span data-stu-id="ad46b-116">The import job will import this data into the following destinations in the storage account:</span></span>  
  
|<span data-ttu-id="ad46b-117">Forrás</span><span class="sxs-lookup"><span data-stu-id="ad46b-117">Source</span></span>|<span data-ttu-id="ad46b-118">Cél virtuális könyvtárat vagy blob</span><span class="sxs-lookup"><span data-stu-id="ad46b-118">Destination virtual directory or blob</span></span>|  
|------------|-------------------------------------------|  
|<span data-ttu-id="ad46b-119">H:\Video</span><span class="sxs-lookup"><span data-stu-id="ad46b-119">H:\Video</span></span>|<span data-ttu-id="ad46b-120">https://mystorageaccount.BLOB.Core.Windows.NET/video</span><span class="sxs-lookup"><span data-stu-id="ad46b-120">https://mystorageaccount.blob.core.windows.net/video</span></span>|  
|<span data-ttu-id="ad46b-121">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="ad46b-121">H:\Photo</span></span>|<span data-ttu-id="ad46b-122">https://mystorageaccount.BLOB.Core.Windows.NET/Photo</span><span class="sxs-lookup"><span data-stu-id="ad46b-122">https://mystorageaccount.blob.core.windows.net/photo</span></span>|  
|<span data-ttu-id="ad46b-123">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="ad46b-123">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="ad46b-124">https://mystorageaccount.BLOB.Core.Windows.NET/Favorite/FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="ad46b-124">https://mystorageaccount.blob.core.windows.net/favorite/FavoriteMovies.ISO</span></span>|  
|<span data-ttu-id="ad46b-125">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="ad46b-125">\\\bigshare\john\music</span></span>|<span data-ttu-id="ad46b-126">https://mystorageaccount.BLOB.Core.Windows.NET/Music</span><span class="sxs-lookup"><span data-stu-id="ad46b-126">https://mystorageaccount.blob.core.windows.net/music</span></span>|  
  
<span data-ttu-id="ad46b-127">A hozzárendelést, a fájl a `H:\Video\Drama\GreatMovie.mov` importálhatja, hogy a blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span><span class="sxs-lookup"><span data-stu-id="ad46b-127">With this mapping, the file `H:\Video\Drama\GreatMovie.mov` will be imported to the blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span></span>  
  
<span data-ttu-id="ad46b-128">Ezt követően annak megállapításához, hogy hány merevlemezek szükségesek, számítási az adatok mérete:</span><span class="sxs-lookup"><span data-stu-id="ad46b-128">Next, to determine how many hard drives are needed, compute the size of the data:</span></span>  
  
`5TB + 30GB + 25GB + 10GB = 5TB + 65GB`  
  
<span data-ttu-id="ad46b-129">Az ebben a példában két 3 TB-os merevlemezeket elegendőnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ad46b-129">For this example, two 3TB hard drives should be sufficient.</span></span> <span data-ttu-id="ad46b-130">Mivel azonban a forráskönyvtár `H:\Video` 5TB adatot, és az egyetlen merevlemez-területtel csak 3TB szükséges hibájához `H:\Video` be a Microsoft Azure Import/Export eszköz futtatása előtt két kisebb könyvtárak: `H:\Video1` és `H:\Video2`.</span><span class="sxs-lookup"><span data-stu-id="ad46b-130">However, since the source directory `H:\Video` has 5TB of data and your single hard drive's capacity is only 3TB, it's necessary to break `H:\Video` into two smaller directories before running the Microsoft Azure Import/Export Tool: `H:\Video1` and `H:\Video2`.</span></span> <span data-ttu-id="ad46b-131">Ebben a lépésben adja eredményül a következő forrás-könyvtárak:</span><span class="sxs-lookup"><span data-stu-id="ad46b-131">This step yields the following source directories:</span></span>  
  
|<span data-ttu-id="ad46b-132">Hely</span><span class="sxs-lookup"><span data-stu-id="ad46b-132">Location</span></span>|<span data-ttu-id="ad46b-133">Méret</span><span class="sxs-lookup"><span data-stu-id="ad46b-133">Size</span></span>|<span data-ttu-id="ad46b-134">Cél virtuális könyvtárat vagy blob</span><span class="sxs-lookup"><span data-stu-id="ad46b-134">Destination virtual directory or blob</span></span>|  
|--------------|----------|-------------------------------------------|  
|<span data-ttu-id="ad46b-135">H:\Video1</span><span class="sxs-lookup"><span data-stu-id="ad46b-135">H:\Video1</span></span>|<span data-ttu-id="ad46b-136">2.5 TB</span><span class="sxs-lookup"><span data-stu-id="ad46b-136">2.5TB</span></span>|<span data-ttu-id="ad46b-137">https://mystorageaccount.BLOB.Core.Windows.NET/video</span><span class="sxs-lookup"><span data-stu-id="ad46b-137">https://mystorageaccount.blob.core.windows.net/video</span></span>|  
|<span data-ttu-id="ad46b-138">H:\Video2</span><span class="sxs-lookup"><span data-stu-id="ad46b-138">H:\Video2</span></span>|<span data-ttu-id="ad46b-139">2.5 TB</span><span class="sxs-lookup"><span data-stu-id="ad46b-139">2.5TB</span></span>|<span data-ttu-id="ad46b-140">https://mystorageaccount.BLOB.Core.Windows.NET/video</span><span class="sxs-lookup"><span data-stu-id="ad46b-140">https://mystorageaccount.blob.core.windows.net/video</span></span>|  
|<span data-ttu-id="ad46b-141">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="ad46b-141">H:\Photo</span></span>|<span data-ttu-id="ad46b-142">30 GB</span><span class="sxs-lookup"><span data-stu-id="ad46b-142">30GB</span></span>|<span data-ttu-id="ad46b-143">https://mystorageaccount.BLOB.Core.Windows.NET/Photo</span><span class="sxs-lookup"><span data-stu-id="ad46b-143">https://mystorageaccount.blob.core.windows.net/photo</span></span>|  
|<span data-ttu-id="ad46b-144">K:\Temp\FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="ad46b-144">K:\Temp\FavoriteMovies.ISO</span></span>|<span data-ttu-id="ad46b-145">25 GB</span><span class="sxs-lookup"><span data-stu-id="ad46b-145">25GB</span></span>|<span data-ttu-id="ad46b-146">https://mystorageaccount.BLOB.Core.Windows.NET/Favorite/FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="ad46b-146">https://mystorageaccount.blob.core.windows.net/favorite/FavoriteMovies.ISO</span></span>|  
|<span data-ttu-id="ad46b-147">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="ad46b-147">\\\bigshare\john\music</span></span>|<span data-ttu-id="ad46b-148">10GB</span><span class="sxs-lookup"><span data-stu-id="ad46b-148">10GB</span></span>|<span data-ttu-id="ad46b-149">https://mystorageaccount.BLOB.Core.Windows.NET/Music</span><span class="sxs-lookup"><span data-stu-id="ad46b-149">https://mystorageaccount.blob.core.windows.net/music</span></span>|  
  
 <span data-ttu-id="ad46b-150">Vegye figyelembe, hogy akkor is, ha a `H:\Video`directory felosztott két könyvtárak a cél virtuális könyvtárba a tárfiókban lévő pontok.</span><span class="sxs-lookup"><span data-stu-id="ad46b-150">Note that even though the `H:\Video`directory has been split to two directories, they point to the same destination virtual directory in the storage account.</span></span> <span data-ttu-id="ad46b-151">Így minden videofájlok megmaradnak az egyetlen `video` a tárfiókban lévő tároló.</span><span class="sxs-lookup"><span data-stu-id="ad46b-151">This way, all video files are maintained under a single `video` container in the storage account.</span></span>  
  
 <span data-ttu-id="ad46b-152">Ezután a fenti forrás könyvtárak egyenletesen oszlanak a két merevlemez-meghajtóval:</span><span class="sxs-lookup"><span data-stu-id="ad46b-152">Next, the above source directories are evenly distributed to the two hard drives:</span></span>  
  
||||  
|-|-|-|  
|<span data-ttu-id="ad46b-153">Merevlemez-meghajtó</span><span class="sxs-lookup"><span data-stu-id="ad46b-153">Hard drive</span></span>|<span data-ttu-id="ad46b-154">Forrás könyvtárak</span><span class="sxs-lookup"><span data-stu-id="ad46b-154">Source directories</span></span>|<span data-ttu-id="ad46b-155">Teljes méret</span><span class="sxs-lookup"><span data-stu-id="ad46b-155">Total size</span></span>|  
|<span data-ttu-id="ad46b-156">Első meghajtó</span><span class="sxs-lookup"><span data-stu-id="ad46b-156">First Drive</span></span>|<span data-ttu-id="ad46b-157">H:\Video1</span><span class="sxs-lookup"><span data-stu-id="ad46b-157">H:\Video1</span></span>|<span data-ttu-id="ad46b-158">2.5 TB + 30 GB</span><span class="sxs-lookup"><span data-stu-id="ad46b-158">2.5TB + 30GB</span></span>|  
||<span data-ttu-id="ad46b-159">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="ad46b-159">H:\Photo</span></span>||  
|<span data-ttu-id="ad46b-160">Második meghajtó</span><span class="sxs-lookup"><span data-stu-id="ad46b-160">Second Drive</span></span>|<span data-ttu-id="ad46b-161">H:\Video2</span><span class="sxs-lookup"><span data-stu-id="ad46b-161">H:\Video2</span></span>|<span data-ttu-id="ad46b-162">2.5 TB + 35 GB</span><span class="sxs-lookup"><span data-stu-id="ad46b-162">2.5TB + 35GB</span></span>|  
||<span data-ttu-id="ad46b-163">K:\Temp\BlueRay.ISO</span><span class="sxs-lookup"><span data-stu-id="ad46b-163">K:\Temp\BlueRay.ISO</span></span>||  
||<span data-ttu-id="ad46b-164">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="ad46b-164">\\\bigshare\john\music</span></span>||  
  
<span data-ttu-id="ad46b-165">Ezenkívül a következő metaadatokat az összes fájl állíthatja be:</span><span class="sxs-lookup"><span data-stu-id="ad46b-165">In addition, you can set the following metadata for all files:</span></span>  
  
-   <span data-ttu-id="ad46b-166">**UploadMethod:** Windows Azure Import/Export szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="ad46b-166">**UploadMethod:** Windows Azure Import/Export service</span></span>  
  
-   <span data-ttu-id="ad46b-167">**DataSetName:** SampleData</span><span class="sxs-lookup"><span data-stu-id="ad46b-167">**DataSetName:** SampleData</span></span>  
  
-   <span data-ttu-id="ad46b-168">**CreationDate:** 10/1/2013</span><span class="sxs-lookup"><span data-stu-id="ad46b-168">**CreationDate:** 10/1/2013</span></span>  
  
<span data-ttu-id="ad46b-169">Ha szeretné beállítani az importált fájlok metaadatait, hozzon létre egy szövegfájlt, `c:\WAImportExport\SampleMetadata.txt`, a következő tartalommal:</span><span class="sxs-lookup"><span data-stu-id="ad46b-169">To set metadata for the imported files, create a text file, `c:\WAImportExport\SampleMetadata.txt`, with the following content:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>  
    <DataSetName>SampleData</DataSetName>  
    <CreationDate>10/1/2013</CreationDate>  
</Metadata>  
```
  
<span data-ttu-id="ad46b-170">Bizonyos tulajdonságait is beállíthat a `FavoriteMovie.ISO` blob:</span><span class="sxs-lookup"><span data-stu-id="ad46b-170">You can also set some properties for the `FavoriteMovie.ISO` blob:</span></span>  
  
-   <span data-ttu-id="ad46b-171">**Content-Type:** application/octet-stream</span><span class="sxs-lookup"><span data-stu-id="ad46b-171">**Content-Type:** application/octet-stream</span></span>  
  
-   <span data-ttu-id="ad46b-172">**Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==</span><span class="sxs-lookup"><span data-stu-id="ad46b-172">**Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ==</span></span>  
  
-   <span data-ttu-id="ad46b-173">**A Cache-Control:** no-cache</span><span class="sxs-lookup"><span data-stu-id="ad46b-173">**Cache-Control:** no-cache</span></span>  
  
<span data-ttu-id="ad46b-174">A tulajdonságok beállításáról, hozzon létre egy szövegfájlt `c:\WAImportExport\SampleProperties.txt`:</span><span class="sxs-lookup"><span data-stu-id="ad46b-174">To set these properties, create a text file, `c:\WAImportExport\SampleProperties.txt`:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
<span data-ttu-id="ad46b-175">Most már készen áll a két merevlemez-meghajtók előkészítése az Azure Import/Export eszköz futtatásához.</span><span class="sxs-lookup"><span data-stu-id="ad46b-175">Now you are ready to run the Azure Import/Export Tool to prepare the two hard drives.</span></span> <span data-ttu-id="ad46b-176">Vegye figyelembe:</span><span class="sxs-lookup"><span data-stu-id="ad46b-176">Note that:</span></span>  
  
-   <span data-ttu-id="ad46b-177">Az első meghajtó X meghajtóként lett-e csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="ad46b-177">The first drive is mounted as drive X.</span></span>  
  
-   <span data-ttu-id="ad46b-178">A második meghajtó Y meghajtóként lett-e csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="ad46b-178">The second drive is mounted as drive Y.</span></span>  
  
-   <span data-ttu-id="ad46b-179">A tárfiók kulcsa `mystorageaccount` van `8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg==`.</span><span class="sxs-lookup"><span data-stu-id="ad46b-179">The key for the storage account `mystorageaccount` is `8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg==`.</span></span>  

## <a name="preparing-disk-for-import-when-data-is-pre-loaded"></a><span data-ttu-id="ad46b-180">Lemez előkészítése importálása, amikor az adatok előzetes betöltése</span><span class="sxs-lookup"><span data-stu-id="ad46b-180">Preparing disk for import when data is pre-loaded</span></span>
 
 <span data-ttu-id="ad46b-181">Ha az adatokat, importálandók már megtalálható a lemezen, használja a jelző /skipwrite.</span><span class="sxs-lookup"><span data-stu-id="ad46b-181">If the data to be imported is already present on the disk, use the flag /skipwrite.</span></span> <span data-ttu-id="ad46b-182">Érték /t és /srcdir, mind a lemez importálása előkészítés alatt kell mutatnia.</span><span class="sxs-lookup"><span data-stu-id="ad46b-182">Value of /t and /srcdir both should point to the disk being prepared for import.</span></span> <span data-ttu-id="ad46b-183">Ha nem minden, az azonos virtuális célkönyvtáron vagy a storage-fiók gyökérkönyvtárában kell a lemezen lévő adatok ugyanaz a parancs minden elkülönítve tartja a /id értékének könyvtár közötti összes futtatása ugyanazon.</span><span class="sxs-lookup"><span data-stu-id="ad46b-183">If not all the data on the disk needs to go to the same destination virtual directory or root of the storage account, run the same command for each directory separately keeping the value of /id same across all runs.</span></span>

>[!NOTE] 
><span data-ttu-id="ad46b-184">Ne adjon meg Format, azt fogja törölni az adatokat a lemezen.</span><span class="sxs-lookup"><span data-stu-id="ad46b-184">Do not specify /format as it will wipe the data on the disk.</span></span> <span data-ttu-id="ad46b-185">Adja meg, / titkosítása vagy /bk attól függően, hogy a lemez már titkosítva van-e.</span><span class="sxs-lookup"><span data-stu-id="ad46b-185">You can specify /encrypt or /bk depending on whether the disk is already encrypted or not.</span></span> 
>

```
    When data is already present on the disk for each drive run the following command.
    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:x:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt /skipwrite
```

## <a name="copy-sessions---first-drive"></a><span data-ttu-id="ad46b-186">Másolja a munkamenet - először meghajtó</span><span class="sxs-lookup"><span data-stu-id="ad46b-186">Copy sessions - first drive</span></span>

<span data-ttu-id="ad46b-187">Az első meghajtó futtassa az Azure Import/Export eszköz kétszer a két forrás könyvtárak másolása:</span><span class="sxs-lookup"><span data-stu-id="ad46b-187">For the first drive, run the Azure Import/Export Tool twice to copy the two source directories:</span></span>  

<span data-ttu-id="ad46b-188">**Először másolja az munkamenet**</span><span class="sxs-lookup"><span data-stu-id="ad46b-188">**First copy session**</span></span>
  
```
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```

<span data-ttu-id="ad46b-189">**Második példány munkamenet**</span><span class="sxs-lookup"><span data-stu-id="ad46b-189">**Second copy session**</span></span>

```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt
```

## <a name="copy-sessions---second-drive"></a><span data-ttu-id="ad46b-190">Másolja a munkamenet - meghajtó másodpercenként</span><span class="sxs-lookup"><span data-stu-id="ad46b-190">Copy sessions - second drive</span></span>
 
<span data-ttu-id="ad46b-191">A második meghajtó, futtassa az Azure Import/Export eszköz háromszorosa, egyszer minden egyes a forrás-könyvtárakhoz, és egyszer az önálló Blu-Ray™ lemezképfájl):</span><span class="sxs-lookup"><span data-stu-id="ad46b-191">For the second drive, run the Azure Import/Export Tool three times, once each for the source directories, and once for the standalone Blu-Ray™ image file):</span></span>  
  
<span data-ttu-id="ad46b-192">**Először másolja az munkamenet**</span><span class="sxs-lookup"><span data-stu-id="ad46b-192">**First copy session**</span></span> 

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Video2 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:y /format /encrypt /srcdir:H:\Video2 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```
  
<span data-ttu-id="ad46b-193">**Második példány munkamenet**</span><span class="sxs-lookup"><span data-stu-id="ad46b-193">**Second copy session**</span></span>

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Music /srcdir:\\bigshare\john\music /dstdir:music/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```  
  
<span data-ttu-id="ad46b-194">**Harmadik másolatot munkamenet**</span><span class="sxs-lookup"><span data-stu-id="ad46b-194">**Third copy session**</span></span>  

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```

## <a name="copy-session-completion"></a><span data-ttu-id="ad46b-195">Másolja a munkamenet befejezése</span><span class="sxs-lookup"><span data-stu-id="ad46b-195">Copy session completion</span></span>

<span data-ttu-id="ad46b-196">Miután végzett a másolat munkamenetek, a két meghajtók leválasztása a másolási számítógépről, és küldje el azokat a megfelelő Windows Azure adatközpontba.</span><span class="sxs-lookup"><span data-stu-id="ad46b-196">Once the copy sessions have completed, you can disconnect the two drives from the copy computer and ship them to the appropriate Windows Azure data center.</span></span> <span data-ttu-id="ad46b-197">A két Adatbázisnapló-fájlok feltöltése lesz `FirstDrive.jrn` és `SecondDrive.jrn`, az importálási feladat létrehozásakor a [Windows Azure felügyeleti portálon](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="ad46b-197">You'll upload the two journal files, `FirstDrive.jrn` and `SecondDrive.jrn`, when you create the import job in the [Windows Azure Management Portal](https://manage.windowsazure.com/).</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="ad46b-198">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ad46b-198">Next steps</span></span>

* [<span data-ttu-id="ad46b-199">Merevlemezek előkészítése importálási feladatokhoz</span><span class="sxs-lookup"><span data-stu-id="ad46b-199">Preparing hard drives for an import job</span></span>](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [<span data-ttu-id="ad46b-200">A gyakran használt parancsok rövid összefoglalása</span><span class="sxs-lookup"><span data-stu-id="ad46b-200">Quick reference for frequently used commands</span></span>](storage-import-export-tool-quick-reference-v1.md) 
