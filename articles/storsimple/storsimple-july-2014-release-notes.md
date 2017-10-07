---
title: "aaaStorSimple 8000 kibocsátási verziója kibocsátási megjegyzései |} Microsoft Docs"
description: "Hello új funkciókat, nyissa meg problémáit és hello elérhető megoldásai 2014. július mutatja be a Microsoft Azure StorSimple release."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 12f1796e-37c3-42b4-b997-a84fc1950c20
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/18/2016
ms.author: v-sharos
ms.openlocfilehash: 74863a3e2811dc7be5e6f482a5be4bbc37e3cd71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-release-version-release-notes---july-2014"></a>A StorSimple 8000 Series kiadásával kibocsátási megjegyzések – 2014. július
## <a name="overview"></a>Áttekintés
hello kövesse kibocsátási megjegyzések a StorSimple 8000 Series hello megnyitott problémák kritikus hello meghatározása a Microsoft Azure StorSimple 2014. július általános elérhetőségével (GA) kiadásában. Ebben a kiadásban toosoftware verzió 6.3.9600.17215 felel meg.  

Hacsak másként nincs megadva, a kibocsátási megjegyzéseket tooall modellek hello StorSimple eszköz alkalmazni. hello kibocsátási megjegyzések folyamatosan frissülnek; megoldást igénylő kritikus fontosságú problémáit ismertté hozzáadásuk után. Központi telepítéséhez a Microsoft Azure StorSimple megoldáshoz fontolja meg a következő információ hello.  

## <a name="known-issues-in-this-release"></a>Ebben a kiadásban ismert problémák
hello a következő táblázat összefoglalja az ismert problémákról, ebben a kiadásban.  

| Nem. | Szolgáltatás | Probléma | Megjegyzések/megoldás | Érvényes toophysical eszköz | Érvényes toovirtual eszköz |
| --- | --- | --- | --- | --- | --- |
| 1 |Gyári beállítások visszaállítása |Bizonyos esetekben a gyári beállítások visszaállítása, végrehajtásakor hello StorSimple eszköz előfordulhat, hogy, és megjeleníti ezt az üzenetet: **toofactory alaphelyzetbe állítása folyamatban van a (fázis 8)**. Ez akkor fordul elő, ha a CTRL + C gombra, amíg hello parancsmag folyamatban van. |CTRL + C nem nyomja meg a gyári beállítások visszaállítása kezdeményezése után. Ha már ebben az állapotban, forduljon a Microsoft Support további lépéseket. |Igen |Nem |
| 2 |Lemez kvórum |Ritka esetekben ha hello EBOD szolgáltatással 8600-eszköz lemezek hello többsége megszakadt a kapcsolat nincs lemez kvórum eredményezi majd hello tárolókészlet sem kapcsolódik a hálózathoz. Offline állapotban maradnak, még akkor is, ha hello lemez újracsatlakoztatását. |Szüksége lesz tooreboot hello eszköz. Ha hello a probléma továbbra is fennáll, forduljon a Microsoft Support további lépéseket. |Igen |Nem |
| 3 |Felhőalapú pillanatfelvétel hibák |Ritka esetekben egy felhő-pillanatfelvételt hello hiba miatt meghiúsulhat **elérte a maximális biztonsági korlátot**. Ez akkor fordul elő, ha az túllépi a hello 255 online klónok ugyanarra az eszközre, a hello azonos eredeti kötet, amelyet már töröltek. | |Igen |Igen |
| 4 |Helytelen vezérlő azonosítója |A vezérlő helyettesítő végrehajtásakor vezérlő 0 lehet, hogy megjelennek, a vezérlő 1. A tartományvezérlő csere közben hello kép hello társ-csomópont betöltésekor hello vezérlő azonosítója is megjelennek kezdetben, hello társ vezérlő azonosítója. Ritka esetben ez a viselkedés előfordulhat, hogy is látható. a rendszer újraindítását követően. |Nincs felhasználói beavatkozásra szükség. Ez a helyzet magától megoldódik, amikor hello vezérlő helyettesítő befejeződése után. |Igen |Nem |
| 5 |Eszközfigyelési diagramok |A StorSimple Manager szolgáltatás hello hello eszköz figyelési diagramok nem működnek a Basic, vagy az NTLM-hitelesítés nincs engedélyezve az hello proxykiszolgálót hello eszközhöz. |Hello webproxy konfigurálása a StorSimple Manager szolgáltatásban regisztrált úgy, hogy a hitelesítés be van állítva tooNONE hello eszköz módosításához. toodo StorSimple Set-HcsWebProxy parancsmag, ez futtatási hello hello Windows PowerShell. |Igen |Igen |
| 6 |Tárfiókok |Hello tárolási toodelete hello tárolási szolgáltatásfiók használata egy nem támogatott forgatókönyvet. Ez vezet tooa helyzetet, amelyben felhasználói nem lehet adatokat beolvasni. | |Igen |Igen |
| 7 |Feladat-visszavétel |A feladat-visszavétel vész-helyreállítási 24 órán belül nem támogatott. | |Igen |Nem |
| 8 |Eszköz feladatátvétel |A kötettároló hello azonos forrás eszköz toodifferent Céleszközök nem támogatja a több feladatátvétele. Egy egyetlen elhalt eszközt toomultiple eszközökről feladatátvételi hello kötettárolók először átadja a feladatokat eszköz hello az adatok tulajdonjoga elvesztése biztosítják. Egy feladatátvétel után ezek kötettárolók jelenik meg, vagy a klasszikus Azure portálon hello meg eltérően viselkednek. | |Igen |Nem |
| 9 |Telepítés |StorSimple Adapter SharePoint telepítéshez, során szüksége tooprovide egy eszköz IP-cím hello telepítés toofinish sikeresen. | |Igen |Nem |
| 10 |Hálózati illesztők |DATA 2 és a DATA 3 hálózati illesztők hello szoftverfrissítési lettek cserélve. |Ha tooconfigure konfigurációkezelővel van szüksége, forduljon a Microsoft Support. |Igen |Nem |

