---
title: "Másolja, vagy helyezze át az adatok Azure Storage az AzCopy Windows |} Microsoft Docs"
description: "Windows-segédprogram az AzCopy segítségével áthelyezi vagy másolja az adatokat, vagy a blob, table és a fájl. Adatok másolása az Azure Storage a helyi fájlokból, vagy másolja az adatokat belül vagy tárfiókok között. Adatok áttelepítése egyszerű Azure Storage."
services: storage
documentationcenter: 
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: aa155738-7c69-4a83-94f8-b97af4461274
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/14/2017
ms.author: seguler
ms.openlocfilehash: 045778822022752295bb634bdf734daaf36ab938
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="transfer-data-with-the-azcopy-on-windows"></a><span data-ttu-id="447a0-105">Adatátvitel az AzCopy a Windows rendszeren</span><span class="sxs-lookup"><span data-stu-id="447a0-105">Transfer data with the AzCopy on Windows</span></span>
<span data-ttu-id="447a0-106">AzCopy az adatok másolása, és az egyszerű parancsokkal optimális teljesítménnyel Microsoft Azure Blob, a fájl és a Table storage egy parancssori segédprogram.</span><span class="sxs-lookup"><span data-stu-id="447a0-106">AzCopy is a command-line utility designed for copying data to and from Microsoft Azure Blob, File, and Table storage using simple commands with optimal performance.</span></span> <span data-ttu-id="447a0-107">Másolhatja adatok egy objektum a másikra a tárfiókon belül vagy tárfiókok között.</span><span class="sxs-lookup"><span data-stu-id="447a0-107">You can copy data from one object to another within your storage account, or between storage accounts.</span></span>

<span data-ttu-id="447a0-108">AzCopy, letöltheti a két verziója van.</span><span class="sxs-lookup"><span data-stu-id="447a0-108">There are two versions of AzCopy that you can download.</span></span> <span data-ttu-id="447a0-109">AzCopy a Windows a .NET-keretrendszer épül, és ez biztosítja a Windows stílus parancssori kapcsolókat.</span><span class="sxs-lookup"><span data-stu-id="447a0-109">AzCopy on Windows is built with .NET Framework, and offers Windows style command-line options.</span></span> <span data-ttu-id="447a0-110">[AzCopy Linux](storage-use-azcopy-linux.md) épül, a .NET Core keretprogram, amelyik ajánlat POSIX-stílusú parancssori kapcsolók Linux platformon célozza.</span><span class="sxs-lookup"><span data-stu-id="447a0-110">[AzCopy on Linux](storage-use-azcopy-linux.md) is built with .NET Core Framework which targets Linux platforms offering POSIX style command-line options.</span></span> <span data-ttu-id="447a0-111">Ez a cikk a Windows AzCopy ismerteti.</span><span class="sxs-lookup"><span data-stu-id="447a0-111">This article covers AzCopy on Windows.</span></span>

