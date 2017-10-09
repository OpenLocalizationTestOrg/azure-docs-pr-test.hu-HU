---
title: "aaaSample munkafolyamat tooprep merevlemez-meghajtók az Azure Import/Export importálási feladat - 1-es verzió |} Microsoft Docs"
description: "Tekintse meg a teljes folyamat hello meghajtók előkészítése az importálási feladat hello Azure Import/Export szolgáltatás egy bemutató."
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
ms.openlocfilehash: eb77831a88c16c14838179a6432ddb06503067dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sample-workflow-tooprepare-hard-drives-for-an-import-job"></a><span data-ttu-id="a0cf8-103">Minta munkafolyamat tooprepare merevlemezeit, az importálási feladat</span><span class="sxs-lookup"><span data-stu-id="a0cf8-103">Sample workflow tooprepare hard drives for an import job</span></span>
<span data-ttu-id="a0cf8-104">Ez a témakör bemutatja, hogyan hello teljes folyamata meghajtók előkészítése az importálási feladat.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-104">This topic walks you through hello complete process of preparing drives for an import job.</span></span>  
  
<span data-ttu-id="a0cf8-105">Ebben a példában importálja a következő adatok azokat a Windows Azure storage-fiók nevű hello `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="a0cf8-105">This example imports hello following data into a Window Azure storage account named `mystorageaccount`:</span></span>  
  
|<span data-ttu-id="a0cf8-106">Hely</span><span class="sxs-lookup"><span data-stu-id="a0cf8-106">Location</span></span>|<span data-ttu-id="a0cf8-107">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0cf8-107">Description</span></span>|  
|--------------|-----------------|  
|<span data-ttu-id="a0cf8-108">H:\Video</span><span class="sxs-lookup"><span data-stu-id="a0cf8-108">H:\Video</span></span>|<span data-ttu-id="a0cf8-109">Gyűjteménye videók, összesen 5 TB.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-109">A collection of videos, 5 TB in total.</span></span>|  
|<span data-ttu-id="a0cf8-110">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="a0cf8-110">H:\Photo</span></span>|<span data-ttu-id="a0cf8-111">Egy gyűjtemény fényképek, összesen 30 GB.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-111">A collection of photos, 30 GB in total.</span></span>|  
|<span data-ttu-id="a0cf8-112">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="a0cf8-112">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="a0cf8-113">A Blu-ray™ lemezképét, 25 GB.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-113">A Blu-Ray™ disk image, 25 GB.</span></span>|  
|<span data-ttu-id="a0cf8-114">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="a0cf8-114">\\\bigshare\john\music</span></span>|<span data-ttu-id="a0cf8-115">Letilthatja a zenei fájlok egy hálózati megosztáson, 10 GB-os teljes gyűjteményét.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-115">A collection of music files on a network share, 10 GB in total.</span></span>|  
  
<span data-ttu-id="a0cf8-116">hello importálási feladat ezek az adatok importálása a következő célhoz hello tárfiók hello:</span><span class="sxs-lookup"><span data-stu-id="a0cf8-116">hello import job imports this data into hello following destinations in hello storage account:</span></span>  
  
|<span data-ttu-id="a0cf8-117">Forrás</span><span class="sxs-lookup"><span data-stu-id="a0cf8-117">Source</span></span>|<span data-ttu-id="a0cf8-118">Cél virtuális könyvtárat vagy blob</span><span class="sxs-lookup"><span data-stu-id="a0cf8-118">Destination virtual directory or blob</span></span>|  
|------------|-------------------------------------------|  
|<span data-ttu-id="a0cf8-119">H:\Video</span><span class="sxs-lookup"><span data-stu-id="a0cf8-119">H:\Video</span></span>|<span data-ttu-id="a0cf8-120">https://mystorageaccount.BLOB.Core.Windows.NET/video</span><span class="sxs-lookup"><span data-stu-id="a0cf8-120">https://mystorageaccount.blob.core.windows.net/video</span></span>|  
|<span data-ttu-id="a0cf8-121">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="a0cf8-121">H:\Photo</span></span>|<span data-ttu-id="a0cf8-122">https://mystorageaccount.BLOB.Core.Windows.NET/Photo</span><span class="sxs-lookup"><span data-stu-id="a0cf8-122">https://mystorageaccount.blob.core.windows.net/photo</span></span>|  
|<span data-ttu-id="a0cf8-123">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="a0cf8-123">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="a0cf8-124">https://mystorageaccount.BLOB.Core.Windows.NET/Favorite/FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="a0cf8-124">https://mystorageaccount.blob.core.windows.net/favorite/FavoriteMovies.ISO</span></span>|  
|<span data-ttu-id="a0cf8-125">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="a0cf8-125">\\\bigshare\john\music</span></span>|<span data-ttu-id="a0cf8-126">https://mystorageaccount.BLOB.Core.Windows.NET/Music</span><span class="sxs-lookup"><span data-stu-id="a0cf8-126">https://mystorageaccount.blob.core.windows.net/music</span></span>|  
  
<span data-ttu-id="a0cf8-127">Ez a leképezés a hello fájl `H:\Video\Drama\GreatMovie.mov` importált toohello BLOB `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-127">With this mapping, hello file `H:\Video\Drama\GreatMovie.mov` is imported toohello blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span></span>  
  
