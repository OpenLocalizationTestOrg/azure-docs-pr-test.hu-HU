---
title: "aaaStorSimple 8000 Series Update 2.2 kibocsátási megjegyzései |} Microsoft Docs"
description: "Hello új funkciókat, problémák és megoldások ismerteti a StorSimple 8000 Series Update 2.2."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 5cf03ea8-2a0f-4552-b6dc-7ea517783d7b
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/18/2016
ms.author: alkohli
ms.openlocfilehash: a2902126f4011fa9ade64f63a8abdec095874fb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-update-22-release-notes"></a>A StorSimple 8000 Series Update 2.2 kibocsátási megjegyzései
## <a name="overview"></a>Áttekintés
hello alábbi kibocsátási megjegyzések leírják hello új szolgáltatásokat és a StorSimple 8000 Series Update 2.2 hello kritikus megnyitott problémák azonosításához. Ebben a kiadásban szereplő hello StorSimple szoftverfrissítések listáját is tartalmaznak. 

2.2-es frissítés alkalmazott tooany StorSimple eszköz kiadásban (GA) vagy a frissítés 0,1 futó frissítés 2.1 keresztül lehet. hello eszköz verziója frissítés 2.2 társított 6.3.9600.17708.

Tekintse át a hello kibocsátási megjegyzések hello központi telepítése előtt frissítse a StorSimple megoldásban lévő hello információt.

> [!IMPORTANT]
> * 2.2-es frissítést csak szoftverfrissítések rendelkezik. Körülbelül 1,5 – 2 óra tooinstall veszi ezt a frissítést. 
> * Ha frissítés 2.1-es verzióját futtatja, ajánlott frissítés 2.2 minél hamarabb alkalmazni.
> * Új kiadásokban nem láthatók frissítések azonnal, mert a hello frissítések fázisokra bontva történő bevezetéséhez végezzük. Néhány napot várni, és a frissítések újra ezeket a majd vizsgálatát hamarosan elérhető lesz.
> 
> 

## <a name="whats-new-in-update-22"></a>2.2-es frissítés újdonságai
hello következő fontos fejlesztést tartalmaz végzett frissítés 2.2.

* **Automatikus terület-visszanyerést optimalizálási** – Ha az adatok törlése a dinamikusan kiosztott kötetek hello nem használt tárolási címblokkokat kell visszaigényelt toobe. Ebben a kiadásban továbbfejlesztett hello terület visszaigénylését folyamattal rendelkeznek a nem használt hello eredményezve hello felhőből válik elérhető gyorsabban az összehasonlított toohello korábbi verzióinak szóköz.
* **Pillanatkép készítése a teljesítményt javító** – frissítés 2.2 rendelkezik továbbfejlesztett hello idő tooprocess egy felhő-pillanatfelvételt bizonyos forgatókönyvekben, ahol nagy méretű köteteket használatban vannak, és nincs minimális toono adatforgalommal. Olyan forgatókönyvekben, amelyek ez a fejlesztés előnyös lehet hello archív kötetek.
* **Támogatási csomag összegyűjtéséhez korlátozására** – hello támogatási csomag gyűjtött, és ebben a kiadásban feltöltött hello módon fejlesztések történtek. 
* **Megbízhatóság fejlesztései frissítése** – ebben a kiadásban rendelkezik hibajavításokat tartalmaz, amelyek egy továbbfejlesztett frissítés megbízhatóságát.

## <a name="issues-fixed-in-update-22"></a>A frissítés 2.2 megoldott problémák
a következő táblák hello frissítések 2.2 és 2.1 javított problémák összegzését tartalmazza.    

