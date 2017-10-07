---
title: "aaaStorSimple 8000 Series Update 2 kibocsátási megjegyzései |} Microsoft Docs"
description: "Hello új funkciókat, problémák és megoldások ismerteti a StorSimple 8000 Series Update 2."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: e2c8bffd-7fc5-4b77-b632-a4f59edacc3a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/24/2016
ms.author: v-sharos
ms.openlocfilehash: 36c75aad900c7b1286a924732967b8ee519a3d4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-update-2-release-notes"></a>A StorSimple 8000 Series Update 2 kibocsátási megjegyzései
## <a name="overview"></a>Áttekintés
hello alábbi kibocsátási megjegyzések leírják hello új szolgáltatásokat és a StorSimple 8000 Series Update 2 hello kritikus megnyitott problémák azonosításához. Hello StorSimple szoftverek listáját, az illesztőprogramok és a lemez belső vezérlőprogram-frissítésekre ebben a kiadásban szereplő is tartalmaznak. 

Frissítés 2 alkalmazott tooany StorSimple eszköz frissítés 1.2-es kiadás (GA) vagy a frissítés 0,1 futási lehet. 2. frissítés társított hello eszköz verziója 6.3.9600.17673.

Tekintse át a hello kibocsátási megjegyzések hello központi telepítése előtt frissítse a StorSimple megoldásban lévő hello információt.

> [!IMPORTANT]
> * Megközelítőleg 4 – 7 óra tooinstall veszi ezt a frissítést (beleértve a Windows hello-frissítések). 
> * 2. frissítés szoftver, LSI illesztőprogram és SSD-belső vezérlőprogram-frissítésekre rendelkezik.
> * Új kiadásokban nem láthatók frissítések azonnal, mert a hello frissítések fázisokra bontva történő bevezetéséhez végezzük. Néhány napot várni, és a frissítések újra ezeket a majd vizsgálatát hamarosan elérhető lesz.
> 
> 

## <a name="whats-new-in-update-2"></a>2. frissítés újdonságai
Frissítés 2 hello a következő új funkciókat vezet be.

* **Helyileg rögzített kötetek** – hello StorSimple 8000 series korábbi kiadásaiban, adatblokkjait volt rétegzett toohello felhő-használata alapján. Nincs módja tooguarantee, amely blokkolja volna hagyja el a helyi történt. A 2. frissítést a kötet létrehozásakor jelölhet ki egy kötetet, a kötetről helyileg rögzített, és az elsődleges adatok nem lesznek rétegzett toohello felhő. A helyileg rögzített kötetek pillanatképek marad másolt toohello felhőalapú biztonsági mentés, így hello felhő alkalmas adatok mobilitás és a katasztrófa utáni helyreállítás céljából. Emellett módosíthatja hello kötettípus (Ez azt jelenti, hogy alakítsa át a rétegzett kötetek toolocally rögzített kötetek, convert helyileg rögzített kötetek tootiered). 
* **A StorSimple virtuális eszköz fejlesztései** – korábban, a StorSimple 8000 series hello elhelyezni, a vész-helyreállítási vagy fejlesztési és tesztelési célú megoldást hello virtuális eszköz. Hiba történt a virtuális eszköz (1100-as modell) csak egy modell. 2. frissítés két virtuáliseszköz-modellek vezet be: 
  
  * 8010-es (korábbi nevükön hello 1100-as) – nincs változás; 30 TB kapacitású, és az Azure standard szintű tárolást használ.
  * 8020-as modellt – 64 TB kapacitású rendelkezik, és a prémium szintű tárolást használ javítja a teljesítményt.
    
    Nincs egyetlen virtuális merevlemez mind a virtuális eszköz két modellben (8010-es/8020-as modell). Hello virtuális eszköz első indításakor hello platform paraméterek észleli, és megfelelő modellverziója hello vonatkozik.
* **Hálózati fejlesztéseket** – 2. frissítés tartalmazza a következő hálózati fejlesztéseket hello:
  
  * Több hálózati adapter hello felhő engedélyezhető, hogy a feladatátvétel akkor fordulhat elő, ha meghibásodik egy hálózati Adaptert.
  * Útválasztási tovább fejlődtek, a felhő rögzített metrikák blokkok engedélyezve van.
  * Online módon tett újrapróbálkozás sikertelen erőforrások a feladatátvétel előtt.
  * Új riasztások service hibák.
