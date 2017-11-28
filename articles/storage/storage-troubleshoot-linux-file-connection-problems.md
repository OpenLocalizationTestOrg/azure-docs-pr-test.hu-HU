---
title: "Azure File storage kapcsolatos problémák elhárítása a Linux |} Microsoft Docs"
description: "Linux Azure File storage problémák hibaelhárítása"
services: storage
documentationcenter: 
author: genlin
manager: willchen
editor: na
tags: storage
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: genli
ms.openlocfilehash: 62cd62ec3a2900f06acacc0852a48b5e3ff1c8cd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshoot-azure-file-storage-problems-in-linux"></a><span data-ttu-id="aa514-103">Azure File storage kapcsolatos problémák elhárítása a Linux</span><span class="sxs-lookup"><span data-stu-id="aa514-103">Troubleshoot Azure File storage problems in Linux</span></span>

<span data-ttu-id="aa514-104">Ez a cikk a Linux-ügyfelek a csatlakozáskor a Microsoft Azure File storage kapcsolatos gyakori problémák sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="aa514-104">This article lists common problems that are related to Microsoft Azure File storage when you connect from Linux clients.</span></span> <span data-ttu-id="aa514-105">Is biztosít lehetséges okokért és megoldásokért ezeket a problémákat.</span><span class="sxs-lookup"><span data-stu-id="aa514-105">It also provides possible causes and resolutions for these problems.</span></span>

<a id="permissiondenied"></a>
## <a name="permission-denied-disk-quota-exceeded-when-you-try-to-open-a-file"></a><span data-ttu-id="aa514-106">"[engedély megtagadva] lemez kvóta túllépve" amikor próbálja megnyitni a fájlt</span><span class="sxs-lookup"><span data-stu-id="aa514-106">"[permission denied] Disk quota exceeded" when you try to open a file</span></span>

<span data-ttu-id="aa514-107">A Linux az alábbihoz hasonló hibaüzenetet kap:</span><span class="sxs-lookup"><span data-stu-id="aa514-107">In Linux, you receive an error message that resembles the following:</span></span>

<span data-ttu-id="aa514-108">**<filename>[a hozzáférés megtagadva] Lemez kvótája túllépve**</span><span class="sxs-lookup"><span data-stu-id="aa514-108">**<filename> [permission denied] Disk quota exceeded**</span></span>

### <a name="cause"></a><span data-ttu-id="aa514-109">Ok</span><span class="sxs-lookup"><span data-stu-id="aa514-109">Cause</span></span>

<span data-ttu-id="aa514-110">Elérte az egyidejű megnyitott leíróinak számára engedélyezett a fájl felső határát.</span><span class="sxs-lookup"><span data-stu-id="aa514-110">You have reached the upper limit of concurrent open handles that are allowed for a file.</span></span>

### <a name="solution"></a><span data-ttu-id="aa514-111">Megoldás</span><span class="sxs-lookup"><span data-stu-id="aa514-111">Solution</span></span>

<span data-ttu-id="aa514-112">Adjon meg kevesebb egyidejű megnyitott leíróinak néhány leírók bezárásával, és próbálkozzon újra a művelettel.</span><span class="sxs-lookup"><span data-stu-id="aa514-112">Reduce the number of concurrent open handles by closing some handles, and then retry the operation.</span></span> <span data-ttu-id="aa514-113">További információkért lásd: [Microsoft Azure Storage teljesítményére és méretezhetőségére ellenőrzőlista](storage-performance-checklist.md).</span><span class="sxs-lookup"><span data-stu-id="aa514-113">For more information, see [Microsoft Azure Storage performance and scalability checklist](storage-performance-checklist.md).</span></span>

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-file-storage-in-linux"></a><span data-ttu-id="aa514-114">Fájl másolása és az Azure File storage a Linux lassú</span><span class="sxs-lookup"><span data-stu-id="aa514-114">Slow file copying to and from Azure File storage in Linux</span></span>

