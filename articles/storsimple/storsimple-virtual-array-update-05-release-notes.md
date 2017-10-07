---
title: "aaaStorSimple virtuális tömb frissítés 0,5 kibocsátási megjegyzései |} Microsoft Docs"
description: "Ismerteti, kritikus nyílt problémák és megoldások hello StorSimple virtuális tömb futtatásához 0,5 frissítése."
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
ms.date: 05/08/2017
ms.author: alkohli
ms.openlocfilehash: a249e2e8f0105dcf6cc02d3136dfa3e37ea615d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-virtual-array-update-05-release-notes"></a>A StorSimple virtuális tömb frissítés 0,5 kibocsátási megjegyzései

## <a name="overview"></a>Áttekintés

hello alábbi kibocsátási megjegyzések hello kritikus megnyitott problémák azonosítása és a Microsoft Azure StorSimple virtuális tömb frissítéseket megoldott problémák hello.

hello kibocsátási megjegyzések folyamatosan frissülnek, és megoldást igénylő kritikus fontosságú problémáit ismertté hozzáadásuk után. Központi telepítéséhez a StorSimple virtuális tömb gondosan tekintse át a kibocsátási megjegyzések hello lévő hello információt.

Frissítés 0,5 megfelel toohello szoftververzió **10.0.10290.0**.

> [!NOTE]
> Frissítések zavaró, és indítsa újra az eszközt. Ha az i/o van folyamatban, hello eszköz azt eredményezi azok, háromszorosa a leállás. Részletes útmutató hogyan tooapply hello frissítés, nyissa meg túl[0,5. számú frissítés](storsimple-virtual-array-install-update-05.md).


## <a name="whats-new-in-hello-update-05"></a>What's new in hello 0,5 frissítés
Frissítés 0,5 elsősorban hibajavítás build. hello fő fejlesztést és hibajavítások a következők:

- **Biztonsági mentés rugalmassági fejlesztései** -ebben a kiadásban rendelkezik, amelyek javítják a biztonsági mentési rugalmassági hello javításokat. Hello a korábbi kiadásokban biztonsági másolatok csak bizonyos kivételek lejárt. Ebben a kiadásban újrapróbálja hello minden biztonsági mentés kivételt, és elérhetővé válnak hello rugalmasabb, biztonsági másolatok.

- **Tárhely-használat figyelését frissítései** -2017. június 30.-től kezdődően hello tárhely-használat figyelését a StorSimple virtuális eszköz adatsorozat rendszerből. Ez vonatkozik az összes hello virtuális tömbök Update futtatása 0,4 vagy alacsonyabb diagramok figyelési toohello. A frissítés akkor toocontinue hello használatának ellenőrzése hello Azure-portálon tárhelyhasználatot szükséges hello módosításokat tartalmaz. Ez a frissítés kritikus 2017. június 30. előtt toocontinue hello hálózatfigyelési szolgáltatást használ.


## <a name="issues-fixed-in-hello-update-05"></a>A frissítés 0,5 hello megoldott problémák

hello következő táblázat az összefoglalást tartalmazza az ebben a kiadásban javított problémák.

| Nem. | Szolgáltatás | Probléma |
| --- | --- | --- |
| 1 |Biztonsági mentési rugalmasság| Hello a korábbi kiadásokban biztonsági másolatok csak bizonyos kivételek lejárt. Ebben a kiadásban a javítás toomake biztonsági másolatait tartalmazza rugalmasabb, minden hello biztonsági kivétel ismételt megpróbálásával.|
| 2 |Figyelés| hello tárhely-használat figyelését a StorSimple virtuális eszköz adatsorozathoz elavulttá válik 2017. június 30 indítása. Ez a művelet hatással van a hello StorSimple Device Manager szolgáltatás fut a StorSimple virtuális tömbök diagramok figyelési hello (1200-as modell). Ebben a kiadásban frissítést tartalmaz, amely a tárhely-használat figyelését túl 2017. június 30 hello virtuális tömbökön hello felhasználói toocontinue hello használatának engedélyezése.|
| 3 |Fájlkiszolgáló| Hello a korábbi kiadásokban a felhasználó véletlenül másolja a titkosított fájlok toohello virtuális tömb. Ebben a kiadásban, amely nem teszi lehetővé a titkosított fájlok toovirtual tömb másolása javítást tartalmaz. Ha az eszköz meglévő titkosított fájlok előzetes toohello frissítése, biztonsági másolatok továbbra is toofail csak az összes hello titkosított fájlok törlése hello rendszerből. |


