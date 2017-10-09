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
ms.openlocfilehash: f836fc6104d8b4ad5660cb110a62f61b40b0b7ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sample-workflow-tooprepare-hard-drives-for-an-import-job"></a><span data-ttu-id="f74e9-103">Minta munkafolyamat tooprepare merevlemezeit, az importálási feladat</span><span class="sxs-lookup"><span data-stu-id="f74e9-103">Sample workflow tooprepare hard drives for an import job</span></span>
<span data-ttu-id="f74e9-104">Ez a témakör bemutatja, hogyan hello teljes folyamata meghajtók előkészítése az importálási feladat.</span><span class="sxs-lookup"><span data-stu-id="f74e9-104">This topic walks you through hello complete process of preparing drives for an import job.</span></span>  
  
<span data-ttu-id="f74e9-105">Ebben a példában importálja a következő adatok azokat a Windows Azure storage-fiók nevű hello `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="f74e9-105">This example imports hello following data into a Window Azure storage account named `mystorageaccount`:</span></span>  
  
|<span data-ttu-id="f74e9-106">Hely</span><span class="sxs-lookup"><span data-stu-id="f74e9-106">Location</span></span>|<span data-ttu-id="f74e9-107">Leírás</span><span class="sxs-lookup"><span data-stu-id="f74e9-107">Description</span></span>|  
|--------------|-----------------|  
|<span data-ttu-id="f74e9-108">H:\Video</span><span class="sxs-lookup"><span data-stu-id="f74e9-108">H:\Video</span></span>|<span data-ttu-id="f74e9-109">Gyűjteménye videók, összesen 5 TB.</span><span class="sxs-lookup"><span data-stu-id="f74e9-109">A collection of videos, 5 TB in total.</span></span>|  
|<span data-ttu-id="f74e9-110">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="f74e9-110">H:\Photo</span></span>|<span data-ttu-id="f74e9-111">Egy gyűjtemény fényképek, összesen 30 GB.</span><span class="sxs-lookup"><span data-stu-id="f74e9-111">A collection of photos, 30 GB in total.</span></span>|  
|<span data-ttu-id="f74e9-112">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="f74e9-112">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="f74e9-113">A Blu-ray™ lemezképét, 25 GB.</span><span class="sxs-lookup"><span data-stu-id="f74e9-113">A Blu-Ray™ disk image, 25 GB.</span></span>|  
|<span data-ttu-id="f74e9-114">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="f74e9-114">\\\bigshare\john\music</span></span>|<span data-ttu-id="f74e9-115">Letilthatja a zenei fájlok egy hálózati megosztáson, 10 GB-os teljes gyűjteményét.</span><span class="sxs-lookup"><span data-stu-id="f74e9-115">A collection of music files on a network share, 10 GB in total.</span></span>|  
  
<span data-ttu-id="f74e9-116">hello importálási feladat ezek az adatok importálása a következő célhoz hello tárfiók hello:</span><span class="sxs-lookup"><span data-stu-id="f74e9-116">hello import job will import this data into hello following destinations in hello storage account:</span></span>  
  
|<span data-ttu-id="f74e9-117">Forrás</span><span class="sxs-lookup"><span data-stu-id="f74e9-117">Source</span></span>|<span data-ttu-id="f74e9-118">Cél virtuális könyvtárat vagy blob</span><span class="sxs-lookup"><span data-stu-id="f74e9-118">Destination virtual directory or blob</span></span>|  
|------------|-------------------------------------------|  
|<span data-ttu-id="f74e9-119">H:\Video</span><span class="sxs-lookup"><span data-stu-id="f74e9-119">H:\Video</span></span>|<span data-ttu-id="f74e9-120">https://mystorageaccount.BLOB.Core.Windows.NET/video</span><span class="sxs-lookup"><span data-stu-id="f74e9-120">https://mystorageaccount.blob.core.windows.net/video</span></span>|  
|<span data-ttu-id="f74e9-121">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="f74e9-121">H:\Photo</span></span>|<span data-ttu-id="f74e9-122">https://mystorageaccount.BLOB.Core.Windows.NET/Photo</span><span class="sxs-lookup"><span data-stu-id="f74e9-122">https://mystorageaccount.blob.core.windows.net/photo</span></span>|  
|<span data-ttu-id="f74e9-123">K:\Temp\FavoriteMovie.ISO</span><span class="sxs-lookup"><span data-stu-id="f74e9-123">K:\Temp\FavoriteMovie.ISO</span></span>|<span data-ttu-id="f74e9-124">https://mystorageaccount.BLOB.Core.Windows.NET/Favorite/FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="f74e9-124">https://mystorageaccount.blob.core.windows.net/favorite/FavoriteMovies.ISO</span></span>|  
|<span data-ttu-id="f74e9-125">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="f74e9-125">\\\bigshare\john\music</span></span>|<span data-ttu-id="f74e9-126">https://mystorageaccount.BLOB.Core.Windows.NET/Music</span><span class="sxs-lookup"><span data-stu-id="f74e9-126">https://mystorageaccount.blob.core.windows.net/music</span></span>|  
  
<span data-ttu-id="f74e9-127">Ez a leképezés a hello fájl `H:\Video\Drama\GreatMovie.mov` importált toohello blob lesz `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span><span class="sxs-lookup"><span data-stu-id="f74e9-127">With this mapping, hello file `H:\Video\Drama\GreatMovie.mov` will be imported toohello blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov`.</span></span>  
  
<span data-ttu-id="f74e9-128">Ezt követően toodetermine hány merevlemezek szükségesek, számítási hello hello adatok mérete:</span><span class="sxs-lookup"><span data-stu-id="f74e9-128">Next, toodetermine how many hard drives are needed, compute hello size of hello data:</span></span>  
  
`5TB + 30GB + 25GB + 10GB = 5TB + 65GB`  
  
<span data-ttu-id="f74e9-129">Az ebben a példában két 3 TB-os merevlemezeket elegendőnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f74e9-129">For this example, two 3TB hard drives should be sufficient.</span></span> <span data-ttu-id="f74e9-130">Mivel azonban hello forráskönyvtár `H:\Video` 5TB adatot, és az egyetlen merevlemez-területtel csak 3TB, szükséges toobreak `H:\Video` történő futtatása előtt két kisebb könyvtárak hello a Microsoft Azure Import/Export eszköz: `H:\Video1` és `H:\Video2`.</span><span class="sxs-lookup"><span data-stu-id="f74e9-130">However, since hello source directory `H:\Video` has 5TB of data and your single hard drive's capacity is only 3TB, it's necessary toobreak `H:\Video` into two smaller directories before running hello Microsoft Azure Import/Export Tool: `H:\Video1` and `H:\Video2`.</span></span> <span data-ttu-id="f74e9-131">Ebben a lépésben adja eredményül a következő forrás könyvtárak hello:</span><span class="sxs-lookup"><span data-stu-id="f74e9-131">This step yields hello following source directories:</span></span>  
  
|<span data-ttu-id="f74e9-132">Hely</span><span class="sxs-lookup"><span data-stu-id="f74e9-132">Location</span></span>|<span data-ttu-id="f74e9-133">Méret</span><span class="sxs-lookup"><span data-stu-id="f74e9-133">Size</span></span>|<span data-ttu-id="f74e9-134">Cél virtuális könyvtárat vagy blob</span><span class="sxs-lookup"><span data-stu-id="f74e9-134">Destination virtual directory or blob</span></span>|  
|--------------|----------|-------------------------------------------|  
|<span data-ttu-id="f74e9-135">H:\Video1</span><span class="sxs-lookup"><span data-stu-id="f74e9-135">H:\Video1</span></span>|<span data-ttu-id="f74e9-136">2.5 TB</span><span class="sxs-lookup"><span data-stu-id="f74e9-136">2.5TB</span></span>|<span data-ttu-id="f74e9-137">https://mystorageaccount.BLOB.Core.Windows.NET/video</span><span class="sxs-lookup"><span data-stu-id="f74e9-137">https://mystorageaccount.blob.core.windows.net/video</span></span>|  
|<span data-ttu-id="f74e9-138">H:\Video2</span><span class="sxs-lookup"><span data-stu-id="f74e9-138">H:\Video2</span></span>|<span data-ttu-id="f74e9-139">2.5 TB</span><span class="sxs-lookup"><span data-stu-id="f74e9-139">2.5TB</span></span>|<span data-ttu-id="f74e9-140">https://mystorageaccount.BLOB.Core.Windows.NET/video</span><span class="sxs-lookup"><span data-stu-id="f74e9-140">https://mystorageaccount.blob.core.windows.net/video</span></span>|  
|<span data-ttu-id="f74e9-141">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="f74e9-141">H:\Photo</span></span>|<span data-ttu-id="f74e9-142">30 GB</span><span class="sxs-lookup"><span data-stu-id="f74e9-142">30GB</span></span>|<span data-ttu-id="f74e9-143">https://mystorageaccount.BLOB.Core.Windows.NET/Photo</span><span class="sxs-lookup"><span data-stu-id="f74e9-143">https://mystorageaccount.blob.core.windows.net/photo</span></span>|  
|<span data-ttu-id="f74e9-144">K:\Temp\FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="f74e9-144">K:\Temp\FavoriteMovies.ISO</span></span>|<span data-ttu-id="f74e9-145">25 GB</span><span class="sxs-lookup"><span data-stu-id="f74e9-145">25GB</span></span>|<span data-ttu-id="f74e9-146">https://mystorageaccount.BLOB.Core.Windows.NET/Favorite/FavoriteMovies.ISO</span><span class="sxs-lookup"><span data-stu-id="f74e9-146">https://mystorageaccount.blob.core.windows.net/favorite/FavoriteMovies.ISO</span></span>|  
|<span data-ttu-id="f74e9-147">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="f74e9-147">\\\bigshare\john\music</span></span>|<span data-ttu-id="f74e9-148">10GB</span><span class="sxs-lookup"><span data-stu-id="f74e9-148">10GB</span></span>|<span data-ttu-id="f74e9-149">https://mystorageaccount.BLOB.Core.Windows.NET/Music</span><span class="sxs-lookup"><span data-stu-id="f74e9-149">https://mystorageaccount.blob.core.windows.net/music</span></span>|  
  
 <span data-ttu-id="f74e9-150">Vegye figyelembe, hogy akkor is, ha hello `H:\Video`directory felosztott tootwo könyvtárak, toohello pontok azonos cél virtuális könyvtár hello tárfiókban.</span><span class="sxs-lookup"><span data-stu-id="f74e9-150">Note that even though hello `H:\Video`directory has been split tootwo directories, they point toohello same destination virtual directory in hello storage account.</span></span> <span data-ttu-id="f74e9-151">Így minden videofájlok megmaradnak az egyetlen `video` hello tárfiókban lévő tároló.</span><span class="sxs-lookup"><span data-stu-id="f74e9-151">This way, all video files are maintained under a single `video` container in hello storage account.</span></span>  
  
 <span data-ttu-id="f74e9-152">A következő forrás címtárai egyenletesen fent hello elosztott toohello két merevlemez-meghajtóval:</span><span class="sxs-lookup"><span data-stu-id="f74e9-152">Next, hello above source directories are evenly distributed toohello two hard drives:</span></span>  
  
||||  
|-|-|-|  
|<span data-ttu-id="f74e9-153">Merevlemez-meghajtó</span><span class="sxs-lookup"><span data-stu-id="f74e9-153">Hard drive</span></span>|<span data-ttu-id="f74e9-154">Forrás könyvtárak</span><span class="sxs-lookup"><span data-stu-id="f74e9-154">Source directories</span></span>|<span data-ttu-id="f74e9-155">Teljes méret</span><span class="sxs-lookup"><span data-stu-id="f74e9-155">Total size</span></span>|  
|<span data-ttu-id="f74e9-156">Első meghajtó</span><span class="sxs-lookup"><span data-stu-id="f74e9-156">First Drive</span></span>|<span data-ttu-id="f74e9-157">H:\Video1</span><span class="sxs-lookup"><span data-stu-id="f74e9-157">H:\Video1</span></span>|<span data-ttu-id="f74e9-158">2.5 TB + 30 GB</span><span class="sxs-lookup"><span data-stu-id="f74e9-158">2.5TB + 30GB</span></span>|  
||<span data-ttu-id="f74e9-159">H:\Photo</span><span class="sxs-lookup"><span data-stu-id="f74e9-159">H:\Photo</span></span>||  
|<span data-ttu-id="f74e9-160">Második meghajtó</span><span class="sxs-lookup"><span data-stu-id="f74e9-160">Second Drive</span></span>|<span data-ttu-id="f74e9-161">H:\Video2</span><span class="sxs-lookup"><span data-stu-id="f74e9-161">H:\Video2</span></span>|<span data-ttu-id="f74e9-162">2.5 TB + 35 GB</span><span class="sxs-lookup"><span data-stu-id="f74e9-162">2.5TB + 35GB</span></span>|  
||<span data-ttu-id="f74e9-163">K:\Temp\BlueRay.ISO</span><span class="sxs-lookup"><span data-stu-id="f74e9-163">K:\Temp\BlueRay.ISO</span></span>||  
||<span data-ttu-id="f74e9-164">\\\bigshare\john\music</span><span class="sxs-lookup"><span data-stu-id="f74e9-164">\\\bigshare\john\music</span></span>||  
  
<span data-ttu-id="f74e9-165">Ezenkívül a következő összes fájlok metaadatait hello állíthatja be:</span><span class="sxs-lookup"><span data-stu-id="f74e9-165">In addition, you can set hello following metadata for all files:</span></span>  
  
-   <span data-ttu-id="f74e9-166">**UploadMethod:** Windows Azure Import/Export szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="f74e9-166">**UploadMethod:** Windows Azure Import/Export service</span></span>  
  
-   <span data-ttu-id="f74e9-167">**DataSetName:** SampleData</span><span class="sxs-lookup"><span data-stu-id="f74e9-167">**DataSetName:** SampleData</span></span>  
  
-   <span data-ttu-id="f74e9-168">**CreationDate:** 10/1/2013</span><span class="sxs-lookup"><span data-stu-id="f74e9-168">**CreationDate:** 10/1/2013</span></span>  
  
<span data-ttu-id="f74e9-169">importált hello fájlok metaadatait tooset hozzon létre egy szövegfájlt, `c:\WAImportExport\SampleMetadata.txt`, a tartalom a következő hello:</span><span class="sxs-lookup"><span data-stu-id="f74e9-169">tooset metadata for hello imported files, create a text file, `c:\WAImportExport\SampleMetadata.txt`, with hello following content:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
    <UploadMethod>Windows Azure Import/Export service</UploadMethod>  
    <DataSetName>SampleData</DataSetName>  
    <CreationDate>10/1/2013</CreationDate>  
</Metadata>  
```
  