-   <span data-ttu-id="aa514-115">Ha nem rendelkezik egy adott minimális i/o vonatkozó méretbeli követelményt, ajánlott, hogy 1 MB, az I/O méretéről az optimális teljesítmény érdekében.</span><span class="sxs-lookup"><span data-stu-id="aa514-115">If you don’t have a specific minimum I/O size requirement, we recommend that you use 1 MB as the I/O size for optimal performance.</span></span>
-   <span data-ttu-id="aa514-116">Ha ismeri a végleges mérethez, amelyet az írási műveletek használatával kiterjesztése, és a szoftver nem kompatibilitási problémákat tapasztal, ha a fájl egy unwritten utóhívás nullákat tartalmaz, majd állítsa be a fájlméret, így minden egyes egy kibővítése írási helyett előre.</span><span class="sxs-lookup"><span data-stu-id="aa514-116">If you know the final size of a file that you are extending by using writes, and your software doesn’t experience compatibility problems when an unwritten tail on the file contains zeros, then set the file size in advance instead of making every write an extending write.</span></span>
-   <span data-ttu-id="aa514-117">A jobb oldali copy metódust használja:</span><span class="sxs-lookup"><span data-stu-id="aa514-117">Use the right copy method:</span></span>
    -   <span data-ttu-id="aa514-118">Használjon [AzCopy](storage-use-azcopy.md#file-copy) bármely két fájlmegosztások közötti átvitel céljából.</span><span class="sxs-lookup"><span data-stu-id="aa514-118">Use [AzCopy](storage-use-azcopy.md#file-copy) for any transfer between two file shares.</span></span>
    -   <span data-ttu-id="aa514-119">Használjon [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) közötti fájlmegosztások a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="aa514-119">Use [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) between file shares on an on-premises computer.</span></span>

<a id="error112"></a>
## <a name="mount-error112-host-is-down-because-of-a-reconnection-time-out"></a><span data-ttu-id="aa514-120">"Error(112) csatlakoztatni: az állomás nem működik" újracsatlakozás időtúllépés miatt</span><span class="sxs-lookup"><span data-stu-id="aa514-120">"Mount error(112): Host is down" because of a reconnection time-out</span></span>

<span data-ttu-id="aa514-121">A "112" csatlakozási hiba akkor lép fel a Linux-ügyfél, amikor az ügyfél hosszú ideig inaktív volt.</span><span class="sxs-lookup"><span data-stu-id="aa514-121">A "112" mount error occurs on the Linux client when the client has been idle for a long time.</span></span> <span data-ttu-id="aa514-122">Kiterjesztett üresjárati idő után az ügyfél kapcsolata megszakad, és a kapcsolat időtúllépés miatt.</span><span class="sxs-lookup"><span data-stu-id="aa514-122">After extended idle time, the client disconnects and the connection times out.</span></span>  

### <a name="cause"></a><span data-ttu-id="aa514-123">Ok</span><span class="sxs-lookup"><span data-stu-id="aa514-123">Cause</span></span>

<span data-ttu-id="aa514-124">A kapcsolat üresjáratban lehet, a következő okok miatt:</span><span class="sxs-lookup"><span data-stu-id="aa514-124">The connection can be idle for the following reasons:</span></span>

-   <span data-ttu-id="aa514-125">Hálózati kommunikációs hibák, amelyek megakadályozzák a TCP-kapcsolat a kiszolgálóval újbóli bekapcsolását. az alapértelmezett beállítás a "soft" csatlakoztatási használata esetén</span><span class="sxs-lookup"><span data-stu-id="aa514-125">Network communication failures that prevent re-establishing a TCP connection to the server when the default “soft” mount option is used</span></span>
-   <span data-ttu-id="aa514-126">Legutóbbi újracsatlakozás tartalmaz, amelyek nem szerepelnek a régebbi kernelek</span><span class="sxs-lookup"><span data-stu-id="aa514-126">Recent reconnection fixes that are not present in older kernels</span></span>

### <a name="solution"></a><span data-ttu-id="aa514-127">Megoldás</span><span class="sxs-lookup"><span data-stu-id="aa514-127">Solution</span></span>

<span data-ttu-id="aa514-128">Ez lehet újracsatlakozni a Linux kernel most problémát részeként a következő módosításokat:</span><span class="sxs-lookup"><span data-stu-id="aa514-128">This reconnection problem in the Linux kernel is now fixed as part of the following changes:</span></span>

- [<span data-ttu-id="aa514-129">Javítás kapcsolódjon újra késleltetésének mellőzése smb3 munkamenet hosszú feldolgozása után szoftvercsatorna újracsatlakozás</span><span class="sxs-lookup"><span data-stu-id="aa514-129">Fix reconnect to not defer smb3 session reconnect long after socket reconnect</span></span>](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93)
-   [<span data-ttu-id="aa514-130">Hívó echo szoftvercsatorna újracsatlakozás után azonnal</span><span class="sxs-lookup"><span data-stu-id="aa514-130">Call echo service immediately after socket reconnect</span></span>](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b8c600120fc87d53642476f48c8055b38d6e14c7)
-   [<span data-ttu-id="aa514-131">CIFS: Javítsa ki a lehetséges memóriasérülést újracsatlakozás során</span><span class="sxs-lookup"><span data-stu-id="aa514-131">CIFS: Fix a possible memory corruption during reconnect</span></span>](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=53e0e11efe9289535b060a51d4cf37c25e0d0f2b)
-   [<span data-ttu-id="aa514-132">CIFS: Javítsa ki a lehetséges dupla zárolását mutex során újracsatlakozás (kernel v4.9 és újabb)</span><span class="sxs-lookup"><span data-stu-id="aa514-132">CIFS: Fix a possible double locking of mutex during reconnect (for kernel v4.9 and later)</span></span>](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=96a988ffeb90dba33a71c3826086fe67c897a183)

<span data-ttu-id="aa514-133">Azonban ezek a változások előfordulhat, hogy nem használatát. még a Linux-disztribúció.</span><span class="sxs-lookup"><span data-stu-id="aa514-133">However, these changes might not be ported yet to all the Linux distributions.</span></span> <span data-ttu-id="aa514-134">A következő népszerű Linux kernelek végzett javítás és egyéb újracsatlakozás javításai: 4.4.40 4.8.16 és 4.9.1..</span><span class="sxs-lookup"><span data-stu-id="aa514-134">This fix and other reconnection fixes are made in the following popular Linux kernels: 4.4.40, 4.8.16, and 4.9.1.</span></span> <span data-ttu-id="aa514-135">Ez a javítás kaphat ilyen ajánlott kernel verziójú frissítése.</span><span class="sxs-lookup"><span data-stu-id="aa514-135">You can get this fix by upgrading to one of these recommended kernel versions.</span></span>

### <a name="workaround"></a><span data-ttu-id="aa514-136">Megkerülő megoldás</span><span class="sxs-lookup"><span data-stu-id="aa514-136">Workaround</span></span>

<span data-ttu-id="aa514-137">Ez a probléma megkerüléséhez kötött megadásával.</span><span class="sxs-lookup"><span data-stu-id="aa514-137">You can work around this problem by specifying a hard mount.</span></span> <span data-ttu-id="aa514-138">Ez arra kényszeríti az ügyfél számára, várjon, amíg létrejön a kapcsolat, vagy explicit módon megszakad, és a hálózati időtúllépések miatt megelőzése érdekében használható.</span><span class="sxs-lookup"><span data-stu-id="aa514-138">This forces the client to wait until a connection is established or until it’s explicitly interrupted and can be used to prevent errors because of network time-outs.</span></span> <span data-ttu-id="aa514-139">Ez a megoldás azonban határozatlan ideig vár okozhatja.</span><span class="sxs-lookup"><span data-stu-id="aa514-139">However, this workaround might cause indefinite waits.</span></span> <span data-ttu-id="aa514-140">Készüljön kapcsolatok szükség szerint állítsa le.</span><span class="sxs-lookup"><span data-stu-id="aa514-140">Be prepared to stop connections as necessary.</span></span>

<span data-ttu-id="aa514-141">A legújabb kernel verziói nem frissíthetők, ha meg a probléma úgy kerülheti megőrzi a fájl található Azure fájlmegosztás 30 másodpercenként vagy kisebb írást.</span><span class="sxs-lookup"><span data-stu-id="aa514-141">If you cannot upgrade to the latest kernel versions, you can work around this problem by keeping a file in the Azure file share that you write to every 30 seconds or less.</span></span> <span data-ttu-id="aa514-142">Írási művelet, például a létrehozott vagy módosított dátum újraírását fájlon kell lennie.</span><span class="sxs-lookup"><span data-stu-id="aa514-142">This must be a write operation, such as rewriting the created or modified date on the file.</span></span> <span data-ttu-id="aa514-143">Ellenkező esetben kaphat gyorsítótárazott eredményt, és a művelet nem válthat ki az újracsatlakozás.</span><span class="sxs-lookup"><span data-stu-id="aa514-143">Otherwise, you might get cached results, and your operation might not trigger the reconnection.</span></span>

<a id="error115"></a>
## <a name="mount-error115-operation-now-in-progress-when-you-mount-azure-file-storage-by-using-smb-30"></a><span data-ttu-id="aa514-144">"Error(115) csatlakoztatni: most a folyamatban lévő művelet" Azure File storage csatlakoztatásakor SMB 3.0 használatával</span><span class="sxs-lookup"><span data-stu-id="aa514-144">"Mount error(115): Operation now in progress" when you mount Azure File storage by using SMB 3.0</span></span>

### <a name="cause"></a><span data-ttu-id="aa514-145">Ok</span><span class="sxs-lookup"><span data-stu-id="aa514-145">Cause</span></span>

<span data-ttu-id="aa514-146">Egyes Linux terjesztésekről még nem támogatják a titkosítási szolgáltatások az SMB 3.0 és a felhasználók előfordulhat, hogy "115" hibaüzenetet kap, ha megpróbálják csatlakoztatási Azure fájltároló SMB 3.0 miatt egy hiányzó funkció használatával.</span><span class="sxs-lookup"><span data-stu-id="aa514-146">Some Linux distributions do not yet support encryption features in SMB 3.0 and users might receive a "115" error message if they try to mount Azure File storage by using SMB 3.0 because of a missing feature.</span></span>

### <a name="solution"></a><span data-ttu-id="aa514-147">Megoldás</span><span class="sxs-lookup"><span data-stu-id="aa514-147">Solution</span></span>

<span data-ttu-id="aa514-148">Az SMB 3.0 Linux titkosítási szolgáltatás 4.11 kernel jelent meg.</span><span class="sxs-lookup"><span data-stu-id="aa514-148">Encryption feature for SMB 3.0 for Linux was introduced in 4.11 kernel.</span></span> <span data-ttu-id="aa514-149">Ez a funkció lehetővé teszi, hogy az Azure fájlmegosztás regisztrációját a helyi vagy egy másik Azure-régiót csatlakoztatását.</span><span class="sxs-lookup"><span data-stu-id="aa514-149">This feature enables mounting of Azure File share from on-premises or a different Azure region.</span></span> <span data-ttu-id="aa514-150">A közzététel időpontjában ez a funkció le lett Ubuntu 17.04 és Ubuntu 16.10 backported.</span><span class="sxs-lookup"><span data-stu-id="aa514-150">At the time of publishing, this functionality has been backported to Ubuntu 17.04 and Ubuntu 16.10.</span></span> <span data-ttu-id="aa514-151">Ha a Linux SMB-ügyfél nem támogatja a titkosítást, csatlakoztassa az Azure File storage SMB 2.1 egy Azure Linux virtuális gép, amely ugyanabban az adatközpontban, a fájl tárolási fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="aa514-151">If your Linux SMB client does not support encryption, mount Azure File storage by using SMB 2.1 from an Azure Linux VM that's in the same datacenter as the File storage account.</span></span>

<a id="slowperformance"></a>
## <a name="slow-performance-on-an-azure-file-share-mounted-on-a-linux-vm"></a><span data-ttu-id="aa514-152">A Linux virtuális gép csatlakoztatott lassú teljesítmény az Azure fájlmegosztások</span><span class="sxs-lookup"><span data-stu-id="aa514-152">Slow performance on an Azure file share mounted on a Linux VM</span></span>

### <a name="cause"></a><span data-ttu-id="aa514-153">Ok</span><span class="sxs-lookup"><span data-stu-id="aa514-153">Cause</span></span>

<span data-ttu-id="aa514-154">Egy lehetséges oka a lassú teljesítmény le van tiltva, gyorsítótárazás.</span><span class="sxs-lookup"><span data-stu-id="aa514-154">One possible cause of slow performance is disabled caching.</span></span>

### <a name="solution"></a><span data-ttu-id="aa514-155">Megoldás</span><span class="sxs-lookup"><span data-stu-id="aa514-155">Solution</span></span>

<span data-ttu-id="aa514-156">Ellenőrizze, hogy gyorsítótárazás le van tiltva, keresse meg a **gyorsítótár =** bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="aa514-156">To check whether caching is disabled, look for the **cache=** entry.</span></span> 

<span data-ttu-id="aa514-157">**Gyorsítótár = none** azt jelzi, hogy gyorsítótárazás le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="aa514-157">**Cache=none** indicates that caching is disabled.</span></span>  <span data-ttu-id="aa514-158">Csatlakoztassa újra a megosztás, az alapértelmezett csatlakoztatási parancs használatával, vagy explicit módon adja hozzá a **gyorsítótár = szigorú** a csatlakoztatási parancs használatával győződjön meg arról, hogy alapértelmezett gyorsítótárazását, vagy "strict" gyorsítótárazási mód engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="aa514-158">Remount the share by using the default mount command or by explicitly adding the **cache=strict** option to the mount command to ensure that default caching or "strict" caching mode is enabled.</span></span>

<span data-ttu-id="aa514-159">Bizonyos esetekben a **serverino** csatlakoztatási beállítás hatására a **ls** parancsot kell futtatni minden könyvtárbejegyzés elleni stat.</span><span class="sxs-lookup"><span data-stu-id="aa514-159">In some scenarios, the **serverino** mount option can cause the **ls** command to run stat against every directory entry.</span></span> <span data-ttu-id="aa514-160">Ezt a viselkedést eredményezi teljesítménycsökkenés nagy könyvtár listázásakor van.</span><span class="sxs-lookup"><span data-stu-id="aa514-160">This behavior results in performance degradation when you're listing a big directory.</span></span> <span data-ttu-id="aa514-161">Ellenőrizheti a csatlakoztatási lehetőségek a **/etc/fstab** bejegyzést:</span><span class="sxs-lookup"><span data-stu-id="aa514-161">You can check the mount options in your **/etc/fstab** entry:</span></span>

`//azureuser.file.core.windows.net/cifs /cifs cifs vers=3.0,serverino,username=xxx,password=xxx,dir_mode=0777,file_mode=0777`

<span data-ttu-id="aa514-162">Azt is ellenőrizheti, hogy a helyes beállításokat használják futtatásával a **sudo csatlakoztatási |} grep cifs** parancs és a kimenetet, például a következő egy példa a kimenetre ellenőrzése:</span><span class="sxs-lookup"><span data-stu-id="aa514-162">You can also check whether the correct options are being used by running the  **sudo mount | grep cifs** command and checking its output, such as the following example output:</span></span>

