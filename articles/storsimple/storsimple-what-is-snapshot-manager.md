---
title: aaaWhat a StorSimple Snapshot Manager? | Microsoft Docs
description: "Hello StorSimple Snapshot Manager, az architektúra és annak szolgáltatásait ismerteti."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 6094c31e-e2d9-4592-8a15-76bdcf60a754
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: v-sharos
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e79ce7b7e1970ac862038af2a0e67065b6fb6eda
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="an-introduction-toostorsimple-snapshot-manager"></a>Egy bevezető tooStorSimple Snapshot Manager

## <a name="overview"></a>Áttekintés
StorSimple Snapshot Manager egy Microsoft Management Console (MMC) beépülő modulja, amely leegyszerűsíti az adatvédelem és biztonsági mentési kezelése a Microsoft Azure StorSimple környezetben. A StorSimple Snapshot Manager segítségével kezelheti a Microsoft Azure StorSimple adatok hello adatközpont és felhő hello egyetlen integrált tárolási megoldásként, így biztonsági mentési folyamat egyszerűsítése, illetve csökkenti a költségeket.

Ez az áttekintés bemutatja a StorSimple Snapshot Manager hello, annak szolgáltatásait ismerteti, és szerepét a Microsoft Azure StorSimple ismerteti. 

Hello egész Microsoft Azure StorSimple rendszer, a SharePoint, beleértve a hello StorSimple eszköz, a StorSimple Manager szolgáltatás, a StorSimple Snapshot Manager és a StorSimple Adapter áttekintését lásd: [StorSimple 8000 series: hibrid felhő tárolási megoldás](storsimple-overview.md). 

> [!NOTE]
> * StorSimple Snapshot Manager toomanage Microsoft Azure StorSimple virtuális tömbök (más néven StorSimple a helyszíni virtuális eszköz) nem használható.
> * Ha a StorSimple eszköz tooinstall StorSimple 2. frissítést, meg arról, hogy toodownload hello legújabb verziójú, a StorSimple Snapshot Manager, a telepítés **StorSimple 2. frissítés telepítése előtt**. hello a StorSimple Snapshot Manager legújabb verziójára visszamenőlegesen kompatibilis, és együttműködik a Microsoft Azure StorSimple kiadott verziói. Ha használ hello a StorSimple Snapshot Manager korábbi verziója, szüksége lesz a tooupdate informatikai (nem szükséges toouninstall hello előző verzió hello új verziójának telepítése előtt).
> 
> 

## <a name="storsimple-snapshot-manager-purpose-and-architecture"></a>StorSimple Snapshot Manager célját és architektúra
StorSimple Snapshot Manager biztosít egy központi kezelőkonzol használható toocreate konzisztens, a biztonsági másolatok időpontban a helyi és a felhő adatokat. Például hello konzolját is használhatja:

* Konfigurálhatja, készítsen biztonsági másolatot, és törölje köteteket.
* Konfigurálja a kötet, amely az adatok biztonsági másolatát csoportok tooensure alkalmazáskonzisztens.
* Biztonsági mentési házirendek kezelése, így az adatok biztonsági másolatát egy előre meghatározott ütemezés szerint.
* Hozzon létre a helyi és felhőalapú pillanatfelvételek, amely hello felhőben tárolt, és a katasztrófa utáni helyreállítás.

StorSimple Snapshot Manager hello beolvassa hello hello VSS-szolgáltatójával hello gazdagépen regisztrált alkalmazások listáját. Ezt követően toocreate alkalmazáskonzisztens biztonsági mentések, ellenőrzi hello köteteket egy alkalmazás által használt, és kötet csoportok tooconfigure javasol. StorSimple Snapshot Manager használ a kötet csoportok toogenerate biztonsági másolatok, amelyek alkalmazáskonzisztens. (Alkalmazás konzisztencia beszélünk, amikor az összes kapcsolódó fájlokat és adatbázisok szinkronizálása és hello alkalmazás egy adott időpontban időben hello igaz állapotát képviselik.) 

