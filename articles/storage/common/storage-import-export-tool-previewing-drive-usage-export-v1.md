---
title: "aaaPreviewing meghajtó használata az Azure Import/Export exportálási feladat - 1-es verzió |} Microsoft Docs"
description: "Ismerje meg, hogyan toopreview hello listája blobok kiválasztott hello Azure Import/Export szolgáltatás exportálási feladat."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 7707d744-7ec7-4de8-ac9b-93a18608dc9a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 7378c159f6d11702cda9ae7654e84d85f9b671b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="previewing-drive-usage-for-an-export-job"></a><span data-ttu-id="220bb-103">Meghajtóhasználat előnézete exportálási feladatokhoz</span><span class="sxs-lookup"><span data-stu-id="220bb-103">Previewing drive usage for an export job</span></span>
<span data-ttu-id="220bb-104">Az exportálási feladatot hoz létre, meg kell toochoose blobok toobe készlete exportált.</span><span class="sxs-lookup"><span data-stu-id="220bb-104">Before you create an export job, you need toochoose a set of blobs toobe exported.</span></span> <span data-ttu-id="220bb-105">hello Microsoft Azure Import/Export szolgáltatás lehetővé teszi a blob elérési utak listájának toouse vagy blob előtagok toorepresent hello blobok választott.</span><span class="sxs-lookup"><span data-stu-id="220bb-105">hello Microsoft Azure Import/Export service allows you toouse a list of blob paths or blob prefixes toorepresent hello blobs you've selected.</span></span>  
  
<span data-ttu-id="220bb-106">A következő lépésben toodetermine hány meghajtók toosend van szüksége.</span><span class="sxs-lookup"><span data-stu-id="220bb-106">Next, you need toodetermine how many drives you need toosend.</span></span> <span data-ttu-id="220bb-107">hello Import/Export eszköz biztosít hello `PreviewExport` hello blobok parancs toopreview meghajtóinak használatát választotta, toouse fog hello meghajtók hello mérete alapján.</span><span class="sxs-lookup"><span data-stu-id="220bb-107">hello Import/Export Tool provides hello `PreviewExport` command toopreview drive usage for hello blobs you selected, based on hello size of hello drives you are going toouse.</span></span>

## <a name="command-line-parameters"></a><span data-ttu-id="220bb-108">Parancssori paraméterek</span><span class="sxs-lookup"><span data-stu-id="220bb-108">Command-line parameters</span></span>

<span data-ttu-id="220bb-109">Használhatja a hello használata esetén a következő paraméterek hello `PreviewExport` az Import/Export eszköz hello parancsot.</span><span class="sxs-lookup"><span data-stu-id="220bb-109">You can use hello following parameters when using hello `PreviewExport` command of hello Import/Export Tool.</span></span>

|<span data-ttu-id="220bb-110">Parancssori paraméter</span><span class="sxs-lookup"><span data-stu-id="220bb-110">Command-line parameter</span></span>|<span data-ttu-id="220bb-111">Leírás</span><span class="sxs-lookup"><span data-stu-id="220bb-111">Description</span></span>|  
|--------------------------|-----------------|  
|<span data-ttu-id="220bb-112">**/ logdir:**< LogDirectory\></span><span class="sxs-lookup"><span data-stu-id="220bb-112">**/logdir:**<LogDirectory\></span></span>|<span data-ttu-id="220bb-113">Választható.</span><span class="sxs-lookup"><span data-stu-id="220bb-113">Optional.</span></span> <span data-ttu-id="220bb-114">hello naplókönyvtár.</span><span class="sxs-lookup"><span data-stu-id="220bb-114">hello log directory.</span></span> <span data-ttu-id="220bb-115">Részletes naplófájlok toothis directory lesz írva.</span><span class="sxs-lookup"><span data-stu-id="220bb-115">Verbose log files will be written toothis directory.</span></span> <span data-ttu-id="220bb-116">Ha nincs naplókönyvtár meg van adva, hello aktuális könyvtárhoz hello naplókönyvtár lesz.</span><span class="sxs-lookup"><span data-stu-id="220bb-116">If no log directory is specified, hello current directory will be used as hello log directory.</span></span>|  
|<span data-ttu-id="220bb-117">**/sn:**< StorageAccountName\></span><span class="sxs-lookup"><span data-stu-id="220bb-117">**/sn:**<StorageAccountName\></span></span>|<span data-ttu-id="220bb-118">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="220bb-118">Required.</span></span> <span data-ttu-id="220bb-119">hello hello hello storage-fiók nevét a feladat exportálja.</span><span class="sxs-lookup"><span data-stu-id="220bb-119">hello name of hello storage account for hello export job.</span></span>|  
|<span data-ttu-id="220bb-120">**/SK:**< StorageAccountKey\></span><span class="sxs-lookup"><span data-stu-id="220bb-120">**/sk:**<StorageAccountKey\></span></span>|<span data-ttu-id="220bb-121">Csak ha nincs megadva a tároló SAS szükséges.</span><span class="sxs-lookup"><span data-stu-id="220bb-121">Required if and only if a container SAS is not specified.</span></span> <span data-ttu-id="220bb-122">hello fiók kulcsának hello hello a feladat exportálja.</span><span class="sxs-lookup"><span data-stu-id="220bb-122">hello account key for hello storage account for hello export job.</span></span>|  
|<span data-ttu-id="220bb-123">**/csas:**< ContainerSas\></span><span class="sxs-lookup"><span data-stu-id="220bb-123">**/csas:**<ContainerSas\></span></span>|<span data-ttu-id="220bb-124">Szükséges, csak ha nincs megadva a tárfiók kulcsára.</span><span class="sxs-lookup"><span data-stu-id="220bb-124">Required if and only if a storage account key is not specified.</span></span> <span data-ttu-id="220bb-125">hello tárolója SAS listaelem hello blobok toobe hello exportálási feladat exportálni.</span><span class="sxs-lookup"><span data-stu-id="220bb-125">hello container SAS for listing hello blobs toobe exported in hello export job.</span></span>|  
|<span data-ttu-id="220bb-126">**/ ExportBlobListFile:**< ExportBlobListFile\></span><span class="sxs-lookup"><span data-stu-id="220bb-126">**/ExportBlobListFile:**<ExportBlobListFile\></span></span>|<span data-ttu-id="220bb-127">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="220bb-127">Required.</span></span> <span data-ttu-id="220bb-128">Elérési út toohello XML blob elérési utak listája tartalmazó fájlt, vagy blob elérési út előtagokat hello blobok toobe exportált.</span><span class="sxs-lookup"><span data-stu-id="220bb-128">Path toohello XML file containing list of blob paths or blob path prefixes for hello blobs toobe exported.</span></span> <span data-ttu-id="220bb-129">hello fájlformátum, amelyet a hello `BlobListBlobPath` hello elemének [Put feladat](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) hello Import/Export szolgáltatás REST API működését.</span><span class="sxs-lookup"><span data-stu-id="220bb-129">hello file format used in hello `BlobListBlobPath` element in hello [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) operation of hello Import/Export service REST API.</span></span>|  
|<span data-ttu-id="220bb-130">**/ DriveSize:**< DriveSize\></span><span class="sxs-lookup"><span data-stu-id="220bb-130">**/DriveSize:**<DriveSize\></span></span>|<span data-ttu-id="220bb-131">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="220bb-131">Required.</span></span> <span data-ttu-id="220bb-132">az exportálási feladat, a meghajtók toouse mérete hello *pl.*, 500 GB, 1,5 TB.</span><span class="sxs-lookup"><span data-stu-id="220bb-132">hello size of drives toouse for an export job, *e.g.*, 500GB, 1.5TB.</span></span>|  