<span data-ttu-id="a0cf8-128">Ezt követően toodetermine hány merevlemezek szükségesek, számítási hello hello adatok mérete:</span><span class="sxs-lookup"><span data-stu-id="a0cf8-128">Next, toodetermine how many hard drives are needed, compute hello size of hello data:</span></span>  
  
`5TB + 30GB + 25GB + 10GB = 5TB + 65GB`  
  
<span data-ttu-id="a0cf8-129">Az ebben a példában két 3 TB merevlemezek elegendőnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-129">For this example, two 3-TB hard drives should be sufficient.</span></span> <span data-ttu-id="a0cf8-130">Mivel hello forráskönyvtár azonban `H:\Video` 5 TB adatot, és az egyetlen merevlemez-területtel csak 3 TB, szükséges toobreak `H:\Video` be két kisebb könyvtárak: `H:\Video1` és `H:\Video2`, mielőtt fut a Microsoft hello Az Azure Import/Export eszköz.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-130">However, since hello source directory `H:\Video` has 5 TB of data and your single hard drive's capacity is only 3 TB, it's necessary toobreak `H:\Video` into two smaller directories: `H:\Video1` and `H:\Video2`, before running hello Microsoft Azure Import/Export Tool.</span></span> <span data-ttu-id="a0cf8-131">Ebben a lépésben adja eredményül a következő forrás könyvtárak hello:</span><span class="sxs-lookup"><span data-stu-id="a0cf8-131">This step yields hello following source directories:</span></span>  
  
|<span data-ttu-id="a0cf8-132">Hely</span><span class="sxs-lookup"><span data-stu-id="a0cf8-132">Location</span></span>|<span data-ttu-id="a0cf8-133">Méret</span><span class="sxs-lookup"><span data-stu-id="a0cf8-133">Size</span></span>|<span data-ttu-id="a0cf8-134">Cél virtuális könyvtárat vagy blob</span><span class="sxs-lookup"><span data-stu-id="a0cf8-134">Destination virtual directory or blob</span></span>|  
|--------------|----------|-------------------------------------------|  
|<span data-ttu-id="a0cf8-135">H:\Video1</span><span class="sxs-lookup"><span data-stu-id="a0cf8-135">H:\Video1</span></span>|<span data-ttu-id="a0cf8-136">2,5 TB</span><span class="sxs-lookup"><span data-stu-id="a0cf8-136">2.5 TB</span></span>|<span data-ttu-id="a0cf8-137">https://mystorageaccount.BLOB.Core.Windows.NET/video</span><span class="sxs-lookup"><span data-stu-id="a0cf8-137">https://mystorageaccount.blob.core.windows.net/video</span></span>|  
|<span data-ttu-id="a0cf8-138">H:\Video2</span><span class="sxs-lookup"><span data-stu-id="a0cf8-138">H:\Video2</span></span>|<span data-ttu-id="a0cf8-139">2,5 TB</span><span class="sxs-lookup"><span data-stu-id="a0cf8-139">2.5 TB</span></span>|<span data-ttu-id="a0cf8-140">https://mystorageaccount.BLOB.Core.Windows.NET/video</span><span class="sxs-lookup"><span data-stu-id="a0cf8-140">https://mystorageaccount.blob.core.windows.net/video</span></span>|  
|<span data-ttu-id="a0cf8-141">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="a0cf8-141">H:\Photo</span></span>|<span data-ttu-id="a0cf8-142">30 GB</span><span class="sxs-lookup"><span data-stu-id="a0cf8-142">30 GB</span></span>|<span data-ttu-id="a0cf8-143">https://mystorageaccount.BLOB.Core.Windows.NET/Photo</span><span class="sxs-lookup"><span data-stu-id="a0cf8-143">https://mystorageaccount.blob.core.windows.net/photo</span></span>|  
|<span data-ttu-id="a0cf8-144">K:\Temp\FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="a0cf8-144">K:\Temp\FavoriteMovies.ISO</span></span>|<span data-ttu-id="a0cf8-145">25 GB</span><span class="sxs-lookup"><span data-stu-id="a0cf8-145">25 GB</span></span>|<span data-ttu-id="a0cf8-146">https://mystorageaccount.BLOB.Core.Windows.NET/Favorite/FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="a0cf8-146">https://mystorageaccount.blob.core.windows.net/favorite/FavoriteMovies.ISO</span></span>|  
|<span data-ttu-id="a0cf8-147">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="a0cf8-147">\\\bigshare\john\music</span></span>|<span data-ttu-id="a0cf8-148">10 GB</span><span class="sxs-lookup"><span data-stu-id="a0cf8-148">10 GB</span></span>|<span data-ttu-id="a0cf8-149">https://mystorageaccount.BLOB.Core.Windows.NET/Music</span><span class="sxs-lookup"><span data-stu-id="a0cf8-149">https://mystorageaccount.blob.core.windows.net/music</span></span>|  
  
 <span data-ttu-id="a0cf8-150">Akkor is, ha hello `H:\Video`directory felosztott tootwo könyvtárak, toohello pontok azonos cél virtuális könyvtár hello tárfiókban.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-150">Even though hello `H:\Video`directory has been split tootwo directories, they point toohello same destination virtual directory in hello storage account.</span></span> <span data-ttu-id="a0cf8-151">Így minden videofájlok megmaradnak az egyetlen `video` hello tárfiókban lévő tároló.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-151">This way, all video files are maintained under a single `video` container in hello storage account.</span></span>  
  
 <span data-ttu-id="a0cf8-152">A következő hello előző forrás címtárai egyenletesen toohello két merevlemez-meghajtóval:</span><span class="sxs-lookup"><span data-stu-id="a0cf8-152">Next, hello previous source directories are evenly distributed toohello two hard drives:</span></span>  
  