## <a name="download-and-install-azcopy"></a><span data-ttu-id="447a0-112">Töltse le és telepítse az AzCopy</span><span class="sxs-lookup"><span data-stu-id="447a0-112">Download and install AzCopy</span></span>
### <a name="azcopy-on-windows"></a><span data-ttu-id="447a0-113">AzCopy Windowson</span><span class="sxs-lookup"><span data-stu-id="447a0-113">AzCopy on Windows</span></span>
<span data-ttu-id="447a0-114">Töltse le a [Windows AzCopy legújabb verzióját](http://aka.ms/downloadazcopy).</span><span class="sxs-lookup"><span data-stu-id="447a0-114">Download the [latest version of AzCopy on Windows](http://aka.ms/downloadazcopy).</span></span>

#### <a name="installation-on-windows"></a><span data-ttu-id="447a0-115">Windows-telepítés</span><span class="sxs-lookup"><span data-stu-id="447a0-115">Installation on Windows</span></span>
<span data-ttu-id="447a0-116">Miután telepítette a AzCopy a Windows, a telepítő használatával, nyisson meg egy parancsablakot, és keresse meg az AzCopy telepítési könyvtárára a számítógép - ha a `AzCopy.exe` végrehajtható található.</span><span class="sxs-lookup"><span data-stu-id="447a0-116">After installing AzCopy on Windows using the installer, open a command window and navigate to the AzCopy installation directory on your computer - where the `AzCopy.exe` executable is located.</span></span> <span data-ttu-id="447a0-117">Ha szükséges, az AzCopy telepítési hely a fájlrendszerbeli elérési is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="447a0-117">If desired, you can add the AzCopy installation location to your system path.</span></span> <span data-ttu-id="447a0-118">Alapértelmezés szerint AzCopy telepítés `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` vagy `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="447a0-118">By default, AzCopy is installed to `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` or `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.</span></span>

## <a name="writing-your-first-azcopy-command"></a><span data-ttu-id="447a0-119">Az első AzCopy parancs írása</span><span class="sxs-lookup"><span data-stu-id="447a0-119">Writing your first AzCopy command</span></span>
<span data-ttu-id="447a0-120">Az AzCopy parancs alapvető szintaxisa:</span><span class="sxs-lookup"><span data-stu-id="447a0-120">The basic syntax for AzCopy commands is:</span></span>

```azcopy
AzCopy /Source:<source> /Dest:<destination> [Options]
```

<span data-ttu-id="447a0-121">Az alábbi példák bemutatják, különféle forgatókönyvekhez, amik másolása adatok és a Microsoft Azure-Blobokkal, fájlok és táblák esetében.</span><span class="sxs-lookup"><span data-stu-id="447a0-121">The following examples demonstrate a variety of scenarios for copying data to and from Microsoft Azure Blobs, Files, and Tables.</span></span> <span data-ttu-id="447a0-122">Tekintse meg a [AzCopy paraméterek](#azcopy-parameters) részletes leírását az egyes mintában használt paraméterek szakaszban.</span><span class="sxs-lookup"><span data-stu-id="447a0-122">Refer to the [AzCopy Parameters](#azcopy-parameters) section for a detailed explanation of the parameters used in each sample.</span></span>

## <a name="blob-download"></a><span data-ttu-id="447a0-123">BLOB: letöltése</span><span class="sxs-lookup"><span data-stu-id="447a0-123">Blob: Download</span></span>
### <a name="download-single-blob"></a><span data-ttu-id="447a0-124">Egy blob letöltése</span><span class="sxs-lookup"><span data-stu-id="447a0-124">Download single blob</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"
```

<span data-ttu-id="447a0-125">Vegye figyelembe, hogy ha a mappa `C:\myfolder` nem létezik, AzCopy fog létrehozni, és töltse le `abc.txt ` az új mappába.</span><span class="sxs-lookup"><span data-stu-id="447a0-125">Note that if the folder `C:\myfolder` does not exist, AzCopy will create it and download `abc.txt ` into the new folder.</span></span>

### <a name="download-single-blob-from-secondary-region"></a><span data-ttu-id="447a0-126">A másodlagos régióba egyetlen blob letöltése</span><span class="sxs-lookup"><span data-stu-id="447a0-126">Download single blob from secondary region</span></span>

```azcopy
AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

<span data-ttu-id="447a0-127">Vegye figyelembe, hogy írásvédett georedundáns tárolás engedélyezve kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="447a0-127">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="download-all-blobs"></a><span data-ttu-id="447a0-128">Töltse le az összes BLOB</span><span class="sxs-lookup"><span data-stu-id="447a0-128">Download all blobs</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S
```

<span data-ttu-id="447a0-129">Tegyük fel, a következő blobot a megadott tárolóban találhatók:</span><span class="sxs-lookup"><span data-stu-id="447a0-129">Assume the following blobs reside in the specified container:</span></span>  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

<span data-ttu-id="447a0-130">A könyvtár a letöltési művelet után `C:\myfolder` a következő fájlokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="447a0-130">After the download operation, the directory `C:\myfolder` will include the following files:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

<span data-ttu-id="447a0-131">Ha nem adja meg a beállítás `/S`, nincs BLOB le lesznek töltve.</span><span class="sxs-lookup"><span data-stu-id="447a0-131">If you do not specify option `/S`, no blobs will be downloaded.</span></span>

### <a name="download-blobs-with-specified-prefix"></a><span data-ttu-id="447a0-132">A megadott előtag blobok letöltése</span><span class="sxs-lookup"><span data-stu-id="447a0-132">Download blobs with specified prefix</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S
```

<span data-ttu-id="447a0-133">Tegyük fel, a következő blobok találhatók a megadott tárolóban.</span><span class="sxs-lookup"><span data-stu-id="447a0-133">Assume the following blobs reside in the specified container.</span></span> <span data-ttu-id="447a0-134">A előtaggal kezdődő összes BLOB `a` tölthető le:</span><span class="sxs-lookup"><span data-stu-id="447a0-134">All blobs beginning with the prefix `a` will be downloaded:</span></span>

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

<span data-ttu-id="447a0-135">A letöltési mappa művelet után `C:\myfolder` a következő fájlokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="447a0-135">After the download operation, the folder `C:\myfolder` will include the following files:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

<span data-ttu-id="447a0-136">Az előtag a virtuális könyvtárban, a blob nevének első részét képező vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="447a0-136">The prefix applies to the virtual directory, which forms the first part of the blob name.</span></span> <span data-ttu-id="447a0-137">A fenti példában a virtuális könyvtár nem egyezik a megadott előtagot, így azt nem töltődik le.</span><span class="sxs-lookup"><span data-stu-id="447a0-137">In the example shown above, the virtual directory does not match the specified prefix, so it is not downloaded.</span></span> <span data-ttu-id="447a0-138">Emellett ha a beállítás `\S` nincs megadva, az AzCopy nem tölti le a blobokat.</span><span class="sxs-lookup"><span data-stu-id="447a0-138">In addition, if the option `\S` is not specified, AzCopy will not download any blobs.</span></span>

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a><span data-ttu-id="447a0-139">Állítsa be az exportált fájlokat lehet ugyanaz, mint a forrás utolsó módosításának időpontja</span><span class="sxs-lookup"><span data-stu-id="447a0-139">Set the last-modified time of exported files to be same as the source blobs</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT
```

<span data-ttu-id="447a0-140">Blobok emellett kizárja a letöltési művelet az utolsó módosításának ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="447a0-140">You can also exclude blobs from the download operation based on their last-modified time.</span></span> <span data-ttu-id="447a0-141">Például, ha ki szeretné zárni a blobok, amelyek utolsó módosításának ideje azonos vagy újabb, mint a célfájl, vegye fel a `/XN` lehetőséget:</span><span class="sxs-lookup"><span data-stu-id="447a0-141">For example, if you want to exclude blobs whose last modified time is the same or newer than the destination file, add the `/XN` option:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN
```

<span data-ttu-id="447a0-142">Vagy ha ki szeretné zárni a blobok, amelyek utolsó módosítás időpontja nem azonos vagy régebbi, mint a célfájl, vegye fel a `/XO` lehetőséget:</span><span class="sxs-lookup"><span data-stu-id="447a0-142">Or if you want to exclude blobs whose last modified time is the same or older than the destination file, add the `/XO` option:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO
```

## <a name="blob-upload"></a><span data-ttu-id="447a0-143">BLOB: feltöltése</span><span class="sxs-lookup"><span data-stu-id="447a0-143">Blob: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="447a0-144">Töltse fel egy fájlból</span><span class="sxs-lookup"><span data-stu-id="447a0-144">Upload single file</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"
```

<span data-ttu-id="447a0-145">Ha a megadott célhely tároló nem létezik, a AzCopy hozza létre, és be feltölteni a fájlt.</span><span class="sxs-lookup"><span data-stu-id="447a0-145">If the specified destination container does not exist, AzCopy will create it and upload the file into it.</span></span>

### <a name="upload-single-file-to-virtual-directory"></a><span data-ttu-id="447a0-146">Egy fájlból töltse fel a virtuális könyvtár</span><span class="sxs-lookup"><span data-stu-id="447a0-146">Upload single file to virtual directory</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt
```

<span data-ttu-id="447a0-147">Ha a megadott virtuális könyvtár nem létezik, AzCopy fel kell töltenie a fájlt a virtuális könyvtár nevét (*pl.*, `vd/abc.txt` a fenti példában).</span><span class="sxs-lookup"><span data-stu-id="447a0-147">If the specified virtual directory does not exist, AzCopy will upload the file to include the virtual directory in its name (*e.g.*, `vd/abc.txt` in the example above).</span></span>

### <a name="upload-all-files"></a><span data-ttu-id="447a0-148">Minden fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="447a0-148">Upload all files</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S
```

<span data-ttu-id="447a0-149">Beállítás megadása `/S` Blob storage rekurzív módon, ami azt jelenti, hogy minden almappa és a fájlok lesz feltöltve, valamint feltölti a megadott könyvtár tartalmát.</span><span class="sxs-lookup"><span data-stu-id="447a0-149">Specifying option `/S` uploads the contents of the specified directory to Blob storage recursively, meaning that all subfolders and their files will be uploaded as well.</span></span> <span data-ttu-id="447a0-150">Például a mappában találhatók a következő fájlok feltételezik `C:\myfolder`:</span><span class="sxs-lookup"><span data-stu-id="447a0-150">For instance, assume the following files reside in folder `C:\myfolder`:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

<span data-ttu-id="447a0-151">A tároló a feltöltési művelet után a következő fájlokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="447a0-151">After the upload operation, the container will include the following files:</span></span>

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

<span data-ttu-id="447a0-152">Ha nem adja meg a beállítás `/S`, AzCopy nem rekurzív módon tölti fel.</span><span class="sxs-lookup"><span data-stu-id="447a0-152">If you do not specify option `/S`, AzCopy will not upload recursively.</span></span> <span data-ttu-id="447a0-153">A tároló a feltöltési művelet után a következő fájlokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="447a0-153">After the upload operation, the container will include the following files:</span></span>

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="447a0-154">Töltse fel a megadott mintának megfelelő fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-154">Upload files matching specified pattern</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S
```

<span data-ttu-id="447a0-155">Tegyük fel, a következő fájlok mappában találhatók `C:\myfolder`:</span><span class="sxs-lookup"><span data-stu-id="447a0-155">Assume the following files reside in folder `C:\myfolder`:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

<span data-ttu-id="447a0-156">A tároló a feltöltési művelet után a következő fájlokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="447a0-156">After the upload operation, the container will include the following files:</span></span>

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

<span data-ttu-id="447a0-157">Ha nem adja meg a beállítás `/S`, AzCopy csak fel kell töltenie a virtuális könyvtár nem található blobok:</span><span class="sxs-lookup"><span data-stu-id="447a0-157">If you do not specify option `/S`, AzCopy will only upload blobs that don't reside in a virtual directory:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a><span data-ttu-id="447a0-158">Adjon meg egy cél blob MIME tartalomtípus</span><span class="sxs-lookup"><span data-stu-id="447a0-158">Specify the MIME content type of a destination blob</span></span>
<span data-ttu-id="447a0-159">Alapértelmezés szerint az AzCopy beállítja egy cél blobot tartalomtípusa `application/octet-stream`.</span><span class="sxs-lookup"><span data-stu-id="447a0-159">By default, AzCopy sets the content type of a destination blob to `application/octet-stream`.</span></span> <span data-ttu-id="447a0-160">3.1.0 verziójával kezdve explicit módon megadhatja a tartalomtípus keresztül beállítás `/SetContentType:[content-type]`.</span><span class="sxs-lookup"><span data-stu-id="447a0-160">Beginning with version 3.1.0, you can explicitly specify the content type via the option `/SetContentType:[content-type]`.</span></span> <span data-ttu-id="447a0-161">Ez a szintaxis a feltöltési művelet állítja be a content-type összes BLOB.</span><span class="sxs-lookup"><span data-stu-id="447a0-161">This syntax sets the content type for all blobs in an upload operation.</span></span>

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4
```

<span data-ttu-id="447a0-162">Ha megad `/SetContentType` nélkül értéket, majd AzCopy állítja be minden egyes blob vagy a fájl tartalomtípusa alapján a fájl kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="447a0-162">If you specify `/SetContentType` without a value, then AzCopy will set each blob or file's content type according to its file extension.</span></span>

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType
```

## <a name="blob-copy"></a><span data-ttu-id="447a0-163">BLOB: másolása</span><span class="sxs-lookup"><span data-stu-id="447a0-163">Blob: Copy</span></span>
### <a name="copy-single-blob-within-storage-account"></a><span data-ttu-id="447a0-164">Másolja át egy blob Storage-fiókban</span><span class="sxs-lookup"><span data-stu-id="447a0-164">Copy single blob within Storage account</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt
```

<span data-ttu-id="447a0-165">Ha egy blobba egy tárfiókon belül másol egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.</span><span class="sxs-lookup"><span data-stu-id="447a0-165">When you copy a blob within a Storage account, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-across-storage-accounts"></a><span data-ttu-id="447a0-166">Másolja át egy blob Storage-fiókok között</span><span class="sxs-lookup"><span data-stu-id="447a0-166">Copy single blob across Storage accounts</span></span>

```azcopy
AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

<span data-ttu-id="447a0-167">A blob Storage-fiókok között másolásakor egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.</span><span class="sxs-lookup"><span data-stu-id="447a0-167">When you copy a blob across Storage accounts, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a><span data-ttu-id="447a0-168">Egy blob másolása másodlagos régióba elsődleges régió</span><span class="sxs-lookup"><span data-stu-id="447a0-168">Copy single blob from secondary region to primary region</span></span>

```azcopy
AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

<span data-ttu-id="447a0-169">Vegye figyelembe, hogy írásvédett georedundáns tárolás engedélyezve kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="447a0-169">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a><span data-ttu-id="447a0-170">Egy blob másolási és a pillanatképek tárfiókok között</span><span class="sxs-lookup"><span data-stu-id="447a0-170">Copy single blob and its snapshots across Storage accounts</span></span>

```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot
```

<span data-ttu-id="447a0-171">A másolási művelet után a céltároló tartalmazza a blob és a pillanatképek.</span><span class="sxs-lookup"><span data-stu-id="447a0-171">After the copy operation, the target container will include the blob and its snapshots.</span></span> <span data-ttu-id="447a0-172">Ha a fenti példában a blob két pillanatképekkel rendelkezik, a tároló tartalmazza a következő blob és pillanatképeket:</span><span class="sxs-lookup"><span data-stu-id="447a0-172">Assuming the blob in the example above has two snapshots, the container will include the following blob and snapshots:</span></span>

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="synchronously-copy-blobs-across-storage-accounts"></a><span data-ttu-id="447a0-173">Szinkron módon másolni BLOB Storage-fiókok</span><span class="sxs-lookup"><span data-stu-id="447a0-173">Synchronously copy blobs across Storage accounts</span></span>
<span data-ttu-id="447a0-174">AzCopy alapértelmezés szerint aszinkron módon másolja az adatokat két tárolási végpontok közötti.</span><span class="sxs-lookup"><span data-stu-id="447a0-174">AzCopy by default copies data between two storage endpoints asynchronously.</span></span> <span data-ttu-id="447a0-175">Ezért a másolási művelet fog futni a háttérben használja, amely nem szolgáltatásiszint-szerződés szempontjából hogyan gyors rendelkezik tartalékolt sávszélesség-kapacitása blob kerülnek, és AzCopy rendszeresen ellenőrzi az-másolat állapota mindaddig, amíg a másolása befejeződött, vagy nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="447a0-175">Therefore, the copy operation will run in the background using spare bandwidth capacity that has no SLA in terms of how fast a blob will be copied, and AzCopy will periodically check the copy status until the copying is completed or failed.</span></span>

<span data-ttu-id="447a0-176">A `/SyncCopy` lehetőség biztosítja, hogy a másolási művelet egységes sebesség fogja kapni.</span><span class="sxs-lookup"><span data-stu-id="447a0-176">The `/SyncCopy` option ensures that the copy operation will get consistent speed.</span></span> <span data-ttu-id="447a0-177">AzCopy a szinkron másolatot helyi memória a megadott forrás másolása a blobok letöltése, és feltöltheti a Blob storage cél végez.</span><span class="sxs-lookup"><span data-stu-id="447a0-177">AzCopy performs the synchronous copy by downloading the blobs to copy from the specified source to local memory, and then uploading them to the Blob storage destination.</span></span>

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy
```

<span data-ttu-id="447a0-178">`/SyncCopy`További kilépő költség képest aszinkron másolási hozhat létre, az ajánlott módszer az, hogy egy Azure virtuális gép, amely ugyanabban a régióban, a forrás tárolási fiókkal kilépő költségek elkerülése érdekében használja ezt a beállítást.</span><span class="sxs-lookup"><span data-stu-id="447a0-178">`/SyncCopy` might generate additional egress cost compared to asynchronous copy, the recommended approach is to use this option in an Azure VM that is in the same region as your source storage account to avoid egress cost.</span></span>

## <a name="file-download"></a><span data-ttu-id="447a0-179">Fájl: letöltése</span><span class="sxs-lookup"><span data-stu-id="447a0-179">File: Download</span></span>
### <a name="download-single-file"></a><span data-ttu-id="447a0-180">Töltse le egy fájlból</span><span class="sxs-lookup"><span data-stu-id="447a0-180">Download single file</span></span>

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

<span data-ttu-id="447a0-181">Ha a megadott forrás az Azure fájlmegosztások, akkor meg kell adnia a fájl pontos nevét (*pl.* `abc.txt`) egyetlen fájl letöltéséhez vagy beállítást adja meg `/S` a megosztás rekurzív módon található összes fájl letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="447a0-181">If the specified source is an Azure file share, then you must either specify the exact file name, (*e.g.* `abc.txt`) to download a single file, or specify option `/S` to download all files in the share recursively.</span></span> <span data-ttu-id="447a0-182">Adjon meg egy fájl mintát és a beállítás próbál `/S` együtt hibát eredményez.</span><span class="sxs-lookup"><span data-stu-id="447a0-182">Attempting to specify both a file pattern and option `/S` together will result in an error.</span></span>

### <a name="download-all-files"></a><span data-ttu-id="447a0-183">Minden fájl letöltése</span><span class="sxs-lookup"><span data-stu-id="447a0-183">Download all files</span></span>

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S
```

<span data-ttu-id="447a0-184">Vegye figyelembe, hogy üres mappák nem lesznek letöltve.</span><span class="sxs-lookup"><span data-stu-id="447a0-184">Note that any empty folders will not be downloaded.</span></span>

## <a name="file-upload"></a><span data-ttu-id="447a0-185">Fájl: feltöltése</span><span class="sxs-lookup"><span data-stu-id="447a0-185">File: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="447a0-186">Töltse fel egy fájlból</span><span class="sxs-lookup"><span data-stu-id="447a0-186">Upload single file</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt
```

### <a name="upload-all-files"></a><span data-ttu-id="447a0-187">Minden fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="447a0-187">Upload all files</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S
```

<span data-ttu-id="447a0-188">Vegye figyelembe, hogy bármely üres mappa nem lesz feltöltve.</span><span class="sxs-lookup"><span data-stu-id="447a0-188">Note that any empty folders will not be uploaded.</span></span>

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="447a0-189">Töltse fel a megadott mintának megfelelő fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-189">Upload files matching specified pattern</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S
```

## <a name="file-copy"></a><span data-ttu-id="447a0-190">Fájl: másolása</span><span class="sxs-lookup"><span data-stu-id="447a0-190">File: Copy</span></span>
### <a name="copy-across-file-shares"></a><span data-ttu-id="447a0-191">Fájlmegosztások másolni</span><span class="sxs-lookup"><span data-stu-id="447a0-191">Copy across file shares</span></span>

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S
```
<span data-ttu-id="447a0-192">Amikor fájlmegosztások között másolhat egy fájlt egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.</span><span class="sxs-lookup"><span data-stu-id="447a0-192">When you copy a file across file shares, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-from-file-share-to-blob"></a><span data-ttu-id="447a0-193">Megosztás és a blob másolása</span><span class="sxs-lookup"><span data-stu-id="447a0-193">Copy from file share to blob</span></span>

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S
```
<span data-ttu-id="447a0-194">Amikor másolhat egy fájlt a blobra, fájlmegosztásról egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.</span><span class="sxs-lookup"><span data-stu-id="447a0-194">When you copy a file from file share to blob, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>


### <a name="copy-from-blob-to-file-share"></a><span data-ttu-id="447a0-195">Fájlmegosztás a blob másolása</span><span class="sxs-lookup"><span data-stu-id="447a0-195">Copy from blob to file share</span></span>

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S
```
<span data-ttu-id="447a0-196">Másolhat egy fájlt blobból fájlmegosztáshoz, amikor egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.</span><span class="sxs-lookup"><span data-stu-id="447a0-196">When you copy a file from blob to file share, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="synchronously-copy-files"></a><span data-ttu-id="447a0-197">Szinkron módon történik a fájlok másolása</span><span class="sxs-lookup"><span data-stu-id="447a0-197">Synchronously copy files</span></span>
<span data-ttu-id="447a0-198">Megadhatja a `/SyncCopy` adatok másolása a File Storage a File Storage, File Storage-Blob Storage és File Storage Blob Storage szinkron módon beállításnál AzCopy ezt hajtja végre az adatok letöltése a helyi memória, és töltse fel újra cél.</span><span class="sxs-lookup"><span data-stu-id="447a0-198">You can specify the `/SyncCopy` option to copy data from File Storage to File Storage, from File Storage to Blob Storage and from Blob Storage to File Storage synchronously, AzCopy does this by downloading the source data to local memory and upload it again to destination.</span></span> <span data-ttu-id="447a0-199">Standard kilépő költség fog vonatkozni.</span><span class="sxs-lookup"><span data-stu-id="447a0-199">Standard egress cost will apply.</span></span>

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy
```

<span data-ttu-id="447a0-200">Amikor másol a File Storage Blob Storage, az alapértelmezett blob típushoz blokkblob, felhasználói beállítást adja meg `/BlobType:page` módosíthatja a blob típusára.</span><span class="sxs-lookup"><span data-stu-id="447a0-200">When copying from File Storage to Blob Storage, the default blob type is block blob, user can specify option `/BlobType:page` to change the destination blob type.</span></span>

<span data-ttu-id="447a0-201">Vegye figyelembe, hogy `/SyncCopy` további költségeket, aszinkron másolási való hasonlítás kilépő hozhat létre, az ajánlott módszer az, hogy ez a beállítás használata az Azure virtuális Gépen, amely ugyanabban a régióban, a forrás tárolási fiókkal kilépő költségek elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="447a0-201">Note that `/SyncCopy` might generate additional egress cost comparing to asynchronous copy, the recommended approach is to use this option in the Azure VM which is in the same region as your source storage account to avoid egress cost.</span></span>

## <a name="table-export"></a><span data-ttu-id="447a0-202">Tábla: exportálása</span><span class="sxs-lookup"><span data-stu-id="447a0-202">Table: Export</span></span>
### <a name="export-table"></a><span data-ttu-id="447a0-203">Tábla exportálása</span><span class="sxs-lookup"><span data-stu-id="447a0-203">Export table</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key
```

<span data-ttu-id="447a0-204">AzCopy ír a megadott célmappát jegyzékfájlt.</span><span class="sxs-lookup"><span data-stu-id="447a0-204">AzCopy writes a manifest file to the specified destination folder.</span></span> <span data-ttu-id="447a0-205">A jegyzékfájl az importálási folyamat szerepel állapítsa meg a szükséges adatok helyét és adatellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="447a0-205">The manifest file is used in the import process to locate the necessary data files and perform data validation.</span></span> <span data-ttu-id="447a0-206">A jegyzékfájl alapértelmezés szerint használja a következő elnevezés szabály szerint:</span><span class="sxs-lookup"><span data-stu-id="447a0-206">The manifest file uses the following naming convention by default:</span></span>

    <account name>_<table name>_<timestamp>.manifest

<span data-ttu-id="447a0-207">Felhasználói is adja meg a beállítást `/Manifest:<manifest file name>` beállítása a jegyzékfájl neve.</span><span class="sxs-lookup"><span data-stu-id="447a0-207">User can also specify the option `/Manifest:<manifest file name>` to set the manifest file name.</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest
```

### <a name="split-export-into-multiple-files"></a><span data-ttu-id="447a0-208">Több fájlra vegyes exportálása</span><span class="sxs-lookup"><span data-stu-id="447a0-208">Split export into multiple files</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100
```

<span data-ttu-id="447a0-209">AzCopy használ egy *kötet index* a megosztott adatok fájlnevekben segítségével különböztetheti meg egymástól több fájlt.</span><span class="sxs-lookup"><span data-stu-id="447a0-209">AzCopy uses a *volume index* in the split data file names to distinguish multiple files.</span></span> <span data-ttu-id="447a0-210">A kötet index két részből áll egy *partíciós kulcs tartományindexszel* és egy *vegyes fájl index*.</span><span class="sxs-lookup"><span data-stu-id="447a0-210">The volume index consists of two parts, a *partition key range index* and a *split file index*.</span></span> <span data-ttu-id="447a0-211">Mindkét indexek nulla alapú.</span><span class="sxs-lookup"><span data-stu-id="447a0-211">Both indexes are zero-based.</span></span>

<span data-ttu-id="447a0-212">A partíciós kulcs tartományindexszel 0 lesz, ha a felhasználó nem adja meg a beállítás `/PKRS`.</span><span class="sxs-lookup"><span data-stu-id="447a0-212">The partition key range index will be 0 if user does not specify option `/PKRS`.</span></span>

<span data-ttu-id="447a0-213">Tegyük fel például, AzCopy két adatfájlok állít elő, miután a felhasználó határozza meg a beállítás `/SplitSize`.</span><span class="sxs-lookup"><span data-stu-id="447a0-213">For instance, suppose AzCopy generates two data files after the user specifies option `/SplitSize`.</span></span> <span data-ttu-id="447a0-214">Az eredményül kapott fájl nevének lehet:</span><span class="sxs-lookup"><span data-stu-id="447a0-214">The resulting data file names might be:</span></span>

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

<span data-ttu-id="447a0-215">Vegye figyelembe, hogy a beállítás értékének a legkisebb lehetséges `/SplitSize` 32 MB-nál.</span><span class="sxs-lookup"><span data-stu-id="447a0-215">Note that the minimum possible value for option `/SplitSize` is 32MB.</span></span> <span data-ttu-id="447a0-216">Ha egy konkrét célhoz Blob-tároló, AzCopy az adatfájl vágási után annak mérete eléri a blob mérete korlátozást (200 GB-os), függetlenül attól, hogy lehetőséget `/SplitSize` a felhasználó által meghatározott.</span><span class="sxs-lookup"><span data-stu-id="447a0-216">If the specified destination is Blob storage, AzCopy will split the data file once its sizes reaches the blob size limitation (200GB), regardless of whether option `/SplitSize` has been specified by the user.</span></span>

### <a name="export-table-to-json-or-csv-data-file-format"></a><span data-ttu-id="447a0-217">Tábla exportálása JSON- vagy CSV fájl formátuma</span><span class="sxs-lookup"><span data-stu-id="447a0-217">Export table to JSON or CSV data file format</span></span>
<span data-ttu-id="447a0-218">Alapértelmezés szerint AzCopy JSON-adatfájljait táblák exportálása.</span><span class="sxs-lookup"><span data-stu-id="447a0-218">AzCopy by default exports tables to JSON data files.</span></span> <span data-ttu-id="447a0-219">Megadhatja, hogy a beállítás `/PayloadFormat:JSON|CSV` JSON vagy a fürt megosztott kötetei szolgáltatás a táblák exportálása.</span><span class="sxs-lookup"><span data-stu-id="447a0-219">You can specify the option `/PayloadFormat:JSON|CSV` to export the tables as JSON or CSV.</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV
```

<span data-ttu-id="447a0-220">A fürt megosztott kötetei szolgáltatás az adattartalom formátuma megadásakor AzCopy is létrehoz egy séma kiterjesztésű fájlt `.schema.csv` minden olyan adatfájl esetében.</span><span class="sxs-lookup"><span data-stu-id="447a0-220">When specifying the CSV payload format, AzCopy will also generate a schema file with file extension `.schema.csv` for each data file.</span></span>

### <a name="export-table-entities-concurrently"></a><span data-ttu-id="447a0-221">Táblaentitásokat egyidejűleg exportálása</span><span class="sxs-lookup"><span data-stu-id="447a0-221">Export table entities concurrently</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"
```

<span data-ttu-id="447a0-222">AzCopy indul entitások exportálása, amikor a felhasználó határozza meg a beállítás párhuzamos műveletek `/PKRS`.</span><span class="sxs-lookup"><span data-stu-id="447a0-222">AzCopy will start concurrent operations to export entities when the user specifies option `/PKRS`.</span></span> <span data-ttu-id="447a0-223">Egyes műveletek egy kulcs partíciótartomány exportálja.</span><span class="sxs-lookup"><span data-stu-id="447a0-223">Each operation exports one partition key range.</span></span>

<span data-ttu-id="447a0-224">Vegye figyelembe, hogy a párhuzamos műveletek számát is beállítás által vezérelt `/NC`.</span><span class="sxs-lookup"><span data-stu-id="447a0-224">Note that the number of concurrent operations is also controlled by option `/NC`.</span></span> <span data-ttu-id="447a0-225">AzCopy Processzormagok száma használja, mint az alapértelmezett érték `/NC` táblaentitásokat, másolásakor akkor is, ha `/NC` nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="447a0-225">AzCopy uses the number of core processors as the default value of `/NC` when copying table entities, even if `/NC` was not specified.</span></span> <span data-ttu-id="447a0-226">Amikor a felhasználó határozza meg a beállítás `/PKRS`, AzCopy használja a két érték közül a kisebbet,-partíció kulcstartományokkal és implicit vagy explicit módon megadott párhuzamos műveletek – a párhuzamos műveletek elindításához számának meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="447a0-226">When the user specifies option `/PKRS`, AzCopy uses the smaller of the two values - partition key ranges versus implicitly or explicitly specified concurrent operations - to determine the number of concurrent operations to start.</span></span> <span data-ttu-id="447a0-227">További tudnivalókért írja be a `AzCopy /?:NC` a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="447a0-227">For more details, type `AzCopy /?:NC` at the command line.</span></span>

### <a name="export-table-to-blob"></a><span data-ttu-id="447a0-228">A blob-tábla exportálása</span><span class="sxs-lookup"><span data-stu-id="447a0-228">Export table to blob</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2
```

<span data-ttu-id="447a0-229">AzCopy hoz létre egy JSON-adatfájlt a blob tárolóba, a következő elnevezési konvenció:</span><span class="sxs-lookup"><span data-stu-id="447a0-229">AzCopy will generate a JSON data file into the blob container with following naming convention:</span></span>

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

<span data-ttu-id="447a0-230">A létrehozott JSON-fájl a következő minimális metaadat az adattartalom formátuma.</span><span class="sxs-lookup"><span data-stu-id="447a0-230">The generated JSON data file follows the payload format for minimal metadata.</span></span> <span data-ttu-id="447a0-231">Ez az adattartalom formátuma a részletekért lásd: [az adattartalom formátuma Table szolgáltatási műveletek](http://msdn.microsoft.com/library/azure/dn535600.aspx).</span><span class="sxs-lookup"><span data-stu-id="447a0-231">For details on this payload format, see [Payload Format for Table Service Operations](http://msdn.microsoft.com/library/azure/dn535600.aspx).</span></span>

<span data-ttu-id="447a0-232">Vegye figyelembe, hogy ha exportál táblák blobokat, AzCopy letölthesse a táblaentitásokat helyi ideiglenes fájlokat és e szervezetek majd feltölteni a blob.</span><span class="sxs-lookup"><span data-stu-id="447a0-232">Note that when exporting tables to blobs, AzCopy will download the Table entities to local temporary data files and then upload those entities to the blob.</span></span> <span data-ttu-id="447a0-233">Ezek ideiglenes fájlokat az alapértelmezett elérési úttal rendelkező napló fájl mappába kerülnek "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>", lehetőséget adhat meg/a napló módosítása [napló-fájlok és mappák] Z: fájl mappa helyét, és így módosíthatja az ideiglenes fájlok helyén.</span><span class="sxs-lookup"><span data-stu-id="447a0-233">These temporary data files are put into the journal file folder with the default path "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>", you can specify option /Z:[journal-file-folder] to change the journal file folder location and thus change the temporary data files location.</span></span> <span data-ttu-id="447a0-234">Az ideiglenes fájlok mérete, amelyekről a táblaentitásokat és a méretet, a megadott beállítás /SplitSize Bár a helyi lemezen lévő ideiglenes adatfájlt törlődnek azonnal Miután feltöltötte a blobra mutató adatok győződjön meg arról, hogy van elég helyi lemezterület ezek ideiglenes fájlokat tárolja a törlés előtt.</span><span class="sxs-lookup"><span data-stu-id="447a0-234">The temporary data files' size is decided by your table entities' size and the size you specified with the option /SplitSize, although the temporary data file in local disk will be deleted instantly once it has been uploaded to the blob, please make sure you have enough local disk space to store these temporary data files before they are deleted.</span></span>

## <a name="table-import"></a><span data-ttu-id="447a0-235">Tábla: importálása</span><span class="sxs-lookup"><span data-stu-id="447a0-235">Table: Import</span></span>
### <a name="import-table"></a><span data-ttu-id="447a0-236">Tábla importálása</span><span class="sxs-lookup"><span data-stu-id="447a0-236">Import table</span></span>

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace
```

<span data-ttu-id="447a0-237">A beállítás `/EntityOperation` azt jelzi, hogyan entitások beszúrására a táblában.</span><span class="sxs-lookup"><span data-stu-id="447a0-237">The option `/EntityOperation` indicates how to insert entities into the table.</span></span> <span data-ttu-id="447a0-238">Lehetséges értékek:</span><span class="sxs-lookup"><span data-stu-id="447a0-238">Possible values are:</span></span>

* <span data-ttu-id="447a0-239">`InsertOrSkip`: A meglévő entitás kihagyja vagy szúr be egy új entitást, ha a tábla nem létezik.</span><span class="sxs-lookup"><span data-stu-id="447a0-239">`InsertOrSkip`: Skips an existing entity or inserts a new entity if it does not exist in the table.</span></span>
* <span data-ttu-id="447a0-240">`InsertOrMerge`: Egyesíti a meglévő entitás vagy szúr be egy új entitást, ha a tábla nem létezik.</span><span class="sxs-lookup"><span data-stu-id="447a0-240">`InsertOrMerge`: Merges an existing entity or inserts a new entity if it does not exist in the table.</span></span>
* <span data-ttu-id="447a0-241">`InsertOrReplace`: A felváltja meglévő entitás vagy szúr be egy új entitást, ha a tábla nem létezik.</span><span class="sxs-lookup"><span data-stu-id="447a0-241">`InsertOrReplace`: Replaces an existing entity or inserts a new entity if it does not exist in the table.</span></span>

<span data-ttu-id="447a0-242">Vegye figyelembe, hogy a beállítás nem adható meg `/PKRS` importálási forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="447a0-242">Note that you cannot specify option `/PKRS` in the import scenario.</span></span> <span data-ttu-id="447a0-243">Az Exportálás kapcsolót kell megadnia a forgatókönyvben eltérően `/PKRS` párhuzamos műveletek, AzCopy lesz alapértelmezés szerint indítása a párhuzamos műveletek tábla importálásakor.</span><span class="sxs-lookup"><span data-stu-id="447a0-243">Unlike the export scenario, in which you must specify option `/PKRS` to start concurrent operations, AzCopy will by default start concurrent operations when you import a table.</span></span> <span data-ttu-id="447a0-244">Az alapértelmezett száma párhuzamos műveletek indítása megegyezik a Processzormagok; száma azonban megadhat egyidejű lehetőséggel különböző számú `/NC`.</span><span class="sxs-lookup"><span data-stu-id="447a0-244">The default number of concurrent operations started is equal to the number of core processors; however, you can specify a different number of concurrent with option `/NC`.</span></span> <span data-ttu-id="447a0-245">További tudnivalókért írja be a `AzCopy /?:NC` a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="447a0-245">For more details, type `AzCopy /?:NC` at the command line.</span></span>

<span data-ttu-id="447a0-246">Ügyeljen arra, hogy AzCopy csak támogatja a JSON, CSV nem importálása.</span><span class="sxs-lookup"><span data-stu-id="447a0-246">Note that AzCopy only supports importing for JSON, not CSV.</span></span> <span data-ttu-id="447a0-247">AzCopy nem támogatja a felhasználó által létrehozott JSON tábla származó és fájlok manifest.</span><span class="sxs-lookup"><span data-stu-id="447a0-247">AzCopy does not support table imports from user-created JSON and manifest files.</span></span> <span data-ttu-id="447a0-248">Az AzCopy tábla exportálása mindkét fájl kell származnia.</span><span class="sxs-lookup"><span data-stu-id="447a0-248">Both of these files must come from an AzCopy table export.</span></span> <span data-ttu-id="447a0-249">Hibák elkerülése érdekében adjon módosítsa az exportált JSON és jegyzékfájl.</span><span class="sxs-lookup"><span data-stu-id="447a0-249">To avoid errors, please do not modify the exported JSON or manifest file.</span></span>

### <a name="import-entities-to-table-using-blobs"></a><span data-ttu-id="447a0-250">Importálás entitások táblába a blobs használata</span><span class="sxs-lookup"><span data-stu-id="447a0-250">Import entities to table using blobs</span></span>
<span data-ttu-id="447a0-251">Tegyük fel, a Blob-tároló tartalmazza a következő: az Azure-tábla és a hozzá tartozó jegyzékfájl jelölő A JSON-fájlt.</span><span class="sxs-lookup"><span data-stu-id="447a0-251">Assume a Blob container contains the following: A JSON file representing an Azure Table and its accompanying manifest file.</span></span>

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

<span data-ttu-id="447a0-252">Entitások importálása egy táblába a jegyzékfájl az adott blob tároló a következő parancsot futtathatja:</span><span class="sxs-lookup"><span data-stu-id="447a0-252">You can run the following command to import entities into a table using the manifest file in that blob container:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"
```

## <a name="other-azcopy-features"></a><span data-ttu-id="447a0-253">Más AzCopy szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="447a0-253">Other AzCopy features</span></span>
### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a><span data-ttu-id="447a0-254">Csak másolja az adatokat, a cél nem létezik</span><span class="sxs-lookup"><span data-stu-id="447a0-254">Only copy data that doesn't exist in the destination</span></span>
<span data-ttu-id="447a0-255">A `/XO` és `/XN` paraméterek lehetővé teszik a régebbi vagy újabb forrás erőforrások másolását, illetve kizárása.</span><span class="sxs-lookup"><span data-stu-id="447a0-255">The `/XO` and `/XN` parameters allow you to exclude older or newer source resources from being copied, respectively.</span></span> <span data-ttu-id="447a0-256">Ha szeretné, amelyek nem léteznek a cél a forrás-erőforrások másolása, mindkét paraméter az AzCopy parancs adhat meg:</span><span class="sxs-lookup"><span data-stu-id="447a0-256">If you only want to copy source resources that don't exist in the destination, you can specify both parameters in the AzCopy command:</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

<span data-ttu-id="447a0-257">Vegye figyelembe, hogy ez nem támogatott, ha a cél- és egy tábla.</span><span class="sxs-lookup"><span data-stu-id="447a0-257">Note that this is not supported when either the source or destination is a table.</span></span>

### <a name="use-a-response-file-to-specify-command-line-parameters"></a><span data-ttu-id="447a0-258">Egy válaszfájl segítségével adja meg a parancssori paraméterek</span><span class="sxs-lookup"><span data-stu-id="447a0-258">Use a response file to specify command-line parameters</span></span>

```azcopy
AzCopy /@:"C:\responsefiles\copyoperation.txt"
```

<span data-ttu-id="447a0-259">AzCopy parancssori paramétereket is megadhat a egy válaszfájlt.</span><span class="sxs-lookup"><span data-stu-id="447a0-259">You can include any AzCopy command-line parameters in a response file.</span></span> <span data-ttu-id="447a0-260">AzCopy dolgozza fel a paraméterek a fájlban, ha a parancssorban megadott lett hajt végre a fájl tartalma közvetlen helyettesítés.</span><span class="sxs-lookup"><span data-stu-id="447a0-260">AzCopy processes the parameters in the file as if they had been specified on the command line, performing a direct substitution with the contents of the file.</span></span>

<span data-ttu-id="447a0-261">Egy válaszfájl nevű feltételezik `copyoperation.txt`, amely tartalmazza a következő sorokat.</span><span class="sxs-lookup"><span data-stu-id="447a0-261">Assume a response file named `copyoperation.txt`, that contains the following lines.</span></span> <span data-ttu-id="447a0-262">Minden egyes AzCopy paraméter adható meg egy sorba</span><span class="sxs-lookup"><span data-stu-id="447a0-262">Each AzCopy parameter can be specified on a single line</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

<span data-ttu-id="447a0-263">vagy külön sorok:</span><span class="sxs-lookup"><span data-stu-id="447a0-263">or on separate lines:</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

<span data-ttu-id="447a0-264">AzCopy sikertelen lesz, ha a paraméter osztani két sort, az itt látható a `/sourcekey` paraméter:</span><span class="sxs-lookup"><span data-stu-id="447a0-264">AzCopy will fail if you split the parameter across two lines, as shown here for the `/sourcekey` parameter:</span></span>

    http://myaccount.blob.core.windows.net/mycontainer
     C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-to-specify-command-line-parameters"></a><span data-ttu-id="447a0-265">Több fájl használható a válasz parancssori paraméterek megadása</span><span class="sxs-lookup"><span data-stu-id="447a0-265">Use multiple response files to specify command-line parameters</span></span>
<span data-ttu-id="447a0-266">Egy válaszfájl nevű feltételezik `source.txt` , amely megadja, hogy a forrás-tároló:</span><span class="sxs-lookup"><span data-stu-id="447a0-266">Assume a response file named `source.txt` that specifies a source container:</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer

<span data-ttu-id="447a0-267">És egy válaszfájlt nevű `dest.txt` egy célmappát, amely meghatározza a fájlrendszer:</span><span class="sxs-lookup"><span data-stu-id="447a0-267">And a response file named `dest.txt` that specifies a destination folder in the file system:</span></span>

    /Dest:C:\myfolder

<span data-ttu-id="447a0-268">És egy válaszfájlt nevű `options.txt` , amely megadja, hogy az AzCopy beállítások:</span><span class="sxs-lookup"><span data-stu-id="447a0-268">And a response file named `options.txt` that specifies options for AzCopy:</span></span>

    /S /Y

<span data-ttu-id="447a0-269">AzCopy hívni a válasz fájlok, amelyek találhatók a könyvtárban található `C:\responsefiles`, használja ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="447a0-269">To call AzCopy with these response files, all of which reside in a directory `C:\responsefiles`, use this command:</span></span>

```azcopy
AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   
```

<span data-ttu-id="447a0-270">AzCopy végrehajtja ezt a parancsot, ahogy bármilyen ha tartalmazza a minden egyes paraméter a parancssorban:</span><span class="sxs-lookup"><span data-stu-id="447a0-270">AzCopy processes this command just as it would if you included all of the individual parameters on the command line:</span></span>

```azcopy
AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y
```

### <a name="specify-a-shared-access-signature-sas"></a><span data-ttu-id="447a0-271">Adja meg a közös hozzáférésű jogosultságkód (SAS)</span><span class="sxs-lookup"><span data-stu-id="447a0-271">Specify a shared access signature (SAS)</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt
```

<span data-ttu-id="447a0-272">Azt is megadhatja egy SAS URI tárolón:</span><span class="sxs-lookup"><span data-stu-id="447a0-272">You can also specify a SAS on the container URI:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S
```

### <a name="journal-file-folder"></a><span data-ttu-id="447a0-273">Napló fájlmappa</span><span class="sxs-lookup"><span data-stu-id="447a0-273">Journal file folder</span></span>
<span data-ttu-id="447a0-274">Minden alkalommal, amikor egy parancs kiadni az AzCopy, ellenőrzi, hogy a napló fájl megtalálható-e az alapértelmezett mappába, vagy hogy keresztül ez a beállítás a megadott mappa létezik.</span><span class="sxs-lookup"><span data-stu-id="447a0-274">Each time you issue a command to AzCopy, it checks whether a journal file exists in the default folder, or whether it exists in a folder that you specified via this option.</span></span> <span data-ttu-id="447a0-275">A napló fájl nem létezik egyik helyen sem, ha az AzCopy kezeli a művelet új, és létrehoz egy új naplófájl.</span><span class="sxs-lookup"><span data-stu-id="447a0-275">If the journal file does not exist in either place, AzCopy treats the operation as new and generates a new journal file.</span></span>

<span data-ttu-id="447a0-276">Ha a napló fájl nem létezik, AzCopy ellenőrzi e megadott parancssort megegyezik-e a parancssorban a napló fájlban.</span><span class="sxs-lookup"><span data-stu-id="447a0-276">If the journal file does exist, AzCopy will check whether the command line that you input matches the command line in the journal file.</span></span> <span data-ttu-id="447a0-277">Ha a két parancssorokat egyeznek, AzCopy folytatja a teljes műveletet.</span><span class="sxs-lookup"><span data-stu-id="447a0-277">If the two command lines match, AzCopy resumes the incomplete operation.</span></span> <span data-ttu-id="447a0-278">Ha nem egyeznek, vagy felülírja napló új művelet indítása és az aktuális művelet megszakítására bekéri.</span><span class="sxs-lookup"><span data-stu-id="447a0-278">If they do not match, you will be prompted to either overwrite the journal file to start a new operation, or to cancel the current operation.</span></span>

<span data-ttu-id="447a0-279">Ha szeretné a naplófájl alapértelmezett helyet használja:</span><span class="sxs-lookup"><span data-stu-id="447a0-279">If you want to use the default location for the journal file:</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z
```

<span data-ttu-id="447a0-280">Ha a beállítás nincs megadva `/Z`, vagy adja meg a beállítás `/Z` anélkül, hogy a mappa elérési útja, a fent látható AzCopy fájlt hoz létre a napló az alapértelmezett helyen, amely `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="447a0-280">If you omit option `/Z`, or specify option `/Z` without the folder path, as shown above, AzCopy creates the journal file in the default location, which is `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span></span> <span data-ttu-id="447a0-281">Ha a napló fájl már létezik, majd AzCopy folytatja a műveletet, a napló-fájl alapján.</span><span class="sxs-lookup"><span data-stu-id="447a0-281">If the journal file already exists, then AzCopy resumes the operation based on the journal file.</span></span>

<span data-ttu-id="447a0-282">Ha szeretne egy egyéni napló elérési útját adja meg:</span><span class="sxs-lookup"><span data-stu-id="447a0-282">If you want to specify a custom location for the journal file:</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\
```

<span data-ttu-id="447a0-283">Ebben a példában a napló fájlt hoz létre, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="447a0-283">This example creates the journal file if it does not already exist.</span></span> <span data-ttu-id="447a0-284">Ha létezik, majd AzCopy folytatja a műveletet, a napló-fájl alapján.</span><span class="sxs-lookup"><span data-stu-id="447a0-284">If it does exist, then AzCopy resumes the operation based on the journal file.</span></span>

<span data-ttu-id="447a0-285">Ha folytatni szeretné az AzCopy műveletet:</span><span class="sxs-lookup"><span data-stu-id="447a0-285">If you want to resume an AzCopy operation:</span></span>

```azcopy
AzCopy /Z:C:\journalfolder\
```

<span data-ttu-id="447a0-286">Ebben a példában újraindítja a legutóbbi műveletet, és nem sikerült végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="447a0-286">This example resumes the last operation, which may have failed to complete.</span></span>

### <a name="generate-a-log-file"></a><span data-ttu-id="447a0-287">A naplófájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="447a0-287">Generate a log file</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V
```

<span data-ttu-id="447a0-288">Ha a beállítás megadja `/V` anélkül, hogy a fájl elérési útját a részletes naplózás, majd AzCopy hozza létre a naplófájlt, amely alapértelmezett helyen `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="447a0-288">If you specify option `/V` without providing a file path to the verbose log, then AzCopy creates the log file in the default location, which is `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span></span>

<span data-ttu-id="447a0-289">Ellenkező esetben egy naplófájlt is létrehozhat egy egyéni helyen:</span><span class="sxs-lookup"><span data-stu-id="447a0-289">Otherwise, you can create an log file in a custom location:</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log
```

<span data-ttu-id="447a0-290">Vegye figyelembe, hogy ha a beállítás a következő relatív elérési utat ad meg `/V`, például a `/V:test/azcopy1.log`, akkor a részletes napló létrehozása az aktuális munkakönyvtárban belül egy almappát `test`.</span><span class="sxs-lookup"><span data-stu-id="447a0-290">Note that if you specify a relative path following option `/V`, such as `/V:test/azcopy1.log`, then the verbose log is created in the current working directory within a subfolder named `test`.</span></span>

### <a name="specify-the-number-of-concurrent-operations-to-start"></a><span data-ttu-id="447a0-291">Adja meg elindítani a párhuzamos műveletek száma</span><span class="sxs-lookup"><span data-stu-id="447a0-291">Specify the number of concurrent operations to start</span></span>
<span data-ttu-id="447a0-292">A beállítás `/NC` egyidejű másolási műveletek számát adja meg.</span><span class="sxs-lookup"><span data-stu-id="447a0-292">Option `/NC` specifies the number of concurrent copy operations.</span></span> <span data-ttu-id="447a0-293">Alapértelmezés szerint az AzCopy bizonyos száma párhuzamos műveletek az adatok átvitel átviteli sebesség növelése elindul.</span><span class="sxs-lookup"><span data-stu-id="447a0-293">By default, AzCopy starts a certain number of concurrent operations to increase the data transfer throughput.</span></span> <span data-ttu-id="447a0-294">A tábla műveleteket párhuzamos műveletek száma megegyezik rendelkezik processzorok száma.</span><span class="sxs-lookup"><span data-stu-id="447a0-294">For Table operations, the number of concurrent operations is equal to the number of processors you have.</span></span> <span data-ttu-id="447a0-295">A Blob és a fájl műveleteket, párhuzamos műveletek számát 8 alkalommal az a szám egyezzen processzorral rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="447a0-295">For Blob and File operations, the number of concurrent operations is equal 8 times the number of processors you have.</span></span> <span data-ttu-id="447a0-296">AzCopy kis sávszélességű hálózaton keresztül futtatja, ha kevesebb /NC hibáját okozta. erőforrás konkurencia elkerülése érdekében a is megadhat.</span><span class="sxs-lookup"><span data-stu-id="447a0-296">If you are running AzCopy across a low-bandwidth network, you can specify a lower number for /NC to avoid failure caused by resource competition.</span></span>

### <a name="run-azcopy-against-azure-storage-emulator"></a><span data-ttu-id="447a0-297">AzCopy futtassa az Azure Storage emulatorban</span><span class="sxs-lookup"><span data-stu-id="447a0-297">Run AzCopy against Azure Storage Emulator</span></span>
<span data-ttu-id="447a0-298">AzCopy ellen is futtathatja a [Azure Storage Emulator](storage-use-emulator.md) a Blobok:</span><span class="sxs-lookup"><span data-stu-id="447a0-298">You can run AzCopy against the [Azure Storage Emulator](storage-use-emulator.md) for Blobs:</span></span>

```azcopy
AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S
```

<span data-ttu-id="447a0-299">és táblák:</span><span class="sxs-lookup"><span data-stu-id="447a0-299">and Tables:</span></span>

```azcopy
AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table
```

## <a name="azcopy-parameters"></a><span data-ttu-id="447a0-300">AzCopy paraméterek</span><span class="sxs-lookup"><span data-stu-id="447a0-300">AzCopy Parameters</span></span>
<span data-ttu-id="447a0-301">AzCopy paramétereinek az alábbiakban található.</span><span class="sxs-lookup"><span data-stu-id="447a0-301">Parameters for AzCopy are described below.</span></span> <span data-ttu-id="447a0-302">Is beírhatja a Súgó a parancssorból a következő parancsok egyikét az AzCopy segítségével:</span><span class="sxs-lookup"><span data-stu-id="447a0-302">You can also type one of the following commands from the command line for help in using AzCopy:</span></span>

* <span data-ttu-id="447a0-303">AzCopy részletes parancssori segítséget:`AzCopy /?`</span><span class="sxs-lookup"><span data-stu-id="447a0-303">For detailed command-line help for AzCopy: `AzCopy /?`</span></span>
* <span data-ttu-id="447a0-304">További információt az AzCopy paramétert:`AzCopy /?:SourceKey`</span><span class="sxs-lookup"><span data-stu-id="447a0-304">For detailed help with any AzCopy parameter: `AzCopy /?:SourceKey`</span></span>
* <span data-ttu-id="447a0-305">A parancssori példák:`AzCopy /?:Samples`</span><span class="sxs-lookup"><span data-stu-id="447a0-305">For command-line examples: `AzCopy /?:Samples`</span></span>

### <a name="sourcesource"></a><span data-ttu-id="447a0-306">/ Source: "forrás"</span><span class="sxs-lookup"><span data-stu-id="447a0-306">/Source:"source"</span></span>
<span data-ttu-id="447a0-307">Megadja a forrás adatait, amelynek be kell másolni.</span><span class="sxs-lookup"><span data-stu-id="447a0-307">Specifies the source data from which to copy.</span></span> <span data-ttu-id="447a0-308">A forrás lehet egy fájl rendszer könyvtár, egy blob-tároló, blob virtuális könyvtár, egy tárolófájl-megosztás, tároló fájl könyvtár, vagy egy Azure-tábla.</span><span class="sxs-lookup"><span data-stu-id="447a0-308">The source can be a file system directory, a blob container, a blob virtual directory, a storage file share, a storage file directory, or an Azure table.</span></span>

<span data-ttu-id="447a0-309">**Alkalmazandó:** blobokat, fájlok, táblák</span><span class="sxs-lookup"><span data-stu-id="447a0-309">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="destdestination"></a><span data-ttu-id="447a0-310">/ Cél: "cél"</span><span class="sxs-lookup"><span data-stu-id="447a0-310">/Dest:"destination"</span></span>
<span data-ttu-id="447a0-311">Megadja azt a helyet, másolja.</span><span class="sxs-lookup"><span data-stu-id="447a0-311">Specifies the destination to copy to.</span></span> <span data-ttu-id="447a0-312">A cél lehet fájl rendszer könyvtár, egy blob-tároló, egy blob virtuális könyvtár, egy tárolói fájlmegosztást, egy tárolási könyvtárának vagy egy Azure-tábla.</span><span class="sxs-lookup"><span data-stu-id="447a0-312">The destination can be a file system directory, a blob container, a blob virtual directory, a storage file share, a storage file directory, or an Azure table.</span></span>

<span data-ttu-id="447a0-313">**Alkalmazandó:** blobokat, fájlok, táblák</span><span class="sxs-lookup"><span data-stu-id="447a0-313">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="patternfile-pattern"></a><span data-ttu-id="447a0-314">/ Mintát: "fájl-minta"</span><span class="sxs-lookup"><span data-stu-id="447a0-314">/Pattern:"file-pattern"</span></span>
<span data-ttu-id="447a0-315">Adja meg a fájl mintát, amely azt jelzi, mely fájlokat másolni.</span><span class="sxs-lookup"><span data-stu-id="447a0-315">Specifies a file pattern that indicates which files to copy.</span></span> <span data-ttu-id="447a0-316">A /Pattern paraméter viselkedését az adatok helye és a rekurzív mód beállítása jelenléte határozza meg.</span><span class="sxs-lookup"><span data-stu-id="447a0-316">The behavior of the /Pattern parameter is determined by the location of the source data, and the presence of the recursive mode option.</span></span> <span data-ttu-id="447a0-317">Rekurzív mód keresztül /s beállítás van megadva.</span><span class="sxs-lookup"><span data-stu-id="447a0-317">Recursive mode is specified via option /S.</span></span>

<span data-ttu-id="447a0-318">Ha a megadott forrás egy könyvtárat a fájlrendszer, majd szabványos helyettesítő karakterek érvényes, és a megadott fájl minta egyezik a könyvtárban lévő fájlokra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="447a0-318">If the specified source is a directory in the file system, then standard wildcards are in effect, and the file pattern provided is matched against files within the directory.</span></span> <span data-ttu-id="447a0-319">Ha beállítása/s van megadva, majd AzCopy is megfelel a megadott minta elleni almappákban alatt a könyvtárban található összes fájl.</span><span class="sxs-lookup"><span data-stu-id="447a0-319">If option /S is specified, then AzCopy also matches the specified pattern against all files in any subfolders beneath the directory.</span></span>

<span data-ttu-id="447a0-320">Ha a megadott forrás egy blob-tároló vagy virtuális könyvtárat, majd helyettesítő karakterek nem érvényesek.</span><span class="sxs-lookup"><span data-stu-id="447a0-320">If the specified source is a blob container or virtual directory, then wildcards are not applied.</span></span> <span data-ttu-id="447a0-321">Ha a beállítás /s kapcsoló meg van adva, majd AzCopy a megadott fájl mintát blob előtagjaként értelmezi.</span><span class="sxs-lookup"><span data-stu-id="447a0-321">If option /S is specified, then AzCopy interprets the specified file pattern as a blob prefix.</span></span> <span data-ttu-id="447a0-322">Ha a beállítás /s kapcsoló nincs megadva, majd AzCopy pontos blob nevekkel fájlminta megegyezik.</span><span class="sxs-lookup"><span data-stu-id="447a0-322">If option /S is not specified, then AzCopy matches the file pattern against exact blob names.</span></span>

<span data-ttu-id="447a0-323">Ha a megadott forrás az Azure fájlmegosztások, akkor állítsa adja meg a pontos fájl (pl. abc.txt) egyetlen fájlba másolni, vagy adja meg a beállítás a a megosztás rekurzív módon másolja az összes fájlt a/s.</span><span class="sxs-lookup"><span data-stu-id="447a0-323">If the specified source is an Azure file share, then you must either specify the exact file name, (e.g. abc.txt) to copy a single file, or specify option /S to copy all files in the share recursively.</span></span> <span data-ttu-id="447a0-324">Adjon meg egy fájl mintát és a beállítás próbál /S együtt jár hiba.</span><span class="sxs-lookup"><span data-stu-id="447a0-324">Attempting to specify both a file pattern and option /S together will result in an error.</span></span>

<span data-ttu-id="447a0-325">AzCopy használja a kis-és nagybetűket megfelelő, ha a/Source a blob-tároló vagy a blob virtuális könyvtár, és használja, minden más esetben azonban nem megfelelő.</span><span class="sxs-lookup"><span data-stu-id="447a0-325">AzCopy uses case-sensitive matching when the /Source is a blob container or blob virtual directory, and uses case-insensitive matching in all the other cases.</span></span>

<span data-ttu-id="447a0-326">Az alapértelmezett fájl minta használható, ha nincs fájl mintát *.*</span><span class="sxs-lookup"><span data-stu-id="447a0-326">The default file pattern used when no file pattern is specified is *.*</span></span> <span data-ttu-id="447a0-327">egy olyan fájlhelyre rendszer vagy egy Azure Storage helyének egy üres előtag.</span><span class="sxs-lookup"><span data-stu-id="447a0-327">for a file system location or an empty prefix for an Azure Storage location.</span></span> <span data-ttu-id="447a0-328">Több fájl minták megadása nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="447a0-328">Specifying multiple file patterns is not supported.</span></span>

<span data-ttu-id="447a0-329">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-329">**Applicable to:** Blobs, Files</span></span>

### <a name="destkeystorage-key"></a><span data-ttu-id="447a0-330">/ DestKey: "storage-kulcsot"</span><span class="sxs-lookup"><span data-stu-id="447a0-330">/DestKey:"storage-key"</span></span>
<span data-ttu-id="447a0-331">Adja meg a tárfiók hívóbetűjét a cél az erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="447a0-331">Specifies the storage account key for the destination resource.</span></span>

<span data-ttu-id="447a0-332">**Alkalmazandó:** blobokat, fájlok, táblák</span><span class="sxs-lookup"><span data-stu-id="447a0-332">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="destsassas-token"></a><span data-ttu-id="447a0-333">/ DestSAS: "sas-token"</span><span class="sxs-lookup"><span data-stu-id="447a0-333">/DestSAS:"sas-token"</span></span>
<span data-ttu-id="447a0-334">OLVASÁSI és írási jogosultsággal a cél egy közös hozzáférésű Jogosultságkód (SAS) határozza meg, (ha van ilyen).</span><span class="sxs-lookup"><span data-stu-id="447a0-334">Specifies a Shared Access Signature (SAS) with READ and WRITE permissions for the destination (if applicable).</span></span> <span data-ttu-id="447a0-335">Helyezze a dupla idézőjelek között, SAS tartalmaz, így előfordulhat, hogy speciális parancssori karaktereket.</span><span class="sxs-lookup"><span data-stu-id="447a0-335">Surround the SAS with double quotes, as it may contains special command-line characters.</span></span>

<span data-ttu-id="447a0-336">Ha a célként megadott erőforrás a blob-tároló, a fájlmegosztás vagy a táblát, vagy adja meg ezt a beállítást, a SAS-jogkivonat követ, vagy a cél blob tároló, a fájlmegosztás vagy a tábla URI, anélkül, hogy ez a beállítás részeként is megadhat az SA-kat.</span><span class="sxs-lookup"><span data-stu-id="447a0-336">If the destination resource is a blob container, file share or table, you can either specify this option followed by the SAS token, or you can specify the SAS as part of the destination blob container, file share or table's URI, without this option.</span></span>

<span data-ttu-id="447a0-337">Ha a forrás és cél mindkét blobokat, majd a cél blob, a forrás blob tárfiókon belül kell lennie.</span><span class="sxs-lookup"><span data-stu-id="447a0-337">If the source and destination are both blobs, then the destination blob must reside within the same storage account as the source blob.</span></span>

<span data-ttu-id="447a0-338">**Alkalmazandó:** blobokat, fájlok, táblák</span><span class="sxs-lookup"><span data-stu-id="447a0-338">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="sourcekeystorage-key"></a><span data-ttu-id="447a0-339">/ SourceKey: "storage-kulcsot"</span><span class="sxs-lookup"><span data-stu-id="447a0-339">/SourceKey:"storage-key"</span></span>
<span data-ttu-id="447a0-340">Adja meg a tárfiók hívóbetűjét a forrás-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="447a0-340">Specifies the storage account key for the source resource.</span></span>

<span data-ttu-id="447a0-341">**Alkalmazandó:** blobokat, fájlok, táblák</span><span class="sxs-lookup"><span data-stu-id="447a0-341">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="sourcesassas-token"></a><span data-ttu-id="447a0-342">/ SourceSAS: "sas-token"</span><span class="sxs-lookup"><span data-stu-id="447a0-342">/SourceSAS:"sas-token"</span></span>
<span data-ttu-id="447a0-343">Adja meg a közös hozzáférésű Jogosultságkód OLVASÁSI és lista engedélyekkel a forrás (ha van ilyen).</span><span class="sxs-lookup"><span data-stu-id="447a0-343">Specifies a Shared Access Signature with READ and LIST permissions for the source (if applicable).</span></span> <span data-ttu-id="447a0-344">Helyezze a dupla idézőjelek között, SAS tartalmaz, így előfordulhat, hogy speciális parancssori karaktereket.</span><span class="sxs-lookup"><span data-stu-id="447a0-344">Surround the SAS with double quotes, as it may contains special command-line characters.</span></span>

<span data-ttu-id="447a0-345">Ha a forrás erőforrás egy blob-tároló, és nem egy kulcs, és SAS-kód nem áll, majd a blob-tároló fogja beolvasni a névtelen hozzáférés keresztül.</span><span class="sxs-lookup"><span data-stu-id="447a0-345">If the source resource is a blob container, and neither a key nor a SAS is provided, then the blob container will be read via anonymous access.</span></span>

<span data-ttu-id="447a0-346">Ha a forrás egy fájlmegosztás vagy a tábla, meg kell adni egy kulcs- vagy SAS-kód.</span><span class="sxs-lookup"><span data-stu-id="447a0-346">If the source is a file share or table, a key or a SAS must be provided.</span></span>

<span data-ttu-id="447a0-347">**Alkalmazandó:** blobokat, fájlok, táblák</span><span class="sxs-lookup"><span data-stu-id="447a0-347">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="s"></a><span data-ttu-id="447a0-348">/ S</span><span class="sxs-lookup"><span data-stu-id="447a0-348">/S</span></span>
<span data-ttu-id="447a0-349">Meghatározza a másolási műveletek rekurzív módját.</span><span class="sxs-lookup"><span data-stu-id="447a0-349">Specifies recursive mode for copy operations.</span></span> <span data-ttu-id="447a0-350">Rekurzív módban AzCopy blobokkal vagy a fájlokat, amelyek megfelelnek a megadott fájl mintát, beleértve az almappákat másolja.</span><span class="sxs-lookup"><span data-stu-id="447a0-350">In recursive mode, AzCopy will copy all blobs or files that match the specified file pattern, including those in subfolders.</span></span>

<span data-ttu-id="447a0-351">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-351">**Applicable to:** Blobs, Files</span></span>

### <a name="blobtypeblock--page--append"></a><span data-ttu-id="447a0-352">/ BlobType: "block" |} "lap" |} "append"</span><span class="sxs-lookup"><span data-stu-id="447a0-352">/BlobType:"block" | "page" | "append"</span></span>
<span data-ttu-id="447a0-353">Megadja, hogy a cél blob egy blokkblob, oldalakra vonatkozó blob vagy hozzáfűző blob.</span><span class="sxs-lookup"><span data-stu-id="447a0-353">Specifies whether the destination blob is a block blob, a page blob, or an append blob.</span></span> <span data-ttu-id="447a0-354">Ez a beállítás csak egy blob feltölteni kívánt esetén alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="447a0-354">This option is applicable only when you are uploading a blob.</span></span> <span data-ttu-id="447a0-355">Ellenkező esetben hiba történik.</span><span class="sxs-lookup"><span data-stu-id="447a0-355">Otherwise, an error is generated.</span></span> <span data-ttu-id="447a0-356">Ha a cél egy blobot, és ez a beállítás nincs megadva, alapértelmezés szerint az AzCopy egy blokkblob hoz létre.</span><span class="sxs-lookup"><span data-stu-id="447a0-356">If the destination is a blob and this option is not specified, by default, AzCopy creates a block blob.</span></span>

<span data-ttu-id="447a0-357">**Alkalmazandó:** Blobok</span><span class="sxs-lookup"><span data-stu-id="447a0-357">**Applicable to:** Blobs</span></span>

### <a name="checkmd5"></a><span data-ttu-id="447a0-358">/ CheckMD5</span><span class="sxs-lookup"><span data-stu-id="447a0-358">/CheckMD5</span></span>
<span data-ttu-id="447a0-359">Kiszámítja az MD5 kivonatoló letöltött adatok, és ellenőrzi, hogy a blob tárolja az MD5 kivonatoló vagy a fájl Content-MD5 tulajdonsága egyezést mutat a kiszámított kivonatát.</span><span class="sxs-lookup"><span data-stu-id="447a0-359">Calculates an MD5 hash for downloaded data and verifies that the MD5 hash stored in the blob or file's Content-MD5 property matches the calculated hash.</span></span> <span data-ttu-id="447a0-360">Az MD5-ellenőrzése ki van kapcsolva, alapértelmezés szerint, meg kell adnia ezt a beállítást, végezze el az MD5 ellenőrzését, amikor az adatok letöltése.</span><span class="sxs-lookup"><span data-stu-id="447a0-360">The MD5 check is turned off by default, so you must specify this option to perform the MD5 check when downloading data.</span></span>

<span data-ttu-id="447a0-361">Figyelje meg, hogy az Azure Storage nem garantálja, hogy naprakész állapotban-e az MD5 kivonatoló a blob vagy a fájl tárolja.</span><span class="sxs-lookup"><span data-stu-id="447a0-361">Note that Azure Storage doesn't guarantee that the MD5 hash stored for the blob or file is up-to-date.</span></span> <span data-ttu-id="447a0-362">Feladata ügyfél frissítése az MD5, amikor a blob vagy a fájl módosítása.</span><span class="sxs-lookup"><span data-stu-id="447a0-362">It is client's responsibility to update the MD5 whenever the blob or file is modified.</span></span>

<span data-ttu-id="447a0-363">AzCopy a Content-MD5 tulajdonság az Azure blob vagy a fájl mindig beállítása után ismét feltölteni a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="447a0-363">AzCopy always sets the Content-MD5 property for an Azure blob or file after uploading it to the service.</span></span>  

<span data-ttu-id="447a0-364">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-364">**Applicable to:** Blobs, Files</span></span>

### <a name="snapshot"></a><span data-ttu-id="447a0-365">Vagy pillanatkép</span><span class="sxs-lookup"><span data-stu-id="447a0-365">/Snapshot</span></span>
<span data-ttu-id="447a0-366">Azt jelzi, hogy pillanatképet továbbít.</span><span class="sxs-lookup"><span data-stu-id="447a0-366">Indicates whether to transfer snapshots.</span></span> <span data-ttu-id="447a0-367">Ez a beállítás csak akkor érvényes, ha a forrás, a blob.</span><span class="sxs-lookup"><span data-stu-id="447a0-367">This option is only valid when the source is a blob.</span></span>

<span data-ttu-id="447a0-368">Az átvitt blob pillanatképek átnevezi a következő formátumban: .extension blob-név (pillanatkép-time)</span><span class="sxs-lookup"><span data-stu-id="447a0-368">The transferred blob snapshots are renamed in this format: blob-name (snapshot-time).extension</span></span>

<span data-ttu-id="447a0-369">Alapértelmezés szerint a pillanatképek nem kerülnek.</span><span class="sxs-lookup"><span data-stu-id="447a0-369">By default, snapshots are not copied.</span></span>

<span data-ttu-id="447a0-370">**Alkalmazandó:** Blobok</span><span class="sxs-lookup"><span data-stu-id="447a0-370">**Applicable to:** Blobs</span></span>

### <a name="vverbose-log-file"></a><span data-ttu-id="447a0-371">/ V: [részletes-naplófájl]</span><span class="sxs-lookup"><span data-stu-id="447a0-371">/V:[verbose-log-file]</span></span>
<span data-ttu-id="447a0-372">Kimeneti részletes üzenetek fájlba.</span><span class="sxs-lookup"><span data-stu-id="447a0-372">Outputs verbose status messages into a log file.</span></span>

<span data-ttu-id="447a0-373">Alapértelmezés szerint a részletes naplófájl neve a AzCopyVerbose.log `%LocalAppData%\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="447a0-373">By default, the verbose log file is named AzCopyVerbose.log in `%LocalAppData%\Microsoft\Azure\AzCopy`.</span></span> <span data-ttu-id="447a0-374">Ha megadja ezt a lehetőséget egy meglévő fájl helyét, a részletes naplózás, amely a fájl lesz hozzáfűzve.</span><span class="sxs-lookup"><span data-stu-id="447a0-374">If you specify an existing file location for this option, the verbose log will be appended to that file.</span></span>  

<span data-ttu-id="447a0-375">**Alkalmazandó:** blobokat, fájlok, táblák</span><span class="sxs-lookup"><span data-stu-id="447a0-375">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="zjournal-file-folder"></a><span data-ttu-id="447a0-376">/ Z: [napló-fájlok és mappák]</span><span class="sxs-lookup"><span data-stu-id="447a0-376">/Z:[journal-file-folder]</span></span>
<span data-ttu-id="447a0-377">A művelet folytatása napló fájl mappáját adja meg.</span><span class="sxs-lookup"><span data-stu-id="447a0-377">Specifies a journal file folder for resuming an operation.</span></span>

<span data-ttu-id="447a0-378">AzCopy mindig támogatja a Folytatás, ha egy művelet megszakadt.</span><span class="sxs-lookup"><span data-stu-id="447a0-378">AzCopy always supports resuming if an operation has been interrupted.</span></span>

<span data-ttu-id="447a0-379">Ha ez a beállítás nincs megadva, vagy egy mappa elérési útját nélkül van megadva, majd AzCopy hozza létre az alapértelmezett helyen, amely % LocalAppData%\Microsoft\Azure\AzCopy a napló fájlt.</span><span class="sxs-lookup"><span data-stu-id="447a0-379">If this option is not specified, or it is specified without a folder path, then AzCopy will create the journal file in the default location, which is %LocalAppData%\Microsoft\Azure\AzCopy.</span></span>

<span data-ttu-id="447a0-380">Minden alkalommal, amikor egy parancs kiadni az AzCopy, ellenőrzi, hogy a napló fájl megtalálható-e az alapértelmezett mappába, vagy hogy keresztül ez a beállítás a megadott mappa létezik.</span><span class="sxs-lookup"><span data-stu-id="447a0-380">Each time you issue a command to AzCopy, it checks whether a journal file exists in the default folder, or whether it exists in a folder that you specified via this option.</span></span> <span data-ttu-id="447a0-381">A napló fájl nem létezik egyik helyen sem, ha az AzCopy kezeli a művelet új, és létrehoz egy új naplófájl.</span><span class="sxs-lookup"><span data-stu-id="447a0-381">If the journal file does not exist in either place, AzCopy treats the operation as new and generates a new journal file.</span></span>

<span data-ttu-id="447a0-382">Ha a napló fájl nem létezik, AzCopy ellenőrzi e megadott parancssort megegyezik-e a parancssorban a napló fájlban.</span><span class="sxs-lookup"><span data-stu-id="447a0-382">If the journal file does exist, AzCopy will check whether the command line that you input matches the command line in the journal file.</span></span> <span data-ttu-id="447a0-383">Ha a két parancssorokat egyeznek, AzCopy folytatja a teljes műveletet.</span><span class="sxs-lookup"><span data-stu-id="447a0-383">If the two command lines match, AzCopy resumes the incomplete operation.</span></span> <span data-ttu-id="447a0-384">Ha nem egyeznek, vagy felülírja napló új művelet indítása és az aktuális művelet megszakítására bekéri.</span><span class="sxs-lookup"><span data-stu-id="447a0-384">If they do not match, you will be prompted to either overwrite the journal file to start a new operation, or to cancel the current operation.</span></span>

<span data-ttu-id="447a0-385">A napló fájl törlődik a művelet sikeres befejezését követően.</span><span class="sxs-lookup"><span data-stu-id="447a0-385">The journal file is deleted upon successful completion of the operation.</span></span>

<span data-ttu-id="447a0-386">Vegye figyelembe, hogy a Folytatás, az AzCopy egy korábbi verziójával létrehozott napló fájlból egy művelet nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="447a0-386">Note that resuming an operation from a journal file created by a previous version of AzCopy is not supported.</span></span>

<span data-ttu-id="447a0-387">**Alkalmazandó:** blobokat, fájlok, táblák</span><span class="sxs-lookup"><span data-stu-id="447a0-387">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="parameter-file"></a><span data-ttu-id="447a0-388">/@:"Parameter-File"</span><span class="sxs-lookup"><span data-stu-id="447a0-388">/@:"parameter-file"</span></span>
<span data-ttu-id="447a0-389">Megadja a paramétereket tartalmazó fájlt.</span><span class="sxs-lookup"><span data-stu-id="447a0-389">Specifies a file that contains parameters.</span></span> <span data-ttu-id="447a0-390">AzCopy dolgozza fel a paramétereket a fájlt a, mintha csak a parancssorban megadott lett.</span><span class="sxs-lookup"><span data-stu-id="447a0-390">AzCopy processes the parameters in the file just as if they had been specified on the command line.</span></span>

<span data-ttu-id="447a0-391">A válaszfájlban több paramétert meg egyetlen vonal, vagy adja meg az egyes paramétereket külön sorban tüntettük.</span><span class="sxs-lookup"><span data-stu-id="447a0-391">In a response file, you can either specify multiple parameters on a single line, or specify each parameter on its own line.</span></span> <span data-ttu-id="447a0-392">Vegye figyelembe, hogy az egyes paraméter nem terjedhetnek többsoros.</span><span class="sxs-lookup"><span data-stu-id="447a0-392">Note that an individual parameter cannot span multiple lines.</span></span>

<span data-ttu-id="447a0-393">Válasz tartalmazhatnak a # karakterrel kezdődő sorok megjegyzések.</span><span class="sxs-lookup"><span data-stu-id="447a0-393">Response files can include comments lines that begin with the # symbol.</span></span>

<span data-ttu-id="447a0-394">Válasz több fájl is megadható.</span><span class="sxs-lookup"><span data-stu-id="447a0-394">You can specify multiple response files.</span></span> <span data-ttu-id="447a0-395">Vegye figyelembe azonban, hogy az AzCopy nem támogatja a beágyazott válaszfájlok.</span><span class="sxs-lookup"><span data-stu-id="447a0-395">However, note that AzCopy does not support nested response files.</span></span>

<span data-ttu-id="447a0-396">**Alkalmazandó:** blobokat, fájlok, táblák</span><span class="sxs-lookup"><span data-stu-id="447a0-396">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="y"></a><span data-ttu-id="447a0-397">/Y</span><span class="sxs-lookup"><span data-stu-id="447a0-397">/Y</span></span>
<span data-ttu-id="447a0-398">Letiltja az összes AzCopy megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="447a0-398">Suppresses all AzCopy confirmation prompts.</span></span>

<span data-ttu-id="447a0-399">**Alkalmazandó:** blobokat, fájlok, táblák</span><span class="sxs-lookup"><span data-stu-id="447a0-399">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="l"></a><span data-ttu-id="447a0-400">/ L</span><span class="sxs-lookup"><span data-stu-id="447a0-400">/L</span></span>
<span data-ttu-id="447a0-401">Megadja a listázási művelet csak; adatot nem másolódik.</span><span class="sxs-lookup"><span data-stu-id="447a0-401">Specifies a listing operation only; no data is copied.</span></span>

<span data-ttu-id="447a0-402">AzCopy értelmezi a használatával, a beállítás értéke a szimuláció, e nélkül a parancssor futtatása a/l és száma, hogy hány objektumok kerülnek beállításnál megadhatja, hogy mely objektumok kereséséhez egyszerre /V kerülnek a részletes napló beállítás.</span><span class="sxs-lookup"><span data-stu-id="447a0-402">AzCopy will interpret the using of this option as a simulation for running the command line without this option /L and count how many objects will be copied, you can specify option /V at the same time to check which objects will be copied in the verbose log.</span></span>

<span data-ttu-id="447a0-403">Ez a beállítás viselkedése is határozza meg az adatok helye és a rekurzív mód beállítás/s és a fájl minta beállítás /Pattern jelenlétét.</span><span class="sxs-lookup"><span data-stu-id="447a0-403">The behavior of this option is also determined by the location of the source data and the presence of the recursive mode option /S and file pattern option /Pattern.</span></span>

<span data-ttu-id="447a0-404">AzCopy, ez a hely LISTÁJÁT, és OLVASÁSI engedélyre van szüksége, ez a beállítás használata esetén.</span><span class="sxs-lookup"><span data-stu-id="447a0-404">AzCopy requires LIST and READ permission of this source location when using this option.</span></span>

<span data-ttu-id="447a0-405">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-405">**Applicable to:** Blobs, Files</span></span>

### <a name="mt"></a><span data-ttu-id="447a0-406">/MT</span><span class="sxs-lookup"><span data-stu-id="447a0-406">/MT</span></span>
<span data-ttu-id="447a0-407">Beállítja a letöltött fájl utolsó módosításának ideje azonosnak kell lennie a forrás blob vagy a fájl.</span><span class="sxs-lookup"><span data-stu-id="447a0-407">Sets the downloaded file's last-modified time to be the same as the source blob or file's.</span></span>

<span data-ttu-id="447a0-408">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-408">**Applicable to:** Blobs, Files</span></span>

### <a name="xn"></a><span data-ttu-id="447a0-409">/XN</span><span class="sxs-lookup"><span data-stu-id="447a0-409">/XN</span></span>
<span data-ttu-id="447a0-410">Nem tartalmazza egy újabb forráserőforrásnak.</span><span class="sxs-lookup"><span data-stu-id="447a0-410">Excludes a newer source resource.</span></span> <span data-ttu-id="447a0-411">Az erőforrás nem kerülnek, ha a forrás utolsó módosítási időpontjának azonos vagy újabb, mint a cél.</span><span class="sxs-lookup"><span data-stu-id="447a0-411">The resource will not be copied if the last modified time of the source is the same or newer than destination.</span></span>

<span data-ttu-id="447a0-412">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-412">**Applicable to:** Blobs, Files</span></span>

### <a name="xo"></a><span data-ttu-id="447a0-413">/XO</span><span class="sxs-lookup"><span data-stu-id="447a0-413">/XO</span></span>
<span data-ttu-id="447a0-414">Nem tartalmazza egy régebbi forráserőforrásnak.</span><span class="sxs-lookup"><span data-stu-id="447a0-414">Excludes an older source resource.</span></span> <span data-ttu-id="447a0-415">Az erőforrás nem kerülnek, ha a forrás utolsó módosítási időpontjának azonos vagy régebbi, mint a cél.</span><span class="sxs-lookup"><span data-stu-id="447a0-415">The resource will not be copied if the last modified time of the source is the same or older than destination.</span></span>

<span data-ttu-id="447a0-416">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-416">**Applicable to:** Blobs, Files</span></span>

### <a name="a"></a><span data-ttu-id="447a0-417">/A</span><span class="sxs-lookup"><span data-stu-id="447a0-417">/A</span></span>
<span data-ttu-id="447a0-418">Csak a Archiválandó fájlok feltöltését.</span><span class="sxs-lookup"><span data-stu-id="447a0-418">Uploads only files that have the Archive attribute set.</span></span>

<span data-ttu-id="447a0-419">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-419">**Applicable to:** Blobs, Files</span></span>

### <a name="iarashcnetoi"></a><span data-ttu-id="447a0-420">/ IA: [RASHCNETOI]</span><span class="sxs-lookup"><span data-stu-id="447a0-420">/IA:[RASHCNETOI]</span></span>
<span data-ttu-id="447a0-421">Csak a megadott attribútumok közül bármelyik rendelkező fájlok feltöltését.</span><span class="sxs-lookup"><span data-stu-id="447a0-421">Uploads only files that have any of the specified attributes set.</span></span>

<span data-ttu-id="447a0-422">A rendelkezésre álló attribútumok a következők:</span><span class="sxs-lookup"><span data-stu-id="447a0-422">Available attributes include:</span></span>

* <span data-ttu-id="447a0-423">R = csak olvasható fájlokat</span><span class="sxs-lookup"><span data-stu-id="447a0-423">R = Read-only files</span></span>
* <span data-ttu-id="447a0-424">A = archiválásra kész</span><span class="sxs-lookup"><span data-stu-id="447a0-424">A = Files ready for archiving</span></span>
* <span data-ttu-id="447a0-425">S rendszerfájlok =</span><span class="sxs-lookup"><span data-stu-id="447a0-425">S = System files</span></span>
* <span data-ttu-id="447a0-426">H = Rejtett fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-426">H = Hidden files</span></span>
* <span data-ttu-id="447a0-427">C = tömörített fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-427">C = Compressed files</span></span>
* <span data-ttu-id="447a0-428">N = normál fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-428">N = Normal files</span></span>
* <span data-ttu-id="447a0-429">E titkosított fájlok =</span><span class="sxs-lookup"><span data-stu-id="447a0-429">E = Encrypted files</span></span>
* <span data-ttu-id="447a0-430">T ideiglenes fájlok =</span><span class="sxs-lookup"><span data-stu-id="447a0-430">T = Temporary files</span></span>
* <span data-ttu-id="447a0-431">O = Offline fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-431">O = Offline files</span></span>
* <span data-ttu-id="447a0-432">I = nem indexelt fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-432">I = Non-indexed files</span></span>

<span data-ttu-id="447a0-433">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-433">**Applicable to:** Blobs, Files</span></span>

### <a name="xarashcnetoi"></a><span data-ttu-id="447a0-434">/ XA: [RASHCNETOI]</span><span class="sxs-lookup"><span data-stu-id="447a0-434">/XA:[RASHCNETOI]</span></span>
<span data-ttu-id="447a0-435">Olyan fájlra, amely a megadott attribútumok közül bármelyik nem tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="447a0-435">Excludes files that have any of the specified attributes set.</span></span>

<span data-ttu-id="447a0-436">A rendelkezésre álló attribútumok a következők:</span><span class="sxs-lookup"><span data-stu-id="447a0-436">Available attributes include:</span></span>

* <span data-ttu-id="447a0-437">R = csak olvasható fájlokat</span><span class="sxs-lookup"><span data-stu-id="447a0-437">R = Read-only files</span></span>
* <span data-ttu-id="447a0-438">A = archiválásra kész</span><span class="sxs-lookup"><span data-stu-id="447a0-438">A = Files ready for archiving</span></span>
* <span data-ttu-id="447a0-439">S rendszerfájlok =</span><span class="sxs-lookup"><span data-stu-id="447a0-439">S = System files</span></span>
* <span data-ttu-id="447a0-440">H = Rejtett fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-440">H = Hidden files</span></span>
* <span data-ttu-id="447a0-441">C = tömörített fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-441">C = Compressed files</span></span>
* <span data-ttu-id="447a0-442">N = normál fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-442">N = Normal files</span></span>
* <span data-ttu-id="447a0-443">E titkosított fájlok =</span><span class="sxs-lookup"><span data-stu-id="447a0-443">E = Encrypted files</span></span>
* <span data-ttu-id="447a0-444">T ideiglenes fájlok =</span><span class="sxs-lookup"><span data-stu-id="447a0-444">T = Temporary files</span></span>
* <span data-ttu-id="447a0-445">O = Offline fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-445">O = Offline files</span></span>
* <span data-ttu-id="447a0-446">I = nem indexelt fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-446">I = Non-indexed files</span></span>

<span data-ttu-id="447a0-447">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-447">**Applicable to:** Blobs, Files</span></span>

### <a name="delimiterdelimiter"></a><span data-ttu-id="447a0-448">/ Elválasztó karakter: "elválasztót".</span><span class="sxs-lookup"><span data-stu-id="447a0-448">/Delimiter:"delimiter"</span></span>
<span data-ttu-id="447a0-449">Azt jelzi, hogy az elválasztó karaktert, amely korlátozza a virtuális könyvtárak egy blob neve.</span><span class="sxs-lookup"><span data-stu-id="447a0-449">Indicates the delimiter character used to delimit virtual directories in a blob name.</span></span>

<span data-ttu-id="447a0-450">Alapértelmezés szerint az AzCopy által használt / elválasztó karakterként.</span><span class="sxs-lookup"><span data-stu-id="447a0-450">By default, AzCopy uses / as the delimiter character.</span></span> <span data-ttu-id="447a0-451">Azonban AzCopy támogatja a közös karakter használatát (például a @, #, vagy %) elválasztó.</span><span class="sxs-lookup"><span data-stu-id="447a0-451">However, AzCopy supports using any common character (such as @, #, or %) as a delimiter.</span></span> <span data-ttu-id="447a0-452">Ha meg kell adnia egy, a következő speciális karaktereket a parancssorban, tegye idézőjelek közé foglalt a fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="447a0-452">If you need to include one of these special characters on the command line, enclose the file name with double quotes.</span></span>

<span data-ttu-id="447a0-453">Ez a beállítás csak akkor alkalmazható blobok letöltése.</span><span class="sxs-lookup"><span data-stu-id="447a0-453">This option is only applicable for downloading blobs.</span></span>

<span data-ttu-id="447a0-454">**Alkalmazandó:** Blobok</span><span class="sxs-lookup"><span data-stu-id="447a0-454">**Applicable to:** Blobs</span></span>

### <a name="ncnumber-of-concurrent-operations"></a><span data-ttu-id="447a0-455">/ NC: "számot-az-egyidejű-műveletek"</span><span class="sxs-lookup"><span data-stu-id="447a0-455">/NC:"number-of-concurrent-operations"</span></span>
<span data-ttu-id="447a0-456">Megadja a párhuzamos műveletek számát.</span><span class="sxs-lookup"><span data-stu-id="447a0-456">Specifies the number of concurrent operations.</span></span>

<span data-ttu-id="447a0-457">AzCopy alapértelmezés szerint elindul bizonyos száma párhuzamos műveletek adatok átvitel növelhető.</span><span class="sxs-lookup"><span data-stu-id="447a0-457">AzCopy by default starts a certain number of concurrent operations to increase the data transfer throughput.</span></span> <span data-ttu-id="447a0-458">Ne feledje, hogy nagy száma párhuzamos műveletek alacsony sávszélességű környezetben előfordulhat, hogy ne terhelje tovább a hálózati kapcsolat tiltása a műveletek teljes befejezését.</span><span class="sxs-lookup"><span data-stu-id="447a0-458">Note that large number of concurrent operations in a low-bandwidth environment may overwhelm the network connection and prevent the operations from fully completing.</span></span> <span data-ttu-id="447a0-459">Párhuzamos műveletek alapján tényleges rendelkezésre álló hálózati sávszélesség szabályozását.</span><span class="sxs-lookup"><span data-stu-id="447a0-459">Throttle concurrent operations based on actual available network bandwidth.</span></span>

<span data-ttu-id="447a0-460">A párhuzamos műveletek felső határa 512.</span><span class="sxs-lookup"><span data-stu-id="447a0-460">The upper limit for concurrent operations is 512.</span></span>

<span data-ttu-id="447a0-461">**Alkalmazandó:** blobokat, fájlok, táblák</span><span class="sxs-lookup"><span data-stu-id="447a0-461">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="sourcetypeblob--table"></a><span data-ttu-id="447a0-462">/ Forrástípus: "Blob" |} "Table"</span><span class="sxs-lookup"><span data-stu-id="447a0-462">/SourceType:"Blob" | "Table"</span></span>
<span data-ttu-id="447a0-463">Megadja, hogy a `source` erőforrás elérhető a helyi fejlesztési környezetben, a storage emulator-beli blob.</span><span class="sxs-lookup"><span data-stu-id="447a0-463">Specifies that the `source` resource is a blob available in the local development environment, running in the storage emulator.</span></span>

<span data-ttu-id="447a0-464">**Alkalmazandó:** Blobok, táblák</span><span class="sxs-lookup"><span data-stu-id="447a0-464">**Applicable to:** Blobs, Tables</span></span>

### <a name="desttypeblob--table"></a><span data-ttu-id="447a0-465">/ DestType: "Blob" |} "Table"</span><span class="sxs-lookup"><span data-stu-id="447a0-465">/DestType:"Blob" | "Table"</span></span>
<span data-ttu-id="447a0-466">Megadja, hogy a `destination` erőforrás elérhető a helyi fejlesztési környezetben, a storage emulator-beli blob.</span><span class="sxs-lookup"><span data-stu-id="447a0-466">Specifies that the `destination` resource is a blob available in the local development environment, running in the storage emulator.</span></span>

<span data-ttu-id="447a0-467">**Alkalmazandó:** Blobok, táblák</span><span class="sxs-lookup"><span data-stu-id="447a0-467">**Applicable to:** Blobs, Tables</span></span>

### <a name="pkrskey1key2key3"></a><span data-ttu-id="447a0-468">/ PKRS: "key&#1;key&#2;key&#3;..."</span><span class="sxs-lookup"><span data-stu-id="447a0-468">/PKRS:"key1#key2#key3#..."</span></span>
<span data-ttu-id="447a0-469">Felosztja a tábla adatexportálási párhuzamosan, ami növeli az exportálási művelet sebességének engedélyezése kulcs partíciótartomány.</span><span class="sxs-lookup"><span data-stu-id="447a0-469">Splits the partition key range to enable exporting table data in parallel, which increases the speed of the export operation.</span></span>

<span data-ttu-id="447a0-470">Ha ez a beállítás nincs megadva, majd AzCopy segítségével egyetlen szálon táblaentitásokat exportálása.</span><span class="sxs-lookup"><span data-stu-id="447a0-470">If this option is not specified, then AzCopy uses a single thread to export table entities.</span></span> <span data-ttu-id="447a0-471">Például, ha a felhasználó határozza meg a /PKRS: "aa #bb", akkor az AzCopy három párhuzamos műveletek kezdődik.</span><span class="sxs-lookup"><span data-stu-id="447a0-471">For example, if the user specifies /PKRS:"aa#bb", then AzCopy starts three concurrent operations.</span></span>

<span data-ttu-id="447a0-472">Egyes műveletek exportálja egy három partíció kulcstartományokkal, alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="447a0-472">Each operation exports one of three partition key ranges, as shown below:</span></span>

  <span data-ttu-id="447a0-473">[első partíciókulcs, aa)</span><span class="sxs-lookup"><span data-stu-id="447a0-473">[first-partition-key, aa)</span></span>

  <span data-ttu-id="447a0-474">[aa, bb)</span><span class="sxs-lookup"><span data-stu-id="447a0-474">[aa, bb)</span></span>

  <span data-ttu-id="447a0-475">[bb, utolsó partíciókulcs]</span><span class="sxs-lookup"><span data-stu-id="447a0-475">[bb, last-partition-key]</span></span>

<span data-ttu-id="447a0-476">**Alkalmazandó:** táblák</span><span class="sxs-lookup"><span data-stu-id="447a0-476">**Applicable to:** Tables</span></span>

### <a name="splitsizefile-size"></a><span data-ttu-id="447a0-477">/ SplitSize: "fájl mérete"</span><span class="sxs-lookup"><span data-stu-id="447a0-477">/SplitSize:"file-size"</span></span>
<span data-ttu-id="447a0-478">Itt adhatja meg az exportált fájlt, osztott mérete MB-ban, a minimális megengedett érték 32.</span><span class="sxs-lookup"><span data-stu-id="447a0-478">Specifies the exported file split size in MB, the minimal value allowed is 32.</span></span>

<span data-ttu-id="447a0-479">Ez a beállítás nincs megadva, AzCopy a táblabeli adatok exportálása egy fájlból.</span><span class="sxs-lookup"><span data-stu-id="447a0-479">If this option is not specified, AzCopy will export table data to single file.</span></span>

<span data-ttu-id="447a0-480">Ha a tábla adatainak exportálása egy blobba, és az exportált fájl mérete eléri a 200 GB-os korlátot a blob-mérethez, majd a AzCopy az az exportált fájl vágási, még akkor is, ha ez a beállítás nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="447a0-480">If the table data is exported to a blob, and the exported file size reaches the 200 GB limit for blob size, then AzCopy will split the exported file, even if this option is not specified.</span></span>

<span data-ttu-id="447a0-481">**Alkalmazandó:** táblák</span><span class="sxs-lookup"><span data-stu-id="447a0-481">**Applicable to:** Tables</span></span>

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a><span data-ttu-id="447a0-482">/ EntityOperation: "InsertOrSkip" |} "InsertOrMerge" |} "InsertOrReplace"</span><span class="sxs-lookup"><span data-stu-id="447a0-482">/EntityOperation:"InsertOrSkip" | "InsertOrMerge" | "InsertOrReplace"</span></span>
<span data-ttu-id="447a0-483">Megadja a tábla importálása működéshez.</span><span class="sxs-lookup"><span data-stu-id="447a0-483">Specifies the table data import behavior.</span></span>

