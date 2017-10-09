---
title: "aaaCopy vagy move data tooAzure tároló a Windows AzCopy |} Microsoft Docs"
description: "Hello AzCopy használata Windows segédprogram toomove vagy másolat adatok tooor blob, table és a fájl. Adatok tooAzure tárolási átmásolhatja a helyi fájlokból, vagy adatok belül vagy tárfiókok között. Az adatok tooAzure Storage könnyen át."
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
ms.openlocfilehash: a77db84c3a3e06f0ad4e87d02b14a5c62ed8d9ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="transfer-data-with-hello-azcopy-on-windows"></a><span data-ttu-id="7c0fc-105">A Windows hello AzCopy az adatátvitel</span><span class="sxs-lookup"><span data-stu-id="7c0fc-105">Transfer data with hello AzCopy on Windows</span></span>
<span data-ttu-id="7c0fc-106">AzCopy adatok tooand másolására az egyszerű parancsokkal optimális teljesítménnyel Microsoft Azure Blob, a fájl és a Table storage egy parancssori segédprogram.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-106">AzCopy is a command-line utility designed for copying data tooand from Microsoft Azure Blob, File, and Table storage using simple commands with optimal performance.</span></span> <span data-ttu-id="7c0fc-107">Adatokat másolhat egy objektum tooanother a tárfiókon belül vagy tárfiókok között.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-107">You can copy data from one object tooanother within your storage account, or between storage accounts.</span></span>

<span data-ttu-id="7c0fc-108">AzCopy, letöltheti a két verziója van.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-108">There are two versions of AzCopy that you can download.</span></span> <span data-ttu-id="7c0fc-109">AzCopy a Windows a .NET-keretrendszer épül, és ez biztosítja a Windows stílus parancssori kapcsolókat.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-109">AzCopy on Windows is built with .NET Framework, and offers Windows style command-line options.</span></span> <span data-ttu-id="7c0fc-110">[AzCopy Linux](storage-use-azcopy-linux.md) épül, a .NET Core keretprogram, amelyik ajánlat POSIX-stílusú parancssori kapcsolók Linux platformon célozza.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-110">[AzCopy on Linux](storage-use-azcopy-linux.md) is built with .NET Core Framework which targets Linux platforms offering POSIX style command-line options.</span></span> <span data-ttu-id="7c0fc-111">Ez a cikk a Windows AzCopy ismerteti.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-111">This article covers AzCopy on Windows.</span></span>

