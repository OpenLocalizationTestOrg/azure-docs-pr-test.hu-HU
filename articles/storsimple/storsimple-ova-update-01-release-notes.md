---
title: "aaaStorSimple virtuális tömb frissítések kibocsátási megjegyzései |} Microsoft Docs"
description: "Ismerteti a kritikus megnyitott problémák és megoldások a StorSimple virtuális tömb hello 0,2 és 0,1 Update futtatása."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 3993864d-2ddd-4302-a2f1-8d737fba6eab
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/16/2016
ms.author: alkohli
ms.openlocfilehash: dfd38890feeb667c95134f2adbb35ce2df165620
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-virtual-array-update-02-and-01-release-notes"></a>A StorSimple virtuális tömb frissítés 0,2 és 0,1 kibocsátási megjegyzései
## <a name="overview"></a>Áttekintés
hello alábbi kibocsátási megjegyzések hello kritikus megnyitott problémák azonosítása és a Microsoft Azure StorSimple virtuális tömb frissítéseket megoldott problémák hello. (A Microsoft Azure StorSimple virtuális tömb, de más néven hello StorSimple helyszíni virtuális eszköz hello StorSimple virtuális eszköz.) 

hello kibocsátási megjegyzések folyamatosan frissülnek, és megoldást igénylő kritikus fontosságú problémáit ismertté hozzáadásuk után. A StorSimple virtuális eszköz telepítése előtt gondosan tekintse át a kibocsátási megjegyzések hello lévő hello információt.

Frissítés 0,2 megfelel toohello szoftververzió **10.0.10280.0**; Frissítés 0,1 verziója **10.0.10279.0**. hello lentebbi listában minden egyes frissítés hello módosításokat. 

> [!NOTE]
> Frissítések zavaró és az eszköz újraindul. Ha az i/o van folyamatban, hello eszköz állásidő gyakorisága.
> 
> 

## <a name="issues-fixed-in-hello-update-02"></a>A frissítés 0,2 hello megoldott problémák
Frissítés 0,2 hozzáadása toohello javítás hello a következő táblázatban leírt összes módosítást a Update 0,1 tartalmazza:

| Szolgáltatás | Probléma |
| --- | --- |
| Frissítések |Hello utolsó kiadásban frissítések nem észlelt automatikusan hello klasszikus Azure portálon, így volt a helyi webes felhasználói felületén tooinstall frissítések hello toouse. Ez a probléma fennáll ebben a kiadásban. 0,2 frissítés telepítése után a jövőbeli frissítések hello klasszikus Azure portál használatával is telepítheti. |

## <a name="whats-new-in-hello-update-01"></a>What's new in hello 0,1 frissítés
Frissítés 0,1 tartalmaz hello következő hibajavításokat és fejlesztéseket. 

* **A felhő kimaradások jobb rugalmasság**: Ez a kiadás számos hibajavítást katasztrófa utáni helyreállítás, biztonsági mentés, visszaállítás és hello eseményeket, a felhő kapcsolat szüneteltetése rétegezéséhez rendelkezik. 
* **Továbbfejlesztett visszaállítási teljesítmény**: Ebben a kiadásban rendelkezik, amely rendelkezik jelentősen kevesebb hello visszaállítási feladat befejezési időpontja hello hibajavításokat tartalmaz.
* **Automatikus terület-visszanyerést optimalizálási**: adatok törlésekor a dinamikusan kiosztott kötetek hello nem használt tárolási címblokkokat kell visszaigényelt toobe. Ebben a kiadásban továbbfejlesztett hello terület visszaigénylését folyamattal rendelkeznek a nem használt hello eredményezve hello felhőből válik elérhető gyorsabban az összehasonlított toohello korábbi verzióinak szóköz.
* **Új virtuális lemez képek**: új virtuális merevlemez VHDX és VMDK is elérhető hello a klasszikus Azure portálon keresztül. A lemezképek tooprovision új frissítés 0,1 eszközök töltheti le.
* **Feladatok állapotának hello portálon hello pontosságának javítása**: hello a szoftver, feladat állapota hello portál jelentéseihez korábbi verziója nem lett a részletes. Ebben a kiadásban a probléma megoldásához.
* **Tartományi csatlakozási élmény**: hibajavításokat tartalmaz a kapcsolódó toodomain csatlakoztatása és hello eszköz átnevezése.

