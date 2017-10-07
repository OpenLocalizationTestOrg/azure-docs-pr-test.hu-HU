---
title: "aaaStorSimple virtuális tömb kibocsátási megjegyzései |} Microsoft Docs"
description: "Kritikus megnyitott problémák és megoldások a StorSimple virtuális tömb hello ismerteti."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 84908160-2b8b-4f4f-a674-f39aaa0bd4de
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/13/2016
ms.author: alkohli
ms.openlocfilehash: ca7b543f95cf5787b7fef39f53887161ebfa7fcb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-virtual-array-release-notes"></a>A StorSimple virtuális tömb kibocsátási megjegyzései
## <a name="overview"></a>Áttekintés
hello alábbi kibocsátási megjegyzések hello kritikus megnyitott problémák azonosításához hello 2016. március általános elérhetőségével (GA) kiadása a Microsoft Azure StorSimple virtuális tömb (más néven hello helyszíni StorSimple virtuális eszköz vagy a StorSimple virtuális hello hello eszközt használ). Ebben a kiadásban toosoftware verzió 10.0.10271.0 felel meg.

hello kibocsátási megjegyzések folyamatosan frissülnek, és megoldást igénylő kritikus fontosságú problémáit ismertté hozzáadásuk után. A StorSimple virtuális eszköz telepítése előtt gondosan tekintse át a kibocsátási megjegyzések hello lévő hello információt. 

hello a következő táblázat összefoglalja az ismert problémákról, ebben a kiadásban.

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

