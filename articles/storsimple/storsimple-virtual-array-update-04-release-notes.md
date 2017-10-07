---
title: "aaaStorSimple virtuális tömb frissítés 0,4 kibocsátási megjegyzései |} Microsoft Docs"
description: "Ismerteti, kritikus nyílt problémák és megoldások hello StorSimple virtuális tömb futtatásához 0,4 frissítése."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/05/2017
ms.author: alkohli
ms.openlocfilehash: 1fd174ff483f4f7b1b4a7853c9d9573d1e948cb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-virtual-array-update-04-release-notes"></a>A StorSimple virtuális tömb frissítés 0,4 kibocsátási megjegyzései

## <a name="overview"></a>Áttekintés

hello alábbi kibocsátási megjegyzések hello kritikus megnyitott problémák azonosítása és a Microsoft Azure StorSimple virtuális tömb frissítéseket megoldott problémák hello.

hello kibocsátási megjegyzések folyamatosan frissülnek, és megoldást igénylő kritikus fontosságú problémáit ismertté hozzáadásuk után. Központi telepítéséhez a StorSimple virtuális tömb gondosan tekintse át a kibocsátási megjegyzések hello lévő hello információt.

Frissítés 0,4 megfelel toohello szoftververzió **10.0.10289.0**.

> [!NOTE]
> Frissítések zavaró, és indítsa újra az eszközt. Ha az i/o van folyamatban, hello eszköz azt eredményezi azok, háromszorosa a leállás.


## <a name="whats-new-in-hello-update-04"></a>What's new in hello 0,4 frissítés
Frissítés 0,4 elsősorban néhány kiegészítésre alapján kialakulhat hibajavítás build. Ebben a verzióban hello korábbi verziójában biztonsági mentési hibák eredményezve több hibák rendelkezik foglalkoztak. hello fő fejlesztést és hibajavítások a következők:

- **Biztonsági mentés a teljesítményt javító** -ebben a kiadásban tett több kulcs fejlesztések tooimprove hello biztonsági mentés teljesítményét. Ennek eredményeképpen hello biztonsági mentések, például fájlok nagy számú lásd: hello idő toocomplete, teljes és növekményes biztonsági mentések jelentős csökkenését.

- **Visszaállítás hatékonyabb** -ebben a kiadásban továbbfejlesztett jelentősen fejleszti a hello visszaállítási teljesítményt, ha sok fájlt használ. 2 – 4 millió fájlokat használnak, azt javasoljuk, hogy egy virtuális tömb 16 GB RAM toosee hello javításait ellátásához. Ha kevesebb mint 2 millió fájlokkal, hello minimális rendszerkövetelményt a hello virtuális gép továbbra is toobe 8 GB RAM.

- **Fejlesztések tooSupport csomag** -hello fejlesztései közé tartozik a lemez, a Processzor, a memória, a hálózati és a felhő, ezáltal növelve az eszköz problémák diagnosztizálása/hibakeresési hello folyamat hello támogatási csomagba hello statisztikája van bejelentkezve.

- **Korlát helyileg rögzített iSCSI kötetek too200 GB** -a helyileg rögzített kötetekhez, azt javasoljuk, hogy korlátozza a StorSimple virtuális tömb tooa 200 GB-os iSCSI kötet. helyi foglalás hello rétegzett kötetek továbbra is fennáll, toobe 10 %-a hello kötetméretet kiosztott, de tárfiókonként maximum 200 GB-os. 

- **Biztonsági mentésével kapcsolatos hibajavítások** -korábbi verzióiban a szoftver kapcsolódó toobackups problémák, amelyek biztonsági mentési hibák történtek. Ezek a hibák rendelkezik foglalkoztak. Ebben a kiadásban.


## <a name="issues-fixed-in-hello-update-04"></a>A frissítés 0,4 hello megoldott problémák

hello következő táblázat az összefoglalást tartalmazza az ebben a kiadásban javított problémák.