## <a name="known-issues-in-hello-update-05"></a>A frissítés 0,5 hello ismert problémák

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
| **8.** |Használható kapacitás megosztások |Előfordulhat, hogy látja fogyasztás megosztani, ha nincs adat hello megosztáson. A felhasználási oka, hogy a megosztások hello használt kapacitás metaadatokat tartalmaz. | |
| **9.** |Vészhelyreállítás |Csak egy fájl server toohello hello vészhelyreállítást hajthatja végre, amely hello forráseszközt ugyanabban a tartományban. Ebben a kiadásban nem támogatott katasztrófa utáni helyreállítás tooa céleszközön egy másik tartományban. |Ez egy későbbi kiadásban van megvalósítva. További információ: túl[a StorSimple virtuális tömb a feladatátvételi és katasztrófa-helyreállítás](storsimple-virtual-array-failover-dr.md) |
| **10.** |Azure PowerShell |a StorSimple virtuális eszköz hello hello Azure PowerShell ebben a kiadásban nem kezelhetők. |Minden hello felügyeleti hello virtuális eszközök használatával kell megoldani hello Azure portál és hello helyi webes felhasználói felület. |
| **11.** |A jelszó módosítása |hello virtuális tömb eszköz konzol csak fogad bemeneti az en-us billentyűzet formátumban. | |
| **12.** |CHAP |CHAP hitelesítő adatok létrehozása után nem lesznek eltávolítva. Emellett ha hello CHAP hitelesítő adatok módosításához tootake hello kötetek offline állapotba kell és majd kapcsolja őket online tootake hatás hello módosítása. |Ezzel a problémával egy későbbi kiadásban. |
| **13.** |az iSCSI-kiszolgáló |"A tárhelyet használja" jelenik meg az iSCSI-kötet hello hello StorSimple Device Manager szolgáltatás és az iSCSI-gazdagép hello eltérőek lehetnek. |hello iSCSI-gazdagép hello filesystem nézet tartozik.<br></br>hello eszköz állapotában hello kötet hello maximális méret lefoglalt hello blokkok látja. |
| **14.** |Fájlkiszolgáló |Ha egy fájlt egy mappában van egy alternatív Data Stream (ADS) társítva, hello ADS nem biztonsági mentése vagy visszaállítása vész-helyreállítási, a Klónozás és az elem szintű helyreállítás. | |
| **15.** |Fájlkiszolgáló |Nem támogatja a szimbolikus csatolást. | |
| **16.** |Fájlkiszolgáló |Fájlok védett által Windows titkosított fájlrendszer (EFS) keresztül másolását vagy tárolt hello StorSimple virtuális tömb fájl server eredmény beállításai nem támogatottak.  | |

## <a name="next-step"></a>Következő lépés
[A frissítés 0,5](storsimple-virtual-array-install-update-05.md) a StorSimple virtuális tömbben.

## <a name="references"></a>Referencia
Egy régebbi kiadási Megjegyzés keres? Ugrás:

* [A StorSimple virtuális tömb frissítés 0,4 kibocsátási megjegyzései](storsimple-virtual-array-update-04-release-notes.md)
* [A StorSimple virtuális tömb frissítés 0,3 kibocsátási megjegyzései](storsimple-ova-update-03-release-notes.md)
* [A StorSimple virtuális tömb frissítés 0,1 és 0,2 kibocsátási megjegyzései](storsimple-ova-update-01-release-notes.md)
* [A StorSimple virtuális tömb általános rendelkezésre állási kibocsátási megjegyzései](storsimple-ova-pp-release-notes.md)