| Nem | Szolgáltatás | Probléma | Érvényes toophysical eszköz | Érvényes toovirtual eszköz |
| --- | --- | --- | --- | --- |
| 1 |Gazdagép-teljesítmény |Hello korábbi kiadásában, gazdagép-oldali teljesítményproblémákat megfigyelt egy helyileg rögzített kötet hello létrehozása során, és helyileg egy rétegzett kötet tooa hello konvertálása során rögzített kötet. Ebben a kiadásban, ezáltal eredményezve hello kötet létrehozása és -konvertálási eljárások során hello állomás teljesítmény javulása fix ezeket a problémákat. |Igen |Nem |
| 2 |Helyileg rögzített kötetek |Ritka esetekben hello rendszer összeomlás lenne egy helyileg rögzített kötet létrehozásakor. Ez a hiba kijavítása ebben a kiadásban. |Igen |Nem |
| 3 |rétegezéséhez |Nem voltak szórványos összeomlik, ha hello metaadatok a StorSimple felhő készülékek (a 8010-es és a 8020-as modell) hello rétegzett túl hello felhő. Ez a probléma fennáll ebben a kiadásban. |Nem |Igen |
| 4 |Pillanatkép létrehozása |A nagy méretű köteteket és minimális toono adatforgalommal forgatókönyvekben növekményes pillanatképek problémák kapcsolódó toohello létrehozásának történt. Ebben a kiadásban ezek a problémák kerültek. |Igen |Igen |
| 5 |Openstack hitelesítés |Használatával Openstack is hello szolgáltatást, ha hello felhasználó fog futni belép egy alkalomszerű hiba kapcsolódó toohello authentication ahol a hello JSON elemző crash eredményezett. A kijavítanak ebben a kiadásban. |Igen |Nem |
| 6 |Gazdagép-oldali másolása |A szoftver korábbi verzióját egy alkalomszerű hiba kapcsolatos toohello ODX időzítési fordult elő, amikor hello adatokat másol egy kötet tooanother kötet. Eredményezne. a vezérlő feladatátvétel és hello rendszer potenciálisan sikerült lépjen a helyreállítási módba. A kijavítanak ebben a kiadásban. |Igen |Nem |
| 7 |A Windows Management Instrumentation (WMI) |Hello korábbi verzióiban szoftver volt webes proxy hiba hello kivétellel több példányát "<ManagementException> szolgáltató betöltési hibához". Ez a hiba kiadott tooa WMI-memóriavesztés volt, és most már rögzített. |Igen |Nem |
| 8 |Frissítés |Bizonyos ritkán hello szoftver, a korábbi verzióinál hello felhasználói tooscan vagy telepítését frissítést közben kapott "CisPowershellHcsscripterror". Ez a probléma fennáll ebben a kiadásban. |Igen |Igen |
| 9 |Támogatási csomag |Ebben a kiadásban történtek fejlesztések toohello módon hello támogatási csomag összegyűjtött és feltöltve. |Igen |Igen |

## <a name="known-issues-in-update-22"></a>A frissítés 2.2 ismert problémák
hello a következő táblázat összefoglalja az ismert problémákról, ebben a kiadásban.

