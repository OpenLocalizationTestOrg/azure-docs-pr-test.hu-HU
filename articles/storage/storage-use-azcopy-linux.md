---
title: "Másolja, vagy helyezze át az adatok Azure Storage az AzCopy Linux |} Microsoft Docs"
description: "Linux-segédprogram az AzCopy segítségével áthelyezi vagy másolja az adatokat, vagy a blob és a fájl tartalmát. Adatok másolása az Azure Storage a helyi fájlokból, vagy másolja az adatokat belül vagy tárfiókok között. Adatok áttelepítése egyszerű Azure Storage."
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
ms.openlocfilehash: d17f63dcee590529756d48d699f78b3fb30f973c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="transfer-data-with-azcopy-on-linux"></a><span data-ttu-id="90adc-105">Adatátvitel az AzCopy Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="90adc-105">Transfer data with AzCopy on Linux</span></span>
<span data-ttu-id="90adc-106">AzCopy Linux egy parancssori segédprogram, az adatok másolása, és a Microsoft Azure-Blob és a fájl tárolási egyszerű parancsokkal optimális teljesítménnyel.</span><span class="sxs-lookup"><span data-stu-id="90adc-106">AzCopy on Linux is a command-line utility designed for copying data to and from Microsoft Azure Blob and File storage using simple commands with optimal performance.</span></span> <span data-ttu-id="90adc-107">Másolhatja adatok egy objektum a másikra a tárfiókon belül vagy tárfiókok között.</span><span class="sxs-lookup"><span data-stu-id="90adc-107">You can copy data from one object to another within your storage account, or between storage accounts.</span></span>

<span data-ttu-id="90adc-108">AzCopy, letöltheti a két verziója van.</span><span class="sxs-lookup"><span data-stu-id="90adc-108">There are two versions of AzCopy that you can download.</span></span> <span data-ttu-id="90adc-109">AzCopy Linux ajánlat POSIX-stílusú parancssori kapcsolók Linux platformon célozza .NET Core keretrendszerrel épül.</span><span class="sxs-lookup"><span data-stu-id="90adc-109">AzCopy on Linux is built with .NET Core Framework, which targets Linux platforms offering POSIX style command-line options.</span></span> <span data-ttu-id="90adc-110">[A Windows AzCopy](storage-use-azcopy.md) a .NET-keretrendszer épül, és ez biztosítja a Windows stílus parancssori kapcsolókat.</span><span class="sxs-lookup"><span data-stu-id="90adc-110">[AzCopy on Windows](storage-use-azcopy.md) is built with .NET Framework, and offers Windows style command-line options.</span></span> <span data-ttu-id="90adc-111">Ez a cikk a Linux AzCopy ismerteti.</span><span class="sxs-lookup"><span data-stu-id="90adc-111">This article covers AzCopy on Linux.</span></span>

## <a name="download-and-install-azcopy"></a><span data-ttu-id="90adc-112">Töltse le és telepítse az AzCopy</span><span class="sxs-lookup"><span data-stu-id="90adc-112">Download and install AzCopy</span></span>
### <a name="installation-on-linux"></a><span data-ttu-id="90adc-113">Linux-telepítés</span><span class="sxs-lookup"><span data-stu-id="90adc-113">Installation on Linux</span></span>