||||  
|-|-|-|  
|<span data-ttu-id="a0cf8-153">Merevlemez-meghajtó</span><span class="sxs-lookup"><span data-stu-id="a0cf8-153">Hard drive</span></span>|<span data-ttu-id="a0cf8-154">Forrás könyvtárak</span><span class="sxs-lookup"><span data-stu-id="a0cf8-154">Source directories</span></span>|<span data-ttu-id="a0cf8-155">Teljes méret</span><span class="sxs-lookup"><span data-stu-id="a0cf8-155">Total size</span></span>|  
|<span data-ttu-id="a0cf8-156">Első meghajtó</span><span class="sxs-lookup"><span data-stu-id="a0cf8-156">First Drive</span></span>|<span data-ttu-id="a0cf8-157">H:\Video1</span><span class="sxs-lookup"><span data-stu-id="a0cf8-157">H:\Video1</span></span>|<span data-ttu-id="a0cf8-158">2.5 TB + 30 GB</span><span class="sxs-lookup"><span data-stu-id="a0cf8-158">2.5 TB + 30 GB</span></span>|  
||<span data-ttu-id="a0cf8-159">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="a0cf8-159">H:\Photo</span></span>||  
|<span data-ttu-id="a0cf8-160">Második meghajtó</span><span class="sxs-lookup"><span data-stu-id="a0cf8-160">Second Drive</span></span>|<span data-ttu-id="a0cf8-161">H:\Video2</span><span class="sxs-lookup"><span data-stu-id="a0cf8-161">H:\Video2</span></span>|<span data-ttu-id="a0cf8-162">2.5 TB + 35 GB</span><span class="sxs-lookup"><span data-stu-id="a0cf8-162">2.5 TB + 35 GB</span></span>|  
||<span data-ttu-id="a0cf8-163">K:\Temp\BlueRay.ISO</span><span class="sxs-lookup"><span data-stu-id="a0cf8-163">K:\Temp\BlueRay.ISO</span></span>||  
||<span data-ttu-id="a0cf8-164">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="a0cf8-164">\\\bigshare\john\music</span></span>||  
  
<span data-ttu-id="a0cf8-165">Ezenkívül a következő összes fájlok metaadatait hello állíthatja be:</span><span class="sxs-lookup"><span data-stu-id="a0cf8-165">In addition, you can set hello following metadata for all files:</span></span>  
  
-   <span data-ttu-id="a0cf8-166">**UploadMethod:** Windows Azure Import/Export szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="a0cf8-166">**UploadMethod:** Windows Azure Import/Export service</span></span>  
  
-   <span data-ttu-id="a0cf8-167">**DataSetName:** SampleData</span><span class="sxs-lookup"><span data-stu-id="a0cf8-167">**DataSetName:** SampleData</span></span>  
  
