---
title: "aaaStorSimple virtuális tömb frissítés 0,6 kibocsátási megjegyzései |} Microsoft Docs"
description: "Ismerteti, kritikus nyílt problémák és megoldások hello StorSimple virtuális tömb futtatásához 0,6 frissítése."
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
ms.date: 05/24/2017
ms.author: alkohli
ms.openlocfilehash: 7b573457dc07ce41e95d49d66ccc174045f31a42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-virtual-array-update-06-release-notes"></a>A StorSimple virtuális tömb frissítés 0,6 kibocsátási megjegyzései

## <a name="overview"></a>Áttekintés

hello alábbi kibocsátási megjegyzések hello kritikus megnyitott problémák azonosítása és a Microsoft Azure StorSimple virtuális tömb frissítéseket megoldott problémák hello.

hello kibocsátási megjegyzések folyamatosan frissülnek, és megoldást igénylő kritikus fontosságú problémáit ismertté hozzáadásuk után. Központi telepítéséhez a StorSimple virtuális tömb gondosan tekintse át a kibocsátási megjegyzések hello lévő hello információt.

Frissítés 0,6 megfelel toohello szoftververzió **10.0.10293.0**.

> [!IMPORTANT]
> - Frissítések zavaró, és indítsa újra az eszközt. Ha az i/o van folyamatban, hello eszköz azt eredményezi azok, háromszorosa a leállás. Részletes útmutató hogyan tooapply hello frissítés, nyissa meg túl[0,6. számú frissítés](storsimple-virtual-array-install-update-06.md).
>
> - Határozottan javasoljuk, hogy azonnal tartalmaz, így a kritikus fontosságú biztonsági javítások 0,6 frissítés telepítése.


## <a name="whats-new-in-hello-update-06"></a>What's new in hello 0,6 frissítés
Frissítés 0,6 fontos frissítése, és azonnal kell telepíteni. A frissítés tartalmazza a következő javítások hello: 

- **Windows biztonsági javítások** -ebben a kiadásban rendelkezik **Windows kritikus fontosságú biztonsági javítások**. Tekintse át a biztonsági frissítések hello biztonsági hibákkal kapcsolatos további információt a következő hello és társított javítások hello:
    - [2016. december csak minőségi biztonsági frissítés a Windows 8.1 és Windows Server 2012 R2](https://support.microsoft.com/help/3205400/december-2016-security-only-quality-update-for-windows-8.1-and-windows-server-2012-r2)
    - [Március 2017 csak minőségi biztonsági frissítés a Windows 8.1 és Windows Server 2012 R2](https://support.microsoft.com/help/4012213/march-2017-security-only-quality-update-for-windows-8-1-and-windows-server-2012-r23)
    - [2017. Előfordulhat, hogy 9 – KB4019213 (csak biztonsági frissítés)](https://support.microsoft.com/help/4019213/windows-8-update-kb4019213)

- **Állítsa vissza a javítás** -a korábbi kiadásokban merült fel a hiba, amely megakadályozná a hello visszaállítás befejezését. Ez a hiba kijavítása ebben a kiadásban.


## <a name="issues-fixed-in-hello-update-06"></a>A frissítés 0,6 hello megoldott problémák

hello következő táblázat az összefoglalást tartalmazza az ebben a kiadásban javított problémák.

| Nem. | Szolgáltatás | Probléma |
| --- | --- | --- |
| 1 |Biztonság| Ez a kiadás fontos Windows biztonsági frissítéseket tartalmaz. Javasoljuk, hogy a frissítés azonnal telepítését.|
| 2 |Visszaállítás| A visszaállítás során hiba történt, amely megakadályozná a hello visszaállítási feladat befejezését versenyhelyzet. hello hibajavítás a versenyhelyzet megoldást.|


## <a name="known-issues-in-hello-update-06"></a>A frissítés 0,6 hello ismert problémák

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
| **17.** |Frissítések |Ha a hiba kódja: 2359302 (hexadecimális 0x240006) keresztül gyorsjavítást tooinstall közben hello helyi felhasználói felület, akkor ez azt jelenti, hogy hello gyorsjavítás már telepítve van az eszközön.   | |

## <a name="next-step"></a>Következő lépés
[A frissítés 0,6](storsimple-virtual-array-install-update-06.md) a StorSimple virtuális tömbben.

## <a name="references"></a>Referencia
Egy régebbi kiadási Megjegyzés keres? Ugrás:

* [A StorSimple virtuális tömb frissítés 0,5 kibocsátási megjegyzései](storsimple-virtual-array-update-05-release-notes.md)
* [A StorSimple virtuális tömb frissítés 0,4 kibocsátási megjegyzései](storsimple-virtual-array-update-04-release-notes.md)
* [A StorSimple virtuális tömb frissítés 0,3 kibocsátási megjegyzései](storsimple-ova-update-03-release-notes.md)
* [A StorSimple virtuális tömb frissítés 0,1 és 0,2 kibocsátási megjegyzései](storsimple-ova-update-01-release-notes.md)
* [A StorSimple virtuális tömb általános rendelkezésre állási kibocsátási megjegyzései](storsimple-ova-pp-release-notes.md)

