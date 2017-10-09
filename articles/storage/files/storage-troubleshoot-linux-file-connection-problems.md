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
# <a name="troubleshoot-azure-file-storage-problems-in-linux"></a>Azure File storage kapcsolatos problémák elhárítása a Linux

A cikk ismerteti a gyakori problémák, amelyek kapcsolódó tooMicrosoft Azure File storage Linux-ügyfelek a csatlakozáskor. Is biztosít lehetséges okokért és megoldásokért ezeket a problémákat.

<a id="permissiondenied"></a>
## <a name="permission-denied-disk-quota-exceeded-when-you-try-tooopen-a-file"></a>"[engedély megtagadva] lemez kvóta túllépve" tooopen egy fájl meg

A Linux hello alábbihoz hasonló hibaüzenetet kap:

**<filename>[a hozzáférés megtagadva] Lemez kvótája túllépve**

### <a name="cause"></a>Ok

Elérte a felső korlátja hello egyidejű megnyitott leíróinak fájl számára engedélyezett.

### <a name="solution"></a>Megoldás

Csökkentse a hello száma párhuzamos megnyitott leíróinak néhány leírók bezárásával, majd próbálkozzon újra a hello műveletet. További információkért lásd: [Microsoft Azure Storage teljesítményére és méretezhetőségére ellenőrzőlista](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-tooand-from-azure-file-storage-in-linux"></a>Lassú fájl tooand másolása az Azure File storage a Linux

-   Még nem rendelkezik egy adott minimális i/o vonatkozó méretbeli követelményt, azt javasoljuk, hogy használható 1 MB hello i/o-méret az optimális teljesítmény.
-   Ha tudja, egy fájlt, amely írási műveletek használatával kiterjesztése hello végső méretét, és a szoftver nem kompatibilitási problémákat tapasztal, amikor egy unwritten utóhívás hello fájlon nullákat tartalmaz, majd állítsa be hello fájlméret előre helyett egy kiterjesztése minden egyes elvégzése írási.
-   Hello jobb copy metódust használja:
    -   Használjon [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#file-copy) bármely két fájlmegosztások közötti átvitel céljából.
    -   Használjon [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) közötti fájlmegosztások a helyi számítógépen.

<a id="error112"></a>
## <a name="mount-error112-host-is-down-because-of-a-reconnection-time-out"></a>"Error(112) csatlakoztatni: az állomás nem működik" újracsatlakozás időtúllépés miatt

Egy "112" csatlakozási hiba esetén hello Linux ügyfélen hello ügyfél hosszú ideig inaktív volt. Kiterjesztett üresjárati idő után hello ügyfél kapcsolata megszakad, és hello kapcsolat időtúllépése.  

### <a name="cause"></a>Ok

hello kapcsolat a következő okok miatt hello üresjáratban lehet:

-   Hálózati kommunikációs hibák, amelyek megakadályozzák a TCP-kapcsolati toohello kiszolgáló újbóli bekapcsolását, hello alapértelmezett "soft" csatlakoztatási beállítás használata esetén
-   Legutóbbi újracsatlakozás tartalmaz, amelyek nem szerepelnek a régebbi kernelek

### <a name="solution"></a>Megoldás

Az újracsatlakozás hello Linux kernel most problémát a következő módosításokat hello részeként:

- [Javítás újracsatlakozás toonot smb3 munkamenet késleltető hosszú feldolgozása után szoftvercsatorna újracsatlakozás](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93)
-   [Hívó echo szoftvercsatorna újracsatlakozás után azonnal](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b8c600120fc87d53642476f48c8055b38d6e14c7)
-   [CIFS: Javítsa ki a lehetséges memóriasérülést újracsatlakozás során](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=53e0e11efe9289535b060a51d4cf37c25e0d0f2b)
-   [CIFS: Javítsa ki a lehetséges dupla zárolását mutex során újracsatlakozás (kernel v4.9 és újabb)](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=96a988ffeb90dba33a71c3826086fe67c897a183)

Azonban ezek a változások előfordulhat, hogy nem tartalomfájlokat még tooall hello Linux terjesztéseket. A következő népszerű Linux kernelek hello végzett javítás és egyéb újracsatlakozás javításai: 4.4.40 4.8.16 és 4.9.1.. Ez a javítás kaphat tooone ajánlott kernel verzió frissítése.

### <a name="workaround"></a>Megkerülő megoldás

Ez a probléma megkerüléséhez kötött megadásával. Ekkor elemezni hello ügyfél toowait, amíg létrejön a kapcsolat, vagy explicit módon megszakad, és a hálózati időtúllépések miatt használt tooprevent hibák lehetnek. Ez a megoldás azonban határozatlan ideig vár okozhatja. Előkészített toostop kapcsolatok szükség lehet.

Ha toohello legújabb kernel verziói nem frissíthetők, használhat a probléma megoldásához hello Azure fájlmegosztás tooevery 30 másodperc lehet írni vagy annál kisebb a fájl tartja. Írási művelet, például a hello újraírását létrehozott vagy módosítás dátuma hello fájlra kell lennie. Ellenkező esetben kaphat gyorsítótárazott eredményt, és a művelet nem válthat ki hello lehet újracsatlakozni.

<a id="error115"></a>
## <a name="mount-error115-operation-now-in-progress-when-you-mount-azure-file-storage-by-using-smb-30"></a>"Error(115) csatlakoztatni: most a folyamatban lévő művelet" Azure File storage csatlakoztatásakor SMB 3.0 használatával

### <a name="cause"></a>Ok

Egyes Linux terjesztésekről még nem támogatják a titkosítási szolgáltatások az SMB 3.0, és a felhasználók előfordulhat, hogy "115" hibaüzenetet kap, ha megpróbálják toomount Azure fájltároló SMB 3.0 miatt egy hiányzó funkció használatával.

### <a name="solution"></a>Megoldás

Az SMB 3.0 Linux titkosítási szolgáltatás 4.11 kernel jelent meg. Ez a funkció lehetővé teszi, hogy az Azure fájlmegosztás regisztrációját a helyi vagy egy másik Azure-régiót csatlakoztatását. A közzététel hello időpontjában ez a funkció le van backported tooUbuntu 17.04 és Ubuntu 16.10. Ha a Linux SMB-ügyfél nem támogatja a titkosítást, Azure File storage csatlakoztatása az Azure Linux virtuális gép, amely az SMB 2.1 használatával hello ugyanabban az adatközpontban, a fájl tárfiók hello.

<a id="slowperformance"></a>
## <a name="slow-performance-on-an-azure-file-share-mounted-on-a-linux-vm"></a>A Linux virtuális gép csatlakoztatott lassú teljesítmény az Azure fájlmegosztások

### <a name="cause"></a>Ok

Egy lehetséges oka a lassú teljesítmény le van tiltva, gyorsítótárazás.

### <a name="solution"></a>Megoldás

toocheck gyorsítótárazás le van tiltva, hogy keressen hello **gyorsítótár =** bejegyzés. 

**Gyorsítótár = none** azt jelzi, hogy gyorsítótárazás le van tiltva.  Újracsatlakoztatást hello megosztás hello alapértelmezett csatlakoztatási parancs használatával, vagy explicit módon adja hozzá a hello **gyorsítótár = szigorú** beállítás toohello csatlakoztatási parancs tooensure, amely az alapértelmezett gyorsítótárazási vagy "strict" gyorsítótárazási mód engedélyezve van.

Bizonyos esetekben hello **serverino** csatlakoztatási beállítás hatására a hello **ls** parancs toorun stat minden könyvtárbejegyzés ellen. Ezt a viselkedést eredményezi teljesítménycsökkenés nagy könyvtár listázásakor van. Hello csatlakoztatási lehetőségek ellenőrizheti a **/etc/fstab** bejegyzést:

`//azureuser.file.core.windows.net/cifs /cifs cifs vers=3.0,serverino,username=xxx,password=xxx,dir_mode=0777,file_mode=0777`

Azt is ellenőrizheti, hogy használják-e a megfelelő hello-beállítások hello futtatásával **sudo csatlakoztatási |} grep cifs** parancs és a kimenetet, például a következő egy példa a kimenetre hello ellenőrzése:

`//mabiccacifs.file.core.windows.net/cifs on /cifs type cifs (rw,relatime,vers=3.0,sec=ntlmssp,cache=strict,username=xxx,domain=X,uid=0,noforceuid,gid=0,noforcegid,addr=192.168.10.1,file_mode=0777, dir_mode=0777,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,actimeo=1)`

Ha hello **gyorsítótár = szigorú** vagy **serverino** elem nem létezik, válassza le a lemezképet, és csatlakoztassa Azure File storage újra hello hello csatlakoztatási parancs futtatásával [dokumentáció](../storage-how-to-use-files-linux.md). Ezt követően az adott hello újbóli **/etc/fstab** bejegyzés rendelkezik hello megfelelő beállításokat.

<a id="timestampslost"></a>
## <a name="time-stamps-were-lost-in-copying-files-from-windows-toolinux"></a>A fájlok másolását a Windows tooLinux időbélyegeket elvesztek

Linux/Unix platformokon hello **cp -p** parancs sikertelen lesz, ha a különböző felhasználók 1 és 2 fájlt a tulajdonosa.

### <a name="cause"></a>Ok

hello force jelzést **f** a COPYFILE eredményezi végrehajtása **cp -p -f** UNIX. Ez a parancs is sikertelen toopreserve hello időbélyegzőjét hello fájl, amely nem Ön a tulajdonosa.

### <a name="workaround"></a>Megkerülő megoldás

Hello tárolási fiók felhasználói hello fájlok másolása használhatja:

- `Useadd : [storage account name]`
- `Passwd [storage account name]`
- `Su [storage account name]`
- `Cp -p filename.txt /share`

## <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.

Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget gyorsan oldja meg a problémát.