-   <span data-ttu-id="a0cf8-168">**CreationDate:** 10/1/2013</span><span class="sxs-lookup"><span data-stu-id="a0cf8-168">**CreationDate:** 10/1/2013</span></span>  
  
<span data-ttu-id="a0cf8-169">importált hello fájlok metaadatait tooset hozzon létre egy szövegfájlt, `c:\WAImportExport\SampleMetadata.txt`, a tartalom a következő hello:</span><span class="sxs-lookup"><span data-stu-id="a0cf8-169">tooset metadata for hello imported files, create a text file, `c:\WAImportExport\SampleMetadata.txt`, with hello following content:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>  
    <DataSetName>SampleData</DataSetName>  
    <CreationDate>10/1/2013</CreationDate>  
</Metadata>  
```
  
<span data-ttu-id="a0cf8-170">Hello bizonyos tulajdonságait is beállíthat `FavoriteMovie.ISO` blob:</span><span class="sxs-lookup"><span data-stu-id="a0cf8-170">You can also set some properties for hello `FavoriteMovie.ISO` blob:</span></span>  
  
-   <span data-ttu-id="a0cf8-171">**Content-Type:** application/octet-stream</span><span class="sxs-lookup"><span data-stu-id="a0cf8-171">**Content-Type:** application/octet-stream</span></span>  
  
-   <span data-ttu-id="a0cf8-172">**Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==</span><span class="sxs-lookup"><span data-stu-id="a0cf8-172">**Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ==</span></span>  
  
-   <span data-ttu-id="a0cf8-173">**A Cache-Control:** no-cache</span><span class="sxs-lookup"><span data-stu-id="a0cf8-173">**Cache-Control:** no-cache</span></span>  
  
<span data-ttu-id="a0cf8-174">tooset ezeket a tulajdonságokat, hozzon létre egy szövegfájlt `c:\WAImportExport\SampleProperties.txt`:</span><span class="sxs-lookup"><span data-stu-id="a0cf8-174">tooset these properties, create a text file, `c:\WAImportExport\SampleProperties.txt`:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
<span data-ttu-id="a0cf8-175">Most már készen áll a toorun hello Azure Import/Export eszköz tooprepare hello két merevlemez-meghajtók áll.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-175">Now you are ready toorun hello Azure Import/Export Tool tooprepare hello two hard drives.</span></span> <span data-ttu-id="a0cf8-176">Vegye figyelembe:</span><span class="sxs-lookup"><span data-stu-id="a0cf8-176">Note that:</span></span>  
  
-   <span data-ttu-id="a0cf8-177">az első meghajtó hello X meghajtóként lett-e csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-177">hello first drive is mounted as drive X.</span></span>  
  
-   <span data-ttu-id="a0cf8-178">hello második meghajtó Y meghajtóként lett-e csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-178">hello second drive is mounted as drive Y.</span></span>  
  
-   <span data-ttu-id="a0cf8-179">hello hello kulcsának `mystorageaccount` van `8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg==`.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-179">hello key for hello storage account `mystorageaccount` is `8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg==`.</span></span>  

## <a name="preparing-disk-for-import-when-data-is-pre-loaded"></a><span data-ttu-id="a0cf8-180">Lemez előkészítése importálása, amikor az adatok előzetes betöltése</span><span class="sxs-lookup"><span data-stu-id="a0cf8-180">Preparing disk for import when data is pre-loaded</span></span>
 
 <span data-ttu-id="a0cf8-181">Ha hello adatok toobe importált már hello lemezen, akkor használja a hello jelző /skipwrite.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-181">If hello data toobe imported is already present on hello disk, use hello flag /skipwrite.</span></span> <span data-ttu-id="a0cf8-182">/t és /srcdir hello értékének kell mindkét pont toohello lemez való importálás.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-182">hello value of /t and /srcdir should both point toohello disk being prepared for import.</span></span> <span data-ttu-id="a0cf8-183">Ha az összes hello adatok toobe importált nem lesz toohello azonos a cél virtuális könyvtár vagy a főtanúsítvány hello tárfiók, azonos parancsot minden célkönyvtáron külön-külön futtassa hello /id hello értékének tartása hello azonos keresztül minden futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-183">If all of hello data toobe imported is not going toohello same destination virtual directory or root of hello storage account, run hello same command for each destination directory separately, keeping hello value of /id hello same across all runs.</span></span>

>[!NOTE] 
><span data-ttu-id="a0cf8-184">Ne adjon meg Format, azt fogja hello lemezen hello adatainak törlése.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-184">Do not specify /format as it will wipe hello data on hello disk.</span></span> <span data-ttu-id="a0cf8-185">Adja meg, / titkosítása vagy /bk attól függően, hogy hello lemez már titkosítva van-e.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-185">You can specify /encrypt or /bk depending on whether hello disk is already encrypted or not.</span></span> 
>

```
    When data is already present on hello disk for each drive run hello following command.
    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:x:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt /skipwrite