<span data-ttu-id="f74e9-170">Hello bizonyos tulajdonságait is beállíthat `FavoriteMovie.ISO` blob:</span><span class="sxs-lookup"><span data-stu-id="f74e9-170">You can also set some properties for hello `FavoriteMovie.ISO` blob:</span></span>  
  
-   <span data-ttu-id="f74e9-171">**Content-Type:** application/octet-stream</span><span class="sxs-lookup"><span data-stu-id="f74e9-171">**Content-Type:** application/octet-stream</span></span>  
  
-   <span data-ttu-id="f74e9-172">**Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==</span><span class="sxs-lookup"><span data-stu-id="f74e9-172">**Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ==</span></span>  
  
-   <span data-ttu-id="f74e9-173">**A Cache-Control:** no-cache</span><span class="sxs-lookup"><span data-stu-id="f74e9-173">**Cache-Control:** no-cache</span></span>  
  
<span data-ttu-id="f74e9-174">tooset ezeket a tulajdonságokat, hozzon létre egy szövegfájlt `c:\WAImportExport\SampleProperties.txt`:</span><span class="sxs-lookup"><span data-stu-id="f74e9-174">tooset these properties, create a text file, `c:\WAImportExport\SampleProperties.txt`:</span></span>  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
<span data-ttu-id="f74e9-175">Most már készen áll a toorun hello Azure Import/Export eszköz tooprepare hello két merevlemez-meghajtók áll.</span><span class="sxs-lookup"><span data-stu-id="f74e9-175">Now you are ready toorun hello Azure Import/Export Tool tooprepare hello two hard drives.</span></span> <span data-ttu-id="f74e9-176">Vegye figyelembe:</span><span class="sxs-lookup"><span data-stu-id="f74e9-176">Note that:</span></span>  
  