* <span data-ttu-id="447a0-484">InsertOrSkip - kihagyja a meglévő entitás vagy szúr be egy új entitást, ha a tábla nem létezik.</span><span class="sxs-lookup"><span data-stu-id="447a0-484">InsertOrSkip - Skips an existing entity or inserts a new entity if it does not exist in the table.</span></span>
* <span data-ttu-id="447a0-485">InsertOrMerge - egyesíti a meglévő entitás vagy szúr be egy új entitást, ha a tábla nem létezik.</span><span class="sxs-lookup"><span data-stu-id="447a0-485">InsertOrMerge - Merges an existing entity or inserts a new entity if it does not exist in the table.</span></span>
* <span data-ttu-id="447a0-486">InsertOrReplace - lecseréli a meglévő entitás vagy szúr be egy új entitást, ha a tábla nem létezik.</span><span class="sxs-lookup"><span data-stu-id="447a0-486">InsertOrReplace - Replaces an existing entity or inserts a new entity if it does not exist in the table.</span></span>

<span data-ttu-id="447a0-487">**Alkalmazandó:** táblák</span><span class="sxs-lookup"><span data-stu-id="447a0-487">**Applicable to:** Tables</span></span>

### <a name="manifestmanifest-file"></a><span data-ttu-id="447a0-488">/ Jegyzékfájl: "jegyzékfájl-file"</span><span class="sxs-lookup"><span data-stu-id="447a0-488">/Manifest:"manifest-file"</span></span>
<span data-ttu-id="447a0-489">A jegyzékfájl megadja a tábla exportálása és az importálási művelet.</span><span class="sxs-lookup"><span data-stu-id="447a0-489">Specifies the manifest file for the table export and import operation.</span></span>

