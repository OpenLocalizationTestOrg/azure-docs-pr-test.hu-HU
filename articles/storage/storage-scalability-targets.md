---
title: "aaaAzure tárolási méretezhetőség és teljesítmény célok |} Microsoft Docs"
description: "További információk a hello méretezhetőségi és Teljesítménycélok az Azure Storage, beleértve a kapacitás, a lekérdezési gyakorisága és a bejövő és kimenő sávszélesség mindkét standard és prémium szintű storage-fiókok. Ismerje meg, teljesítménycéloknak partíciók hello Azure Storage szolgáltatások belül."
services: storage
documentationcenter: na
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: be721bd3-159f-40a1-88c1-96418537fe75
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 07/12/2017
ms.author: robinsh
ms.openlocfilehash: 98de116a01b64f3418808a5f626b6c70d8d432e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-scalability-and-performance-targets"></a>Az Azure Storage méretezhetőségi és teljesítménycéljai
## <a name="overview"></a>Áttekintés
Ez a témakör ismerteti a hello méretezhetőség és teljesítmény témakörök a Microsoft Azure Storage. Egyéb Azure korlátozását összefoglalását lásd: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md).

> [!NOTE]
> Az összes tárfiók hello új egyszerű hálózati topológia futtatnak, és attól függetlenül, létrehozásuk időpontja, az alábbiakban leírt hello méretezhetőségi és Teljesítménycélok támogatja. További információ a hello Azure Storage egyszerű hálózati architektúra és a méretezhetőség: [Microsoft Azure Storage: A magas rendelkezésre álló felhőalapú tárolási szolgáltatásba az erős konzisztencia](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).
> 
> [!IMPORTANT]
> az itt felsorolt hello méretezhetőségi és Teljesítménycélok csúcskategóriás célozza, de elérhető. Minden esetben hello kérelem sebesség és a tárfiók megvalósítani sávszélesség függ, tárolt objektumok hello mérete hello hozzáférési minták funkcióját, és hello Terheléstípus az alkalmazás hajt végre. Lehet, hogy tootest a szolgáltatás toodetermine hogy teljesítménye megfelel-e a követelményeknek. Ha lehetséges elkerülése érdekében a forgalom hello aránya hirtelen teljesítményt, és győződjön meg arról, hogy forgalom jól elosztott partíciók között.
> 
> Elérésekor az alkalmazás milyen partíció kezelni tud a munkaterheléshez hello legfeljebb, Azure Storage megkezdődik tooreturn hibakód 503-as (kiszolgáló elfoglalt), vagy által visszaadott hibakód: 500 (a művelet időkorlátja lejár) válaszokat. Ha ez történik, hello alkalmazás újrapróbálkozások egy exponenciális leállítási házirendet kell használni. hello exponenciális leállítási hello terhelése hello partíció toodecrease és tooease igényeiben jelentkező kimenő forgalom toothat partíció lehetővé teszi.
> 
> 

Ha hello kell az alkalmazás ennél nagyobb hello méretezhetőségi célok egyetlen tárfiók, létrehozhatja az alkalmazás toouse több tárfiókot, és az adatok objektumok partícióazonosító adott tárfiókok között. Lásd: [Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/) a mennyiségi díjszabásról.

## <a name="scalability-targets-for-blobs-queues-tables-and-files"></a>Méretezhetőségi célok BLOB, üzenetsorok, táblák és fájlok
[!INCLUDE [azure-storage-limits](../../includes/azure-storage-limits.md)]

<!-- conceptual info about disk limits -- applies toounmanaged and managed -->
## <a name="scalability-targets-for-virtual-machine-disks"></a>A virtuális gép lemezeinek vonatkozó méretezhetőségi célok
[!INCLUDE [azure-storage-limits-vm-disks](../../includes/azure-storage-limits-vm-disks.md)]

Lásd: [Windows Virtuálisgép-méretek](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) vagy [Linux Virtuálisgép-méretek](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) további részleteket.

## <a name="managed-virtual-machine-disks"></a>Felügyelt virtuális gépek lemezei

[!INCLUDE [azure-storage-limits-vm-disks-managed](../../includes/azure-storage-limits-vm-disks-managed.md)]

## <a name="unmanaged-virtual-machine-disks"></a>Nem felügyelt virtuális gépek lemezei
[!INCLUDE [azure-storage-limits-vm-disks-standard](../../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../../includes/azure-storage-limits-vm-disks-premium.md)]

