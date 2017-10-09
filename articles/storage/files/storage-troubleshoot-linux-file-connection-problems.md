---
title: "aaaTroubleshoot Azure File storage problémák Linux |} Microsoft Docs"
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
ms.openlocfilehash: 4bdc3c6ed2e48f245060a03632fca9bd14d33545
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-file-storage-problems-in-linux"></a><span data-ttu-id="839c2-103">Azure File storage kapcsolatos problémák elhárítása a Linux</span><span class="sxs-lookup"><span data-stu-id="839c2-103">Troubleshoot Azure File storage problems in Linux</span></span>

<span data-ttu-id="839c2-104">A cikk ismerteti a gyakori problémák, amelyek kapcsolódó tooMicrosoft Azure File storage Linux-ügyfelek a csatlakozáskor.</span><span class="sxs-lookup"><span data-stu-id="839c2-104">This article lists common problems that are related tooMicrosoft Azure File storage when you connect from Linux clients.</span></span> <span data-ttu-id="839c2-105">Is biztosít lehetséges okokért és megoldásokért ezeket a problémákat.</span><span class="sxs-lookup"><span data-stu-id="839c2-105">It also provides possible causes and resolutions for these problems.</span></span>

<a id="permissiondenied"></a>
## <a name="permission-denied-disk-quota-exceeded-when-you-try-tooopen-a-file"></a><span data-ttu-id="839c2-106">"[engedély megtagadva] lemez kvóta túllépve" tooopen egy fájl meg</span><span class="sxs-lookup"><span data-stu-id="839c2-106">"[permission denied] Disk quota exceeded" when you try tooopen a file</span></span>

<span data-ttu-id="839c2-107">A Linux hello alábbihoz hasonló hibaüzenetet kap:</span><span class="sxs-lookup"><span data-stu-id="839c2-107">In Linux, you receive an error message that resembles hello following:</span></span>

<span data-ttu-id="839c2-108">**<filename>[a hozzáférés megtagadva] Lemez kvótája túllépve**</span><span class="sxs-lookup"><span data-stu-id="839c2-108">**<filename> [permission denied] Disk quota exceeded**</span></span>

### <a name="cause"></a><span data-ttu-id="839c2-109">Ok</span><span class="sxs-lookup"><span data-stu-id="839c2-109">Cause</span></span>

<span data-ttu-id="839c2-110">Elérte a felső korlátja hello egyidejű megnyitott leíróinak fájl számára engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="839c2-110">You have reached hello upper limit of concurrent open handles that are allowed for a file.</span></span>

### <a name="solution"></a><span data-ttu-id="839c2-111">Megoldás</span><span class="sxs-lookup"><span data-stu-id="839c2-111">Solution</span></span>