<span data-ttu-id="447a0-490">Ez a beállítás nem kötelező, az exportálás során, AzCopy előre definiált nevű jegyzékfájlt hoz létre, ha ez a beállítás nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="447a0-490">This option is optional during the export operation, AzCopy will generate a manifest file with predefined name if this option is not specified.</span></span>

<span data-ttu-id="447a0-491">Ez a beállítás akkor szükséges az adatfájlok helyének az importálási művelet során.</span><span class="sxs-lookup"><span data-stu-id="447a0-491">This option is required during the import operation for locating the data files.</span></span>

<span data-ttu-id="447a0-492">**Alkalmazandó:** táblák</span><span class="sxs-lookup"><span data-stu-id="447a0-492">**Applicable to:** Tables</span></span>

### <a name="synccopy"></a><span data-ttu-id="447a0-493">/ SyncCopy</span><span class="sxs-lookup"><span data-stu-id="447a0-493">/SyncCopy</span></span>
<span data-ttu-id="447a0-494">Azt jelzi, hogy szinkron módon történik a blobok vagy két Azure Storage-végpontok közötti fájlok másolása.</span><span class="sxs-lookup"><span data-stu-id="447a0-494">Indicates whether to synchronously copy blobs or files between two Azure Storage endpoints.</span></span>