| Nem. | Szolgáltatás | Probléma | Megjegyzések / megoldás | Érvényes toophysical eszköz | Érvényes toovirtual eszköz |
| --- | --- | --- | --- | --- | --- |
| 1 |Lemez kvórum |Ritka esetben ha hello EBOD szolgáltatással 8600-eszköz lemezek hello többsége megszakadt a kapcsolat nincs lemez kvórum eredményezi majd hello tárolókészlet lesz kapcsolat nélkül. Offline állapotban maradnak, még akkor is, ha hello lemez újracsatlakoztatását. |Szüksége lesz tooreboot hello eszköz. Ha hello a probléma továbbra is fennáll, forduljon a Microsoft Support további lépéseket. |Igen |Nem |
| 2 |Helytelen vezérlő azonosítója |A vezérlő helyettesítő végrehajtásakor vezérlő 0 lehet, hogy megjelennek, a vezérlő 1. A tartományvezérlő csere közben hello kép hello társ-csomópont betöltésekor hello vezérlő azonosítója is megjelennek kezdetben, hello társ vezérlő azonosítója. Ritka esetben ez a viselkedés előfordulhat, hogy is látható. a rendszer újraindítását követően. |Nincs felhasználói beavatkozásra szükség. Ez a helyzet magától megoldódik, amikor hello vezérlő helyettesítő befejeződése után. |Igen |Nem |
| 3 |Tárfiókok |Hello tárolási toodelete hello tárolási szolgáltatásfiók használata egy nem támogatott forgatókönyvet. Ez vezet tooa helyzetet, amelyben felhasználói nem lehet adatokat beolvasni. | |Igen |Igen |
| 4 |Eszköz feladatátvétel |A kötettároló hello azonos forrás eszköz toodifferent Céleszközök nem támogatja a több feladatátvétele. Egy egyetlen elhalt eszközt toomultiple eszközökről feladatátvételi hello kötettárolók először átadja a feladatokat eszköz hello az adatok tulajdonjoga elvesztése biztosítják. Egy feladatátvétel után ezek kötettárolók jelenik meg, vagy a klasszikus Azure portálon hello meg eltérően viselkednek. | |Igen |Nem |
| 5 |Telepítés |StorSimple Adapter SharePoint telepítéshez, során kell tooprovide egy eszköz IP-cím ahhoz, hogy hello telepítés toofinish sikeresen. | |Igen |Nem |
| 6 |Webproxy |Ha a webproxy-konfigurációja HTTPS hello meg van adva protokoll, akkor az eszköz-szolgáltatások közötti kommunikáció néven érinti, és hello eszköz nélküli módba. Támogatási csomag is tudna generálni hello folyamat során fel jelentős erőforrásokat az eszközön. |Győződjön meg arról, hogy hello webes proxy URL-címe HTTP, a megadott protokoll hello. További információ: túl[az eszköz webalkalmazás-proxy konfigurálása](storsimple-configure-web-proxy.md). |Igen |Nem |
| 7 |Webproxy |Ha a konfigurált és engedélyezett a webalkalmazás-proxy regisztrált egy eszközt, akkor szüksége lesz toorestart hello aktív vezérlő az eszközön. | |Igen |Nem |
| 8 |Magas felhő késéssel és nagy i/o-munkaterhelés |Amikor a StorSimple eszköz nagyon magas felhő késések (másodperc sorrendben) és a magas i/o-munkaterhelés észlel, hello eszköz kötetek csökkent állapotba, és hello i/o "az eszköz nem áll készen" hiba miatt sikertelen lehet. |Meg kell, újraindítás hello eszközvezérlők toomanually, vagy végezzen el egy eszköz feladatátvételi toorecover ebben a helyzetben a. |Igen |Nem |
| 9 |Azure PowerShell |Hello StorSimple parancsmag használatakor **Get-AzureStorSimpleStorageAccountCredential &#124; Select-Object - először 1 - várakozási** tooselect hello első objektum, így hozhat létre egy új **VolumeContainer** objektum hello parancsmag minden hello objektumot ad vissza. |Hello parancsmag burkolása zárójelek között az alábbiak szerint: **(Get-Azure-StorSimpleStorageAccountCredential) &#124; Select-Object - először 1 - várakozási** |Igen |Igen |
| 10 |Migrálás |Több kötet tároló áttelepítési lett átadva, hello Európai legfrissebb biztonsági mentés esetén csak az első kötettároló hello pontos. Emellett a párhuzamos áttelepítés hello hello első kötettároló az első 4 biztonsági mentések áttelepítése után kezdődik. |Azt javasoljuk, hogy egyszerre több kötet tároló át. |Igen |Nem |
| 11 |Migrálás |Hello visszaállítása után a kötetek nem lettek hozzáadva toohello biztonsági mentési házirend vagy hello virtuális lemez csoport. |A kötetek tooa a biztonsági mentési házirend rendelés toocreate biztonsági mentései kell tooadd. |Igen |Igen |
| 12 |Migrálás |Hello áttelepítés befejezése után hello 5000/7000-es adatsorozat eszköz nem kell-e érni hello áttelepített adatok tárolók. |Azt javasoljuk, hogy törölje-e hello áttelepített adatok tárolók hello áttelepítés befejezése és véglegesítése után. |Igen |Nem |
| 13 |Klónozás és vész-Helyreállítási |1. frissítést futtató a StorSimple eszköz nem tudja klónozni, vagy hajtsa végre a vész-helyreállítási tooa eszköz 1 frissítés előtti szoftvert futtató. |Szüksége lesz tooupdate hello target eszköz tooUpdate 1 tooallow ezeket a műveleteket |Igen |Igen |
| 14 |Migrálás |Áttelepítés konfigurációs biztonsági másolat nem tud az adatsorozat 5000-7000-es eszközön, kötet csoportok nem társított kötetekkel vannak. |Nincs társított kötetek összes hello üres kötet csoportok törlése, majd próbálkozzon újra a hello konfigurációs biztonsági másolat. |Igen |Nem |
| 15 |Az Azure PowerShell-parancsmagok és a helyileg rögzített kötetek |Nem hozható létre egy helyileg rögzített kötet Azure PowerShell-parancsmagok használatával. (Azure PowerShell használatával hoz létre kötet lesz helyezhető el.) |Mindig legyen hello StorSimple Manager szolgáltatás tooconfigure helyileg rögzített kötetek. |Igen |Nem |
| 16 |A helyileg rögzített kötetekhez szabad terület |Ha töröl egy helyileg rögzített kötetet, hello szabad lemezterület az új kötetekre nem frissülhet azonnal. hello StorSimple Manager szolgáltatás frissítések hello helyi lemezterület körülbelül óránként. |Várjon, amíg egy óraba toocreate hello új kötet meg. |Igen |Nem |
| 17 |Helyileg rögzített kötetek |A visszaállítási feladat hello ideiglenes pillanatkép biztonsági másolatából hello biztonságimásolat-katalógus, de csak hello visszaállítási feladat hello időtartamát mutatja. Emellett elérhetővé teszi a virtuális lemez csoport előtaggal rendelkező **tmpCollection** a hello **biztonsági mentési házirendek** oldalon, de csak időtartamára hello hello visszaállítási feladat. |Ez akkor fordulhat elő, ha a visszaállítási feladat csak a helyi számítógépre van rögzítve, kötetek vagy a helyileg rögzített és a rétegzett kötetek kombinációját. Ha hello visszaállítási feladat csak a rétegzett kötetek tartalmazza, majd ezt a viselkedést nem történik. Meg kell adni a felhasználói beavatkozás nélkül. |Igen |Nem |
| 18 |Helyileg rögzített kötetek |Ha a helyreállítási feladat megszakítása, a tartományvezérlő feladatátvétel hello visszaállítási feladat megjeleníti ezt követően azonnal **sikertelen** helyett **visszavonva**. Ha a helyreállítási feladat sikertelen lesz, a tartományvezérlő feladatátvétel hello visszaállítási feladat megjeleníti ezt követően azonnal **visszavonva** helyett **sikertelen**. |Ez akkor fordulhat elő, ha a visszaállítási feladat csak a helyi számítógépre van rögzítve, kötetek vagy a helyileg rögzített és a rétegzett kötetek kombinációját. Ha hello visszaállítási feladat csak a rétegzett kötetek tartalmazza, majd ezt a viselkedést nem történik. Meg kell adni a felhasználói beavatkozás nélkül. |Igen |Nem |
| 19 |Helyileg rögzített kötetek |Ha a helyreállítási feladat megszakítása vagy a visszaállítás sikertelen lesz, majd a tartományvezérlő feladatátvétel, ha hello megjelenik egy további visszaállítási feladat **feladatok** lap. |Ez akkor fordulhat elő, ha a visszaállítási feladat csak a helyi számítógépre van rögzítve, kötetek vagy a helyileg rögzített és a rétegzett kötetek kombinációját. Ha hello visszaállítási feladat csak a rétegzett kötetek tartalmazza, majd ezt a viselkedést nem történik. Meg kell adni a felhasználói beavatkozás nélkül. |Igen |Nem |
| 20 |Helyileg rögzített kötetek |Ha rétegzett kötetet (létrehozott és 1.2-es frissítés a klónozott vagy korábbi) tooconvert tooa helyileg rögzített kötet és az eszköz nincs elegendő szabad terület vagy egy felhőalapú leállás van, majd hello clone(s) sérülését. |A probléma csak a köteteket, amelyek a létrehozott és a klónozott frissítés előtti 2.1 szoftvert. Ez egy ritka eset legyen. | | |
| 21 |Kötet átalakítás |Nem frissíthető hello ACRs csatolt tooa kötet, amíg folyamatban van egy kötet átalakítás (rögzített rétegzett toolocally vagy fordítva). Hello ACRs frissítése adatsérülést eredményezhet. |Szükség esetén – hello ACRs előzetes toohello kötet átalakítás frissítése, és ne végezzen további ACR frissítések hello átalakítás közben. | | |

## <a name="controller-and-firmware-updates-in-update-22"></a>A tartományvezérlő és a belső vezérlőprogram frissítési 2.2 frissítések
Ez a kiadás csak szoftveres frissítést tartalmaz. Azonban ha egy verziója előzetes tooUpdate 2 frissít, akkor tooinstall illesztőprogram, Storport, Spaceport, és (bizonyos esetekben) az eszköz belső vezérlőprogram-frissítések lemez.

További információt a hogyan tooinstall hello illesztőprogram Storport, Spaceport és lemez belső vezérlőprogram-frissítésekre: [2.2-es frissítés telepítése](storsimple-install-update-21.md) a StorSimple eszköz.

## <a name="virtual-device-updates-in-update-22"></a>A frissítés 2.2 virtuális eszköz frissítése
A frissítés alkalmazott toohello virtuális eszköz nem lehet. Új virtuális eszközök létrehozott toobe kell. 

## <a name="next-step"></a>Következő lépés
Ismerje meg, hogyan túl[2.2-es frissítés telepítése](storsimple-install-update-21.md) a StorSimple eszköz.