## <a name="issues-fixed-in-hello-update-01"></a>A frissítés 0,1 hello megoldott problémák
hello következő táblázat az összefoglalást tartalmazza az ebben a kiadásban javított problémák.

| Nem. | Szolgáltatás | Probléma |
| --- | --- | --- |
| 1 |VMDK-FÁJL |VMware verzióban hello operációsrendszer-lemezek ritka, amely riasztások és a normál működés megszakítása fordult elő. Ez volt a rögzített ebben a kiadásban. |
| 2 |az iSCSI-kiszolgáló |Hello utolsó kiadásban hello felhasználó volt szükséges toospecify a StorSimple virtuális eszköz engedélyezett hálózati csatolóhoz átjáró. Ez a viselkedés ebben a kiadásban megváltozott, így hello felhasználó rendelkezik-e tooconfigure legalább egy átjáró összes hello engedélyezve van a hálózati adaptereken. |
| 3 |Támogatási csomag |A szoftver korábbi verzióját hello, támogatja a csomag gyűjtemény hello csomag mérete 1 GB-nál nagyobb volt sikertelen volt. Ez a probléma fennáll ebben a kiadásban. |
| 4 |Felhő hozzáférés |Hello utolsó kiadásban Ha hello StorSimple virtuális tömb nem rendelkezett a hálózati kapcsolatot, és újra lett indítva, hello helyi felhasználói felület kellene kapcsolódási problémák. Ez a probléma javítását az ebben a kiadásban. |
| 5 |Figyelési diagramok |Hello korábbi változatban, egy eszköz feladatátvételt követő hello felhő kapacitás kihasználtságát diagramok helytelen értékeket a klasszikus Azure portálon hello jelenik meg. A jelenlegi kiadásban hello rögzített. |

## <a name="known-issues-in-hello-update-01"></a>A frissítés 0,1 hello ismert problémák
hello következő táblázat összefoglalja az ismert problémákról a StorSimple virtuális tömb hello és kiadás feljegyzett hello korábbi verzióiról hello problémák tartalmazza. **hello problémák kiadás ebben a kiadásban nincs jelezve vannak csillaggal jelölt. A listában szereplő szinte minden hello problémák hello GA kiadás a StorSimple virtuális tömb átvitt rendelkezik.**