<span data-ttu-id="447a0-495">Alapértelmezés szerint AzCopy kiszolgálóoldali aszinkron másolatot használja.</span><span class="sxs-lookup"><span data-stu-id="447a0-495">AzCopy by default uses server-side asynchronous copy.</span></span> <span data-ttu-id="447a0-496">Adja meg ezt a beállítást, amely letölti a blobokat vagy -fájlok helyi memória, és feltölti Azure Storage szinkron másolatot végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="447a0-496">Specify this option to perform a synchronous copy, which downloads blobs or files to local memory and then uploads them to Azure Storage.</span></span>

<span data-ttu-id="447a0-497">Ezt a beállítást is használhatja, amikor Blob-tároló, a File storage belül, vagy a File storage vagy fordítva blobtárolóból belül a fájlok másolása.</span><span class="sxs-lookup"><span data-stu-id="447a0-497">You can use this option when copying files within Blob storage, within File storage, or from Blob storage to File storage or vice versa.</span></span>

<span data-ttu-id="447a0-498">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-498">**Applicable to:** Blobs, Files</span></span>

### <a name="setcontenttypecontent-type"></a><span data-ttu-id="447a0-499">/ SetContentType: "content-type"</span><span class="sxs-lookup"><span data-stu-id="447a0-499">/SetContentType:"content-type"</span></span>
<span data-ttu-id="447a0-500">Megadja a MIME content-type cél blobokkal vagy a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="447a0-500">Specifies the MIME content type for destination blobs or files.</span></span>