## <a name="download-and-install-azcopy"></a><span data-ttu-id="7c0fc-112">Töltse le és telepítse az AzCopy</span><span class="sxs-lookup"><span data-stu-id="7c0fc-112">Download and install AzCopy</span></span>
### <a name="azcopy-on-windows"></a><span data-ttu-id="7c0fc-113">AzCopy Windowson</span><span class="sxs-lookup"><span data-stu-id="7c0fc-113">AzCopy on Windows</span></span>
<span data-ttu-id="7c0fc-114">Töltse le a hello [Windows AzCopy legújabb verzióját](http://aka.ms/downloadazcopy).</span><span class="sxs-lookup"><span data-stu-id="7c0fc-114">Download hello [latest version of AzCopy on Windows](http://aka.ms/downloadazcopy).</span></span>

#### <a name="installation-on-windows"></a><span data-ttu-id="7c0fc-115">Windows-telepítés</span><span class="sxs-lookup"><span data-stu-id="7c0fc-115">Installation on Windows</span></span>
<span data-ttu-id="7c0fc-116">Miután telepítette a AzCopy a Windows hello installer használatával, nyisson meg egy parancsablakot, és keresse meg a toohello AzCopy telepítési könyvtárára a számítógép -, ahol hello `AzCopy.exe` végrehajtható található.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-116">After installing AzCopy on Windows using hello installer, open a command window and navigate toohello AzCopy installation directory on your computer - where hello `AzCopy.exe` executable is located.</span></span> <span data-ttu-id="7c0fc-117">Ha szükséges, hello AzCopy telepítési tooyour rendszer elérési útjának is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-117">If desired, you can add hello AzCopy installation location tooyour system path.</span></span> <span data-ttu-id="7c0fc-118">Alapértelmezés szerint AzCopy túl van telepítve`%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` vagy `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-118">By default, AzCopy is installed too`%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` or `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.</span></span>

## <a name="writing-your-first-azcopy-command"></a><span data-ttu-id="7c0fc-119">Az első AzCopy parancs írása</span><span class="sxs-lookup"><span data-stu-id="7c0fc-119">Writing your first AzCopy command</span></span>
<span data-ttu-id="7c0fc-120">hello alapvető AzCopy parancs szintaxisa a következő:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-120">hello basic syntax for AzCopy commands is:</span></span>

```azcopy
AzCopy /Source:<source> /Dest:<destination> [Options]
```

<span data-ttu-id="7c0fc-121">hello a következő példák bemutatják, különféle forgatókönyvekhez, amik a Microsoft Azure-Blobokkal, fájlok és táblák adatok tooand másolására.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-121">hello following examples demonstrate a variety of scenarios for copying data tooand from Microsoft Azure Blobs, Files, and Tables.</span></span> <span data-ttu-id="7c0fc-122">Tekintse meg a toohello [AzCopy paraméterek](#azcopy-parameters) részletes leírását az egyes mintában használt hello paraméterek szakaszban.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-122">Refer toohello [AzCopy Parameters](#azcopy-parameters) section for a detailed explanation of hello parameters used in each sample.</span></span>

## <a name="blob-download"></a><span data-ttu-id="7c0fc-123">BLOB: letöltése</span><span class="sxs-lookup"><span data-stu-id="7c0fc-123">Blob: Download</span></span>
### <a name="download-single-blob"></a><span data-ttu-id="7c0fc-124">Egy blob letöltése</span><span class="sxs-lookup"><span data-stu-id="7c0fc-124">Download single blob</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"
```

<span data-ttu-id="7c0fc-125">Vegye figyelembe, hogy ha hello mappa `C:\myfolder` nem létezik, AzCopy fog létrehozni, és töltse le `abc.txt ` hello új mappába.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-125">Note that if hello folder `C:\myfolder` does not exist, AzCopy will create it and download `abc.txt ` into hello new folder.</span></span>

### <a name="download-single-blob-from-secondary-region"></a><span data-ttu-id="7c0fc-126">A másodlagos régióba egyetlen blob letöltése</span><span class="sxs-lookup"><span data-stu-id="7c0fc-126">Download single blob from secondary region</span></span>

```azcopy
AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

<span data-ttu-id="7c0fc-127">Vegye figyelembe, hogy írásvédett georedundáns tárolás engedélyezve kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-127">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="download-all-blobs"></a><span data-ttu-id="7c0fc-128">Töltse le az összes BLOB</span><span class="sxs-lookup"><span data-stu-id="7c0fc-128">Download all blobs</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S
```

<span data-ttu-id="7c0fc-129">Hello következő feltételezik hello megadott tárolóban található blobok:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-129">Assume hello following blobs reside in hello specified container:</span></span>  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

<span data-ttu-id="7c0fc-130">Hello letöltési művelet után hello directory `C:\myfolder` hello a következő fájlokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-130">After hello download operation, hello directory `C:\myfolder` will include hello following files:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

<span data-ttu-id="7c0fc-131">Ha nem adja meg a beállítás `/S`, nincs BLOB le lesznek töltve.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-131">If you do not specify option `/S`, no blobs will be downloaded.</span></span>

### <a name="download-blobs-with-specified-prefix"></a><span data-ttu-id="7c0fc-132">A megadott előtag blobok letöltése</span><span class="sxs-lookup"><span data-stu-id="7c0fc-132">Download blobs with specified prefix</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S
```

<span data-ttu-id="7c0fc-133">Hello következő feltételezik hello megadott tárolóban található blobok.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-133">Assume hello following blobs reside in hello specified container.</span></span> <span data-ttu-id="7c0fc-134">Hello előtaggal kezdődő összes BLOB `a` tölthető le:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-134">All blobs beginning with hello prefix `a` will be downloaded:</span></span>

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

<span data-ttu-id="7c0fc-135">Hello letöltési művelet után hello mappa `C:\myfolder` hello a következő fájlokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-135">After hello download operation, hello folder `C:\myfolder` will include hello following files:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

<span data-ttu-id="7c0fc-136">hello előtag toohello virtuális könyvtár, hello hello blob neve az első részét képező vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-136">hello prefix applies toohello virtual directory, which forms hello first part of hello blob name.</span></span> <span data-ttu-id="7c0fc-137">Hello a fenti példában hello virtuális könyvtár nem azonos hello megadott előtag, így azt nem töltődik le.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-137">In hello example shown above, hello virtual directory does not match hello specified prefix, so it is not downloaded.</span></span> <span data-ttu-id="7c0fc-138">Emellett, ha hello lehetőséget `\S` nincs megadva, az AzCopy nem tölti le a blobokat.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-138">In addition, if hello option `\S` is not specified, AzCopy will not download any blobs.</span></span>

### <a name="set-hello-last-modified-time-of-exported-files-toobe-same-as-hello-source-blobs"></a><span data-ttu-id="7c0fc-139">Állítsa be az exportált fájlokat toobe hello utolsó módosításának időpontja szerint forrás blobok hello</span><span class="sxs-lookup"><span data-stu-id="7c0fc-139">Set hello last-modified time of exported files toobe same as hello source blobs</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT
```

<span data-ttu-id="7c0fc-140">Blobok emellett kizárja hello letöltési művelet az utolsó módosításának ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-140">You can also exclude blobs from hello download operation based on their last-modified time.</span></span> <span data-ttu-id="7c0fc-141">Például, ha azt szeretné, hogy a tooexclude blobot, amelynek utolsó módosítási időpontjának hello azonos vagy újabb, mint hello célfájl, adja meg hello `/XN` lehetőséget:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-141">For example, if you want tooexclude blobs whose last modified time is hello same or newer than hello destination file, add hello `/XN` option:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN
```

<span data-ttu-id="7c0fc-142">Vagy ha azt szeretné, hogy tooexclude blobot, amelynek utolsó módosítási időpontjának hello azonos vagy régebbi, mint hello célfájl, adja hozzá a hello `/XO` lehetőséget:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-142">Or if you want tooexclude blobs whose last modified time is hello same or older than hello destination file, add hello `/XO` option:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO
```

## <a name="blob-upload"></a><span data-ttu-id="7c0fc-143">BLOB: feltöltése</span><span class="sxs-lookup"><span data-stu-id="7c0fc-143">Blob: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="7c0fc-144">Töltse fel egy fájlból</span><span class="sxs-lookup"><span data-stu-id="7c0fc-144">Upload single file</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"
```

<span data-ttu-id="7c0fc-145">Ha hello megadott tároló nem létezik, a AzCopy hozza létre, és bele hello-fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-145">If hello specified destination container does not exist, AzCopy will create it and upload hello file into it.</span></span>

### <a name="upload-single-file-toovirtual-directory"></a><span data-ttu-id="7c0fc-146">Egy fájlból toovirtual directory feltöltése</span><span class="sxs-lookup"><span data-stu-id="7c0fc-146">Upload single file toovirtual directory</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt
```

<span data-ttu-id="7c0fc-147">Ha hello megadott virtuális könyvtár nem létezik, AzCopy fel kell töltenie hello tooinclude hello virtuális könyvtárának nevében (*pl.*, `vd/abc.txt` a fenti példában hello).</span><span class="sxs-lookup"><span data-stu-id="7c0fc-147">If hello specified virtual directory does not exist, AzCopy will upload hello file tooinclude hello virtual directory in its name (*e.g.*, `vd/abc.txt` in hello example above).</span></span>

### <a name="upload-all-files"></a><span data-ttu-id="7c0fc-148">Minden fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="7c0fc-148">Upload all files</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S
```

<span data-ttu-id="7c0fc-149">Beállítás megadása `/S` hello feltöltések hello tartalmát a megadott könyvtár tooBlob tárolási rekurzív módon, ami azt jelenti, hogy minden almappa és a fájlok lesz feltöltve is.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-149">Specifying option `/S` uploads hello contents of hello specified directory tooBlob storage recursively, meaning that all subfolders and their files will be uploaded as well.</span></span> <span data-ttu-id="7c0fc-150">Például azt feltételezik hello következő mappában található fájlok `C:\myfolder`:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-150">For instance, assume hello following files reside in folder `C:\myfolder`:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

<span data-ttu-id="7c0fc-151">Hello tároló hello feltöltési művelet után következő fájlok hello tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-151">After hello upload operation, hello container will include hello following files:</span></span>

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

<span data-ttu-id="7c0fc-152">Ha nem adja meg a beállítás `/S`, AzCopy nem rekurzív módon tölti fel.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-152">If you do not specify option `/S`, AzCopy will not upload recursively.</span></span> <span data-ttu-id="7c0fc-153">Hello tároló hello feltöltési művelet után következő fájlok hello tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-153">After hello upload operation, hello container will include hello following files:</span></span>

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="7c0fc-154">Töltse fel a megadott mintának megfelelő fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-154">Upload files matching specified pattern</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S
```

<span data-ttu-id="7c0fc-155">Hello következő feltételezik mappában található fájlok `C:\myfolder`:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-155">Assume hello following files reside in folder `C:\myfolder`:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

<span data-ttu-id="7c0fc-156">Hello tároló hello feltöltési művelet után következő fájlok hello tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-156">After hello upload operation, hello container will include hello following files:</span></span>

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

<span data-ttu-id="7c0fc-157">Ha nem adja meg a beállítás `/S`, AzCopy csak fel kell töltenie a virtuális könyvtár nem található blobok:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-157">If you do not specify option `/S`, AzCopy will only upload blobs that don't reside in a virtual directory:</span></span>

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-hello-mime-content-type-of-a-destination-blob"></a><span data-ttu-id="7c0fc-158">Adjon meg egy cél BLOB hello MIME tartalomtípus</span><span class="sxs-lookup"><span data-stu-id="7c0fc-158">Specify hello MIME content type of a destination blob</span></span>
<span data-ttu-id="7c0fc-159">Alapértelmezés szerint AzCopy beállítja hello tartalomtípusa cél blob túl`application/octet-stream`.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-159">By default, AzCopy sets hello content type of a destination blob too`application/octet-stream`.</span></span> <span data-ttu-id="7c0fc-160">3.1.0 verziójával kezdve explicit módon megadhatja hello tartalomtípus keresztül hello beállítás `/SetContentType:[content-type]`.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-160">Beginning with version 3.1.0, you can explicitly specify hello content type via hello option `/SetContentType:[content-type]`.</span></span> <span data-ttu-id="7c0fc-161">Ez a szintaxis hello content-type összes BLOB a feltöltési művelet állítja be.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-161">This syntax sets hello content type for all blobs in an upload operation.</span></span>

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4
```

<span data-ttu-id="7c0fc-162">Ha megad `/SetContentType` nélkül értéket, majd AzCopy állítja be minden egyes blob vagy a fájl tartalomtípusa szerint tooits fájl kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-162">If you specify `/SetContentType` without a value, then AzCopy will set each blob or file's content type according tooits file extension.</span></span>

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType
```

## <a name="blob-copy"></a><span data-ttu-id="7c0fc-163">BLOB: másolása</span><span class="sxs-lookup"><span data-stu-id="7c0fc-163">Blob: Copy</span></span>
### <a name="copy-single-blob-within-storage-account"></a><span data-ttu-id="7c0fc-164">Másolja át egy blob Storage-fiókban</span><span class="sxs-lookup"><span data-stu-id="7c0fc-164">Copy single blob within Storage account</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt
```

<span data-ttu-id="7c0fc-165">Ha egy blobba egy tárfiókon belül másol egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-165">When you copy a blob within a Storage account, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-across-storage-accounts"></a><span data-ttu-id="7c0fc-166">Másolja át egy blob Storage-fiókok között</span><span class="sxs-lookup"><span data-stu-id="7c0fc-166">Copy single blob across Storage accounts</span></span>

```azcopy
AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

<span data-ttu-id="7c0fc-167">A blob Storage-fiókok között másolásakor egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-167">When you copy a blob across Storage accounts, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-from-secondary-region-tooprimary-region"></a><span data-ttu-id="7c0fc-168">Egy blob másolása másodlagos régióba tooprimary régió</span><span class="sxs-lookup"><span data-stu-id="7c0fc-168">Copy single blob from secondary region tooprimary region</span></span>

```azcopy
AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

<span data-ttu-id="7c0fc-169">Vegye figyelembe, hogy írásvédett georedundáns tárolás engedélyezve kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-169">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a><span data-ttu-id="7c0fc-170">Egy blob másolási és a pillanatképek tárfiókok között</span><span class="sxs-lookup"><span data-stu-id="7c0fc-170">Copy single blob and its snapshots across Storage accounts</span></span>

```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot
```

<span data-ttu-id="7c0fc-171">Hello másolási művelet után hello céltároló hello blob és a pillanatképek tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-171">After hello copy operation, hello target container will include hello blob and its snapshots.</span></span> <span data-ttu-id="7c0fc-172">Ha a fenti példában hello hello blob két pillanatképekkel rendelkezik, hello tároló tartalmazza hello következő blob és a pillanatképeket:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-172">Assuming hello blob in hello example above has two snapshots, hello container will include hello following blob and snapshots:</span></span>

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="synchronously-copy-blobs-across-storage-accounts"></a><span data-ttu-id="7c0fc-173">Szinkron módon másolni BLOB Storage-fiókok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-173">Synchronously copy blobs across Storage accounts</span></span>
<span data-ttu-id="7c0fc-174">AzCopy alapértelmezés szerint aszinkron módon másolja az adatokat két tárolási végpontok közötti.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-174">AzCopy by default copies data between two storage endpoints asynchronously.</span></span> <span data-ttu-id="7c0fc-175">Ezért hello másolási művelet tartalékolt sávszélesség-kapacitása, amelynek nincs SLA-t használó hello háttérben fog futni milyen gyors blob kerülnek, valamint AzCopy rendszeres időközönként ellenőrizze hello másolat állapota, amíg nem fejeződött be, vagy hello másolása nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-175">Therefore, hello copy operation will run in hello background using spare bandwidth capacity that has no SLA in terms of how fast a blob will be copied, and AzCopy will periodically check hello copy status until hello copying is completed or failed.</span></span>

<span data-ttu-id="7c0fc-176">Hello `/SyncCopy` lehetőség biztosítja, hogy hello másolási művelet egységes sebesség fogja kapni.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-176">hello `/SyncCopy` option ensures that hello copy operation will get consistent speed.</span></span> <span data-ttu-id="7c0fc-177">AzCopy elkészíti hello szinkron hello blobok letöltésével toocopy hello a megadott forrás toolocal memória, és majd feltöltheti toohello Blob célhelyet.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-177">AzCopy performs hello synchronous copy by downloading hello blobs toocopy from hello specified source toolocal memory, and then uploading them toohello Blob storage destination.</span></span>

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy
```

<span data-ttu-id="7c0fc-178">`/SyncCopy`hozhat létre további költségeket összehasonlított tooasynchronous másolási hello kilépő ajánlott megközelítést alkalmazva van toouse ezt a beállítást egy Azure virtuális gép, amely hello és a forrás tárolási fiók tooavoid kilépő költségű ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-178">`/SyncCopy` might generate additional egress cost compared tooasynchronous copy, hello recommended approach is toouse this option in an Azure VM that is in hello same region as your source storage account tooavoid egress cost.</span></span>

## <a name="file-download"></a><span data-ttu-id="7c0fc-179">Fájl: letöltése</span><span class="sxs-lookup"><span data-stu-id="7c0fc-179">File: Download</span></span>
### <a name="download-single-file"></a><span data-ttu-id="7c0fc-180">Töltse le egy fájlból</span><span class="sxs-lookup"><span data-stu-id="7c0fc-180">Download single file</span></span>

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

<span data-ttu-id="7c0fc-181">Hello adott forrása az Azure fájlmegosztások, akkor meg kell adnia hello pontosan a fájl nevét, ha (*pl.* `abc.txt`) toodownload egyetlen fájl, vagy adja meg a beállítás `/S` összes toodownload hello megosztáson található fájlok rekurzív módon.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-181">If hello specified source is an Azure file share, then you must either specify hello exact file name, (*e.g.* `abc.txt`) toodownload a single file, or specify option `/S` toodownload all files in hello share recursively.</span></span> <span data-ttu-id="7c0fc-182">Kísérlet toospecify egy fájl és egyaránt lehetőség `/S` együtt hibát eredményez.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-182">Attempting toospecify both a file pattern and option `/S` together will result in an error.</span></span>

### <a name="download-all-files"></a><span data-ttu-id="7c0fc-183">Minden fájl letöltése</span><span class="sxs-lookup"><span data-stu-id="7c0fc-183">Download all files</span></span>

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S
```

<span data-ttu-id="7c0fc-184">Vegye figyelembe, hogy üres mappák nem lesznek letöltve.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-184">Note that any empty folders will not be downloaded.</span></span>

## <a name="file-upload"></a><span data-ttu-id="7c0fc-185">Fájl: feltöltése</span><span class="sxs-lookup"><span data-stu-id="7c0fc-185">File: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="7c0fc-186">Töltse fel egy fájlból</span><span class="sxs-lookup"><span data-stu-id="7c0fc-186">Upload single file</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt
```

### <a name="upload-all-files"></a><span data-ttu-id="7c0fc-187">Minden fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="7c0fc-187">Upload all files</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S
```

<span data-ttu-id="7c0fc-188">Vegye figyelembe, hogy bármely üres mappa nem lesz feltöltve.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-188">Note that any empty folders will not be uploaded.</span></span>

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="7c0fc-189">Töltse fel a megadott mintának megfelelő fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-189">Upload files matching specified pattern</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S
```

## <a name="file-copy"></a><span data-ttu-id="7c0fc-190">Fájl: másolása</span><span class="sxs-lookup"><span data-stu-id="7c0fc-190">File: Copy</span></span>
### <a name="copy-across-file-shares"></a><span data-ttu-id="7c0fc-191">Fájlmegosztások másolni</span><span class="sxs-lookup"><span data-stu-id="7c0fc-191">Copy across file shares</span></span>

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S
```
<span data-ttu-id="7c0fc-192">Amikor fájlmegosztások között másolhat egy fájlt egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-192">When you copy a file across file shares, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-from-file-share-tooblob"></a><span data-ttu-id="7c0fc-193">Megosztás tooblob fájl másolása</span><span class="sxs-lookup"><span data-stu-id="7c0fc-193">Copy from file share tooblob</span></span>

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S
```
<span data-ttu-id="7c0fc-194">Ha fájlt átmásolni fájl megosztási tooblob, egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-194">When you copy a file from file share tooblob, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>


### <a name="copy-from-blob-toofile-share"></a><span data-ttu-id="7c0fc-195">A blob toofile megosztásból másolása</span><span class="sxs-lookup"><span data-stu-id="7c0fc-195">Copy from blob toofile share</span></span>

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S
```
<span data-ttu-id="7c0fc-196">Amikor blob toofile megosztásból másolhat egy fájlt egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-196">When you copy a file from blob toofile share, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="synchronously-copy-files"></a><span data-ttu-id="7c0fc-197">Szinkron módon történik a fájlok másolása</span><span class="sxs-lookup"><span data-stu-id="7c0fc-197">Synchronously copy files</span></span>
<span data-ttu-id="7c0fc-198">Megadhatja a hello `/SyncCopy` toocopy adatait a File Storage tooFile tároló, a File Storage tooBlob tárolási lehetőséget, és a tárolási Blob Storage tooFile szinkron módon történik, AzCopy ennek hello forrás adatok toolocal memória töltése és töltse fel az újra toodestination.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-198">You can specify hello `/SyncCopy` option toocopy data from File Storage tooFile Storage, from File Storage tooBlob Storage and from Blob Storage tooFile Storage synchronously, AzCopy does this by downloading hello source data toolocal memory and upload it again toodestination.</span></span> <span data-ttu-id="7c0fc-199">Standard kilépő költség fog vonatkozni.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-199">Standard egress cost will apply.</span></span>

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy
```

<span data-ttu-id="7c0fc-200">Amikor a File Storage tooBlob tárolási másol, hello alapértelmezett blob típushoz blokkblob, felhasználói beállítást adja meg `/BlobType:page` toochange hello céltípus blob.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-200">When copying from File Storage tooBlob Storage, hello default blob type is block blob, user can specify option `/BlobType:page` toochange hello destination blob type.</span></span>

<span data-ttu-id="7c0fc-201">Vegye figyelembe, hogy `/SyncCopy` összehasonlító tooasynchronous másolási költség további kilépő hozhat létre, hello ajánlott megoldás, ezt a beállítást, az Azure virtuális Gépen, amely hello hello toouse és a forrás tárolási fiók tooavoid kilépő költségű ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-201">Note that `/SyncCopy` might generate additional egress cost comparing tooasynchronous copy, hello recommended approach is toouse this option in hello Azure VM which is in hello same region as your source storage account tooavoid egress cost.</span></span>

## <a name="table-export"></a><span data-ttu-id="7c0fc-202">Tábla: exportálása</span><span class="sxs-lookup"><span data-stu-id="7c0fc-202">Table: Export</span></span>
### <a name="export-table"></a><span data-ttu-id="7c0fc-203">Tábla exportálása</span><span class="sxs-lookup"><span data-stu-id="7c0fc-203">Export table</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key
```

<span data-ttu-id="7c0fc-204">AzCopy ír a jegyzékfájl toohello megadott célmappát.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-204">AzCopy writes a manifest file toohello specified destination folder.</span></span> <span data-ttu-id="7c0fc-205">hello jegyzékfájl hello importálási folyamat toolocate hello szükséges adatok fájlokat használnak, és az adatellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-205">hello manifest file is used in hello import process toolocate hello necessary data files and perform data validation.</span></span> <span data-ttu-id="7c0fc-206">hello jegyzékfájl hello elnevezési alapértelmezés szerint a következő használja:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-206">hello manifest file uses hello following naming convention by default:</span></span>

    <account name>_<table name>_<timestamp>.manifest

<span data-ttu-id="7c0fc-207">Felhasználói is megadhat hello beállítás `/Manifest:<manifest file name>` tooset hello jegyzékfájl neve.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-207">User can also specify hello option `/Manifest:<manifest file name>` tooset hello manifest file name.</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest
```

### <a name="split-export-into-multiple-files"></a><span data-ttu-id="7c0fc-208">Több fájlra vegyes exportálása</span><span class="sxs-lookup"><span data-stu-id="7c0fc-208">Split export into multiple files</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100
```

<span data-ttu-id="7c0fc-209">AzCopy használ egy *kötet index* adatok hello a fájlnevek toodistinguish több fájlt.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-209">AzCopy uses a *volume index* in hello split data file names toodistinguish multiple files.</span></span> <span data-ttu-id="7c0fc-210">hello kötet index két részből áll egy *partíciós kulcs tartományindexszel* és egy *vegyes fájl index*.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-210">hello volume index consists of two parts, a *partition key range index* and a *split file index*.</span></span> <span data-ttu-id="7c0fc-211">Mindkét indexek nulla alapú.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-211">Both indexes are zero-based.</span></span>

<span data-ttu-id="7c0fc-212">hello partíciós kulcs tartományindexszel 0 lesz, ha a felhasználó nem adja meg a beállítás `/PKRS`.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-212">hello partition key range index will be 0 if user does not specify option `/PKRS`.</span></span>

<span data-ttu-id="7c0fc-213">Tegyük fel például, AzCopy két adatfájlok állít elő, miután hello felhasználó határozza meg a beállítás `/SplitSize`.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-213">For instance, suppose AzCopy generates two data files after hello user specifies option `/SplitSize`.</span></span> <span data-ttu-id="7c0fc-214">adatok fájlnevek eredő hello lehet:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-214">hello resulting data file names might be:</span></span>

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

<span data-ttu-id="7c0fc-215">Vegye figyelembe, hogy hello lehetséges minimális értékének a beállításhoz tartozó `/SplitSize` 32 MB-nál.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-215">Note that hello minimum possible value for option `/SplitSize` is 32MB.</span></span> <span data-ttu-id="7c0fc-216">Ha hello megadott célobjektuma Blob-tároló, AzCopy vágási hello adatfájl egyszer annak mérete eléri hello blob mérete korlátozás (200 GB-os), függetlenül attól, hogy lehetőséget `/SplitSize` hello felhasználó által meghatározott.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-216">If hello specified destination is Blob storage, AzCopy will split hello data file once its sizes reaches hello blob size limitation (200GB), regardless of whether option `/SplitSize` has been specified by hello user.</span></span>

### <a name="export-table-toojson-or-csv-data-file-format"></a><span data-ttu-id="7c0fc-217">Tábla tooJSON vagy CSV-fájlformátumot adatok exportálása</span><span class="sxs-lookup"><span data-stu-id="7c0fc-217">Export table tooJSON or CSV data file format</span></span>
<span data-ttu-id="7c0fc-218">Alapértelmezés szerint AzCopy táblák tooJSON adatfájlok exportálja.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-218">AzCopy by default exports tables tooJSON data files.</span></span> <span data-ttu-id="7c0fc-219">Hello beállítást adhat meg `/PayloadFormat:JSON|CSV` tooexport hello táblák JSON vagy a fürt megosztott kötetei szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-219">You can specify hello option `/PayloadFormat:JSON|CSV` tooexport hello tables as JSON or CSV.</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV
```

<span data-ttu-id="7c0fc-220">Hello CSV az adattartalom formátuma megadásakor AzCopy is létrehoz egy séma kiterjesztésű fájlt `.schema.csv` minden olyan adatfájl esetében.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-220">When specifying hello CSV payload format, AzCopy will also generate a schema file with file extension `.schema.csv` for each data file.</span></span>

### <a name="export-table-entities-concurrently"></a><span data-ttu-id="7c0fc-221">Táblaentitásokat egyidejűleg exportálása</span><span class="sxs-lookup"><span data-stu-id="7c0fc-221">Export table entities concurrently</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"
```

<span data-ttu-id="7c0fc-222">AzCopy indítása párhuzamos műveletek tooexport entitások hello felhasználó határozza meg a beállítás `/PKRS`.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-222">AzCopy will start concurrent operations tooexport entities when hello user specifies option `/PKRS`.</span></span> <span data-ttu-id="7c0fc-223">Egyes műveletek egy kulcs partíciótartomány exportálja.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-223">Each operation exports one partition key range.</span></span>

<span data-ttu-id="7c0fc-224">Vegye figyelembe, hogy hello száma párhuzamos műveletek is beállítás által vezérelt `/NC`.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-224">Note that hello number of concurrent operations is also controlled by option `/NC`.</span></span> <span data-ttu-id="7c0fc-225">AzCopy Processzormagok száma hello használja, mint a hello alapértelmezett értékének `/NC` táblaentitásokat, másolásakor akkor is, ha `/NC` nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-225">AzCopy uses hello number of core processors as hello default value of `/NC` when copying table entities, even if `/NC` was not specified.</span></span> <span data-ttu-id="7c0fc-226">Ha a hello felhasználói beállítás megad `/PKRS`, AzCopy hello kisebb hello két értékek - partíció kulcstartományokkal és implicit vagy explicit módon megadott párhuzamos műveletek - toodetermine hello száma párhuzamos műveletek toostart használja.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-226">When hello user specifies option `/PKRS`, AzCopy uses hello smaller of hello two values - partition key ranges versus implicitly or explicitly specified concurrent operations - toodetermine hello number of concurrent operations toostart.</span></span> <span data-ttu-id="7c0fc-227">További tudnivalókért írja be a `AzCopy /?:NC` hello parancssorból.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-227">For more details, type `AzCopy /?:NC` at hello command line.</span></span>

### <a name="export-table-tooblob"></a><span data-ttu-id="7c0fc-228">Exportálási tábla tooblob</span><span class="sxs-lookup"><span data-stu-id="7c0fc-228">Export table tooblob</span></span>

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2
```

<span data-ttu-id="7c0fc-229">AzCopy hoz létre egy JSON-adatfájlt a következő elnevezési konvenció hello blob tárolóba:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-229">AzCopy will generate a JSON data file into hello blob container with following naming convention:</span></span>

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

<span data-ttu-id="7c0fc-230">hello létrehozott JSON-adatfájlt a következő hello az adattartalom formátuma a szükséges metaadatokat.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-230">hello generated JSON data file follows hello payload format for minimal metadata.</span></span> <span data-ttu-id="7c0fc-231">Ez az adattartalom formátuma a részletekért lásd: [az adattartalom formátuma Table szolgáltatási műveletek](http://msdn.microsoft.com/library/azure/dn535600.aspx).</span><span class="sxs-lookup"><span data-stu-id="7c0fc-231">For details on this payload format, see [Payload Format for Table Service Operations](http://msdn.microsoft.com/library/azure/dn535600.aspx).</span></span>

<span data-ttu-id="7c0fc-232">Vegye figyelembe, hogy táblák tooblobs exportálásakor AzCopy fog hello tábla entitások toolocal ideiglenes adatfájlok le, majd töltse fel ezeket az entitásokat toohello blob.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-232">Note that when exporting tables tooblobs, AzCopy will download hello Table entities toolocal temporary data files and then upload those entities toohello blob.</span></span> <span data-ttu-id="7c0fc-233">Ezek ideiglenes fájlokat kerüljenek mappába hello napló fájl hello alapértelmezett elérési úttal rendelkező "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>", lehetőséget adhat meg/Z: [napló-fájlok és mappák] toochange hello napló fájlok mappáját, és így a hello ideiglenes fájlok helyének módosításához.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-233">These temporary data files are put into hello journal file folder with hello default path "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>", you can specify option /Z:[journal-file-folder] toochange hello journal file folder location and thus change hello temporary data files location.</span></span> <span data-ttu-id="7c0fc-234">hello ideiglenes fájlok mérete, amelyekről a táblaentitásokat és hello mérete a megadott hello beállítás /SplitSize, bár a helyi lemez hello ideiglenes fájlt törli azonnal azt követően adatok feltöltése toohello blob, ellenőrizze, hogy Ön Ahhoz, hogy elég helyi lemez terület toostore ezek ideiglenes fájlokat akkor azok törlődnek.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-234">hello temporary data files' size is decided by your table entities' size and hello size you specified with hello option /SplitSize, although hello temporary data file in local disk will be deleted instantly once it has been uploaded toohello blob, please make sure you have enough local disk space toostore these temporary data files before they are deleted.</span></span>

## <a name="table-import"></a><span data-ttu-id="7c0fc-235">Tábla: importálása</span><span class="sxs-lookup"><span data-stu-id="7c0fc-235">Table: Import</span></span>
### <a name="import-table"></a><span data-ttu-id="7c0fc-236">Tábla importálása</span><span class="sxs-lookup"><span data-stu-id="7c0fc-236">Import table</span></span>

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace
```

<span data-ttu-id="7c0fc-237">a beállítás hello `/EntityOperation` azt jelzi, hogyan tooinsert entitások be hello tábla.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-237">hello option `/EntityOperation` indicates how tooinsert entities into hello table.</span></span> <span data-ttu-id="7c0fc-238">Lehetséges értékek:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-238">Possible values are:</span></span>

* <span data-ttu-id="7c0fc-239">`InsertOrSkip`: A meglévő entitás kihagyja vagy szúr be egy új entitást, ha hello tábla nem létezik.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-239">`InsertOrSkip`: Skips an existing entity or inserts a new entity if it does not exist in hello table.</span></span>
* <span data-ttu-id="7c0fc-240">`InsertOrMerge`: Egyesíti a meglévő entitás vagy szúr be egy új entitást, ha hello tábla nem létezik.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-240">`InsertOrMerge`: Merges an existing entity or inserts a new entity if it does not exist in hello table.</span></span>
* <span data-ttu-id="7c0fc-241">`InsertOrReplace`: A felváltja meglévő entitás vagy szúr be egy új entitást, ha hello tábla nem létezik.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-241">`InsertOrReplace`: Replaces an existing entity or inserts a new entity if it does not exist in hello table.</span></span>

<span data-ttu-id="7c0fc-242">Vegye figyelembe, hogy a beállítás nem adható meg `/PKRS` hello importálás esetén.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-242">Note that you cannot specify option `/PKRS` in hello import scenario.</span></span> <span data-ttu-id="7c0fc-243">Hello exportálási forgatókönyv, amelyben a kapcsolót kell megadnia eltérően `/PKRS` toostart párhuzamos műveletek, AzCopy alapértelmezés szerint elindul párhuzamos műveletek tábla importálásakor.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-243">Unlike hello export scenario, in which you must specify option `/PKRS` toostart concurrent operations, AzCopy will by default start concurrent operations when you import a table.</span></span> <span data-ttu-id="7c0fc-244">hello alapértelmezett száma párhuzamos műveletek indítása processzormagokkal; egyenlő toohello száma azonban megadhat egyidejű lehetőséggel különböző számú `/NC`.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-244">hello default number of concurrent operations started is equal toohello number of core processors; however, you can specify a different number of concurrent with option `/NC`.</span></span> <span data-ttu-id="7c0fc-245">További tudnivalókért írja be a `AzCopy /?:NC` hello parancssorból.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-245">For more details, type `AzCopy /?:NC` at hello command line.</span></span>

<span data-ttu-id="7c0fc-246">Ügyeljen arra, hogy AzCopy csak támogatja a JSON, CSV nem importálása.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-246">Note that AzCopy only supports importing for JSON, not CSV.</span></span> <span data-ttu-id="7c0fc-247">AzCopy nem támogatja a felhasználó által létrehozott JSON tábla származó és fájlok manifest.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-247">AzCopy does not support table imports from user-created JSON and manifest files.</span></span> <span data-ttu-id="7c0fc-248">Az AzCopy tábla exportálása mindkét fájl kell származnia.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-248">Both of these files must come from an AzCopy table export.</span></span> <span data-ttu-id="7c0fc-249">tooavoid hibák, ne módosítsa hello exportált JSON vagy jegyzékfájlt.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-249">tooavoid errors, please do not modify hello exported JSON or manifest file.</span></span>

### <a name="import-entities-tootable-using-blobs"></a><span data-ttu-id="7c0fc-250">Importálás entitások tootable blobs használata</span><span class="sxs-lookup"><span data-stu-id="7c0fc-250">Import entities tootable using blobs</span></span>
<span data-ttu-id="7c0fc-251">Tegyük fel, a Blob-tároló tartalmazza hello alábbi: az Azure-tábla és a hozzá tartozó jegyzékfájl jelölő A JSON-fájlt.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-251">Assume a Blob container contains hello following: A JSON file representing an Azure Table and its accompanying manifest file.</span></span>

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

<span data-ttu-id="7c0fc-252">Futtathatja a következő parancs tooimport entitások egy táblába a blob tárolóban hello jegyzékfájl hello:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-252">You can run hello following command tooimport entities into a table using hello manifest file in that blob container:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"
```

## <a name="other-azcopy-features"></a><span data-ttu-id="7c0fc-253">Más AzCopy szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="7c0fc-253">Other AzCopy features</span></span>
### <a name="only-copy-data-that-doesnt-exist-in-hello-destination"></a><span data-ttu-id="7c0fc-254">Csak másolja az adatokat, amely a célként megadott hello nem létezik</span><span class="sxs-lookup"><span data-stu-id="7c0fc-254">Only copy data that doesn't exist in hello destination</span></span>
<span data-ttu-id="7c0fc-255">Hello `/XO` és `/XN` paraméterek tooexclude régebbi vagy újabb forrás erőforrások másolását, illetve lehetővé teszik.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-255">hello `/XO` and `/XN` parameters allow you tooexclude older or newer source resources from being copied, respectively.</span></span> <span data-ttu-id="7c0fc-256">Ha csak toocopy forrás erőforrásokat, amelyek nem léteznek a hello cél, mindkét paraméter hello AzCopy parancs adhat meg:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-256">If you only want toocopy source resources that don't exist in hello destination, you can specify both parameters in hello AzCopy command:</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

<span data-ttu-id="7c0fc-257">Vegye figyelembe, hogy ez nem támogatott esetén hello cél- és egy tábla.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-257">Note that this is not supported when either hello source or destination is a table.</span></span>

### <a name="use-a-response-file-toospecify-command-line-parameters"></a><span data-ttu-id="7c0fc-258">A válasz fájl toospecify parancssori paraméterekkel</span><span class="sxs-lookup"><span data-stu-id="7c0fc-258">Use a response file toospecify command-line parameters</span></span>

```azcopy
AzCopy /@:"C:\responsefiles\copyoperation.txt"
```

<span data-ttu-id="7c0fc-259">AzCopy parancssori paramétereket is megadhat a egy válaszfájlt.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-259">You can include any AzCopy command-line parameters in a response file.</span></span> <span data-ttu-id="7c0fc-260">AzCopy folyamatok hello hello fájlban paramétereket, mintha hello parancssorban közvetlen helyettesítés hello tartalma hello fájl végrehajtása meg lett.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-260">AzCopy processes hello parameters in hello file as if they had been specified on hello command line, performing a direct substitution with hello contents of hello file.</span></span>

<span data-ttu-id="7c0fc-261">Egy válaszfájl nevű feltételezik `copyoperation.txt`, amely tartalmazza a következő sorokat hello.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-261">Assume a response file named `copyoperation.txt`, that contains hello following lines.</span></span> <span data-ttu-id="7c0fc-262">Minden egyes AzCopy paraméter adható meg egy sorba</span><span class="sxs-lookup"><span data-stu-id="7c0fc-262">Each AzCopy parameter can be specified on a single line</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

<span data-ttu-id="7c0fc-263">vagy külön sorok:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-263">or on separate lines:</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

<span data-ttu-id="7c0fc-264">AzCopy sikertelen lesz, ha hello paraméter osztani két sort, ahogy az itt látható a hello `/sourcekey` paraméter:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-264">AzCopy will fail if you split hello parameter across two lines, as shown here for hello `/sourcekey` parameter:</span></span>

    http://myaccount.blob.core.windows.net/mycontainer
     C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-toospecify-command-line-parameters"></a><span data-ttu-id="7c0fc-265">Több válasz fájlok toospecify parancssori paraméterekkel</span><span class="sxs-lookup"><span data-stu-id="7c0fc-265">Use multiple response files toospecify command-line parameters</span></span>
<span data-ttu-id="7c0fc-266">Egy válaszfájl nevű feltételezik `source.txt` , amely megadja, hogy a forrás-tároló:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-266">Assume a response file named `source.txt` that specifies a source container:</span></span>

    /Source:http://myaccount.blob.core.windows.net/mycontainer

<span data-ttu-id="7c0fc-267">És egy válaszfájlt nevű `dest.txt` hello fájlrendszer, amely egy célmappát határozza meg:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-267">And a response file named `dest.txt` that specifies a destination folder in hello file system:</span></span>

    /Dest:C:\myfolder

<span data-ttu-id="7c0fc-268">És egy válaszfájlt nevű `options.txt` , amely megadja, hogy az AzCopy beállítások:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-268">And a response file named `options.txt` that specifies options for AzCopy:</span></span>

    /S /Y

<span data-ttu-id="7c0fc-269">AzCopy toocall a válasz fájlok, amelyek találhatók a könyvtárban található `C:\responsefiles`, használja ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-269">toocall AzCopy with these response files, all of which reside in a directory `C:\responsefiles`, use this command:</span></span>

```azcopy
AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   
```

<span data-ttu-id="7c0fc-270">AzCopy végrehajtja ezt a parancsot, ha minden egyes paraméterek hello hello parancssorban szerepeltetett volna:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-270">AzCopy processes this command just as it would if you included all of hello individual parameters on hello command line:</span></span>

```azcopy
AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y
```

### <a name="specify-a-shared-access-signature-sas"></a><span data-ttu-id="7c0fc-271">Adja meg a közös hozzáférésű jogosultságkód (SAS)</span><span class="sxs-lookup"><span data-stu-id="7c0fc-271">Specify a shared access signature (SAS)</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt
```

<span data-ttu-id="7c0fc-272">Azt is megadhatja a SAS hello tárolóra URI:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-272">You can also specify a SAS on hello container URI:</span></span>

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S
```

### <a name="journal-file-folder"></a><span data-ttu-id="7c0fc-273">Napló fájlmappa</span><span class="sxs-lookup"><span data-stu-id="7c0fc-273">Journal file folder</span></span>
<span data-ttu-id="7c0fc-274">Minden alkalommal, amikor egy parancs tooAzCopy kiadása ellenőrzi, hogy a napló fájl megtalálható-e hello alapértelmezett mappát, vagy hogy keresztül ez a beállítás a megadott mappa létezik.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-274">Each time you issue a command tooAzCopy, it checks whether a journal file exists in hello default folder, or whether it exists in a folder that you specified via this option.</span></span> <span data-ttu-id="7c0fc-275">Hello napló fájl nem létezik egyik helyen sem, ha AzCopy hello művelet új kezeli, és létrehoz egy új naplófájl.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-275">If hello journal file does not exist in either place, AzCopy treats hello operation as new and generates a new journal file.</span></span>

<span data-ttu-id="7c0fc-276">Ha hello napló fájl nem létezik, AzCopy ellenőrzi e megadott hello parancssori megegyezik-e hello parancssori hello napló fájlban.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-276">If hello journal file does exist, AzCopy will check whether hello command line that you input matches hello command line in hello journal file.</span></span> <span data-ttu-id="7c0fc-277">Hello két parancssorokat felel meg, ha az AzCopy hello hiányos művelet folytatása.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-277">If hello two command lines match, AzCopy resumes hello incomplete operation.</span></span> <span data-ttu-id="7c0fc-278">Ha nem egyeznek, fogja felszólító tooeither felülírása hello napló toostart új művelet, vagy toocancel hello aktuális műveletet.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-278">If they do not match, you will be prompted tooeither overwrite hello journal file toostart a new operation, or toocancel hello current operation.</span></span>

<span data-ttu-id="7c0fc-279">Ha azt szeretné, hogy toouse hello hello napló fájl alapértelmezett helye:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-279">If you want toouse hello default location for hello journal file:</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z
```

<span data-ttu-id="7c0fc-280">Ha a beállítás nincs megadva `/Z`, vagy adja meg a beállítás `/Z` nélkül hello mappa elérési útja, a fent látható AzCopy fájlt hoz létre hello napló hello alapértelmezett helyen, amely `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-280">If you omit option `/Z`, or specify option `/Z` without hello folder path, as shown above, AzCopy creates hello journal file in hello default location, which is `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span></span> <span data-ttu-id="7c0fc-281">Ha hello napló fájl már létezik, majd AzCopy folytatja hello művelet hello napló fájl alapján.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-281">If hello journal file already exists, then AzCopy resumes hello operation based on hello journal file.</span></span>

<span data-ttu-id="7c0fc-282">Ha azt szeretné, hogy egy egyéni helyet hello napló fájl toospecify:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-282">If you want toospecify a custom location for hello journal file:</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\
```

<span data-ttu-id="7c0fc-283">Ez a példa hello napló fájlt hoz létre, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-283">This example creates hello journal file if it does not already exist.</span></span> <span data-ttu-id="7c0fc-284">Ha létezik, majd AzCopy folytatja hello művelet hello napló fájl alapján.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-284">If it does exist, then AzCopy resumes hello operation based on hello journal file.</span></span>

<span data-ttu-id="7c0fc-285">Ha azt szeretné, hogy tooresume AzCopy művelet:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-285">If you want tooresume an AzCopy operation:</span></span>

```azcopy
AzCopy /Z:C:\journalfolder\
```

<span data-ttu-id="7c0fc-286">Ez a példa hello utolsó művelet, amely talán nem sikerült toocomplete folytatja.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-286">This example resumes hello last operation, which may have failed toocomplete.</span></span>

### <a name="generate-a-log-file"></a><span data-ttu-id="7c0fc-287">A naplófájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="7c0fc-287">Generate a log file</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V
```

<span data-ttu-id="7c0fc-288">Ha a beállítás megadja `/V` anélkül, hogy a fájl elérési útja toohello részletes naplózás, majd AzCopy hello naplófájl hello alapértelmezett a helyen hozza létre, amely `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-288">If you specify option `/V` without providing a file path toohello verbose log, then AzCopy creates hello log file in hello default location, which is `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.</span></span>

<span data-ttu-id="7c0fc-289">Ellenkező esetben egy naplófájlt is létrehozhat egy egyéni helyen:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-289">Otherwise, you can create an log file in a custom location:</span></span>

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log
```

<span data-ttu-id="7c0fc-290">Vegye figyelembe, hogy ha a beállítás a következő relatív elérési utat ad meg `/V`, például a `/V:test/azcopy1.log`, majd hello részletes napló jön létre az aktuális munkakönyvtárban hello belül egy almappát `test`.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-290">Note that if you specify a relative path following option `/V`, such as `/V:test/azcopy1.log`, then hello verbose log is created in hello current working directory within a subfolder named `test`.</span></span>

### <a name="specify-hello-number-of-concurrent-operations-toostart"></a><span data-ttu-id="7c0fc-291">Adja meg a hello száma párhuzamos műveletek toostart</span><span class="sxs-lookup"><span data-stu-id="7c0fc-291">Specify hello number of concurrent operations toostart</span></span>
<span data-ttu-id="7c0fc-292">A beállítás `/NC` hello egyidejű másolási műveletek számát adja meg.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-292">Option `/NC` specifies hello number of concurrent copy operations.</span></span> <span data-ttu-id="7c0fc-293">Alapértelmezés szerint az AzCopy elindul egy bizonyos száma párhuzamos műveletek átviteli tooincrease hello adatátvitelt.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-293">By default, AzCopy starts a certain number of concurrent operations tooincrease hello data transfer throughput.</span></span> <span data-ttu-id="7c0fc-294">A tábla műveleteket hello száma párhuzamos műveletek rendelkezik processzorok száma egyenlő toohello.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-294">For Table operations, hello number of concurrent operations is equal toohello number of processors you have.</span></span> <span data-ttu-id="7c0fc-295">A Blob és a fájl műveleteket, hello száma párhuzamos műveletek egyenlő 8 alkalommal hello számú processzorral rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-295">For Blob and File operations, hello number of concurrent operations is equal 8 times hello number of processors you have.</span></span> <span data-ttu-id="7c0fc-296">Ha AzCopy kis sávszélességű hálózaton keresztül futtatja, megadhatja a /NC tooavoid hibáját okozta. erőforrás verseny kevesebb.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-296">If you are running AzCopy across a low-bandwidth network, you can specify a lower number for /NC tooavoid failure caused by resource competition.</span></span>

### <a name="run-azcopy-against-azure-storage-emulator"></a><span data-ttu-id="7c0fc-297">AzCopy futtassa az Azure Storage emulatorban</span><span class="sxs-lookup"><span data-stu-id="7c0fc-297">Run AzCopy against Azure Storage Emulator</span></span>
<span data-ttu-id="7c0fc-298">AzCopy is futtathatók hello [Azure Storage Emulator](storage-use-emulator.md) a Blobok:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-298">You can run AzCopy against hello [Azure Storage Emulator](storage-use-emulator.md) for Blobs:</span></span>

```azcopy
AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S
```

<span data-ttu-id="7c0fc-299">és táblák:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-299">and Tables:</span></span>

```azcopy
AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table
```

## <a name="azcopy-parameters"></a><span data-ttu-id="7c0fc-300">AzCopy paraméterek</span><span class="sxs-lookup"><span data-stu-id="7c0fc-300">AzCopy Parameters</span></span>
<span data-ttu-id="7c0fc-301">AzCopy paramétereinek az alábbiakban található.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-301">Parameters for AzCopy are described below.</span></span> <span data-ttu-id="7c0fc-302">Egy hello parancsok parancssorból hello AzCopy használatával kapcsolatos segítséget a következő beírásával:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-302">You can also type one of hello following commands from hello command line for help in using AzCopy:</span></span>

* <span data-ttu-id="7c0fc-303">AzCopy részletes parancssori segítséget:`AzCopy /?`</span><span class="sxs-lookup"><span data-stu-id="7c0fc-303">For detailed command-line help for AzCopy: `AzCopy /?`</span></span>
* <span data-ttu-id="7c0fc-304">További információt az AzCopy paramétert:`AzCopy /?:SourceKey`</span><span class="sxs-lookup"><span data-stu-id="7c0fc-304">For detailed help with any AzCopy parameter: `AzCopy /?:SourceKey`</span></span>
* <span data-ttu-id="7c0fc-305">A parancssori példák:`AzCopy /?:Samples`</span><span class="sxs-lookup"><span data-stu-id="7c0fc-305">For command-line examples: `AzCopy /?:Samples`</span></span>

### <a name="sourcesource"></a><span data-ttu-id="7c0fc-306">/ Source: "forrás"</span><span class="sxs-lookup"><span data-stu-id="7c0fc-306">/Source:"source"</span></span>
<span data-ttu-id="7c0fc-307">Határozza meg, melyik toocopy hello forrásfájlok adatait.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-307">Specifies hello source data from which toocopy.</span></span> <span data-ttu-id="7c0fc-308">hello forrás lehet egy fájl rendszer könyvtár, egy blob-tároló, blob virtuális könyvtár, egy tárolófájl-megosztás, tároló fájl könyvtár, vagy egy Azure-tábla.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-308">hello source can be a file system directory, a blob container, a blob virtual directory, a storage file share, a storage file directory, or an Azure table.</span></span>

<span data-ttu-id="7c0fc-309">**Alkalmazandó:** blobokat, fájlok, táblák</span><span class="sxs-lookup"><span data-stu-id="7c0fc-309">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="destdestination"></a><span data-ttu-id="7c0fc-310">/ Cél: "cél"</span><span class="sxs-lookup"><span data-stu-id="7c0fc-310">/Dest:"destination"</span></span>
<span data-ttu-id="7c0fc-311">Megadja a hello cél toocopy számára.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-311">Specifies hello destination toocopy to.</span></span> <span data-ttu-id="7c0fc-312">hello cél lehet egy fájl rendszer könyvtár, egy blob-tároló, blob virtuális könyvtár, egy tárolófájl-megosztás, tároló fájl könyvtár, vagy egy Azure-tábla.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-312">hello destination can be a file system directory, a blob container, a blob virtual directory, a storage file share, a storage file directory, or an Azure table.</span></span>

<span data-ttu-id="7c0fc-313">**Alkalmazandó:** blobokat, fájlok, táblák</span><span class="sxs-lookup"><span data-stu-id="7c0fc-313">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="patternfile-pattern"></a><span data-ttu-id="7c0fc-314">/ Mintát: "fájl-minta"</span><span class="sxs-lookup"><span data-stu-id="7c0fc-314">/Pattern:"file-pattern"</span></span>
<span data-ttu-id="7c0fc-315">Adja meg a fájl mintát, amely azt jelzi, mely fájlok toocopy.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-315">Specifies a file pattern that indicates which files toocopy.</span></span> <span data-ttu-id="7c0fc-316">hello /Pattern paraméter hello viselkedését hello forrásadatok hello helyét, és hello jelenléte hello rekurzív mód beállítása határozza meg.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-316">hello behavior of hello /Pattern parameter is determined by hello location of hello source data, and hello presence of hello recursive mode option.</span></span> <span data-ttu-id="7c0fc-317">Rekurzív mód keresztül /s beállítás van megadva.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-317">Recursive mode is specified via option /S.</span></span>

<span data-ttu-id="7c0fc-318">Ha hello megadott forrás könyvtár hello fájlrendszer, akkor szokásos helyettesítő karakterek érvényesek, és hello fájl minta biztosította a rendszer megkeres hello könyvtárban lévő fájlokra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-318">If hello specified source is a directory in hello file system, then standard wildcards are in effect, and hello file pattern provided is matched against files within hello directory.</span></span> <span data-ttu-id="7c0fc-319">Ha beállítás /s kapcsoló meg van adva, majd AzCopy is megegyezik a megadott minta hello hello könyvtár alatt almappákban található összes fájl ellen.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-319">If option /S is specified, then AzCopy also matches hello specified pattern against all files in any subfolders beneath hello directory.</span></span>

<span data-ttu-id="7c0fc-320">Ha hello adott forrásból származnak. a blob-tároló vagy a virtuális könyvtár, majd helyettesítő karakterek nem érvényesek.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-320">If hello specified source is a blob container or virtual directory, then wildcards are not applied.</span></span> <span data-ttu-id="7c0fc-321">Ha a beállítás /s kapcsoló meg van adva, majd AzCopy hello megadott fájlminta blob előtagjaként értelmezi.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-321">If option /S is specified, then AzCopy interprets hello specified file pattern as a blob prefix.</span></span> <span data-ttu-id="7c0fc-322">Ha a beállítás /s kapcsoló nincs megadva, majd AzCopy megegyezik hello fájl mintával pontos blob nevekkel.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-322">If option /S is not specified, then AzCopy matches hello file pattern against exact blob names.</span></span>

<span data-ttu-id="7c0fc-323">Ha hello adott forrása az Azure fájlmegosztások, akkor meg kell adnia hello pontosan a fájl nevét (pl. abc.txt) toocopy egyetlen fájlban, vagy adja meg a beállítás /S toocopy minden fájl hello megosztás rekurzív módon.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-323">If hello specified source is an Azure file share, then you must either specify hello exact file name, (e.g. abc.txt) toocopy a single file, or specify option /S toocopy all files in hello share recursively.</span></span> <span data-ttu-id="7c0fc-324">Kísérlet toospecify egy fájl és egyaránt lehetőség /S együtt jár hiba.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-324">Attempting toospecify both a file pattern and option /S together will result in an error.</span></span>

<span data-ttu-id="7c0fc-325">AzCopy használja a kis-és nagybetűket megfelelő, ha hello/Source a blob-tároló vagy a blob virtuális könyvtár, és használja, azonban nem minden egyező hello más esetekben.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-325">AzCopy uses case-sensitive matching when hello /Source is a blob container or blob virtual directory, and uses case-insensitive matching in all hello other cases.</span></span>

<span data-ttu-id="7c0fc-326">hello alapértelmezett fájl használható, ha nincs fájl minta van megadva a mintája, *.*</span><span class="sxs-lookup"><span data-stu-id="7c0fc-326">hello default file pattern used when no file pattern is specified is *.*</span></span> <span data-ttu-id="7c0fc-327">egy olyan fájlhelyre rendszer vagy egy Azure Storage helyének egy üres előtag.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-327">for a file system location or an empty prefix for an Azure Storage location.</span></span> <span data-ttu-id="7c0fc-328">Több fájl minták megadása nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-328">Specifying multiple file patterns is not supported.</span></span>

<span data-ttu-id="7c0fc-329">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-329">**Applicable to:** Blobs, Files</span></span>

### <a name="destkeystorage-key"></a><span data-ttu-id="7c0fc-330">/ DestKey: "storage-kulcsot"</span><span class="sxs-lookup"><span data-stu-id="7c0fc-330">/DestKey:"storage-key"</span></span>
<span data-ttu-id="7c0fc-331">Megadja a hello tárfiók kulcsa hello cél erőforrás.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-331">Specifies hello storage account key for hello destination resource.</span></span>

<span data-ttu-id="7c0fc-332">**Alkalmazandó:** blobokat, fájlok, táblák</span><span class="sxs-lookup"><span data-stu-id="7c0fc-332">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="destsassas-token"></a><span data-ttu-id="7c0fc-333">/ DestSAS: "sas-token"</span><span class="sxs-lookup"><span data-stu-id="7c0fc-333">/DestSAS:"sas-token"</span></span>
<span data-ttu-id="7c0fc-334">Adja meg a közös hozzáférésű Jogosultságkód (SAS) hello cél OLVASÁSI és írási engedélyekkel (ha van ilyen).</span><span class="sxs-lookup"><span data-stu-id="7c0fc-334">Specifies a Shared Access Signature (SAS) with READ and WRITE permissions for hello destination (if applicable).</span></span> <span data-ttu-id="7c0fc-335">Az idézőjelek közé foglalt, SAS hello között legyen, tartalmaz, így előfordulhat, hogy speciális parancssori karaktereket.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-335">Surround hello SAS with double quotes, as it may contains special command-line characters.</span></span>

<span data-ttu-id="7c0fc-336">Ha hello cél erőforrás a blob-tároló, a fájlmegosztás vagy a táblát, vagy adja meg ezt a beállítást hello SAS-jogkivonat követ, vagy hello SAS hello cél blob tároló, a fájlmegosztás vagy a tábla URI, anélkül, hogy ez a beállítás részeként is megadhat.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-336">If hello destination resource is a blob container, file share or table, you can either specify this option followed by hello SAS token, or you can specify hello SAS as part of hello destination blob container, file share or table's URI, without this option.</span></span>

<span data-ttu-id="7c0fc-337">Ha hello forrás és cél mindkét blobokat, akkor hello cél blob kell lennie, hello belül ugyanaz, mint hello forrás blob storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-337">If hello source and destination are both blobs, then hello destination blob must reside within hello same storage account as hello source blob.</span></span>

<span data-ttu-id="7c0fc-338">**Alkalmazandó:** blobokat, fájlok, táblák</span><span class="sxs-lookup"><span data-stu-id="7c0fc-338">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="sourcekeystorage-key"></a><span data-ttu-id="7c0fc-339">/ SourceKey: "storage-kulcsot"</span><span class="sxs-lookup"><span data-stu-id="7c0fc-339">/SourceKey:"storage-key"</span></span>
<span data-ttu-id="7c0fc-340">Megadja a hello tárfiók kulcsa hello forrás erőforrás.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-340">Specifies hello storage account key for hello source resource.</span></span>

<span data-ttu-id="7c0fc-341">**Alkalmazandó:** blobokat, fájlok, táblák</span><span class="sxs-lookup"><span data-stu-id="7c0fc-341">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="sourcesassas-token"></a><span data-ttu-id="7c0fc-342">/ SourceSAS: "sas-token"</span><span class="sxs-lookup"><span data-stu-id="7c0fc-342">/SourceSAS:"sas-token"</span></span>
<span data-ttu-id="7c0fc-343">Megadja egy közös hozzáférésű Jogosultságkód és lista engedélyt hello forrás (ha van ilyen).</span><span class="sxs-lookup"><span data-stu-id="7c0fc-343">Specifies a Shared Access Signature with READ and LIST permissions for hello source (if applicable).</span></span> <span data-ttu-id="7c0fc-344">Az idézőjelek közé foglalt, SAS hello között legyen, tartalmaz, így előfordulhat, hogy speciális parancssori karaktereket.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-344">Surround hello SAS with double quotes, as it may contains special command-line characters.</span></span>

<span data-ttu-id="7c0fc-345">Ha hello forráserőforrásnak egy blob-tároló, és nem egy kulcs, és SAS-kód nem áll, majd hello blob tároló fogja beolvasni a névtelen hozzáférés keresztül.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-345">If hello source resource is a blob container, and neither a key nor a SAS is provided, then hello blob container will be read via anonymous access.</span></span>

<span data-ttu-id="7c0fc-346">Ha hello forrás egy fájlmegosztás vagy a tábla, meg kell adni egy kulcs- vagy SAS-kód.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-346">If hello source is a file share or table, a key or a SAS must be provided.</span></span>

<span data-ttu-id="7c0fc-347">**Alkalmazandó:** blobokat, fájlok, táblák</span><span class="sxs-lookup"><span data-stu-id="7c0fc-347">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="s"></a><span data-ttu-id="7c0fc-348">/ S</span><span class="sxs-lookup"><span data-stu-id="7c0fc-348">/S</span></span>
<span data-ttu-id="7c0fc-349">Meghatározza a másolási műveletek rekurzív módját.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-349">Specifies recursive mode for copy operations.</span></span> <span data-ttu-id="7c0fc-350">Rekurzív módban AzCopy másolatot készít az összes BLOB vagy fájlokat, amelyek megfelelnek a megadott fájl hello mintát, almappák lévőket is beleértve.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-350">In recursive mode, AzCopy will copy all blobs or files that match hello specified file pattern, including those in subfolders.</span></span>

<span data-ttu-id="7c0fc-351">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-351">**Applicable to:** Blobs, Files</span></span>

### <a name="blobtypeblock--page--append"></a><span data-ttu-id="7c0fc-352">/ BlobType: "block" |} "lap" |} "append"</span><span class="sxs-lookup"><span data-stu-id="7c0fc-352">/BlobType:"block" | "page" | "append"</span></span>
<span data-ttu-id="7c0fc-353">Megadja, hogy hello cél blob egy blokkblob, oldalakra vonatkozó blob vagy hozzáfűző blob.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-353">Specifies whether hello destination blob is a block blob, a page blob, or an append blob.</span></span> <span data-ttu-id="7c0fc-354">Ez a beállítás csak egy blob feltölteni kívánt esetén alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-354">This option is applicable only when you are uploading a blob.</span></span> <span data-ttu-id="7c0fc-355">Ellenkező esetben hiba történik.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-355">Otherwise, an error is generated.</span></span> <span data-ttu-id="7c0fc-356">Ha hello cél blob, és ez a beállítás nincs megadva, alapértelmezés szerint az AzCopy egy blokkblob hoz létre.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-356">If hello destination is a blob and this option is not specified, by default, AzCopy creates a block blob.</span></span>

<span data-ttu-id="7c0fc-357">**Alkalmazandó:** Blobok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-357">**Applicable to:** Blobs</span></span>

### <a name="checkmd5"></a><span data-ttu-id="7c0fc-358">/ CheckMD5</span><span class="sxs-lookup"><span data-stu-id="7c0fc-358">/CheckMD5</span></span>
<span data-ttu-id="7c0fc-359">Kiszámítja az MD5 kivonatoló letöltött adatok, és ellenőrzi, hogy hello MD5 kivonatoló hello blob tárolja, vagy a fájl Content-MD5 tulajdonsága egyezést mutat a hello kiszámítása kivonatát.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-359">Calculates an MD5 hash for downloaded data and verifies that hello MD5 hash stored in hello blob or file's Content-MD5 property matches hello calculated hash.</span></span> <span data-ttu-id="7c0fc-360">hello MD5 ellenőrzése ki van kapcsolva alapértelmezés szerint, az adatok letöltése során meg kell adnia ezt a beállítást tooperform hello MD5 az ellenőrzést.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-360">hello MD5 check is turned off by default, so you must specify this option tooperform hello MD5 check when downloading data.</span></span>

<span data-ttu-id="7c0fc-361">Vegye figyelembe, hogy Azure Storage nem garantálja, hogy hello MD5 kivonatoló hello BLOB tárolja, vagy a fájl naprakész.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-361">Note that Azure Storage doesn't guarantee that hello MD5 hash stored for hello blob or file is up-to-date.</span></span> <span data-ttu-id="7c0fc-362">Amikor hello blob vagy a fájl módosítása ügyfél felelősségi tooupdate hello MD5.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-362">It is client's responsibility tooupdate hello MD5 whenever hello blob or file is modified.</span></span>

<span data-ttu-id="7c0fc-363">AzCopy mindig hello Content-MD5 tulajdonság beállítása az Azure blob vagy a fájl után ismét feltölteni a toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-363">AzCopy always sets hello Content-MD5 property for an Azure blob or file after uploading it toohello service.</span></span>  

<span data-ttu-id="7c0fc-364">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-364">**Applicable to:** Blobs, Files</span></span>

### <a name="snapshot"></a><span data-ttu-id="7c0fc-365">Vagy pillanatkép</span><span class="sxs-lookup"><span data-stu-id="7c0fc-365">/Snapshot</span></span>
<span data-ttu-id="7c0fc-366">Azt jelzi, hogy tootransfer pillanatképeket.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-366">Indicates whether tootransfer snapshots.</span></span> <span data-ttu-id="7c0fc-367">Ez a beállítás csak akkor érvényes, ha egy blob hello forrás.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-367">This option is only valid when hello source is a blob.</span></span>

<span data-ttu-id="7c0fc-368">hello átvitt blob pillanatképek átnevezi a következő formátumban: .extension blob-név (pillanatkép-time)</span><span class="sxs-lookup"><span data-stu-id="7c0fc-368">hello transferred blob snapshots are renamed in this format: blob-name (snapshot-time).extension</span></span>

<span data-ttu-id="7c0fc-369">Alapértelmezés szerint a pillanatképek nem kerülnek.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-369">By default, snapshots are not copied.</span></span>

<span data-ttu-id="7c0fc-370">**Alkalmazandó:** Blobok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-370">**Applicable to:** Blobs</span></span>

### <a name="vverbose-log-file"></a><span data-ttu-id="7c0fc-371">/ V: [részletes-naplófájl]</span><span class="sxs-lookup"><span data-stu-id="7c0fc-371">/V:[verbose-log-file]</span></span>
<span data-ttu-id="7c0fc-372">Kimeneti részletes üzenetek fájlba.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-372">Outputs verbose status messages into a log file.</span></span>

<span data-ttu-id="7c0fc-373">Alapértelmezés szerint hello részletes naplófájl neve a AzCopyVerbose.log `%LocalAppData%\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-373">By default, hello verbose log file is named AzCopyVerbose.log in `%LocalAppData%\Microsoft\Azure\AzCopy`.</span></span> <span data-ttu-id="7c0fc-374">Ha megadja ezt a lehetőséget egy meglévő fájl helyét, hello részletes napló lesz hozzáfűzött toothat fájl.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-374">If you specify an existing file location for this option, hello verbose log will be appended toothat file.</span></span>  

<span data-ttu-id="7c0fc-375">**Alkalmazandó:** blobokat, fájlok, táblák</span><span class="sxs-lookup"><span data-stu-id="7c0fc-375">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="zjournal-file-folder"></a><span data-ttu-id="7c0fc-376">/ Z: [napló-fájlok és mappák]</span><span class="sxs-lookup"><span data-stu-id="7c0fc-376">/Z:[journal-file-folder]</span></span>
<span data-ttu-id="7c0fc-377">A művelet folytatása napló fájl mappáját adja meg.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-377">Specifies a journal file folder for resuming an operation.</span></span>

<span data-ttu-id="7c0fc-378">AzCopy mindig támogatja a Folytatás, ha egy művelet megszakadt.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-378">AzCopy always supports resuming if an operation has been interrupted.</span></span>

<span data-ttu-id="7c0fc-379">Ez a beállítás nincs megadva, vagy egy mappa elérési útját nélkül van megadva, majd AzCopy fog létrehozni hello naplófájl hello alapértelmezett helye a % LocalAppData%\Microsoft\Azure\AzCopy.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-379">If this option is not specified, or it is specified without a folder path, then AzCopy will create hello journal file in hello default location, which is %LocalAppData%\Microsoft\Azure\AzCopy.</span></span>

<span data-ttu-id="7c0fc-380">Minden alkalommal, amikor egy parancs tooAzCopy kiadása ellenőrzi, hogy a napló fájl megtalálható-e hello alapértelmezett mappát, vagy hogy keresztül ez a beállítás a megadott mappa létezik.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-380">Each time you issue a command tooAzCopy, it checks whether a journal file exists in hello default folder, or whether it exists in a folder that you specified via this option.</span></span> <span data-ttu-id="7c0fc-381">Hello napló fájl nem létezik egyik helyen sem, ha AzCopy hello művelet új kezeli, és létrehoz egy új naplófájl.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-381">If hello journal file does not exist in either place, AzCopy treats hello operation as new and generates a new journal file.</span></span>

<span data-ttu-id="7c0fc-382">Ha hello napló fájl nem létezik, AzCopy ellenőrzi e megadott hello parancssori megegyezik-e hello parancssori hello napló fájlban.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-382">If hello journal file does exist, AzCopy will check whether hello command line that you input matches hello command line in hello journal file.</span></span> <span data-ttu-id="7c0fc-383">Hello két parancssorokat felel meg, ha az AzCopy hello hiányos művelet folytatása.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-383">If hello two command lines match, AzCopy resumes hello incomplete operation.</span></span> <span data-ttu-id="7c0fc-384">Ha nem egyeznek, fogja felszólító tooeither felülírása hello napló toostart új művelet, vagy toocancel hello aktuális műveletet.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-384">If they do not match, you will be prompted tooeither overwrite hello journal file toostart a new operation, or toocancel hello current operation.</span></span>

<span data-ttu-id="7c0fc-385">hello napló fájl hello művelet sikeres befejezését követően törlődik.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-385">hello journal file is deleted upon successful completion of hello operation.</span></span>

<span data-ttu-id="7c0fc-386">Vegye figyelembe, hogy a Folytatás, az AzCopy egy korábbi verziójával létrehozott napló fájlból egy művelet nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-386">Note that resuming an operation from a journal file created by a previous version of AzCopy is not supported.</span></span>

<span data-ttu-id="7c0fc-387">**Alkalmazandó:** blobokat, fájlok, táblák</span><span class="sxs-lookup"><span data-stu-id="7c0fc-387">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="parameter-file"></a><span data-ttu-id="7c0fc-388">/@:"Parameter-File"</span><span class="sxs-lookup"><span data-stu-id="7c0fc-388">/@:"parameter-file"</span></span>
<span data-ttu-id="7c0fc-389">Megadja a paramétereket tartalmazó fájlt.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-389">Specifies a file that contains parameters.</span></span> <span data-ttu-id="7c0fc-390">AzCopy folyamatok hello hello fájlban paramétereket, mintha csak hello parancssorban megadott lett.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-390">AzCopy processes hello parameters in hello file just as if they had been specified on hello command line.</span></span>

<span data-ttu-id="7c0fc-391">A válaszfájlban több paramétert meg egyetlen vonal, vagy adja meg az egyes paramétereket külön sorban tüntettük.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-391">In a response file, you can either specify multiple parameters on a single line, or specify each parameter on its own line.</span></span> <span data-ttu-id="7c0fc-392">Vegye figyelembe, hogy az egyes paraméter nem terjedhetnek többsoros.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-392">Note that an individual parameter cannot span multiple lines.</span></span>

<span data-ttu-id="7c0fc-393">Válasz tartalmazhatnak hello # karakterrel kezdődő sorok megjegyzések.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-393">Response files can include comments lines that begin with hello # symbol.</span></span>

<span data-ttu-id="7c0fc-394">Válasz több fájl is megadható.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-394">You can specify multiple response files.</span></span> <span data-ttu-id="7c0fc-395">Vegye figyelembe azonban, hogy az AzCopy nem támogatja a beágyazott válaszfájlok.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-395">However, note that AzCopy does not support nested response files.</span></span>

<span data-ttu-id="7c0fc-396">**Alkalmazandó:** blobokat, fájlok, táblák</span><span class="sxs-lookup"><span data-stu-id="7c0fc-396">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="y"></a><span data-ttu-id="7c0fc-397">/Y</span><span class="sxs-lookup"><span data-stu-id="7c0fc-397">/Y</span></span>
<span data-ttu-id="7c0fc-398">Letiltja az összes AzCopy megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-398">Suppresses all AzCopy confirmation prompts.</span></span>

<span data-ttu-id="7c0fc-399">**Alkalmazandó:** blobokat, fájlok, táblák</span><span class="sxs-lookup"><span data-stu-id="7c0fc-399">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="l"></a><span data-ttu-id="7c0fc-400">/ L</span><span class="sxs-lookup"><span data-stu-id="7c0fc-400">/L</span></span>
<span data-ttu-id="7c0fc-401">Megadja a listázási művelet csak; adatot nem másolódik.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-401">Specifies a listing operation only; no data is copied.</span></span>

<span data-ttu-id="7c0fc-402">AzCopy értelmezi hello segítségével ezt a beállítást a szimuláció futó hello parancssor e nélkül az/l és száma, hogy hány objektumok kerülnek beállításnál megadhatja a beállítást, hello /V azonos idő toocheck, mely objektumok hello részletes naplóba kerülnek.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-402">AzCopy will interpret hello using of this option as a simulation for running hello command line without this option /L and count how many objects will be copied, you can specify option /V at hello same time toocheck which objects will be copied in hello verbose log.</span></span>

<span data-ttu-id="7c0fc-403">Ez a beállítás viselkedése hello is határozza meg hello forrásadatok hello helye és hello rekurzív mód beállítás/s és a fájl minta beállítás /Pattern hello jelenlétét.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-403">hello behavior of this option is also determined by hello location of hello source data and hello presence of hello recursive mode option /S and file pattern option /Pattern.</span></span>

<span data-ttu-id="7c0fc-404">AzCopy, ez a hely LISTÁJÁT, és OLVASÁSI engedélyre van szüksége, ez a beállítás használata esetén.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-404">AzCopy requires LIST and READ permission of this source location when using this option.</span></span>

<span data-ttu-id="7c0fc-405">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-405">**Applicable to:** Blobs, Files</span></span>

### <a name="mt"></a><span data-ttu-id="7c0fc-406">/MT</span><span class="sxs-lookup"><span data-stu-id="7c0fc-406">/MT</span></span>
<span data-ttu-id="7c0fc-407">Beállítja a hello letöltött fájl utolsó módosításának ideje toobe hello azonos hello forrás blob vagy fájl.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-407">Sets hello downloaded file's last-modified time toobe hello same as hello source blob or file's.</span></span>

<span data-ttu-id="7c0fc-408">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-408">**Applicable to:** Blobs, Files</span></span>

### <a name="xn"></a><span data-ttu-id="7c0fc-409">/XN</span><span class="sxs-lookup"><span data-stu-id="7c0fc-409">/XN</span></span>
<span data-ttu-id="7c0fc-410">Nem tartalmazza egy újabb forráserőforrásnak.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-410">Excludes a newer source resource.</span></span> <span data-ttu-id="7c0fc-411">hello erőforrás nem kerülnek, ha hello hello forrás utolsó módosítási időpontjának hello azonos vagy újabb, mint a cél.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-411">hello resource will not be copied if hello last modified time of hello source is hello same or newer than destination.</span></span>

<span data-ttu-id="7c0fc-412">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-412">**Applicable to:** Blobs, Files</span></span>

### <a name="xo"></a><span data-ttu-id="7c0fc-413">/XO</span><span class="sxs-lookup"><span data-stu-id="7c0fc-413">/XO</span></span>
<span data-ttu-id="7c0fc-414">Nem tartalmazza egy régebbi forráserőforrásnak.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-414">Excludes an older source resource.</span></span> <span data-ttu-id="7c0fc-415">hello erőforrás nem kerülnek, ha hello hello forrás utolsó módosítási időpontjának hello azonos vagy régebbi, mint a cél.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-415">hello resource will not be copied if hello last modified time of hello source is hello same or older than destination.</span></span>

<span data-ttu-id="7c0fc-416">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-416">**Applicable to:** Blobs, Files</span></span>

### <a name="a"></a><span data-ttu-id="7c0fc-417">/A</span><span class="sxs-lookup"><span data-stu-id="7c0fc-417">/A</span></span>
<span data-ttu-id="7c0fc-418">Csak a hello Archiválandó fájlok feltöltését.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-418">Uploads only files that have hello Archive attribute set.</span></span>

<span data-ttu-id="7c0fc-419">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-419">**Applicable to:** Blobs, Files</span></span>

### <a name="iarashcnetoi"></a><span data-ttu-id="7c0fc-420">/ IA: [RASHCNETOI]</span><span class="sxs-lookup"><span data-stu-id="7c0fc-420">/IA:[RASHCNETOI]</span></span>
<span data-ttu-id="7c0fc-421">Feltöltések csak a fájlok hello bármelyike megadott attribútumok beállítása.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-421">Uploads only files that have any of hello specified attributes set.</span></span>

<span data-ttu-id="7c0fc-422">A rendelkezésre álló attribútumok a következők:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-422">Available attributes include:</span></span>

* <span data-ttu-id="7c0fc-423">R = csak olvasható fájlokat</span><span class="sxs-lookup"><span data-stu-id="7c0fc-423">R = Read-only files</span></span>
* <span data-ttu-id="7c0fc-424">A = archiválásra kész</span><span class="sxs-lookup"><span data-stu-id="7c0fc-424">A = Files ready for archiving</span></span>
* <span data-ttu-id="7c0fc-425">S rendszerfájlok =</span><span class="sxs-lookup"><span data-stu-id="7c0fc-425">S = System files</span></span>
* <span data-ttu-id="7c0fc-426">H = Rejtett fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-426">H = Hidden files</span></span>
* <span data-ttu-id="7c0fc-427">C = tömörített fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-427">C = Compressed files</span></span>
* <span data-ttu-id="7c0fc-428">N = normál fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-428">N = Normal files</span></span>
* <span data-ttu-id="7c0fc-429">E titkosított fájlok =</span><span class="sxs-lookup"><span data-stu-id="7c0fc-429">E = Encrypted files</span></span>
* <span data-ttu-id="7c0fc-430">T ideiglenes fájlok =</span><span class="sxs-lookup"><span data-stu-id="7c0fc-430">T = Temporary files</span></span>
* <span data-ttu-id="7c0fc-431">O = Offline fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-431">O = Offline files</span></span>
* <span data-ttu-id="7c0fc-432">I = nem indexelt fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-432">I = Non-indexed files</span></span>

<span data-ttu-id="7c0fc-433">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-433">**Applicable to:** Blobs, Files</span></span>

### <a name="xarashcnetoi"></a><span data-ttu-id="7c0fc-434">/ XA: [RASHCNETOI]</span><span class="sxs-lookup"><span data-stu-id="7c0fc-434">/XA:[RASHCNETOI]</span></span>
<span data-ttu-id="7c0fc-435">Nem tartalmazza a fájlra, amely bármelyik megadott hello attribútumok beállítása.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-435">Excludes files that have any of hello specified attributes set.</span></span>

<span data-ttu-id="7c0fc-436">A rendelkezésre álló attribútumok a következők:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-436">Available attributes include:</span></span>

* <span data-ttu-id="7c0fc-437">R = csak olvasható fájlokat</span><span class="sxs-lookup"><span data-stu-id="7c0fc-437">R = Read-only files</span></span>
* <span data-ttu-id="7c0fc-438">A = archiválásra kész</span><span class="sxs-lookup"><span data-stu-id="7c0fc-438">A = Files ready for archiving</span></span>
* <span data-ttu-id="7c0fc-439">S rendszerfájlok =</span><span class="sxs-lookup"><span data-stu-id="7c0fc-439">S = System files</span></span>
* <span data-ttu-id="7c0fc-440">H = Rejtett fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-440">H = Hidden files</span></span>
* <span data-ttu-id="7c0fc-441">C = tömörített fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-441">C = Compressed files</span></span>
* <span data-ttu-id="7c0fc-442">N = normál fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-442">N = Normal files</span></span>
* <span data-ttu-id="7c0fc-443">E titkosított fájlok =</span><span class="sxs-lookup"><span data-stu-id="7c0fc-443">E = Encrypted files</span></span>
* <span data-ttu-id="7c0fc-444">T ideiglenes fájlok =</span><span class="sxs-lookup"><span data-stu-id="7c0fc-444">T = Temporary files</span></span>
* <span data-ttu-id="7c0fc-445">O = Offline fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-445">O = Offline files</span></span>
* <span data-ttu-id="7c0fc-446">I = nem indexelt fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-446">I = Non-indexed files</span></span>

<span data-ttu-id="7c0fc-447">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-447">**Applicable to:** Blobs, Files</span></span>

### <a name="delimiterdelimiter"></a><span data-ttu-id="7c0fc-448">/ Elválasztó karakter: "elválasztót".</span><span class="sxs-lookup"><span data-stu-id="7c0fc-448">/Delimiter:"delimiter"</span></span>
<span data-ttu-id="7c0fc-449">Azt jelzi, hello elválasztó karakter egy blob neve toodelimit virtuális könyvtárak használni.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-449">Indicates hello delimiter character used toodelimit virtual directories in a blob name.</span></span>

<span data-ttu-id="7c0fc-450">Alapértelmezés szerint az AzCopy által használt / hello elválasztó karakterként.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-450">By default, AzCopy uses / as hello delimiter character.</span></span> <span data-ttu-id="7c0fc-451">Azonban AzCopy támogatja a közös karakter használatát (például a @, #, vagy %) elválasztó.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-451">However, AzCopy supports using any common character (such as @, #, or %) as a delimiter.</span></span> <span data-ttu-id="7c0fc-452">Ha egy, a következő különleges karakterek hello parancssorban tooinclude van szüksége, tegye a hello fájlnevet idézőjelekkel együtt.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-452">If you need tooinclude one of these special characters on hello command line, enclose hello file name with double quotes.</span></span>

<span data-ttu-id="7c0fc-453">Ez a beállítás csak akkor alkalmazható blobok letöltése.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-453">This option is only applicable for downloading blobs.</span></span>

<span data-ttu-id="7c0fc-454">**Alkalmazandó:** Blobok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-454">**Applicable to:** Blobs</span></span>

### <a name="ncnumber-of-concurrent-operations"></a><span data-ttu-id="7c0fc-455">/ NC: "számot-az-egyidejű-műveletek"</span><span class="sxs-lookup"><span data-stu-id="7c0fc-455">/NC:"number-of-concurrent-operations"</span></span>
<span data-ttu-id="7c0fc-456">Hello párhuzamos műveletek számát adja meg.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-456">Specifies hello number of concurrent operations.</span></span>

<span data-ttu-id="7c0fc-457">AzCopy alapértelmezés szerint elindul egy bizonyos száma párhuzamos műveletek átviteli tooincrease hello adatátvitelt.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-457">AzCopy by default starts a certain number of concurrent operations tooincrease hello data transfer throughput.</span></span> <span data-ttu-id="7c0fc-458">Ne feledje, hogy nagy száma párhuzamos műveletek alacsony sávszélességű környezetben előfordulhat, hogy ne terhelje tovább hello hálózati kapcsolat teljes befejezését hello műveletek tiltása.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-458">Note that large number of concurrent operations in a low-bandwidth environment may overwhelm hello network connection and prevent hello operations from fully completing.</span></span> <span data-ttu-id="7c0fc-459">Párhuzamos műveletek alapján tényleges rendelkezésre álló hálózati sávszélesség szabályozását.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-459">Throttle concurrent operations based on actual available network bandwidth.</span></span>

<span data-ttu-id="7c0fc-460">hello felső párhuzamos műveletek határa 512.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-460">hello upper limit for concurrent operations is 512.</span></span>

<span data-ttu-id="7c0fc-461">**Alkalmazandó:** blobokat, fájlok, táblák</span><span class="sxs-lookup"><span data-stu-id="7c0fc-461">**Applicable to:** Blobs, Files, Tables</span></span>

### <a name="sourcetypeblob--table"></a><span data-ttu-id="7c0fc-462">/ Forrástípus: "Blob" |} "Table"</span><span class="sxs-lookup"><span data-stu-id="7c0fc-462">/SourceType:"Blob" | "Table"</span></span>
<span data-ttu-id="7c0fc-463">Meghatározza, hogy hello `source` erőforrás áll rendelkezésre hello helyi fejlesztői környezetben futó hello storage emulator blob.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-463">Specifies that hello `source` resource is a blob available in hello local development environment, running in hello storage emulator.</span></span>

<span data-ttu-id="7c0fc-464">**Alkalmazandó:** Blobok, táblák</span><span class="sxs-lookup"><span data-stu-id="7c0fc-464">**Applicable to:** Blobs, Tables</span></span>

### <a name="desttypeblob--table"></a><span data-ttu-id="7c0fc-465">/ DestType: "Blob" |} "Table"</span><span class="sxs-lookup"><span data-stu-id="7c0fc-465">/DestType:"Blob" | "Table"</span></span>
<span data-ttu-id="7c0fc-466">Meghatározza, hogy hello `destination` erőforrás áll rendelkezésre hello helyi fejlesztői környezetben futó hello storage emulator blob.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-466">Specifies that hello `destination` resource is a blob available in hello local development environment, running in hello storage emulator.</span></span>

<span data-ttu-id="7c0fc-467">**Alkalmazandó:** Blobok, táblák</span><span class="sxs-lookup"><span data-stu-id="7c0fc-467">**Applicable to:** Blobs, Tables</span></span>

### <a name="pkrskey1key2key3"></a><span data-ttu-id="7c0fc-468">/ PKRS: "key&#1;key&#2;key&#3;..."</span><span class="sxs-lookup"><span data-stu-id="7c0fc-468">/PKRS:"key1#key2#key3#..."</span></span>
<span data-ttu-id="7c0fc-469">Elágazást hello partíciós kulcs tartomány tooenable tábla adatexportálási párhuzamosan, ami növeli a hello az exportálási művelet hello sebességét.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-469">Splits hello partition key range tooenable exporting table data in parallel, which increases hello speed of hello export operation.</span></span>

<span data-ttu-id="7c0fc-470">Ha ez a beállítás nincs megadva, az AzCopy egy egyetlen szálon tooexport táblaentitásokat használja.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-470">If this option is not specified, then AzCopy uses a single thread tooexport table entities.</span></span> <span data-ttu-id="7c0fc-471">Például ha hello felhasználó határozza meg a /PKRS: "aa #bb", akkor az AzCopy három párhuzamos műveletek kezdődik.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-471">For example, if hello user specifies /PKRS:"aa#bb", then AzCopy starts three concurrent operations.</span></span>

<span data-ttu-id="7c0fc-472">Egyes műveletek exportálja egy három partíció kulcstartományokkal, alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-472">Each operation exports one of three partition key ranges, as shown below:</span></span>

  <span data-ttu-id="7c0fc-473">[első partíciókulcs, aa)</span><span class="sxs-lookup"><span data-stu-id="7c0fc-473">[first-partition-key, aa)</span></span>

  <span data-ttu-id="7c0fc-474">[aa, bb)</span><span class="sxs-lookup"><span data-stu-id="7c0fc-474">[aa, bb)</span></span>

  <span data-ttu-id="7c0fc-475">[bb, utolsó partíciókulcs]</span><span class="sxs-lookup"><span data-stu-id="7c0fc-475">[bb, last-partition-key]</span></span>

<span data-ttu-id="7c0fc-476">**Alkalmazandó:** táblák</span><span class="sxs-lookup"><span data-stu-id="7c0fc-476">**Applicable to:** Tables</span></span>

### <a name="splitsizefile-size"></a><span data-ttu-id="7c0fc-477">/ SplitSize: "fájl mérete"</span><span class="sxs-lookup"><span data-stu-id="7c0fc-477">/SplitSize:"file-size"</span></span>
<span data-ttu-id="7c0fc-478">Megadja az exportált fájl mérete MB vágási hello, hello minimális engedélyezett értéke 32.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-478">Specifies hello exported file split size in MB, hello minimal value allowed is 32.</span></span>

<span data-ttu-id="7c0fc-479">Ha ez a beállítás nincs megadva, AzCopy tábla toosingle adatfájl exportálja.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-479">If this option is not specified, AzCopy will export table data toosingle file.</span></span>

<span data-ttu-id="7c0fc-480">Ha hello tábla adatai exportált tooa blob, és hello exportált fájl mérete eléri a következőt hello 200 GB-os korlát blob mérete, majd a AzCopy az hello exportált fájlt, osztott, még akkor is, ha ez a beállítás nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-480">If hello table data is exported tooa blob, and hello exported file size reaches hello 200 GB limit for blob size, then AzCopy will split hello exported file, even if this option is not specified.</span></span>

<span data-ttu-id="7c0fc-481">**Alkalmazandó:** táblák</span><span class="sxs-lookup"><span data-stu-id="7c0fc-481">**Applicable to:** Tables</span></span>

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a><span data-ttu-id="7c0fc-482">/ EntityOperation: "InsertOrSkip" |} "InsertOrMerge" |} "InsertOrReplace"</span><span class="sxs-lookup"><span data-stu-id="7c0fc-482">/EntityOperation:"InsertOrSkip" | "InsertOrMerge" | "InsertOrReplace"</span></span>
<span data-ttu-id="7c0fc-483">Megadja a hello tábla importálása működéshez.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-483">Specifies hello table data import behavior.</span></span>

* <span data-ttu-id="7c0fc-484">InsertOrSkip - kihagyja a meglévő entitás vagy szúr be egy új entitást, ha hello tábla nem létezik.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-484">InsertOrSkip - Skips an existing entity or inserts a new entity if it does not exist in hello table.</span></span>
* <span data-ttu-id="7c0fc-485">InsertOrMerge - egyesíti a meglévő entitás vagy szúr be egy új entitást, ha hello tábla nem létezik.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-485">InsertOrMerge - Merges an existing entity or inserts a new entity if it does not exist in hello table.</span></span>
* <span data-ttu-id="7c0fc-486">InsertOrReplace - lecseréli a meglévő entitás vagy szúr be egy új entitást, ha hello tábla nem létezik.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-486">InsertOrReplace - Replaces an existing entity or inserts a new entity if it does not exist in hello table.</span></span>

<span data-ttu-id="7c0fc-487">**Alkalmazandó:** táblák</span><span class="sxs-lookup"><span data-stu-id="7c0fc-487">**Applicable to:** Tables</span></span>

### <a name="manifestmanifest-file"></a><span data-ttu-id="7c0fc-488">/ Jegyzékfájl: "jegyzékfájl-file"</span><span class="sxs-lookup"><span data-stu-id="7c0fc-488">/Manifest:"manifest-file"</span></span>
<span data-ttu-id="7c0fc-489">Adja meg a jegyzékfájl hello hello tábla exportálása, az importálási művelet.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-489">Specifies hello manifest file for hello table export and import operation.</span></span>

<span data-ttu-id="7c0fc-490">Ez a beállítás akkor választható hello az exportálási művelet során, AzCopy előre definiált nevű jegyzékfájlt hoz létre, ha ez a beállítás nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-490">This option is optional during hello export operation, AzCopy will generate a manifest file with predefined name if this option is not specified.</span></span>

<span data-ttu-id="7c0fc-491">Ez a beállítás akkor szükséges hello adatfájlok helyének hello importálási művelet során.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-491">This option is required during hello import operation for locating hello data files.</span></span>

<span data-ttu-id="7c0fc-492">**Alkalmazandó:** táblák</span><span class="sxs-lookup"><span data-stu-id="7c0fc-492">**Applicable to:** Tables</span></span>

### <a name="synccopy"></a><span data-ttu-id="7c0fc-493">/ SyncCopy</span><span class="sxs-lookup"><span data-stu-id="7c0fc-493">/SyncCopy</span></span>
<span data-ttu-id="7c0fc-494">Azt jelzi, hogy toosynchronously másolása blobokkal vagy a fájlokat két Azure Storage-végpontok közötti.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-494">Indicates whether toosynchronously copy blobs or files between two Azure Storage endpoints.</span></span>

<span data-ttu-id="7c0fc-495">Alapértelmezés szerint AzCopy kiszolgálóoldali aszinkron másolatot használja.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-495">AzCopy by default uses server-side asynchronous copy.</span></span> <span data-ttu-id="7c0fc-496">Adja meg ezt a beállítást egy szinkron tooperform másolja, amely letölti a blobokat vagy toolocal memória fájlok és majd feltölti tooAzure tároló.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-496">Specify this option tooperform a synchronous copy, which downloads blobs or files toolocal memory and then uploads them tooAzure Storage.</span></span>

<span data-ttu-id="7c0fc-497">Ezt a beállítást is használhatja, amikor belül Blob-tároló, a File storage belül, vagy a Blob storage tooFile tároló- és fordítva fordítva a fájlok másolása.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-497">You can use this option when copying files within Blob storage, within File storage, or from Blob storage tooFile storage or vice versa.</span></span>

<span data-ttu-id="7c0fc-498">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-498">**Applicable to:** Blobs, Files</span></span>

### <a name="setcontenttypecontent-type"></a><span data-ttu-id="7c0fc-499">/ SetContentType: "content-type"</span><span class="sxs-lookup"><span data-stu-id="7c0fc-499">/SetContentType:"content-type"</span></span>
<span data-ttu-id="7c0fc-500">Megadja a hello MIME content-type cél blobokkal vagy a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-500">Specifies hello MIME content type for destination blobs or files.</span></span>

<span data-ttu-id="7c0fc-501">AzCopy beállítása a blob tartalomtípusa hello, vagy a fájl tooapplication/octet-stream alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-501">AzCopy sets hello content type for a blob or file tooapplication/octet-stream by default.</span></span> <span data-ttu-id="7c0fc-502">Beállíthatja a hello content-type blobokkal vagy a fájlok explicit megadása ehhez a beállításhoz tartozó értéket.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-502">You can set hello content type for all blobs or files by explicitly specifying a value for this option.</span></span>

<span data-ttu-id="7c0fc-503">Ha megadja ezt a beállítást érték nélküli, majd AzCopy állítja be minden egyes blob vagy a fájl tartalomtípusa szerint tooits fájl kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-503">If you specify this option without a value, then AzCopy will set each blob or file's content type according tooits file extension.</span></span>

<span data-ttu-id="7c0fc-504">**Alkalmazandó:** Blobok, fájlok</span><span class="sxs-lookup"><span data-stu-id="7c0fc-504">**Applicable to:** Blobs, Files</span></span>

### <a name="payloadformatjson--csv"></a><span data-ttu-id="7c0fc-505">/ PayloadFormat: "JSON" |} "CSV"</span><span class="sxs-lookup"><span data-stu-id="7c0fc-505">/PayloadFormat:"JSON" | "CSV"</span></span>
<span data-ttu-id="7c0fc-506">Meghatározza a hello tábla exportált fájlt hello formátumot.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-506">Specifies hello format of hello table exported data file.</span></span>

<span data-ttu-id="7c0fc-507">Ha ez a beállítás nincs megadva, alapértelmezés szerint AzCopy exportálja tábla adatfájl JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-507">If this option is not specified, by default AzCopy exports table data file in JSON format.</span></span>

<span data-ttu-id="7c0fc-508">**Alkalmazandó:** táblák</span><span class="sxs-lookup"><span data-stu-id="7c0fc-508">**Applicable to:** Tables</span></span>

## <a name="known-issues-and-best-practices"></a><span data-ttu-id="7c0fc-509">Ismert problémák és ajánlott eljárások</span><span class="sxs-lookup"><span data-stu-id="7c0fc-509">Known Issues and Best Practices</span></span>
### <a name="limit-concurrent-writes-while-copying-data"></a><span data-ttu-id="7c0fc-510">Adatok másolása egyidejű írási műveletek korlátozása</span><span class="sxs-lookup"><span data-stu-id="7c0fc-510">Limit concurrent writes while copying data</span></span>
<span data-ttu-id="7c0fc-511">AzCopy rendelkező fájlokat vagy a BLOB másolása esetén vegye figyelembe, hogy egy másik alkalmazás módosítja hello adatokat másol, amíg.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-511">When you copy blobs or files with AzCopy, keep in mind that another application may be modifying hello data while you are copying it.</span></span> <span data-ttu-id="7c0fc-512">Ha lehetséges győződjön meg arról, hogy hello adatok másolása nem áll módosítás alatt hello másolási művelet során.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-512">If possible, ensure that hello data you are copying is not being modified during hello copy operation.</span></span> <span data-ttu-id="7c0fc-513">Például amikor egy Azure virtuális géphez társított virtuális merevlemez másolása, győződjön meg arról, hogy más alkalmazás nem jelenleg írás toohello VHD-t.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-513">For example, when copying a VHD associated with an Azure virtual machine, make sure that no other applications are currently writing toohello VHD.</span></span> <span data-ttu-id="7c0fc-514">Egy jó módszer toodo ezen nem lízing hello erőforrás toobe másolva.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-514">A good way toodo this is by leasing hello resource toobe copied.</span></span> <span data-ttu-id="7c0fc-515">Alternatív megoldásként pillanatkép létrehozása a virtuális merevlemez hello először, és másolja hello pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-515">Alternately, you can create a snapshot of hello VHD first and then copy hello snapshot.</span></span>

<span data-ttu-id="7c0fc-516">Ha más alkalmazásokat tooblobs vagy fájlok írásában közben másolni, akkor ne feledje, hogy hello idő hello feladat befejezése nem megelőzése érdekében hello másolt erőforrások már nincs teljes paritásos hello forrás erőforrásokkal.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-516">If you cannot prevent other applications from writing tooblobs or files while they are being copied, then keep in mind that by hello time hello job finishes, hello copied resources may no longer have full parity with hello source resources.</span></span>

### <a name="run-one-azcopy-instance-on-one-machine"></a><span data-ttu-id="7c0fc-517">Futtassa az AzCopy egy példány egy számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-517">Run one AzCopy instance on one machine.</span></span>
<span data-ttu-id="7c0fc-518">AzCopy tervezett toomaximize hello kihasználtságát a gép erőforrás tooaccelerate hello adatátviteli, azt javasoljuk, hogy csak egy AzCopy példány egy számítógépen futtatja, és hello beállítást adja meg `/NC` Ha több egyidejű műveletek van szüksége.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-518">AzCopy is designed toomaximize hello utilization of your machine resource tooaccelerate hello data transfer, we recommend you run only one AzCopy instance on one machine, and specify hello option `/NC` if you need more concurrent operations.</span></span> <span data-ttu-id="7c0fc-519">További tudnivalókért írja be a `AzCopy /?:NC` hello parancssorból.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-519">For more details, type `AzCopy /?:NC` at hello command line.</span></span>

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a><span data-ttu-id="7c0fc-520">FIPS szabványnak megfelelő MD5 algoritmusok engedélyezése az AzCopy amikor meg "használata FIPS szabványnak megfelelő algoritmusok használata titkosításhoz, kivonatoláshoz és aláíráshoz".</span><span class="sxs-lookup"><span data-stu-id="7c0fc-520">Enable FIPS compliant MD5 algorithms for AzCopy when you "Use FIPS compliant algorithms for encryption, hashing and signing".</span></span>
<span data-ttu-id="7c0fc-521">AzCopy alapértelmezés szerint a .NET MD5 megvalósítási toocalculate hello MD5 használja az objektumok másolásakor, de néhány biztonsági követelmények, amelyeket AzCopy tooenable FIPS szabványnak megfelelő MD5-beállítás.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-521">AzCopy by default uses .NET MD5 implementation toocalculate hello MD5 when copying objects, but there are some security requirements that need AzCopy tooenable FIPS compliant MD5 setting.</span></span>

<span data-ttu-id="7c0fc-522">Létrehozhat egy app.config fájl `AzCopy.exe.config` tulajdonsággal `AzureStorageUseV1MD5` , tegye a AzCopy.exe tartalékoljon.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-522">You can create an app.config file `AzCopy.exe.config` with property `AzureStorageUseV1MD5` and put it aside with AzCopy.exe.</span></span>

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

<span data-ttu-id="7c0fc-523">"AzureStorageUseV1MD5" • tulajdonság igaz - hello alapértelmezett érték, AzCopy fogja használni .NET MD5 végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-523">For property "AzureStorageUseV1MD5" • True - hello default value, AzCopy will use .NET MD5 implementation.</span></span>
<span data-ttu-id="7c0fc-524">• Hamis – AzCopy FIPS szabványnak megfelelő MD5 algoritmust használja.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-524">• False – AzCopy will use FIPS compliant MD5 algorithm.</span></span>

<span data-ttu-id="7c0fc-525">Vegye figyelembe, hogy FIPS szabványnak megfelelő algoritmus a Windows-számítógépen alapértelmezés szerint le van tiltva, a Futtatás ablakba írja be a secpol.msc, és ellenőrizze, hogy ezt a kapcsolót, a biztonsági beállítások -> helyi házirend -> biztonsági beállítások -> rendszer-kriptográfia: használata FIPS-kompatibilis algoritmusok használata titkosításhoz, kivonatoláshoz és aláíráshoz.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-525">Note that FIPS compliant algorithms is disabled by default on your Windows machine, you can type secpol.msc in your Run window and check this switch at Security Setting->Local Policy->Security Options->System cryptography: Use FIPS compliant algorithms for encryption, hashing and signing.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7c0fc-526">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7c0fc-526">Next steps</span></span>
<span data-ttu-id="7c0fc-527">Azure Storage és AzCopy kapcsolatos további információkért tekintse meg a következő erőforrások toohello.</span><span class="sxs-lookup"><span data-stu-id="7c0fc-527">For more information about Azure Storage and AzCopy, refer toohello following resources.</span></span>

### <a name="azure-storage-documentation"></a><span data-ttu-id="7c0fc-528">Az Azure Storage-dokumentáció:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-528">Azure Storage documentation:</span></span>
* [<span data-ttu-id="7c0fc-529">Bevezetés tooAzure tároló</span><span class="sxs-lookup"><span data-stu-id="7c0fc-529">Introduction tooAzure Storage</span></span>](storage-introduction.md)
* [<span data-ttu-id="7c0fc-530">Hogyan toouse a .NET-Blob-tároló</span><span class="sxs-lookup"><span data-stu-id="7c0fc-530">How toouse Blob storage from .NET</span></span>](storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="7c0fc-531">Hogyan toouse File storage .NET</span><span class="sxs-lookup"><span data-stu-id="7c0fc-531">How toouse File storage from .NET</span></span>](storage-dotnet-how-to-use-files.md)
* [<span data-ttu-id="7c0fc-532">Hogyan toouse a Table storage a .NET használatával</span><span class="sxs-lookup"><span data-stu-id="7c0fc-532">How toouse Table storage from .NET</span></span>](storage-dotnet-how-to-use-tables.md)
* [<span data-ttu-id="7c0fc-533">Hogyan toocreate, kezelése vagy törlése</span><span class="sxs-lookup"><span data-stu-id="7c0fc-533">How toocreate, manage, or delete a storage account</span></span>](storage-create-storage-account.md)
* [<span data-ttu-id="7c0fc-534">Adatátvitel az AzCopy Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="7c0fc-534">Transfer data with AzCopy on Linux</span></span>](storage-use-azcopy-linux.md)

### <a name="azure-storage-blog-posts"></a><span data-ttu-id="7c0fc-535">Az Azure Storage blogbejegyzések:</span><span class="sxs-lookup"><span data-stu-id="7c0fc-535">Azure Storage blog posts:</span></span>
* [<span data-ttu-id="7c0fc-536">Introducing Azure Storage adatátviteli könyvtár megtekintés</span><span class="sxs-lookup"><span data-stu-id="7c0fc-536">Introducing Azure Storage Data Movement Library Preview</span></span>](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [<span data-ttu-id="7c0fc-537">AzCopy: Bevezetéséről szinkron másolatot és testreszabott tartalom típusa</span><span class="sxs-lookup"><span data-stu-id="7c0fc-537">AzCopy: Introducing synchronous copy and customized content type</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [<span data-ttu-id="7c0fc-538">AzCopy: Általános rendelkezésre állási az AzCopy 3.0 és a tábla és a fájl-támogatással rendelkező az AzCopy 4.0 előzetes bejelentése</span><span class="sxs-lookup"><span data-stu-id="7c0fc-538">AzCopy: Announcing General Availability of AzCopy 3.0 plus preview release of AzCopy 4.0 with Table and File support</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [<span data-ttu-id="7c0fc-539">AzCopy: A felügyeleti teendők központjaként másolással optimalizált</span><span class="sxs-lookup"><span data-stu-id="7c0fc-539">AzCopy: Optimized for Large-Scale Copy Scenarios</span></span>](http://go.microsoft.com/fwlink/?LinkId=507682)
* [<span data-ttu-id="7c0fc-540">AzCopy: Írásvédett georedundáns tárolás támogatása</span><span class="sxs-lookup"><span data-stu-id="7c0fc-540">AzCopy: Support for read-access geo-redundant storage</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [<span data-ttu-id="7c0fc-541">AzCopy: Adatátvitelt újra elindítható móddal és SAS-jogkivonat</span><span class="sxs-lookup"><span data-stu-id="7c0fc-541">AzCopy: Transfer data with re-startable mode and SAS token</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [<span data-ttu-id="7c0fc-542">AzCopy: Kereszt-fiók másolási Blob használatával</span><span class="sxs-lookup"><span data-stu-id="7c0fc-542">AzCopy: Using cross-account Copy Blob</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [<span data-ttu-id="7c0fc-543">AzCopy: Feltöltése/fájlok letöltése Azure Blobokra vonatkozó</span><span class="sxs-lookup"><span data-stu-id="7c0fc-543">AzCopy: Uploading/downloading files for Azure Blobs</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

