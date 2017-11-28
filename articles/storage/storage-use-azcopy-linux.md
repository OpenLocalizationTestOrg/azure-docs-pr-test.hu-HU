---
title: "aaaCopy, vagy helyezze adatok tooAzure Linux AzCopy szolgáltatással |} Microsoft Docs"
description: "Hello AzCopy használata Linux segédprogram toomove vagy másolat adatok tooor a blob és a fájl tartalmát. Adatok tooAzure tárolási átmásolhatja a helyi fájlokból, vagy adatok belül vagy tárfiókok között. Az adatok tooAzure Storage könnyen át."
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
ms.date: 05/11/2017
ms.author: seguler
ms.openlocfilehash: dccb03c9e8cc3ea661494e7834f307b0e3e30cb9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="transfer-data-with-azcopy-on-linux"></a><span data-ttu-id="3f256-105">Adatátvitel az AzCopy Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="3f256-105">Transfer data with AzCopy on Linux</span></span>
<span data-ttu-id="3f256-106">AzCopy Linux egy parancssori segédprogram, az adatok tooand másolását a Microsoft Azure-Blob és a fájl tárolási egyszerű parancsokkal optimális teljesítménnyel.</span><span class="sxs-lookup"><span data-stu-id="3f256-106">AzCopy on Linux is a command-line utility designed for copying data tooand from Microsoft Azure Blob and File storage using simple commands with optimal performance.</span></span> <span data-ttu-id="3f256-107">Adatokat másolhat egy objektum tooanother a tárfiókon belül vagy tárfiókok között.</span><span class="sxs-lookup"><span data-stu-id="3f256-107">You can copy data from one object tooanother within your storage account, or between storage accounts.</span></span>

<span data-ttu-id="3f256-108">AzCopy, letöltheti a két verziója van.</span><span class="sxs-lookup"><span data-stu-id="3f256-108">There are two versions of AzCopy that you can download.</span></span> <span data-ttu-id="3f256-109">AzCopy Linux ajánlat POSIX-stílusú parancssori kapcsolók Linux platformon célozza .NET Core keretrendszerrel épül.</span><span class="sxs-lookup"><span data-stu-id="3f256-109">AzCopy on Linux is built with .NET Core Framework, which targets Linux platforms offering POSIX style command-line options.</span></span> <span data-ttu-id="3f256-110">[A Windows AzCopy](storage-use-azcopy.md) a .NET-keretrendszer épül, és ez biztosítja a Windows stílus parancssori kapcsolókat.</span><span class="sxs-lookup"><span data-stu-id="3f256-110">[AzCopy on Windows](storage-use-azcopy.md) is built with .NET Framework, and offers Windows style command-line options.</span></span> <span data-ttu-id="3f256-111">Ez a cikk a Linux AzCopy ismerteti.</span><span class="sxs-lookup"><span data-stu-id="3f256-111">This article covers AzCopy on Linux.</span></span>

## <a name="download-and-install-azcopy"></a><span data-ttu-id="3f256-112">Töltse le és telepítse az AzCopy</span><span class="sxs-lookup"><span data-stu-id="3f256-112">Download and install AzCopy</span></span>
### <a name="installation-on-linux"></a><span data-ttu-id="3f256-113">Linux-telepítés</span><span class="sxs-lookup"><span data-stu-id="3f256-113">Installation on Linux</span></span>

