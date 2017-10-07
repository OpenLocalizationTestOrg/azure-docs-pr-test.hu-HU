---
title: "prémium szintű Storage aaaHigh-teljesítmény és az Azure által kezelt lemezeken virtuális gépek |} Microsoft Docs"
description: "További tudnivalók a prémium szintű Storage nagy teljesítményű és felügyelt lemezek Azure virtuális gépekhez. Az Azure DS-méretek, DSv2-méretek, GS sorozatnak, és támogatja a prémium szintű Storage Fs sorozatú virtuális gépeket."
services: storage
documentationcenter: 
author: ramankumarlive
manager: aungoo-msft
editor: tysonn
ms.assetid: e2a20625-6224-4187-8401-abadc8f1de91
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: ramankum
ms.openlocfilehash: 2474fa75116fe394672fde48520441fa80cf434f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="high-performance-premium-storage-and-managed-disks-for-vms"></a>Prémium szintű Storage nagy teljesítményű és a virtuális gépek felügyelt lemezek
Prémium szintű Storage nagy teljesítményű, alacsony késésű támogatása a virtuális gépek (VM) továbbítja a bemeneti/kimeneti (I/O)-igényes munkaterhelések. Prémium szintű Storage használó Virtuálisgép-lemezek SSD-meghajtót (SSD) adatait tárolják. tootake előny hello sebesség és a prémium szintű storage lemezek teljesítményét, áttelepítheti a meglévő virtuális gép lemezek tooPremium tároló.

Az Azure számos prémium szintű storage lemezek tooa VM csatolhat. Több lemez használatával biztosít az alkalmazások folyamatos too256 TB tárhelyet virtuális gépenként. Prémium szintű Storage az alkalmazások 80000 másodpercenkénti I/O műveletek (IOPS) virtuális gépenként, és a lemez átviteli be too2, 000 megabájt / másodperc (MB/s) virtuális gépenként érhető el. Olvasási műveletek nagyon kis késleltetést biztosítanak.

Prémium szintű Storage Azure felajánlja hello tootruly növekedési és shift kibővített vállalati alkalmazások, például Dynamics AX, a Dynamics CRM-hez, a Exchange Server, a SAP Business Suite és a SharePoint farmok toohello felhő. Teljesítmény-igényes adatbázis-terhelések alkalmazások, például az SQL Server, Oracle, MongoDB, MySQL és Redis, egységes magas teljesítménye és kis késleltetése igénylő futtathat.

> [!NOTE]
> Hello az alkalmazás legjobb teljesítmény érdekében azt javasoljuk, hogy az áttelepített bármely magas IOPS tooPremium tárolási igénylő méretű lemezt. Ha a lemez nem igényli a magas iops értéket, lekapcsolva tartja szabványos Azure Storage segítségével korlát költségek. Standard szintű tárolást, a Virtuálisgép-lemez adatainak (merevlemezes HDD) meghajtók helyett SSD meghajtókon tárolják.
> 

Azure kétféleképpen toocreate prémium tárolólemezek virtuális gépek esetén:

* **Nem felügyelt lemezek**

    hello eredeti mód a nem felügyelt toouse lemezek. Egy nem felügyelt lemezt a hello toostore hello virtuális merevlemez (VHD) fájlok, amelyek megfelelnek a tooyour virtuális gép lemezeivel használható storage-fiókok kezelése. VHD-fájlokat, az Azure storage-fiókok lapblobokat tárolódnak. 

* **Felügyelt lemezek**

    Ha úgy dönt, [Azure felügyelt lemezek](storage-managed-disks-overview.md), Azure kezeli hello storage-fiókok, amelyek a virtuális gép lemezeit használja. Megadhatja, hogy hello lemeztípus (prémium és Standard) és a szükséges hello lemez hello méretét. Azure hoz létre, és kezeli a hello lemez meg. Nincs több, a tárfiókok méretezhetőségének korlátai belül maradnak tárolási fiókok tooensure hello lemezek helyezésével kapcsolatos tooworry. Azure kezeli, amely meg.

Azt javasoljuk, hogy ezt a módot felügyelt lemezek, a sok szolgáltatásainak tootake előnyeit.

prémium szintű Storage, használatába tooget [az ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/). 

A meglévő virtuális gépek tooPremium tárolási áttelepítéssel kapcsolatos információkért lásd: [egy Windows virtuális gép átalakítása nem felügyelt lemezek toomanaged lemezekből](../virtual-machines/virtual-machines-windows-convert-unmanaged-to-managed-disks.md) vagy [Linux virtuális gép átalakítása nem felügyelt lemezek toomanaged lemezekből](../virtual-machines/linux/convert-unmanaged-to-managed-disks.md).