| Nem. | Szolgáltatás | Probléma |
| --- | --- | --- |
| 1 |Biztonsági mentés teljesítményét|Hello a korábbi kiadásokban hello biztonsági mentések sok fájlt érintő időt vesz igénybe egy hosszú ideig toocomplete (sorrendben hello nap). Ebben a kiadásban hello teljes és növekményes biztonsági mentések is tekintse meg a hello idő toocompletion jelentős mértékben csökken. |
| 2 |Támogatási csomag|Lemez, a CPU, a memória, a hálózati és a felhő statisztikák most jelentkezett be toohello támogatási naplófájlok így hello támogatási csomag nagyon hatékonyan bármely eszköz problémák elhárításához.|
| 3 |Biztonsági mentés |A korábbi kiadásokban biztonsági másolatok hosszú ideig futó biztonsági mentési hibák eredményezve hello eszközön egy helyet crunch vezethet. Ezt a hibát egy adott időpontban legfeljebb 5 biztonsági mentések tooqueue tételével címzettjei ebben a kiadásban.|
| 4 |iSCSI | A korábbi kiadásokban hello helyi lefoglalás rétegzett vagy helyileg rögzített kötetek lett kiépítve hello kötet mérete 10 %-át. Ebben a kiadásban hello helyi lefoglalás az összes iSCSI-kötet (helyileg rögzített vagy Szintezett) korlátozva too10 % legfeljebb legfeljebb 200 GB-ot (nagyobb, mint 2 TB rétegzett kötetek) ezáltal felszabadítás fel további területet hello helyi lemezen. Azt javasoljuk, hogy ebben a kiadásban hello helyileg rögzített kötetek kell-e a korlátozott too200 GB.|


## <a name="known-issues-in-hello-update-04"></a>A frissítés 0,4 hello ismert problémák

hello következő táblázat összefoglalja az ismert problémákról a StorSimple virtuális tömb hello és kiadás feljegyzett hello korábbi verzióiról hello problémák tartalmazza. 