<span data-ttu-id="3f256-114">AzCopy Linux szüksége van a .NET Core keretrendszer hello platformon.</span><span class="sxs-lookup"><span data-stu-id="3f256-114">AzCopy on Linux requires .NET Core framework on hello platform.</span></span> <span data-ttu-id="3f256-115">Hello telepítési utasításait lásd a hello [.NET Core](https://www.microsoft.com/net/core#linuxubuntu) lap.</span><span class="sxs-lookup"><span data-stu-id="3f256-115">See hello installation instructions on hello [.NET Core](https://www.microsoft.com/net/core#linuxubuntu) page.</span></span>

<span data-ttu-id="3f256-116">Tegyük fel most telepítse a .NET Core Ubuntu 16.10.</span><span class="sxs-lookup"><span data-stu-id="3f256-116">As an example, let's install .NET Core on Ubuntu 16.10.</span></span> <span data-ttu-id="3f256-117">Hello legújabb telepítési útmutató a Microsoft [.NET Core Linux](https://www.microsoft.com/net/core#linuxubuntu) telepítési oldal.</span><span class="sxs-lookup"><span data-stu-id="3f256-117">For hello latest installation guide, visit [.NET Core on Linux](https://www.microsoft.com/net/core#linuxubuntu) installation page.</span></span>


```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ yakkety main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
sudo apt-get update
sudo apt-get install dotnet-dev-1.0.3
```

<span data-ttu-id="3f256-118">A .NET Core telepítése után töltse le és telepítse az AzCopy.</span><span class="sxs-lookup"><span data-stu-id="3f256-118">Once you have installed .NET Core, download and install AzCopy.</span></span>

```bash
wget -O azcopy.tar.gz https://aka.ms/downloadazcopyprlinux
tar -xf azcopy.tar.gz
sudo ./install.sh
```

<span data-ttu-id="3f256-119">Hello kibontott fájlokat is távolítható el, ha az AzCopy Linux rendszeren telepítve van.</span><span class="sxs-lookup"><span data-stu-id="3f256-119">You can remove hello extracted files once AzCopy on Linux is installed.</span></span> <span data-ttu-id="3f256-120">Másik lehetőségként nem jogosult felügyelő, ha is futtathatja hello parancsfájl használatával AzCopy "azcopy" hello kibontott mappában.</span><span class="sxs-lookup"><span data-stu-id="3f256-120">Alternatively if you do not have superuser privileges, you can also run AzCopy using hello shell script 'azcopy' in hello extracted folder.</span></span> 

### <a name="alternative-installation-on-ubuntu"></a><span data-ttu-id="3f256-121">Ubuntu alternatív telepítése</span><span class="sxs-lookup"><span data-stu-id="3f256-121">Alternative Installation on Ubuntu</span></span>

<span data-ttu-id="3f256-122">**Ubuntu 14.04**</span><span class="sxs-lookup"><span data-stu-id="3f256-122">**Ubuntu 14.04**</span></span>

<span data-ttu-id="3f256-123">Apt-forrás hozzáadása a .net Core:</span><span class="sxs-lookup"><span data-stu-id="3f256-123">Add apt source for .Net Core:</span></span>

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

<span data-ttu-id="3f256-124">Microsoft Linux termék tárház apt forrás hozzáadása, és telepítse az AzCopy:</span><span class="sxs-lookup"><span data-stu-id="3f256-124">Add apt source for Microsoft Linux product repository and install AzCopy:</span></span>

```bash
curl https://packages.microsoft.com/config/ubuntu/14.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

<span data-ttu-id="3f256-125">**Ubuntu 16.04**</span><span class="sxs-lookup"><span data-stu-id="3f256-125">**Ubuntu 16.04**</span></span>

<span data-ttu-id="3f256-126">Apt-forrás hozzáadása a .net Core:</span><span class="sxs-lookup"><span data-stu-id="3f256-126">Add apt source for .Net Core:</span></span>

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

<span data-ttu-id="3f256-127">Microsoft Linux termék tárház apt forrás hozzáadása, és telepítse az AzCopy:</span><span class="sxs-lookup"><span data-stu-id="3f256-127">Add apt source for Microsoft Linux product repository and install AzCopy:</span></span>

```bash
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

<span data-ttu-id="3f256-128">**Ubuntu 16.10**</span><span class="sxs-lookup"><span data-stu-id="3f256-128">**Ubuntu 16.10**</span></span>

<span data-ttu-id="3f256-129">Apt-forrás hozzáadása a .net Core:</span><span class="sxs-lookup"><span data-stu-id="3f256-129">Add apt source for .Net Core:</span></span>

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ yakkety main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

<span data-ttu-id="3f256-130">Microsoft Linux termék tárház apt forrás hozzáadása, és telepítse az AzCopy:</span><span class="sxs-lookup"><span data-stu-id="3f256-130">Add apt source for Microsoft Linux product repository and install AzCopy:</span></span>

```bash
curl https://packages.microsoft.com/config/ubuntu/16.10/prod.list > ./microsoft-prod.list
sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
```

```bash
sudo apt-get update
sudo apt-get install azcopy
```

## <a name="writing-your-first-azcopy-command"></a><span data-ttu-id="3f256-131">Az első AzCopy parancs írása</span><span class="sxs-lookup"><span data-stu-id="3f256-131">Writing your first AzCopy command</span></span>
<span data-ttu-id="3f256-132">hello alapvető AzCopy parancs szintaxisa a következő:</span><span class="sxs-lookup"><span data-stu-id="3f256-132">hello basic syntax for AzCopy commands is:</span></span>

```azcopy
azcopy --source <source> --destination <destination> [Options]
```

<span data-ttu-id="3f256-133">hello a következő példák bemutatják, a Microsoft Azure-BLOB és a fájlok másolását adatok tooand különböző lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="3f256-133">hello following examples demonstrate various scenarios for copying data tooand from Microsoft Azure Blobs and Files.</span></span> <span data-ttu-id="3f256-134">Tekintse meg a toohello `azcopy --help` menü használt minden egyes minta hello paraméterek részletes leírását.</span><span class="sxs-lookup"><span data-stu-id="3f256-134">Refer toohello `azcopy --help` menu for a detailed explanation of hello parameters used in each sample.</span></span>

## <a name="blob-download"></a><span data-ttu-id="3f256-135">BLOB: letöltése</span><span class="sxs-lookup"><span data-stu-id="3f256-135">Blob: Download</span></span>
### <a name="download-single-blob"></a><span data-ttu-id="3f256-136">Egy blob letöltése</span><span class="sxs-lookup"><span data-stu-id="3f256-136">Download single blob</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="3f256-137">Ha hello mappa `/mnt/myfiles` nem létezik, AzCopy létrehozza, és letölti `abc.txt ` hello új mappába.</span><span class="sxs-lookup"><span data-stu-id="3f256-137">If hello folder `/mnt/myfiles` does not exist, AzCopy creates it and downloads `abc.txt ` into hello new folder.</span></span>

### <a name="download-single-blob-from-secondary-region"></a><span data-ttu-id="3f256-138">A másodlagos régióba egyetlen blob letöltése</span><span class="sxs-lookup"><span data-stu-id="3f256-138">Download single blob from secondary region</span></span>

```azcopy
azcopy \
    --source https://myaccount-secondary.blob.core.windows.net/mynewcontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="3f256-139">Vegye figyelembe, hogy írásvédett georedundáns tárolás engedélyezve kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="3f256-139">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="download-all-blobs"></a><span data-ttu-id="3f256-140">Töltse le az összes BLOB</span><span class="sxs-lookup"><span data-stu-id="3f256-140">Download all blobs</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

<span data-ttu-id="3f256-141">Hello következő feltételezik hello megadott tárolóban található blobok:</span><span class="sxs-lookup"><span data-stu-id="3f256-141">Assume hello following blobs reside in hello specified container:</span></span>  

```
abc.txt
abc1.txt
abc2.txt
vd1/a.txt
vd1/abcd.txt
```

<span data-ttu-id="3f256-142">Hello letöltési művelet után hello directory `/mnt/myfiles` tartalmazza a következő fájlok hello:</span><span class="sxs-lookup"><span data-stu-id="3f256-142">After hello download operation, hello directory `/mnt/myfiles` includes hello following files:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/vd1/a.txt
/mnt/myfiles/vd1/abcd.txt
```

<span data-ttu-id="3f256-143">Ha nem adja meg a beállítás `--recursive`, nincs blob le lesznek töltve.</span><span class="sxs-lookup"><span data-stu-id="3f256-143">If you do not specify option `--recursive`, no blob will be downloaded.</span></span>

### <a name="download-blobs-with-specified-prefix"></a><span data-ttu-id="3f256-144">A megadott előtag blobok letöltése</span><span class="sxs-lookup"><span data-stu-id="3f256-144">Download blobs with specified prefix</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "a" \
    --recursive
```

<span data-ttu-id="3f256-145">Hello következő feltételezik hello megadott tárolóban található blobok.</span><span class="sxs-lookup"><span data-stu-id="3f256-145">Assume hello following blobs reside in hello specified container.</span></span> <span data-ttu-id="3f256-146">Hello előtaggal kezdődő összes BLOB `a` letöltődnek.</span><span class="sxs-lookup"><span data-stu-id="3f256-146">All blobs beginning with hello prefix `a` are downloaded.</span></span>

```
abc.txt
abc1.txt
abc2.txt
xyz.txt
vd1\a.txt
vd1\abcd.txt
```

<span data-ttu-id="3f256-147">Hello letöltési művelet után hello mappa `/mnt/myfiles` tartalmazza a következő fájlok hello:</span><span class="sxs-lookup"><span data-stu-id="3f256-147">After hello download operation, hello folder `/mnt/myfiles` includes hello following files:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
```

<span data-ttu-id="3f256-148">hello előtag toohello virtuális könyvtár, hello hello blob neve az első részét képező vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="3f256-148">hello prefix applies toohello virtual directory, which forms hello first part of hello blob name.</span></span> <span data-ttu-id="3f256-149">Hello a fenti példában hello virtuális könyvtár nem azonos hello megadott előtag, így nincs blob letöltődik.</span><span class="sxs-lookup"><span data-stu-id="3f256-149">In hello example shown above, hello virtual directory does not match hello specified prefix, so no blob is downloaded.</span></span> <span data-ttu-id="3f256-150">Emellett, ha hello lehetőséget `--recursive` nincs megadva, az AzCopy nem tölti le a blobokat.</span><span class="sxs-lookup"><span data-stu-id="3f256-150">In addition, if hello option `--recursive` is not specified, AzCopy does not download any blobs.</span></span>

### <a name="set-hello-last-modified-time-of-exported-files-toobe-same-as-hello-source-blobs"></a><span data-ttu-id="3f256-151">Állítsa be az exportált fájlokat toobe hello utolsó módosításának időpontja szerint forrás blobok hello</span><span class="sxs-lookup"><span data-stu-id="3f256-151">Set hello last-modified time of exported files toobe same as hello source blobs</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination "/mnt/myfiles" \
    --source-key <key> \
    --preserve-last-modified-time
```

<span data-ttu-id="3f256-152">Blobok emellett kizárja hello letöltési művelet az utolsó módosításának ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="3f256-152">You can also exclude blobs from hello download operation based on their last-modified time.</span></span> <span data-ttu-id="3f256-153">Például, ha azt szeretné, hogy a tooexclude blobot, amelynek utolsó módosítási időpontjának hello azonos vagy újabb, mint hello célfájl, adja meg hello `--exclude-newer` lehetőséget:</span><span class="sxs-lookup"><span data-stu-id="3f256-153">For example, if you want tooexclude blobs whose last modified time is hello same or newer than hello destination file, add hello `--exclude-newer` option:</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-newer
```

<span data-ttu-id="3f256-154">Vagy ha azt szeretné, hogy tooexclude blobot, amelynek utolsó módosítási időpontjának hello azonos vagy régebbi, mint hello célfájl, adja hozzá a hello `--exclude-older` lehetőséget:</span><span class="sxs-lookup"><span data-stu-id="3f256-154">Or if you want tooexclude blobs whose last modified time is hello same or older than hello destination file, add hello `--exclude-older` option:</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-older
```

## <a name="blob-upload"></a><span data-ttu-id="3f256-155">BLOB: feltöltése</span><span class="sxs-lookup"><span data-stu-id="3f256-155">Blob: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="3f256-156">Töltse fel egy fájlból</span><span class="sxs-lookup"><span data-stu-id="3f256-156">Upload single file</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="3f256-157">Ha hello megadott tároló nem létezik, AzCopy létrehozza, és feltöltések fájl hello bele.</span><span class="sxs-lookup"><span data-stu-id="3f256-157">If hello specified destination container does not exist, AzCopy creates it and uploads hello file into it.</span></span>

### <a name="upload-single-file-toovirtual-directory"></a><span data-ttu-id="3f256-158">Egy fájlból toovirtual directory feltöltése</span><span class="sxs-lookup"><span data-stu-id="3f256-158">Upload single file toovirtual directory</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="3f256-159">Ha hello megadott virtuális könyvtár nem létezik, AzCopy feltölt hello tooinclude hello virtuális könyvtárának a hello blob neve (*pl.*, `vd/abc.txt` a fenti példában hello).</span><span class="sxs-lookup"><span data-stu-id="3f256-159">If hello specified virtual directory does not exist, AzCopy uploads hello file tooinclude hello virtual directory in hello blob name (*e.g.*, `vd/abc.txt` in hello example above).</span></span>

### <a name="upload-all-files"></a><span data-ttu-id="3f256-160">Minden fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="3f256-160">Upload all files</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --recursive
```

<span data-ttu-id="3f256-161">Beállítás megadása `--recursive` hello feltöltések hello tartalmát a megadott könyvtár tooBlob tárolási rekurzív módon, ami azt jelenti, hogy minden almappa és a fájljaikat, valamint feltöltése.</span><span class="sxs-lookup"><span data-stu-id="3f256-161">Specifying option `--recursive` uploads hello contents of hello specified directory tooBlob storage recursively, meaning that all subfolders and their files are uploaded as well.</span></span> <span data-ttu-id="3f256-162">Például azt feltételezik hello következő mappában található fájlok `/mnt/myfiles`:</span><span class="sxs-lookup"><span data-stu-id="3f256-162">For instance, assume hello following files reside in folder `/mnt/myfiles`:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

<span data-ttu-id="3f256-163">Hello feltöltési művelet után hello tároló tartalmazza a következő fájlok hello:</span><span class="sxs-lookup"><span data-stu-id="3f256-163">After hello upload operation, hello container includes hello following files:</span></span>

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

<span data-ttu-id="3f256-164">Ha a beállítás hello `--recursive` nincs megadva, csak hello következő három fájlok feltöltése:</span><span class="sxs-lookup"><span data-stu-id="3f256-164">When hello option `--recursive` is not specified, only hello following three files are uploaded:</span></span>

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="3f256-165">Töltse fel a megadott mintának megfelelő fájlok</span><span class="sxs-lookup"><span data-stu-id="3f256-165">Upload files matching specified pattern</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "a*" \
    --recursive
```

<span data-ttu-id="3f256-166">Hello következő feltételezik mappában található fájlok `/mnt/myfiles`:</span><span class="sxs-lookup"><span data-stu-id="3f256-166">Assume hello following files reside in folder `/mnt/myfiles`:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/xyz.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

<span data-ttu-id="3f256-167">Hello feltöltési művelet után hello tároló tartalmazza a következő fájlok hello:</span><span class="sxs-lookup"><span data-stu-id="3f256-167">After hello upload operation, hello container includes hello following files:</span></span>

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

<span data-ttu-id="3f256-168">Ha a beállítás hello `--recursive` nincs megadva, az AzCopy kihagyja alkönyvtár lévő fájlok:</span><span class="sxs-lookup"><span data-stu-id="3f256-168">When hello option `--recursive` is not specified, AzCopy skips files that are in sub-directories:</span></span>

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="specify-hello-mime-content-type-of-a-destination-blob"></a><span data-ttu-id="3f256-169">Adjon meg egy cél BLOB hello MIME tartalomtípus</span><span class="sxs-lookup"><span data-stu-id="3f256-169">Specify hello MIME content type of a destination blob</span></span>
<span data-ttu-id="3f256-170">Alapértelmezés szerint AzCopy beállítja hello tartalomtípusa cél blob túl`application/octet-stream`.</span><span class="sxs-lookup"><span data-stu-id="3f256-170">By default, AzCopy sets hello content type of a destination blob too`application/octet-stream`.</span></span> <span data-ttu-id="3f256-171">Azonban, explicit módon megadhat hello tartalomtípus keresztül hello beállítás `--set-content-type [content-type]`.</span><span class="sxs-lookup"><span data-stu-id="3f256-171">However, you can explicitly specify hello content type via hello option `--set-content-type [content-type]`.</span></span> <span data-ttu-id="3f256-172">Ez a szintaxis hello content-type összes BLOB a feltöltési művelet állítja be.</span><span class="sxs-lookup"><span data-stu-id="3f256-172">This syntax sets hello content type for all blobs in an upload operation.</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type "video/mp4"
```

<span data-ttu-id="3f256-173">Ha hello lehetőséget `--set-content-type` egy érték nélkül van megadva, majd az AzCopy állítja be, minden egyes blob vagy a fájl CODEPAGE tartalomtípus szerint tooits fájl kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="3f256-173">If hello option `--set-content-type` is specified without a value, then AzCopy sets each blob or file's content type according tooits file extension.</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type
```

## <a name="blob-copy"></a><span data-ttu-id="3f256-174">BLOB: másolása</span><span class="sxs-lookup"><span data-stu-id="3f256-174">Blob: Copy</span></span>
### <a name="copy-single-blob-within-storage-account"></a><span data-ttu-id="3f256-175">Másolja át egy blob Storage-fiókban</span><span class="sxs-lookup"><span data-stu-id="3f256-175">Copy single blob within Storage account</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1 \
    --destination https://myaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key> \
    --dest-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="3f256-176">Ha--másolatának szinkronizálása beállítás nélkül egy blobot másol egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.</span><span class="sxs-lookup"><span data-stu-id="3f256-176">When you copy a blob without --sync-copy option, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-across-storage-accounts"></a><span data-ttu-id="3f256-177">Másolja át egy blob Storage-fiókok között</span><span class="sxs-lookup"><span data-stu-id="3f256-177">Copy single blob across Storage accounts</span></span>

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1 \
    --destination https://destaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt"
```

<span data-ttu-id="3f256-178">Ha--másolatának szinkronizálása beállítás nélkül egy blobot másol egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.</span><span class="sxs-lookup"><span data-stu-id="3f256-178">When you copy a blob without --sync-copy option, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-from-secondary-region-tooprimary-region"></a><span data-ttu-id="3f256-179">Egy blob másolása másodlagos régióba tooprimary régió</span><span class="sxs-lookup"><span data-stu-id="3f256-179">Copy single blob from secondary region tooprimary region</span></span>

```azcopy
azcopy \
    --source https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 \
    --destination https://myaccount2.blob.core.windows.net/mynewcontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt"
```

<span data-ttu-id="3f256-180">Vegye figyelembe, hogy írásvédett georedundáns tárolás engedélyezve kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="3f256-180">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a><span data-ttu-id="3f256-181">Egy blob másolási és a pillanatképek tárfiókok között</span><span class="sxs-lookup"><span data-stu-id="3f256-181">Copy single blob and its snapshots across Storage accounts</span></span>

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1 \
    --destination https://destaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt" \
    --include-snapshot
```

<span data-ttu-id="3f256-182">Hello másolási művelet után a hello céltároló hello blob és a pillanatképek tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3f256-182">After hello copy operation, hello target container includes hello blob and its snapshots.</span></span> <span data-ttu-id="3f256-183">hello tároló tartalmazza hello következő blob és a pillanatképek:</span><span class="sxs-lookup"><span data-stu-id="3f256-183">hello container includes hello following blob and its snapshots:</span></span>

```
abc.txt
abc (2013-02-25 080757).txt
abc (2014-02-21 150331).txt
```

### <a name="synchronously-copy-blobs-across-storage-accounts"></a><span data-ttu-id="3f256-184">Szinkron módon másolni BLOB Storage-fiókok</span><span class="sxs-lookup"><span data-stu-id="3f256-184">Synchronously copy blobs across Storage accounts</span></span>
<span data-ttu-id="3f256-185">AzCopy alapértelmezés szerint aszinkron módon másolja az adatokat két tárolási végpontok közötti.</span><span class="sxs-lookup"><span data-stu-id="3f256-185">AzCopy by default copies data between two storage endpoints asynchronously.</span></span> <span data-ttu-id="3f256-186">Ezért hello másolási művelet futtatása hello háttérben tartalékolt sávszélesség-kapacitása, amelynek nincs szolgáltatásiszint-szerződés szempontjából hogyan gyors blob használatával kell másolni.</span><span class="sxs-lookup"><span data-stu-id="3f256-186">Therefore, hello copy operation runs in hello background using spare bandwidth capacity that has no SLA in terms of how fast a blob is copied.</span></span> 

<span data-ttu-id="3f256-187">Hello `--sync-copy` lehetőség biztosítja, hogy hello másolási művelet lekérdezi az egységes sebesség.</span><span class="sxs-lookup"><span data-stu-id="3f256-187">hello `--sync-copy` option ensures that hello copy operation gets consistent speed.</span></span> <span data-ttu-id="3f256-188">AzCopy elkészíti hello szinkron hello blobok letöltésével toocopy hello a megadott forrás toolocal memória, és majd feltöltheti toohello Blob célhelyet.</span><span class="sxs-lookup"><span data-stu-id="3f256-188">AzCopy performs hello synchronous copy by downloading hello blobs toocopy from hello specified source toolocal memory, and then uploading them toohello Blob storage destination.</span></span>

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/myContainer/ \
    --destination https://myaccount2.blob.core.windows.net/myContainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --include "ab" \
    --sync-copy
```

<span data-ttu-id="3f256-189">`--sync-copy`További kilépő képest költség tooasynchronous másolási hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="3f256-189">`--sync-copy` might generate additional egress cost compared tooasynchronous copy.</span></span> <span data-ttu-id="3f256-190">az ajánlott megközelítést alkalmazva hello toouse van ez a beállítás egy Azure virtuális gép, hello lévő és a forrás tárolási fiók tooavoid kilépő költségű ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="3f256-190">hello recommended approach is toouse this option in an Azure VM, that is in hello same region as your source storage account tooavoid egress cost.</span></span>

## <a name="file-download"></a><span data-ttu-id="3f256-191">Fájl: letöltése</span><span class="sxs-lookup"><span data-stu-id="3f256-191">File: Download</span></span>
### <a name="download-single-file"></a><span data-ttu-id="3f256-192">Töltse le egy fájlból</span><span class="sxs-lookup"><span data-stu-id="3f256-192">Download single file</span></span>

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/myfolder1/ \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="3f256-193">Hello adott forrása az Azure fájlmegosztások, akkor meg kell adnia hello pontosan a fájl nevét, ha (*pl.* `abc.txt`) toodownload egyetlen fájl, vagy adja meg a beállítás `--recursive` összes toodownload hello megosztáson található fájlok rekurzív módon.</span><span class="sxs-lookup"><span data-stu-id="3f256-193">If hello specified source is an Azure file share, then you must either specify hello exact file name, (*e.g.* `abc.txt`) toodownload a single file, or specify option `--recursive` toodownload all files in hello share recursively.</span></span> <span data-ttu-id="3f256-194">Kísérlet toospecify egy fájl és egyaránt lehetőség `--recursive` hiba együtt eredményez.</span><span class="sxs-lookup"><span data-stu-id="3f256-194">Attempting toospecify both a file pattern and option `--recursive` together results in an error.</span></span>

### <a name="download-all-files"></a><span data-ttu-id="3f256-195">Minden fájl letöltése</span><span class="sxs-lookup"><span data-stu-id="3f256-195">Download all files</span></span>

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/ \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

<span data-ttu-id="3f256-196">Vegye figyelembe, hogy a rendszer nem tölti le üres mappák.</span><span class="sxs-lookup"><span data-stu-id="3f256-196">Note that any empty folders are not downloaded.</span></span>

## <a name="file-upload"></a><span data-ttu-id="3f256-197">Fájl: feltöltése</span><span class="sxs-lookup"><span data-stu-id="3f256-197">File: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="3f256-198">Töltse fel egy fájlból</span><span class="sxs-lookup"><span data-stu-id="3f256-198">Upload single file</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --include abc.txt
```

### <a name="upload-all-files"></a><span data-ttu-id="3f256-199">Minden fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="3f256-199">Upload all files</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --recursive
```

<span data-ttu-id="3f256-200">Vegye figyelembe, hogy az üres mappák nem töltődött fel.</span><span class="sxs-lookup"><span data-stu-id="3f256-200">Note that any empty folders are not uploaded.</span></span>

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="3f256-201">Töltse fel a megadott mintának megfelelő fájlok</span><span class="sxs-lookup"><span data-stu-id="3f256-201">Upload files matching specified pattern</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --include "ab*" \
    --recursive
```

## <a name="file-copy"></a><span data-ttu-id="3f256-202">Fájl: másolása</span><span class="sxs-lookup"><span data-stu-id="3f256-202">File: Copy</span></span>
### <a name="copy-across-file-shares"></a><span data-ttu-id="3f256-203">Fájlmegosztások másolni</span><span class="sxs-lookup"><span data-stu-id="3f256-203">Copy across file shares</span></span>

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
<span data-ttu-id="3f256-204">Amikor fájlmegosztások között másolhat egy fájlt egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.</span><span class="sxs-lookup"><span data-stu-id="3f256-204">When you copy a file across file shares, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-from-file-share-tooblob"></a><span data-ttu-id="3f256-205">Megosztás tooblob fájl másolása</span><span class="sxs-lookup"><span data-stu-id="3f256-205">Copy from file share tooblob</span></span>

```azcopy
azcopy \ 
    --source https://myaccount1.file.core.windows.net/myfileshare/ \
    --destination https://myaccount2.blob.core.windows.net/mycontainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
<span data-ttu-id="3f256-206">Ha fájlt átmásolni fájl megosztási tooblob, egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.</span><span class="sxs-lookup"><span data-stu-id="3f256-206">When you copy a file from file share tooblob, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-from-blob-toofile-share"></a><span data-ttu-id="3f256-207">A blob toofile megosztásból másolása</span><span class="sxs-lookup"><span data-stu-id="3f256-207">Copy from blob toofile share</span></span>

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/mycontainer/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
<span data-ttu-id="3f256-208">Amikor blob toofile megosztásból másolhat egy fájlt egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.</span><span class="sxs-lookup"><span data-stu-id="3f256-208">When you copy a file from blob toofile share, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="synchronously-copy-files"></a><span data-ttu-id="3f256-209">Szinkron módon történik a fájlok másolása</span><span class="sxs-lookup"><span data-stu-id="3f256-209">Synchronously copy files</span></span>
<span data-ttu-id="3f256-210">Megadhatja a hello `--sync-copy` toocopy adatok File Storage tooFile tároló, a File Storage tooBlob tároló és a tárolási Blob Storage tooFile szinkron módon lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="3f256-210">You can specify hello `--sync-copy` option toocopy data from File Storage tooFile Storage, from File Storage tooBlob Storage and from Blob Storage tooFile Storage synchronously.</span></span> <span data-ttu-id="3f256-211">AzCopy művelet hello forrás adatok toolocal memória letöltés és toodestination feltöltése szerint fut.</span><span class="sxs-lookup"><span data-stu-id="3f256-211">AzCopy runs this operation by downloading hello source data toolocal memory, and then uploading it toodestination.</span></span> <span data-ttu-id="3f256-212">Ebben az esetben a szabványos kilépő költség érvényes.</span><span class="sxs-lookup"><span data-stu-id="3f256-212">In this case, standard egress cost applies.</span></span>

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive \
    --sync-copy
```

<span data-ttu-id="3f256-213">Amikor a File Storage tooBlob tárolási másol, hello alapértelmezett blob típushoz blokkblob, felhasználói beállítást adja meg `/BlobType:page` toochange hello céltípus blob.</span><span class="sxs-lookup"><span data-stu-id="3f256-213">When copying from File Storage tooBlob Storage, hello default blob type is block blob, user can specify option `/BlobType:page` toochange hello destination blob type.</span></span>

<span data-ttu-id="3f256-214">Vegye figyelembe, hogy `--sync-copy` hozhat létre további kilépő összehasonlító tooasynchronous másolási költsége.</span><span class="sxs-lookup"><span data-stu-id="3f256-214">Note that `--sync-copy` might generate additional egress cost comparing tooasynchronous copy.</span></span> <span data-ttu-id="3f256-215">az ajánlott megközelítést alkalmazva hello toouse van ez a beállítás egy Azure virtuális gép, hello lévő és a forrás tárolási fiók tooavoid kilépő költségű ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="3f256-215">hello recommended approach is toouse this option in an Azure VM, that is in hello same region as your source storage account tooavoid egress cost.</span></span>

## <a name="other-azcopy-features"></a><span data-ttu-id="3f256-216">Más AzCopy szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="3f256-216">Other AzCopy features</span></span>
### <a name="only-copy-data-that-doesnt-exist-in-hello-destination"></a><span data-ttu-id="3f256-217">Csak másolja az adatokat, amely a célként megadott hello nem létezik</span><span class="sxs-lookup"><span data-stu-id="3f256-217">Only copy data that doesn't exist in hello destination</span></span>
<span data-ttu-id="3f256-218">Hello `--exclude-older` és `--exclude-newer` paraméterek tooexclude régebbi vagy újabb forrás erőforrások másolását, illetve lehetővé teszik.</span><span class="sxs-lookup"><span data-stu-id="3f256-218">hello `--exclude-older` and `--exclude-newer` parameters allow you tooexclude older or newer source resources from being copied, respectively.</span></span> <span data-ttu-id="3f256-219">Ha csak toocopy forrás erőforrásokat, amelyek nem léteznek a hello cél, mindkét paraméter hello AzCopy parancs adhat meg:</span><span class="sxs-lookup"><span data-stu-id="3f256-219">If you only want toocopy source resources that don't exist in hello destination, you can specify both parameters in hello AzCopy command:</span></span>

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --exclude-older --exclude-newer

    --source /mnt/myfiles --destination http://myaccount.file.core.windows.net/myfileshare --dest-key <destkey> --recursive --exclude-older --exclude-newer

    --source http://myaccount.blob.core.windows.net/mycontainer --destination http://myaccount.blob.core.windows.net/mycontainer1 --source-key <sourcekey> --dest-key <destkey> --recursive --exclude-older --exclude-newer

### <a name="use-a-configuration-file-toospecify-command-line-parameters"></a><span data-ttu-id="3f256-220">Egy konfigurációs fájl toospecify parancssori paraméterek használata</span><span class="sxs-lookup"><span data-stu-id="3f256-220">Use a configuration file toospecify command-line parameters</span></span>

```azcopy
azcopy --config-file "azcopy-config.ini"
```

<span data-ttu-id="3f256-221">AzCopy parancssori paramétereket is megadhat a konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="3f256-221">You can include any AzCopy command-line parameters in a configuration file.</span></span> <span data-ttu-id="3f256-222">AzCopy folyamatok hello hello fájlban paramétereket, mintha hello parancssorban közvetlen helyettesítés hello tartalma hello fájl végrehajtása meg lett.</span><span class="sxs-lookup"><span data-stu-id="3f256-222">AzCopy processes hello parameters in hello file as if they had been specified on hello command line, performing a direct substitution with hello contents of hello file.</span></span>

<span data-ttu-id="3f256-223">Tegyük fel, a konfigurációs fájl nevű `copyoperation`, amely tartalmazza a következő sorokat hello.</span><span class="sxs-lookup"><span data-stu-id="3f256-223">Assume a configuration file named `copyoperation`, that contains hello following lines.</span></span> <span data-ttu-id="3f256-224">Minden egyes AzCopy paraméter egy sorba adható meg.</span><span class="sxs-lookup"><span data-stu-id="3f256-224">Each AzCopy parameter can be specified on a single line.</span></span>

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --quiet

<span data-ttu-id="3f256-225">vagy külön sorok:</span><span class="sxs-lookup"><span data-stu-id="3f256-225">or on separate lines:</span></span>

    --source http://myaccount.blob.core.windows.net/mycontainer
    --destination /mnt/myfiles
    --source-key<sourcekey>
    --recursive
    --quiet

<span data-ttu-id="3f256-226">AzCopy sikertelen lesz, ha hello paraméter osztani két sort, ahogy az itt látható a hello `--source-key` paraméter:</span><span class="sxs-lookup"><span data-stu-id="3f256-226">AzCopy fails if you split hello parameter across two lines, as shown here for hello `--source-key` parameter:</span></span>

    http://myaccount.blob.core.windows.net/mycontainer
    /mnt/myfiles
    --sourcekey
    <sourcekey>
    --recursive
    --quiet

### <a name="specify-a-shared-access-signature-sas"></a><span data-ttu-id="3f256-227">Adja meg a közös hozzáférésű jogosultságkód (SAS)</span><span class="sxs-lookup"><span data-stu-id="3f256-227">Specify a shared access signature (SAS)</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1 \
    --destination https://myaccount.blob.core.windows.net/mycontainer2 \
    --source-sas <SAS1> \
    --dest-sas <SAS2> \
    --include abc.txt
```

<span data-ttu-id="3f256-228">Azt is megadhatja a SAS hello tárolóra URI:</span><span class="sxs-lookup"><span data-stu-id="3f256-228">You can also specify a SAS on hello container URI:</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken \
    --destination /mnt/myfiles \
    --recursive
```

<span data-ttu-id="3f256-229">AzCopy jelenleg csak támogatja a hello [fiók SAS](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-shared-access-signature-part-1).</span><span class="sxs-lookup"><span data-stu-id="3f256-229">Note that AzCopy currently only supports hello [Account SAS](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-shared-access-signature-part-1).</span></span>

### <a name="journal-file-folder"></a><span data-ttu-id="3f256-230">Napló fájlmappa</span><span class="sxs-lookup"><span data-stu-id="3f256-230">Journal file folder</span></span>
<span data-ttu-id="3f256-231">Minden alkalommal, amikor egy parancs tooAzCopy kiadása ellenőrzi, hogy a napló fájl megtalálható-e hello alapértelmezett mappát, vagy hogy keresztül ez a beállítás a megadott mappa létezik.</span><span class="sxs-lookup"><span data-stu-id="3f256-231">Each time you issue a command tooAzCopy, it checks whether a journal file exists in hello default folder, or whether it exists in a folder that you specified via this option.</span></span> <span data-ttu-id="3f256-232">Hello napló fájl nem létezik egyik helyen sem, ha AzCopy hello művelet új kezeli, és létrehoz egy új naplófájl.</span><span class="sxs-lookup"><span data-stu-id="3f256-232">If hello journal file does not exist in either place, AzCopy treats hello operation as new and generates a new journal file.</span></span>

<span data-ttu-id="3f256-233">Hello napló fájl létezik, az AzCopy ellenőrzi, hogy megadott hello parancssori megegyezik-e parancssori hello hello napló fájlban.</span><span class="sxs-lookup"><span data-stu-id="3f256-233">If hello journal file does exist, AzCopy checks whether hello command line that you input matches hello command line in hello journal file.</span></span> <span data-ttu-id="3f256-234">Hello két parancssorokat felel meg, ha az AzCopy hello hiányos művelet folytatása.</span><span class="sxs-lookup"><span data-stu-id="3f256-234">If hello two command lines match, AzCopy resumes hello incomplete operation.</span></span> <span data-ttu-id="3f256-235">Ha nem egyeznek, az AzCopy felhasználói tooeither felülírása hello napló fájl toostart új művelet, vagy toocancel hello aktuális művelet megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="3f256-235">If they do not match, AzCopy prompts user tooeither overwrite hello journal file toostart a new operation, or toocancel hello current operation.</span></span>

<span data-ttu-id="3f256-236">Ha azt szeretné, hogy toouse hello hello napló fájl alapértelmezett helye:</span><span class="sxs-lookup"><span data-stu-id="3f256-236">If you want toouse hello default location for hello journal file:</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --resume
```

<span data-ttu-id="3f256-237">Ha a beállítás nincs megadva `--resume`, vagy adja meg a beállítás `--resume` nélkül hello mappa elérési útja, a fent látható AzCopy fájlt hoz létre hello napló hello alapértelmezett helyen, amely `~\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="3f256-237">If you omit option `--resume`, or specify option `--resume` without hello folder path, as shown above, AzCopy creates hello journal file in hello default location, which is `~\Microsoft\Azure\AzCopy`.</span></span> <span data-ttu-id="3f256-238">Ha hello napló fájl már létezik, majd AzCopy folytatja hello művelet hello napló fájl alapján.</span><span class="sxs-lookup"><span data-stu-id="3f256-238">If hello journal file already exists, then AzCopy resumes hello operation based on hello journal file.</span></span>

<span data-ttu-id="3f256-239">Ha azt szeretné, hogy egy egyéni helyet hello napló fájl toospecify:</span><span class="sxs-lookup"><span data-stu-id="3f256-239">If you want toospecify a custom location for hello journal file:</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key key \
    --resume "/mnt/myjournal"
```

<span data-ttu-id="3f256-240">Ez a példa hello napló fájlt hoz létre, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="3f256-240">This example creates hello journal file if it does not already exist.</span></span> <span data-ttu-id="3f256-241">Ha létezik, majd AzCopy folytatja hello művelet hello napló fájl alapján.</span><span class="sxs-lookup"><span data-stu-id="3f256-241">If it does exist, then AzCopy resumes hello operation based on hello journal file.</span></span>

<span data-ttu-id="3f256-242">Ha azt szeretné, hogy az AzCopy művelet tooresume, ismételje meg a hello ugyanezt a parancsot.</span><span class="sxs-lookup"><span data-stu-id="3f256-242">If you want tooresume an AzCopy operation, repeat hello same command.</span></span> <span data-ttu-id="3f256-243">AzCopy Linux majd megerősítést kér fogja kérni:</span><span class="sxs-lookup"><span data-stu-id="3f256-243">AzCopy on Linux then will prompt for confirmation:</span></span>

```azcopy
Incomplete operation with same command line detected at hello journal directory "/home/myaccount/Microsoft/Azure/AzCopy", do you want tooresume hello operation? Choose Yes tooresume, choose No toooverwrite hello journal toostart a new operation. (Yes/No)
```

### <a name="output-verbose-logs"></a><span data-ttu-id="3f256-244">Kimeneti részletes naplókat</span><span class="sxs-lookup"><span data-stu-id="3f256-244">Output verbose logs</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --verbose
```

### <a name="specify-hello-number-of-concurrent-operations-toostart"></a><span data-ttu-id="3f256-245">Adja meg a hello száma párhuzamos műveletek toostart</span><span class="sxs-lookup"><span data-stu-id="3f256-245">Specify hello number of concurrent operations toostart</span></span>
<span data-ttu-id="3f256-246">A beállítás `--parallel-level` hello egyidejű másolási műveletek számát adja meg.</span><span class="sxs-lookup"><span data-stu-id="3f256-246">Option `--parallel-level` specifies hello number of concurrent copy operations.</span></span> <span data-ttu-id="3f256-247">Alapértelmezés szerint az AzCopy elindul egy bizonyos száma párhuzamos műveletek átviteli tooincrease hello adatátvitelt.</span><span class="sxs-lookup"><span data-stu-id="3f256-247">By default, AzCopy starts a certain number of concurrent operations tooincrease hello data transfer throughput.</span></span> <span data-ttu-id="3f256-248">hello száma párhuzamos műveletek nyolc alkalommal hello processzorral rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="3f256-248">hello number of concurrent operations is equal eight times hello number of processors you have.</span></span> <span data-ttu-id="3f256-249">Ha AzCopy kis sávszélességű hálózaton keresztül futtatja, megadhatja a--párhuzamos szintű tooavoid hibáját okozta. erőforrás verseny kevesebb.</span><span class="sxs-lookup"><span data-stu-id="3f256-249">If you are running AzCopy across a low-bandwidth network, you can specify a lower number for --parallel-level tooavoid failure caused by resource competition.</span></span>

[!TIP]
><span data-ttu-id="3f256-250">tooview hello teljes listáját az AzCopy paraméterek, tekintse meg a "azcopy – súgó" menü.</span><span class="sxs-lookup"><span data-stu-id="3f256-250">tooview hello complete list of AzCopy parameters, check out 'azcopy --help' menu.</span></span>

## <a name="known-issues-and-best-practices"></a><span data-ttu-id="3f256-251">Ismert problémák és ajánlott eljárások</span><span class="sxs-lookup"><span data-stu-id="3f256-251">Known issues and best practices</span></span>
### <a name="error-net-core-is-not-found-in-hello-system"></a><span data-ttu-id="3f256-252">Hiba: A .NET Core nem található a hello rendszer.</span><span class="sxs-lookup"><span data-stu-id="3f256-252">Error: .NET Core is not found in hello system.</span></span>
<span data-ttu-id="3f256-253">Ha arról, hogy a .NET Core nincs telepítve a hello rendszer hibát észlel, elérési út toohello .NET Core bináris hello `dotnet` lehet, hogy hiányzik.</span><span class="sxs-lookup"><span data-stu-id="3f256-253">If you encounter an error stating that .NET Core is not installed in hello system, hello PATH toohello .NET Core binary `dotnet` may be missing.</span></span>

<span data-ttu-id="3f256-254">A rendezés tooaddress probléma, hello .NET Core bináris hello rendszerben található:</span><span class="sxs-lookup"><span data-stu-id="3f256-254">In order tooaddress this issue, find hello .NET Core binary in hello system:</span></span>
```bash
sudo find / -name dotnet
```

<span data-ttu-id="3f256-255">Ez visszaad hello elérési toohello dotnet bináris.</span><span class="sxs-lookup"><span data-stu-id="3f256-255">This returns hello path toohello dotnet binary.</span></span> 

    /opt/rh/rh-dotnetcore11/root/usr/bin/dotnet
    /opt/rh/rh-dotnetcore11/root/usr/lib64/dotnetcore/dotnet
    /opt/rh/rh-dotnetcore11/root/usr/lib64/dotnetcore/shared/Microsoft.NETCore.App/1.1.2/dotnet

<span data-ttu-id="3f256-256">Az elérési út toohello ELÉRÉSI változó most hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="3f256-256">Now add this path toohello PATH variable.</span></span> <span data-ttu-id="3f256-257">A sudo secure_path toocontain hello elérési toohello dotnet bináris szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="3f256-257">For sudo, edit secure_path toocontain hello path toohello dotnet binary:</span></span>
```bash 
sudo visudo
### Append hello path found in hello preceding example too'secure_path' variable
```

<span data-ttu-id="3f256-258">Ebben a példában secure_path változó olvassa be, mint:</span><span class="sxs-lookup"><span data-stu-id="3f256-258">In this example, secure_path variable reads as:</span></span>

```
secure_path = /sbin:/bin:/usr/sbin:/usr/bin:/opt/rh/rh-dotnetcore11/root/usr/bin/
```

<span data-ttu-id="3f256-259">Hello aktuális felhasználó esetében.bash_profile/.profile tooinclude hello elérési toohello dotnet bináris PATH változóban szerkesztése</span><span class="sxs-lookup"><span data-stu-id="3f256-259">For hello current user, edit .bash_profile/.profile tooinclude hello path toohello dotnet binary in PATH variable</span></span> 
```bash
vi ~/.bash_profile
### Append hello path found in hello preceding example too'PATH' variable
```

<span data-ttu-id="3f256-260">Győződjön meg arról, hogy a .NET Core most az elérési út:</span><span class="sxs-lookup"><span data-stu-id="3f256-260">Verify that .NET Core is now in PATH:</span></span>
```bash
which dotnet
sudo which dotnet
```

### <a name="error-installing-azcopy"></a><span data-ttu-id="3f256-261">Hiba történt az AzCopy telepítése</span><span class="sxs-lookup"><span data-stu-id="3f256-261">Error Installing AzCopy</span></span>
<span data-ttu-id="3f256-262">Ha hibát tapasztal az AzCopy telepítési, megpróbálhatja újból hello bash parancsfájlok használatát hello AzCopy kibontott toorun `azcopy` mappa.</span><span class="sxs-lookup"><span data-stu-id="3f256-262">If you encounter issues with AzCopy installation, you may try toorun AzCopy using hello bash script in hello extracted `azcopy` folder.</span></span>

```bash
cd azcopy
./azcopy
```

### <a name="limit-concurrent-writes-while-copying-data"></a><span data-ttu-id="3f256-263">Adatok másolása egyidejű írási műveletek korlátozása</span><span class="sxs-lookup"><span data-stu-id="3f256-263">Limit concurrent writes while copying data</span></span>
<span data-ttu-id="3f256-264">AzCopy rendelkező fájlokat vagy a BLOB másolása esetén vegye figyelembe, hogy egy másik alkalmazás módosítja hello adatokat másol, amíg.</span><span class="sxs-lookup"><span data-stu-id="3f256-264">When you copy blobs or files with AzCopy, keep in mind that another application may be modifying hello data while you are copying it.</span></span> <span data-ttu-id="3f256-265">Ha lehetséges győződjön meg arról, hogy hello adatok másolása nem áll módosítás alatt hello másolási művelet során.</span><span class="sxs-lookup"><span data-stu-id="3f256-265">If possible, ensure that hello data you are copying is not being modified during hello copy operation.</span></span> <span data-ttu-id="3f256-266">Például amikor egy Azure virtuális géphez társított virtuális merevlemez másolása, győződjön meg arról, hogy más alkalmazás nem jelenleg írás toohello VHD-t.</span><span class="sxs-lookup"><span data-stu-id="3f256-266">For example, when copying a VHD associated with an Azure virtual machine, make sure that no other applications are currently writing toohello VHD.</span></span> <span data-ttu-id="3f256-267">Egy jó módszer toodo ezen nem lízing hello erőforrás toobe másolva.</span><span class="sxs-lookup"><span data-stu-id="3f256-267">A good way toodo this is by leasing hello resource toobe copied.</span></span> <span data-ttu-id="3f256-268">Alternatív megoldásként pillanatkép létrehozása a virtuális merevlemez hello először, és másolja hello pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="3f256-268">Alternately, you can create a snapshot of hello VHD first and then copy hello snapshot.</span></span>

<span data-ttu-id="3f256-269">Ha más alkalmazásokat tooblobs vagy fájlok írásában közben másolni, akkor ne feledje, hogy hello idő hello feladat befejezése nem megelőzése érdekében hello másolt erőforrások már nincs teljes paritásos hello forrás erőforrásokkal.</span><span class="sxs-lookup"><span data-stu-id="3f256-269">If you cannot prevent other applications from writing tooblobs or files while they are being copied, then keep in mind that by hello time hello job finishes, hello copied resources may no longer have full parity with hello source resources.</span></span>

### <a name="run-one-azcopy-instance-on-one-machine"></a><span data-ttu-id="3f256-270">Futtassa az AzCopy egy példány egy számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3f256-270">Run one AzCopy instance on one machine.</span></span>
<span data-ttu-id="3f256-271">AzCopy tervezett toomaximize hello kihasználtságát a gép erőforrás tooaccelerate hello adatátviteli, azt javasoljuk, hogy csak egy AzCopy példány egy számítógépen futtatja, és hello beállítást adja meg `--parallel-level` Ha több egyidejű műveletek van szüksége.</span><span class="sxs-lookup"><span data-stu-id="3f256-271">AzCopy is designed toomaximize hello utilization of your machine resource tooaccelerate hello data transfer, we recommend you run only one AzCopy instance on one machine, and specify hello option `--parallel-level` if you need more concurrent operations.</span></span> <span data-ttu-id="3f256-272">További tudnivalókért írja be a `AzCopy --help parallel-level` hello parancssorból.</span><span class="sxs-lookup"><span data-stu-id="3f256-272">For more details, type `AzCopy --help parallel-level` at hello command line.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3f256-273">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3f256-273">Next steps</span></span>
<span data-ttu-id="3f256-274">Azure Storage és AzCopy kapcsolatos további információkért tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="3f256-274">For more information about Azure Storage and AzCopy, see hello following resources:</span></span>

### <a name="azure-storage-documentation"></a><span data-ttu-id="3f256-275">Az Azure Storage-dokumentáció:</span><span class="sxs-lookup"><span data-stu-id="3f256-275">Azure Storage documentation:</span></span>
* [<span data-ttu-id="3f256-276">Bevezetés tooAzure tároló</span><span class="sxs-lookup"><span data-stu-id="3f256-276">Introduction tooAzure Storage</span></span>](storage-introduction.md)
* [<span data-ttu-id="3f256-277">Tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="3f256-277">Create a storage account</span></span>](storage-create-storage-account.md)
* [<span data-ttu-id="3f256-278">A Tártallózó alkalmazással blobok kezelése</span><span class="sxs-lookup"><span data-stu-id="3f256-278">Manage blobs with Storage Explorer</span></span>](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-explorer-blobs)
* [<span data-ttu-id="3f256-279">Az Azure Storage hello Azure CLI 2.0 használatával</span><span class="sxs-lookup"><span data-stu-id="3f256-279">Using hello Azure CLI 2.0 with Azure Storage</span></span>](storage-azure-cli.md)
* [<span data-ttu-id="3f256-280">Hogyan toouse Blob storage-ának C++</span><span class="sxs-lookup"><span data-stu-id="3f256-280">How toouse Blob storage from C++</span></span>](storage-c-plus-plus-how-to-use-blobs.md)
* [<span data-ttu-id="3f256-281">Hogyan toouse Blob storage-ának Java</span><span class="sxs-lookup"><span data-stu-id="3f256-281">How toouse Blob storage from Java</span></span>](storage-java-how-to-use-blob-storage.md)
* [<span data-ttu-id="3f256-282">Hogyan toouse Blob-tároló Node.js-ből</span><span class="sxs-lookup"><span data-stu-id="3f256-282">How toouse Blob storage from Node.js</span></span>](storage-nodejs-how-to-use-blob-storage.md)
* [<span data-ttu-id="3f256-283">Hogyan toouse Blob storage-ának Python</span><span class="sxs-lookup"><span data-stu-id="3f256-283">How toouse Blob storage from Python</span></span>](storage-python-how-to-use-blob-storage.md)

### <a name="azure-storage-blog-posts"></a><span data-ttu-id="3f256-284">Az Azure Storage blogbejegyzések:</span><span class="sxs-lookup"><span data-stu-id="3f256-284">Azure Storage blog posts:</span></span>
* [<span data-ttu-id="3f256-285">AzCopy lévő Linux előzetes bejelentése</span><span class="sxs-lookup"><span data-stu-id="3f256-285">Announcing AzCopy on Linux Preview</span></span>](https://azure.microsoft.com/en-in/blog/announcing-azcopy-on-linux-preview/)
* [<span data-ttu-id="3f256-286">Introducing Azure Storage adatátviteli könyvtár megtekintés</span><span class="sxs-lookup"><span data-stu-id="3f256-286">Introducing Azure Storage Data Movement Library Preview</span></span>](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [<span data-ttu-id="3f256-287">AzCopy: Bevezetéséről szinkron másolatot és testreszabott tartalom típusa</span><span class="sxs-lookup"><span data-stu-id="3f256-287">AzCopy: Introducing synchronous copy and customized content type</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [<span data-ttu-id="3f256-288">AzCopy: Általános rendelkezésre állási az AzCopy 3.0 és a tábla és a fájl-támogatással rendelkező az AzCopy 4.0 előzetes bejelentése</span><span class="sxs-lookup"><span data-stu-id="3f256-288">AzCopy: Announcing General Availability of AzCopy 3.0 plus preview release of AzCopy 4.0 with Table and File support</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [<span data-ttu-id="3f256-289">AzCopy: A felügyeleti teendők központjaként másolással optimalizált</span><span class="sxs-lookup"><span data-stu-id="3f256-289">AzCopy: Optimized for Large-Scale Copy Scenarios</span></span>](http://go.microsoft.com/fwlink/?LinkId=507682)
* [<span data-ttu-id="3f256-290">AzCopy: Írásvédett georedundáns tárolás támogatása</span><span class="sxs-lookup"><span data-stu-id="3f256-290">AzCopy: Support for read-access geo-redundant storage</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [<span data-ttu-id="3f256-291">AzCopy: Adatátvitelt újraindítható móddal és SAS-jogkivonat</span><span class="sxs-lookup"><span data-stu-id="3f256-291">AzCopy: Transfer data with restartable mode and SAS token</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [<span data-ttu-id="3f256-292">AzCopy: Kereszt-fiók másolási Blob használatával</span><span class="sxs-lookup"><span data-stu-id="3f256-292">AzCopy: Using cross-account Copy Blob</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [<span data-ttu-id="3f256-293">AzCopy: Feltöltése/fájlok letöltése Azure Blobokra vonatkozó</span><span class="sxs-lookup"><span data-stu-id="3f256-293">AzCopy: Uploading/downloading files for Azure Blobs</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