```

## <a name="copy-sessions---first-drive"></a><span data-ttu-id="a0cf8-186">Másolja a munkamenet - először meghajtó</span><span class="sxs-lookup"><span data-stu-id="a0cf8-186">Copy sessions - first drive</span></span>

<span data-ttu-id="a0cf8-187">Hello első meghajtót futtassa hello Azure Import/Export eszköz kétszer két toocopy hello forrás-könyvtárak:</span><span class="sxs-lookup"><span data-stu-id="a0cf8-187">For hello first drive, run hello Azure Import/Export Tool twice toocopy hello two source directories:</span></span>  

<span data-ttu-id="a0cf8-188">**Először másolja az munkamenet**</span><span class="sxs-lookup"><span data-stu-id="a0cf8-188">**First copy session**</span></span>
  
```
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```

<span data-ttu-id="a0cf8-189">**Második példány munkamenet**</span><span class="sxs-lookup"><span data-stu-id="a0cf8-189">**Second copy session**</span></span>

```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt
```

## <a name="copy-sessions---second-drive"></a><span data-ttu-id="a0cf8-190">Másolja a munkamenet - meghajtó másodpercenként</span><span class="sxs-lookup"><span data-stu-id="a0cf8-190">Copy sessions - second drive</span></span>
 
<span data-ttu-id="a0cf8-191">A hello második meghajtó, futtassa hello Azure Import/Export eszköz háromszor, amennyiben az egyes hello a forrás-könyvtárak, és egyszer az hello önálló Blu-Ray™ fájl kép):</span><span class="sxs-lookup"><span data-stu-id="a0cf8-191">For hello second drive, run hello Azure Import/Export Tool three times, once each for hello source directories, and once for hello standalone Blu-Ray™ image file):</span></span>  
  
<span data-ttu-id="a0cf8-192">**Először másolja az munkamenet**</span><span class="sxs-lookup"><span data-stu-id="a0cf8-192">**First copy session**</span></span> 

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Video2 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:y /format /encrypt /srcdir:H:\Video2 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```
  
<span data-ttu-id="a0cf8-193">**Második példány munkamenet**</span><span class="sxs-lookup"><span data-stu-id="a0cf8-193">**Second copy session**</span></span>

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Music /srcdir:\\bigshare\john\music /dstdir:music/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```  
  
<span data-ttu-id="a0cf8-194">**Harmadik másolatot munkamenet**</span><span class="sxs-lookup"><span data-stu-id="a0cf8-194">**Third copy session**</span></span>  

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```

## <a name="copy-session-completion"></a><span data-ttu-id="a0cf8-195">Másolja a munkamenet befejezése</span><span class="sxs-lookup"><span data-stu-id="a0cf8-195">Copy session completion</span></span>

<span data-ttu-id="a0cf8-196">Hello másolási munkamenetek befejezése után hello két meghajtók leválasztása hello másolási számítógépről, és toohello megfelelő Windows Azure-adatközpont küldje el.</span><span class="sxs-lookup"><span data-stu-id="a0cf8-196">Once hello copy sessions have completed, you can disconnect hello two drives from hello copy computer and ship them toohello appropriate Windows Azure data center.</span></span> <span data-ttu-id="a0cf8-197">A két hello Adatbázisnapló-fájlok feltöltése `FirstDrive.jrn` és `SecondDrive.jrn`, hello hello importálási feladat létrehozásakor [Windows Azure portálon](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="a0cf8-197">Upload hello two journal files, `FirstDrive.jrn` and `SecondDrive.jrn`, when you create hello import job in hello [Windows Azure portal](https://manage.windowsazure.com/).</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="a0cf8-198">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a0cf8-198">Next steps</span></span>

* [<span data-ttu-id="a0cf8-199">Merevlemezek előkészítése importálási feladatokhoz</span><span class="sxs-lookup"><span data-stu-id="a0cf8-199">Preparing hard drives for an import job</span></span>](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [<span data-ttu-id="a0cf8-200">A gyakran használt parancsok rövid összefoglalása</span><span class="sxs-lookup"><span data-stu-id="a0cf8-200">Quick reference for frequently used commands</span></span>](../storage-import-export-tool-quick-reference-v1.md) 