| Nem. | Szolgáltatás | Probléma | Megkerülő megoldás/megjegyzések |
| --- | --- | --- | --- |
| **1.** |Frissítések |hello előzetes létrehozott virtuális eszközök hello támogatott frissített tooa általánosan rendelkezésre álló verzió nem lehet. |A virtuális eszközök kell feladatátvételt hello általánosan rendelkezésre álló kiadási vész-helyreállítási munkafolyamat használatát. |
| **2.** |Kiépített adatlemez |A megadott méretű adatlemezt kiépítése után, és hello megfelelő StorSimple virtuális eszköz létrehozása kell nem bontsa ki vagy hello adatlemez zsugorítani. Kísérlet történt egy toodo hello helyi rétegeken hello eszköz összes hello adatok leállását eredményezi. | |
| **3.** |Csoportházirend |Ha egy eszköz a tartományhoz, a Csoportházirend alkalmazása kedvezőtlen hatással lehet hello eszköz műveletet. |Győződjön meg arról, hogy a virtuális tömb saját szervezeti egységben (OU) az Active Directory és nem a csoportházirend-objektumok (GPO) alkalmazott tooit. |
| **4.** |Helyi webes felhasználói felület |A fokozott biztonság funkció engedélyezése az Internet Explorerben (IE ESC), előfordulhat, hogy egyes helyi felhasználói felület weboldalak például hibaelhárítás vagy karbantartási nem működik megfelelően. Gombok ezeken a lapokon nem is működnek. |Kapcsolja ki az Internet Explorer fokozott biztonsági funkciók. |
| **5.** |Helyi webes felhasználói felület |A Hyper-V virtuális gépen a hálózati illesztőket hello webes felhasználói felület, 10 GB/s-felületek jelennek meg, hogy a hello. |Ez a viselkedés a Hyper-V tükre. Hyper-V mindig látható a virtuális hálózati adapterek 10 GB/s. |
| **6.** |Rétegzett kötetek vagy megosztások |Az alkalmazások, amelyek használhatók a hello StorSimple rétegzett kötetek zárolását bájttartomány nem támogatott. Ha bájt tartomány zárolás engedélyezve van, nem StorSimple rétegezéséhez működik. |Ajánlott mértékeket tartalmazza: <br></br>Kapcsolja ki a bájttartomány zárolás a az alkalmazás logikáját.<br></br>Adja meg az alkalmazás tooput adatok helyileg rögzített kötetekhez mint tootiered kötetek.<br></br>*Ismeret*: Ha engedélyezve van az bájt tartomány zárolás használatával helyileg rögzített kötetek, hello helyileg rögzített kötet lehet online még hello visszaállítás befejezése előtt. Ezekben az esetekben ha a helyreállítás van folyamatban, majd várnia kell hello visszaállítási toocomplete. |
| **7.** |Rétegzett megosztások |Nagy fájlok használata a kimenő lassú réteg eredményezheti. |Munka nagy fájlok esetében azt javasoljuk, hogy hello legnagyobb fájl hello a fájlmegosztás méretének % 3-nál kisebb. |
| **8.** |Használható kapacitás megosztások |Előfordulhat, hogy látja fogyasztás megosztani, ha nincs adat hello megosztáson. Ennek az az oka hello használt kapacitás megosztások metaadatokat tartalmaz. | |
| **9.** |Vészhelyreállítás |Csak egy fájl server toohello hello vészhelyreállítást hajthatja végre, amely hello forráseszközt ugyanabban a tartományban. Ebben a kiadásban nem támogatott katasztrófa utáni helyreállítás tooa céleszközön egy másik tartományban. |Ez egy későbbi kiadásban van megvalósítva. |
| **10.** |Azure PowerShell |a StorSimple virtuális eszköz hello hello Azure PowerShell ebben a kiadásban nem kezelhetők. |Minden hello felügyeleti hello virtuális eszközök hello a klasszikus Azure portálon és hello helyi webes felhasználói felületen keresztül kell végezni. |
| **11.** |A jelszó módosítása |hello virtuális tömb eszköz konzol csak en-US billentyűzet formátumú bemenetet fogad el. | |
| **12.** |CHAP |CHAP hitelesítő adatok létrehozása után nem lesznek eltávolítva. Emellett ha hello CHAP hitelesítő adatok módosításához tootake hello kötetek offline állapotba kell és majd kapcsolja őket online tootake hatás hello módosítása. |Ezzel a problémával egy későbbi kiadásban. |
| **13.** |az iSCSI-kiszolgáló |"A tárhelyet használja" jelenik meg az iSCSI-kötet hello hello StorSimple Manager szolgáltatás és az iSCSI-gazdagép hello eltérőek lehetnek. |hello iSCSI-gazdagép hello filesystem nézet tartozik.<br></br>hello eszköz állapotában hello kötet hello maximális méret lefoglalt hello blokkok látja. |
| **14.** |Fájlkiszolgáló |Ha egy fájlt egy mappában van egy alternatív Data Stream (ADS) társítva, hello ADS nem biztonsági mentése vagy visszaállítása vész-helyreállítási, a Klónozás és az elem szintű helyreállítás. | |
| **15.** |Fájlkiszolgáló |Nem támogatja a szimbolikus csatolást. | |
| **16.** |Fájlkiszolgáló |Fájlok védett által Windows titkosított fájlrendszer (EFS) keresztül másolását vagy tárolt hello StorSimple virtuális tömb fájl server eredmény beállításai nem támogatottak.  | |

## <a name="next-step"></a>Következő lépés
[A frissítés 0,4](storsimple-virtual-array-install-update-04.md) a StorSimple virtuális tömbben.

## <a name="references"></a>Referencia
Egy régebbi kiadási Megjegyzés keres? Ugrás: 

* [A StorSimple virtuális tömb frissítés 0,3 kibocsátási megjegyzései](storsimple-ova-update-03-release-notes.md)
* [A StorSimple virtuális tömb frissítés 0,1 és 0,2 kibocsátási megjegyzései](storsimple-ova-update-01-release-notes.md)
* [A StorSimple virtuális tömb általános rendelkezésre állási kibocsátási megjegyzései](storsimple-ova-pp-release-notes.md)