-   <span data-ttu-id="f74e9-177">az első meghajtó hello X meghajtóként lett-e csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="f74e9-177">hello first drive is mounted as drive X.</span></span>  
  
-   <span data-ttu-id="f74e9-178">hello második meghajtó Y meghajtóként lett-e csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="f74e9-178">hello second drive is mounted as drive Y.</span></span>  
  
-   <span data-ttu-id="f74e9-179">hello hello kulcsának `mystorageaccount` van `8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg==`.</span><span class="sxs-lookup"><span data-stu-id="f74e9-179">hello key for hello storage account `mystorageaccount` is `8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg==`.</span></span>  

## <a name="preparing-disk-for-import-when-data-is-pre-loaded"></a><span data-ttu-id="f74e9-180">Lemez előkészítése importálása, amikor az adatok előzetes betöltése</span><span class="sxs-lookup"><span data-stu-id="f74e9-180">Preparing disk for import when data is pre-loaded</span></span>
 
 <span data-ttu-id="f74e9-181">Ha hello adatok toobe importált már hello lemezen, akkor használja a hello jelző /skipwrite.</span><span class="sxs-lookup"><span data-stu-id="f74e9-181">If hello data toobe imported is already present on hello disk, use hello flag /skipwrite.</span></span> <span data-ttu-id="f74e9-182">/T és /srcdir mindkét értékének toohello lemez importálása előkészítés alatt kell mutatnia.</span><span class="sxs-lookup"><span data-stu-id="f74e9-182">Value of /t and /srcdir both should point toohello disk being prepared for import.</span></span> <span data-ttu-id="f74e9-183">Ha az adatok nem mindegyik hello hello lemez kell toogo toohello azonos cél virtuális könyvtár, vagy a főtanúsítvány hello tárfiók, azonos parancsot minden directory külön tartása /id hello értékének futtatási hello azonos keresztül minden futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="f74e9-183">If not all hello data on hello disk needs toogo toohello same destination virtual directory or root of hello storage account, run hello same command for each directory separately keeping hello value of /id same across all runs.</span></span>

