---
title: "aaaMicrosoft Azure Tártallózó (előzetes verzió) kibocsátási megjegyzései |} Microsoft Docs"
description: "Kibocsátási megjegyzések a Microsoft Azure Tártallózó (előzetes verzió)"
services: storage
documentationcenter: na
author: cawa
manager: paulyuk
editor: 
ms.assetid: 
ms.service: storage
ms.devlang: multiple
ms.topic: release-notes
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2017
ms.author: cawa
ms.openlocfilehash: 44ff6dc8e2015f4eb71fa13098b832fbbf48ccac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-azure-storage-explorer-preview-release-notes"></a><span data-ttu-id="a889b-103">Kibocsátási megjegyzések a Microsoft Azure Tártallózó (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="a889b-103">Microsoft Azure Storage Explorer (Preview) release notes</span></span>

<span data-ttu-id="a889b-104">A cikkben hello kibocsátási megjegyzések a 0.8.16. Azure Tártallózó (előzetes verzió) kiadása, valamint a kibocsátási megjegyzések a korábbi verziók.</span><span class="sxs-lookup"><span data-stu-id="a889b-104">This article contains hello release notes for Azure Storage Explorer 0.8.16 (Preview) release, as well as release notes for previous versions.</span></span>

<span data-ttu-id="a889b-105">[A Microsoft Azure Tártallózó (előzetes verzió)](./vs-azure-tools-storage-manage-with-storage-explorer.md) egy különálló alkalmazás, amely lehetővé teszi az Azure Storage-adatokkal Windows, a macOS és a Linux tooeasily használata.</span><span class="sxs-lookup"><span data-stu-id="a889b-105">[Microsoft Azure Storage Explorer (Preview)](./vs-azure-tools-storage-manage-with-storage-explorer.md) is a standalone app that enables you tooeasily work with Azure Storage data on Windows, macOS, and Linux.</span></span>

## <a name="version-0816-preview"></a><span data-ttu-id="a889b-106">Verzió 0.8.16 (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="a889b-106">Version 0.8.16 (Preview)</span></span>
<span data-ttu-id="a889b-107">8/21/2017</span><span class="sxs-lookup"><span data-stu-id="a889b-107">8/21/2017</span></span>