| Nem. | Szolgáltatás | Probléma | Megkerülő megoldás/megjegyzések |
| --- | --- | --- | --- |
| **1.** |Frissítések |hello előzetes létrehozott virtuális eszközök hello támogatott frissített tooa általánosan rendelkezésre álló verzió nem lehet. |A virtuális eszközök kell feladatátvételt hello általánosan rendelkezésre álló kiadási vész-helyreállítási munkafolyamat használatát. |
| **2.** |Kiépített adatlemez |A megadott méretű adatlemezt kiépítése után, és hello megfelelő StorSimple virtuális eszköz létrehozása kell nem bontsa ki vagy hello adatlemez zsugorítani. Toodo kísérlet így eredményez az összes hello adat a helyi rétegeken hello hello eszköz. | |
| **3.** |Csoportházirend |Ha egy eszköz a tartományhoz, a Csoportházirend alkalmazása kedvezőtlen hatással lehet hello eszköz műveletet. |Győződjön meg arról, hogy a virtuális tömb saját szervezeti egységben (OU) az Active Directory és nem a csoportházirend-objektumok (GPO) alkalmazott tooit. |
| **4.** |Helyi webes felhasználói felület |A fokozott biztonság funkció engedélyezése az Internet Explorerben (IE ESC), előfordulhat, hogy egyes helyi felhasználói felület weboldalak például hibaelhárítás vagy karbantartási nem működik megfelelően. Gombok ezeken a lapokon nem is működnek. |Kapcsolja ki az Internet Explorer fokozott biztonsági funkciók. |
| **5.** |Helyi webes felhasználói felület |A Hyper-V virtuális gépen a hálózati illesztőket hello webes felhasználói felület, 10 GB/s-felületek jelennek meg, hogy a hello. |Ez a viselkedés a Hyper-V tükre. Hyper-V mindig látható a virtuális hálózati adapterek 10 GB/s. |
| **6.** |Rétegzett kötetek vagy megosztások |Az alkalmazások, amelyek használhatók a hello StorSimple rétegzett kötetek zárolását bájttartomány nem támogatott. Ha bájt tartomány zárolás engedélyezve van, a StorSimple rétegezéséhez nem fog működni. |Ajánlott mértékeket tartalmazza: <br></br>Kapcsolja ki a bájttartomány zárolás a az alkalmazás logikáját.<br></br>Adja meg az alkalmazás tooput adatok helyileg rögzített kötetekhez mint tootiered kötetek.<br></br>*Ismeret*: Ha használatával helyileg rögzített kötetek, és engedélyezve van az bájt tartomány zárolás, vegye figyelembe, hogy hello helyileg rögzített kötet lehet online még hello visszaállítás befejezése előtt. Ezekben az esetekben ha a helyreállítás van folyamatban, majd várnia kell hello visszaállítási toocomplete. |
| **7.** |Rétegzett megosztások |Nagy fájlok használata a kimenő lassú réteg eredményezheti. |Munka nagy fájlok esetében azt javasoljuk, hogy hello legnagyobb fájl hello a fájlmegosztás méretének % 3-nál kisebb. |
| **8.** |Használható kapacitás megosztások |Előfordulhat, hogy látja fogyasztás megosztása hello hiányában hello megosztáson lévő adatokat. Ennek az az oka hello használt kapacitás megosztások metaadatokat tartalmaz. | |
| **9.** |Vészhelyreállítás |Csak egy fájl server toohello hello vészhelyreállítást hajthatja végre, amely hello forráseszközt ugyanabban a tartományban. Ebben a kiadásban nem támogatott katasztrófa utáni helyreállítás tooa céleszközön egy másik tartományban. |Ez egy későbbi kiadásban hajtják végre. |
| **10.** |Azure PowerShell |a StorSimple virtuális eszköz hello hello Azure PowerShell ebben a kiadásban nem kezelhetők. |Minden hello felügyeleti hello virtuális eszközök hello a klasszikus Azure portálon és hello helyi webes felhasználói felületen keresztül kell végezni. |
| **11.** |A jelszó módosítása |hello virtuális tömb eszköz konzol csak en-US billentyűzet formátumú bemenetet fogad el. | |
| **12.** |CHAP |CHAP hitelesítő adatok létrehozása után nem lesznek eltávolítva. Emellett ha hello CHAP hitelesítő adatok módosításához fog tootake hello kötetek offline állapotba kell és majd kapcsolja őket online tootake hatás hello módosítása. |Ezek kiiktatása a későbbi kiadásra. |
| **13.** |az iSCSI-kiszolgáló |"A tárhelyet használja" jelenik meg az iSCSI-kötet hello hello StorSimple Manager szolgáltatás és az iSCSI-gazdagép hello eltérőek lehetnek. |hello iSCSI-gazdagép hello filesystem nézet tartozik.<br></br>hello eszköz állapotában hello kötet hello maximális méret lefoglalt hello blokkok látja. |
| **14.** |Fájl server * |Ha egy fájlt egy mappában van egy alternatív Data Stream (ADS) társítva, hello ADS nem biztonsági mentése vagy visszaállítása vész-helyreállítási, a Klónozás és az elem szintű helyreállítás. | |

## <a name="next-step"></a>Következő lépés
[Frissítések telepítése](storsimple-ova-install-update-01.md) a StorSimple virtuális tömbben.