>[!NOTE] 
><span data-ttu-id="f74e9-184">Ne adjon meg Format, azt fogja hello lemezen hello adatainak törlése.</span><span class="sxs-lookup"><span data-stu-id="f74e9-184">Do not specify /format as it will wipe hello data on hello disk.</span></span> <span data-ttu-id="f74e9-185">Adja meg, / titkosítása vagy /bk attól függően, hogy hello lemez már titkosítva van-e.</span><span class="sxs-lookup"><span data-stu-id="f74e9-185">You can specify /encrypt or /bk depending on whether hello disk is already encrypted or not.</span></span> 
>

```
    When data is already present on hello disk for each drive run hello following command.
    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:x:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt /skipwrite
```

## <a name="copy-sessions---first-drive"></a><span data-ttu-id="f74e9-186">Másolja a munkamenet - először meghajtó</span><span class="sxs-lookup"><span data-stu-id="f74e9-186">Copy sessions - first drive</span></span>

<span data-ttu-id="f74e9-187">Hello első meghajtót futtassa hello Azure Import/Export eszköz kétszer két toocopy hello forrás-könyvtárak:</span><span class="sxs-lookup"><span data-stu-id="f74e9-187">For hello first drive, run hello Azure Import/Export Tool twice toocopy hello two source directories:</span></span>  

