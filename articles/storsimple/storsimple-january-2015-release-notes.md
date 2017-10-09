---
title: "aaaStorSimple 8000 frissítése 0,2 kibocsátási megjegyzései |} Microsoft Docs"
description: "Hello új szolgáltatások és javításokat, nyissa meg a problémákat, és ismerteti hello elérhető megoldásai 2015. januári Microsoft Azure StorSimple release (frissítés 0,2)."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: d9684ae3-b38f-4678-9d70-e5dbc6b03350
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/16/2016
ms.author: v-sharos
ms.openlocfilehash: 1cee795df0b53d9b9276bc33074cf1a7aa188835
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-update-02-release-notes---january-2015"></a>A StorSimple 8000 Series Update 0,2 kibocsátási megjegyzések – 2015. januári
## <a name="overview"></a>Áttekintés
hello alábbi kibocsátási megjegyzések hello kritikus megnyitott problémák azonosításához a Microsoft Azure StorSimple 2015. januári kiadása hello. Hello StorSimple szoftverek listáját és a belső vezérlőprogram-frissítésekre ebben a kiadásban szereplő is tartalmazzák. Ez az hello második kiadás után hello a StorSimple 8000 Series kiadási verziójával történt a 2014. július általánosan elérhető.

A frissítés nem módosul hello. októberi frissítés hello fizikai eszköz szoftverének verziójával. Toobe verzió 6.3.9600.17312 továbbra is. Kép: hello virtuális eszköz által használt hello kép ebben a kiadásban megváltozott. Azt jelzi, ezért minden hello új virtuális eszköz létrehozása után 1/20/2015 jelenik-e meg: 6.3.9600.17361 hello verziója.  

Tekintse át a kibocsátási megjegyzésekben hello hello 2015. januári frissítés tárolt információ a következő hello.

> [!IMPORTANT]
> * A frissítés nem érhető el a Windows Update, és nem telepíthető, például más frissítéseket. Az eszköz nem fog kapni a frissítés, még akkor is, ha az alkalmazott hello frissítések hello klasszikus Azure portál használatával. A frissítés csak 2015. januári 20 után létrehozott toovirtual eszközökre vonatkozik. 
> * hello StorSimple január kiadása nem tartalmaz semmilyen frissítések toohello StorSimple fizikai eszköz. Továbbra is érvényesek lesznek minden elérhető Windows frissítések toohello virtuális eszközt, beleértve a legutóbbi biztonsági javítások, de nem fogják látni hello StorSimple fizikai eszköz verziója megváltozik.
> 
> 

