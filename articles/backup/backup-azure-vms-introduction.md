---
title: "aaaPlanning a virtuális gép biztonsági mentési infrastruktúra az Azure-ban |} Microsoft Docs"
description: "Az Azure virtuális gépek tooback megtervezésekor fontos tudnivalók találhatók"
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "virtuális gépek biztonsági mentése, a virtuális gépek biztonsági mentése"
ms.assetid: 19d2cf82-1f60-43e1-b089-9238042887a9
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/18/2017
ms.author: markgal;trinadhk
ms.openlocfilehash: d11982431610000293038ee6aa7df8e7bc2d8b70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="plan-your-vm-backup-infrastructure-in-azure"></a>Virtuális gép biztonsági infrastruktúrájának megtervezése az Azure-ban
Ez a cikk ismerteti a teljesítmény- és erőforrás-javaslatok toohelp a virtuális gép biztonsági mentési infrastruktúra megtervezése. Egyúttal meghatározza, milyen kulcsfontosságú elemeit annak hello biztonsági mentési szolgáltatás; lehet, hogy ezeket az jellemzőket fontos meghatározni, hogy az architektúra a kapacitástervezés és ütemezés. Ha megismerte [a környezet előkészítése](backup-azure-vms-prepare.md), tervezési lépése hello következő megkezdése előtt [virtuális gépeinek tooback](backup-azure-vms.md). Ha az Azure virtuális gépek több információra van szüksége, tekintse meg a hello [Virtual Machines – dokumentáció](https://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="how-does-azure-back-up-virtual-machines"></a>Hogyan működik az Azure virtuális gépek biztonsági mentése?
Hello Azure Backup szolgáltatás a biztonsági mentési feladatot indít el hello ütemezett időpontban, amikor elindítja a hello tartalék mellék tootake időpontban pillanatkép. hello Azure Backup szolgáltatás által használt hello _VMSnapshot_ bővítményt, és a Windows hello _VMSnapshotLinux_ Linux bővítményt. hello bővítmény hello első virtuális gép biztonsági mentés során települ. tooinstall hello bővítmény: hello VM futnia kell. Hello virtuális gép nem fut, ha biztonsági mentési szolgáltatás hello pillanatképet készít a hello az alapul szolgáló tárolási (mert nem alkalmazás írási műveletek hello VM leállítása közben történik).

Windows virtuális gépek pillanatkép létrehozása van folyamatban, amikor hello biztonsági mentési szolgáltatás koordinálja a kötet árnyékmásolata szolgáltatás (VSS) tooget hello hello virtuális gépek lemezeit konzisztens pillanatképet. Ha biztonsági mentést készít a Linux virtuális gépek, a saját egyéni parancsfájlok tooensure konzisztencia írhat VM pillanatkép létrehozása van folyamatban. Meghívja a parancsfájlokat a részletek a cikk későbbi részében találhatók.

Amennyiben az Azure Backup szolgáltatás hello hello pillanatfelvételt, hello adata átvitt toohello tárolóban. toomaximize hatékonyságát, hello szolgáltatás azonosítja, és csak a hello korábbi biztonsági mentés óta megváltoztak adattípusnak hello blokkok átvitele.

![Azure virtuális gép biztonsági mentési architektúra](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

Hello adatátvitel befejezésekor hello pillanatkép törlődik, és egy helyreállítási pontot hoz létre.

> [!NOTE]
> 1. Hello biztonsági mentési folyamat során Azure biztonsági mentés nem tartalmazza a hello ideiglenes lemez toohello virtuális gépet. További információkért lásd: hello blog a [ideiglenes tárolási](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/).
> 2. Azure biztonsági mentési fogad adatokat tároló szintű pillanatképet és, hogy a pillanatkép-toovault átvitel, mert nem módosítható hello tárfiókkulcsok hello biztonsági mentési feladat befejezéséig.
> 3. Prémium szintű virtuális gépekhez a Microsoft hello pillanatkép toostorage fiók másolása. Ez a toomake meg arról, hogy Azure Backup szolgáltatás lekérdezi-e elegendő IOPS adatok toovault átvitelére. A tárolási további példányának hello VM lefoglalt méret szerint fel van töltve. 
>

### <a name="data-consistency"></a>Adatkonzisztencia
Biztonsági mentése és visszaállítása az üzleti szempontból fontos adatokhoz van bonyolítja hello tényt, hogy üzleti szempontból fontos adatokhoz kell biztonsági mentése közben hello alkalmazások, amelyek hello adatok futnak egymással. tooaddress a, Azure biztonsági mentés támogatja alkalmazáskonzisztens biztonsági mentés a Windows és a Linux virtuális gépek
#### <a name="windows-vm"></a>Windowsos VM
Azure biztonsági mentés VSS teljes biztonsági mentés a Windows virtuális gépeken időt vesz igénybe. (további információk [VSS teljes biztonsági mentés](http://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx)). tooenable VSS másolásos biztonsági mentéshez, következő beállításjegyzékbeli kulcs igények toobe beállítása a virtuális gép hello hello.

```
[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
"USEVSSCOPYBACKUP"="TRUE"
```

#### <a name="linux-vms"></a>Linux rendszerű virtuális gépek
Azure biztonsági mentés egy parancsfájl-kezelési keretrendszert biztosít. tooensure alkalmazás konzisztencia biztonsági mentésekor a Linux virtuális gépek, hozzon létre egyéni előtti és utáni parancsfájlok, amelyek vezérlik hello biztonsági mentési munkafolyamat és a környezetben. Azure biztonsági mentés hello előtti parancsfájl hív hello virtuális gép pillanatképének készítése előtt, és meghívja a hello utáni parancsfájl hello virtuális gép pillanatkép-feladat befejeződése után. További részletekért lásd: [alkalmazás konzisztens VM a biztonsági mentéseket parancsfájl előtti és utáni parancsfájl](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).
> [!NOTE]
> Azure biztonsági mentés csak hív meg felhasználói írott hello előtti és utáni parancsfájlokat. Hello előtti parancsfájl és utáni parancsfájlok végrehajtása sikertelen, ha Azure biztonsági mentés állapotúként jelöli meg hello helyreállítási pont alkalmazás konzisztens. Azonban hello ügyfél esetén végső soron felelős hello alkalmazás konzisztencia egyéni parancsfájlok segítségével.
>


Ez a táblázat ismerteti, hogy az Azure virtuális gép biztonsági mentés során fordul elő, és helyreállítási eljárásokat konzisztencia és hello feltételek hello típusát.

| Konzisztencia | VSS-alapú | MAGYARÁZAT és részletek |
| --- | --- | --- |
| Alkalmazáskonzisztencia |Igen, a Windows rendszerhez|Alkalmazás konzisztencia ideális munkaterhelésekhez, biztosítja, hogy:<ol><li> virtuális gép hello *elindul a*. <li>Nincs *sérülés nem*. <li>Nincs *adatvesztés nélküli*.<li> hello adata hello alkalmazásának hello időpontban biztonsági – VSS vagy előkészítő/utólagos parancsfájl segítségével bevonásával hello adatokat használó konzisztens toohello alkalmazást.</ol> <li>*Windows virtuális gépek*-legtöbb Microsoft-munkaterhelések rendelkezik munkaterhelés-specifikus műveletek kapcsolódó toodata konzisztencia VSS-írók. Például a Microsoft SQL Server, amely biztosítja, hogy hello írások toohello tranzakciósnapló-fájljának és hello adatbázis megfelelően történik-e VSS-író tartozik. Az Azure Windows virtuális gép biztonsági mentések, toocreate az alkalmazáskonzisztens helyreállítási pont hello tartalék mellék kell meghívni hello VSS munkafolyamat és befejeződését, mielőtt hello virtuális gép pillanatképének készítése. A pontos hello Azure virtuális gép pillanatkép toobe VSS-író hello Azure virtuális alkalmazások elvégzése után is. (További hello [VSS alapjait](http://blogs.technet.com/b/josebda/archive/2007/10/10/the-basics-of-the-volume-shadow-copy-service-vss.aspx) és mély alaposabban hello részleteit [működésének](https://technet.microsoft.com/library/cc785914%28v=ws.10%29.aspx)). </li> <li> *Linux virtuális gépek*-ügyfelek végrehajtható [egyéni parancsfájl előtti és utáni parancsfájl tooensure alkalmazás konzisztencia](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent). </li> |
| Fájlrendszer-konzisztenciával |Igen – Windows-alapú számítógépekhez |Két esetben, ahol a hello helyreállítási pont lehet *fájlrendszer konzisztens*:<ul><li>Linux virtuális gépek Azure nélkül előtti-script/követő-script, vagy ha előtti-script/követő-script nem sikerült biztonsági mentést. <li>VSS-hiba történt a Windows Azure virtuális gépek biztonsági mentését.</li></ul> Mindkét esetben a hello ajánlott, elvégezhető a tooensure, amelyek: <ol><li> virtuális gép hello *elindul a*. <li>Nincs *sérülés nem*.<li>Nincs *adatvesztés nélküli*.</ol> Alkalmazásokat a saját "javítás felfelé" tooimplement mechanizmus hello visszaállított adatok kell. |
| Összeomlás-konzisztencia |Nem |Ebben az esetben ez egyenértékű tooa virtuális gép "összeomlási" tapasztaló (keresztül letölthető vagy rögzített reset). Összeomlási konzisztencia általában akkor fordul elő, amikor hello Azure virtuális gép le van állítva a biztonsági mentés hello időpontjában. Összeomlás-konzisztens helyreállítási pontot hello adattárolóra--vagy hello operációs rendszer vagy hello alkalmazás hello szempontjából nincs garancia hello konzisztencia hello adatok körül biztosít. Hello adatok közül kizárólag a biztonsági mentés hello időpontjában hello lemez már létezik rögzíti, majd biztonsági mentés. <br/> <br/> Amíg vannak általában nem garantálja, operációs rendszer indítása, lemez-ellenőrzése után hello eljárás, például a chkdsk eszközhöz, toofix okozta hibák. Bármely a memóriában levő vagy írási műveleteket, amelyek még nincsenek átvitt toohello lemez elvesznek. hello alkalmazás általában követi a saját hitelesítési mechanizmus, abban az esetben adatok visszaállítása szükséges toobe történik. <br><br>Tegyük fel ha hello tranzakciónapló bejegyzéseit, amelyek még nincsenek hello adatbázis, a majd hello adatbázis letiltását a visszaállítás addig, amíg hello adatok nem konzisztens. Amikor adatokat (például átnyúló kötetek) több virtuális lemez van elosztva, a összeomlás-konzisztens helyreállítási pontot nem garanciákat nyújt hello adatok hello nézetet. |

## <a name="performance-and-resource-utilization"></a>Teljesítmény- és erőforrás-használat
Telepített helyszíni biztonsági mentési szoftver, például meg kell terveznie a kapacitás és az erőforrás-használat igényeinek biztonsági mentésekor az Azure virtuális gépeken. Hello [Azure Storage korlátozza](../azure-subscription-service-limits.md#storage-limits) határozza meg, hogyan toostructure VM központi telepítések tooget maximális teljesítményét minimálisan befolyásolja toorunning munkaterhelések.

Nagy figyelmet toohello következő Azure Storage korlátozza a biztonsági mentés teljesítményét tervezése során:

* Maximális kilépő / storage-fiók
* A tárfiók egy teljes kérelmek gyakorisága

### <a name="storage-account-limits"></a>Tárfiókok korlátai
Biztonsági mentési adatokat másolni a tárfiókból, toohello bemeneti/kimeneti műveletek száma másodpercenként (IOPS) és a kimenő forgalom (vagy átviteli) metrikák hello tárfiók hozzáadása. At hello azonos iops-érték és átviteli idő, a virtuális gépek is használ fel. hello cél tooensure biztonsági másolat, és a virtuális gép forgalom nem haladhatja meg a tárfiókok korlátai.

### <a name="number-of-disks"></a>Lemezek száma
hello biztonsági mentési folyamat minél gyorsabban megpróbál toocomplete egy biztonsági mentési feladat. Ennek során, akkor használhatja a lehető legtöbb erőforrást. Azonban az összes i/o-műveletek hello csak korlátozott *egyetlen BLOB tároló átviteli*, 60 MB másodpercenként legfeljebb tartalmaz. Egy kísérlet toomaximize gyorsasága hello biztonsági mentési folyamat ellenőrzi hello virtuális gép lemezeinek készíteni tooback *párhuzamosan*. Ha egy virtuális gép négy lemezzel rendelkezik, a hello szolgáltatás megkísérli tooback párhuzamosan összes négy lemezeket. Hello **lemezek száma** tárolási fiók biztonsági mentési forgalom meghatározásának egyik legfontosabb szempontja hello biztonsági mentése, folyamatban.

### <a name="backup-schedule"></a>Biztonsági mentés ütemezése
Egy további tényezőt, amely hatással van a teljesítmény hello **biztonsági mentés ütemezése**. Ha hello házirendek konfigurálásához, így minden virtuális gép biztonsági mentése: hello azonos időben, a forgalom a probléma megoldásáig ütemezett. hello biztonsági mentési folyamat megpróbál tooback párhuzamosan összes lemezeket. tooreduce hello biztonsági mentési érkező forgalmat egy tárfiókot, készítsen biztonsági másolatot a különböző virtuális gépek különböző időpontban hello nap-átlapolás nélkül.

## <a name="capacity-planning"></a>Kapacitástervezés
Hello előző tényezők üzembe, szükség van tooplan hello tárhely kihasználtsága igényli. Töltse le a hello [virtuális gép biztonsági mentési kapacitástervezési Excel-táblázat](https://gallery.technet.microsoft.com/Azure-Backup-Storage-a46d7e33) toosee hello hatását a lemez és a biztonsági mentés ütemezése lehetőségeket.

### <a name="backup-throughput"></a>Biztonsági mentési átviteli sebesség
Az egyes lemezek biztonsági mentés alatt Azure Backup szolgáltatás beolvassa hello blokkok hello lemezen, és tárolja, csak a módosított adatokat (növekményes biztonsági mentés) hello. hello következő táblázatban hello átlagos biztonsági másolat szolgáltatás átviteli értékeket. A következő adatok hello használ, megbecsülheti hello időn meg a megadott méretű tooback szükséges.

| Biztonsági mentési művelet | Hányad átviteli sebesség |
| --- | --- |
| Kezdeti biztonsági mentés |160 MB/s |
| A növekményes biztonsági mentés (DR) |640 MB/s <br><br> Átviteli sebesség elhagyta jelentősen hello megváltozásakor szükséges adatokat a (biztonsági mentés toobe) megvédheti hello lemez között.|

## <a name="total-vm-backup-time"></a>Virtuális gép teljes biztonsági mentés ideje
Hello biztonságimásolat-készítési időpont a legtöbb olvasását és az adatok másolásának telik, amíg a többi művelet járulnak toohello szükséges teljes idő tooback virtuális gépet:

* Túl szükséges időtartamnak[telepítse vagy frissítse a hello tartalék mellék](backup-azure-vms.md).
* Pillanatkép ideje, ami hello igénybe vett idő tootrigger pillanatképet. A pillanatképeket kiváltott Bezárás toohello ütemezett biztonsági mentés időpontja.
* Várakozási ideje. Hello biztonsági mentési szolgáltatás biztonsági mentések több ügyfél feldolgozása, mert biztonsági mentési adatok másolását pillanatkép toohello biztonsági mentési vagy helyreállítási szolgáltatások tároló esetleg nem azonnal elindítani. A csúcsterhelés napszakokban hello várakozási is stretch tooeight órában toohello feldolgozott biztonsági másolatok száma miatt. Azonban hello teljes virtuális gép biztonsági mentés ideje 24 óránál kevesebb napi biztonsági mentési házirendek.
* Adatátviteli idő, a biztonsági mentéshez szükséges idő szolgáltatásmódosításaiban toocompute hello növekményes korábbi biztonsági mentésből, és ezeket a módosításokat toovault tárolási átvitele.

### <a name="why-am-i-observing-longer12-hours-backup-time"></a>Miért vagyok I betartásával longer(>12 hours) biztonsági mentés ideje?
Biztonsági mentés két fázisból áll: pillanatképek készítése és átvitele hello pillanatképek toohello tárolóban. hello biztonsági mentési szolgáltatás optimalizálja a tároláshoz. Hello pillanatkép adatok tooa tároló átvitelekor hello szolgáltatást csak átviszi a növekményes változásokat hello előző pillanatképet.  toodetermine hello növekményes változásokat, hello szolgáltatás kiszámítja hello ellenőrzőösszeg hello blokkal. Ha a blokk módosul, hello blokk egy blokk toobe toohello tároló küldött azonosítja. Majd hello szolgáltatás csukja további mindegyik hello azonosított blokkok lehetőségek toominimize hello adatok tootransfer keres. Kiértékelése minden módosult blokkokat, után hello szolgáltatás hello módosítások egyesíti, és elküldi azokat toohello tároló. Néhány örökölt alkalmazás kicsi, töredezett írások optimálisak nem tárolására. Hello pillanatkép tartalmazza a sok kisméretű, töredezett írási műveleteket, ha a hello szolgáltatás hello alkalmazások által írt hello adatfeldolgozás további időt tölt. alkalmazás írási blokk javasolt Azure, a VM, hello belül futó alkalmazások hello legalább 8 KB-os. Ha az alkalmazás egy legfeljebb 8 KB-os blokkméretet, a biztonsági mentés teljesítményét történik. Az alkalmazás tooimprove biztonsági mentési teljesítményének hangolása kapcsolatban lásd: [alkalmazások az Azure storage optimális teljesítményének hangolása](../storage/common/storage-premium-storage-performance.md). Bár a biztonsági mentés teljesítményét hello foglalkozó prémium szintű storage példák használ, hello útmutatást esetén szabványos tárolólemezek alkalmazható.

## <a name="total-restore-time"></a>Teljes helyreállítás időpontjára
A visszaállítási művelet két fő sub tevékenységből áll: az adatok másolása vissza hello tároló kiválasztott toohello ügyfél tárfiókja, és hello virtuális gépet hoz létre. Az adatok másolásának vissza hello tárolóból hello biztonsági másolatokat tároló belső az Azure-ban, és hello ügyfél tárfiókja tároló függ. Toocopy adatok szükséges idő függ:
* Várakozási idő - hello szolgáltatás folyamatok feladatok visszaállíthatók, hello több ügyfél óta egy időben visszaállítási kérelmek várólista be.
* Az adatmásolási idő - adatok másolásakor hello tároló toohello ügyfél tárfiókja. Visszaállítás idő függ az IOPS és átviteli Azure Backup szolgáltatás lekérdezi a kijelölt hello ügyfél tárfiókja. tooreduce hello hello visszaállítási folyamat során másolja, válasszon egy tárfiókot, más alkalmazás írások és olvasások nincs betöltve.

## <a name="best-practices"></a>Ajánlott eljárások
Javasoljuk, hogy ezeket a gyakorlatokat, virtuális gépek biztonsági mentéseinek konfigurálása közben követően:

* Nem ütemezhető hello azonos cloud service tooback hello: a több mint 10 klasszikus virtuális gépeinek ugyanannyi időt vesz igénybe. Ha azt szeretné, hogy több virtuális gépeinek az azonos felhőszolgáltatás tooback, lépcsőzetes hello biztonsági másolat egy órával kezdési idejét.
* Nem ütemezhető be a virtuális gépek legfeljebb 40 tooback mentése: hello ugyanannyi időt vesz igénybe.
* Virtuális gép biztonsági mentések ütemezése során csúcsidőn kívül. Ezt úgy hello biztonsági mentési szolgáltatás IOPS használja az adatok átviteléhez a hello ügyfél tárolási fiók toohello tárolójából.
* Győződjön meg arról, hogy a házirend érvényben van-e elosztva a eltérő tárfiókokból virtuális gépeken. Javasoljuk, hogy legfeljebb 20 teljes lemezek egyetlen tárfiókból hello védi ugyanazt biztonsági mentés ütemezése. Ha nagyobb, mint 20 lemezek tárfiókokban, ossza szét több házirendek tooget különböző virtuális gépek hello szükséges IOPS hello átviteli fázis hello biztonsági mentési folyamat során.
* Ne állítson vissza egy tárolási toosame prémium szintű tárfiók futó virtuális gép. Ha hello visszaállítási művelet folyamat egybevág hello biztonsági mentési művelet, hello csökkenti a biztonsági mentéshez elérhető iops-érték.
* A prémium szintű virtuális gép biztonsági mentése győződjön meg arról, hogy az állomások premium lemezek rendelkezik-e legalább 50 % szabad terület a sikeres biztonsági mentéshez pillanatkép átmeneti tárolási fiók. 
* Győződjön meg arról, hogy a Linux virtuális gépeken python verzió engedélyezett a biztonsági mentés 2.7

## <a name="data-encryption"></a>Adattitkosítás
Azure biztonsági mentés titkosítja az adatokat a hello biztonsági mentési folyamatának részeként. Azonban hello VM belül az adatokat titkosíthatja és hello védett adatok biztonsági mentése zökkenőmentesen (tudjon meg többet az [titkosított adatok biztonsági mentését](backup-azure-vms-encryption.md)).

## <a name="calculating-hello-cost-of-protected-instances"></a>Védett példányok hello költségét kiszámítása
Azure Backup használatával biztonsági mentése Azure virtuális gépek vonatkoznak túl[Azure Backup árairól](https://azure.microsoft.com/pricing/details/backup/). Védett példányok számítási hello hello alapuló *tényleges* hello virtuális gép, amely hello összege hello virtuális gép – hello "erőforrás lemez." kivételével az összes hello adat mérete

A virtuális gépek biztonsági mentéséről árképzési *nem* adatok lemezre csatlakoztatott toohello virtuális gépek hello legnagyobb támogatott mérete alapján. Árképzési hello tényleges hello adatlemez tárolt adatok alapján. Ehhez hasonlóan hello biztonsági másolatok tárolásának számlázási tárolt adatokat az Azure biztonsági mentési szolgáltatásban, ez az egyes helyreállítási pontok hello adat hello összege hello mennyisége alapján történik.

Vegyük például, egy szabványos A2 méretű virtuális gépekről, amelyek két adatlemeznek 1 TB maximális mérettel. hello alábbi táblázat áttekintést nyújt az egyes lemezeken tárolt hello tényleges adatokat:

| Lemez típusa | Maximális méret | Tényleges adatok |
| --- | --- | --- |
| Operációsrendszer-lemez |1023 GB |17 GB |
| Helyi lemez / erőforrás lemez |135 GB |5 GB (biztonsági mentés nem tartalmazza) |
| Adatlemez 1 |1023 GB |30 GB |
| Adatlemez 2 |1023 GB |0 GB |

Hello *tényleges* hello virtuális gépet ebben az esetben mérete 17 GB + 30 GB + 0 GB = 47 GB. A védett példányméretének (47 GB) lesz hello havi számlán hello alapját. Hello, hello virtuális gépen adatok mennyisége növekszik, hello védett példányméretének használt számlázási változásainak megfelelően.

Számlázási hello első sikeres biztonsági mentés befejezéséig nem indul el. Ezen a ponton a tároló és a védett példányok hello számlázási kezdődik. Számlázási továbbra is fennáll, mindaddig, amíg nincs *bármely tárolóban tárolt adatok biztonsági mentését* hello virtuális géphez. Ha hello virtuális gép védelmének megszüntetéséről, de a virtuális gép biztonsági mentési adatok létezik-e az adott tárolóban, számlázási továbbra is.

A megadott virtuális gép leáll, csak akkor, ha hello védelem ki van kapcsolva számlázási *és* minden biztonsági mentési adatok törlése. Amikor a védelem leáll, de nincs aktív biztonsági mentési feladatok, hello utolsó sikeres VM biztonsági mentés mérete hello hello védett példányméretének hello havi számlán használt válik.

## <a name="questions"></a>Kérdései vannak?
Ha kérdése van, vagy ha bármely új szolgáltatása része, toosee szeretné [visszajelzést küldhet](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Következő lépések
* [Készítsen biztonsági másolatot a virtuális gépek](backup-azure-vms.md)
* [Kezelheti a virtuális gép biztonsági mentése](backup-azure-manage-vms.md)
* [Virtuális gépek visszaállítása](backup-azure-restore-vms.md)
* [Virtuális gép biztonsági másolatával kapcsolatos problémák elhárítása](backup-azure-vms-troubleshoot.md)