StorSimple Snapshot Manager biztonsági mentések utat hello növekményes pillanatképek, amely csak a hello változtatásai rögzítéséhez hello utolsó biztonsági mentés óta. Emiatt biztonsági mentések kevesebb tárolási rendszerre van szüksége, és hozható létre és gyorsan vissza. StorSimple Snapshot Manager hello Windows kötet árnyékmásolata szolgáltatás (VSS) tooensure, hogy a pillanatképek alkalmazáskonzisztens adatok rögzítéséhez használja. (További információkért látogasson toohello integráció a Windows a kötet árnyékmásolata szolgáltatás szakasszal.) A StorSimple Snapshot Manager létrehozhat biztonsági mentési ütemterv vagy azonnali biztonsági másolatok készítése, igény szerint. Ha a biztonsági másolat, a StorSimple Snapshot Manager segítségével toorestore adatait van szüksége a helyi katalógus vagy a felhőalapú pillanatfelvételek közül választhat. Az Azure StorSimple visszaállítja csak hello adatokhoz, szükséges, amely megakadályozza a késlelteti az adatok rendelkezésre állását visszaállítási műveletek során.)

![StorSimple Snapshot Manager architektúrája](./media/storsimple-what-is-snapshot-manager/HCS_SSM_Overview.png)

**StorSimple Snapshot Manager architektúrája** 

## <a name="support-for-multiple-volume-types"></a>Több kötet típusok támogatása
StorSimple Snapshot Manager tooconfigure hello használja, és készítsen biztonsági másolatot a következő kötetek típusú hello: 

* **Alapszintű kötetek** – egyszerű kötet egy olyan egyetlen partíció egy alaplemezen. 
* **Egyszerű kötet** – egyszerű kötet dinamikus kötet, amely tartalmaz egyetlen dinamikus lemez szabad lemezterület. Egyszerű kötet áll egy egyetlen területet a lemezen, vagy az egymáshoz kapcsolódó több régióba hello ugyanazt a lemezt. (Kötet is kialakítható egyszerű csak dinamikus lemezeken.) Egyszerű kötetekkel nincsenek hibatűrő.
* **A dinamikus kötetek** – A dinamikus kötet olyan kötet dinamikus lemezen létrehozni. Dinamikus lemezek a dinamikus lemezek a számítógépben található kötetek adatbázis tootrack információt használnak. 
* **A dinamikus kötetek tükrözés** – tükrözés dinamikus kötetek hello RAID 1 architektúra épül. RAID 1 két vagy több lemez, és egy tükrözött készlet megegyezik adatot ír. Egy olvasási kérést majd bármely hello tartalmazó lemezt kell kezelnie a kért adatokat.
* **Fürt megosztott kötetei** – a fürt megosztott kötetei (CSV), egy feladatátvevő fürtben több csomópont is egyszerre olvasására vagy írására toohello ugyanazt a lemezt. Feladatátvétel az egyik csomópont tooanother csomópont akkor fordulhat elő, gyorsan, anélkül, hogy a meghajtó tulajdonosa vagy csatlakoztatni, leválasztása és eltávolítása a kötet módosítják. 

> [!IMPORTANT]
> Ne keverje a CSV-k és a CSV a hello azonos pillanatkép. A CSV-k és CSV pillanatképbe keverése nem támogatott. 
> 
> 

StorSimple Snapshot Manager toorestore teljes kötet csoportokkal vagy egyéni kötetek klónozása, és az egyes fájlok helyreállítása.