### <a name="download-azure-storage-explorer-0816-preview"></a><span data-ttu-id="a889b-108">Töltse le az Azure Tártallózó (előzetes verzió) 0.8.16</span><span class="sxs-lookup"><span data-stu-id="a889b-108">Download Azure Storage Explorer 0.8.16 (Preview)</span></span>
- [<span data-ttu-id="a889b-109">A Windows Azure Tártallózó (előzetes verzió) 0.8.16</span><span class="sxs-lookup"><span data-stu-id="a889b-109">Azure Storage Explorer 0.8.16 (Preview) for Windows</span></span>](https://go.microsoft.com/fwlink/?LinkId=708343)
- [<span data-ttu-id="a889b-110">A Mac Azure Tártallózó (előzetes verzió) 0.8.16</span><span class="sxs-lookup"><span data-stu-id="a889b-110">Azure Storage Explorer 0.8.16 (Preview) for Mac</span></span>](https://go.microsoft.com/fwlink/?LinkId=708342)
- [<span data-ttu-id="a889b-111">Linux rendszerhez készült Azure Tártallózó (előzetes verzió) 0.8.16</span><span class="sxs-lookup"><span data-stu-id="a889b-111">Azure Storage Explorer 0.8.16 (Preview) for Linux</span></span>](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a><span data-ttu-id="a889b-112">új</span><span class="sxs-lookup"><span data-stu-id="a889b-112">New</span></span>
* <span data-ttu-id="a889b-113">Amikor megnyit egy blobot, Tártallózó kérni fogja tooupload hello letöltött fájl változás észlelésekor</span><span class="sxs-lookup"><span data-stu-id="a889b-113">When you open a blob, Storage Explorer will prompt you tooupload hello downloaded file if a change is detected</span></span>
* <span data-ttu-id="a889b-114">Azure verem bejelentkezési élmény</span><span class="sxs-lookup"><span data-stu-id="a889b-114">Enhanced Azure Stack sign-in experience</span></span>
* <span data-ttu-id="a889b-115">Fájlok továbbfejlesztett hello teljesítmény sok kis feltöltése/Letöltés: hello azonos idő</span><span class="sxs-lookup"><span data-stu-id="a889b-115">Improved hello performance of uploading/downloading many small files at hello same time</span></span>


### <a name="fixes"></a><span data-ttu-id="a889b-116">Javítások</span><span class="sxs-lookup"><span data-stu-id="a889b-116">Fixes</span></span>
* <span data-ttu-id="a889b-117">Néhány blob típusok kiválasztása túl "cseréje" során egy feltöltési ütközést néha eredményezne hello feltöltés újraindítja.</span><span class="sxs-lookup"><span data-stu-id="a889b-117">For some blob types, choosing too"replace" during an upload conflict would sometimes result in hello upload being restarted.</span></span> 
* <span data-ttu-id="a889b-118">Verzióban 0.8.15 a feltöltések volna néha megrekedésének 99 %.</span><span class="sxs-lookup"><span data-stu-id="a889b-118">In version 0.8.15, uploads would sometimes stall at 99%.</span></span>
* <span data-ttu-id="a889b-119">Feltöltésekor a rendszer fájlok tooa fájlmegosztáshoz, ha úgy dönt, amely még nem létezett tooupload tooa directory, a feltöltést sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="a889b-119">When uploading files tooa file share, if you chose tooupload tooa directory which did not yet exist, your upload would fail.</span></span>
* <span data-ttu-id="a889b-120">Tártallózó helytelenül lett időbélyegeket közös hozzáférésű jogosultságkód valamint tábla létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="a889b-120">Storage Explorer was incorrectly generating time stamps for shared access signatures and table queries.</span></span>


<span data-ttu-id="a889b-121">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="a889b-121">Known Issues</span></span>
* <span data-ttu-id="a889b-122">Név és kulcs kapcsolati karakterlánc nem működik.</span><span class="sxs-lookup"><span data-stu-id="a889b-122">Using a name and key connection string does not currently work.</span></span> <span data-ttu-id="a889b-123">A következő verzióra hello rögzítettek.</span><span class="sxs-lookup"><span data-stu-id="a889b-123">It will be fixed in hello next release.</span></span> <span data-ttu-id="a889b-124">Addig is használhat nevével és a kulcs csatolása.</span><span class="sxs-lookup"><span data-stu-id="a889b-124">Until then you can use attach with name and key.</span></span>
* <span data-ttu-id="a889b-125">Ha tooopen egy Windows-fájl érvénytelen nevű fájl, hello letöltési egy fájl nem található az hibát eredményez.</span><span class="sxs-lookup"><span data-stu-id="a889b-125">If you try tooopen a file with an invalid Windows file name, hello download will result in a file not found error.</span></span>
* <span data-ttu-id="a889b-126">Után a "Mégse gombra" kattintva meg olyan feladatra, amíg az adott feladat toocancel is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="a889b-126">After clicking "Cancel" on a task, it may take a while for that task toocancel.</span></span> <span data-ttu-id="a889b-127">Ez a hello Azure Storage csomópont könyvtár korlátozása.</span><span class="sxs-lookup"><span data-stu-id="a889b-127">This is a limitation of hello Azure Storage Node library.</span></span>
* <span data-ttu-id="a889b-128">Egy blob feltöltése befejezése után hello fülre, amely hello feltöltés kezdeményezett frissül.</span><span class="sxs-lookup"><span data-stu-id="a889b-128">After completing a blob upload, hello tab which initiated hello upload is refreshed.</span></span> <span data-ttu-id="a889b-129">Módosult az előző viselkedését, és is miatt a toobe visszakerül a hello tároló toohello gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="a889b-129">This is a change from previous behavior, and will also cause you toobe taken back toohello root of hello container you are in.</span></span>
* <span data-ttu-id="a889b-130">Ha úgy dönt, hogy helytelen PIN-kód/intelligens kártya tanúsítványának hello, akkor szüksége lesz a rendelés toohave Tártallózó toorestart feledje, hogy döntést.</span><span class="sxs-lookup"><span data-stu-id="a889b-130">If you choose hello wrong PIN/Smartcard certificate, then you will need toorestart in order toohave Storage Explorer forget that decision.</span></span>
* <span data-ttu-id="a889b-131">hello fiók beállítások panel jelenhet meg, hogy kell-e tooreenter hitelesítő adatok toofilter előfizetések.</span><span class="sxs-lookup"><span data-stu-id="a889b-131">hello account settings panel may show that you need tooreenter credentials toofilter subscriptions.</span></span>
* <span data-ttu-id="a889b-132">Blobok (külön-külön vagy átnevezett blob tárolóhoz belül) átnevezése nem őrzi meg a pillanatképeket.</span><span class="sxs-lookup"><span data-stu-id="a889b-132">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="a889b-133">Minden más tulajdonságok és metaadatok BLOB-, fájl-és entitások egy átnevezési megőriz.</span><span class="sxs-lookup"><span data-stu-id="a889b-133">All other properties and metadata for blobs, files and entities are preserved during a rename.</span></span>
* <span data-ttu-id="a889b-134">Bár az Azure-verem jelenleg nem támogatja a fájlmegosztásokat, fájlmegosztások csomópont továbbra is egy csatolt verem Azure storage-fiók alatt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a889b-134">Although Azure Stack doesn't currently support Files Shares, a File Shares node still appears under an attached Azure Stack storage account.</span></span>
* <span data-ttu-id="a889b-135">A felhasználók számára az Ubuntu 14.04, szüksége lesz a ÖET működik-e toodate tooensure – hello futtatásával ehhez a következő parancsokat, és indítsa újra a gépet:</span><span class="sxs-lookup"><span data-stu-id="a889b-135">For users on Ubuntu 14.04, you will need tooensure GCC is up toodate - this can be done by running hello following commands, and then restarting your machine:</span></span>

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* <span data-ttu-id="a889b-136">Ubuntu 17.04 felhasználójához szüksége lesz a tooinstall GConf - ez végezhető el a következő parancsokat, és indítsa újra a gépet hello fut:</span><span class="sxs-lookup"><span data-stu-id="a889b-136">For users on Ubuntu 17.04, you will need tooinstall GConf - this can be done by running hello following commands, and then restarting your machine:</span></span>

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-0814-preview"></a><span data-ttu-id="a889b-137">Verzió 0.8.14 (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="a889b-137">Version 0.8.14 (Preview)</span></span>
<span data-ttu-id="a889b-138">06/22/2017</span><span class="sxs-lookup"><span data-stu-id="a889b-138">06/22/2017</span></span>

### <a name="download-azure-storage-explorer-0814-preview"></a><span data-ttu-id="a889b-139">Töltse le az Azure Tártallózó (előzetes verzió) 0.8.14</span><span class="sxs-lookup"><span data-stu-id="a889b-139">Download Azure Storage Explorer 0.8.14 (Preview)</span></span>
* [<span data-ttu-id="a889b-140">Töltse le a Windows Azure Tártallózó (előzetes verzió) 0.8.14</span><span class="sxs-lookup"><span data-stu-id="a889b-140">Download Azure Storage Explorer 0.8.14 (Preview) for Windows</span></span>](https://go.microsoft.com/fwlink/?LinkId=809306)
* [<span data-ttu-id="a889b-141">Töltse le a Mac Azure Tártallózó (előzetes verzió) 0.8.14</span><span class="sxs-lookup"><span data-stu-id="a889b-141">Download Azure Storage Explorer 0.8.14 (Preview) for Mac</span></span>](https://go.microsoft.com/fwlink/?LinkId=809307)
* [<span data-ttu-id="a889b-142">Linux rendszerhez készült Azure Tártallózó (előzetes verzió) 0.8.14 letöltése</span><span class="sxs-lookup"><span data-stu-id="a889b-142">Download Azure Storage Explorer 0.8.14 (Preview) for Linux</span></span>](https://go.microsoft.com/fwlink/?LinkId=809308)

### <a name="new"></a><span data-ttu-id="a889b-143">új</span><span class="sxs-lookup"><span data-stu-id="a889b-143">New</span></span>

* <span data-ttu-id="a889b-144">Számos fontos biztonsági frissítést rendelés tootake előnyeit too1.7.2, frissített elektronsugár verziója</span><span class="sxs-lookup"><span data-stu-id="a889b-144">Updated Electron version too1.7.2 in order tootake advantage of several critical security updates</span></span>
* <span data-ttu-id="a889b-145">Most már elérheti, hello online hibaelhárítási útmutató hello Súgó menü</span><span class="sxs-lookup"><span data-stu-id="a889b-145">You can now quickly access hello online troubleshooting guide from hello help menu</span></span>
* <span data-ttu-id="a889b-146">A Tártallózó hibaelhárítási [útmutató][2]</span><span class="sxs-lookup"><span data-stu-id="a889b-146">Storage Explorer Troubleshooting [Guide][2]</span></span>
* <span data-ttu-id="a889b-147">[Útmutatás] [ 3] a tooan Azure verem előfizetés csatlakozó</span><span class="sxs-lookup"><span data-stu-id="a889b-147">[Instructions][3] on connecting tooan Azure Stack subscription</span></span>

### <a name="known-issues"></a><span data-ttu-id="a889b-148">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="a889b-148">Known Issues</span></span>

* <span data-ttu-id="a889b-149">Hello delete mappa megerősítő párbeszédpanelen levő hello kattintások Linux nincs regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="a889b-149">Buttons on hello delete folder confirmation dialog don't register with hello mouse clicks on Linux.</span></span> <span data-ttu-id="a889b-150">Kerülő megoldás lehet toouse hello Enter billentyűt</span><span class="sxs-lookup"><span data-stu-id="a889b-150">Workaround is toouse hello Enter key</span></span>
* <span data-ttu-id="a889b-151">Ha úgy dönt, hogy helytelen PIN-kód/intelligens kártya tanúsítványának hello, akkor szüksége lesz a rendelés toohave Tártallózó toorestart elfelejti hello döntési</span><span class="sxs-lookup"><span data-stu-id="a889b-151">If you choose hello wrong PIN/Smartcard certificate then you will need toorestart in order toohave Storage Explorer forget hello decision</span></span>
* <span data-ttu-id="a889b-152">Mivel a fájlok feltöltése: hello azonos és blobokat több mint 3 csoport idő hibákat okozhat</span><span class="sxs-lookup"><span data-stu-id="a889b-152">Having more than 3 groups of blobs or files uploading at hello same time may cause errors</span></span>
* <span data-ttu-id="a889b-153">hello fiók beállítások panel jelenhet meg, hogy a rendelés toofilter előfizetések tooreenter-felhasználó hitelesítő adatait kell</span><span class="sxs-lookup"><span data-stu-id="a889b-153">hello account settings panel may show that you need tooreenter credentials in order toofilter subscriptions</span></span>
* <span data-ttu-id="a889b-154">Blobok (külön-külön vagy átnevezett blob tárolóhoz belül) átnevezése nem őrzi meg a pillanatképeket.</span><span class="sxs-lookup"><span data-stu-id="a889b-154">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="a889b-155">Minden más tulajdonságok és metaadatok BLOB-, fájl-és entitások egy átnevezési megőriz.</span><span class="sxs-lookup"><span data-stu-id="a889b-155">All other properties and metadata for blobs, files and entities are preserved during a rename.</span></span>
* <span data-ttu-id="a889b-156">Bár az Azure-verem jelenleg nem támogatja a fájlmegosztások, fájlmegosztások csomópont továbbra is egy csatolt verem Azure storage-fiók alatt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a889b-156">Although Azure Stack doesn't currently support File Shares, a File Shares node still appears under an attached Azure Stack storage account.</span></span> 
* <span data-ttu-id="a889b-157">Ubuntu 14.04 telepítés igényeinek ÖET verzió frissítése, vagy frissített – lépéseket tooupgrade alatt van:</span><span class="sxs-lookup"><span data-stu-id="a889b-157">Ubuntu 14.04 install needs gcc version updated or upgraded – steps tooupgrade are below:</span></span>

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```




## <a name="previous-releases"></a><span data-ttu-id="a889b-158">Korábbi kiadások</span><span class="sxs-lookup"><span data-stu-id="a889b-158">Previous releases</span></span>

* [<span data-ttu-id="a889b-159">0.8.13 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-159">Version 0.8.13</span></span>](#version-0813)
* [<span data-ttu-id="a889b-160">Verzió 0.8.12 / 0.8.11 / 0.8.10</span><span class="sxs-lookup"><span data-stu-id="a889b-160">Version 0.8.12 / 0.8.11 / 0.8.10</span></span>](#version-0812--0811--0810)
* [<span data-ttu-id="a889b-161">Verzió 0.8.9 / 0.8.8</span><span class="sxs-lookup"><span data-stu-id="a889b-161">Version 0.8.9 / 0.8.8</span></span>](#version-089--088)
* [<span data-ttu-id="a889b-162">0.8.7 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-162">Version 0.8.7</span></span>](#version-087)
* [<span data-ttu-id="a889b-163">0.8.6 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-163">Version 0.8.6</span></span>](#version-086)
* [<span data-ttu-id="a889b-164">0.8.5 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-164">Version 0.8.5</span></span>](#version-085)
* [<span data-ttu-id="a889b-165">0.8.4 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-165">Version 0.8.4</span></span>](#version-084)
* [<span data-ttu-id="a889b-166">0.8.3 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-166">Version 0.8.3</span></span>](#version-083)
* [<span data-ttu-id="a889b-167">0.8.2 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-167">Version 0.8.2</span></span>](#version-082)
* [<span data-ttu-id="a889b-168">0.8.0 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-168">Version 0.8.0</span></span>](#version-080)
* [<span data-ttu-id="a889b-169">0.7.20160509.0 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-169">Version 0.7.20160509.0</span></span>](#version-07201605090)
* [<span data-ttu-id="a889b-170">0.7.20160325.0 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-170">Version 0.7.20160325.0</span></span>](#version-07201603250)
* [<span data-ttu-id="a889b-171">0.7.20160129.1 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-171">Version 0.7.20160129.1</span></span>](#version-07201601291)
* [<span data-ttu-id="a889b-172">0.7.20160105.0 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-172">Version 0.7.20160105.0</span></span>](#version-07201601050)
* [<span data-ttu-id="a889b-173">0.7.20151116.0 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-173">Version 0.7.20151116.0</span></span>](#version-07201511160)


### <a name="version-0813"></a><span data-ttu-id="a889b-174">0.8.13 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-174">Version 0.8.13</span></span>
<span data-ttu-id="a889b-175">05/12/2017</span><span class="sxs-lookup"><span data-stu-id="a889b-175">05/12/2017</span></span>

#### <a name="new"></a><span data-ttu-id="a889b-176">új</span><span class="sxs-lookup"><span data-stu-id="a889b-176">New</span></span>

* <span data-ttu-id="a889b-177">A Tártallózó hibaelhárítási [útmutató][2]</span><span class="sxs-lookup"><span data-stu-id="a889b-177">Storage Explorer Troubleshooting [Guide][2]</span></span>
* <span data-ttu-id="a889b-178">[Útmutatás] [ 3] a tooan Azure verem előfizetés csatlakozó</span><span class="sxs-lookup"><span data-stu-id="a889b-178">[Instructions][3] on connecting tooan Azure Stack subscription</span></span>

#### <a name="fixes"></a><span data-ttu-id="a889b-179">Javítások</span><span class="sxs-lookup"><span data-stu-id="a889b-179">Fixes</span></span>

* <span data-ttu-id="a889b-180">Rögzített: A fájl feltöltése volna, amely egy kevés a memória, nagy valószínűséggel</span><span class="sxs-lookup"><span data-stu-id="a889b-180">Fixed: File upload had a high chance of causing an out of memory error</span></span>
* <span data-ttu-id="a889b-181">Rögzített: Most már bejelentkezhet PIN-kód/intelligens kártya</span><span class="sxs-lookup"><span data-stu-id="a889b-181">Fixed: You can now sign in with PIN/Smartcard</span></span>
* <span data-ttu-id="a889b-182">Rögzített: Nyissa meg a portál most works Azure Kína, a Németországi Azure, a Azure Amerikai Egyesült államokbeli kormányzati és az Azure verem</span><span class="sxs-lookup"><span data-stu-id="a889b-182">Fixed: Open in Portal now works with Azure China, Azure Germany, Azure US Government, and Azure Stack</span></span>
* <span data-ttu-id="a889b-183">Rögzített: A mappa tooa blobtárolóban feltöltésekor "Szabálytalan műveletet" hiba néha lépne</span><span class="sxs-lookup"><span data-stu-id="a889b-183">Fixed: While uploading a folder tooa blob container, an "Illegal operation" error would sometimes occur</span></span>
* <span data-ttu-id="a889b-184">Rögzített: Az összes le lett tiltva, a pillanatképek felügyelete során</span><span class="sxs-lookup"><span data-stu-id="a889b-184">Fixed: Select all was disabled while managing snapshots</span></span>
* <span data-ttu-id="a889b-185">Rögzített: hello alap blob metaadatait hello előfordulhat, hogy első felül után a pillanatképek hello tulajdonságainak megtekintése</span><span class="sxs-lookup"><span data-stu-id="a889b-185">Fixed: hello metadata of hello base blob might get overwritten after viewing hello properties of its snapshots</span></span>

#### <a name="known-issues"></a><span data-ttu-id="a889b-186">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="a889b-186">Known Issues</span></span>

* <span data-ttu-id="a889b-187">Ha úgy dönt, hogy helytelen PIN-kód/intelligens kártya tanúsítványának hello, akkor szüksége lesz a rendelés toohave Tártallózó toorestart elfelejti hello döntési</span><span class="sxs-lookup"><span data-stu-id="a889b-187">If you choose hello wrong PIN/Smartcard certificate then you will need toorestart in order toohave Storage Explorer forget hello decision</span></span>
* <span data-ttu-id="a889b-188">Bejövő vagy kimenő nagyított, hello nagyítási szintjét, rövid ideig alaphelyzetbe toohello alapértelmezett szint</span><span class="sxs-lookup"><span data-stu-id="a889b-188">While zoomed in or out, hello zoom level may momentarily reset toohello default level</span></span>
* <span data-ttu-id="a889b-189">Mivel a fájlok feltöltése: hello azonos és blobokat több mint 3 csoport idő hibákat okozhat</span><span class="sxs-lookup"><span data-stu-id="a889b-189">Having more than 3 groups of blobs or files uploading at hello same time may cause errors</span></span>
* <span data-ttu-id="a889b-190">hello fiók beállítások panel jelenhet meg, hogy a rendelés toofilter előfizetések tooreenter-felhasználó hitelesítő adatait kell</span><span class="sxs-lookup"><span data-stu-id="a889b-190">hello account settings panel may show that you need tooreenter credentials in order toofilter subscriptions</span></span>
* <span data-ttu-id="a889b-191">Blobok (külön-külön vagy átnevezett blob tárolóhoz belül) átnevezése nem őrzi meg a pillanatképeket.</span><span class="sxs-lookup"><span data-stu-id="a889b-191">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="a889b-192">Minden más tulajdonságok és metaadatok BLOB-, fájl-és entitások egy átnevezési megőriz.</span><span class="sxs-lookup"><span data-stu-id="a889b-192">All other properties and metadata for blobs, files and entities are preserved during a rename.</span></span>
* <span data-ttu-id="a889b-193">Bár az Azure-verem jelenleg nem támogatja a fájlmegosztásokat, fájlmegosztások csomópont továbbra is egy csatolt verem Azure storage-fiók alatt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a889b-193">Although Azure Stack doesn't currently support Files Shares, a File Shares node still appears under an attached Azure Stack storage account.</span></span> 
* <span data-ttu-id="a889b-194">Ubuntu 14.04 telepítés igényeinek ÖET verzió frissítése, vagy frissített – lépéseket tooupgrade alatt van:</span><span class="sxs-lookup"><span data-stu-id="a889b-194">Ubuntu 14.04 install needs gcc version updated or upgraded – steps tooupgrade are below:</span></span>

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-0812--0811--0810"></a><span data-ttu-id="a889b-195">Verzió 0.8.12 / 0.8.11 / 0.8.10</span><span class="sxs-lookup"><span data-stu-id="a889b-195">Version 0.8.12 / 0.8.11 / 0.8.10</span></span>
<span data-ttu-id="a889b-196">04/07/2017</span><span class="sxs-lookup"><span data-stu-id="a889b-196">04/07/2017</span></span>

#### <a name="new"></a><span data-ttu-id="a889b-197">új</span><span class="sxs-lookup"><span data-stu-id="a889b-197">New</span></span>

* <span data-ttu-id="a889b-198">A Tártallózó automatikusan leáll egy frissítés a hello frissítési értesítés</span><span class="sxs-lookup"><span data-stu-id="a889b-198">Storage Explorer will now automatically close when you install an update from hello update notification</span></span>
* <span data-ttu-id="a889b-199">Helyben gyors hozzáférést biztosít a fokozott élményt nyújtó a gyakran használt erőforrásokat használata</span><span class="sxs-lookup"><span data-stu-id="a889b-199">In-place quick access provides an enhanced experience for working with your frequently accessed resources</span></span>
* <span data-ttu-id="a889b-200">Hello Blob tároló szerkesztő, most már megtekintheti a bérelt blob tartozik virtual machine</span><span class="sxs-lookup"><span data-stu-id="a889b-200">In hello Blob Container editor, you can now see which virtual machine a leased blob belongs to</span></span>
* <span data-ttu-id="a889b-201">Most csukja össze hello bal oldali panelen</span><span class="sxs-lookup"><span data-stu-id="a889b-201">You can now collapse hello left side panel</span></span>
* <span data-ttu-id="a889b-202">Felderítési most már fut a hello azonos idő letölthető</span><span class="sxs-lookup"><span data-stu-id="a889b-202">Discovery now runs at hello same time as download</span></span>
* <span data-ttu-id="a889b-203">Felhasználási statisztikáinak a hello Blob tároló, a fájlmegosztás és a tábla szerkesztők toosee hello az erőforrás vagy méretének kiválasztása</span><span class="sxs-lookup"><span data-stu-id="a889b-203">Use Statistics in hello Blob Container, File Share, and Table editors toosee hello size of your resource or selection</span></span>
* <span data-ttu-id="a889b-204">Most már bejelentkezhet tooAzure Active Directory (AAD) alapú Azure verem fiókok is.</span><span class="sxs-lookup"><span data-stu-id="a889b-204">You can now sign-in tooAzure Active Directory (AAD) based Azure Stack accounts.</span></span> 
* <span data-ttu-id="a889b-205">Feltöltés archív fájlok tooPremium storage-fiókok több mint 32 MB lehet</span><span class="sxs-lookup"><span data-stu-id="a889b-205">You can now upload archive files over 32MB tooPremium storage accounts</span></span>
* <span data-ttu-id="a889b-206">Továbbfejlesztett kisegítő támogatása</span><span class="sxs-lookup"><span data-stu-id="a889b-206">Improved accessibility support</span></span>
* <span data-ttu-id="a889b-207">Mostantól hozzáadhatja azok megbízható Base-64 kódolású X.509 SSL-tanúsítványok által is tooEdit -&gt; SSL-tanúsítványok -&gt; tanúsítványok importálása</span><span class="sxs-lookup"><span data-stu-id="a889b-207">You can now add trusted Base-64 encoded X.509 SSL certificates by going tooEdit -&gt; SSL Certificates -&gt; Import Certificates</span></span>

#### <a name="fixes"></a><span data-ttu-id="a889b-208">Javítások</span><span class="sxs-lookup"><span data-stu-id="a889b-208">Fixes</span></span>

* <span data-ttu-id="a889b-209">Rögzített: után frissíteni egy fiók hitelesítő adatait, hello fanézetben volna néha nem automatikus frissítése</span><span class="sxs-lookup"><span data-stu-id="a889b-209">Fixed: after refreshing an account's credentials, hello tree view would sometimes not automatically refresh</span></span>
* <span data-ttu-id="a889b-210">Rögzített: emulátor üzenetsorok és táblák SAS-kód generálása eredményeznének egy URL-cím érvénytelen</span><span class="sxs-lookup"><span data-stu-id="a889b-210">Fixed: generating a SAS for emulator queues and tables would result in an invalid URL</span></span>
* <span data-ttu-id="a889b-211">Rögzített: prémium szintű storage-fiókok most bővíthetők a proxy be van kapcsolva</span><span class="sxs-lookup"><span data-stu-id="a889b-211">Fixed: premium storage accounts can now be expanded while a proxy is enabled</span></span>
* <span data-ttu-id="a889b-212">Rögzített: hello alkalmazni gomb hello fiókok kezelése lap csatlakoztatás nem működik, ha kijelölt 1 vagy 0-fiókok</span><span class="sxs-lookup"><span data-stu-id="a889b-212">Fixed: hello apply button on hello accounts management page would not work if you had 1 or 0 accounts selected</span></span>
* <span data-ttu-id="a889b-213">Rögzített: ütközés megoldások igénylő blobok feltöltése meghiúsulhat - 0.8.11 rögzített</span><span class="sxs-lookup"><span data-stu-id="a889b-213">Fixed: uploading blobs that require conflict resolutions may fail - fixed in 0.8.11</span></span> 
* <span data-ttu-id="a889b-214">Rögzített: visszajelzés küldése lett feltörése 0.8.11 - 0.8.12 rögzített</span><span class="sxs-lookup"><span data-stu-id="a889b-214">Fixed: sending feedback was broken in 0.8.11 - fixed in 0.8.12</span></span> 

#### <a name="known-issues"></a><span data-ttu-id="a889b-215">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="a889b-215">Known Issues</span></span>

* <span data-ttu-id="a889b-216">A frissítés too0.8.10 után szüksége lesz a toorefresh összes hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="a889b-216">After upgrading too0.8.10, you will need toorefresh all of your credentials.</span></span>
* <span data-ttu-id="a889b-217">Bejövő vagy kimenő nagyított, hello nagyítási szint rövid ideig lehetséges, hogy alaphelyzetbe toohello alapértelmezett szintjét.</span><span class="sxs-lookup"><span data-stu-id="a889b-217">While zoomed in or out, hello zoom level may momentarily reset toohello default level.</span></span>
* <span data-ttu-id="a889b-218">Mivel a fájlok feltöltése: hello azonos és blobokat több mint 3 csoport idő hibákat okozhat.</span><span class="sxs-lookup"><span data-stu-id="a889b-218">Having more than 3 groups of blobs or files uploading at hello same time may cause errors.</span></span>
* <span data-ttu-id="a889b-219">hello fiók beállítások panel jelenhet meg, hogy a rendelés toofilter előfizetések tooreenter-felhasználó hitelesítő adatait kell.</span><span class="sxs-lookup"><span data-stu-id="a889b-219">hello account settings panel may show that you need tooreenter credentials in order toofilter subscriptions.</span></span>
* <span data-ttu-id="a889b-220">Blobok (külön-külön vagy átnevezett blob tárolóhoz belül) átnevezése nem őrzi meg a pillanatképeket.</span><span class="sxs-lookup"><span data-stu-id="a889b-220">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="a889b-221">Minden más tulajdonságok és metaadatok BLOB-, fájl-és entitások egy átnevezési megőriz.</span><span class="sxs-lookup"><span data-stu-id="a889b-221">All other properties and metadata for blobs, files and entities are preserved during a rename.</span></span>
* <span data-ttu-id="a889b-222">Bár az Azure-verem jelenleg nem támogatja a fájlmegosztásokat, fájlmegosztások csomópont továbbra is egy csatolt verem Azure storage-fiók alatt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a889b-222">Although Azure Stack doesn't currently support Files Shares, a File Shares node still appears under an attached Azure Stack storage account.</span></span> 
* <span data-ttu-id="a889b-223">Ubuntu 14.04 telepítés igényeinek ÖET verzió frissítése, vagy frissített – lépéseket tooupgrade alatt van:</span><span class="sxs-lookup"><span data-stu-id="a889b-223">Ubuntu 14.04 install needs gcc version updated or upgraded – steps tooupgrade are below:</span></span>

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-089--088"></a><span data-ttu-id="a889b-224">Verzió 0.8.9 / 0.8.8</span><span class="sxs-lookup"><span data-stu-id="a889b-224">Version 0.8.9 / 0.8.8</span></span>
<span data-ttu-id="a889b-225">02/23/2017</span><span class="sxs-lookup"><span data-stu-id="a889b-225">02/23/2017</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/R6gonK3cYAc?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/SrRPCm94mfE?ecver=1" frameborder="0" allowfullscreen></iframe>


#### <a name="new"></a><span data-ttu-id="a889b-226">új</span><span class="sxs-lookup"><span data-stu-id="a889b-226">New</span></span>

* <span data-ttu-id="a889b-227">A Tártallózó 0.8.9 automatikusan letölti hello legújabb verzióját a frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="a889b-227">Storage Explorer 0.8.9 will automatically download hello latest version for updates.</span></span>
* <span data-ttu-id="a889b-228">Gyorsjavítás: a portál használatával létrehozott SAS URI tooattach hiba történt a tárfiók eredményezne.</span><span class="sxs-lookup"><span data-stu-id="a889b-228">Hotfix: using a portal generated SAS URI tooattach a storage account would result in an error.</span></span>
* <span data-ttu-id="a889b-229">Most létrehozására, kezelésére és blob pillanatképek lépteti elő.</span><span class="sxs-lookup"><span data-stu-id="a889b-229">You can now create, manage, and promote blob snapshots.</span></span>
* <span data-ttu-id="a889b-230">Most már bejelentkezhet tooAzure Kína, a Németországi Azure és az Azure Amerikai Egyesült államokbeli kormányzati fiókok.</span><span class="sxs-lookup"><span data-stu-id="a889b-230">You can now sign in tooAzure China, Azure Germany, and Azure US Government accounts.</span></span>
* <span data-ttu-id="a889b-231">Mostantól megváltoztatható hello nagyítási szintjét.</span><span class="sxs-lookup"><span data-stu-id="a889b-231">You can now change hello zoom level.</span></span> <span data-ttu-id="a889b-232">Hello Nézet menü tooZoom kicsinyítés és Nagyítás alaphelyzetbe állítja a hello beállításokat használják.</span><span class="sxs-lookup"><span data-stu-id="a889b-232">Use hello options in hello View menu tooZoom In, Zoom Out, and Reset Zoom.</span></span>
* <span data-ttu-id="a889b-233">Unicode-karaktereket mostantól támogatja a felhasználói blobok és a fájlok metaadatait.</span><span class="sxs-lookup"><span data-stu-id="a889b-233">Unicode characters are now supported in user metadata for blobs and files.</span></span>
* <span data-ttu-id="a889b-234">Kisegítő lehetőségek fejlesztései.</span><span class="sxs-lookup"><span data-stu-id="a889b-234">Accessibility improvements.</span></span>
* <span data-ttu-id="a889b-235">hello frissítési értesítés hello következő verziójára kibocsátási megjegyzéseket is megtekinthetők.</span><span class="sxs-lookup"><span data-stu-id="a889b-235">hello next version's release notes can be viewed from hello update notification.</span></span> <span data-ttu-id="a889b-236">Hello aktuális kibocsátási megjegyzések hello Súgó menüjében is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="a889b-236">You can also view hello current release notes from hello Help menu.</span></span>

#### <a name="fixes"></a><span data-ttu-id="a889b-237">Javítások</span><span class="sxs-lookup"><span data-stu-id="a889b-237">Fixes</span></span>

* <span data-ttu-id="a889b-238">Rögzített: hello verziószámának megfelelően megjelenik a Windows Vezérlőpult</span><span class="sxs-lookup"><span data-stu-id="a889b-238">Fixed: hello version number is now correctly displayed in Control Panel on Windows</span></span>
* <span data-ttu-id="a889b-239">Javítani: keresési már nem korlátozott too50, 000 csomópontok</span><span class="sxs-lookup"><span data-stu-id="a889b-239">Fixed: search is no longer limited too50,000 nodes</span></span>
* <span data-ttu-id="a889b-240">Rögzített: feltöltés tooa fájlmegosztást hoz végtelen Ha hello célkönyvtáron már nem létezik</span><span class="sxs-lookup"><span data-stu-id="a889b-240">Fixed: upload tooa file share spun forever if hello destination directory did not already exist</span></span>
* <span data-ttu-id="a889b-241">Rögzített: jobb stabilitás hosszú feltöltések és letöltések</span><span class="sxs-lookup"><span data-stu-id="a889b-241">Fixed: improved stability for long uploads and downloads</span></span>

#### <a name="known-issues"></a><span data-ttu-id="a889b-242">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="a889b-242">Known Issues</span></span>

* <span data-ttu-id="a889b-243">Bejövő vagy kimenő nagyított, hello nagyítási szint rövid ideig lehetséges, hogy alaphelyzetbe toohello alapértelmezett szintjét.</span><span class="sxs-lookup"><span data-stu-id="a889b-243">While zoomed in or out, hello zoom level may momentarily reset toohello default level.</span></span>
* <span data-ttu-id="a889b-244">A gyors hozzáférés csak olyan alapú előfizetés elemek működik.</span><span class="sxs-lookup"><span data-stu-id="a889b-244">Quick Access only works with subscription based items.</span></span> <span data-ttu-id="a889b-245">Ebben a kiadásban nem támogatottak a helyi erőforrások vagy a kulcs- vagy SAS-jogkivonat keresztül csatlakozó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="a889b-245">Local resources or resources attached via key or SAS token are not supported in this release.</span></span>
* <span data-ttu-id="a889b-246">Gyors elérés érdekében néhány másodperc toonavigate toohello cél erőforráson, attól függően, hogy hány erőforrásokat vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="a889b-246">It may take Quick Access a few seconds toonavigate toohello target resource, depending on how many resources you have.</span></span>
* <span data-ttu-id="a889b-247">Mivel a fájlok feltöltése: hello azonos és blobokat több mint 3 csoport idő hibákat okozhat.</span><span class="sxs-lookup"><span data-stu-id="a889b-247">Having more than 3 groups of blobs or files uploading at hello same time may cause errors.</span></span>

<span data-ttu-id="a889b-248">12/16/2016</span><span class="sxs-lookup"><span data-stu-id="a889b-248">12/16/2016</span></span>
### <a name="version-087"></a><span data-ttu-id="a889b-249">0.8.7 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-249">Version 0.8.7</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/Me4Y4jxoer8?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="a889b-250">új</span><span class="sxs-lookup"><span data-stu-id="a889b-250">New</span></span>

* <span data-ttu-id="a889b-251">Kiválaszthatja, hogyan tooresolve ütközések hello elején egy adott frissítés letöltése vagy hello tevékenységek ablakban másolja a munkamenet</span><span class="sxs-lookup"><span data-stu-id="a889b-251">You can choose how tooresolve conflicts at hello beginning of an update, download or copy session in hello Activities window</span></span>
* <span data-ttu-id="a889b-252">Rámutat egy lapon toosee hello teljes elérési útja hello tárolási erőforrás</span><span class="sxs-lookup"><span data-stu-id="a889b-252">Hover over a tab toosee hello full path of hello storage resource</span></span>
* <span data-ttu-id="a889b-253">Ha egy lap gombra kattint, hello bal oldali navigációs ablak helyére szinkronizálják</span><span class="sxs-lookup"><span data-stu-id="a889b-253">When you click on a tab, it synchronizes with its location in hello left side navigation pane</span></span>

#### <a name="fixes"></a><span data-ttu-id="a889b-254">Javítások</span><span class="sxs-lookup"><span data-stu-id="a889b-254">Fixes</span></span>

* <span data-ttu-id="a889b-255">Rögzített: Tártallózó mostantól a megbízható alkalmazások Mac gépen</span><span class="sxs-lookup"><span data-stu-id="a889b-255">Fixed: Storage Explorer is now a trusted app on Mac</span></span>
* <span data-ttu-id="a889b-256">Rögzített: Ubuntu 14.04 támogatja</span><span class="sxs-lookup"><span data-stu-id="a889b-256">Fixed: Ubuntu 14.04 is again supported</span></span>
* <span data-ttu-id="a889b-257">Rögzített: Néha hello fiók felhasználói felület tokenkódot előfizetések betöltésekor</span><span class="sxs-lookup"><span data-stu-id="a889b-257">Fixed: Sometimes hello add account UI flashes when loading subscriptions</span></span>
* <span data-ttu-id="a889b-258">Rögzített: Néha nem minden tárolási erőforrások szereplő hello bal oldali navigációs panelen</span><span class="sxs-lookup"><span data-stu-id="a889b-258">Fixed: Sometimes not all storage resources were listed in hello left side navigation pane</span></span>
* <span data-ttu-id="a889b-259">Rögzített: hello műveletpanelen néha megjelenített üres műveletek</span><span class="sxs-lookup"><span data-stu-id="a889b-259">Fixed: hello action pane sometimes displayed empty actions</span></span>
* <span data-ttu-id="a889b-260">Rögzített: hello ablakméret utolsó lezárt hello munkamenetből most őrződnek meg</span><span class="sxs-lookup"><span data-stu-id="a889b-260">Fixed: hello window size from hello last closed session is now retained</span></span>
* <span data-ttu-id="a889b-261">Rögzített: Megnyithatja a hello több lapot azonos erőforrást hello helyi menü</span><span class="sxs-lookup"><span data-stu-id="a889b-261">Fixed: You can open multiple tabs for hello same resource using hello context menu</span></span>

#### <a name="known-issues"></a><span data-ttu-id="a889b-262">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="a889b-262">Known Issues</span></span>

* <span data-ttu-id="a889b-263">A gyors hozzáférés csak olyan alapú előfizetés elemek működik.</span><span class="sxs-lookup"><span data-stu-id="a889b-263">Quick Access only works with subscription based items.</span></span> <span data-ttu-id="a889b-264">Helyi erőforrások vagy a kulcs- vagy SAS-jogkivonat keresztül csatlakozó erőforrásokat nem támogatott ebben a kiadásban</span><span class="sxs-lookup"><span data-stu-id="a889b-264">Local resources or resources attached via key or SAS token are not supported in this release</span></span>
* <span data-ttu-id="a889b-265">Gyors elérés érdekében néhány másodperc toonavigate toohello cél erőforráson, attól függően, hogy hány erőforrásokat is eltarthat</span><span class="sxs-lookup"><span data-stu-id="a889b-265">It may take Quick Access a few seconds toonavigate toohello target resource, depending on how many resources you have</span></span>
* <span data-ttu-id="a889b-266">Mivel a fájlok feltöltése: hello azonos és blobokat több mint 3 csoport idő hibákat okozhat</span><span class="sxs-lookup"><span data-stu-id="a889b-266">Having more than 3 groups of blobs or files uploading at hello same time may cause errors</span></span>
* <span data-ttu-id="a889b-267">Keresési kezeli, ezt követően a körülbelül 50 000 csomópontokon - keresés teljesítmény negatív hatással lehet, vagy nem kezelt kivételt okozhat</span><span class="sxs-lookup"><span data-stu-id="a889b-267">Search handles searching across roughly 50,000 nodes - after this, performance may be impacted or may cause unhandled exception</span></span>
* <span data-ttu-id="a889b-268">Hello az első alkalommal használja a Tártallózó hello macOS, a jelenhet meg több kér, felhasználó engedély tooaccess kulcslánc megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="a889b-268">For hello first time using hello Storage Explorer on macOS, you might see multiple prompts asking for user's permission tooaccess keychain.</span></span> <span data-ttu-id="a889b-269">Javasoljuk, hogy mindig választja, az így hello kérdés nem jelenik meg újra</span><span class="sxs-lookup"><span data-stu-id="a889b-269">We suggest you select Always Allow so hello prompt won't show up again</span></span>

<span data-ttu-id="a889b-270">11/18/2016</span><span class="sxs-lookup"><span data-stu-id="a889b-270">11/18/2016</span></span>
### <a name="version-086"></a><span data-ttu-id="a889b-271">0.8.6 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-271">Version 0.8.6</span></span>

#### <a name="new"></a><span data-ttu-id="a889b-272">új</span><span class="sxs-lookup"><span data-stu-id="a889b-272">New</span></span>

* <span data-ttu-id="a889b-273">PIN-kód a leggyakrabban használt szolgáltatások toohello gyorselérési egyszerű kezelhetőség lehet</span><span class="sxs-lookup"><span data-stu-id="a889b-273">You can now pin most frequently used services toohello Quick Access for easy navigation</span></span>
* <span data-ttu-id="a889b-274">Több szerkesztők egyes lapjainak most már úgy is megnyithatja.</span><span class="sxs-lookup"><span data-stu-id="a889b-274">You can now open multiple editors in different tabs.</span></span> <span data-ttu-id="a889b-275">Egy kattintással tooopen ideiglenes fülre. Kattintson duplán a tooopen állandó fülre. Is rákattinthat a hello ideiglenes lapon toomake azt egy állandó lap</span><span class="sxs-lookup"><span data-stu-id="a889b-275">Single click tooopen a temporary tab; double click tooopen a permanent tab. You can also click on hello temporary tab toomake it a permanent tab</span></span>
* <span data-ttu-id="a889b-276">Hajtottunk észlelhető teljesítménybeli és stabilitását érintő fejlesztések feltölti és tölti le, különösen olyan nagyméretű fájlok gyors gépeken</span><span class="sxs-lookup"><span data-stu-id="a889b-276">We have made noticeable performance and stability improvements for uploads and downloads, especially for large files on fast machines</span></span>
* <span data-ttu-id="a889b-277">Blob tárolók most hozhatók létre üres "virtuális" mappák</span><span class="sxs-lookup"><span data-stu-id="a889b-277">Empty "virtual" folders can now be created in blob containers</span></span>
* <span data-ttu-id="a889b-278">Azt újra vezetett be az új kibővített substring keresés hatókörös keresésre, most két választási lehetősége van a keresés:</span><span class="sxs-lookup"><span data-stu-id="a889b-278">We have re-introduced scoped search with our new enhanced substring search, so you now have two options for searching:</span></span> 
    * <span data-ttu-id="a889b-279">Globális keresés - csak meg kell adnia a kívánt keresőkifejezést hello keresési szövegmező</span><span class="sxs-lookup"><span data-stu-id="a889b-279">Global search - just enter a search term into hello search textbox</span></span>
    * <span data-ttu-id="a889b-280">Hatókörös keresésre - hello Nagyítót ábrázoló ikonra következő tooa csomópontot, kattintson, majd vegye fel a keresési kifejezés toohello vége hello elérési utat, vagy kattintson a jobb gombbal, "keresési az itt"</span><span class="sxs-lookup"><span data-stu-id="a889b-280">Scoped search - click hello magnifying glass icon next tooa node, then add a search term toohello end of hello path, or right-click and select "Search from Here"</span></span>
* <span data-ttu-id="a889b-281">Jelentek meg különböző témák: ritka (alapértelmezett), sötét, kontrasztos fekete és kontrasztos fehér.</span><span class="sxs-lookup"><span data-stu-id="a889b-281">We have added various themes: Light (default), Dark, High Contrast Black, and High Contrast White.</span></span> <span data-ttu-id="a889b-282">Nyissa meg tooEdit -&gt; témák toochange témák használatát igény szerint</span><span class="sxs-lookup"><span data-stu-id="a889b-282">Go tooEdit -&gt; Themes toochange your theming preference</span></span>
* <span data-ttu-id="a889b-283">Módosíthatja a Blob és a fájl tulajdonságait</span><span class="sxs-lookup"><span data-stu-id="a889b-283">You can modify Blob and file properties</span></span>
* <span data-ttu-id="a889b-284">Most már támogatott kódolt (base64) és kódolatlan üzenetek</span><span class="sxs-lookup"><span data-stu-id="a889b-284">We now support encoded (base64) and unencoded queue messages</span></span>
* <span data-ttu-id="a889b-285">Linux, a 64 bites operációs rendszer mostantól szükséges.</span><span class="sxs-lookup"><span data-stu-id="a889b-285">On Linux, a 64-bit OS is now required.</span></span> <span data-ttu-id="a889b-286">Ebben a kiadásban csak támogatott 64 bites Ubuntu 16.04.1 LTS</span><span class="sxs-lookup"><span data-stu-id="a889b-286">For this release we only support 64-bit Ubuntu 16.04.1 LTS</span></span>
* <span data-ttu-id="a889b-287">Frissítettük az embléma!</span><span class="sxs-lookup"><span data-stu-id="a889b-287">We have updated our logo!</span></span>

#### <a name="fixes"></a><span data-ttu-id="a889b-288">Javítások</span><span class="sxs-lookup"><span data-stu-id="a889b-288">Fixes</span></span>

* <span data-ttu-id="a889b-289">Rögzített: Képernyőn befagyasztási problémák</span><span class="sxs-lookup"><span data-stu-id="a889b-289">Fixed: Screen freezing problems</span></span>
* <span data-ttu-id="a889b-290">Rögzített: Fokozott biztonság</span><span class="sxs-lookup"><span data-stu-id="a889b-290">Fixed: Enhanced security</span></span>
* <span data-ttu-id="a889b-291">Rögzített: Néha duplikált csatolt fiókot fog megjelenni</span><span class="sxs-lookup"><span data-stu-id="a889b-291">Fixed: Sometimes duplicate attached accounts could appear</span></span>
* <span data-ttu-id="a889b-292">Rögzített: Nincs megadva content típusú blob tudta előállítani kivétel</span><span class="sxs-lookup"><span data-stu-id="a889b-292">Fixed: A blob with an undefined content type could generate an exception</span></span>
* <span data-ttu-id="a889b-293">Rögzített: Nyitó hello lekérdezés Panel a program üres táblát nem sikerült</span><span class="sxs-lookup"><span data-stu-id="a889b-293">Fixed: Opening hello Query Panel on an empty table was not possible</span></span>
* <span data-ttu-id="a889b-294">Rögzített: Változó a keresési hibák</span><span class="sxs-lookup"><span data-stu-id="a889b-294">Fixed: Varies bugs in Search</span></span>
* <span data-ttu-id="a889b-295">Rögzített: Hello olyan erőforrások száma, ha "Több betöltése" gombra kattintva 50 too100 betöltődnek nőtt</span><span class="sxs-lookup"><span data-stu-id="a889b-295">Fixed: Increased hello number of resources loaded from 50 too100 when clicking "Load More"</span></span>
* <span data-ttu-id="a889b-296">Rögzített: Első alkalommal történő futtatásakor, a fiók alá van írva, ha azt immár előfizetéseket fiók alapértelmezés szerint</span><span class="sxs-lookup"><span data-stu-id="a889b-296">Fixed: On first run, if an account is signed into, we now select all subscriptions for that account by default</span></span> 

#### <a name="known-issues"></a><span data-ttu-id="a889b-297">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="a889b-297">Known Issues</span></span>

* <span data-ttu-id="a889b-298">Hello Tártallózó jelen kiadása nem futtatható Ubuntu 14.04</span><span class="sxs-lookup"><span data-stu-id="a889b-298">This release of hello Storage Explorer does not run on Ubuntu 14.04</span></span>
* <span data-ttu-id="a889b-299">tooopen több lapot a ugyanazt az erőforrást, így folyamatosan nem kattintson a hello hello ugyanazt az erőforrást.</span><span class="sxs-lookup"><span data-stu-id="a889b-299">tooopen multiple tabs for hello same resource, do not continuously click on hello same resource.</span></span> <span data-ttu-id="a889b-300">Kattintson egy másik erőforrás és majd lépjen vissza, majd a hello eredeti erőforrás tooopen azt újra egy másik lapján</span><span class="sxs-lookup"><span data-stu-id="a889b-300">Click on another resource and then go back and then click on hello original resource tooopen it again in another tab</span></span> 
* <span data-ttu-id="a889b-301">A gyors hozzáférés csak olyan alapú előfizetés elemek működik.</span><span class="sxs-lookup"><span data-stu-id="a889b-301">Quick Access only works with subscription based items.</span></span> <span data-ttu-id="a889b-302">Helyi erőforrások vagy a kulcs- vagy SAS-jogkivonat keresztül csatlakozó erőforrásokat nem támogatott ebben a kiadásban</span><span class="sxs-lookup"><span data-stu-id="a889b-302">Local resources or resources attached via key or SAS token are not supported in this release</span></span>
* <span data-ttu-id="a889b-303">Gyors elérés érdekében néhány másodperc toonavigate toohello cél erőforráson, attól függően, hogy hány erőforrásokat is eltarthat</span><span class="sxs-lookup"><span data-stu-id="a889b-303">It may take Quick Access a few seconds toonavigate toohello target resource, depending on how many resources you have</span></span>
* <span data-ttu-id="a889b-304">Mivel a fájlok feltöltése: hello azonos és blobokat több mint 3 csoport idő hibákat okozhat</span><span class="sxs-lookup"><span data-stu-id="a889b-304">Having more than 3 groups of blobs or files uploading at hello same time may cause errors</span></span>
* <span data-ttu-id="a889b-305">Keresési kezeli, ezt követően a körülbelül 50 000 csomópontokon - keresés teljesítmény negatív hatással lehet, vagy nem kezelt kivételt okozhat</span><span class="sxs-lookup"><span data-stu-id="a889b-305">Search handles searching across roughly 50,000 nodes - after this, performance may be impacted or may cause unhandled exception</span></span>

<span data-ttu-id="a889b-306">10/03/2016</span><span class="sxs-lookup"><span data-stu-id="a889b-306">10/03/2016</span></span>
### <a name="version-085"></a><span data-ttu-id="a889b-307">0.8.5 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-307">Version 0.8.5</span></span>

#### <a name="new"></a><span data-ttu-id="a889b-308">új</span><span class="sxs-lookup"><span data-stu-id="a889b-308">New</span></span>

* <span data-ttu-id="a889b-309">Ekkor a portál által létrehozott SAS használja a kulcsok tooattach tooStorage fiókjainak és erőforrásainak</span><span class="sxs-lookup"><span data-stu-id="a889b-309">Can now use Portal-generated SAS keys tooattach tooStorage Accounts and resources</span></span>

#### <a name="fixes"></a><span data-ttu-id="a889b-310">Javítások</span><span class="sxs-lookup"><span data-stu-id="a889b-310">Fixes</span></span>

* <span data-ttu-id="a889b-311">Rögzített: egyes esetekben a keresés közben versenyhelyzet okozott csomópontok toobecome nem bővíthető</span><span class="sxs-lookup"><span data-stu-id="a889b-311">Fixed: race condition during search sometimes caused nodes toobecome non-expandable</span></span>
* <span data-ttu-id="a889b-312">Rögzített: "HTTP használata" nem működik, amikor tooStorage fiókok összekapcsolása fióknevet és kulcsot</span><span class="sxs-lookup"><span data-stu-id="a889b-312">Fixed: "Use HTTP" doesn't work when connecting tooStorage Accounts with account name and key</span></span>
* <span data-ttu-id="a889b-313">Rögzített: SAS-kulcsok (kifejezetten portál által létrehozott állók közül.) a hibaüzenetet adja vissza "záró perjelet"</span><span class="sxs-lookup"><span data-stu-id="a889b-313">Fixed: SAS keys (specially Portal-generated ones) return a "trailing slash" error</span></span>
* <span data-ttu-id="a889b-314">Rögzített: tábla importálása problémák</span><span class="sxs-lookup"><span data-stu-id="a889b-314">Fixed: table import issues</span></span>
    * <span data-ttu-id="a889b-315">Egyes esetekben partíciókulcs- és sorkulcsa lettek visszaállítva</span><span class="sxs-lookup"><span data-stu-id="a889b-315">Sometimes partition key and row key were reversed</span></span>
    * <span data-ttu-id="a889b-316">Nem lehet "null" Partíciókulcsok tooread</span><span class="sxs-lookup"><span data-stu-id="a889b-316">Unable tooread "null" Partition Keys</span></span>

#### <a name="known-issues"></a><span data-ttu-id="a889b-317">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="a889b-317">Known Issues</span></span>

* <span data-ttu-id="a889b-318">Keresési kezeli a keresés - körülbelül 50 000-csomópont között, előfordulhat, hogy lehet a teljesítményre</span><span class="sxs-lookup"><span data-stu-id="a889b-318">Search handles searching across roughly 50,000 nodes - after this, performance may be impacted</span></span>
* <span data-ttu-id="a889b-319">Azure verem jelenleg nem támogatott fájlok, így tooexpand fájlok próbált megjelenítése hiba</span><span class="sxs-lookup"><span data-stu-id="a889b-319">Azure Stack doesn't currently support Files, so trying tooexpand Files will show an error</span></span>

<span data-ttu-id="a889b-320">09/12/2016</span><span class="sxs-lookup"><span data-stu-id="a889b-320">09/12/2016</span></span>
### <a name="version-084"></a><span data-ttu-id="a889b-321">0.8.4 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-321">Version 0.8.4</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/cr5tOGyGrIQ?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="a889b-322">új</span><span class="sxs-lookup"><span data-stu-id="a889b-322">New</span></span>

* <span data-ttu-id="a889b-323">Közvetlen hivatkozások toostorage fiókok, tárolók, várólisták, táblák létrehozása vagy fájlmegosztásokat és könnyen megosztható tooyour erőforrások - Windows és az elérésére támogatja a Mac OS</span><span class="sxs-lookup"><span data-stu-id="a889b-323">Generate direct links toostorage accounts, containers, queues, tables, or file shares for sharing and easy access tooyour resources - Windows and Mac OS support</span></span>
* <span data-ttu-id="a889b-324">Keresse meg a blobtárolók, táblák, üzenetsorok, fájlmegosztások vagy hello keresőmezőbe a storage-fiókok</span><span class="sxs-lookup"><span data-stu-id="a889b-324">Search for your blob containers, tables, queues, file shares, or storage accounts from hello search box</span></span>
* <span data-ttu-id="a889b-325">Most csoportosíthatja záradékok hello tábla Lekérdezésszerkesztőben</span><span class="sxs-lookup"><span data-stu-id="a889b-325">You can now group clauses in hello table query builder</span></span>
* <span data-ttu-id="a889b-326">Nevezze át, és a másolás/beillesztés blob tárolók, fájlmegosztások, táblák, blobok, blob mappákat, fájlokat és könyvtárakat az SAS-csatolású fiókok és a tárolók belül</span><span class="sxs-lookup"><span data-stu-id="a889b-326">Rename and copy/paste blob containers, file shares, tables, blobs, blob folders, files and directories from within SAS-attached accounts and containers</span></span>
* <span data-ttu-id="a889b-327">Átnevezése és lemásolná a blob-tárolók és a fájlmegosztások a tulajdonságok és metaadatok most megőrzése</span><span class="sxs-lookup"><span data-stu-id="a889b-327">Renaming and copying blob containers and file shares now preserve properties and metadata</span></span>

#### <a name="fixes"></a><span data-ttu-id="a889b-328">Javítások</span><span class="sxs-lookup"><span data-stu-id="a889b-328">Fixes</span></span>

* <span data-ttu-id="a889b-329">Rögzített: nem szerkeszthető táblaentitásokat tartalmazó logikai vagy bináris tulajdonság</span><span class="sxs-lookup"><span data-stu-id="a889b-329">Fixed: cannot edit table entities if they contain boolean or binary properties</span></span>

#### <a name="known-issues"></a><span data-ttu-id="a889b-330">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="a889b-330">Known Issues</span></span>

* <span data-ttu-id="a889b-331">Keresési kezeli a keresés - körülbelül 50 000-csomópont között, előfordulhat, hogy lehet a teljesítményre</span><span class="sxs-lookup"><span data-stu-id="a889b-331">Search handles searching across roughly 50,000 nodes - after this, performance may be impacted</span></span>

<span data-ttu-id="a889b-332">08/03/2016</span><span class="sxs-lookup"><span data-stu-id="a889b-332">08/03/2016</span></span>
### <a name="version-083"></a><span data-ttu-id="a889b-333">0.8.3 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-333">Version 0.8.3</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/HeGW-jkSd9Y?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="a889b-334">új</span><span class="sxs-lookup"><span data-stu-id="a889b-334">New</span></span>

* <span data-ttu-id="a889b-335">Nevezze át a tárolók, táblák, fájlmegosztások</span><span class="sxs-lookup"><span data-stu-id="a889b-335">Rename containers, tables, file shares</span></span>
* <span data-ttu-id="a889b-336">Lekérdezés-szerkesztő fejlett élményt</span><span class="sxs-lookup"><span data-stu-id="a889b-336">Improved Query builder experience</span></span>
* <span data-ttu-id="a889b-337">Képes toosave és a lekérdezések</span><span class="sxs-lookup"><span data-stu-id="a889b-337">Ability toosave and load queries</span></span>
* <span data-ttu-id="a889b-338">Hivatkozások toostorage fiókok vagy a tárolókat, várólisták, táblák közvetlen vagy fájlmegosztások megosztásához, és könnyen fér hozzá az erőforrások (csak Windows - macOS támogatása hamarosan elérhető!)</span><span class="sxs-lookup"><span data-stu-id="a889b-338">Direct links toostorage accounts or containers, queues, tables, or file shares for sharing and easily accessing your resources (Windows-only - macOS support coming soon!)</span></span>
* <span data-ttu-id="a889b-339">Képes toomanage, és konfigurálja a CORS-szabályokat</span><span class="sxs-lookup"><span data-stu-id="a889b-339">Ability toomanage and configure CORS rules</span></span>

#### <a name="fixes"></a><span data-ttu-id="a889b-340">Javítások</span><span class="sxs-lookup"><span data-stu-id="a889b-340">Fixes</span></span>

* <span data-ttu-id="a889b-341">Rögzített: Microsoft Accounts ismételt hitelesítés szükséges 8 – 12 óránként</span><span class="sxs-lookup"><span data-stu-id="a889b-341">Fixed: Microsoft Accounts require re-authentication every 8-12 hours</span></span>

#### <a name="known-issues"></a><span data-ttu-id="a889b-342">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="a889b-342">Known Issues</span></span>

* <span data-ttu-id="a889b-343">Egyes esetekben hello felhasználói felület előfordulhat, hogy válaszol - hello ablak maximalizálva segít a probléma megoldásához</span><span class="sxs-lookup"><span data-stu-id="a889b-343">Sometimes hello UI might appear frozen - maximizing hello window helps resolve this issue</span></span>
* <span data-ttu-id="a889b-344">macOS telepítés emelt jogosultsági szintű lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="a889b-344">macOS install may require elevated permissions</span></span>
* <span data-ttu-id="a889b-345">Fiók beállítások panel jelenhet meg, hogy a rendelés toofilter előfizetések tooreenter-felhasználó hitelesítő adatait kell</span><span class="sxs-lookup"><span data-stu-id="a889b-345">Account settings panel may show that you need tooreenter credentials in order toofilter subscriptions</span></span>
* <span data-ttu-id="a889b-346">Átnevezési fájlmegosztások, a blob-tárolók és a táblák nem őrzi meg a metaadat- vagy más tulajdonságaiból hello tárolóhoz, például a fájlmegosztási kvóta, a nyilvános hozzáférési szint vagy a hozzáférési házirendek</span><span class="sxs-lookup"><span data-stu-id="a889b-346">Renaming file shares, blob containers, and tables does not preserve metadata or other properties on hello container, such as file share quota, public access level or access policies</span></span>
* <span data-ttu-id="a889b-347">Blobok (külön-külön vagy átnevezett blob tárolóhoz belül) átnevezése nem őrzi meg a pillanatképeket.</span><span class="sxs-lookup"><span data-stu-id="a889b-347">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="a889b-348">Minden más tulajdonságok és metaadatok BLOB-, fájl-és entitások megőriz egy átnevezése</span><span class="sxs-lookup"><span data-stu-id="a889b-348">All other properties and metadata for blobs, files and entities are preserved during a rename</span></span>
* <span data-ttu-id="a889b-349">Másolással vagy erőforrások átnevezése nem működik SAS-csatolású fiókok belül</span><span class="sxs-lookup"><span data-stu-id="a889b-349">Copying or renaming resources does not work within SAS-attached accounts</span></span>

<span data-ttu-id="a889b-350">07/07/2016</span><span class="sxs-lookup"><span data-stu-id="a889b-350">07/07/2016</span></span>
### <a name="version-082"></a><span data-ttu-id="a889b-351">0.8.2 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-351">Version 0.8.2</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/nYgKbRUNYZA?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="a889b-352">új</span><span class="sxs-lookup"><span data-stu-id="a889b-352">New</span></span>

* <span data-ttu-id="a889b-353">Storage-fiókok vannak csoportosítva, előfizetések; fejlesztési tárolás és a kulcs- vagy SAS keresztül csatlakozó erőforrásokat (helyi és kapcsolódó) csomópont alatt látható</span><span class="sxs-lookup"><span data-stu-id="a889b-353">Storage Accounts are grouped by subscriptions; development storage and resources attached via key or SAS are shown under (Local and Attached) node</span></span>
* <span data-ttu-id="a889b-354">Jelentkezzen ki a "Azure-fiók beállítások" panelen fiókokhoz</span><span class="sxs-lookup"><span data-stu-id="a889b-354">Sign off from accounts in "Azure Account Settings" panel</span></span>
* <span data-ttu-id="a889b-355">Proxy beállításainak tooenable konfigurálhatja és kezelheti a bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a889b-355">Configure proxy settings tooenable and manage sign-in</span></span>
* <span data-ttu-id="a889b-356">Hozzon létre és blob címbérleteket megszüntetése</span><span class="sxs-lookup"><span data-stu-id="a889b-356">Create and break blob leases</span></span>
* <span data-ttu-id="a889b-357">Nyissa meg a blob-tárolók, várólisták, táblák és az egy kattintással fájlok</span><span class="sxs-lookup"><span data-stu-id="a889b-357">Open blob containers, queues, tables, and files with single-click</span></span>

#### <a name="fixes"></a><span data-ttu-id="a889b-358">Javítások</span><span class="sxs-lookup"><span data-stu-id="a889b-358">Fixes</span></span>

* <span data-ttu-id="a889b-359">Rögzített: a .NET vagy Java könyvtárak beszúrni üzenetek vannak nem megfelelően dekódolni a Base64 kódolású anyag</span><span class="sxs-lookup"><span data-stu-id="a889b-359">Fixed: queue messages inserted with .NET or Java libraries are not properly decoded from base64</span></span>
* <span data-ttu-id="a889b-360">Rögzített: $metrics táblák nem láthatók a Blob Storage-fiókok</span><span class="sxs-lookup"><span data-stu-id="a889b-360">Fixed: $metrics tables are not shown for Blob Storage accounts</span></span>
* <span data-ttu-id="a889b-361">Rögzített: táblák csomópont nem működik a helyi (fejlesztés) tároláshoz</span><span class="sxs-lookup"><span data-stu-id="a889b-361">Fixed: tables node does not work for local (Development) storage</span></span>

#### <a name="known-issues"></a><span data-ttu-id="a889b-362">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="a889b-362">Known Issues</span></span>

* <span data-ttu-id="a889b-363">macOS telepítés emelt jogosultsági szintű lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="a889b-363">macOS install may require elevated permissions</span></span>

<span data-ttu-id="a889b-364">06/15/2016</span><span class="sxs-lookup"><span data-stu-id="a889b-364">06/15/2016</span></span>
### <a name="version-080"></a><span data-ttu-id="a889b-365">0.8.0 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-365">Version 0.8.0</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/ycfQhKztSIY?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/k4_kOUCZ0WA?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/3zEXJcGdl_k?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="a889b-366">új</span><span class="sxs-lookup"><span data-stu-id="a889b-366">New</span></span>

* <span data-ttu-id="a889b-367">Megosztás-támogatás: megtekintése, feltöltése, letöltés, fájlok és könyvtárak, SAS URI-azonosítók másolására (létrehozása, és csatlakozzon)</span><span class="sxs-lookup"><span data-stu-id="a889b-367">File share support: viewing, uploading, downloading, copying files and directories, SAS URIs (create and connect)</span></span>
* <span data-ttu-id="a889b-368">Növelt felhasználói élmény a SAS URI-azonosító vagy kulcsait tooStorage összekapcsolása</span><span class="sxs-lookup"><span data-stu-id="a889b-368">Improved user experience for connecting tooStorage with SAS URIs or account keys</span></span>
* <span data-ttu-id="a889b-369">Tábla lekérdezési eredmények exportálása</span><span class="sxs-lookup"><span data-stu-id="a889b-369">Export table query results</span></span>
* <span data-ttu-id="a889b-370">Tábla oszlop újra sorrendbe rendezés és a Testreszabás</span><span class="sxs-lookup"><span data-stu-id="a889b-370">Table column reordering and customization</span></span>
* <span data-ttu-id="a889b-371">Blob tárolók $logs és $metrics táblák megtekintését Storage-fiókok engedélyezett metrikák</span><span class="sxs-lookup"><span data-stu-id="a889b-371">Viewing $logs blob containers and $metrics tables for Storage Accounts with enabled metrics</span></span>
* <span data-ttu-id="a889b-372">Továbbfejlesztett exportálási és viselkedésének importálni, mostantól tartalmazza a tulajdonságérték-típus</span><span class="sxs-lookup"><span data-stu-id="a889b-372">Improved export and import behavior, now includes property value type</span></span>

#### <a name="fixes"></a><span data-ttu-id="a889b-373">Javítások</span><span class="sxs-lookup"><span data-stu-id="a889b-373">Fixes</span></span>

* <span data-ttu-id="a889b-374">Rögzített: vagy nagy blobok feltöltése eredményezhet teljes feltöltések/letöltések</span><span class="sxs-lookup"><span data-stu-id="a889b-374">Fixed: uploading or downloading large blobs can result in incomplete uploads/downloads</span></span>
* <span data-ttu-id="a889b-375">Rögzített: Szerkesztés, hozzáadását és importálása egy numerikus karakterlánc-érték ("1") rendelkező entitás konvertálja toodouble</span><span class="sxs-lookup"><span data-stu-id="a889b-375">Fixed: editing, adding, or importing an entity with a numeric string value ("1") will convert it toodouble</span></span>
* <span data-ttu-id="a889b-376">Rögzített: Nem tooexpand hello csomópontjának hello helyi fejlesztési környezet</span><span class="sxs-lookup"><span data-stu-id="a889b-376">Fixed: Unable tooexpand hello table node in hello local development environment</span></span>

#### <a name="known-issues"></a><span data-ttu-id="a889b-377">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="a889b-377">Known Issues</span></span>

* <span data-ttu-id="a889b-378">$metrics táblák nem láthatók a Blob Storage-fiókok</span><span class="sxs-lookup"><span data-stu-id="a889b-378">$metrics tables are not visible for Blob Storage accounts</span></span>
* <span data-ttu-id="a889b-379">Programozott módon hozzáadott üzenetek jelenhetnek meg megfelelően Ha köszönőüzenetei Base64 kódolás használatával kódolt</span><span class="sxs-lookup"><span data-stu-id="a889b-379">Queue messages added programmatically may not be displayed correctly if hello messages are encoded using Base64 encoding</span></span>

<span data-ttu-id="a889b-380">05/17/2016</span><span class="sxs-lookup"><span data-stu-id="a889b-380">05/17/2016</span></span>
### <a name="version-07201605090"></a><span data-ttu-id="a889b-381">0.7.20160509.0 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-381">Version 0.7.20160509.0</span></span>

#### <a name="new"></a><span data-ttu-id="a889b-382">új</span><span class="sxs-lookup"><span data-stu-id="a889b-382">New</span></span>

* <span data-ttu-id="a889b-383">Jobb hiba történt az alkalmazás kezelése összeomlik</span><span class="sxs-lookup"><span data-stu-id="a889b-383">Better error handling for app crashes</span></span>

#### <a name="fixes"></a><span data-ttu-id="a889b-384">Javítások</span><span class="sxs-lookup"><span data-stu-id="a889b-384">Fixes</span></span>

* <span data-ttu-id="a889b-385">Ahol információs sáv üzenetek néha nem jelennek meg, ha a bejelentkezési hitelesítő adatok volt szükség rögzített hiba</span><span class="sxs-lookup"><span data-stu-id="a889b-385">Fixed bug where InfoBar messages sometimes don't show up when sign-in credentials were required</span></span>

#### <a name="known-issues"></a><span data-ttu-id="a889b-386">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="a889b-386">Known Issues</span></span>

* <span data-ttu-id="a889b-387">Táblázatok: Hozzáadása, szerkesztése, vagy olyan entitás, amely egy kétértelműen numerikus érték, például az "1" vagy "1.0" tulajdonsággal rendelkezik importálása és felhasználói záma toosend hello azt egy `Edm.String`, hello érték vissza határozza meg, egy Edm.Double API hello ügyfélen keresztül</span><span class="sxs-lookup"><span data-stu-id="a889b-387">Tables: Adding, editing, or importing an entity that has a property with an ambiguously numeric value, such as "1" or "1.0", and hello user tries toosend it as an `Edm.String`, hello value will come back through hello client API as an Edm.Double</span></span>

<span data-ttu-id="a889b-388">03/31/2016</span><span class="sxs-lookup"><span data-stu-id="a889b-388">03/31/2016</span></span>

### <a name="version-07201603250"></a><span data-ttu-id="a889b-389">0.7.20160325.0 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-389">Version 0.7.20160325.0</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/imbgBRHX65A?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/ceX-P8XZ-s8?ecver=1" frameborder="0" allowfullscreen></iframe>


#### <a name="new"></a><span data-ttu-id="a889b-390">új</span><span class="sxs-lookup"><span data-stu-id="a889b-390">New</span></span>

* <span data-ttu-id="a889b-391">Táblázat a támogatási szolgálathoz: megtekintésre lekérdezése, importálása és exportálása CRUD műveletek az entitások</span><span class="sxs-lookup"><span data-stu-id="a889b-391">Table support: viewing, querying, export, import, and CRUD operations for entities</span></span>
* <span data-ttu-id="a889b-392">Feldolgozási sor támogatási: megtekintését, hozzáadását, dequeueing üzenetek</span><span class="sxs-lookup"><span data-stu-id="a889b-392">Queue support: viewing, adding, dequeueing messages</span></span>
* <span data-ttu-id="a889b-393">SAS URI-azonosítók Storage-fiókok létrehozása</span><span class="sxs-lookup"><span data-stu-id="a889b-393">Generating SAS URIs for Storage Accounts</span></span>
* <span data-ttu-id="a889b-394">SAS URI-azonosítók tooStorage fiókok összekapcsolása</span><span class="sxs-lookup"><span data-stu-id="a889b-394">Connecting tooStorage Accounts with SAS URIs</span></span>
* <span data-ttu-id="a889b-395">A jövőbeli frissítések tooStorage Explorer vonatkozó frissítési értesítések</span><span class="sxs-lookup"><span data-stu-id="a889b-395">Update notifications for future updates tooStorage Explorer</span></span>
* <span data-ttu-id="a889b-396">Frissített Megjelenés és működés</span><span class="sxs-lookup"><span data-stu-id="a889b-396">Updated look and feel</span></span>

#### <a name="fixes"></a><span data-ttu-id="a889b-397">Javítások</span><span class="sxs-lookup"><span data-stu-id="a889b-397">Fixes</span></span>

* <span data-ttu-id="a889b-398">Teljesítmény és megbízhatóság fejlesztései</span><span class="sxs-lookup"><span data-stu-id="a889b-398">Performance and reliability improvements</span></span>

### <a name="known-issues-amp-mitigations"></a><span data-ttu-id="a889b-399">Ismert problémák &amp; megoldást</span><span class="sxs-lookup"><span data-stu-id="a889b-399">Known Issues &amp; Mitigations</span></span>

* <span data-ttu-id="a889b-400">A nagy blob fájlok letöltésének nem működik megfelelően – AzCopy használatát, amíg a hiba megoldása érdekében azt javasoljuk</span><span class="sxs-lookup"><span data-stu-id="a889b-400">Download of large blob files does not work correctly - we recommend using AzCopy while we address this issue</span></span> 
* <span data-ttu-id="a889b-401">Fiók hitelesítő adatai nem fogja beolvasni nem hello kezdőmappa nem található vagy nem lehet írni a gyorsítótárba</span><span class="sxs-lookup"><span data-stu-id="a889b-401">Account credentials will not be retrieved nor cached if hello home folder cannot be found or cannot be written to</span></span>
* <span data-ttu-id="a889b-402">Ha azt vannak hozzáadása, szerkesztése és importálása olyan entitás, amely egy kétértelműen numerikus érték, például az "1" vagy "1.0" tulajdonsággal rendelkezik, és hello felhasználó megpróbál toosend azt egy `Edm.String`, hello érték vissza határozza meg, egy Edm.Double API hello ügyfélen keresztül</span><span class="sxs-lookup"><span data-stu-id="a889b-402">If we are adding, editing, or importing an entity that has a property with an ambiguously numeric value, such as "1" or "1.0", and hello user tries toosend it as an `Edm.String`, hello value will come back through hello client API as an Edm.Double</span></span>
* <span data-ttu-id="a889b-403">Többsoros rekordot tartalmazó CSV-fájlok importálásakor hello adatok előfordulhat, hogy darabolja vagy titkosított</span><span class="sxs-lookup"><span data-stu-id="a889b-403">When importing CSV files with multiline records, hello data may get chopped or scrambled</span></span>

<span data-ttu-id="a889b-404">02/03/2016</span><span class="sxs-lookup"><span data-stu-id="a889b-404">02/03/2016</span></span>

### <a name="version-07201601291"></a><span data-ttu-id="a889b-405">0.7.20160129.1 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-405">Version 0.7.20160129.1</span></span>

#### <a name="fixes"></a><span data-ttu-id="a889b-406">Javítások</span><span class="sxs-lookup"><span data-stu-id="a889b-406">Fixes</span></span>

* <span data-ttu-id="a889b-407">Jobb összesített teljesítményt biztosít, feltöltését, letöltése és a BLOB másolása</span><span class="sxs-lookup"><span data-stu-id="a889b-407">Improved overall performance when uploading, downloading and copying blobs</span></span>

<span data-ttu-id="a889b-408">01/14/2016</span><span class="sxs-lookup"><span data-stu-id="a889b-408">01/14/2016</span></span>

### <a name="version-07201601050"></a><span data-ttu-id="a889b-409">0.7.20160105.0 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-409">Version 0.7.20160105.0</span></span>

#### <a name="new"></a><span data-ttu-id="a889b-410">új</span><span class="sxs-lookup"><span data-stu-id="a889b-410">New</span></span>

* <span data-ttu-id="a889b-411">Linux-támogatás (paritásos szolgáltatások tooOSX)</span><span class="sxs-lookup"><span data-stu-id="a889b-411">Linux support (parity features tooOSX)</span></span>
* <span data-ttu-id="a889b-412">Blob tárolók hozzáadása a megosztott hozzáférési aláírásokkal (SAS-) kulcs</span><span class="sxs-lookup"><span data-stu-id="a889b-412">Add blob containers with Shared Access Signatures (SAS) key</span></span>
* <span data-ttu-id="a889b-413">Kínai Azure Storage-fiókok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a889b-413">Add Storage Accounts for Azure China</span></span>
* <span data-ttu-id="a889b-414">Egyéni végpontokat a Storage-fiókok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a889b-414">Add Storage Accounts with custom endpoints</span></span>
* <span data-ttu-id="a889b-415">Megnyithatja és megtekintheti a hello tartalmát szöveges kép blobok és</span><span class="sxs-lookup"><span data-stu-id="a889b-415">Open and view hello contents text and picture blobs</span></span>
* <span data-ttu-id="a889b-416">A blob tulajdonságai és metaadatok megtekintése és szerkesztése</span><span class="sxs-lookup"><span data-stu-id="a889b-416">View and edit blob properties and metadata</span></span>

#### <a name="fixes"></a><span data-ttu-id="a889b-417">Javítások</span><span class="sxs-lookup"><span data-stu-id="a889b-417">Fixes</span></span>

* <span data-ttu-id="a889b-418">Rögzített: feltöltés és letöltés választható nagyszámú BLOB (500 +) néha okozhat hello app toohave fehér képernyő</span><span class="sxs-lookup"><span data-stu-id="a889b-418">Fixed: uploading or download a large number of blobs (500+) may sometimes cause hello app toohave a white screen</span></span> 
* <span data-ttu-id="a889b-419">Rögzített: blob tároló nyilvános hozzáférési szint, hello új érték nem frissül addig, amíg újból be hello tároló hello helyezi a hangsúlyt.</span><span class="sxs-lookup"><span data-stu-id="a889b-419">Fixed: when setting blob container public access level, hello new value is not updated until you re-set hello focus on hello container.</span></span> <span data-ttu-id="a889b-420">Emellett hello párbeszédpanel mindig az alapértelmezett túl nincs nyilvános hozzáférés-", és nem hello tényleges jelenlegi értéke.</span><span class="sxs-lookup"><span data-stu-id="a889b-420">Also, hello dialog always defaults too"No public access", and not hello actual current value.</span></span>
* <span data-ttu-id="a889b-421">Jobb általános billentyűzet/kisegítő és a felhasználói felület támogatja</span><span class="sxs-lookup"><span data-stu-id="a889b-421">Better overall keyboard/accessibility and UI support</span></span>
* <span data-ttu-id="a889b-422">Ha a szóköz karakter hosszú fussa útkövetését előzmények szöveg</span><span class="sxs-lookup"><span data-stu-id="a889b-422">Breadcrumbs history text wraps when it's long with white space</span></span>
* <span data-ttu-id="a889b-423">SAS párbeszédpanel támogatja a bemenet-ellenőrzést</span><span class="sxs-lookup"><span data-stu-id="a889b-423">SAS dialog supports input validation</span></span>
* <span data-ttu-id="a889b-424">Helyi tárolás továbbra is toobe érhető el, akkor is, ha a felhasználó hitelesítő adatainak tanúsítványa lejárt</span><span class="sxs-lookup"><span data-stu-id="a889b-424">Local storage continues toobe available even if user credentials have expired</span></span>
* <span data-ttu-id="a889b-425">Egy megnyitott blob-tároló törlésekor hello blob explorer hello jobb oldalán ki van zárva</span><span class="sxs-lookup"><span data-stu-id="a889b-425">When an opened blob container is deleted, hello blob explorer on hello right side is closed</span></span>

#### <a name="known-issues"></a><span data-ttu-id="a889b-426">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="a889b-426">Known Issues</span></span>

* <span data-ttu-id="a889b-427">Linux telepítés igényeinek ÖET verzió frissítése, vagy frissített – lépéseket tooupgrade alatt van:</span><span class="sxs-lookup"><span data-stu-id="a889b-427">Linux install needs gcc version updated or upgraded – steps tooupgrade are below:</span></span> 
    * `sudo add-apt-repository ppa:ubuntu-toolchain-r/test`
    * `sudo apt-get update`
    * `sudo apt-get upgrade`
    * `sudo apt-get dist-upgrade`

<span data-ttu-id="a889b-428">11/18/2015</span><span class="sxs-lookup"><span data-stu-id="a889b-428">11/18/2015</span></span>
### <a name="version-07201511160"></a><span data-ttu-id="a889b-429">0.7.20151116.0 verzió</span><span class="sxs-lookup"><span data-stu-id="a889b-429">Version 0.7.20151116.0</span></span>

#### <a name="new"></a><span data-ttu-id="a889b-430">új</span><span class="sxs-lookup"><span data-stu-id="a889b-430">New</span></span>

* <span data-ttu-id="a889b-431">macOS, és a Windows-verziók</span><span class="sxs-lookup"><span data-stu-id="a889b-431">macOS, and Windows versions</span></span>
* <span data-ttu-id="a889b-432">A Storage-fiókok tooview bejelentkezés, – használja a szervezeti fiók Microsoft Account, 2FA, stb.</span><span class="sxs-lookup"><span data-stu-id="a889b-432">Sign in tooview your Storage Accounts – use your Org Account, Microsoft Account, 2FA, etc.</span></span>
* <span data-ttu-id="a889b-433">Helyi fejlesztési tárolási (storage emulatorral csak Windows)</span><span class="sxs-lookup"><span data-stu-id="a889b-433">Local development storage (use storage emulator, Windows-only)</span></span>
* <span data-ttu-id="a889b-434">Az Azure Resource Manager és klasszikus erőforrás-támogatás</span><span class="sxs-lookup"><span data-stu-id="a889b-434">Azure Resource Manager and Classic resource support</span></span>
* <span data-ttu-id="a889b-435">Hozzon létre vagy töröljön a blobokat, üzenetsorokat vagy táblák</span><span class="sxs-lookup"><span data-stu-id="a889b-435">Create and delete blobs, queues, or tables</span></span>
* <span data-ttu-id="a889b-436">Keresse meg az adott blobokat, üzenetsorokat vagy táblák</span><span class="sxs-lookup"><span data-stu-id="a889b-436">Search for specific blobs, queues, or tables</span></span>
* <span data-ttu-id="a889b-437">Blob tárolók hello tartalmát felfedezés</span><span class="sxs-lookup"><span data-stu-id="a889b-437">Explore hello contents of blob containers</span></span>
* <span data-ttu-id="a889b-438">Megtekintheti és haladjon végig a könyvtárak</span><span class="sxs-lookup"><span data-stu-id="a889b-438">View and navigate through directories</span></span>
* <span data-ttu-id="a889b-439">Töltse fel, töltse le és törölje a blobok és mappák</span><span class="sxs-lookup"><span data-stu-id="a889b-439">Upload, download, and delete blobs and folders</span></span>
* <span data-ttu-id="a889b-440">A blob tulajdonságai és metaadatok megtekintése és szerkesztése</span><span class="sxs-lookup"><span data-stu-id="a889b-440">View and edit blob properties and metadata</span></span>
* <span data-ttu-id="a889b-441">SAS-kulcsok létrehozása</span><span class="sxs-lookup"><span data-stu-id="a889b-441">Generate SAS keys</span></span>
* <span data-ttu-id="a889b-442">Kezelése és tárolása hozzáférési házirendek (SAP) létrehozása</span><span class="sxs-lookup"><span data-stu-id="a889b-442">Manage and create Stored Access Policies (SAP)</span></span>
* <span data-ttu-id="a889b-443">Keresés a blobokban előtag alapján</span><span class="sxs-lookup"><span data-stu-id="a889b-443">Search for blobs by prefix</span></span>
* <span data-ttu-id="a889b-444">Húzza az n dobja el a fájlok tooupload vagy letöltése</span><span class="sxs-lookup"><span data-stu-id="a889b-444">Drag 'n drop files tooupload or download</span></span>

#### <a name="known-issues"></a><span data-ttu-id="a889b-445">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="a889b-445">Known Issues</span></span>

* <span data-ttu-id="a889b-446">Blob tároló nyilvános hozzáférési szint beállításakor a hello új érték nem frissül, amíg újra vizsgál hello hello tárolóra</span><span class="sxs-lookup"><span data-stu-id="a889b-446">When setting blob container public access level, hello new value is not updated until you re-set hello focus on hello container</span></span>
* <span data-ttu-id="a889b-447">Nyissa meg a hello párbeszédpanel tooset hello nyilvános hozzáférési szint, mindig mutat "Nem nyilvános hozzáférés" hello alapértelmezett, és nem hello tényleges aktuális érték</span><span class="sxs-lookup"><span data-stu-id="a889b-447">When you open hello dialog tooset hello public access level, it always shows "No public access" as hello default, and not hello actual current value</span></span>
* <span data-ttu-id="a889b-448">A letöltött blobokat nem nevezhető át.</span><span class="sxs-lookup"><span data-stu-id="a889b-448">Cannot rename downloaded blobs</span></span>
* <span data-ttu-id="a889b-449">Rendszer néha "elakadnak" a egy folyamatban lévő tevékenységek naplóbejegyzések állapot, ha hiba lép fel, és hello hiba nem jelenik meg</span><span class="sxs-lookup"><span data-stu-id="a889b-449">Activity log entries will sometimes get "stuck" in an in progress state when an error occurs, and hello error is not displayed</span></span>
* <span data-ttu-id="a889b-450">Néha összeomlik vagy viszont teljesen fehér tooupload közben, vagy nagyszámú blobok letöltése</span><span class="sxs-lookup"><span data-stu-id="a889b-450">Sometimes crashes or turns completely white when trying tooupload or download a large number of blobs</span></span>
* <span data-ttu-id="a889b-451">A másolási műveletek néha megszakítása nem működik</span><span class="sxs-lookup"><span data-stu-id="a889b-451">Sometimes canceling a copy operation does not work</span></span>
* <span data-ttu-id="a889b-452">Során hozza létre a tárolót (blob vagy sor vagy tábla), ha érvénytelen bemeneti és a folytatáshoz toocreate az egy másik tárolóhoz típusa egy másik, fókusza nem adható meg hello új típus</span><span class="sxs-lookup"><span data-stu-id="a889b-452">During creating a container (blob/queue/table), if you input an invalid name and proceed toocreate another under a different container type you cannot set focus on hello new type</span></span>
* <span data-ttu-id="a889b-453">Nem hozható létre új mappát, vagy nem mappa átnevezése</span><span class="sxs-lookup"><span data-stu-id="a889b-453">Can't create new folder or rename folder</span></span>




[2]: https://support.microsoft.com/en-us/help/4021389/storage-explorer-troubleshooting-guide
[3]: vs-azure-tools-storage-manage-with-storage-explorer.md