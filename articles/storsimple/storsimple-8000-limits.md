---
title: "aaaStorSimple 8000 sorozat rendszer korlátok |} Microsoft Docs"
description: "Ismerteti a rendszer korlátozásai és a StorSimple 8000 series összetevők és kapcsolatok az ajánlott méreteket."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: c7392678-0924-46c6-9c59-1665cb9b6586
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/28/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: def89a2c1d0ddc71f1743e592c5112b09d72e971
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-storsimple-8000-series-system-limits"></a>A StorSimple 8000 series rendszer korlátai

## <a name="overview"></a>Áttekintés

A StorSimple méretezhető és rugalmas tárolási nyújt az adatközpontban. Van azonban néhány korlátot, érdemes figyelembe venni, tervezése, üzembe helyezéséhez és üzemeltetéséhez a a StorSimple megoldásban. hello következő táblázat ismerteti ezeket a határértékeket, továbbá néhány javaslatot is tartalmaz, hogy megkaphassa hello legtöbb kívül a StorSimple megoldásban.

| Korlátazonosító | Korlát | Megjegyzések |
| --- | --- | --- |
| Tárfiók hitelesítő adatainak maximális száma |64 | |
| Kötettárolók maximális száma |64 | |
| Kötetek maximális száma |255 | |
| Helyileg rögzített kötetek maximális száma |32 | |
| Ütemezésének per sávszélességsablon maximális száma |168 |Óránként, naponta (24 * 7) hello hét ütemezése. |
| A rétegzett kötetek fizikai eszközök maximális mérete |A 8100 és 8600 64 TB |8100 és 8600 különböző fizikai eszközök. |
| A virtuális eszközök, az Azure-ban a rétegzett kötetek maximális mérete |30 TB lehet a 8010-es <br></br> A 8020-as modell 64 TB |8010-es és a 8020-as modell olyan virtuális Azure használó eszközöket Standard és Premium tárolására kulcsattribútumokkal. |
| Fizikai eszközön helyileg rögzített kötet maximális mérete |A 8100 8.5 TB <br></br> A 8600 22.5 TB |8100 és 8600 különböző fizikai eszközök. |
| ISCSI-kapcsolatok maximális száma |512 | |
| A kezdeményezők iSCSI-kapcsolatok maximális száma |512 | |
| Hozzáférés-vezérlési rekordokat eszközönként maximális száma |64 | |
| A biztonsági mentési házirend / kötetek maximális száma |20 | |
| Biztonsági mentés ütemezése (a biztonsági mentési házirend) tárolása maximális száma |64 | |
| Egy biztonsági mentési házirend ütemezések maximális száma |10 | |
| Bármilyen típusú, amelynek meg kell őrizni kötetenként pillanatképek maximális száma |256 |Ez a szám tartalmazza helyi pillanatképeket és felhőbeli pillanatképeket. |
| Lehet, hogy bármely eszköz megtalálható a pillanatképek maximális száma |10,000 | |
| A biztonsági másolat, visszaállítási párhuzamosan is, vagy a klónozáshoz kötetek maximális számát |16 |<ul><li>Ha több mint 16 kötetek, ezért a feldolgozásuk egymás után amint elérhetővé válnak a feldolgozási tárolóhelye.</li><li>Új biztonsági másolatot egy klónozott vagy visszaállított rétegzett kötet nem kerülhet sor, amíg hello művelet be nem fejeződik. Egy helyi kötet, azonban biztonsági mentések engedélyezettek után hello kötet online állapotban.</li></ul> |
| Visszaállítás és helyreállítás ideje rétegzett kötetek klónozása |< 2 perc |<ul><li>a restore vagy a Klónozás működését, függetlenül hello kötet mérete 2 percen belül hello kötet legyen elérhető.</li><li>hello kötet kezdetben lehet a teljesítmény lassabb, mint a szokásos módon legtöbb hello adatok és metaadatok továbbra is hello felhőben található. Teljesítmény megnőhet, adatfolyamok hello felhő toohello StorSimple eszközön.</li><li>hello teljes ideje toodownload metaadatok lefoglalt kötet mérete hello függ. Metaadatok automatikusan bevinni hello eszköz hello háttérben hello díj TB-nyi lefoglalt kötet adatait 5 perc. Ez érinthet internetes sávszélesség toohello felhő.</li><li>visszaállítás hello, vagy a Klónozás befejeződött, ha minden hello metaadatok hello eszközön.</li><li>Biztonsági mentési művelet nem hajtható végre, amíg hello visszaállítási, vagy a Klónozás teljesen kész. |
| Állítsa vissza a helyileg rögzített kötetek helyreállítása ideje |< 2 perc |<ul><li>hello kötet hello visszaállítási művelet, függetlenül attól, hello kötet mérete, 2 percen belül legyen elérhető.</li><li>hello kötet kezdetben lehet a teljesítmény lassabb, mint a szokásos módon legtöbb hello adatok és metaadatok továbbra is hello felhőben található. Teljesítmény megnőhet, adatfolyamok hello felhő toohello StorSimple eszközön.</li><li>hello teljes ideje toodownload metaadatok lefoglalt kötet mérete hello függ. Metaadatok automatikusan bevinni hello eszköz hello háttérben hello díj TB-nyi lefoglalt kötet adatait 5 perc. Ez érinthet internetes sávszélesség toohello felhő.</li><li>Ellentétben a rétegzett kötetek, a helyileg rögzített kötetekhez hello kötet adatait is tölt le helyileg hello eszközön. hello visszaállítási művelet befejeződött, amikor minden hello kötetadatokról állapotba toohello eszköz.</li><li>hello visszaállítási műveletek hosszú lehet. hello teljes toocomplete hello visszaállítás egy korábbi időpontra kiépített hello helyi kötet, az internetes sávszélességet és hello meglévő adatok hello eszközön hello méretétől függ. Hello helyileg rögzített kötetet a biztonsági mentési műveletek engedélyezettek, amíg hello visszaállítási művelet van folyamatban. |
| A felhőalapú pillanatfelvételek feldolgozási sebessége |15 perc/TB |<ul><li>Minimális időtartam toomake hello felhőalapú pillanatfelvétel készen áll a feltöltési / TB lefoglalt kötet adatok biztonsági mentése. </li><li> Adja hozzá a megadott idő toohello pillanatkép feltöltés idő hello internetes sávszélesség toocloud által érintett összes felhő pillanatkép idő kiszámítása. |
| Maximális ügyfél olvasási/írási teljesítmény (amikor a hello SSD-réteghez és kiszolgálása között) * |920/720 MB/s az egyetlen 10 GbE hálózati adapter |Másolatot too2x az MPIO és két hálózati adapterrel. |
| Maximális ügyfél olvasási/írási teljesítmény (ha szolgáltatott hello HDD-réteget a) * |120/250 MB/s | |
| Maximális ügyfél olvasási/írási teljesítmény (ha szolgáltatott hello felhő rétegtől) * Update 3 és újabb ** |40/60 MB/s, rétegzett kötet<br><br>60/80 MB/s, rétegzett kötet archiválási jelölőnégyzetet a kötet létrehozása során |Olvasás átviteli ügyfelek létrehozása és fenntartása elegendő i/o-várólistamélység függ. <br><br>Elért sebessége hello használt alapul szolgáló tárolási fiók hello sebességétől függ. |

&#42; I/o-típusonkénti maximális átviteli sebesség és 100 százalék olvasási 100 százalék írási forgatókönyvek mérték. Tényleges átviteli alacsonyabb lehet, és attól függ, i/o kombinálhatók és a hálózati feltételek.

&#42; &#42; Teljesítmény számok előzetes tooUpdate 3 alacsonyabb lehet.

## <a name="next-steps"></a>Következő lépések
Felülvizsgálati hello [StorSimple rendszerkövetelmények](storsimple-8000-system-requirements.md).