* [Kötetek és a kötet csoportok](#volumes-and-volume-groups) 
* [Biztonsági mentési típusok és a biztonsági mentési házirendek](#backup-types-and-backup-policies) 

További információ a StorSimple Snapshot Manager-szolgáltatásokat, és hogyan toouse, [StorSimple Snapshot Manager felhasználói felületén](storsimple-use-snapshot-manager.md).

## <a name="volumes-and-volume-groups"></a>Kötetek és a kötet csoportok
A StorSimple Snapshot Manager, akkor hozzon létre köteteket, és majd beállíthatja azokat kötet csoportokba. 

StorSimple Snapshot Manager kötet csoportok toocreate biztonsági másolatok, amelyek alkalmazáskonzisztens használja. Alkalmazás konzisztencia beszélünk, amikor az összes kapcsolódó fájlokat és adatbázisok szinkronizálása, és az adott időben alkalmazást hello valós állapotát képviselik. Kötet csoportok (más néven ezek *konzisztencia csoportok*) alkotnak hello alapján egy biztonsági mentési vagy visszaállítási feladat.

Kötet csoportok nem ugyanaz, mint a kötettárolók hello szövegrészt. A kötettároló tartalmaz egy vagy több kötet egy felhőbeli tárfiókot és egyéb attribútumait, például a titkosítás és a sávszélesség-használatát. Egyetlen kötettároló mentése vékonyan létesített too256 StorSimple-köteteket is tartalmazhat. Kötettárolók kapcsolatos további információkért lépjen túl[kezelése a kötettárolók](storsimple-manage-volume-containers.md). Kötet csoportok biztonsági mentési műveletek toofacilitate konfigurált kötetek gyűjteményei. Ha két kötetek, amelyek tartoznak toodifferent kötettárolók, helyezze el őket egy kötet csoportban, majd hozza létre a biztonsági mentési házirend, az adott kötet csoporthoz, minden olyan kötetre készül biztonsági másolat hello megfelelő mennyiségi tárolóban, megfelelő tárolási hello használata fiók.

> [!NOTE]
> Egy kötet csoportban található összes kötetet egy felhőszolgáltató kell származnia.
> 
> 

## <a name="integration-with-windows-volume-shadow-copy-service"></a>Integráció a Windows a kötet árnyékmásolata szolgáltatás
StorSimple Snapshot Manager hello toocapture alkalmazáskonzisztens adatok Windows kötet árnyékmásolata szolgáltatás (VSS) használja. VSS alkalmazás konzisztencia elősegíti a Kötetárnyékmásolat-felismerésre képes alkalmazások toocoordinate hello létrehozása növekményes pillanatképek kapcsolatba lép. VSS biztosítja, hogy hello alkalmazások ideiglenesen inaktív vagy videokártyának, amikor a pillanatfelvételeket készít. 

hello StorSimple Snapshot Manager végrehajtásának VSS működik az SQL Server és az általános NTFS-köteteket. hello folyamat a következőképpen történik: 

1. A kérelmező, amely általában egy adatok kezelése és adatvédelmi megoldás (például a StorSimple Snapshot Manager) vagy a biztonságimásolat-készítő alkalmazás, VSS hív, és kéri az hello író szoftver hello célalkalmazásban toogather adatait.
2. VSS névjegyek hello író összetevő tooretrieve hello adatok leírását. hello író hello adatok toobe biztonsági másolat hello leírását adja eredményül. 
3. VSS jelet hello író tooprepare hello alkalmazásnak a biztonsági mentéshez. hello író hello adatokat a biztonsági mentéshez előkészíti a nyitott tranzakciók végrehajtásával, frissítése tranzakciós naplókat, és így tovább, majd értesíti a VSS-t.
4. VSS hello író arra utasítja a tootemporarily stop hello alkalmazások adatait tárolja, és győződjön meg arról, hogy semmilyen adatot ír toohello kötet hello árnyékmásolat elkészítése közben. Ez a lépés biztosítja az adatok konzisztenciájának, és legfeljebb 60 másodpercet vesz igénybe.
5. VSS arra utasítja a hello szolgáltató toocreate hello árnyékmásolatot. Szolgáltatók, amely lehet a szoftver - vagy hardveralapú, aktuálisan futó és az igény szerinti árnyékmásolatait hozza őket létre hello kötetek kezelése. hello szolgáltató hello árnyékmásolatot hoz, és a VSS értesíti, amikor elkészült.
6. VSS névjegyek hello író toonotify hello alkalmazás i/o folytathatja, és is, hogy i/o árnyékmásolat során sikeresen szünetel tooconfirm másolja a létrehozása. 
7. Ha hello másolása sikeres volt, VSS adja vissza hello másolat helyét toohello kérelmezőnek. 
8. Ha az adatok írása megtörtént, amíg hello árnyékmásolat létrejött, majd hello biztonsági mentés inkonzisztens lesz. VSS hello árnyékmásolat törlése, és értesíti a hello kérelmezőnek. hello kérelmező automatikusan ismételje meg a biztonsági mentési folyamat hello, vagy értesítik a rendszergazda tooretry hello azt egy későbbi időpontban.

Tekintse meg a következő ábra hello.

![VSS-folyamat](./media/storsimple-what-is-snapshot-manager/HCS_SSM_VSS_process.png)

**A Windows a kötet árnyékmásolata szolgáltatás folyamat** 

## <a name="backup-types-and-backup-policies"></a>Biztonsági mentési típusok és a biztonsági mentési házirendek
A StorSimple Snapshot Manager, az adatok biztonsági másolatát és tárolására is használható, helyileg és hello felhőben található. StorSimple Snapshot Manager tooback adatok azonnal használhatja, vagy használhatja a biztonsági mentési házirend toocreate ütemezés szerint automatikusan biztonsági másolatok fogadására. Biztonsági mentési házirendek is lehetővé teszik a toospecify hány pillanatfelvételek megmaradnak. 

### <a name="backup-types"></a>Biztonsági mentési típusok
StorSimple Snapshot Manager toocreate hello a következő biztonsági mentési típusok használhatók:

* **A pillanatképek helyi** – helyi pillanatképeket másolatai időpontban kötet hello StorSimple eszközön tárolt adatokat. Általában ilyen típusú biztonsági mentést hozható létre és gyorsan visszaállítva. Egy helyi pillanatfelvétel a helyi biztonsági másolat ugyanúgy használhatja.
* **Felhőalapú pillanatfelvételek** – felhőalapú pillanatfelvételek másolatai időpontban kötet hello felhőben tárolt adatokat. Egy felhőalapú pillanatfelvétel értéke megegyezik egy másik, a telephelytől távoli tárolórendszer replikálva tooa pillanatkép. Felhőalapú pillanatfelvételek különösen hasznosak a vész-helyreállítási eljárással.

### <a name="on-demand-and-scheduled-backups"></a>Igény szerinti és ütemezett biztonsági mentések tiltása
A StorSimple Snapshot Manager egy egyszeri biztonsági mentési toobe létre azonnal is kezdeményezhető, vagy egy biztonsági mentési házirend tooschedule ismétlődő biztonsági mentési műveletek is használhat.

A biztonsági mentési házirend olyan használható tooschedule rendszeres biztonsági mentések automatikus szabályok készlete. A biztonsági mentési házirend lehetővé teszi toodefine hello gyakoriságát és paramétereket pillanatképek készítése a egy adott kötet csoportot. Mindkét helyi házirendek toospecify érvényességének kezdő és záró dátum, idő, gyakoriságot és megőrzési követelményeinek, használhat és felhőalapú pillanatfelvételek. A házirend érvényben van, megadhatja azt követően azonnal. 

StorSimple Snapshot Manager tooconfigure használhatja, vagy konfigurálja újra a biztonsági mentési házirendek, amikor szükséges. 

Konfigurálja a következő információkat az Ön által létrehozott biztonsági mentési házirendet hello:

* **Név** – hello hello egyedi nevét kiválasztva a biztonsági mentési házirend.
* **Típus** – hello típusú biztonsági mentési házirend; helyi pillanatfelvétel és felhőbeli pillanatfelvétel.
* **Kötet csoport** – hello kötet csoport toowhich hello kiválasztva a biztonsági mentési házirend hozzá van rendelve.
* **Megőrzési** – hello tooretain biztonsági másolatok száma. Ha bejelöli a hello **összes** , minden egyes biztonsági másolatok biztonsági másolatok kötetenkénti maximális száma hello eléréséig mezőjébe, mely pont hello házirend meghiúsul, és ad hibaüzenetet. Megadhat egy biztonsági mentések tooretain (közötti 1 és 64) száma.
* **Dátum** – hello hello biztonsági mentési házirend létrehozásának dátumát.

A biztonsági mentési házirendek konfigurálásával kapcsolatos információkért lásd az túl[használja a StorSimple Snapshot Manager toocreate és kezelheti a biztonsági mentési házirendek](storsimple-snapshot-manager-manage-backup-policies.md).

### <a name="backup-job-monitoring-and-management"></a>Biztonsági mentési feladat ellenőrzése és kezelése
StorSimple Snapshot Manager toomonitor hello használja, és a közelgő, ütemezett és befejezett biztonsági mentési feladatok kezelése. StorSimple Snapshot Manager emellett mentése befejeződött too64 biztonsági mentés katalógusa. Hello katalógus toofind használja, és a kötetek vagy fájlok visszaállítása. 

A biztonsági mentési feladatok figyelésével kapcsolatos tudnivalókat lásd túl[használja a StorSimple Snapshot Manager tooview és kezelheti a biztonsági mentési feladatok](storsimple-snapshot-manager-manage-backup-jobs.md).

## <a name="next-steps"></a>Következő lépések
* További információ [használatával StorSimple Snapshot Manager tooadminister a StorSimple megoldásban](storsimple-snapshot-manager-admin.md).
* Töltse le [StorSimple Snapshot Manager](https://www.microsoft.com/download/details.aspx?id=44220).

