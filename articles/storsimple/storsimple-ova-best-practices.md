---
title: "a StorSimple virtuális tömb aaaBest eljárások |} Microsoft Docs"
description: "Központi telepítésére és felügyeletére a StorSimple virtuális tömb hello hello a bevált gyakorlat ismertetése."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 57ac6eeb-c47c-442d-a5f4-b360d81a76a6
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/08/2017
ms.author: alkohli
ms.openlocfilehash: f242364365e07f86b78e2f9bb2acfb3aa98e83e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-virtual-array-best-practices"></a>A StorSimple virtuális tömb gyakorlati tanácsok
## <a name="overview"></a>Áttekintés
Microsoft Azure StorSimple virtuális tömbjének értéke egy integrált tárolási megoldás, amely kezeli a tárolási feladatok elvégzéséről egy Microsoft Azure felhőalapú tárolást és hipervizor futtató helyszíni virtuális eszköz között. A StorSimple virtuális tömbjének értéke egy költséghatékony és költséghatékony alternatív toohello 8000 sorozat fizikai tömb. hello virtuális tömb futtathatja a meglévő hipervizor-infrastruktúrát, hello iSCSI és hello SMB protokollt is támogatja, és jól használható távoli office/fiókirodák számára. További információ hello StorSimple megoldásokról: túl[Microsoft Azure StorSimple áttekintése](https://www.microsoft.com/en-us/server-cloud/products/storsimple/overview.aspx).

Ez a cikk foglalkozik hello ajánlott eljárások során hello kezdeti beállítás, telepítés és felügyelete a StorSimple virtuális tömb hello megvalósítva. Az alábbi gyakorlati tanácsok nyújtanak érvényesített hello beállítása és kezelése a virtuális tömbjét. Ez a cikk célja felé hello informatikai rendszergazdák, akik telepítéséhez és kezeléséhez az adatközpontjaikban virtuális tömbök hello.

Azt javasoljuk, hogy vonatkozó hello bevált gyakorlatok toohelp biztosítani az eszköz továbbra is a megfelelőségi módosításakor toohello telepítési vagy működési áramlását. Kell problémába ütközik közben hajt végre az alábbi gyakorlati tanácsok a virtuális tömb a [forduljon a Microsoft Support](storsimple-virtual-array-log-support-ticket.md) segítségért.

## <a name="configuration-best-practices"></a>Technológiának az ajánlott eljárásokkal
Az alábbi gyakorlati tanácsok kell követni hello kezdeti telepítése és központi telepítési hello virtuális tömbök során toobe hello irányelvek foglalkozik. Az alábbi gyakorlati tanácsok ezen kapcsolódó toohello hello virtuális gép, csoportházirend-beállítások, méretezése, kiépítés beállítása hello hálózatok, storage-fiókok konfigurálása és megosztások létrehozása és a kötetek hello virtuális tömb tartalmazza. 

### <a name="provisioning"></a>Kiépítés
A StorSimple virtuális tömb annak a kiszolgálónak egy virtuális gép (VM) hello hipervizor (Hyper-V vagy VMware) regisztrálva. Hello virtuális gépek kiépítésekor győződjön meg arról, hogy az állomás képes toodedicate elegendő erőforrással. További információ: túl[minimális követelményeinek](storsimple-virtual-array-deploy2-provision-hyperv.md#step-1-ensure-that-the-host-system-meets-minimum-virtual-array-requirements) tooprovision tömb.

A következő gyakorlati tanácsok hello virtuális tömb létesítésekor hello megvalósításához:

|  | Hyper-V | VMware |
| --- | --- | --- |
| **Virtuális gép típusa** |**2. generációs** használata Windows Server 2012 vagy újabb virtuális gép és egy *.vhdx* kép. <br></br> **1. generációs** használata a Windows Server 2008 vagy újabb virtuális gép és egy *.vhd* kép. |Virtuális gép verziója 8-11 használatakor *.vmdk* kép. |
| **Memória típusa** |Beállítása **statikus memória**. <br></br> Ne használjon hello **dinamikus memória** lehetőséget. | |
| **Adattípus-lemez** |Mint kiépítése **dinamikusan bővülő**.<br></br> **Rögzített méretű** hosszú időt vesz igénybe. <br></br> Ne használjon hello **különbséglemezek** lehetőséget. |Használjon hello **rendelkezés dinamikusan** lehetőséget. |
| **Adatok lemez módosítása** |Bővítése vagy zsugorítását nem engedélyezett. Egy kísérlet toodo úgy hello összes hello helyi adatok elvesztését, eszközön eredményez. |Bővítése vagy zsugorítását nem engedélyezett. Egy kísérlet toodo úgy hello összes hello helyi adatok elvesztését, eszközön eredményez. |

### <a name="sizing"></a>Méretezése
A StorSimple virtuális tömb osztályozás, vegye figyelembe a következő tényezőket hello:

* Kötetek vagy megosztások helyi foglalás. Körülbelül 12 % hello terület minden kiosztott rétegzett kötetet vagy megosztást a hello helyi rétegen van fenntartva. Nagyjából 10 % hello terület is számára van fenntartva a fájlrendszer egy helyileg rögzített kötet.
* Pillanatkép-terhelés. Nagyjából 15 % terület hello helyi rétegen a pillanatképek számára van fenntartva.
* Visszaállítások szükségességét. Ha új műveletként visszaállítási tesz, méretezési hello terület a visszaállítási művelethez szükséges kell fiók. Visszaállítás történik tooa megosztás vagy hello mennyiségű azonos méretűnek.
* Néhány puffer váratlan növekedésének a kell kiosztani.

Hello megelőző tényezők alapján, követelmények méretezése hello ábrázolható a következő egyenlet hello:

`Total usable local disk size = (Total provisioned locally pinned volume/share size including space for file system) + (Max (local reservation for a volume/share) for all tiered volumes/share) + (Local reservation for all tiered volumes/shares)`

`Data disk size = Total usable local disk size + Snapshot overhead + buffer for unexpected growth or new share or volume`

hello következő példák bemutatják a követelmények alapján egy virtuális tömb méretezése.

#### <a name="example-1"></a>1. példa:
A virtuális tömb kívánt toobe tudni

* kiépítése 2 TB rétegzett kötetet vagy megosztást.
* egy 1 TB-os kiépítése rétegzett kötetet vagy megosztást.
* helyileg rögzített kötet 300 GB-os kiépítése, vagy a megosztási.

Hello az előző kötetek vagy megosztások, tudassa velünk hello helyi rétegen hello lemezterület kiszámításához.

Első lépésként minden rétegzett kötet vagy megosztás, helyi lefoglalás egyenlő too12 % lenne hello kötet, illetve megosztási méretű. Hello helyileg rögzített kötetet vagy megosztást helyi lefoglalás esetén 10 %-a hello helyileg rögzített kötet, illetve megosztási mérete (továbbá toohello kiépített mérete). Ebben a példában van szüksége

* 240 GB helyi lefoglalás (2 TB rétegzett kötet vagy megosztás)
* 120 GB helyi lefoglalás (1 TB rétegzett kötet vagy megosztás)
* A helyileg rögzített kötetet vagy megosztást (hozzáadása 10 %-a helyi foglalási toohello kiépített 300 GB-os méret) 330 GB

hello hello helyi rétegen, amennyiben szükséges teljes lemezterület van: 240 GB + 120 GB + 330 GB = 690 GB.

Hello helyi rétegen legalább annyi szabad helyre második, mint legnagyobb egyetlen foglalás hello kell. Ezt a felesleges mennyiséget használják, ha egy felhő-pillanatfelvételt a toorestore van szüksége. Ebben a példában a hello legnagyobb helyi foglalás a 330 GB (beleértve a fájlrendszer foglalási), így szeretné beállítani, hogy toohello 690 GB: 690 GB + 330 GB = 1020 GB.
Ha azt hajtja végre a következő további visszaállítások, azt is mindig felszabadítása hello hello korábbi visszaállítási művelet.

Harmadik, igazolnia kell a teljes helyi 15 %-át a lemezterület-eddig toostore helyi pillanatképeket, így csak 85 %-a, nem érhető el. Ebben a példában, amely körüli lenne 1020 GB = 0.85&ast;kiosztott adatok lemezre TB. Igen, hello kiosztott adatlemez lenne (1020&ast;(1/0.85)) 1200-as GB = 1,20 TB = ~ 1,25 TB (toonearest KVARTILIS kerekítés)

Amíg nem várt növekedés és új visszaállítások, körül a helyi lemezen kell kiépíteni 1,25-1,5 TB.

> [!NOTE]
> Azt javasoljuk, hogy hello helyi lemez kiosztása. Ez a javaslat oka az, hogy hello visszaállítási területre csak van szükség, ha azt szeretné, hogy öt napnál régebbi toorestore adatokat. Elemszintű helyreállítás lehetővé teszi a hello toorestore adatok öt napnál anélkül, hogy a visszaállítási területnek hello.


#### <a name="example-2"></a>2. példa:
A virtuális tömb kívánt toobe tudni

* kiépítése 2 TB rétegzett kötet
* 300 GB-os helyileg rögzített kötet létrehozása

A rétegzett kötetek vagy megosztások helyi lemezterület-foglalás és a helyileg rögzített kötetek vagy megosztások 10 % 12 % alapján, igazolnia kell

* 240 GB helyi lefoglalás (2 TB rétegzett kötet vagy megosztás)
* A helyileg rögzített kötetet vagy megosztást (10 %-a helyi lefoglalás toohello kiépített 300 GB lemezterület hozzáadása) 330 GB

Teljes terület szükséges hello helyi rétegen van: 240 GB + 330 GB = 570 GB

hello helyi terület a visszaállítási művelethez szükséges 330 GB.

a teljes lemezterület 15 %-át használt toostore pillanatképek, így csak 0.85 érhető el. Igen, hello lemez mérete (900&ast;(1/0.85)) 1.06 TB = ~ 1,25 TB (toonearest KVARTILIS kerekítés)

Amíg nem várt növekedésének, oszthat 1,25-1,5 TB helyi lemezen.

### <a name="group-policy"></a>Csoportházirend
A csoportházirend olyan infrastruktúra, amely lehetővé teszi a felhasználók és számítógépek adott konfigurációi tooimplement. Csoportházirend-beállításokat csoportházirend-objektumok (GPO-k), amelyek csatolt toohello tartalmazza a következő Active Directory tartományi szolgáltatások (AD DS) tárolók: webhelyekhez, tartományokhoz vagy szervezeti egységekhez (OU-k). 

Ha a virtuális tömb a tartományhoz, a csoportházirend-objektumok alkalmazott tooit lehet. A csoportházirend-objektumok, például víruskereső szoftver, amely hátrányosan befolyásolhatja a StorSimple virtuális tömb hello hello működésének alkalmazásokat telepíthet.

Ezért javasoljuk, hogy:

* Győződjön meg arról, hogy a virtuális tömb saját szervezeti egységben (OU) az Active Directory.
* Győződjön meg arról, hogy egyetlen csoportházirend-objektumok (GPO) alkalmazott tooyour virtuális tömb. Letilthatja az öröklést, amely virtuális tömb (gyermekcsomópont) hello tooensure nem automatikusan örökölt egyetlen csoportházirend-objektum hello. További információ: túl[öröklődés blokkolása](https://technet.microsoft.com/library/cc731076.aspx).

### <a name="networking"></a>Hálózat
a virtuális tömb hello hálózati konfigurációja hello helyi webes felhasználói felület segítségével történik. Egy virtuális hálózati adapteren keresztül, amelyben ki van építve hello virtuális tömb hello hipervizor engedélyezve van. Használjon hello [hálózati beállítások](storsimple-virtual-array-deploy3-fs-setup.md) tooconfigure hello virtuális hálózati illesztő IP-cím, alhálózati és átjáró lapon.  Beállíthatja úgy is hello elsődleges és másodlagos DNS-kiszolgáló, a beállításokat és a nem kötelező proxybeállításainak az eszközhöz. A legtöbb hello hálózati konfiguráció egy egyszeri beállítást. Felülvizsgálati hello [hálózati követelményei StorSimple](storsimple-ova-system-requirements.md#networking-requirements) előzetes toodeploying hello virtuális tömb.

A virtuális tömb való telepítésekor, azt javasoljuk, hogy pontosan kövesse az alábbi gyakorlati tanácsok:

* Győződjön meg arról, hogy mely hello virtuális tömb telepítése mindig hello hálózati hello toodedicate 5 MB/s internetes sávszélességet (vagy ennél nagyobb).
  
  * Internetes sávszélesség szükséges a munkaterhelés jellemzőit és hello adatcsere sebességétől függ.
  * hello változását, amelyek kezelhetők közvetlenül arányos tooyour internetes sávszélességet. Például ha a biztonsági másolat egy 5 MB/s sávszélesség 8 óra körül 18 GB adatok változás lehetővé teszi. Négy alkalommal nagyobb sávszélességgel (20 MB/s) négy alkalommal további adatok módosítása (72 GB) képes kezelni.
* Győződjön meg arról, Internet kapcsolat toohello mindig érhető el. Szórványos vagy nem megbízható internetes kapcsolatok toohello eszközök hozzáférési toodata hello felhőben adatvesztést eredményezhet, és nem használható konfigurációt eredményezhet.
* Ha azt tervezi, toodeploy az eszköz iSCSI-kiszolgálóként:
  
  * Azt javasoljuk, hogy tiltsa le a hello **IP-cím automatikus beszerzése** (DHCP) lehetőséget.
  * Statikus IP-címek konfigurálásához. Konfigurálnia kell egy elsődleges és másodlagos DNS-kiszolgáló.
  * Ha több hálózati adapterrel határozza meg a virtuális tömb, csak hello első hálózati adapter (alapértelmezés szerint ez az interfész van **Ethernet**) el lehet érni hello felhő. toocontrol hello típusú forgalom, több virtuális hálózati adapterek létrehozása a virtuális tömb (iSCSI-kiszolgálóként konfigurált) és az illesztők toodifferent alhálózatok csatlakozzon.
* toothrottle hello felhő sávszélesség csak (hello virtuális tömb által használt), konfigurálja a sávszélesség-szabályozás a hello útválasztó vagy hello tűzfalat. Ha megadja, hogy a hipervizor szabályozását, azt fogja szabályozás többek között az iSCSI- és SMB helyett csak a hello felhő sávszélesség összes hello protokollt.
* A hipervizorok engedélyezve van, győződjön meg arról, hogy időszinkronizálást. Ha a Hyper-v rendszerű, jelölje ki a virtuális tömb hello Hyper-V kezelőjében, nyissa meg túl**beállítások &gt; Integration Services**, és győződjön meg arról, hogy hello **időszinkronizálás** be van jelölve.

### <a name="storage-accounts"></a>Tárfiókok
A StorSimple virtuális tömb társítható egy tárfiókot. Ez a tárfiók lehet egy olyan fiók az hello automatikusan létrehozott tárfiók hello szolgáltatást, vagy a tárfiók ugyanahhoz az előfizetéshez kapcsolódó tooanother előfizetés. További információkért lásd: hogyan túl[a virtuális tömb storage-fiókok kezelése](storsimple-virtual-array-manage-storage-accounts.md).

A következő javaslatok a virtuális tömb társított tárfiókok használata hello.

* Több virtuális tömbök egy tárfiókkal rendelkező csatoláskor számításba a hello maximális kapacitás (64 TB) egy virtuális tömb és hello maximális mérete (500 TB) egy tárfiókot. Ez a teljes méretű virtuális tömbökben, amelyek adott tárolási fiók tooabout 7 társítható hello számának korlátozása.
* Új tárfiók létrehozásakor
  
  * Ajánlott létrehozni a hello régió legközelebbi toohello távoli office/fiókiroda hol a StorSimple virtuális telepített toominimize késések fordulnak elő.
  * Figyelembe kell vennie, hogy Ön nem helyezhetők át a tárfiók különböző régiókban. Különböző előfizetéseknél is nem helyezhető át a szolgáltatást.
  * Használja a storage-fiók, amely redundancia hello adatközpont között. Georedundáns tárolás (GRS), a zóna redundáns tárolás (ZRS) és a helyileg redundáns tárolás (LRS) használatát támogatja a virtuális tömb való használatra. További információ a tárfiókok különböző típusú hello lépjen túl[az Azure storage replikációs](../storage/common/storage-redundancy.md).

### <a name="shares-and-volumes"></a>Megosztások és kötetek
A StorSimple virtuális tömb megosztások fájlkiszolgáló és a kötetek, ha az iSCSI-kiszolgálóként konfigurált konfigurálásakor oszthat meg. hello gyakorlati tanácsok a megosztások és kötetek létrehozása kapcsolódó toohello méretét és hello típus konfigurálva.

#### <a name="volumeshare-size"></a>Kötet, illetve megosztási mérete
A virtuális tömb megosztások fájlkiszolgáló és a kötetek, ha az iSCSI-kiszolgálóként konfigurált konfigurálásakor oszthat meg. hello gyakorlati tanácsok a megosztások és kötetek létrehozása toohello mérete vonatkoznak, és hello konfigurált típusa. 

Ne feledje hello megosztások vagy kötetek a virtuális eszközön kiépítésekor a következő gyakorlati tanácsok.

* hello méretek kiépített relatív toohello fájlméret rétegzett megosztás befolyásolhatja a hello rétegezési teljesítményt. Nagy fájlok használata a lassú réteghez kimenő eredményezheti. Munka nagy fájlok esetében azt javasoljuk, hogy hello legnagyobb fájl hello a fájlmegosztás méretének % 3-nál kisebb.
* Legfeljebb 16 kötetek vagy megosztások hello virtuális tömb hozható létre. Hello méretkorlátait hello a helyileg rögzített, és a rétegzett kötetek vagy megosztások, mindig tekintse meg a toohello [korlátozza a StorSimple virtuális tömb](storsimple-ova-limits.md).
* Egy köteten, a várt hello adatok felhasználásához, valamint a jövőbeli növekedésre tényező létrehozásakor. hello kötet később nem bonthatók ki.
* Hello kötet létrehozása után a StorSimple hello kötet hello mérete nem zsugorítható.
* Amikor tooa írása rétegzett kötet StorSimple, a, ha hello kötetadatokról elér egy meghatározott küszöbérték (relatív toohello helyi terület hello kötet számára fenntartva), hello IO folyamatban van. Folytatás toowrite toothis kötet lelassítja hello IO jelentősen. Bár a tooa írhat rétegzett kötet (nem aktívan leállítása kiépített hello kapacitása túl írás hello felhasználót) kiosztott kapacitásánál nagyobbra úgy, hogy rendelkezik túllépte riasztási értesítés toohello hatása. Miután hello riasztást látja, rendkívül fontos, hogy szánjon javító intézkedéseket, például a törlés hello kötet (kötet bővítése jelenleg nem támogatott).
* Vész-helyreállítási használatából adódó mert megengedett megosztások vagy kötetek száma hello 16 és hello maximális száma párhuzamos feldolgozható megosztások vagy kötetek is 16, megosztások vagy kötetek hello száma nem hatással vannak a helyreállítási Időkorlát és RTO.

#### <a name="volumeshare-type"></a>Kötet, illetve megosztási típusa
StorSimple hello használata alapján két kötet, illetve megosztási-típusokat támogatja: helyileg rögzített és rétegzett. Helyileg rögzített kötetek vagy megosztások mivel hello rétegzett kötetek vagy megosztások vannak vékonyan létesített vannak kiosztása. Nem alakítható át egy helyileg rögzített kötet, illetve megosztási tootiered vagy *fordítva* létrehozását követően.

Azt javasoljuk, hogy a StorSimple-kötetek vagy megosztások konfigurálásakor a következő gyakorlati tanácsok hello megvalósításához:

* Hello kötettípus toodeploy tervezi, mielőtt létrehozna egy kötet hello munkaterhelések alapján azonosíthatja. Használjon, amely van szükség az adatok helyi garanciákat (még akkor is során egy felhőalapú leállás) és, amely felhő alacsony késleltetésű munkaterhelések helyileg rögzített kötetekhez. Miután egy kötet hoz létre a virtuális tömb, hello kötet típusa nem módosítható a helyileg rögzített tootiered vagy *viszont*. Helyileg rögzített kötetekhez például létrehozása, ha SQL munkaterhelések vagy a munkaterhelések virtuális gépeket (VM); használja a rétegzett kötetek fájl megosztás munkaterhelésekhez.
* A beállítást hello gyakran használt archív adatokhoz a nagy méretűek meghatározásakor. Ez a beállítás engedélyezve van egy nagyobb deduplikációs adattömbméret 512 k használatos tooexpedite hello adatátvitel toohello felhő.

#### <a name="volume-format"></a>Kötet formátumban
Miután az iSCSI-kiszolgálón létrehozott StorSimple-köteteket, szüksége tooinitialize csatlakoztatási és formátum hello kötetek. A művelet végrehajtását hello csatlakozó állomás tooyour StorSimple eszközön. Következő gyakorlati tanácsokat csatlakoztatása és hello StorSimple gazdagépen kötetek formázását.

* Gyorsformázást a StorSimple-köteteket.
* A StorSimple-kötet formázása, amikor egy foglalásiegység-méret (Ausztráliai) 64 KB-os használata (alapértelmezett érték 4 KB-os). hello 64 KB-os Ausztráliai tesztekre alapozva házon általános StorSimple munkaterheléseket és más munkaterhelésekhez.
* Ha a StorSimple virtuális tömbhöz az iSCSI-kiszolgálóként konfigurált hello nem használatával átnyúló kötetek vagy a dinamikus lemezek ezeken a köteteken, vagy lemezek StorSimple használata nem támogatott.

#### <a name="share-access"></a>Fájlmegosztási hozzáférést
A virtuális tömb fájlkiszolgáló megosztások létrehozásakor kövesse az alábbi irányelveket:

* Megosztás létrehozásakor rendelje hozzá a felhasználói csoport helyett egy-egy felhasználóhoz megosztás rendszergazdaként.
* Hello NTFS-engedélyek kezelheti a Windows Explorer használatával hello megosztások szerkesztésével hello megosztás létrehozása után.

#### <a name="volume-access"></a>Kötet hozzáférésével
A StorSimple virtuális tömb hello iSCSI kötetek konfigurálásakor fontos toocontrol hozzáférést is szükség. toodetermine, amely állomáskiszolgáló kötetek eléréséhez, hozzon létre, és hozzáférés-vezérlési rekordokat (ACRs) társítani a StorSimple-köteteket.

Amikor ACRs konfigurálása a StorSimple-köteteket a következő gyakorlati tanácsok hello használata:

* Legalább egy ACR mindig társíthat egy köteten.

* Egynél több ACR tooa kötet hozzárendelésekor győződjön meg arról, hogy hello kötet nincs felfedve oly módon, ahol egyidejűleg hozzáférhetők egynél több nem fürtözött gazdagép. Több ACRs tooa kötet hozzárendelt, egy figyelmeztető üzenet jelenik meg, az Ön tooreview a konfigurációt.

### <a name="data-security-and-encryption"></a>Adatbiztonság és -titkosítás
A StorSimple virtuális tömb rendelkezik adatok biztonsági és titkosítási funkciók, amelyek biztosítják a hello titkosítás és az adatok sértetlenségét. Ha használja ezeket a funkciókat, javasoljuk, hogy pontosan kövesse az alábbi gyakorlati tanácsok: 

* A felhőalapú tárolás titkosítási kulcs toogenerate AES-256 titkosításának definiálhatja a hello adatokat küldi el a virtuális tömb toohello felhőből. Ez a kulcs nincs szükség, ha az adatok a titkosított toobegin. hello kulcs létrehozható és biztonságos kulcskezelés rendszert használ, mint maradjon [az Azure key vault](../key-vault/key-vault-whatis.md).
* Konfigurálás hello tárfiók hello StorSimple Manager szolgáltatással, győződjön meg arról, hogy engedélyezi-e SSL-mód toocreate hello egy biztonságos csatornát a StorSimple eszköz és a hello közötti hálózati kommunikáció a felhő.
* A storage-fiókok (hello Azure Storage szolgáltatás elérésével) kulcsainak újragenerálása hello rendszeresen bármely tooaccount módosítja tooaccess rendszergazdák megváltozott hello listája alapján.
* A virtuális tömb adatok tömörített, és a deduplikált tooAzure elküldés előtt. A Windows Server-állomáson hello az Adatdeduplikáció szerepkör-szolgáltatás használatával nem ajánlott.

## <a name="operational-best-practices"></a>Gyakorlati tanácsok az üzemeltetéshez
hello gyakorlati tanácsok az üzemeltetéshez olyan irányelvek, napi szintű felügyeletéért hello vagy hello virtuális tömb során betartandó. Ezeket a gyakorlatokat fedik le az adott felügyeleti feladatokhoz, mint a biztonsági másolatok, visszaállítást végezni egy biztonságimásolat-készlet, a feladatátvétel végrehajtásához, inaktiválása és törlése hello tömb, figyelési rendszer használati és, fogadására és vírus futó vizsgálatokat végez, a virtuális tömbben.

### <a name="backups"></a>Biztonsági másolatok
a virtuális tömb hello adatok biztonsági mentésének toohello felhő két módon, egy alapértelmezett automatikus hello eszköz teljes kiindulási 22:30, vagy manuális igény szerinti biztonsági mentés napi biztonsági mentéshez. Alapértelmezés szerint hello eszköz automatikusan létrehozza a rajta található összes hello adatok napi felhőbeli pillanatképeket. További információ: túl[készítsen biztonsági másolatot a StorSimple virtuális tömb](storsimple-virtual-array-backup.md).

hello gyakoriság és megőrzési társított hello alapértelmezett biztonsági mentések nem módosítható, de be lehet állítani, mely hello napi biztonsági mentései minden nap kezdeményezett hello idő. Biztonsági mentések automatikus konfigurálásakor hello hello kezdési idejét, azt javasoljuk, hogy:

* A biztonsági mentések ütemezése essen. Biztonsági mentés kezdési ideje számos állomás IO nem kell egyeznie.
* Indítsa el igény szerinti manuális biztonsági mentés tooperform tervezése során egy eszköz feladatátvételi vagy előzetes toohello karbantartási időszak, a virtuális tömb tooprotect hello adatokat.

### <a name="restore"></a>Visszaállítás
Egy biztonságimentés-készlet két módon állíthatja vissza: tooanother kötetet vagy megosztást vissza, vagy az elemszintű helyreállítás (csak egy virtuális fájlkiszolgálóként konfigurált tömb elérhető). Elemszintű helyreállítás lehetővé teszi a fájlok és mappák minden hello megosztás felhőalapú biztonsági másolatából részletes helyreállítási toodo hello StorSimple eszközön. További információ: túl[állítsa vissza biztonsági másolatból](storsimple-virtual-array-clone.md).

A visszaállítás végrehajtásakor tartsa hello a következő irányelvek figyelembe vételével:

* A StorSimple virtuális tömb nem támogatja a védelem visszaállítása. Ez azonban könnyen lehet megvalósítani egy kétlépéses folyamat: Szabadítson fel helyet a virtuális hello tömb, és helyreállíthatja a tooanother kötetet vagy megosztást.
* Egy helyi kötet visszaállításakor tartsa szem előtt tartva hello restore lesz hosszú ideig futó művelet. Bár a hello kötet gyorsan ismét online elérhető, a hello adatai hello háttérben hidratált toobe továbbra is.
* hello kötet típus marad hello azonos hello visszaállítási folyamat során. A rétegzett kötetek helyreállítása tooanother rétegzett kötet, és egy helyileg rögzített kötet tooanother helyileg rögzített kötet.
* Ha próbált toorestore egy kötet vagy egy megosztást a biztonságimásolat-készletből, ha hello helyreállítási feladat sikertelen lesz, egy tároló kötetet vagy megosztást továbbra is létrehozható hello portálon. Fontos, törölje a nem használt célkötetet, vagy a megosztás hello portál toominimize ehhez az elemhez jövőbeli kérdéseket.

### <a name="failover-and-disaster-recovery"></a>Feladatátvétel és a katasztrófa utáni helyreállítás
Eszköz feladatátvevő toomigrate lehetővé teszi az adatokat egy *forrás* hello datacenter tooanother eszköz *cél* eszköz található hello azonos vagy különböző földrajzi elhelyezkedését. hello eszköz feladatátvétel során hello teljes eszközhöz. A feladatátvételi hello felhőbeli adatát hello forráseszközt hello target eszköz tulajdonjogának toothat változik.

A StorSimple virtuális tömb csak azonos hello által kezelt virtuális tömb tooanother átkapcsolás lehet StorSimple Manager szolgáltatás. Egy feladatátvevő tooan 8000 sorozat vagy (mint hello egy hello forrás eszköz) egy másik StorSimple Manager szolgáltatás által kezelt tömb nem engedélyezett. További információ az hello feladatátvételi szempontokat részletező cikket, toolearn túl nyissa meg[hello eszköz feladatátvételi Előfeltételek](storsimple-virtual-array-failover-dr.md).

Amikor keresztül hajtja végre a sikertelen a virtuális tömb, vegye figyelembe hello a következőket:

* A tervezett feladatátvételhez is ajánlott bevált gyakorlat az tootake összes hello kötetek vagy megosztások offline előzetes tooinitiating hello feladatátvételi. Útmutatás alapján hello operációsrendszer-specifikus tootake hello kötetek vagy megosztások offline hello gazdagép először, és majd azokat offline állapotba a virtuális eszközön.
* A fájl server vészhelyreállítás (DR) azt javasoljuk, hogy csatlakozik-e hello target eszköz toohello pedig automatikusan oldja fel ugyanabban a tartományban úgy, hogy a megosztás engedélyei hello hello forrásaként. Csak hello feladatátvételi tooa céleszköz hello ugyanabban a tartományban támogatott ebben a kiadásban.
* Ha hello vész-Helyreállítási sikeresen befejeződött, a rendszer automatikusan törli a hello forráseszközt. Bár a hello eszköz már nem érhető el, hello gazdarendszer kiépített hello virtuális gép továbbra is fogyassza az erőforrásokat. Azt javasoljuk, hogy törölje a virtuális gép a gazdagép rendszer tooprevent a a számoljanak fel Önnek a díjakat.
* Vegye figyelembe, hogy akkor is, ha hello feladatátvétel nem jár sikerrel, **hello adatok mindig biztonságos felhőben hello**. Vegye figyelembe a következő három forgatókönyv mely hello a feladatátvétel nem fejeződött be sikeresen hello:
  
  * Hiba történt a szakaszaiban hello kezdeti hello feladatátvételi, például amikor hello vész-Helyreállítási előtti a rendszer éppen ellenőrzi. Ebben a helyzetben a céleszközön továbbra is használható. Megpróbálkozhat a hello feladatátvétel hello a cél ugyanarra az eszközre.
  * Hiba történt a hello tényleges feladatátvételt folyamat során. Ebben az esetben hello céleszköz van megjelölve használható. Konfigurálnia kell kiépíteni és egy másik virtuális céltömb valamint, hogy a feladatátvételre használni.
  * hello feladatátvevő teljes hello forrás eszköz törölve lett a következő, de hello céleszköz problémákkal rendelkezik, és minden adat nem férhet hozzá. hello adatok hello felhőben továbbra is biztonságban, és könnyen olvasható egy másik virtuális tömb létrehozása és használata a vész-Helyreállítási hello a céleszközön.

### <a name="deactivate"></a>inaktiválása
Ha inaktiválja a StorSimple virtuális tömb, akkor Server hello kapcsolat hello eszköz és hello megfelelő StorSimple Manager szolgáltatás között. Az inaktiválást van egy **állandó** műveletet, és nem vonható vissza. Deaktivált eszköz nem lehet regisztrálni a StorSimple Manager szolgáltatás hello újra. További információ: túl[inaktiválja és törölje a StorSimple virtuális tömb](storsimple-virtual-array-deactivate-and-delete-device.md).

Tartsa szem előtt ajánlott eljárások a következő, amikor a virtuális tömb inaktiválása hello:

* Pillanatkép készítése felhő minden hello adatok előzetes toodeactivating egy virtuális eszközt. Ha egy virtuális tömb inaktiválja az összes hello helyi eszköz adat elvész. Felhő pillanatképének lehetővé teszi a toorecover adatok egy későbbi időpontban.
* A StorSimple virtuális tömb inaktiválása előtt győződjön meg arról, hogy toostop, vagy törölje az ügyfelek és kiszolgálók azokon az eszközökön.
* Deaktivált eszköz törlése, ha már nem használ, hogy azt nem keletkeznek költségek.

### <a name="monitoring"></a>Figyelés
amely a StorSimple virtuális tömb folyamatos állapota kifogástalan, tooensure toomonitor kell hello tömb, és győződjön meg arról, információt fogadni hello rendszer többek között a riasztásokat. toomonitor hello hello virtuális tömb, a következő gyakorlati tanácsok megvalósítása hello általános állapotát:

* Figyelési tootrack hello lemezhasználati a virtuális tömb adatlemez, valamint az operációsrendszer-lemez hello konfigurálása. Ha a Hyper-v-T futtató, használhatja a virtualizációs gazdagépek a System Center Virtual Machine Manager (SCVMM) és a System Center Operations Manager (SCOM) toomonitor kombinációja.
* E-mail értesítések beállítása a virtuális tömb toosend riasztások bizonyos használati szinten.                                                                                                                                                                                                

### <a name="index-search-and-virus-scan-applications"></a>Index keresési és vírus scan alkalmazások
A StorSimple virtuális tömb automatikusan is réteg hello helyi rétegen toohello Microsoft Azure felhőbe adatait. Ha egy alkalmazás, például egy index keresési vagy víruskeresést a StorSimple tárolt használt tooscan hello adatokat, tootake gondot, hogy hello felhőbeli adatát nem beolvasása érhető el, és vissza toohello helyi rétegen lekért szüksége.

Azt javasoljuk, hogy a virtuális tömb hello keresési vagy vírus indexvizsgálatot konfigurálásakor a következő gyakorlati tanácsok hello megvalósítása:

* Tiltsa le a teljes vizsgálat automatikusan konfigurált műveleteket.
* A rétegzett kötetek hello index keresési vagy vírus vizsgálat alkalmazás tooperform egy növekményes vizsgálat konfigurálása. Ez lenne ellenőrizni, hogy csak hello új adatok valószínűleg elhelyezkedhet hello helyi rétegen. rétegzett toohello felhő hello adatok növekményes művelet közben nem érhető el.
* Győződjön meg arról, hello helyes keresési szűrők és beállítások vannak konfigurálva, hogy csak olyan hello fájltípusok beolvasott beolvasása. Például képfájlok (JPEG, GIF és TIFF), és műszaki osztály rajzok nem kell futtatni hello növekményes és teljes index rebuild során.

Windows indexelése folyamat használata esetén kövesse az alábbiakat:

* Ne használjon Windows indexelő hello rétegzett kötetek, akkor visszahívja nagy mennyiségű adat (több TB-nyi) hello felhőből Ha hello index újraépítése gyakran toobe. Hello index újraépítése volna az összes fájl típusok tooindex azok tartalmának lekéréséhez.
* A helyileg rögzített kötetekhez folyamat indexelő, ez csak elérésére hello helyi rétegeken toobuild hello index (hello felhő adatok nem érhető el) lévő adatok hello Windows használja.

### <a name="byte-range-locking"></a>Bájt tartomány zárolása
Alkalmazások zárolhatja a bájtban megadott címtartomány hello fájlok belül. Ha bájt tartomány zárolás engedélyezve van a tooyour StorSimple írás hello alkalmazásokat, majd rétegezéséhez nem működik a virtuális tömbjét. A hello rétegezési toowork hello fájlok érhető el minden területéhez nem lehet zárolt. A virtuális tömb a rétegzett kötetek bájt tartomány zárolása nem támogatott.

Ajánlott intézkedések tooalleviate bájt tartomány zárolás tartalmazza:

* Kapcsolja ki a bájttartomány zárolás a az alkalmazás logikáját.
* Használjon helyileg rögzített kötetek (ahelyett, hogy a Szintezett) a jelen alkalmazáshoz rendelt hello adatokhoz. Helyileg rögzített kötetek nem réteg hello felhőbe.
* Ha helyileg használatával rögzített kötetek bájt tartomány zárolás engedélyezve van, hello kötet származhatnak online hello visszaállítás befejezése előtt. Ezekben az esetekben meg kell várnia a hello visszaállítási toobe befejeződött.

## <a name="multiple-arrays"></a>Több tömbök
Több virtuális tömbök telepített toobe tooaccount szükséges adatok, amelyek így hatással lesznek a hello eszköz hello teljesítményét hello felhő sikerült kerülnek egy egyre bővülő munkakészlete. Ezekben az esetekben akkor ajánlott tooscale eszközök hello munkakészlet növekedésével. Ehhez szükséges egy vagy több hello helyszíni adatközpontban hozzáadott eszközök toobe. Hello eszközök hozzáadásakor meg a következőket teheti:

* Vegyes hello aktuális beállítása az adatok.
* Telepítse az új munkaterhelések toonew őket.
* Ha több virtuális tömbök üzembe, azt javasoljuk, hogy a terheléselosztó szempontjából, hello tömb szét a különböző hipervizor-állomás.
* Több virtuális tömbök (Ha be van állítva, mint egy fájl vagy iSCSI-kiszolgáló) olyan elosztott fájl rendszer Namespace is telepíthető. A részletes lépéseket lásd túl[elosztott fájl rendszer Namespace megoldást hibrid felhőalapú tárolás telepítési útmutató](https://www.microsoft.com/download/details.aspx?id=45507). Az elosztott fájlrendszer replikációs jelenleg nem ajánlott hello virtuális tömb való használatra. 

## <a name="see-also"></a>Lásd még:
Ismerje meg, hogyan túl[felügyelete a StorSimple virtuális tömb](storsimple-virtual-array-manager-service-administration.md) hello StorSimple Manager szolgáltatás segítségével.