<span data-ttu-id="f74e9-188">**Először másolja az munkamenet**</span><span class="sxs-lookup"><span data-stu-id="f74e9-188">**First copy session**</span></span>
  
```
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```

<span data-ttu-id="f74e9-189">**Második példány munkamenet**</span><span class="sxs-lookup"><span data-stu-id="f74e9-189">**Second copy session**</span></span>

```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt
```

## <a name="copy-sessions---second-drive"></a><span data-ttu-id="f74e9-190">Másolja a munkamenet - meghajtó másodpercenként</span><span class="sxs-lookup"><span data-stu-id="f74e9-190">Copy sessions - second drive</span></span>
 
<span data-ttu-id="f74e9-191">A hello második meghajtó, futtassa hello Azure Import/Export eszköz háromszor, amennyiben az egyes hello a forrás-könyvtárak, és egyszer az hello önálló Blu-Ray™ fájl kép):</span><span class="sxs-lookup"><span data-stu-id="f74e9-191">For hello second drive, run hello Azure Import/Export Tool three times, once each for hello source directories, and once for hello standalone Blu-Ray™ image file):</span></span>  
  
<span data-ttu-id="f74e9-192">**Először másolja az munkamenet**</span><span class="sxs-lookup"><span data-stu-id="f74e9-192">**First copy session**</span></span> 

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Video2 /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:y /format /encrypt /srcdir:H:\Video2 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```
  
<span data-ttu-id="f74e9-193">**Második példány munkamenet**</span><span class="sxs-lookup"><span data-stu-id="f74e9-193">**Second copy session**</span></span>

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:Music /srcdir:\\bigshare\john\music /dstdir:music/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt  
```  
  