> [!NOTE]
> Prémium szintű Storage a legtöbb régióban érhető el. Hello elérhető régiók listáját a [Azure-szolgáltatások régiónként](https://azure.microsoft.com/regions/#services), nézze meg, amelyben hello régiókban támogatott prémium szintű támogatási mérete sorozatú virtuális gépek (DS-méretek, DSV2-méretek, GS sorozatnak és Fs sorozatú virtuális gépek) támogatottak.
> 

## <a name="features"></a>Szolgáltatások

Az alábbiakban néhány hello szolgáltatást a prémium szintű Storage:

* **Prémium szintű storage-lemezekhez**

    Prémium szintű Storage támogatja virtuális gépek lemezei használható csatolt toospecific mérete sorozatú virtuális gépeket. Prémium szintű Storage támogatja a DS-méretek, DSv2-méretek, GS-méretek, Ls-sorozat és Fs sorozatú virtuális gépeket. Megválaszthatja, a hét lemezméret: P4 (32GB), P6 (64 GB-os), P10 (128GB), P20 (512GB), P30 (1024GB), P40 (2048GB), P50 (4095GB). P4 és P6 lemezméret csak még támogatottak felügyelt lemezek. Minden lemez mérete a saját teljesítmény specifikációi. Az alkalmazás követelményeitől függően egy vagy több lemezt tooyour VM csatolhat. Azt írják le részletesebben hello specifikációk [prémium szintű Storage méretezhetőségi és Teljesítménycélok](#scalability-and-performance-targets).

* **Prémium szintű lapblobokat**

    Prémium szintű Storage támogatja a lapblobokat. Prémium szintű Storage a virtuális gépek lap blobok toostore állandó, nem felügyelt lemezek használhatók. Eltérően Azure standard szintű tárolást prémium szintű Storage nem támogatja a blokkblobokat, hozzáfűző blobokat, fájlok, táblák vagy várólisták. Prémium szintű lapblobokat támogatja P10 tooP50 és P60 hat méretek (8191GiB). Az oldalakra vonatkozó blob P60 prémium nincs támogatott toobe VM lemezként csatolt. 

    Minden objektum, a prémium szintű tárfiók helyezett oldalakra vonatkozó blob lesz. az oldalakra vonatkozó blob hello illesztése tooone a hello támogatott kiosztott méretét. Ezért a prémium szintű tárfiók nem készült toobe használt toostore apró blobokat.

* **Prémium szintű storage-fiók**

    toostart prémium szintű Storage használatával hozzon létre egy prémium szintű tárfiók nem felügyelt lemezek. A hello [Azure-portálon](https://portal.azure.com), a prémium szintű tárfiók toocreate válasszon hello **prémium** teljesítményszinttel. Jelölje be hello **helyileg redundáns tárolás (LRS)** replikációs beállítás. Is létrehozhatja a prémium szintű tárfiók hello beállítástípus túl**Premium_LRS** a hello alábbi helyek egyikét:
    * [Storage REST API felülete](https://docs.microsoft.com/rest/api/storageservices/Azure-Storage-Services-REST-API-Reference) (2014-02-14-es vagy újabb verziója)
    * [Szolgáltatásfelügyelet REST API](http://msdn.microsoft.com/library/azure/ee460799.aspx) (2014-10-01 verzió vagy újabb verzióra, az Azure klasszikus üzembe helyezés)
    * [Az Azure Storage erőforrás szolgáltató REST API](https://docs.microsoft.com/rest/api/storagerp) (az Azure Resource Manager üzembe helyezések)
    * [Az Azure PowerShell](../powershell-install-configure.md) (0.8.10 verzió vagy újabb)

    információ a prémium szintű tárfiókok korlátairól, toolearn lásd [prémium szintű Storage méretezhetőségi és Teljesítménycélok](#premium-storage-scalability-and-performance-targets).

* **Prémium szintű helyileg redundáns tárolás**

    A prémium szintű tárfiók csak a helyileg redundáns tárolás hello replikációs beállításként támogatja. Helyileg redundáns tárolás három másolatot hello adatok tartja egyetlen régión belül. Regionális katasztrófa utáni helyreállítás, biztonsági másolatot kell készíteni a virtuális gép lemezeit, egy másik régióban használatával [Azure biztonsági mentés](../backup/backup-introduction-to-azure-backup.md). A georedundáns tárolás (GRS) fiókot és hello biztonsági mentési tárolónak kell használnia. 

    Azure a tárfiók tárolóként a nem kezelt lemezeken használ. Ha hoz létre egy Azure DS-méretek, DSv2-méretek, GS-méretek, vagy Fs sorozatú virtuális gép, és nem felügyelt lemezeket, és válassza ki a prémium szintű storage-fiók, az operációs rendszer, és adatlemezek vannak tárolva, hogy a tárfiók.

## <a name="supported-vms"></a>Támogatott virtuális gépek
Prémium szintű Storage támogatja a DS-méretek, DSv2-méretek, GS-méretek, Ls-sorozat és Fs sorozatú virtuális gépeket. Az ilyen méretű standard és prémium szintű storage lemezek is használhatók. Nem használhat a prémium szintű storage lemezekhez Virtuálisgép-sorozat, amelyek nem prémium szintű Storage-kompatibilis.

További információ a Virtuálisgép-típusokon és a Windows Azure-méretek: [Windows Virtuálisgép-méretek](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Linux virtuális gép típusát és méretét, az Azure-ban kapcsolatos információkért lásd: [Linux Virtuálisgép-méretek](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Ezek közé tartoznak a hello DS-méretek, DSv2-méretek, GS-méretek, hello funkcióit Ls-sorozat és Fs sorozatú virtuális gépek:

* **A felhőalapú szolgáltatás**

    DS sorozatnak virtuális gépek tooa felhőalapú szolgáltatást csak a DS sorozatnak virtuális gépeket adhat hozzá. Ne adjon hozzá a DS sorozatnak virtuális gépek tooan meglévő felhőalapú szolgáltatást típustól eltérő, DS sorozatnak virtuális gépeket. Áttelepítheti a meglévő VHD-k tooa új felhőalapú szolgáltatás, amely csak a DS sorozatnak virtuális gépeken futtatja. Ha azt szeretné, toouse hello hello új felhőalapú szolgáltatás, amelyen a DS sorozatnak virtuális gépekre, használja az ugyanazon virtuális IP-cím [fenntartott IP-cím](../virtual-network/virtual-networks-instance-level-public-ip.md). GS sorozatnak virtuális gépek tooan meglévő felhőalapú szolgáltatást csak a GS sorozatnak virtuális gépeket lehet hozzáadni.

* **Operációsrendszer-lemez**

    A prémium szintű Storage VM toouse beállíthat egy prémium szintű vagy egy szabványos operációsrendszer-lemez. Hello legjobb élmény érdekében azt javasoljuk, a prémium szintű Storage-alapú operációs rendszer tárolására.

* **Adatlemezek**

    Prémium szintű használható, és a standard lemezek hello azonos prémium szintű Storage virtuális gép. Prémium szintű Storage a virtuális gép kiépítése, és több állandó adatok lemezek toohello VM csatolni. Ha szükséges, tooincrease hello kapacitást és teljesítményt hello kötet, akkor a lemezeken is paritásos.

    > [!NOTE]
    > Ha a prémium szintű storage adatlemezek meg paritásos [tárolóhelyek](http://technet.microsoft.com/library/hh831739.aspx), állítsa be a tárolóhelyek az egyes lemezek használt 1 oszlop. Ellenkező esetben a teljes csíkozott hello kötet teljesítménye lehet kisebb, mint a várt forgalom egyenetlen eloszlását miatt hello lemezeken. Alapértelmezés szerint a Kiszolgálókezelőben állíthat be az oszlopok too8 lemezeket. Ha több mint 8 lemez, használja a PowerShell toocreate hello kötet. Adja meg manuálisan hello az oszlopok számát. Ellenkező esetben hello Server kezelő felhasználói felületén továbbra is toouse 8 oszlopok, még akkor is, ha több lemezt rendelkezik. Ha egy egyetlen paritásos készlet 32 lemezre, például 32 oszlopok megadása. oszlopok száma toospecify hello hello virtuális lemezt használ, a hello [New-VirtualDisk](http://technet.microsoft.com/library/hh848643.aspx) PowerShell-parancsmag használatát hello *numberofcolumns tulajdonsághoz* paraméter. További információkért lásd: [tárolóhelyek – áttekintés](http://technet.microsoft.com/library/hh831739.aspx) és [tárolási tárolóhelyek – gyakori kérdések](http://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx).
    >
    > 

* **Gyorsítótár**

    Virtuális gépek hello mérete sorozat, amely támogatja a prémium szintű Storage egyedi gyorsítótárazási lehetővé teszi a magas szintű teljesítményt és a késés van. gyorsítótárazás funkció hello meghaladja a mögöttes prémium szintű tároló lemez teljesítménye. Beállíthatja, hogy túl gyorsítótárazási házirend prémium szintű storage lemezeken hello lemez**ReadOnly**, **ReadWrite**, vagy **nincs**. hello alapértelmezett lemez gyorsítótárazási házirend **ReadOnly** minden prémium adatlemezekhez és **ReadWrite** operációsrendszer-lemezek. Az alkalmazás optimális teljesítmény érdekében használjon hello javítsa ki a gyorsítótár-beállítást. Az olvasási műveleteket, vagy csak olvasható adatlemezek, például az SQL Server-adatfájlok, állítsa például hello lemez gyorsítótárazási házirend túl**ReadOnly**. Írási műveleteket vagy a csak írható adatlemezek, például az SQL Server naplófájlokat, állítsa be a gyorsítótárazási házirend túl hello lemez**nincs**. További információ az optimalizálás, prémium szintű Storage, a tervezési toolearn lásd: [terv teljesítmény a prémium szintű Storage](storage-premium-storage-performance.md).

* **Elemzés**

    tooanalyze VM teljesítmény lemezek használatával a prémium szintű Storage, kapcsolja be a Virtuálisgép-diagnosztika a hello [Azure-portálon](https://portal.azure.com). További információkért lásd: [Azure Diagnostics kiterjesztésű monitoring Azure virtuális gép](https://azure.microsoft.com/blog/2014/09/02/windows-azure-virtual-machine-monitoring-with-wad-extension/). 

    toosee lemez teljesítménye, operációs rendszer-alapú eszközök, például [Windows Teljesítményfigyelő](https://technet.microsoft.com/library/cc749249.aspx) Windows virtuális gépek és hello [iostat](http://linux.die.net/man/1/iostat) Linux virtuális gépek parancsot.

* **VM skálázási korlátai és teljesítmény**

    Minden prémium szintű Storage-támogatott Virtuálisgép-méretet méretkorlátai és teljesítmény specifikációk IOPS, sávszélesség és hello számát virtuális gépenként csatolható lemezek rendelkezik. Prémium szintű storage lemezek virtuális gépek használatakor győződjön meg arról, hogy nincs-e elegendő iops-érték és a virtuális gép toodrive lemez forgalom sávszélességét.

    Például a STANDARD_DS1 virtuális gépek sávszélessége dedikált 32 MB/s a prémium szintű tároló lemez forgalmához. Egy P10 prémium szintű tároló lemez a 100 MB/s sávszélességet biztosít. Ha egy P10 prémium szintű tároló lemez csatolt toothis VM, azt csak lépjen be too32 MB/s. Legfeljebb 100 MB/s hello P10 lemezt biztosíthat hello nem használható.

    Hello a DS sorozatnak hello legnagyobb VM jelenleg hello Standard_DS15_v2. hello Standard_DS15_v2 összes lemezt fel too960 MB/s is biztosít. hello legnagyobb virtuális gép a GS sorozatnak hello hello Standard_GS5. hello Standard_GS5 biztosíthat mentése too2, 000 MB/s között az összes lemez.

    Vegye figyelembe, hogy ezek a korlátozások csak a lemez forgalom. Ezek a korlátozások nem tartalmaznak, találatot eredményező gyorsítótárbeli kereséseinek és a hálózati forgalom. Egy külön sávszélesség érhető el virtuális gépek hálózati forgalmához. A hálózati forgalom a sávszélesség eltér a prémium szintű storage lemezekhez által használt dedikált hello sávszélesség.

    Hello legfrissebb információk maximális IOPS és átviteli sebesség (sávszélesség) prémium szintű Storage által támogatott virtuális gépek: [Windows Virtuálisgép-méretek](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) vagy [Linux Virtuálisgép-méretek](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

    További információ a prémium szintű storage lemezek és az iops-érték és az átviteli Sebességkorlát hello táblázatban hello a következő szakaszban találja.

## <a name="scalability-and-performance-targets"></a>Méretezhetőségi és teljesítménycélok
Ez a szakasz azt ismerteti hello méretezhetőség és teljesítmény célok tooconsider prémium szintű Storage használatakor.

Prémium szintű storage-fiókok a következő méretezhetőségi célok hello rendelkezik:

| Teljes kapacitásával | A helyileg redundáns tárolás fiók teljes sávszélesség |
| --- | --- | 
| Lemez kapacitás: 35 TB <br>Pillanatkép-kapacitás: 10 TB | Másolatot too50 Gigabit / másodperc, a bejövő<sup>1</sup> + kimenő<sup>2</sup> |

<sup>1</sup> tooa tárfiók küldött minden adat (kérelmek)

<sup>2</sup> tárfiók érkező összes adat (válasz)

További információkért lásd: [Azure Storage méretezhetőségi és Teljesítménycélok](storage-scalability-targets.md).

Ha prémium szintű tárfiókok nem felügyelt lemezek használ, és az alkalmazás meghaladja a hello méretezhetőségi célok egyetlen tárfiók, érdemes lehet toomigrate toomanaged lemezek. Ha nem szeretné toomigrate toomanaged lemezek, build az alkalmazás toouse több tárfiókot. Ezt követően partícióazonosító az adatok adott tárfiókok között. Például ha több virtuális gép között szeretné tooattach 51-TB-os lemezeken, elosztva őket két storage-fiókok. 35 TB hello korlát a egyetlen prémium szintű tárfiók. Győződjön meg arról, hogy egy prémium szintű tárfiók soha nem legfeljebb 35 TB-nyi kiosztott lemezeket.

### <a name="premium-storage-disk-limits"></a>Prémium szintű Tárfiókok lemez korlátai
Rendelkezés a prémium szintű tároló lemez hello hello lemez mérete határozza meg, amikor hello maximális iops-érték és átviteli sebesség (sávszélesség). Az Azure támogatási tárolólemezek hét típusú kínál: P4 (kezelt lemezek csak), P6 (kezelt lemezek csak), P10, P20, P30, P40 és P50. Minden prémium típusú adott korlátai vannak a iops-érték és a teljesítményt. Hello lemeztípusok korlátairól hello a következő táblázat ismerteti:

| Prémium szintű lemezek típusa  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Lemezméret           | 32 GB| 64 GB| 128 GB| 512 GB            | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOPS-érték lemezenként       | 120   | 240   | 500   | 2300              | 5000              | 7500              | 7500              | 
| Adattovábbítás lemezenként | 25 MB / s  | 50 MB / s  | 100 MB / s | 150 MB / s | 200 MB / s | 250 MB / s | 250 MB / s | 

> [!NOTE]
> Ellenőrizze, hogy elegendő sávszélesség érhető el a virtuális gépek toodrive lemez forgalma a [prémium szintű Storage által támogatott virtuális gépek](#premium-storage-supported-vms). Ellenkező esetben a lemez adatátviteli sebessége és IOPS értéke korlátozott toolower értékeket. Maximális átviteli sebesség és IOPS hello VM használati korlátait, nem hello lemez korlátok hello megelőző táblázatban leírtak alapján.  
> 
> 

Az alábbiakban néhány fontos dolgot tooknow prémium szintű Storage méretezhetőségi és Teljesítménycélok kapcsolatban:

* **Kiosztott kapacitást és teljesítményt**

    Amikor egy prémium szintű tároló lemez, Standard szintű tárolást eltérően garantáltan a hello kapacitás, IOPS és átviteli sebessége a lemezen. Például ha P50 lemez létrehozása, Azure kiépítését 4095 GB-os tárolási kapacitás, 7500 IOPS és 250 MB/s átviteli az, hogy a lemez. Az alkalmazás használhat egyetlen hello kapacitást és teljesítményt részét sem.

* **Lemez mérete**

    Az Azure maps hello lemez mérete (kerekítve) toohello legközelebbi prémium szintű tároló lemez lehetőséget, az előző szakaszban hello hello tábla megadott. Például a lemez mérete 100 GB-os besorolásának P10 beállításként. Műveleteket hajthat végre mentése too500 IOPS, az too100 MB/s átviteli be. Hasonlóképpen ekkora lemezt 400 GB egy P20 lesz minősítve. Másolatot too2, 300 IOPS 150 MB/s átviteli sebességgel képes végezni.
    
    > [!NOTE]
    > Könnyedén növelheti a meglévő lemezek hello mérete. Például érdemes 30 GB-os lemezre too128 GB, vagy még akkor is, too1 TB tooincrease hello méretének. Vagy, érdemes lehet tooconvert a P20 tooa P30 lemezről lemezre, mivel szüksége további kapacitást vagy további iops-érték és az átvitel. 
    > 
 
* **I/o-mérete**

    hello egy i/o mérete 256 KB. Ha hello átvitt adatok kisebb, mint 256 KB, ez 1 i/o-egység tekintendő. Nagyobb i/o-méretek számlálási több i/o mérete 256 KB. Például 1100 KB i/o 5 i/o-egységek számítanak.

* **Átviteli sebesség**

    hello átviteli korlát írások toohello lemez tartalmazza, és a lemez olvasási műveletek nem szolgáltatott hello gyorsítótárból tartalmazza. Például egy P10 lemez rendelkezik 100 MB/s átviteli lemezenként. Néhány példa a P10 lemezként érvényes átviteli hello a következő táblázatban láthatók:

    | Maximális átviteli sebesség P10 lemezenként | Nem-gyorsítótár beolvasása a lemezről | Nem-gyorsítótár ír toodisk |
    | --- | --- | --- |
    | 100 MB/s | 100 MB/s | 0 |
    | 100 MB/s | 0 | 100 MB/s |
    | 100 MB/s | 60 MB/s | 40 MB/s |

* **Találatot eredményező gyorsítótárbeli kereséseinek**

    Találatot eredményező gyorsítótárbeli kereséseinek IOPS vagy hello lemez átviteli hello nincs korlátozva. Például azonosítójú adatlemez használatakor egy **ReadOnly** gyorsítótár beállítása a prémium szintű Storage, olvasási hello gyorsítótárból szolgáltatott által támogatott virtuális gépen nincsenek tulajdonos toohello IOPS és átviteli caps hello lemez. Ha egy lemez hello munkaterhelés túlnyomórészt tesz az olvasási és nagyon nagy átviteli kapacitás kaphat. hello gyorsítótár pedig a tulajdonos tooseparate IOPS átviteli sebességének korlátai: hello VM szint alapján hello Virtuálisgép-méretet. DS sorozatnak virtuális gépek körülbelül 4000 iops-érték és a központi gyorsítótár és a helyi SSD i/o-/ 33 MB/s átviteli rendelkezik. GS sorozatnak virtuális gépek legfeljebb 5000 iops teljesítményt és a központi gyorsítótár és a helyi SSD i/o-/ 50 MB/s átviteli rendelkezik. 

## <a name="throttling"></a>Szabályozás
Sávszélesség-szabályozás fordulhatnak elő, ha az alkalmazás IOPS vagy átviteli meghaladja a prémium szintű tároló lemez lefoglalt hello korlátairól. Sávszélesség-szabályozás is okozhat, ha a lemez teljes forgalom között az összes lemezt a virtuális gép hello meghaladja hello lemez sávszélesség korlátja hello VM érhető el. sávszélesség-szabályozás tooavoid, azt javasoljuk, hogy a függőben lévő hello lemez i/o-kérelmek száma hello korlátozza. Használja a határértéket, méretezhetőségi és Teljesítménycélok ellátta hello lemez, és hello lemez sávszélesség érhető el toohello virtuális gép alapján.  

Az alkalmazás érhető el hello legkisebb mértékű késleltetést, amikor arra tervezték, tooavoid sávszélesség-szabályozás. Azonban ha hello függőben lévő hello lemez i/o-kérelmek száma túl kicsi, az alkalmazás nem tudja kihasználni a hello maximális iops-érték és átviteli szintek, amelyek a rendelkezésre álló toohello lemez.

hello a következő példák bemutatják, hogyan toocalculate szabályozás szinttel. Minden számítás egy i/o foglalásiegység-méret 256 KB-os alapulnak.

### <a name="example-1"></a>1. példa
Az alkalmazás egy második P10 lemezen feldolgozott 495 i/o-egység 16 KB-os méret. hello i/o-egységek számítanak 495 iops-érték. Ha 2 MB i/o második hello azonos, i/o-egység hello összesen egyenlő too495 + 8 iops-érték. Ennek az az oka 2 MB i/o = 2048 KB / 256 KB-os = 8 i/o-egység, ha hello i/o-foglalásiegység-méret 256 KB. 495 + 8 hello összege meghaladja a hello 500 IOPS-korláttal hello lemezként, mert a sávszélesség-szabályozás akkor következik be.

### <a name="example-2"></a>2. példa
Az alkalmazás feldolgozott 400 i/o-egység P10 lemezen 256 KB-os méret. hello teljes sávszélességre van (400 &#215; 256) / 1 024 KB = 100 MB/s. Egy P10 lemez van 100 MB/s átviteli sebesség korlátozva. Ha az alkalmazás további i/o-műveletek megpróbál tooperform, hogy a második, mert az nagyobb hello lefoglalt maximális van szabályozva.

### <a name="example-3"></a>3. példa
DS4 virtuális gép csatlakoztatott két P30 lemezzel rendelkezik. Minden egyes P30 lemez képes a 200 MB/s átviteli sebesség. DS4 virtuális gép azonban a teljes sávszélesség lemezkapacitás 256 MB/s van. Mindkét csatlakoztatott lemezek toohello maximális átviteli sebesség: hello DS4 virtuális gép nem meghajtója ugyanannyi időt vesz igénybe. tooresolve, ez is kiálló tárolókkal forgalom 200 MB/s egy lemezen és 56 MB/s a hello más lemez. Ha a lemez forgalom hello összege 256 MB/s keresztül kerül, a lemez forgalom folyamatban van.

> [!NOTE]
> Ha a lemez forgalom legtöbbször a kis i/o-méretek, az alkalmazás valószínűleg elért hello IOPS-korláttal előtt hello átviteli korlátot. Azonban ha hello lemez forgalom leginkább a nagy i/o-méretek, az alkalmazás valószínűleg elért hello átviteli korlát először hello IOPS-korláttal helyett. Optimális i/o-méretek segítségével maximalizálhatja az alkalmazás IOPS és átviteli sebesség. Függőben lévő lemez i/o-kérelmek száma hello is korlátozható.
> 

toolearn a prémium szintű Storage használatával nagy teljesítményű tervezésével kapcsolatos további információkért lásd: [terv teljesítmény a prémium szintű Storage](storage-premium-storage-performance.md).

## <a name="snapshots-and-copy-blob"></a>A pillanatképek és a Blob másolása

toohello társzolgáltatás, hello VHD-fájlt az oldalakra vonatkozó blob. Pillanatképek készítése a lapblobokat, és másolja őket tooanother helyre, például tooa eltérő tárfiók.

### <a name="unmanaged-disks"></a>Nem felügyelt lemezek

Hozzon létre [növekményes pillanatképek](storage-incremental-snapshots.md) a nem felügyelt premium lemezek hello azonos módon teszi lehetővé a pillanatképek standard szintű tárolóval. Prémium szintű Storage hello replikációs beállítás csak a helyileg redundáns tárolás támogatja. Azt javasoljuk, hogy pillanatképeket, és másolja hello pillanatképek tooa georedundáns standard szintű tárfiók. További információkért lásd: [Azure Storage redundancia beállítások](storage-redundancy.md).

Ha a lemez csatlakoztatott tooa VM, néhány hello API műveletek nem engedélyezettek. Például nem hajtható végre egy [másolási Blob](/rest/api/storageservices/Copy-Blob) , hogy a blob, ha hello lemez művelet csatolt tooa virtuális gép. Ehelyett először létre kell hoznia, hogy a blob pillanatképe hello segítségével [pillanatkép Blob](/rest/api/storageservices/Snapshot-Blob) REST API-t. Ezután hajtson végre hello [másolási Blob](/rest/api/storageservices/Copy-Blob) hello pillanatkép toocopy hello a csatlakoztatott lemezen. Másik lehetőségként hello lemezt leválasztani, és végezze el a szükséges műveleteket.

a következő korlátozások hello toopremium tárolási blob pillanatképek vonatkoznak:

| Prémium szintű tárolási kapacitása | Érték |
| --- | --- |
| Egy blob pillanatképek maximális száma | 100 |
| A pillanatképek tárfiókok kapacitásával<br>(Csak a pillanatképek adatokat tartalmazza. Nem tartalmaz adatokat alap BLOB.) | 10 TB |
| Egymást követő pillanatképek közötti minimális időtartam | 10 perc |

a pillanatfelvételek másolatait georedundáns toomaintain, átmásolhatja a pillanatképek a prémium szintű fiók tooa standard georedundáns tárolás tárfiók AzCopy vagy a Blob másolása. További információkért lásd: [adatátvitelt hello AzCopy parancssori segédprogram](storage-use-azcopy.md) és [másolási Blob](/rest/api/storageservices/Copy-Blob).

A prémium szintű storage-fiók ellen lapblobokat REST műveleteinek kapcsolatos részletes információkért lásd: [szolgáltatási műveletek prémium szintű Azure Storage Blob](http://go.microsoft.com/fwlink/?LinkId=521969).

### <a name="managed-disks"></a>Felügyelt lemezek

A felügyelt lemezes pillanatképet hello felügyelt lemezes csak olvasható másolatát. Standard szintű felügyelt lemezes hello pillanatkép tárolja. Jelenleg [növekményes pillanatképek](storage-incremental-snapshots.md) felügyelt lemezek használata nem támogatott. Hogyan tootake felügyelt lemezes, a pillanatkép: toolearn [egy Azure kezelt állapotúként tárolt virtuális merevlemez másolatának létrehozása lemez által felügyelt pillanatképek használata a Windows](../virtual-machines/virtual-machines-windows-snapshot-copy-managed-disk.md) vagy [egy Azure kezelt állapotúként tárolt virtuális merevlemez másolatának létrehozása lemez használatával kezelt a Linux pillanatképek](../virtual-machines/linux/snapshot-copy-managed-disk.md).

Ha egy felügyelt lemezes csatolt tooa VM, néhány hello API műveletek nem engedélyezettek. Például egy közös hozzáférésű jogosultságkód (SAS) tooperform a másolási műveletek nem generálható hello lemez pedig csatolt tooa virtuális gép. Ehelyett először létre kell hoznia egy pillanatkép hello lemez, és hajtsa végre az hello pillanatkép hello példányát. Alternatív megoldásként hello lemezt leválasztani és majd hozzon létre egy SAS tooperform hello másolási műveletet.


## <a name="premium-storage-for-linux-vms"></a>Prémium szintű Storage Linux virtuális gépekhez
A következő információk toohelp beállítása a prémium szintű Storage a Linux virtuális gépek hello használhatja:

tooachieve méretezhetőség prémium szintű Storage, célozza, a prémium szintű storage összes lemez is van gyorsítótár készlet túl**ReadOnly** vagy **nincs**, le kell tiltani "korlátok" hello fájlrendszer csatlakoztatásakor. Ebben a forgatókönyvben korlátok nem szükséges, mert hello írása toopremium tárolólemezek tartós vonatkozó beállítások. Amikor hello írási kérelem sikeresen befejeződik, a adatok toohello állandó tároló került. toodisable "akadályok," hello, a következő módszerek egyikét használhatja. Válassza ki a fájlrendszer egy hello:
  
* A **reiserFS**, toodisable korlátok, használja a hello `barrier=none` csatlakoztatási lehetőséget. (tooenable korlátok, használjon `barrier=flush`.)
* A **ext3/ext4**, toodisable korlátok, használja a hello `barrier=0` csatlakoztatási lehetőséget. (tooenable korlátok, használjon `barrier=1`.)
* A **XFS**, toodisable korlátok, használja a hello `nobarrier` csatlakoztatási lehetőséget. (tooenable korlátok, használjon `barrier`.)
* Prémium szintű storage lemezek túl beállítása gyorsítótárával**ReadWrite**, engedélyezze az írási tartóssága korlátok.
* A kötet címkék toopersist hello VM, újraindítása után frissítenie kell /etc/fstab hello univerzálisan egyedi azonosítóval (UUID) hivatkozások toohello lemezzel. További információkért lásd: [hozzáadása egy felügyelt lemezes tooa Linux virtuális gép](../virtual-machines/linux/add-disk.md).

prémium szintű Azure Storage a következő Linux terjesztésekről hello érvényesítése. A jobb teljesítmény és stabilitását prémium szintű Storage, azt javasoljuk, hogy frissítse a virtuális gépek tooone verzió, minimális (tooa vagy újabb verzió). Legújabb Linux integrációs szolgáltatások (LIS), v4.0, néhány igényelnek hello hello Azure. toodownload és a telepítés egy terjesztési hivatkozásra hello hello a következő táblázatban felsorolt. Jelenleg felvenni toohello rendszerképlistájában, azt végezze el az érvényesítés. Vegye figyelembe, hogy az érvényesítést megjelenítése, hogy a teljesítmény változhat az egyes lemezképek. Teljesítmény a munkaterhelés jellemzőit és a lemezkép beállításainak függ. Különböző képek hangolt különböző típusú munkaterhelések.

| Disztribúció | Verzió | Támogatott kernel | Részletek |
| --- | --- | --- | --- |
| Ubuntu | 12.04 | 3.2.0-75.110+ | Ubuntu-12_04_5-LTS-amd64-Server-20150119-en-us-30GB |
| Ubuntu | 14.04 | 3.13.0-44.73+ | Ubuntu-14_04_1-LTS-amd64-Server-20150123-en-us-30GB |
| Debian | 7.x, 8.x | 3.16.7-ckt4-1+ | &nbsp; |
| SUSE | SLES 12| 3.12.36-38.1+| SUSE-sles-12-prioritás-v20150213 <br> SUSE-sles-12-v20150213 |
| SUSE | SLES 11 SP4 | 3.0.101-0.63.1+ | &nbsp; |
| CoreOS | 584.0.0+| 3.18.4+ | CoreOS 584.0.0 |
| CentOS | 6.5, 6.6, 6.7, 7.0 | &nbsp; | [Szükséges LIS4](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) <br> *Lásd a következő szakaszban hello megjegyzést* |
| CentOS | 7.1+ | 3.10.0-229.1.2.el7+ | [Ajánlott LIS4](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) <br> *Lásd a következő szakaszban hello megjegyzést* |
| Red Hat Enterprise Linux (RHEL) | 6.8+, 7.2+ | &nbsp; | &nbsp; |
| Oracle | 6.0+, 7.2+ | &nbsp; | UEK4 vagy RHCK |
| Oracle | 7.0-7.1 | &nbsp; | UEK4 vagy RHCK keresztüli rendelkező[LIS 4.1 +](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) |
| Oracle | 6.4-6.7 | &nbsp; | UEK4 vagy RHCK keresztüli rendelkező[LIS 4.1 +](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) |


### <a name="lis-drivers-for-openlogic-centos"></a>OpenLogic CentOS LIS illesztőprogramok

Ha OpenLogic CentOS virtuális gépek futnak, futtassa a következő parancs tooinstall hello legújabb illesztőprogramokkal hello:

```
sudo rpm -e hypervkvpd  ## (Might return an error if not installed. That's OK.)
sudo yum install microsoft-hyper-v
```

tooactivate hello új illesztőprogramok, indítsa újra a hello számítógépet.

## <a name="pricing-and-billing"></a>Árak és számlázás

Prémium szintű Storage használata esetén alkalmazza a következő számlázási szempontok hello:

* **Prémium szintű tároló lemez és a blob mérete**

    Egy prémium szintű tároló lemez vagy a blob számlázási hello lemez vagy blob kiépített hello méretétől függ. Az Azure maps hello (kerekítve) kiosztott mérete toohello legközelebbi prémium szintű tároló lemez lehetőséget. További információkért lásd: hello táblájában [prémium szintű Storage méretezhetőségi és Teljesítménycélok](#premium-storage-scalability-and-performance-targets). Minden lemez maps tooa támogatott kiépített lemez méretét, és ennek megfelelően van-e terhelve. A kiépített lemez számlázási óránkénti arányosítva van hello prémium szintű Storage ajánlat hello havi díjakon használatával. Például ha kiépített P10 lemezt, és 20 óra múlva törli azt, kell fizetni az hello P10 ajánlat arányosított too20 üzemideje (óra). Ettől függetlenül hello adatmennyiség tényleges toohello lemez vagy hello IOPS és használt átviteli sebességet.

* **Prémium szintű nem felügyelt lemezek pillanatképek**

    Nem felügyelt premium lemezek pillanatfelvételeket számlázása hello további kapacitást hello pillanatképek használják. A pillanatképek kapcsolatos további információkért lásd: [pillanatkép létrehozása a blob](/rest/api/storageservices/Snapshot-Blob).

* **Prémium szintű lemezek pillanatképek felügyelt**

    Felügyelt lemezes pillanatképet hello lemez csak olvasható másolatát. Standard szintű felügyelt lemezes hello lemezen tárolja. A pillanatkép költségek hello azonos szabványos lemez kezeli. Például ha egy pillanatképet, 128 GB-os prémium szintű felügyelt lemezes, hello pillanatkép hello költségét egyenértékű tooa 128 GB-os standard szintű felügyelt lemezes.  

* **Kimenő adatátvitel**

    [Kimenő adatátvitel](https://azure.microsoft.com/pricing/details/data-transfers/) (adatok kilépő Azure adatközpontjaiban) gigabájtalapú sávszélesség-használat.

Prémium szintű Storage, a prémium szintű Storage által támogatott virtuális gépek és a felügyelt lemezek árazással kapcsolatos részletes információkért lásd: ezek a cikkek:

* [Az Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/)
* [Virtuális gépek díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Felügyelt lemezek díjszabása](https://azure.microsoft.com/pricing/details/managed-disks/)

## <a name="azure-backup-support"></a>Az Azure Backup-támogatás 

Regionális katasztrófa utáni helyreállítás, biztonsági másolatot kell készíteni a virtuális gép lemezeit, egy másik régióban használatával [Azure biztonsági mentési](../backup/backup-introduction-to-azure-backup.md) és egy biztonsági mentési tárolóval Georedundáns tárfiókot.

toocreate idő-alapú biztonsági mentések, könnyű VM-helyreállítás és biztonsági másolatok megőrzésének házirendek, a biztonsági mentési feladatot a Azure biztonsági másolattal. Nem felügyelt és a felügyelt biztonsági mentés is használható. További információkért lásd: [Azure biztonsági mentés nem felügyelt lemezzel rendelkező virtuális gépek](../backup/backup-azure-vms-first-look-arm.md) és [felügyelt lemezzel rendelkező virtuális gépek Azure Backup](../backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup). 

## <a name="next-steps"></a>Következő lépések
Prémium szintű Storage kapcsolatos további információkért tekintse meg a következő cikkek hello.

### <a name="design-and-implement-with-premium-storage"></a>Tervezési és a prémium szintű Storage végrehajtása
* [A prémium szintű Storage teljesítmény tervezése](storage-premium-storage-performance.md)
* [Prémium szintű Storage BLOB storage műveletek](http://go.microsoft.com/fwlink/?LinkId=521969)

### <a name="operational-guidance"></a>Műveleti útmutató
* [Prémium szintű Storage tooAzure áttelepítése](storage-migration-to-premium-storage.md)

### <a name="blog-posts"></a>Blogbejegyzések
* [Prémium szintű Storage általánosan elérhető](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/)
* [Közzétevő hello GS sorozatnak: prémium szintű Storage támogatási toohello legnagyobb hozzáadása a virtuális gépek hello nyilvános felhő](https://azure.microsoft.com/blog/azure-has-the-most-powerful-vms-in-the-public-cloud/)