<span data-ttu-id="447a0-501">AzCopy állítja be az egy blob vagy a fájl tartalomtípusa application/octet-stream alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="447a0-501">AzCopy sets the content type for a blob or file to application/octet-stream by default.</span></span> <span data-ttu-id="447a0-502">Beállíthatja az összes BLOB vagy fájlok tartalomtípusa explicit megadása ehhez a beállításhoz tartozó értéket.</span><span class="sxs-lookup"><span data-stu-id="447a0-502">You can set the content type for all blobs or files by explicitly specifying a value for this option.</span></span>

<span data-ttu-id="447a0-503">Ha megadja ezt a beállítást érték nélküli, majd AzCopy állítja be minden egyes blob vagy a fájl tartalomtípusa alapján a fájl kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="447a0-503">If you specify this option without a value, then AzCopy will set each blob or file's content type according to its file extension.</span></span>

<span data-ttu-id="447a0-504">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="447a0-504">**Applicable to:** Blobs, Files</span></span>

### <a name="payloadformatjson--csv"></a><span data-ttu-id="447a0-505">/ PayloadFormat: "JSON" |} "CSV"</span><span class="sxs-lookup"><span data-stu-id="447a0-505">/PayloadFormat:"JSON" | "CSV"</span></span>
<span data-ttu-id="447a0-506">Megadja a tábla exportált adatok fájl formátumát.</span><span class="sxs-lookup"><span data-stu-id="447a0-506">Specifies the format of the table exported data file.</span></span>