* **Frissítési fejlesztései** – frissítse 1.2-es és korábbi verzióiban hello StorSimple 8000 series két csatornákon keresztül frissült: Windows frissítés fürtözés, a iSCSI, és a stb és a Microsoft Update bináris fájljait és a belső vezérlőprogram.
    2. frissítés a Microsoft Update használja, minden frissítés. Ez a kell vezethet tooless idő a javítás vagy a feladatátvételek során. 
* **Belső vezérlőprogram-frissítésekre** – hello belső vezérlőprogramjának következő frissítések tartoznak:
  
  * LSI: lsi_sas2.sys 2.00.72.10 termékverziója
  * Csak SSD (HDD frissítések): XMGG, XGEG, KZ50, F6C2 és VR08
* **Proaktív támogatási** – 2. frissítés lehetővé teszi, hogy a Microsoft toopull további diagnosztikai adatokat hello eszközről. Ha a műveleti csapata az eszközöket, amelyek problémák vannak, azt jobb felszerelt toocollect információkkal hello eszközről és problémák elemzéséhez. **2. frissítés elfogadásával, hogy lehetővé teszik a számunkra tooprovide a proaktív támogatás**.    

## <a name="issues-fixed-in-update-2"></a>A 2. frissítésben megoldott problémák
a következő táblák hello frissítések 2 javított problémák összegzését tartalmazza.    

| Nem. | Szolgáltatás | Probléma | Érvényes toophysical eszköz | Érvényes toovirtual eszköz |
| --- | --- | --- | --- | --- |
| 1 |Hálózati illesztők |Után egy frissítési tooUpdate 1 hello StorSimple Manager szolgáltatás jelentette, hogy hello adat2 és Data3 portok nem egy tartományvezérlőn. Ez a hiba kijavítása. |Igen |Nem |
| 2 |Frissítések |Egy frissítési tooUpdate 1 után hallható riasztások történt hello több eszközre a klasszikus Azure portálon. Ez a hiba kijavítása. |Igen |Nem |
| 3 |Openstack hitelesítés |Ha használatával Openstack a felhőbeli szolgáltató, sikerült hibaüzenet, hogy a felhőbeli hitelesítési karakterlánc túl hosszú volt-e. A probléma javítását. |Igen |Nem |

## <a name="known-issues-in-update-2"></a>Ismert problémák a 2. frissítés
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
| 20 |Helyileg rögzített kötetek |Ha rétegzett kötetet (létrehozott és 1.2-es frissítés a klónozott vagy korábbi) tooconvert tooa helyileg rögzített kötet és az eszköz nincs elegendő szabad terület vagy egy felhőalapú leállás van, majd hello clone(s) sérülését. |A probléma csak a köteteket, amelyek a szoftverfrissítések létrehozott és a klónozott frissítés előtti 2. Ez egy ritka eset legyen. | | |
| 21 |Kötet átalakítás |Nem frissíthető hello ACRs csatolt tooa kötet, amíg folyamatban van egy kötet átalakítás (rögzített rétegzett toolocally vagy fordítva). Hello ACRs frissítése adatsérülést eredményezhet. |Szükség esetén – hello ACRs előzetes toohello kötet átalakítás frissítése, és ne végezzen további ACR frissítések hello átalakítás közben. | | |

## <a name="controller-and-firmware-updates-in-update-2"></a>A tartományvezérlő és a belső vezérlőprogram frissítések Update 2.
Ebben a kiadásban frissíti hello illesztőprogram és hello lemez belső vezérlőprogram az eszközön.

* Tájékozódhat hello LSI belső vezérlőprogram frissítése, olvassa el a Microsoft Tudásbázis 3121900. 
* Tájékozódhat hello lemez belső vezérlőprogram frissítése, olvassa el a Microsoft Tudásbázis 3121899.

## <a name="virtual-device-updates-in-update-2"></a>A 2. frissítésben virtuális eszköz frissítése
A frissítés alkalmazott toohello virtuális eszköz nem lehet. Új virtuális eszközök létrehozott toobe kell. 

## <a name="next-step"></a>Következő lépés
Ismerje meg, hogyan túl[2. frissítés telepítése](storsimple-install-update-2.md) a StorSimple eszköz.