## <a name="scalability-targets-for-azure-resource-manager"></a>Az Azure resource manager méretezhetőségi célok
[!INCLUDE [azure-storage-limits-azure-resource-manager](../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="partitions-in-azure-storage"></a>Az Azure Storage a partíciók
Minden objektum, amely tárolja az Azure Storage (BLOB, üzenetek, entitásokat és fájlok) tárolt adatok tooa partíció tartozik, és a partíciókulcs azonosíthatók. hello partíció határozza meg, hogyan Azure Storage egyenlegének betöltése blobokat, üzenetek, entitásokat és fájlok között kiszolgálók toomeet hello forgalmi igényeinek azokat az objektumokat. hello partíciós kulcs egyedi és használt toolocate egy blob, üzenetet vagy entitás.

a fent látható hello tábla [méretezhetőségi célok Storage-fiókok](#standard-storage-accounts) listák az egyes szolgáltatások egyes partícióinak teljesítménycéloknak hello.

Partíciók terheléselosztás és a méretezhetőség hatással vannak az egyes hello tárolószolgáltatásokhoz hello a következő módokon:

* **Blobok**: hello partíció egy blob kulcsa fiók nevét, a tároló neve + a blob neve. Ez azt jelenti, hogy a minden egyes blob saját partícióval rendelkezhet, ha hello blob terhelése megköveteli azt. Blobok terjeszthető rendelés tooscale kimenő hozzáférést toothem található kiszolgálókra, de egyetlen blob csak egyetlen kiszolgáló szolgálhatók ki. Blobok logikailag csoportosíthatók blobtárolók, amíg nincsenek nem ez a csoportosítás a particionálási megvalósítását.
* **Fájlok**: hello partíció fájl kulcsa fiók neve + fájl megosztási név. Ez azt jelenti, hogy minden fájl a fájlmegosztásokon is egyetlen partícióra vannak.
* **Üzenetek**: hello partíciós kulcs üzenet az hello fióknév + várólista neve, így az összes üzenetet a várólistában egyetlen partícióra vannak csoportosítva, és egy önálló kiszolgáló által kiszolgált. Különböző várólisták feldolgozása történhet toobalance hello azonban sok várólisták a tárfiók lehet betölteni a különböző kiszolgálókon.
* **Entitások**: hello partíció egy entitás kulcsa fióknév + táblanév + partíciós kulcs, ahol hello partíciós kulcs értéke hello hello szükséges felhasználói **PartitionKey** hello entitás tulajdonság. Egyazon partíciókulcs-értékkel azonos particionálása és által rendelkezésre hello azonos hello vannak csoportosítva hello rendelkező összes entitás partíció kiszolgáló. Ez az egy fontos pont toounderstand az alkalmazás kialakításában. Az alkalmazás kell terheléselosztást hello méretezhetőség előnyei entitások hello data access előnyei csoportosítása egyetlen partícióra entitások több partíción keresztül terjednek.  

A legfontosabb előnye toogrouping az entitások egy tábla egy egyetlen partícióra készletének, hogy lehetséges tooperform atomi kötegműveletek keresztül azonos particionálása, mert egyetlen kiszolgálón létezik partíció hello szerepelnek. Ezért ha tooperform kötegműveletek entitások csoportján, fontolja meg a hello csoportosíthatja őket ugyanazzal a partíciókulccsal. 

A hello ugyanakkor, azonos táblában, de rendelkezik a különböző partíciókulcsúak hello lévő entitások lehet terhelésű történik a különböző kiszolgálókon, így lehetséges toohave nagyobb méretezhetőségét.

Részletes particionálási stratégia, a táblák találhatók tervezésével kapcsolatos ajánlások [Itt](https://msdn.microsoft.com/library/azure/hh508997.aspx).

## <a name="see-also"></a>Lásd még:
* [Tárolás díjszabása](https://azure.microsoft.com/pricing/details/storage/)
* [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md)
* [Premium Storage: Nagy teljesítményű tárolási szolgáltatás Azure-beli virtuális gépek számítási feladataihoz](storage-premium-storage.md)
* [Az Azure Storage replikáció](storage-redundancy.md)
* [A Microsoft Azure tárolási teljesítmény és méretezhetőség ellenőrzőlista](storage-performance-checklist.md)
* [A Microsoft Azure Storage: Egy magas rendelkezésre állású felhőalapú tárolási szolgáltatásba erős konzisztencia](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