<span data-ttu-id="447a0-507">Ha ez a beállítás nincs megadva, alapértelmezés szerint AzCopy exportálja tábla adatfájl JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="447a0-507">If this option is not specified, by default AzCopy exports table data file in JSON format.</span></span>

<span data-ttu-id="447a0-508">**Alkalmazandó:** táblák</span><span class="sxs-lookup"><span data-stu-id="447a0-508">**Applicable to:** Tables</span></span>

## <a name="known-issues-and-best-practices"></a><span data-ttu-id="447a0-509">Ismert problémák és ajánlott eljárások</span><span class="sxs-lookup"><span data-stu-id="447a0-509">Known Issues and Best Practices</span></span>
### <a name="limit-concurrent-writes-while-copying-data"></a><span data-ttu-id="447a0-510">Adatok másolása egyidejű írási műveletek korlátozása</span><span class="sxs-lookup"><span data-stu-id="447a0-510">Limit concurrent writes while copying data</span></span>
<span data-ttu-id="447a0-511">AzCopy rendelkező fájlokat vagy a BLOB másolása esetén vegye figyelembe, hogy egy másik alkalmazás módosítja az adatok másolása, amíg.</span><span class="sxs-lookup"><span data-stu-id="447a0-511">When you copy blobs or files with AzCopy, keep in mind that another application may be modifying the data while you are copying it.</span></span> <span data-ttu-id="447a0-512">Ha lehetséges győződjön meg arról, hogy a másolt adatok nem áll módosítás alatt a másolási művelet során.</span><span class="sxs-lookup"><span data-stu-id="447a0-512">If possible, ensure that the data you are copying is not being modified during the copy operation.</span></span> <span data-ttu-id="447a0-513">Például, ha egy Azure virtuális géphez társított virtuális merevlemez, győződjön meg arról, hogy más alkalmazás nem jelenleg írás a virtuális merevlemezhez.</span><span class="sxs-lookup"><span data-stu-id="447a0-513">For example, when copying a VHD associated with an Azure virtual machine, make sure that no other applications are currently writing to the VHD.</span></span> <span data-ttu-id="447a0-514">Egy jó úgy ehhez, hogy az erőforrás másolandó lízing.</span><span class="sxs-lookup"><span data-stu-id="447a0-514">A good way to do this is by leasing the resource to be copied.</span></span> <span data-ttu-id="447a0-515">Alternatív megoldásként először hozza létre a virtuális merevlemez pillanatképet, és másolja a pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="447a0-515">Alternately, you can create a snapshot of the VHD first and then copy the snapshot.</span></span>

