---
title: "Az Azure Storage méretezhetőségi és teljesítménycéloknak |} Microsoft Docs"
description: "További tudnivalók a méretezhetőségi és Teljesítménycélok az Azure Storage, beleértve a kapacitás, a lekérdezési gyakorisága és a bejövő és kimenő sávszélesség mindkét standard és prémium szintű storage-fiókok. Ismerje meg az Azure Storage szolgáltatások belül partíciók teljesítménycéloknak."
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
ms.openlocfilehash: ed90e5d63e4c93f9c5054b02d2b4457b44caf6eb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-storage-scalability-and-performance-targets"></a>Az Azure Storage méretezhetőségi és teljesítménycéljai
## <a name="overview"></a>Áttekintés
Ez a témakör ismerteti a méretezhetőség és teljesítmény témakörök a Microsoft Azure Storage. Egyéb Azure korlátozását összefoglalását lásd: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md).

> [!NOTE]
> Az összes tárfiók futtassa az új egyszerű hálózati topológia és a méretezhetőségi és Teljesítménycélok, függetlenül attól, létrehozásuk időpontja, az alábbiakban leírt támogatja. További információ az Azure Storage egyszerű hálózati architektúra és a méretezhetőség: [Microsoft Azure Storage: A magas rendelkezésre álló felhőalapú tárolási szolgáltatásba az erős konzisztencia](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).
> 
> [!IMPORTANT]
> Az itt felsorolt méretezhetőségi és Teljesítménycélok csúcskategóriás célozza, de elérhető. Minden olyan esetben, a lekérdezési gyakorisága és a sávszélesség érhető el a tárolási fiók attól függ, hogy az objektumok tárolja, a hozzáférési minták funkcióját, mérete, és milyen típusú az alkalmazás hajt végre. Mindenképpen ellenőrizze a szolgáltatást annak meghatározásához, hogy a teljesítményét, megfelel-e a követelményeknek. Ha lehetséges elkerülése érdekében a forgalom arányát hirtelen teljesítményt, és győződjön meg arról, hogy forgalom jól elosztott partíciók között.
> 
> Ha az alkalmazás eléri a korlátot, mi partíció kezelni tud a munkaterhelés számára, Azure Storage megkezdik 503-as (kiszolgáló elfoglalt) vagy a hiba 500 (a művelet időkorlátja lejár) válaszok adja vissza. Ha ez történik, az alkalmazás újrapróbálkozások egy exponenciális leállítási házirendet kell használni. Az exponenciális leállítási lehetővé teszi a terhelés a partíción, és kimenő forgalmának kiszolgálására adott partíció igényeiben jelentkező megkönnyítése érdekében.
> 
> 

Ha az alkalmazás igényeinek túllépi a méretezhetőségi célok egyetlen tárfiók, építenie az alkalmazást több tárfiókot használni, és az adatok objektumok partícióazonosító adott tárfiókok között. Lásd: [Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/) a mennyiségi díjszabásról.

## <a name="scalability-targets-for-blobs-queues-tables-and-files"></a>Méretezhetőségi célok BLOB, üzenetsorok, táblák és fájlok
[!INCLUDE [azure-storage-limits](../../includes/azure-storage-limits.md)]

<!-- conceptual info about disk limits -- applies to unmanaged and managed -->
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
Minden objektum, amely tárolja az Azure Storage (BLOB, üzenetek, entitásokat és fájlok) tárolt adatok egy partíció tartozik, és a partíciókulcs azonosíthatók. A partíció határozza meg, hogyan Azure Storage egyenlegének betöltése blobokat, üzenetek, entitásokat és fájlok azokat az objektumokat a forgalom igényeinek kiszolgáló között. A partíciós kulcs egyedi, és egy blob, üzenetet vagy entitás kereséséhez használható.

A fenti táblázatban [méretezhetőségi célok Storage-fiókok](#standard-storage-accounts) a teljesítménycéloknak, minden egyes szolgáltatás egyes partícióinak sorolja fel.

Partíciók hatással terheléselosztás és a méretezhetőség minden, a tárolási szolgáltatások az alábbi módokon:

* **Blobok**: A partíció egy blob kulcsa fiók nevét, a tároló neve + a blob neve. Ez azt jelenti, hogy a minden egyes blob saját partícióval rendelkezhet, ha a blob terhelése megköveteli azt. Blobok terjeszthető környezetekben, sok kiszolgálón ahhoz, hogy terjessze ki őket a hozzáférést, de egyetlen blob csak egyetlen kiszolgáló szolgálhatók ki. Blobok logikailag csoportosíthatók blobtárolók, amíg nincsenek nem ez a csoportosítás a particionálási megvalósítását.
* **Fájlok**: A fájl partíciós kulcsa fiók neve + fájl megosztási név. Ez azt jelenti, hogy minden fájl a fájlmegosztásokon is egyetlen partícióra vannak.
* **Üzenetek**: üzenet a partíciós kulcs az a fiók neve + várólista neve, az összes üzenetet a várólistában egyetlen partícióra vannak csoportosítva, és egyetlen kiszolgáló által szolgáltatott. Különböző várólisták feldolgozása történhet terhelését a sok várólisták a tárfiók lehet azonban különböző kiszolgálókon.
* **Entitások**: A partíciós kulcs az entitás neve + táblanév fiók + partícióazonosító kulcs, ahol a partíciós kulcs értéke a szükséges felhasználói **PartitionKey** tulajdonság az entitás. Az egyazon partíciókulcs-értékkel rendelkező összes entitás partícióra vannak csoportosítva, és az azonos partíció-kiszolgáló által kiszolgált. Ez az alkalmazás tervezésének megértése fontos megfontolandó. Az alkalmazás kell terheléselosztást a méretezhetőség előnyeit entitások data access előnyeit csoportosítása egyetlen partícióra entitások több partíción keresztül terjednek.  

A kulcs az egy tábla entitások készletének csoportosítása egyetlen partícióra előnye, hogy lehet, mivel egy kiszolgálón létezik partíció atomi kötegelt műveletek végrehajtásához között szerepelnek a partícióra. Ezért ha szeretne egy entitások csoportján kötegelt műveleteket, fontolja meg az azonos partíciókulcsú csoportosíthatja őket. 

Másrészről, szervezetek, amelyek ugyanazon táblában, de rendelkezik a különböző partíciókulcsúak is lehet az elosztott terhelésű történik a különböző kiszolgálókon, így lehetséges, hogy nagyobb méretezhetőségét.

Részletes particionálási stratégia, a táblák találhatók tervezésével kapcsolatos ajánlások [Itt](https://msdn.microsoft.com/library/azure/hh508997.aspx).

## <a name="see-also"></a>Lásd még:
* [Tárolás díjszabása](https://azure.microsoft.com/pricing/details/storage/)
* [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md)
* [Premium Storage: Nagy teljesítményű tárolási szolgáltatás Azure-beli virtuális gépek számítási feladataihoz](storage-premium-storage.md)
* [Az Azure Storage replikáció](storage-redundancy.md)
* [A Microsoft Azure tárolási teljesítmény és méretezhetőség ellenőrzőlista](storage-performance-checklist.md)
* [A Microsoft Azure Storage: Egy magas rendelkezésre állású felhőalapú tárolási szolgáltatásba erős konzisztencia](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

