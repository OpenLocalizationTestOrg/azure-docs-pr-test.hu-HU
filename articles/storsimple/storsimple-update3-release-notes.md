---
title: "aaaStorSimple 8000 Series Update 3 kibocsátási megjegyzései |} Microsoft Docs"
description: "Hello új funkciókat, problémák és megoldások ismerteti a StorSimple 8000 Series Update 3."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 2158aa7a-4ac3-42ba-8796-610d1adb984d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5bfcba61f7f210531437f650eafaad4dbd8c7c45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="update-3-release-notes-for-your-storsimple-8000-series-device"></a>Frissítse a StorSimple 8000 series eszköz 3 kibocsátási megjegyzései

## <a name="overview"></a>Áttekintés
hello alábbi kibocsátási megjegyzések leírják hello új szolgáltatásokat és a StorSimple 8000 Series Update 3 hello kritikus megnyitott problémák azonosításához. Ebben a kiadásban szereplő hello StorSimple szoftverfrissítések listáját is tartalmaznak. 

Update 3 alkalmazott tooany StorSimple eszköz kiadásban (GA) vagy a frissítés 0,1 futó frissítés 2.2 keresztül lehet. hello eszköz verziója frissítés 3 társított 6.3.9600.17759.

Tekintse át a hello kibocsátási megjegyzések hello központi telepítése előtt frissítse a StorSimple megoldásban lévő hello információt.

> [!IMPORTANT]
> * Frissítés 3 eszközhöz, LSI illesztőprogram és a belső vezérlőprogram és Storport és Spaceport frissíti. Körülbelül 1,5 – 2 óra tooinstall veszi ezt a frissítést. 
> * Új kiadásokban nem láthatók frissítések azonnal, mert a hello frissítések fázisokra bontva történő bevezetéséhez végezzük. Néhány napot várni, és a frissítések újra ezeket a majd vizsgálatát hamarosan elérhető lesz.
> 
> 

## <a name="whats-new-in-update-3"></a>What's new in Update 3
hello következő fontos fejlesztést tartalmaz, és hibajavítások végzett frissítés 3.

* **Automatikus terület-visszanyerést módosítások** – hello rendszer gyorsabb végrehajtását eredményezi hello készenléti tartományvezérlőn futtassa hello terület-visszanyerést algoritmusok Update 3 indítása. A terület-visszaigényléssel szükséges toowork hello portok további információkért tekintse meg a toohello [hálózati követelményei StorSimple](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
* **A teljesítményt javító** – Update 3 olvasási és írási teljesítményt toohello felhő javult.
* **Áttelepítéssel kapcsolatos fejlesztések** – ebben a kiadásban több programhiba javítások és fejlesztések adatsorozat 5000/7000-es eszközök too8000 adatsorozat eszközökről hello áttelepítési szolgáltatás történik. További információ a hogyan toouse hello áttelepítési funkcióját: túl[5000/7000-es adatsorozat eszköz too8000 adatsorozat eszközről áttelepítési](https://gallery.technet.microsoft.com/Azure-StorSimple-50007000-c1a0460b). 
* **Figyelési kapcsolódó javítások** – ebben a kiadásban hibák kapcsolódó toomonitoring diagramokat, a szolgáltatás irányítópultját és az eszköz irányítópult javítva lett.

## <a name="issues-fixed-in-update-3"></a>A frissítés 3 megoldott problémák
a következő táblák hello Update 3. javított problémák összegzését tartalmazza.    

| Nem | Szolgáltatás | Probléma | Érvényes toophysical eszköz | Érvényes toovirtual eszköz |
| --- | --- | --- | --- | --- |
| 1 |Gazdagép-oldali adatok áttelepítése |A hello korábbi verzió, a StorSimple felhő készülék hello lett offline állapotra vált, a gazdagép-oldali adatok áttelepítése során. Ez a probléma fennáll ebben a kiadásban. |Nem |Igen |
| 2 |Helyileg rögzített kötetek |Hello a korábbi verzióban volt problémák kapcsolódó tooI/O-hibák, a kötet konvertálási hibák és a helyileg rögzített kötetekhez datapath sikertelen. Ezek a problémák legfelső szintű okozott és rögzített ebben a kiadásban. |Igen |Nem |
| 3 |Figyelés |Több problémák kapcsolódó tooreporting egységek és figyelését, valamint eszköz irányítópult diagramok, amelyben helytelen információt jelent meg, a helyileg rögzített kötetek. Ebben a kiadásban ezek a problémák kerültek. |Igen |Nem |
| 4 |Nagy mennyiségű írási i/o |StorSimple használata esetén a nagy írási műveletek munkaterhelések használatakor hello felhasználói futnak be egy alkalomszerű hiba ahol hello munkakészlet volt folyamatban rétegzett hello felhőbe. A kijavítanak ebben a kiadásban. |Igen |Igen |
| 5 |Biztonsági mentés |Bizonyos ritkán hello korábbi verzióiban szoftver amikor a felhasználó elvégez egy távoli Klónozás biztonsági másolatot, azok volna hibákba felhő, és hello művelet lenne hiba. Ebben a kiadásban hello a probléma fennáll, és hello művelet sikeresen befejeződik. |Igen |Igen |
| 6 |Biztonsági mentési házirend |Bizonyos esetekben a ritka hello a korábban kiadott szoftver történt hiba kapcsolódó biztonsági mentési házirend toohello törlését. Ez a probléma fennáll ebben a kiadásban. |Igen |Igen |

## <a name="known-issues-in-update-3"></a>A frissítés 3 ismert problémák
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
| 22 |Frissítések |Frissítés 3 alkalmazásakor hello **karbantartási** hello Azure klasszikus portál lesz a lap megjelenítése hello következő üzenet kapcsolódó tooUpdate 2 – "a StorSimple 8000 series Update 2 tartalmaz hello lehetőségét, hogy a Microsoft tooproactively gyűjtése adatainak naplózása az eszközről, ha azt észleli a potenciális problémák". Ez félrevezető módon azt jelzi, hogy hello eszköz alatt frissített tooUpdate 2. Miután hello eszköz frissítése succeesfully tooUpdate 3, ez az üzenet véglegesen eltűnik. |Ez a viselkedés egy későbbi kiadásban lesz kijavítva. |Igen |Nem |

## <a name="controller-and-firmware-updates-in-update-3"></a>A tartományvezérlő és a belső vezérlőprogram frissítések Update 3.
Ebben a kiadásban rendelkezik LSI illesztőprogram és a belső vezérlőprogram frissítések. Hogyan tooinstall hello LSI illesztőprogram és a belső vezérlőprogram-frissítésekre: további információk [Update 3 telepítéséhez](storsimple-install-update-3.md) a StorSimple eszköz.

## <a name="virtual-device-updates-in-update-3"></a>A frissítés 3 virtuális eszköz frissítése
A frissítés alkalmazott toohello StorSimple felhő készülék (más néven hello virtuális eszköz) nem lehet. Új virtuális eszközök létrehozott toobe kell. 

## <a name="next-step"></a>Következő lépés
Ismerje meg, hogyan túl[Update 3 telepítéséhez](storsimple-install-update-3.md) a StorSimple eszköz.