<span data-ttu-id="f74e9-194">**Harmadik másolatot munkamenet**</span><span class="sxs-lookup"><span data-stu-id="f74e9-194">**Third copy session**</span></span>  

```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```

## <a name="copy-session-completion"></a><span data-ttu-id="f74e9-195">Másolja a munkamenet befejezése</span><span class="sxs-lookup"><span data-stu-id="f74e9-195">Copy session completion</span></span>

<span data-ttu-id="f74e9-196">Hello másolási munkamenetek befejezése után hello két meghajtók leválasztása hello másolási számítógépről, és toohello megfelelő Windows Azure-adatközpont küldje el.</span><span class="sxs-lookup"><span data-stu-id="f74e9-196">Once hello copy sessions have completed, you can disconnect hello two drives from hello copy computer and ship them toohello appropriate Windows Azure data center.</span></span> <span data-ttu-id="f74e9-197">Hello két Adatbázisnapló-fájlok feltöltése lesz `FirstDrive.jrn` és `SecondDrive.jrn`, hello hello importálási feladat létrehozásakor [Windows Azure felügyeleti portálon](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="f74e9-197">You'll upload hello two journal files, `FirstDrive.jrn` and `SecondDrive.jrn`, when you create hello import job in hello [Windows Azure Management Portal](https://manage.windowsazure.com/).</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="f74e9-198">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f74e9-198">Next steps</span></span>

* [<span data-ttu-id="f74e9-199">Merevlemezek előkészítése importálási feladatokhoz</span><span class="sxs-lookup"><span data-stu-id="f74e9-199">Preparing hard drives for an import job</span></span>](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [<span data-ttu-id="f74e9-200">A gyakran használt parancsok rövid összefoglalása</span><span class="sxs-lookup"><span data-stu-id="f74e9-200">Quick reference for frequently used commands</span></span>](storage-import-export-tool-quick-reference-v1.md) 