`//mabiccacifs.file.core.windows.net/cifs on /cifs type cifs (rw,relatime,vers=3.0,sec=ntlmssp,cache=strict,username=xxx,domain=X,uid=0,noforceuid,gid=0,noforcegid,addr=192.168.10.1,file_mode=0777, dir_mode=0777,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,actimeo=1)`

<span data-ttu-id="aa514-163">Ha a **gyorsítótár = szigorú** vagy **serverino** elem nem létezik, válassza le a lemezképet, és csatlakoztassa Azure File storage újra a csatlakoztatási parancs futtatásával a [dokumentáció](storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="aa514-163">If the **cache=strict** or **serverino** option is not present, unmount and mount Azure File storage again by running the mount command from the [documentation](storage-how-to-use-files-linux.md).</span></span> <span data-ttu-id="aa514-164">Ezt követően újbóli ellenőrzéséhez, hogy a **/etc/fstab** bejegyzés rendelkezik a megfelelő beállításokat.</span><span class="sxs-lookup"><span data-stu-id="aa514-164">Then, recheck that the **/etc/fstab** entry has the correct options.</span></span>

<a id="timestampslost"></a>
## <a name="time-stamps-were-lost-in-copying-files-from-windows-to-linux"></a><span data-ttu-id="aa514-165">A fájlok másolása a Windows Linux időbélyegeket elvesztek</span><span class="sxs-lookup"><span data-stu-id="aa514-165">Time stamps were lost in copying files from Windows to Linux</span></span>

<span data-ttu-id="aa514-166">Linux/Unix platformokon a **cp -p** parancs sikertelen lesz, ha a különböző felhasználók 1 és 2 fájlt a tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="aa514-166">On Linux/Unix platforms, the **cp -p** command fails if file 1 and file 2 are owned by different users.</span></span>

### <a name="cause"></a><span data-ttu-id="aa514-167">Ok</span><span class="sxs-lookup"><span data-stu-id="aa514-167">Cause</span></span>

<span data-ttu-id="aa514-168">A force jelzést **f** a COPYFILE eredményezi végrehajtása **cp -p -f** UNIX.</span><span class="sxs-lookup"><span data-stu-id="aa514-168">The force flag **f** in COPYFILE results in executing **cp -p -f** on Unix.</span></span> <span data-ttu-id="aa514-169">Ez a parancs is sikertelen lesz, a fájlt, amely nem Ön a tulajdonosa időbélyegzőjét megőrzése érdekében.</span><span class="sxs-lookup"><span data-stu-id="aa514-169">This command also fails to preserve the time stamp of the file that you don't own.</span></span>

### <a name="workaround"></a><span data-ttu-id="aa514-170">Megkerülő megoldás</span><span class="sxs-lookup"><span data-stu-id="aa514-170">Workaround</span></span>

<span data-ttu-id="aa514-171">A tárolási fiók felhasználójának használ a fájlok másolása:</span><span class="sxs-lookup"><span data-stu-id="aa514-171">Use the storage account user for copying the files:</span></span>

- `Useadd : [storage account name]`
- `Passwd [storage account name]`
- `Su [storage account name]`
- `Cp -p filename.txt /share`

## <a name="need-help-contact-support"></a><span data-ttu-id="aa514-172">Segítség</span><span class="sxs-lookup"><span data-stu-id="aa514-172">Need help?</span></span> <span data-ttu-id="aa514-173">Forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="aa514-173">Contact support.</span></span>

<span data-ttu-id="aa514-174">Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a gyorsan megoldott probléma eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="aa514-174">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your problem resolved quickly.</span></span>