<span data-ttu-id="447a0-516">A blobok vagy a fájlok írása közben másolja őket, hogy más alkalmazások nem megakadályozása, majd vegye figyelembe, hogy az idő, a feladat befejeződik, a másolt erőforrások már nincs a forrás-erőforrások teljes paritás.</span><span class="sxs-lookup"><span data-stu-id="447a0-516">If you cannot prevent other applications from writing to blobs or files while they are being copied, then keep in mind that by the time the job finishes, the copied resources may no longer have full parity with the source resources.</span></span>

### <a name="run-one-azcopy-instance-on-one-machine"></a><span data-ttu-id="447a0-517">Futtassa az AzCopy egy példány egy számítógépen.</span><span class="sxs-lookup"><span data-stu-id="447a0-517">Run one AzCopy instance on one machine.</span></span>
<span data-ttu-id="447a0-518">AzCopy célja, hogy felgyorsítsa az adatok átvitele a gép erőforrás-felhasználás, azt javasoljuk, hogy csak egy AzCopy példány egy számítógépen futtatja, és adja meg a beállítást `/NC` Ha több egyidejű műveletek van szüksége.</span><span class="sxs-lookup"><span data-stu-id="447a0-518">AzCopy is designed to maximize the utilization of your machine resource to accelerate the data transfer, we recommend you run only one AzCopy instance on one machine, and specify the option `/NC` if you need more concurrent operations.</span></span> <span data-ttu-id="447a0-519">További tudnivalókért írja be a `AzCopy /?:NC` a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="447a0-519">For more details, type `AzCopy /?:NC` at the command line.</span></span>

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a><span data-ttu-id="447a0-520">FIPS szabványnak megfelelő MD5 algoritmusok engedélyezése az AzCopy amikor meg "használata FIPS szabványnak megfelelő algoritmusok használata titkosításhoz, kivonatoláshoz és aláíráshoz".</span><span class="sxs-lookup"><span data-stu-id="447a0-520">Enable FIPS compliant MD5 algorithms for AzCopy when you "Use FIPS compliant algorithms for encryption, hashing and signing".</span></span>
<span data-ttu-id="447a0-521">AzCopy alapértelmezés szerint a .NET-MD5 végrehajtása használatával kiszámítja az MD5, objektumok másolásakor, de néhány biztonsági követelményt, amelyet AzCopy FIPS szabványnak megfelelő MD5 beállításnak az engedélyezéséhez szükség van.</span><span class="sxs-lookup"><span data-stu-id="447a0-521">AzCopy by default uses .NET MD5 implementation to calculate the MD5 when copying objects, but there are some security requirements that need AzCopy to enable FIPS compliant MD5 setting.</span></span>

<span data-ttu-id="447a0-522">Létrehozhat egy app.config fájl `AzCopy.exe.config` tulajdonsággal `AzureStorageUseV1MD5` , tegye a AzCopy.exe tartalékoljon.</span><span class="sxs-lookup"><span data-stu-id="447a0-522">You can create an app.config file `AzCopy.exe.config` with property `AzureStorageUseV1MD5` and put it aside with AzCopy.exe.</span></span>

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

<span data-ttu-id="447a0-523">Tulajdonság "AzureStorageUseV1MD5" • True - az alapértelmezett érték AzCopy .NET MD5 végrehajtására fogja használni.</span><span class="sxs-lookup"><span data-stu-id="447a0-523">For property "AzureStorageUseV1MD5" • True - The default value, AzCopy will use .NET MD5 implementation.</span></span>
<span data-ttu-id="447a0-524">• Hamis – AzCopy FIPS szabványnak megfelelő MD5 algoritmust használja.</span><span class="sxs-lookup"><span data-stu-id="447a0-524">• False – AzCopy will use FIPS compliant MD5 algorithm.</span></span>

<span data-ttu-id="447a0-525">Vegye figyelembe, hogy FIPS szabványnak megfelelő algoritmus a Windows-számítógépen alapértelmezés szerint le van tiltva, a Futtatás ablakba írja be a secpol.msc, és ellenőrizze, hogy ezt a kapcsolót, a biztonsági beállítások -> helyi házirend -> biztonsági beállítások -> rendszer-kriptográfia: használata FIPS-kompatibilis algoritmusok használata titkosításhoz, kivonatoláshoz és aláíráshoz.</span><span class="sxs-lookup"><span data-stu-id="447a0-525">Note that FIPS compliant algorithms is disabled by default on your Windows machine, you can type secpol.msc in your Run window and check this switch at Security Setting->Local Policy->Security Options->System cryptography: Use FIPS compliant algorithms for encryption, hashing and signing.</span></span>

## <a name="next-steps"></a><span data-ttu-id="447a0-526">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="447a0-526">Next steps</span></span>
<span data-ttu-id="447a0-527">Azure Storage és AzCopy kapcsolatos további információkért tekintse meg a következőket.</span><span class="sxs-lookup"><span data-stu-id="447a0-527">For more information about Azure Storage and AzCopy, refer to the following resources.</span></span>

### <a name="azure-storage-documentation"></a><span data-ttu-id="447a0-528">Az Azure Storage-dokumentáció:</span><span class="sxs-lookup"><span data-stu-id="447a0-528">Azure Storage documentation:</span></span>
* [<span data-ttu-id="447a0-529">Az Azure Storage bemutatása</span><span class="sxs-lookup"><span data-stu-id="447a0-529">Introduction to Azure Storage</span></span>](storage-introduction.md)
* [<span data-ttu-id="447a0-530">A .NET-Blob-tároló használata</span><span class="sxs-lookup"><span data-stu-id="447a0-530">How to use Blob storage from .NET</span></span>](storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="447a0-531">File storage .NET használata</span><span class="sxs-lookup"><span data-stu-id="447a0-531">How to use File storage from .NET</span></span>](storage-dotnet-how-to-use-files.md)
* [<span data-ttu-id="447a0-532">A Table storage a .NET használatával</span><span class="sxs-lookup"><span data-stu-id="447a0-532">How to use Table storage from .NET</span></span>](storage-dotnet-how-to-use-tables.md)
* [<span data-ttu-id="447a0-533">Tárfiókok létrehozása, kezelése vagy tárfiók törlése</span><span class="sxs-lookup"><span data-stu-id="447a0-533">How to create, manage, or delete a storage account</span></span>](storage-create-storage-account.md)
* [<span data-ttu-id="447a0-534">Adatátvitel az AzCopy Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="447a0-534">Transfer data with AzCopy on Linux</span></span>](storage-use-azcopy-linux.md)

### <a name="azure-storage-blog-posts"></a><span data-ttu-id="447a0-535">Az Azure Storage blogbejegyzések:</span><span class="sxs-lookup"><span data-stu-id="447a0-535">Azure Storage blog posts:</span></span>
* [<span data-ttu-id="447a0-536">Introducing Azure Storage adatátviteli könyvtár megtekintés</span><span class="sxs-lookup"><span data-stu-id="447a0-536">Introducing Azure Storage Data Movement Library Preview</span></span>](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [<span data-ttu-id="447a0-537">AzCopy: Bevezetéséről szinkron másolatot és testreszabott tartalom típusa</span><span class="sxs-lookup"><span data-stu-id="447a0-537">AzCopy: Introducing synchronous copy and customized content type</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [<span data-ttu-id="447a0-538">AzCopy: Általános rendelkezésre állási az AzCopy 3.0 és a tábla és a fájl-támogatással rendelkező az AzCopy 4.0 előzetes bejelentése</span><span class="sxs-lookup"><span data-stu-id="447a0-538">AzCopy: Announcing General Availability of AzCopy 3.0 plus preview release of AzCopy 4.0 with Table and File support</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [<span data-ttu-id="447a0-539">AzCopy: A felügyeleti teendők központjaként másolással optimalizált</span><span class="sxs-lookup"><span data-stu-id="447a0-539">AzCopy: Optimized for Large-Scale Copy Scenarios</span></span>](http://go.microsoft.com/fwlink/?LinkId=507682)
* [<span data-ttu-id="447a0-540">AzCopy: Írásvédett georedundáns tárolás támogatása</span><span class="sxs-lookup"><span data-stu-id="447a0-540">AzCopy: Support for read-access geo-redundant storage</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [<span data-ttu-id="447a0-541">AzCopy: Adatátvitelt újra elindítható móddal és SAS-jogkivonat</span><span class="sxs-lookup"><span data-stu-id="447a0-541">AzCopy: Transfer data with re-startable mode and SAS token</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [<span data-ttu-id="447a0-542">AzCopy: Kereszt-fiók másolási Blob használatával</span><span class="sxs-lookup"><span data-stu-id="447a0-542">AzCopy: Using cross-account Copy Blob</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [<span data-ttu-id="447a0-543">AzCopy: Feltöltése/fájlok letöltése Azure Blobokra vonatkozó</span><span class="sxs-lookup"><span data-stu-id="447a0-543">AzCopy: Uploading/downloading files for Azure Blobs</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