<span data-ttu-id="839c2-112">Csökkentse a hello száma párhuzamos megnyitott leíróinak néhány leírók bezárásával, majd próbálkozzon újra a hello műveletet.</span><span class="sxs-lookup"><span data-stu-id="839c2-112">Reduce hello number of concurrent open handles by closing some handles, and then retry hello operation.</span></span> <span data-ttu-id="839c2-113">További információkért lásd: [Microsoft Azure Storage teljesítményére és méretezhetőségére ellenőrzőlista](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="839c2-113">For more information, see [Microsoft Azure Storage performance and scalability checklist](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).</span></span>

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-tooand-from-azure-file-storage-in-linux"></a><span data-ttu-id="839c2-114">Lassú fájl tooand másolása az Azure File storage a Linux</span><span class="sxs-lookup"><span data-stu-id="839c2-114">Slow file copying tooand from Azure File storage in Linux</span></span>

-   <span data-ttu-id="839c2-115">Még nem rendelkezik egy adott minimális i/o vonatkozó méretbeli követelményt, azt javasoljuk, hogy használható 1 MB hello i/o-méret az optimális teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="839c2-115">If you don’t have a specific minimum I/O size requirement, we recommend that you use 1 MB as hello I/O size for optimal performance.</span></span>
-   <span data-ttu-id="839c2-116">Ha tudja, egy fájlt, amely írási műveletek használatával kiterjesztése hello végső méretét, és a szoftver nem kompatibilitási problémákat tapasztal, amikor egy unwritten utóhívás hello fájlon nullákat tartalmaz, majd állítsa be hello fájlméret előre helyett egy kiterjesztése minden egyes elvégzése írási.</span><span class="sxs-lookup"><span data-stu-id="839c2-116">If you know hello final size of a file that you are extending by using writes, and your software doesn’t experience compatibility problems when an unwritten tail on hello file contains zeros, then set hello file size in advance instead of making every write an extending write.</span></span>
-   <span data-ttu-id="839c2-117">Hello jobb copy metódust használja:</span><span class="sxs-lookup"><span data-stu-id="839c2-117">Use hello right copy method:</span></span>
    -   <span data-ttu-id="839c2-118">Használjon [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#file-copy) bármely két fájlmegosztások közötti átvitel céljából.</span><span class="sxs-lookup"><span data-stu-id="839c2-118">Use [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#file-copy) for any transfer between two file shares.</span></span>
    -   <span data-ttu-id="839c2-119">Használjon [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) közötti fájlmegosztások a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="839c2-119">Use [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) between file shares on an on-premises computer.</span></span>

<a id="error112"></a>
## <a name="mount-error112-host-is-down-because-of-a-reconnection-time-out"></a><span data-ttu-id="839c2-120">"Error(112) csatlakoztatni: az állomás nem működik" újracsatlakozás időtúllépés miatt</span><span class="sxs-lookup"><span data-stu-id="839c2-120">"Mount error(112): Host is down" because of a reconnection time-out</span></span>

<span data-ttu-id="839c2-121">Egy "112" csatlakozási hiba esetén hello Linux ügyfélen hello ügyfél hosszú ideig inaktív volt.</span><span class="sxs-lookup"><span data-stu-id="839c2-121">A "112" mount error occurs on hello Linux client when hello client has been idle for a long time.</span></span> <span data-ttu-id="839c2-122">Kiterjesztett üresjárati idő után hello ügyfél kapcsolata megszakad, és hello kapcsolat időtúllépése.</span><span class="sxs-lookup"><span data-stu-id="839c2-122">After extended idle time, hello client disconnects and hello connection times out.</span></span>  

### <a name="cause"></a><span data-ttu-id="839c2-123">Ok</span><span class="sxs-lookup"><span data-stu-id="839c2-123">Cause</span></span>

<span data-ttu-id="839c2-124">hello kapcsolat a következő okok miatt hello üresjáratban lehet:</span><span class="sxs-lookup"><span data-stu-id="839c2-124">hello connection can be idle for hello following reasons:</span></span>

-   <span data-ttu-id="839c2-125">Hálózati kommunikációs hibák, amelyek megakadályozzák a TCP-kapcsolati toohello kiszolgáló újbóli bekapcsolását, hello alapértelmezett "soft" csatlakoztatási beállítás használata esetén</span><span class="sxs-lookup"><span data-stu-id="839c2-125">Network communication failures that prevent re-establishing a TCP connection toohello server when hello default “soft” mount option is used</span></span>
-   <span data-ttu-id="839c2-126">Legutóbbi újracsatlakozás tartalmaz, amelyek nem szerepelnek a régebbi kernelek</span><span class="sxs-lookup"><span data-stu-id="839c2-126">Recent reconnection fixes that are not present in older kernels</span></span>

### <a name="solution"></a><span data-ttu-id="839c2-127">Megoldás</span><span class="sxs-lookup"><span data-stu-id="839c2-127">Solution</span></span>

<span data-ttu-id="839c2-128">Az újracsatlakozás hello Linux kernel most problémát a következő módosításokat hello részeként:</span><span class="sxs-lookup"><span data-stu-id="839c2-128">This reconnection problem in hello Linux kernel is now fixed as part of hello following changes:</span></span>

- [<span data-ttu-id="839c2-129">Javítás újracsatlakozás toonot smb3 munkamenet késleltető hosszú feldolgozása után szoftvercsatorna újracsatlakozás</span><span class="sxs-lookup"><span data-stu-id="839c2-129">Fix reconnect toonot defer smb3 session reconnect long after socket reconnect</span></span>](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93)
-   [<span data-ttu-id="839c2-130">Hívó echo szoftvercsatorna újracsatlakozás után azonnal</span><span class="sxs-lookup"><span data-stu-id="839c2-130">Call echo service immediately after socket reconnect</span></span>](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b8c600120fc87d53642476f48c8055b38d6e14c7)
-   [<span data-ttu-id="839c2-131">CIFS: Javítsa ki a lehetséges memóriasérülést újracsatlakozás során</span><span class="sxs-lookup"><span data-stu-id="839c2-131">CIFS: Fix a possible memory corruption during reconnect</span></span>](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=53e0e11efe9289535b060a51d4cf37c25e0d0f2b)
-   [<span data-ttu-id="839c2-132">CIFS: Javítsa ki a lehetséges dupla zárolását mutex során újracsatlakozás (kernel v4.9 és újabb)</span><span class="sxs-lookup"><span data-stu-id="839c2-132">CIFS: Fix a possible double locking of mutex during reconnect (for kernel v4.9 and later)</span></span>](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=96a988ffeb90dba33a71c3826086fe67c897a183)

<span data-ttu-id="839c2-133">Azonban ezek a változások előfordulhat, hogy nem tartalomfájlokat még tooall hello Linux terjesztéseket.</span><span class="sxs-lookup"><span data-stu-id="839c2-133">However, these changes might not be ported yet tooall hello Linux distributions.</span></span> <span data-ttu-id="839c2-134">A következő népszerű Linux kernelek hello végzett javítás és egyéb újracsatlakozás javításai: 4.4.40 4.8.16 és 4.9.1..</span><span class="sxs-lookup"><span data-stu-id="839c2-134">This fix and other reconnection fixes are made in hello following popular Linux kernels: 4.4.40, 4.8.16, and 4.9.1.</span></span> <span data-ttu-id="839c2-135">Ez a javítás kaphat tooone ajánlott kernel verzió frissítése.</span><span class="sxs-lookup"><span data-stu-id="839c2-135">You can get this fix by upgrading tooone of these recommended kernel versions.</span></span>

### <a name="workaround"></a><span data-ttu-id="839c2-136">Megkerülő megoldás</span><span class="sxs-lookup"><span data-stu-id="839c2-136">Workaround</span></span>

<span data-ttu-id="839c2-137">Ez a probléma megkerüléséhez kötött megadásával.</span><span class="sxs-lookup"><span data-stu-id="839c2-137">You can work around this problem by specifying a hard mount.</span></span> <span data-ttu-id="839c2-138">Ekkor elemezni hello ügyfél toowait, amíg létrejön a kapcsolat, vagy explicit módon megszakad, és a hálózati időtúllépések miatt használt tooprevent hibák lehetnek.</span><span class="sxs-lookup"><span data-stu-id="839c2-138">This forces hello client toowait until a connection is established or until it’s explicitly interrupted and can be used tooprevent errors because of network time-outs.</span></span> <span data-ttu-id="839c2-139">Ez a megoldás azonban határozatlan ideig vár okozhatja.</span><span class="sxs-lookup"><span data-stu-id="839c2-139">However, this workaround might cause indefinite waits.</span></span> <span data-ttu-id="839c2-140">Előkészített toostop kapcsolatok szükség lehet.</span><span class="sxs-lookup"><span data-stu-id="839c2-140">Be prepared toostop connections as necessary.</span></span>

<span data-ttu-id="839c2-141">Ha toohello legújabb kernel verziói nem frissíthetők, használhat a probléma megoldásához hello Azure fájlmegosztás tooevery 30 másodperc lehet írni vagy annál kisebb a fájl tartja.</span><span class="sxs-lookup"><span data-stu-id="839c2-141">If you cannot upgrade toohello latest kernel versions, you can work around this problem by keeping a file in hello Azure file share that you write tooevery 30 seconds or less.</span></span> <span data-ttu-id="839c2-142">Írási művelet, például a hello újraírását létrehozott vagy módosítás dátuma hello fájlra kell lennie.</span><span class="sxs-lookup"><span data-stu-id="839c2-142">This must be a write operation, such as rewriting hello created or modified date on hello file.</span></span> <span data-ttu-id="839c2-143">Ellenkező esetben kaphat gyorsítótárazott eredményt, és a művelet nem válthat ki hello lehet újracsatlakozni.</span><span class="sxs-lookup"><span data-stu-id="839c2-143">Otherwise, you might get cached results, and your operation might not trigger hello reconnection.</span></span>

<a id="error115"></a>
## <a name="mount-error115-operation-now-in-progress-when-you-mount-azure-file-storage-by-using-smb-30"></a><span data-ttu-id="839c2-144">"Error(115) csatlakoztatni: most a folyamatban lévő művelet" Azure File storage csatlakoztatásakor SMB 3.0 használatával</span><span class="sxs-lookup"><span data-stu-id="839c2-144">"Mount error(115): Operation now in progress" when you mount Azure File storage by using SMB 3.0</span></span>

### <a name="cause"></a><span data-ttu-id="839c2-145">Ok</span><span class="sxs-lookup"><span data-stu-id="839c2-145">Cause</span></span>

<span data-ttu-id="839c2-146">Egyes Linux terjesztésekről még nem támogatják a titkosítási szolgáltatások az SMB 3.0, és a felhasználók előfordulhat, hogy "115" hibaüzenetet kap, ha megpróbálják toomount Azure fájltároló SMB 3.0 miatt egy hiányzó funkció használatával.</span><span class="sxs-lookup"><span data-stu-id="839c2-146">Some Linux distributions do not yet support encryption features in SMB 3.0 and users might receive a "115" error message if they try toomount Azure File storage by using SMB 3.0 because of a missing feature.</span></span>

### <a name="solution"></a><span data-ttu-id="839c2-147">Megoldás</span><span class="sxs-lookup"><span data-stu-id="839c2-147">Solution</span></span>

<span data-ttu-id="839c2-148">Az SMB 3.0 Linux titkosítási szolgáltatás 4.11 kernel jelent meg.</span><span class="sxs-lookup"><span data-stu-id="839c2-148">Encryption feature for SMB 3.0 for Linux was introduced in 4.11 kernel.</span></span> <span data-ttu-id="839c2-149">Ez a funkció lehetővé teszi, hogy az Azure fájlmegosztás regisztrációját a helyi vagy egy másik Azure-régiót csatlakoztatását.</span><span class="sxs-lookup"><span data-stu-id="839c2-149">This feature enables mounting of Azure File share from on-premises or a different Azure region.</span></span> <span data-ttu-id="839c2-150">A közzététel hello időpontjában ez a funkció le van backported tooUbuntu 17.04 és Ubuntu 16.10.</span><span class="sxs-lookup"><span data-stu-id="839c2-150">At hello time of publishing, this functionality has been backported tooUbuntu 17.04 and Ubuntu 16.10.</span></span> <span data-ttu-id="839c2-151">Ha a Linux SMB-ügyfél nem támogatja a titkosítást, Azure File storage csatlakoztatása az Azure Linux virtuális gép, amely az SMB 2.1 használatával hello ugyanabban az adatközpontban, a fájl tárfiók hello.</span><span class="sxs-lookup"><span data-stu-id="839c2-151">If your Linux SMB client does not support encryption, mount Azure File storage by using SMB 2.1 from an Azure Linux VM that's in hello same datacenter as hello File storage account.</span></span>

<a id="slowperformance"></a>
## <a name="slow-performance-on-an-azure-file-share-mounted-on-a-linux-vm"></a><span data-ttu-id="839c2-152">A Linux virtuális gép csatlakoztatott lassú teljesítmény az Azure fájlmegosztások</span><span class="sxs-lookup"><span data-stu-id="839c2-152">Slow performance on an Azure file share mounted on a Linux VM</span></span>

### <a name="cause"></a><span data-ttu-id="839c2-153">Ok</span><span class="sxs-lookup"><span data-stu-id="839c2-153">Cause</span></span>

<span data-ttu-id="839c2-154">Egy lehetséges oka a lassú teljesítmény le van tiltva, gyorsítótárazás.</span><span class="sxs-lookup"><span data-stu-id="839c2-154">One possible cause of slow performance is disabled caching.</span></span>

### <a name="solution"></a><span data-ttu-id="839c2-155">Megoldás</span><span class="sxs-lookup"><span data-stu-id="839c2-155">Solution</span></span>

<span data-ttu-id="839c2-156">toocheck gyorsítótárazás le van tiltva, hogy keressen hello **gyorsítótár =** bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="839c2-156">toocheck whether caching is disabled, look for hello **cache=** entry.</span></span> 

<span data-ttu-id="839c2-157">**Gyorsítótár = none** azt jelzi, hogy gyorsítótárazás le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="839c2-157">**Cache=none** indicates that caching is disabled.</span></span>  <span data-ttu-id="839c2-158">Újracsatlakoztatást hello megosztás hello alapértelmezett csatlakoztatási parancs használatával, vagy explicit módon adja hozzá a hello **gyorsítótár = szigorú** beállítás toohello csatlakoztatási parancs tooensure, amely az alapértelmezett gyorsítótárazási vagy "strict" gyorsítótárazási mód engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="839c2-158">Remount hello share by using hello default mount command or by explicitly adding hello **cache=strict** option toohello mount command tooensure that default caching or "strict" caching mode is enabled.</span></span>

<span data-ttu-id="839c2-159">Bizonyos esetekben hello **serverino** csatlakoztatási beállítás hatására a hello **ls** parancs toorun stat minden könyvtárbejegyzés ellen.</span><span class="sxs-lookup"><span data-stu-id="839c2-159">In some scenarios, hello **serverino** mount option can cause hello **ls** command toorun stat against every directory entry.</span></span> <span data-ttu-id="839c2-160">Ezt a viselkedést eredményezi teljesítménycsökkenés nagy könyvtár listázásakor van.</span><span class="sxs-lookup"><span data-stu-id="839c2-160">This behavior results in performance degradation when you're listing a big directory.</span></span> <span data-ttu-id="839c2-161">Hello csatlakoztatási lehetőségek ellenőrizheti a **/etc/fstab** bejegyzést:</span><span class="sxs-lookup"><span data-stu-id="839c2-161">You can check hello mount options in your **/etc/fstab** entry:</span></span>

`//azureuser.file.core.windows.net/cifs /cifs cifs vers=3.0,serverino,username=xxx,password=xxx,dir_mode=0777,file_mode=0777`

<span data-ttu-id="839c2-162">Azt is ellenőrizheti, hogy használják-e a megfelelő hello-beállítások hello futtatásával **sudo csatlakoztatási |} grep cifs** parancs és a kimenetet, például a következő egy példa a kimenetre hello ellenőrzése:</span><span class="sxs-lookup"><span data-stu-id="839c2-162">You can also check whether hello correct options are being used by running hello  **sudo mount | grep cifs** command and checking its output, such as hello following example output:</span></span>

`//mabiccacifs.file.core.windows.net/cifs on /cifs type cifs (rw,relatime,vers=3.0,sec=ntlmssp,cache=strict,username=xxx,domain=X,uid=0,noforceuid,gid=0,noforcegid,addr=192.168.10.1,file_mode=0777, dir_mode=0777,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,actimeo=1)`

<span data-ttu-id="839c2-163">Ha hello **gyorsítótár = szigorú** vagy **serverino** elem nem létezik, válassza le a lemezképet, és csatlakoztassa Azure File storage újra hello hello csatlakoztatási parancs futtatásával [dokumentáció](../storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="839c2-163">If hello **cache=strict** or **serverino** option is not present, unmount and mount Azure File storage again by running hello mount command from hello [documentation](../storage-how-to-use-files-linux.md).</span></span> <span data-ttu-id="839c2-164">Ezt követően az adott hello újbóli **/etc/fstab** bejegyzés rendelkezik hello megfelelő beállításokat.</span><span class="sxs-lookup"><span data-stu-id="839c2-164">Then, recheck that hello **/etc/fstab** entry has hello correct options.</span></span>

<a id="timestampslost"></a>
## <a name="time-stamps-were-lost-in-copying-files-from-windows-toolinux"></a><span data-ttu-id="839c2-165">A fájlok másolását a Windows tooLinux időbélyegeket elvesztek</span><span class="sxs-lookup"><span data-stu-id="839c2-165">Time stamps were lost in copying files from Windows tooLinux</span></span>

<span data-ttu-id="839c2-166">Linux/Unix platformokon hello **cp -p** parancs sikertelen lesz, ha a különböző felhasználók 1 és 2 fájlt a tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="839c2-166">On Linux/Unix platforms, hello **cp -p** command fails if file 1 and file 2 are owned by different users.</span></span>

### <a name="cause"></a><span data-ttu-id="839c2-167">Ok</span><span class="sxs-lookup"><span data-stu-id="839c2-167">Cause</span></span>

<span data-ttu-id="839c2-168">hello force jelzést **f** a COPYFILE eredményezi végrehajtása **cp -p -f** UNIX.</span><span class="sxs-lookup"><span data-stu-id="839c2-168">hello force flag **f** in COPYFILE results in executing **cp -p -f** on Unix.</span></span> <span data-ttu-id="839c2-169">Ez a parancs is sikertelen toopreserve hello időbélyegzőjét hello fájl, amely nem Ön a tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="839c2-169">This command also fails toopreserve hello time stamp of hello file that you don't own.</span></span>

### <a name="workaround"></a><span data-ttu-id="839c2-170">Megkerülő megoldás</span><span class="sxs-lookup"><span data-stu-id="839c2-170">Workaround</span></span>

<span data-ttu-id="839c2-171">Hello tárolási fiók felhasználói hello fájlok másolása használhatja:</span><span class="sxs-lookup"><span data-stu-id="839c2-171">Use hello storage account user for copying hello files:</span></span>

- `Useadd : [storage account name]`
- `Passwd [storage account name]`
- `Su [storage account name]`
- `Cp -p filename.txt /share`

## <a name="need-help-contact-support"></a><span data-ttu-id="839c2-172">Segítség</span><span class="sxs-lookup"><span data-stu-id="839c2-172">Need help?</span></span> <span data-ttu-id="839c2-173">Forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="839c2-173">Contact support.</span></span>

<span data-ttu-id="839c2-174">Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget gyorsan oldja meg a problémát.</span><span class="sxs-lookup"><span data-stu-id="839c2-174">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget your problem resolved quickly.</span></span>