## <a name="command-line-example"></a><span data-ttu-id="220bb-133">Parancssori – példa</span><span class="sxs-lookup"><span data-stu-id="220bb-133">Command-line example</span></span>

<span data-ttu-id="220bb-134">hello következő példa bemutatja, hogyan hello `PreviewExport` parancs:</span><span class="sxs-lookup"><span data-stu-id="220bb-134">hello following example demonstrates hello `PreviewExport` command:</span></span>  
  
```  
WAImportExport.exe PreviewExport /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\WAImportExport\mybloblist.xml /DriveSize:500GB    
```  
  
<span data-ttu-id="220bb-135">hello blob lista exportfájl blob nevének és tartalmazhat blob-előtagok, ahogy az itt látható:</span><span class="sxs-lookup"><span data-stu-id="220bb-135">hello export blob list file may contain blob names and blob prefixes, as shown here:</span></span>  
  
```xml 
<?xml version="1.0" encoding="utf-8"?>  
<BlobList>  
<BlobPath>pictures/animals/koala.jpg</BlobPath>  
<BlobPathPrefix>/vhds/</BlobPathPrefix>  
<BlobPathPrefix>/movies/</BlobPathPrefix>  
</BlobList>  
```

<span data-ttu-id="220bb-136">hello Azure Import/Export eszköz exportált összes BLOB toobe sorolja fel, és számítja ki, hogyan őket hello meghajtóit megadott mérete, bármilyen szükséges terhelést, figyelembe véve majd meghajtók hello számának becslése toopack toohold hello blobokat és a meghajtó használata szükséges információ.</span><span class="sxs-lookup"><span data-stu-id="220bb-136">hello Azure Import/Export Tool lists all blobs toobe exported and calculates how toopack them into drives of hello specified size, taking into account any necessary overhead, then estimates hello number of drives needed toohold hello blobs and drive usage information.</span></span>  
  
<span data-ttu-id="220bb-137">Itt látható egy példa hello kimeneti, és nincs megadva tájékoztató naplófájlokat:</span><span class="sxs-lookup"><span data-stu-id="220bb-137">Here is an example of hello output, with informational logs omitted:</span></span>  
  
```  
Number of unique blob paths/prefixes:   3  
Number of duplicate blob paths/prefixes:        0  
Number of nonexistent blob paths/prefixes:      1  
  
Drive size:     500.00 GB  
Number of blobs that can be exported:   6  
Number of blobs that cannot be exported:        2  
Number of drives needed:        3  
        Drive #1:       blobs = 1, occupied space = 454.74 GB  
        Drive #2:       blobs = 3, occupied space = 441.37 GB  
        Drive #3:       blobs = 2, occupied space = 131.28 GB    
```  
  
## <a name="next-steps"></a><span data-ttu-id="220bb-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="220bb-138">Next steps</span></span>

* [<span data-ttu-id="220bb-139">Az Azure Import/Export eszköz referencia</span><span class="sxs-lookup"><span data-stu-id="220bb-139">Azure Import/Export Tool reference</span></span>](../storage-import-export-tool-how-to-v1.md)