<span data-ttu-id="90adc-114">AzCopy Linux szüksége van a .NET Core keretrendszer a platformon.</span><span class="sxs-lookup"><span data-stu-id="90adc-114">AzCopy on Linux requires .NET Core framework on the platform.</span></span> <span data-ttu-id="90adc-115">A telepítési utasításokat tekintse meg a [.NET Core](https://www.microsoft.com/net/core#linuxubuntu) lap.</span><span class="sxs-lookup"><span data-stu-id="90adc-115">See the installation instructions on the [.NET Core](https://www.microsoft.com/net/core#linuxubuntu) page.</span></span>

<span data-ttu-id="90adc-116">Tegyük fel most telepítse a .NET Core Ubuntu 16.10.</span><span class="sxs-lookup"><span data-stu-id="90adc-116">As an example, let's install .NET Core on Ubuntu 16.10.</span></span> <span data-ttu-id="90adc-117">A legfrissebb telepítési útmutató a Microsoft [.NET Core Linux](https://www.microsoft.com/net/core#linuxubuntu) telepítési oldal.</span><span class="sxs-lookup"><span data-stu-id="90adc-117">For the latest installation guide, visit [.NET Core on Linux](https://www.microsoft.com/net/core#linuxubuntu) installation page.</span></span>


```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ yakkety main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
sudo apt-get update
sudo apt-get install dotnet-dev-1.0.3
```

<span data-ttu-id="90adc-118">A .NET Core telepítése után töltse le és telepítse az AzCopy.</span><span class="sxs-lookup"><span data-stu-id="90adc-118">Once you have installed .NET Core, download and install AzCopy.</span></span>

```bash
wget -O azcopy.tar.gz https://aka.ms/downloadazcopyprlinux
tar -xf azcopy.tar.gz
sudo ./install.sh
```

<span data-ttu-id="90adc-119">A kibontott fájlokat is távolítható el, ha az AzCopy Linux rendszeren telepítve van.</span><span class="sxs-lookup"><span data-stu-id="90adc-119">You can remove the extracted files once AzCopy on Linux is installed.</span></span> <span data-ttu-id="90adc-120">Másik lehetőségként nem jogosult felügyelő, ha is futtathatja a kibontott mappában található a "azcopy" parancsfájl segítségével AzCopy.</span><span class="sxs-lookup"><span data-stu-id="90adc-120">Alternatively if you do not have superuser privileges, you can also run AzCopy using the shell script 'azcopy' in the extracted folder.</span></span> 

### <a name="alternative-installation-on-ubuntu"></a><span data-ttu-id="90adc-121">Ubuntu alternatív telepítése</span><span class="sxs-lookup"><span data-stu-id="90adc-121">Alternative Installation on Ubuntu</span></span>

<span data-ttu-id="90adc-122">**Ubuntu 14.04**</span><span class="sxs-lookup"><span data-stu-id="90adc-122">**Ubuntu 14.04**</span></span>

<span data-ttu-id="90adc-123">Apt-forrás hozzáadása a .net Core:</span><span class="sxs-lookup"><span data-stu-id="90adc-123">Add apt source for .Net Core:</span></span>

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

<span data-ttu-id="90adc-124">Microsoft Linux termék tárház apt forrás hozzáadása, és telepítse az AzCopy:</span><span class="sxs-lookup"><span data-stu-id="90adc-124">Add apt source for Microsoft Linux product repository and install AzCopy:</span></span>

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

<span data-ttu-id="90adc-125">**Ubuntu 16.04**</span><span class="sxs-lookup"><span data-stu-id="90adc-125">**Ubuntu 16.04**</span></span>

<span data-ttu-id="90adc-126">Apt-forrás hozzáadása a .net Core:</span><span class="sxs-lookup"><span data-stu-id="90adc-126">Add apt source for .Net Core:</span></span>

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

<span data-ttu-id="90adc-127">Microsoft Linux termék tárház apt forrás hozzáadása, és telepítse az AzCopy:</span><span class="sxs-lookup"><span data-stu-id="90adc-127">Add apt source for Microsoft Linux product repository and install AzCopy:</span></span>

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

<span data-ttu-id="90adc-128">**Ubuntu 16.10**</span><span class="sxs-lookup"><span data-stu-id="90adc-128">**Ubuntu 16.10**</span></span>

<span data-ttu-id="90adc-129">Apt-forrás hozzáadása a .net Core:</span><span class="sxs-lookup"><span data-stu-id="90adc-129">Add apt source for .Net Core:</span></span>

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ yakkety main" > /etc/apt/sources.list.d/dotnetdev.list' 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
```

<span data-ttu-id="90adc-130">Microsoft Linux termék tárház apt forrás hozzáadása, és telepítse az AzCopy:</span><span class="sxs-lookup"><span data-stu-id="90adc-130">Add apt source for Microsoft Linux product repository and install AzCopy:</span></span>

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

## <a name="writing-your-first-azcopy-command"></a><span data-ttu-id="90adc-131">Az első AzCopy parancs írása</span><span class="sxs-lookup"><span data-stu-id="90adc-131">Writing your first AzCopy command</span></span>
<span data-ttu-id="90adc-132">Az AzCopy parancs alapvető szintaxisa:</span><span class="sxs-lookup"><span data-stu-id="90adc-132">The basic syntax for AzCopy commands is:</span></span>

```azcopy
azcopy --source <source> --destination <destination> [Options]
```

<span data-ttu-id="90adc-133">Az alábbi példák bemutatják a különböző forgatókönyvek másolása adatok és a Microsoft Azure-BLOB és a fájlok.</span><span class="sxs-lookup"><span data-stu-id="90adc-133">The following examples demonstrate various scenarios for copying data to and from Microsoft Azure Blobs and Files.</span></span> <span data-ttu-id="90adc-134">Tekintse meg a `azcopy --help` menü egyes mintában használt paraméterek részletes leírását.</span><span class="sxs-lookup"><span data-stu-id="90adc-134">Refer to the `azcopy --help` menu for a detailed explanation of the parameters used in each sample.</span></span>

## <a name="blob-download"></a><span data-ttu-id="90adc-135">BLOB: letöltése</span><span class="sxs-lookup"><span data-stu-id="90adc-135">Blob: Download</span></span>
### <a name="download-single-blob"></a><span data-ttu-id="90adc-136">Egy blob letöltése</span><span class="sxs-lookup"><span data-stu-id="90adc-136">Download single blob</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="90adc-137">Ha a mappa `/mnt/myfiles` nem létezik, AzCopy létrehozza, és letölti `abc.txt ` az új mappába.</span><span class="sxs-lookup"><span data-stu-id="90adc-137">If the folder `/mnt/myfiles` does not exist, AzCopy creates it and downloads `abc.txt ` into the new folder.</span></span>

### <a name="download-single-blob-from-secondary-region"></a><span data-ttu-id="90adc-138">A másodlagos régióba egyetlen blob letöltése</span><span class="sxs-lookup"><span data-stu-id="90adc-138">Download single blob from secondary region</span></span>

```azcopy
azcopy \
    --source https://myaccount-secondary.blob.core.windows.net/mynewcontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="90adc-139">Vegye figyelembe, hogy írásvédett georedundáns tárolás engedélyezve kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="90adc-139">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="download-all-blobs"></a><span data-ttu-id="90adc-140">Töltse le az összes BLOB</span><span class="sxs-lookup"><span data-stu-id="90adc-140">Download all blobs</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

<span data-ttu-id="90adc-141">Tegyük fel, a következő blobot a megadott tárolóban találhatók:</span><span class="sxs-lookup"><span data-stu-id="90adc-141">Assume the following blobs reside in the specified container:</span></span>  

```
abc.txt
abc1.txt
abc2.txt
vd1/a.txt
vd1/abcd.txt
```

<span data-ttu-id="90adc-142">A könyvtár a letöltési művelet után `/mnt/myfiles` a következő fájlokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="90adc-142">After the download operation, the directory `/mnt/myfiles` includes the following files:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/vd1/a.txt
/mnt/myfiles/vd1/abcd.txt
```

<span data-ttu-id="90adc-143">Ha nem adja meg a beállítás `--recursive`, nincs blob le lesznek töltve.</span><span class="sxs-lookup"><span data-stu-id="90adc-143">If you do not specify option `--recursive`, no blob will be downloaded.</span></span>

### <a name="download-blobs-with-specified-prefix"></a><span data-ttu-id="90adc-144">A megadott előtag blobok letöltése</span><span class="sxs-lookup"><span data-stu-id="90adc-144">Download blobs with specified prefix</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "a" \
    --recursive
```

<span data-ttu-id="90adc-145">Tegyük fel, a következő blobok találhatók a megadott tárolóban.</span><span class="sxs-lookup"><span data-stu-id="90adc-145">Assume the following blobs reside in the specified container.</span></span> <span data-ttu-id="90adc-146">A előtaggal kezdődő összes BLOB `a` letöltődnek.</span><span class="sxs-lookup"><span data-stu-id="90adc-146">All blobs beginning with the prefix `a` are downloaded.</span></span>

```
abc.txt
abc1.txt
abc2.txt
xyz.txt
vd1\a.txt
vd1\abcd.txt
```

<span data-ttu-id="90adc-147">A letöltési mappa művelet után `/mnt/myfiles` a következő fájlokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="90adc-147">After the download operation, the folder `/mnt/myfiles` includes the following files:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
```

<span data-ttu-id="90adc-148">Az előtag a virtuális könyvtárban, a blob nevének első részét képező vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="90adc-148">The prefix applies to the virtual directory, which forms the first part of the blob name.</span></span> <span data-ttu-id="90adc-149">A fenti példában a virtuális könyvtár nem egyezik a megadott előtagot, így nincs blob letöltődik.</span><span class="sxs-lookup"><span data-stu-id="90adc-149">In the example shown above, the virtual directory does not match the specified prefix, so no blob is downloaded.</span></span> <span data-ttu-id="90adc-150">Emellett ha a beállítás `--recursive` nincs megadva, az AzCopy nem tölti le a blobokat.</span><span class="sxs-lookup"><span data-stu-id="90adc-150">In addition, if the option `--recursive` is not specified, AzCopy does not download any blobs.</span></span>

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a><span data-ttu-id="90adc-151">Állítsa be az exportált fájlokat lehet ugyanaz, mint a forrás utolsó módosításának időpontja</span><span class="sxs-lookup"><span data-stu-id="90adc-151">Set the last-modified time of exported files to be same as the source blobs</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination "/mnt/myfiles" \
    --source-key <key> \
    --preserve-last-modified-time
```

<span data-ttu-id="90adc-152">Blobok emellett kizárja a letöltési művelet az utolsó módosításának ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="90adc-152">You can also exclude blobs from the download operation based on their last-modified time.</span></span> <span data-ttu-id="90adc-153">Például, ha ki szeretné zárni a blobok, amelyek utolsó módosításának ideje azonos vagy újabb, mint a célfájl, vegye fel a `--exclude-newer` lehetőséget:</span><span class="sxs-lookup"><span data-stu-id="90adc-153">For example, if you want to exclude blobs whose last modified time is the same or newer than the destination file, add the `--exclude-newer` option:</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-newer
```

<span data-ttu-id="90adc-154">Vagy ha ki szeretné zárni a blobok, amelyek utolsó módosítás időpontja nem azonos vagy régebbi, mint a célfájl, vegye fel a `--exclude-older` lehetőséget:</span><span class="sxs-lookup"><span data-stu-id="90adc-154">Or if you want to exclude blobs whose last modified time is the same or older than the destination file, add the `--exclude-older` option:</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer \
    --destination /mnt/myfiles \
    --source-key <key> \
    --preserve-last-modified-time \
    --exclude-older
```

## <a name="blob-upload"></a><span data-ttu-id="90adc-155">BLOB: feltöltése</span><span class="sxs-lookup"><span data-stu-id="90adc-155">Blob: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="90adc-156">Töltse fel egy fájlból</span><span class="sxs-lookup"><span data-stu-id="90adc-156">Upload single file</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="90adc-157">Ha a megadott célhely tároló nem létezik, az AzCopy létrehozása, és a fájlt tölt be.</span><span class="sxs-lookup"><span data-stu-id="90adc-157">If the specified destination container does not exist, AzCopy creates it and uploads the file into it.</span></span>

### <a name="upload-single-file-to-virtual-directory"></a><span data-ttu-id="90adc-158">Egy fájlból töltse fel a virtuális könyvtár</span><span class="sxs-lookup"><span data-stu-id="90adc-158">Upload single file to virtual directory</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="90adc-159">Ha a megadott virtuális könyvtár nem létezik, az AzCopy feltölti a fájlt a virtuális könyvtárat a blob nevének (*pl.*, `vd/abc.txt` a fenti példában).</span><span class="sxs-lookup"><span data-stu-id="90adc-159">If the specified virtual directory does not exist, AzCopy uploads the file to include the virtual directory in the blob name (*e.g.*, `vd/abc.txt` in the example above).</span></span>

### <a name="upload-all-files"></a><span data-ttu-id="90adc-160">Minden fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="90adc-160">Upload all files</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --recursive
```

<span data-ttu-id="90adc-161">Beállítás megadása `--recursive` feltölt Blob storage rekurzív módon, ami azt jelenti, hogy minden almappa és a fájlok feltöltése, valamint a megadott könyvtár tartalmát.</span><span class="sxs-lookup"><span data-stu-id="90adc-161">Specifying option `--recursive` uploads the contents of the specified directory to Blob storage recursively, meaning that all subfolders and their files are uploaded as well.</span></span> <span data-ttu-id="90adc-162">Például a mappában találhatók a következő fájlok feltételezik `/mnt/myfiles`:</span><span class="sxs-lookup"><span data-stu-id="90adc-162">For instance, assume the following files reside in folder `/mnt/myfiles`:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

<span data-ttu-id="90adc-163">A tároló a feltöltési művelet után a következő fájlokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="90adc-163">After the upload operation, the container includes the following files:</span></span>

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

<span data-ttu-id="90adc-164">Ha a beállítás `--recursive` nincs megadva, csak a következő három fájlok feltöltése:</span><span class="sxs-lookup"><span data-stu-id="90adc-164">When the option `--recursive` is not specified, only the following three files are uploaded:</span></span>

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="90adc-165">Töltse fel a megadott mintának megfelelő fájlok</span><span class="sxs-lookup"><span data-stu-id="90adc-165">Upload files matching specified pattern</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --include "a*" \
    --recursive
```

<span data-ttu-id="90adc-166">Tegyük fel, a következő fájlok mappában találhatók `/mnt/myfiles`:</span><span class="sxs-lookup"><span data-stu-id="90adc-166">Assume the following files reside in folder `/mnt/myfiles`:</span></span>

```
/mnt/myfiles/abc.txt
/mnt/myfiles/abc1.txt
/mnt/myfiles/abc2.txt
/mnt/myfiles/xyz.txt
/mnt/myfiles/subfolder/a.txt
/mnt/myfiles/subfolder/abcd.txt
```

<span data-ttu-id="90adc-167">A tároló a feltöltési művelet után a következő fájlokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="90adc-167">After the upload operation, the container includes the following files:</span></span>

```
abc.txt
abc1.txt
abc2.txt
subfolder/a.txt
subfolder/abcd.txt
```

<span data-ttu-id="90adc-168">Ha a beállítás `--recursive` nincs megadva, az AzCopy kihagyja alkönyvtár lévő fájlok:</span><span class="sxs-lookup"><span data-stu-id="90adc-168">When the option `--recursive` is not specified, AzCopy skips files that are in sub-directories:</span></span>

```
abc.txt
abc1.txt
abc2.txt
```

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a><span data-ttu-id="90adc-169">Adjon meg egy cél blob MIME tartalomtípus</span><span class="sxs-lookup"><span data-stu-id="90adc-169">Specify the MIME content type of a destination blob</span></span>
<span data-ttu-id="90adc-170">Alapértelmezés szerint az AzCopy beállítja egy cél blobot tartalomtípusa `application/octet-stream`.</span><span class="sxs-lookup"><span data-stu-id="90adc-170">By default, AzCopy sets the content type of a destination blob to `application/octet-stream`.</span></span> <span data-ttu-id="90adc-171">Azonban közvetlenül megadhatja a tartalomtípus keresztül beállítás `--set-content-type [content-type]`.</span><span class="sxs-lookup"><span data-stu-id="90adc-171">However, you can explicitly specify the content type via the option `--set-content-type [content-type]`.</span></span> <span data-ttu-id="90adc-172">Ez a szintaxis a feltöltési művelet állítja be a content-type összes BLOB.</span><span class="sxs-lookup"><span data-stu-id="90adc-172">This syntax sets the content type for all blobs in an upload operation.</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type "video/mp4"
```

<span data-ttu-id="90adc-173">Ha a beállítás `--set-content-type` egy érték nélkül van megadva, majd az AzCopy állítja be, minden egyes blob vagy a fájl tartalomtípusa alapján a fájl kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="90adc-173">If the option `--set-content-type` is specified without a value, then AzCopy sets each blob or file's content type according to its file extension.</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/myContainer/ \
    --dest-key <key> \
    --include "ab" \
    --set-content-type
```

## <a name="blob-copy"></a><span data-ttu-id="90adc-174">BLOB: másolása</span><span class="sxs-lookup"><span data-stu-id="90adc-174">Blob: Copy</span></span>
### <a name="copy-single-blob-within-storage-account"></a><span data-ttu-id="90adc-175">Másolja át egy blob Storage-fiókban</span><span class="sxs-lookup"><span data-stu-id="90adc-175">Copy single blob within Storage account</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1 \
    --destination https://myaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key> \
    --dest-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="90adc-176">Ha--másolatának szinkronizálása beállítás nélkül egy blobot másol egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.</span><span class="sxs-lookup"><span data-stu-id="90adc-176">When you copy a blob without --sync-copy option, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-across-storage-accounts"></a><span data-ttu-id="90adc-177">Másolja át egy blob Storage-fiókok között</span><span class="sxs-lookup"><span data-stu-id="90adc-177">Copy single blob across Storage accounts</span></span>

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1 \
    --destination https://destaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt"
```

<span data-ttu-id="90adc-178">Ha--másolatának szinkronizálása beállítás nélkül egy blobot másol egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.</span><span class="sxs-lookup"><span data-stu-id="90adc-178">When you copy a blob without --sync-copy option, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a><span data-ttu-id="90adc-179">Egy blob másolása másodlagos régióba elsődleges régió</span><span class="sxs-lookup"><span data-stu-id="90adc-179">Copy single blob from secondary region to primary region</span></span>

```azcopy
azcopy \
    --source https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 \
    --destination https://myaccount2.blob.core.windows.net/mynewcontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt"
```

<span data-ttu-id="90adc-180">Vegye figyelembe, hogy írásvédett georedundáns tárolás engedélyezve kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="90adc-180">Note that you must have read-access geo-redundant storage enabled.</span></span>

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a><span data-ttu-id="90adc-181">Egy blob másolási és a pillanatképek tárfiókok között</span><span class="sxs-lookup"><span data-stu-id="90adc-181">Copy single blob and its snapshots across Storage accounts</span></span>

```azcopy
azcopy \
    --source https://sourceaccount.blob.core.windows.net/mycontainer1 \
    --destination https://destaccount.blob.core.windows.net/mycontainer2 \
    --source-key <key1> \
    --dest-key <key2> \
    --include "abc.txt" \
    --include-snapshot
```

<span data-ttu-id="90adc-182">A másolási művelet után a céltároló tartalmaz a blob és a pillanatképek.</span><span class="sxs-lookup"><span data-stu-id="90adc-182">After the copy operation, the target container includes the blob and its snapshots.</span></span> <span data-ttu-id="90adc-183">A tároló a következő blob és a pillanatképek tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="90adc-183">The container includes the following blob and its snapshots:</span></span>

```
abc.txt
abc (2013-02-25 080757).txt
abc (2014-02-21 150331).txt
```

### <a name="synchronously-copy-blobs-across-storage-accounts"></a><span data-ttu-id="90adc-184">Szinkron módon másolni BLOB Storage-fiókok</span><span class="sxs-lookup"><span data-stu-id="90adc-184">Synchronously copy blobs across Storage accounts</span></span>
<span data-ttu-id="90adc-185">AzCopy alapértelmezés szerint aszinkron módon másolja az adatokat két tárolási végpontok közötti.</span><span class="sxs-lookup"><span data-stu-id="90adc-185">AzCopy by default copies data between two storage endpoints asynchronously.</span></span> <span data-ttu-id="90adc-186">Ezért a másolási művelet fut a háttérben, amelynek nincs szolgáltatásiszint-szerződés szempontjából hogyan gyors blob kapacitás tartalékolt sávszélesség használatával kell másolni.</span><span class="sxs-lookup"><span data-stu-id="90adc-186">Therefore, the copy operation runs in the background using spare bandwidth capacity that has no SLA in terms of how fast a blob is copied.</span></span> 

<span data-ttu-id="90adc-187">A `--sync-copy` lehetőség biztosítja, hogy a másolási művelet lekérdezi az egységes sebesség.</span><span class="sxs-lookup"><span data-stu-id="90adc-187">The `--sync-copy` option ensures that the copy operation gets consistent speed.</span></span> <span data-ttu-id="90adc-188">AzCopy a szinkron másolatot helyi memória a megadott forrás másolása a blobok letöltése, és feltöltheti a Blob storage cél végez.</span><span class="sxs-lookup"><span data-stu-id="90adc-188">AzCopy performs the synchronous copy by downloading the blobs to copy from the specified source to local memory, and then uploading them to the Blob storage destination.</span></span>

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/myContainer/ \
    --destination https://myaccount2.blob.core.windows.net/myContainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --include "ab" \
    --sync-copy
```

<span data-ttu-id="90adc-189">`--sync-copy`További kilépő költség képest aszinkron másolási hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="90adc-189">`--sync-copy` might generate additional egress cost compared to asynchronous copy.</span></span> <span data-ttu-id="90adc-190">Az ajánlott módszer, hogy ezt a beállítást használja egy Azure virtuális gép, és a kimenő forgalom költségek elkerülése érdekében forrás tárfiók ugyanabban a régióban található.</span><span class="sxs-lookup"><span data-stu-id="90adc-190">The recommended approach is to use this option in an Azure VM, that is in the same region as your source storage account to avoid egress cost.</span></span>

## <a name="file-download"></a><span data-ttu-id="90adc-191">Fájl: letöltése</span><span class="sxs-lookup"><span data-stu-id="90adc-191">File: Download</span></span>
### <a name="download-single-file"></a><span data-ttu-id="90adc-192">Töltse le egy fájlból</span><span class="sxs-lookup"><span data-stu-id="90adc-192">Download single file</span></span>

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/myfolder1/ \
    --destination /mnt/myfiles \
    --source-key <key> \
    --include "abc.txt"
```

<span data-ttu-id="90adc-193">Ha a megadott forrás az Azure fájlmegosztások, akkor meg kell adnia a fájl pontos nevét (*pl.* `abc.txt`) egyetlen fájl letöltéséhez vagy beállítást adja meg `--recursive` a megosztás rekurzív módon található összes fájl letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="90adc-193">If the specified source is an Azure file share, then you must either specify the exact file name, (*e.g.* `abc.txt`) to download a single file, or specify option `--recursive` to download all files in the share recursively.</span></span> <span data-ttu-id="90adc-194">Adjon meg egy fájl mintát és a beállítás próbál `--recursive` hiba együtt eredményez.</span><span class="sxs-lookup"><span data-stu-id="90adc-194">Attempting to specify both a file pattern and option `--recursive` together results in an error.</span></span>

### <a name="download-all-files"></a><span data-ttu-id="90adc-195">Minden fájl letöltése</span><span class="sxs-lookup"><span data-stu-id="90adc-195">Download all files</span></span>

```azcopy
azcopy \
    --source https://myaccount.file.core.windows.net/myfileshare/ \
    --destination /mnt/myfiles \
    --source-key <key> \
    --recursive
```

<span data-ttu-id="90adc-196">Vegye figyelembe, hogy a rendszer nem tölti le üres mappák.</span><span class="sxs-lookup"><span data-stu-id="90adc-196">Note that any empty folders are not downloaded.</span></span>

## <a name="file-upload"></a><span data-ttu-id="90adc-197">Fájl: feltöltése</span><span class="sxs-lookup"><span data-stu-id="90adc-197">File: Upload</span></span>
### <a name="upload-single-file"></a><span data-ttu-id="90adc-198">Töltse fel egy fájlból</span><span class="sxs-lookup"><span data-stu-id="90adc-198">Upload single file</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --include abc.txt
```

### <a name="upload-all-files"></a><span data-ttu-id="90adc-199">Minden fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="90adc-199">Upload all files</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --recursive
```

<span data-ttu-id="90adc-200">Vegye figyelembe, hogy az üres mappák nem töltődött fel.</span><span class="sxs-lookup"><span data-stu-id="90adc-200">Note that any empty folders are not uploaded.</span></span>

### <a name="upload-files-matching-specified-pattern"></a><span data-ttu-id="90adc-201">Töltse fel a megadott mintának megfelelő fájlok</span><span class="sxs-lookup"><span data-stu-id="90adc-201">Upload files matching specified pattern</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.file.core.windows.net/myfileshare/ \
    --dest-key <key> \
    --include "ab*" \
    --recursive
```

## <a name="file-copy"></a><span data-ttu-id="90adc-202">Fájl: másolása</span><span class="sxs-lookup"><span data-stu-id="90adc-202">File: Copy</span></span>
### <a name="copy-across-file-shares"></a><span data-ttu-id="90adc-203">Fájlmegosztások másolni</span><span class="sxs-lookup"><span data-stu-id="90adc-203">Copy across file shares</span></span>

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
<span data-ttu-id="90adc-204">Amikor fájlmegosztások között másolhat egy fájlt egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.</span><span class="sxs-lookup"><span data-stu-id="90adc-204">When you copy a file across file shares, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-from-file-share-to-blob"></a><span data-ttu-id="90adc-205">Megosztás és a blob másolása</span><span class="sxs-lookup"><span data-stu-id="90adc-205">Copy from file share to blob</span></span>

```azcopy
azcopy \ 
    --source https://myaccount1.file.core.windows.net/myfileshare/ \
    --destination https://myaccount2.blob.core.windows.net/mycontainer/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
<span data-ttu-id="90adc-206">Amikor másolhat egy fájlt a blobra, fájlmegosztásról egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.</span><span class="sxs-lookup"><span data-stu-id="90adc-206">When you copy a file from file share to blob, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="copy-from-blob-to-file-share"></a><span data-ttu-id="90adc-207">Fájlmegosztás a blob másolása</span><span class="sxs-lookup"><span data-stu-id="90adc-207">Copy from blob to file share</span></span>

```azcopy
azcopy \
    --source https://myaccount1.blob.core.windows.net/mycontainer/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive
```
<span data-ttu-id="90adc-208">Másolhat egy fájlt blobból fájlmegosztáshoz, amikor egy [kiszolgálóoldali másolatot](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) művelet.</span><span class="sxs-lookup"><span data-stu-id="90adc-208">When you copy a file from blob to file share, a [server-side copy](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) operation is performed.</span></span>

### <a name="synchronously-copy-files"></a><span data-ttu-id="90adc-209">Szinkron módon történik a fájlok másolása</span><span class="sxs-lookup"><span data-stu-id="90adc-209">Synchronously copy files</span></span>
<span data-ttu-id="90adc-210">Megadhatja a `--sync-copy` beállítás adatokat másolni a File Storage a File Storage, File Storage-Blob Storage és a Blob Storage a File Storage szinkron módon történik.</span><span class="sxs-lookup"><span data-stu-id="90adc-210">You can specify the `--sync-copy` option to copy data from File Storage to File Storage, from File Storage to Blob Storage and from Blob Storage to File Storage synchronously.</span></span> <span data-ttu-id="90adc-211">AzCopy futtatja ezt a műveletet az adatok letöltése a helyi memória, és majd ismét feltölteni a cél.</span><span class="sxs-lookup"><span data-stu-id="90adc-211">AzCopy runs this operation by downloading the source data to local memory, and then uploading it to destination.</span></span> <span data-ttu-id="90adc-212">Ebben az esetben a szabványos kilépő költség érvényes.</span><span class="sxs-lookup"><span data-stu-id="90adc-212">In this case, standard egress cost applies.</span></span>

```azcopy
azcopy \
    --source https://myaccount1.file.core.windows.net/myfileshare1/ \
    --destination https://myaccount2.file.core.windows.net/myfileshare2/ \
    --source-key <key1> \
    --dest-key <key2> \
    --recursive \
    --sync-copy
```

<span data-ttu-id="90adc-213">Amikor másol a File Storage Blob Storage, az alapértelmezett blob típushoz blokkblob, felhasználói beállítást adja meg `/BlobType:page` módosíthatja a blob típusára.</span><span class="sxs-lookup"><span data-stu-id="90adc-213">When copying from File Storage to Blob Storage, the default blob type is block blob, user can specify option `/BlobType:page` to change the destination blob type.</span></span>

<span data-ttu-id="90adc-214">Vegye figyelembe, hogy `--sync-copy` további költségeket, aszinkron másolási való hasonlítás kilépő hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="90adc-214">Note that `--sync-copy` might generate additional egress cost comparing to asynchronous copy.</span></span> <span data-ttu-id="90adc-215">Az ajánlott módszer, hogy ezt a beállítást használja egy Azure virtuális gép, és a kimenő forgalom költségek elkerülése érdekében forrás tárfiók ugyanabban a régióban található.</span><span class="sxs-lookup"><span data-stu-id="90adc-215">The recommended approach is to use this option in an Azure VM, that is in the same region as your source storage account to avoid egress cost.</span></span>

## <a name="other-azcopy-features"></a><span data-ttu-id="90adc-216">Más AzCopy szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="90adc-216">Other AzCopy features</span></span>
### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a><span data-ttu-id="90adc-217">Csak másolja az adatokat, a cél nem létezik</span><span class="sxs-lookup"><span data-stu-id="90adc-217">Only copy data that doesn't exist in the destination</span></span>
<span data-ttu-id="90adc-218">A `--exclude-older` és `--exclude-newer` paraméterek lehetővé teszik a régebbi vagy újabb forrás erőforrások másolását, illetve kizárása.</span><span class="sxs-lookup"><span data-stu-id="90adc-218">The `--exclude-older` and `--exclude-newer` parameters allow you to exclude older or newer source resources from being copied, respectively.</span></span> <span data-ttu-id="90adc-219">Ha szeretné, amelyek nem léteznek a cél a forrás-erőforrások másolása, mindkét paraméter az AzCopy parancs adhat meg:</span><span class="sxs-lookup"><span data-stu-id="90adc-219">If you only want to copy source resources that don't exist in the destination, you can specify both parameters in the AzCopy command:</span></span>

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --exclude-older --exclude-newer

    --source /mnt/myfiles --destination http://myaccount.file.core.windows.net/myfileshare --dest-key <destkey> --recursive --exclude-older --exclude-newer

    --source http://myaccount.blob.core.windows.net/mycontainer --destination http://myaccount.blob.core.windows.net/mycontainer1 --source-key <sourcekey> --dest-key <destkey> --recursive --exclude-older --exclude-newer

### <a name="use-a-configuration-file-to-specify-command-line-parameters"></a><span data-ttu-id="90adc-220">A konfigurációs fájlt használja a parancssori paraméterek megadása</span><span class="sxs-lookup"><span data-stu-id="90adc-220">Use a configuration file to specify command-line parameters</span></span>

```azcopy
azcopy --config-file "azcopy-config.ini"
```

<span data-ttu-id="90adc-221">AzCopy parancssori paramétereket is megadhat a konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="90adc-221">You can include any AzCopy command-line parameters in a configuration file.</span></span> <span data-ttu-id="90adc-222">AzCopy dolgozza fel a paraméterek a fájlban, ha a parancssorban megadott lett hajt végre a fájl tartalma közvetlen helyettesítés.</span><span class="sxs-lookup"><span data-stu-id="90adc-222">AzCopy processes the parameters in the file as if they had been specified on the command line, performing a direct substitution with the contents of the file.</span></span>

<span data-ttu-id="90adc-223">Tegyük fel, a konfigurációs fájl nevű `copyoperation`, amely tartalmazza a következő sorokat.</span><span class="sxs-lookup"><span data-stu-id="90adc-223">Assume a configuration file named `copyoperation`, that contains the following lines.</span></span> <span data-ttu-id="90adc-224">Minden egyes AzCopy paraméter egy sorba adható meg.</span><span class="sxs-lookup"><span data-stu-id="90adc-224">Each AzCopy parameter can be specified on a single line.</span></span>

    --source http://myaccount.blob.core.windows.net/mycontainer --destination /mnt/myfiles --source-key <sourcekey> --recursive --quiet

<span data-ttu-id="90adc-225">vagy külön sorok:</span><span class="sxs-lookup"><span data-stu-id="90adc-225">or on separate lines:</span></span>

    --source http://myaccount.blob.core.windows.net/mycontainer
    --destination /mnt/myfiles
    --source-key<sourcekey>
    --recursive
    --quiet

<span data-ttu-id="90adc-226">AzCopy sikertelen lesz, ha a paraméter osztani két sort, az itt látható a `--source-key` paraméter:</span><span class="sxs-lookup"><span data-stu-id="90adc-226">AzCopy fails if you split the parameter across two lines, as shown here for the `--source-key` parameter:</span></span>

    http://myaccount.blob.core.windows.net/mycontainer
    /mnt/myfiles
    --sourcekey
    <sourcekey>
    --recursive
    --quiet

### <a name="specify-a-shared-access-signature-sas"></a><span data-ttu-id="90adc-227">Adja meg a közös hozzáférésű jogosultságkód (SAS)</span><span class="sxs-lookup"><span data-stu-id="90adc-227">Specify a shared access signature (SAS)</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1 \
    --destination https://myaccount.blob.core.windows.net/mycontainer2 \
    --source-sas <SAS1> \
    --dest-sas <SAS2> \
    --include abc.txt
```

<span data-ttu-id="90adc-228">Azt is megadhatja egy SAS URI tárolón:</span><span class="sxs-lookup"><span data-stu-id="90adc-228">You can also specify a SAS on the container URI:</span></span>

```azcopy
azcopy \
    --source https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken \
    --destination /mnt/myfiles \
    --recursive
```

<span data-ttu-id="90adc-229">Vegye figyelembe, hogy az AzCopy jelenleg csak az támogatja a [fiók SAS](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-shared-access-signature-part-1).</span><span class="sxs-lookup"><span data-stu-id="90adc-229">Note that AzCopy currently only supports the [Account SAS](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-shared-access-signature-part-1).</span></span>

### <a name="journal-file-folder"></a><span data-ttu-id="90adc-230">Napló fájlmappa</span><span class="sxs-lookup"><span data-stu-id="90adc-230">Journal file folder</span></span>
<span data-ttu-id="90adc-231">Minden alkalommal, amikor egy parancs kiadni az AzCopy, ellenőrzi, hogy a napló fájl megtalálható-e az alapértelmezett mappába, vagy hogy keresztül ez a beállítás a megadott mappa létezik.</span><span class="sxs-lookup"><span data-stu-id="90adc-231">Each time you issue a command to AzCopy, it checks whether a journal file exists in the default folder, or whether it exists in a folder that you specified via this option.</span></span> <span data-ttu-id="90adc-232">A napló fájl nem létezik egyik helyen sem, ha az AzCopy kezeli a művelet új, és létrehoz egy új naplófájl.</span><span class="sxs-lookup"><span data-stu-id="90adc-232">If the journal file does not exist in either place, AzCopy treats the operation as new and generates a new journal file.</span></span>

<span data-ttu-id="90adc-233">Ha a napló fájl nem létezik, AzCopy ellenőrzi, hogy a parancssor, amely a megadott megegyezik-e a parancssorban a napló fájlban.</span><span class="sxs-lookup"><span data-stu-id="90adc-233">If the journal file does exist, AzCopy checks whether the command line that you input matches the command line in the journal file.</span></span> <span data-ttu-id="90adc-234">Ha a két parancssorokat egyeznek, AzCopy folytatja a teljes műveletet.</span><span class="sxs-lookup"><span data-stu-id="90adc-234">If the two command lines match, AzCopy resumes the incomplete operation.</span></span> <span data-ttu-id="90adc-235">Ha nem egyeznek, a AzCopy felszólítja a felhasználót vagy felülírja ezt a napló fájlt egy új művelet indítása és az aktuális művelet megszakítására.</span><span class="sxs-lookup"><span data-stu-id="90adc-235">If they do not match, AzCopy prompts user to either overwrite the journal file to start a new operation, or to cancel the current operation.</span></span>

<span data-ttu-id="90adc-236">Ha szeretné a naplófájl alapértelmezett helyet használja:</span><span class="sxs-lookup"><span data-stu-id="90adc-236">If you want to use the default location for the journal file:</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --resume
```

<span data-ttu-id="90adc-237">Ha a beállítás nincs megadva `--resume`, vagy adja meg a beállítás `--resume` anélkül, hogy a mappa elérési útja, a fent látható AzCopy fájlt hoz létre a napló az alapértelmezett helyen, amely `~\Microsoft\Azure\AzCopy`.</span><span class="sxs-lookup"><span data-stu-id="90adc-237">If you omit option `--resume`, or specify option `--resume` without the folder path, as shown above, AzCopy creates the journal file in the default location, which is `~\Microsoft\Azure\AzCopy`.</span></span> <span data-ttu-id="90adc-238">Ha a napló fájl már létezik, majd AzCopy folytatja a műveletet, a napló-fájl alapján.</span><span class="sxs-lookup"><span data-stu-id="90adc-238">If the journal file already exists, then AzCopy resumes the operation based on the journal file.</span></span>

<span data-ttu-id="90adc-239">Ha szeretne egy egyéni napló elérési útját adja meg:</span><span class="sxs-lookup"><span data-stu-id="90adc-239">If you want to specify a custom location for the journal file:</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key key \
    --resume "/mnt/myjournal"
```

<span data-ttu-id="90adc-240">Ebben a példában a napló fájlt hoz létre, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="90adc-240">This example creates the journal file if it does not already exist.</span></span> <span data-ttu-id="90adc-241">Ha létezik, majd AzCopy folytatja a műveletet, a napló-fájl alapján.</span><span class="sxs-lookup"><span data-stu-id="90adc-241">If it does exist, then AzCopy resumes the operation based on the journal file.</span></span>

<span data-ttu-id="90adc-242">Ha azt szeretné, az AzCopy művelet folytatásához ismételje meg ugyanezt a parancsot.</span><span class="sxs-lookup"><span data-stu-id="90adc-242">If you want to resume an AzCopy operation, repeat the same command.</span></span> <span data-ttu-id="90adc-243">AzCopy Linux majd megerősítést kér fogja kérni:</span><span class="sxs-lookup"><span data-stu-id="90adc-243">AzCopy on Linux then will prompt for confirmation:</span></span>

```azcopy
Incomplete operation with same command line detected at the journal directory "/home/myaccount/Microsoft/Azure/AzCopy", do you want to resume the operation? Choose Yes to resume, choose No to overwrite the journal to start a new operation. (Yes/No)
```

### <a name="output-verbose-logs"></a><span data-ttu-id="90adc-244">Kimeneti részletes naplókat</span><span class="sxs-lookup"><span data-stu-id="90adc-244">Output verbose logs</span></span>

```azcopy
azcopy \
    --source /mnt/myfiles \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --verbose
```

### <a name="specify-the-number-of-concurrent-operations-to-start"></a><span data-ttu-id="90adc-245">Adja meg elindítani a párhuzamos műveletek száma</span><span class="sxs-lookup"><span data-stu-id="90adc-245">Specify the number of concurrent operations to start</span></span>
<span data-ttu-id="90adc-246">A beállítás `--parallel-level` egyidejű másolási műveletek számát adja meg.</span><span class="sxs-lookup"><span data-stu-id="90adc-246">Option `--parallel-level` specifies the number of concurrent copy operations.</span></span> <span data-ttu-id="90adc-247">Alapértelmezés szerint az AzCopy bizonyos száma párhuzamos műveletek az adatok átvitel átviteli sebesség növelése elindul.</span><span class="sxs-lookup"><span data-stu-id="90adc-247">By default, AzCopy starts a certain number of concurrent operations to increase the data transfer throughput.</span></span> <span data-ttu-id="90adc-248">A párhuzamos műveletek száma rendelkezik processzorok száma nyolc alkalommal.</span><span class="sxs-lookup"><span data-stu-id="90adc-248">The number of concurrent operations is equal eight times the number of processors you have.</span></span> <span data-ttu-id="90adc-249">AzCopy kis sávszélességű hálózaton keresztül futtatja, ha sikertelen a erőforrás konkurencia elkerülése érdekében a--párhuzamos szintű kevesebb is megadhat.</span><span class="sxs-lookup"><span data-stu-id="90adc-249">If you are running AzCopy across a low-bandwidth network, you can specify a lower number for --parallel-level to avoid failure caused by resource competition.</span></span>

[!TIP]
><span data-ttu-id="90adc-250">AzCopy paraméterek teljes listájának megtekintéséhez tekintse meg a "azcopy – súgó" menü.</span><span class="sxs-lookup"><span data-stu-id="90adc-250">To view the complete list of AzCopy parameters, check out 'azcopy --help' menu.</span></span>

## <a name="known-issues-and-best-practices"></a><span data-ttu-id="90adc-251">Ismert problémák és ajánlott eljárások</span><span class="sxs-lookup"><span data-stu-id="90adc-251">Known issues and best practices</span></span>
### <a name="error-net-core-is-not-found-in-the-system"></a><span data-ttu-id="90adc-252">Hiba: A .NET Core nem található a rendszerben.</span><span class="sxs-lookup"><span data-stu-id="90adc-252">Error: .NET Core is not found in the system.</span></span>
<span data-ttu-id="90adc-253">Ha a program hibát kap, hogy a .NET Core nincs telepítve a rendszeren, a .NET Core bináris fájl elérési ÚTJA `dotnet` lehet, hogy hiányzik.</span><span class="sxs-lookup"><span data-stu-id="90adc-253">If you encounter an error stating that .NET Core is not installed in the system, the PATH to the .NET Core binary `dotnet` may be missing.</span></span>

<span data-ttu-id="90adc-254">Ahhoz, hogy a probléma megoldásához keresse meg a .NET Core bináris a rendszerben:</span><span class="sxs-lookup"><span data-stu-id="90adc-254">In order to address this issue, find the .NET Core binary in the system:</span></span>
```bash
sudo find / -name dotnet
```

<span data-ttu-id="90adc-255">Ez az elérési útját adja a dotnet bináris.</span><span class="sxs-lookup"><span data-stu-id="90adc-255">This returns the path to the dotnet binary.</span></span> 

    /opt/rh/rh-dotnetcore11/root/usr/bin/dotnet
    /opt/rh/rh-dotnetcore11/root/usr/lib64/dotnetcore/dotnet
    /opt/rh/rh-dotnetcore11/root/usr/lib64/dotnetcore/shared/Microsoft.NETCore.App/1.1.2/dotnet

<span data-ttu-id="90adc-256">Most az elérési út hozzáadása a PATH változóban.</span><span class="sxs-lookup"><span data-stu-id="90adc-256">Now add this path to the PATH variable.</span></span> <span data-ttu-id="90adc-257">A sudo secure_path tartalmazó elérési útját a bináris dotnet szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="90adc-257">For sudo, edit secure_path to contain the path to the dotnet binary:</span></span>
```bash 
sudo visudo
### Append the path found in the preceding example to 'secure_path' variable
```

<span data-ttu-id="90adc-258">Ebben a példában secure_path változó olvassa be, mint:</span><span class="sxs-lookup"><span data-stu-id="90adc-258">In this example, secure_path variable reads as:</span></span>

```
secure_path = /sbin:/bin:/usr/sbin:/usr/bin:/opt/rh/rh-dotnetcore11/root/usr/bin/
```

<span data-ttu-id="90adc-259">Az aktuális felhasználó szerkesztése.bash_profile/.profile elérési útját a PATH változóban bináris dotnet tartalmazza</span><span class="sxs-lookup"><span data-stu-id="90adc-259">For the current user, edit .bash_profile/.profile to include the path to the dotnet binary in PATH variable</span></span> 
```bash
vi ~/.bash_profile
### Append the path found in the preceding example to 'PATH' variable
```

<span data-ttu-id="90adc-260">Győződjön meg arról, hogy a .NET Core most az elérési út:</span><span class="sxs-lookup"><span data-stu-id="90adc-260">Verify that .NET Core is now in PATH:</span></span>
```bash
which dotnet
sudo which dotnet
```

### <a name="error-installing-azcopy"></a><span data-ttu-id="90adc-261">Hiba történt az AzCopy telepítése</span><span class="sxs-lookup"><span data-stu-id="90adc-261">Error Installing AzCopy</span></span>
<span data-ttu-id="90adc-262">Ha hibát tapasztal az AzCopy telepítési, megpróbálhatja újból futtatni a bash parancsfájlok használatát a kibontott AzCopy `azcopy` mappa.</span><span class="sxs-lookup"><span data-stu-id="90adc-262">If you encounter issues with AzCopy installation, you may try to run AzCopy using the bash script in the extracted `azcopy` folder.</span></span>

```bash
cd azcopy
./azcopy
```

### <a name="limit-concurrent-writes-while-copying-data"></a><span data-ttu-id="90adc-263">Adatok másolása egyidejű írási műveletek korlátozása</span><span class="sxs-lookup"><span data-stu-id="90adc-263">Limit concurrent writes while copying data</span></span>
<span data-ttu-id="90adc-264">AzCopy rendelkező fájlokat vagy a BLOB másolása esetén vegye figyelembe, hogy egy másik alkalmazás módosítja az adatok másolása, amíg.</span><span class="sxs-lookup"><span data-stu-id="90adc-264">When you copy blobs or files with AzCopy, keep in mind that another application may be modifying the data while you are copying it.</span></span> <span data-ttu-id="90adc-265">Ha lehetséges győződjön meg arról, hogy a másolt adatok nem áll módosítás alatt a másolási művelet során.</span><span class="sxs-lookup"><span data-stu-id="90adc-265">If possible, ensure that the data you are copying is not being modified during the copy operation.</span></span> <span data-ttu-id="90adc-266">Például, ha egy Azure virtuális géphez társított virtuális merevlemez, győződjön meg arról, hogy más alkalmazás nem jelenleg írás a virtuális merevlemezhez.</span><span class="sxs-lookup"><span data-stu-id="90adc-266">For example, when copying a VHD associated with an Azure virtual machine, make sure that no other applications are currently writing to the VHD.</span></span> <span data-ttu-id="90adc-267">Egy jó úgy ehhez, hogy az erőforrás másolandó lízing.</span><span class="sxs-lookup"><span data-stu-id="90adc-267">A good way to do this is by leasing the resource to be copied.</span></span> <span data-ttu-id="90adc-268">Alternatív megoldásként először hozza létre a virtuális merevlemez pillanatképet, és másolja a pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="90adc-268">Alternately, you can create a snapshot of the VHD first and then copy the snapshot.</span></span>

<span data-ttu-id="90adc-269">A blobok vagy a fájlok írása közben másolja őket, hogy más alkalmazások nem megakadályozása, majd vegye figyelembe, hogy az idő, a feladat befejeződik, a másolt erőforrások már nincs a forrás-erőforrások teljes paritás.</span><span class="sxs-lookup"><span data-stu-id="90adc-269">If you cannot prevent other applications from writing to blobs or files while they are being copied, then keep in mind that by the time the job finishes, the copied resources may no longer have full parity with the source resources.</span></span>

### <a name="run-one-azcopy-instance-on-one-machine"></a><span data-ttu-id="90adc-270">Futtassa az AzCopy egy példány egy számítógépen.</span><span class="sxs-lookup"><span data-stu-id="90adc-270">Run one AzCopy instance on one machine.</span></span>
<span data-ttu-id="90adc-271">AzCopy célja, hogy felgyorsítsa az adatok átvitele a gép erőforrás-felhasználás, azt javasoljuk, hogy csak egy AzCopy példány egy számítógépen futtatja, és adja meg a beállítást `--parallel-level` Ha több egyidejű műveletek van szüksége.</span><span class="sxs-lookup"><span data-stu-id="90adc-271">AzCopy is designed to maximize the utilization of your machine resource to accelerate the data transfer, we recommend you run only one AzCopy instance on one machine, and specify the option `--parallel-level` if you need more concurrent operations.</span></span> <span data-ttu-id="90adc-272">További tudnivalókért írja be a `AzCopy --help parallel-level` a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="90adc-272">For more details, type `AzCopy --help parallel-level` at the command line.</span></span>

## <a name="next-steps"></a><span data-ttu-id="90adc-273">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="90adc-273">Next steps</span></span>
<span data-ttu-id="90adc-274">Azure Storage és AzCopy kapcsolatos további információkért lásd a következőket:</span><span class="sxs-lookup"><span data-stu-id="90adc-274">For more information about Azure Storage and AzCopy, see the following resources:</span></span>

### <a name="azure-storage-documentation"></a><span data-ttu-id="90adc-275">Az Azure Storage-dokumentáció:</span><span class="sxs-lookup"><span data-stu-id="90adc-275">Azure Storage documentation:</span></span>
* [<span data-ttu-id="90adc-276">Az Azure Storage bemutatása</span><span class="sxs-lookup"><span data-stu-id="90adc-276">Introduction to Azure Storage</span></span>](storage-introduction.md)
* [<span data-ttu-id="90adc-277">Tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="90adc-277">Create a storage account</span></span>](storage-create-storage-account.md)
* [<span data-ttu-id="90adc-278">A Tártallózó alkalmazással blobok kezelése</span><span class="sxs-lookup"><span data-stu-id="90adc-278">Manage blobs with Storage Explorer</span></span>](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-explorer-blobs)
* [<span data-ttu-id="90adc-279">Az Azure parancssori felület 2.0 használatával az Azure Storage</span><span class="sxs-lookup"><span data-stu-id="90adc-279">Using the Azure CLI 2.0 with Azure Storage</span></span>](storage-azure-cli.md)
* [<span data-ttu-id="90adc-280">Blob storage-ának C++ használata</span><span class="sxs-lookup"><span data-stu-id="90adc-280">How to use Blob storage from C++</span></span>](storage-c-plus-plus-how-to-use-blobs.md)
* [<span data-ttu-id="90adc-281">How to use Blob storage from Java (A Blob Storage használata Javával)</span><span class="sxs-lookup"><span data-stu-id="90adc-281">How to use Blob storage from Java</span></span>](storage-java-how-to-use-blob-storage.md)
* [<span data-ttu-id="90adc-282">How to use Blob storage from Node.js (A Blob Storage használata Node.js-sel)</span><span class="sxs-lookup"><span data-stu-id="90adc-282">How to use Blob storage from Node.js</span></span>](storage-nodejs-how-to-use-blob-storage.md)
* [<span data-ttu-id="90adc-283">How to use Blob storage from Pythonnal (A Blob Storage használata Pythonnal)</span><span class="sxs-lookup"><span data-stu-id="90adc-283">How to use Blob storage from Python</span></span>](storage-python-how-to-use-blob-storage.md)

### <a name="azure-storage-blog-posts"></a><span data-ttu-id="90adc-284">Az Azure Storage blogbejegyzések:</span><span class="sxs-lookup"><span data-stu-id="90adc-284">Azure Storage blog posts:</span></span>
* [<span data-ttu-id="90adc-285">AzCopy lévő Linux előzetes bejelentése</span><span class="sxs-lookup"><span data-stu-id="90adc-285">Announcing AzCopy on Linux Preview</span></span>](https://azure.microsoft.com/en-in/blog/announcing-azcopy-on-linux-preview/)
* [<span data-ttu-id="90adc-286">Introducing Azure Storage adatátviteli könyvtár megtekintés</span><span class="sxs-lookup"><span data-stu-id="90adc-286">Introducing Azure Storage Data Movement Library Preview</span></span>](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [<span data-ttu-id="90adc-287">AzCopy: Bevezetéséről szinkron másolatot és testreszabott tartalom típusa</span><span class="sxs-lookup"><span data-stu-id="90adc-287">AzCopy: Introducing synchronous copy and customized content type</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [<span data-ttu-id="90adc-288">AzCopy: Általános rendelkezésre állási az AzCopy 3.0 és a tábla és a fájl-támogatással rendelkező az AzCopy 4.0 előzetes bejelentése</span><span class="sxs-lookup"><span data-stu-id="90adc-288">AzCopy: Announcing General Availability of AzCopy 3.0 plus preview release of AzCopy 4.0 with Table and File support</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [<span data-ttu-id="90adc-289">AzCopy: A felügyeleti teendők központjaként másolással optimalizált</span><span class="sxs-lookup"><span data-stu-id="90adc-289">AzCopy: Optimized for Large-Scale Copy Scenarios</span></span>](http://go.microsoft.com/fwlink/?LinkId=507682)
* [<span data-ttu-id="90adc-290">AzCopy: Írásvédett georedundáns tárolás támogatása</span><span class="sxs-lookup"><span data-stu-id="90adc-290">AzCopy: Support for read-access geo-redundant storage</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [<span data-ttu-id="90adc-291">AzCopy: Adatátvitelt újraindítható móddal és SAS-jogkivonat</span><span class="sxs-lookup"><span data-stu-id="90adc-291">AzCopy: Transfer data with restartable mode and SAS token</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [<span data-ttu-id="90adc-292">AzCopy: Kereszt-fiók másolási Blob használatával</span><span class="sxs-lookup"><span data-stu-id="90adc-292">AzCopy: Using cross-account Copy Blob</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [<span data-ttu-id="90adc-293">AzCopy: Feltöltése/fájlok letöltése Azure Blobokra vonatkozó</span><span class="sxs-lookup"><span data-stu-id="90adc-293">AzCopy: Uploading/downloading files for Azure Blobs</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