## <a name="whats-new-in-hello-january-release"></a>What's new in hello január kiadás
A frissítés tartalmazza a javítás kapcsolódó toohello kötetek offline állapotra vált hello virtuális eszközön. (Lásd: [ebben a kiadásban javított](#issues-fixed-in-the-january-release).)  

hello frissítés nem tartalmaz új szolgáltatások vagy funkciók.  

## <a name="issues-fixed-in-hello-january-release"></a>Javított hello január kiadás
hello következő táblázat ismerteti a frissítés rögzített hello probléma.

| Nem. | Szolgáltatás | Probléma | Érvényes toophysical eszköz | Érvényes toovirtual eszköz |
| --- | --- | --- | --- | --- |
| 1 |Kötetek offline állapotra vált |Amikor magas felhő késések továbbra is fennáll, több percig, hello StorSimple virtuális eszköz kötetek kapcsolat nélküli módba hello állomáson. Ez a javítás növeli a felhő késések, ezáltal minimalizálja a hello olyan helyzetekben, amelyek hello kötetek offline toogo állomásokon hello küszöbértékét. |Nem |Igen |

## <a name="known-issues-in-hello-january-release"></a>Hello január kiadásban ismert problémák
hello a következő táblázat összefoglalja az ismert problémákról, ebben a kiadásban.

| Nem. | Szolgáltatás | Probléma | Megjegyzések/megoldás | Érvényes toophysical eszköz | Érvényes toovirtual eszköz |
| --- | --- | --- | --- | --- | --- |
| 1 |Gyári beállítások visszaállítása |Bizonyos esetekben a gyári beállítások visszaállítása, végrehajtásakor hello StorSimple eszköz előfordulhat, hogy, és megjeleníti ezt az üzenetet: **toofactory alaphelyzetbe állítása folyamatban van a (8 fázis).** Ez akkor fordul elő, ha a CTRL + C gombra, amíg hello parancsmag folyamatban van. |CTRL + C nem nyomja meg a gyári beállítások visszaállítása kezdeményezése után. Ha már ebben az állapotban, forduljon a Microsoft Support további lépéseket. |Igen |Nem |
| 2 |Lemez kvórum |Ritka esetekben ha hello EBOD szolgáltatással 8600-eszköz lemezek hello többsége megszakadt a kapcsolat nincs lemez kvórum eredményezi majd hello tárolókészlet sem kapcsolódik a hálózathoz. Offline állapotban maradnak, még akkor is, ha hello lemez újracsatlakoztatását. |Szüksége lesz tooreboot hello eszköz. Ha hello a probléma továbbra is fennáll, forduljon a Microsoft Support további lépéseket. |Igen |Nem |
| 3 |Felhőalapú pillanatfelvétel hibák |Ritka esetekben egy felhő-pillanatfelvételt hello hiba miatt meghiúsulhat **elérte a maximális biztonsági korlátot**. Ez akkor fordul elő, ha az túllépi a hello 255 online klónok ugyanarra az eszközre, a hello azonos eredeti kötet, amelyet már töröltek. | |Igen |Igen |
| 4 |Helytelen vezérlő azonosítója |A vezérlő helyettesítő végrehajtásakor vezérlő 0 lehet, hogy megjelennek, a vezérlő 1. A tartományvezérlő csere közben hello kép hello társ-csomópont betöltésekor hello vezérlő azonosítója is megjelennek kezdetben, hello társ vezérlő azonosítója.  Ritka esetben ez a viselkedés előfordulhat, hogy is látható. a rendszer újraindítását követően. |Nincs felhasználói beavatkozásra szükség. Ez a helyzet magától megoldódik, amikor hello vezérlő helyettesítő befejeződése után. |Igen |Nem |
| 5 |Eszközfigyelési diagramok |A StorSimple Manager szolgáltatás hello hello eszköz figyelési diagramok nem működnek a Basic, vagy az NTLM-hitelesítés nincs engedélyezve az hello proxykiszolgálót hello eszközhöz. |Hello webproxy konfigurálása a StorSimple Manager szolgáltatásban regisztrált úgy, hogy a hitelesítés be van állítva tooNONE hello eszköz módosításához. toodo StorSimple Set-HcsWebProxy parancsmag, ez futtatási hello hello Windows PowerShell. |Igen |Igen |
| 6 |Tárfiókok |Hello tárolási toodelete hello tárolási szolgáltatásfiók használata egy nem támogatott forgatókönyvet. Ez vezet tooa helyzetet, amelyben felhasználói nem lehet adatokat beolvasni. | |Igen |Igen |
| 7 |Eszköz feladatátvétel |A kötettároló hello azonos forrás eszköz toodifferent Céleszközök nem támogatja a több feladatátvétele. |Egy egyetlen elhalt eszközt toomultiple eszközökről feladatátvételi hello kötettárolók először átadja a feladatokat eszköz hello az adatok tulajdonjoga elvesztése biztosítják. Egy feladatátvétel után ezek kötettárolók jelenik meg, vagy a klasszikus Azure portálon hello meg eltérően viselkednek. |Igen |Nem |
| 8 |Telepítés |StorSimple Adapter SharePoint telepítéshez, során kell tooprovide egy eszköz IP-cím ahhoz, hogy hello telepítés toofinish sikeresen. | |Igen |Nem |
| 9 |Webproxy |Ha a webproxy-konfigurációja HTTPS hello meg van adva protokoll, akkor az eszköz-szolgáltatások közötti kommunikáció néven érinti, és hello eszköz nélküli módba. Támogatási csomag is tudna generálni hello folyamat során fel jelentős erőforrásokat az eszközön. |Győződjön meg arról, hogy hello webes proxy URL-címe HTTP, a megadott protokoll hello. További információ hogyan túl[az eszköz webalkalmazás-proxy konfigurálása](storsimple-configure-web-proxy.md). |Igen |Nem |
| 10 |Webproxy |Ha a konfigurált és engedélyezett a webalkalmazás-proxy regisztrált egy eszközt, akkor szüksége lesz toorestart hello aktív vezérlő az eszközön. | |Igen |Nem |
| 11 |Magas felhő késéssel és nagy i/o-munkaterhelés |Amikor a StorSimple eszköz nagyon magas felhő késések (másodperc sorrendben) és a magas i/o-munkaterhelés észlel, hello eszköz kötetek csökkent állapotba, és hello i/o "az eszköz nem áll készen" hiba miatt sikertelen lehet. |Meg kell, újraindítás hello eszközvezérlők toomanually, vagy végezzen el egy eszköz feladatátvételi toorecover ebben a helyzetben a. |Igen |Nem |

## <a name="physical-device-updates-in-hello-january-release"></a>Hello január kiadásban fizikai eszköz frissítése
A frissítés nem tartalmaz semmilyen egyéb módosítások toohello StorSimple eszközt.

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-hello-january-release"></a>Soros csatlakozású SCSI (SAS) vezérlő és a belső vezérlőprogram frissítések hello január kiadásban
Ebben a kiadásban nem tartalmaz a frissítések toohello soros csatlakozású SCSI (SAS) vezérlő vagy hello belső vezérlőprogramját. hello illesztőprogram frissítéséhez hello október, 2014 kiadásban volt. 

## <a name="virtual-device-updates-in-hello-january-release"></a>Hello január kiadásban virtuális eszköz frissítése
Ez a kiadás egy frissített lemezképet a virtuális eszköz hello tartalmaz. Minden hello virtuális eszköz létrehozása után 2015. januári 20 hello szoftververzió jelenik meg: 6.3.9600.17361.

