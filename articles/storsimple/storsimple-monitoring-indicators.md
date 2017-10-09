---
title: "ellenőrzési mutatók aaaStorSimple |} Microsoft Docs"
description: "Hello fénykibocsátó dióda (LED) és a használt hallható riasztások toomonitor hello hello StorSimple eszköz állapotának ismerteti."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 59dee7b9-ca6d-4fd9-96e6-a0071e8d248e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: e690b8f4727272f5fbb8886a594a046f794a1380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-monitoring-indicators-toomanage-your-device"></a>Figyelési mutatók toomanage StorSimple használni az eszközét
## <a name="overview"></a>Áttekintés
A StorSimple eszköz fénykibocsátó dióda (LED) tartalmaz, és a riasztások használható toomonitor hello modulok és hello StorSimple eszköz általános állapotáról. ellenőrzési mutatók hello hello hardverösszetevők hello eszköz elsődleges ház és hello EBOD ház található. ellenőrzési mutatók hello LED vagy a hallható riasztások lehet.

Három LED használt állapotok tooindicate hello modul állapota van: zöld, villogó zöld toored sárga vagy piros-sárga.  

* Zöld LED működési állapotát képviselik.  
* Zöld toored-sárga villogó LED képviselő hello jelenléte nem kritikus feltételek, amelyeknek a felhasználói beavatkozást igényel.  
* Piros-sárga LED jelzi, hogy nincs-e hello modulon belül található kritikus hibát.  

hello a cikk hátralévő része ismerteti, különféle figyelési kijelző LED, hello StorSimple eszközön helyükre hello hello LED állapotok alapján hello Eszközállapot és hallható riasztások az esetleg hozzá tartozó.

## <a name="front-panel-indicator-leds"></a>Előlapi panel kijelző LED
hello előlapján, más néven hello *műveletek panel* vagy *ops panel*, hello összesített állapota minden hello modul hello rendszer jeleníti meg. hello előlapja hello elsődleges StorSimple és hello EBOD ház azonos, és az alábbi ábra szemlélteti.  

   ![Eszköz előlapi panel][1]

hello előlapja tartalmazza a következő mutatók hello:  

1. Némító gomb
2. Energiagazdálkodási kijelző LED-jét (zöld/piros-sárga)
3. A modul hibát jelző vezetett (a piros-sárga-és kikapcsolása)
4. Tartalék logikai mutató vezetett (a piros sárga KIKAPCSOLÁS
5. Egység azonosító megjelenítése  

hello hello előlapi panel LED hello eszköz és a hello EBOD ház jelentős különbség, hello **rendszer egység azonosítószámot** hello LED képernyőjén látható. hello alapértelmezett egység hello eszközön megjelenő azonosító **00**, mivel a hello alapértelmezett egység azonosítója: hello EBOD ház megjelenő **01**. Ez lehetővé teszi tooquickly megkülönböztetni hello eszköz és hello EBOD ház hello eszköz bekapcsolása. Ha az eszköz ki van kapcsolva, használja a található hello információk [kapcsolja be az új eszköz](storsimple-turn-device-on-or-off.md#turn-on-a-new-device) toodifferentiate hello eszköz hello EBOD ház.  

## <a name="front-panel-led-status"></a>Előlapi panel LED állapota
A következő tábla tooidentify hello állapotukat a hello LED hello előlapján hello eszköz vagy hello EBOD ház hello használata.  

| Rendszer energiagazdálkodási | A modul hiba | Logikai hiba | Riasztás | status |
| --- | --- | --- | --- | --- |
| Piros-sárga |KIKAPCSOLVA |KIKAPCSOLVA |N/A |AC power elveszett, a biztonsági mentési power vagy AC power működő és hello vezérlő modulok eltávolítva. |
| Zöld |ON |ON |N/A |(5s) panelen bekapcsolás OPS teszt állapota |
| Zöld |KIKAPCSOLVA |KIKAPCSOLVA |N/A |Bekapcsolás jó összes funkciók |
| Zöld |ON |N/A |PCM tartalék LED, ventilátor tartalék LED |Bármely PCM hibája, ventilátor hibája, vagy hőmérsékletű keresztül |
| Zöld |ON |N/A |I/o-modul LED |Bármely tartományvezérlő modul hiba |
| Zöld |ON |N/A |N/A |Ház logika hiba |
| Zöld |Flash |N/A |Modul állapota vezérlő modul vezetett. PCM tartalék LED, ventilátor tartalék LED |Ismeretlen vezérlő modultípus telepítve, I2C bus sikertelen, a tartományvezérlő modul létfontosságú termék data (VPD) konfigurációs hiba |

## <a name="power-cooling-module-pcm-indicator-leds"></a>Energiagazdálkodási modul (PCM) mutató LED hűtési
Energiagazdálkodási hűtési modul (PCM) mutató LED hello hello elsődleges ház vagy EBOD ház minden PCM modul oldalán található. Ez a témakör ismerteti hogyan toouse hello LED toomonitor hello állapotát a StorSimple eszközt a következő.  

* Az elsődleges ház hello PCM LED
* A hello EBOD ház PCM LED

## <a name="pcm-leds-for-hello-primary-enclosure"></a>Az elsődleges ház hello PCM LED
hello StorSimple eszköz egy 764W PCM modul egy további akkumulátor rendelkezik. hello alábbi ábrán hello LED panel hello eszközhöz.  

   ![Az elsődleges ház hello PCM LED][2]

Jelmagyarázat LED:

1. AC áramszünet esetén
2. Hiba ventilátor
3. Akkumulátor hiba
4. PCM OK
5. DC hiba
6. Jó akkumulátor  

hello állapotának PCM fel van tüntetve hello hello panel vezetett. hello eszköz PCM LED panel hat LED rendelkezik. Négy ezek LED hello tápegység és hello ventilátor hello állapotának megjelenítése. két LED fennmaradó hello hello biztonsági mentési akkumulátor modulja hello PCM hello állapotát jelzi. A következő táblák toodetermine hello állapotának hello PCM hello is használhatja.  

### <a name="pcm-indicator-leds-for-power-supply-and-fan"></a>PCM kijelző LED tápegység és ventilátor
| status | PCM OK (zöld) | AC sikertelen (sárga) | Ventilátor sikertelen (sárga) | Tartományvezérlő sikertelen (sárga) |
| --- | --- | --- | --- | --- |
| Nincs AC power (tooenclosure) |KIKAPCSOLVA |KIKAPCSOLVA |KIKAPCSOLVA |KIKAPCSOLVA |
| Nincs AC power (csak ez a PCM) |KIKAPCSOLVA |ON |KIKAPCSOLVA |ON |
| AC jelen PCM ON - OK |ON |KIKAPCSOLVA |KIKAPCSOLVA |KIKAPCSOLVA |
| PCM sikertelen (ventilátor sikertelen) |KIKAPCSOLVA |KIKAPCSOLVA |ON |N/A |
| PCM hiba (keresztül amp feszültség keresztül aktuális keresztül) |KIKAPCSOLVA |ON |ON |ON |
| PCM (ventilátor tolerancia kívül) |ON |KIKAPCSOLVA |KIKAPCSOLVA |ON |
| Készenléti kiszolgálói mód |Villogó |KIKAPCSOLVA |KIKAPCSOLVA |KIKAPCSOLVA |
| PCM belső vezérlőprogram letöltése |KIKAPCSOLVA |Villogó |Villogó |Villogó |

### <a name="pcm-indicator-leds-for-hello-backup-battery"></a>PCM mutató LED a biztonsági mentési hello akkumulátor
| status | Jó akkumulátor (zöld) | Akkumulátor hiba (sárga) |
| --- | --- | --- |
| Nem található akkumulátor |KIKAPCSOLVA |KIKAPCSOLVA |
| Jelen és megterhelni akkumulátor |ON |KIKAPCSOLVA |
| Akkumulátor díjszabási vagy karbantartás lezárása |Villogó |KIKAPCSOLVA |
| Akkumulátor "soft" hiba (helyreállítható) |KIKAPCSOLVA |Villogó |
| Akkumulátor "rögzített" hiba (helyreállíthatatlan) |KIKAPCSOLVA |ON |
| Ártalmatlanított akkumulátor |Villogó |KIKAPCSOLVA |

## <a name="pcm-leds-for-hello-ebod-enclosure"></a>A hello EBOD ház PCM LED
hello EBOD ház egy 580W PCM és további telep nem rendelkezik. hello PCM panelje hello EBOD ház rendelkezik kijelző LED csak a hello áramforrások és hello ventilátor. hello alábbi ábrán ezek LED.

   ![A hello EBOD ház PCM LED][3] 

A következő tábla toodetermine hello állapotának hello PCM hello is használhatja.  

| status | PCM OK (zöld) | AC sikertelen (sárga) | Ventilátor sikertelen (sárga) | Tartományvezérlő sikertelen (sárga) |
| --- | --- | --- | --- | --- |
| Nincs AC power (tooenclosure) |KIKAPCSOLVA |KIKAPCSOLVA |KIKAPCSOLVA |KIKAPCSOLVA |
| Nincs AC power (csak ez a PCM) |KIKAPCSOLVA |ON |KIKAPCSOLVA |ON |
| AC jelen PCM ON – OK |ON |KIKAPCSOLVA |KIKAPCSOLVA |KIKAPCSOLVA |
| PCM sikertelen (ventilátor sikertelen) |KIKAPCSOLVA |KIKAPCSOLVA |ON |X |
| (Keresztül amp keresztül feszültség keresztül aktuális PCM hiba |KIKAPCSOLVA |ON |ON |ON |
| PCM (ventilátor tolerancia kívül) |ON |KIKAPCSOLVA |KIKAPCSOLVA |ON |
| Készenléti modell |Villogó |KIKAPCSOLVA |KIKAPCSOLVA |KIKAPCSOLVA |
| PCM belső vezérlőprogram letöltése |KIKAPCSOLVA |Villogó |Villogó |Villogó |

## <a name="controller-module-indicator-leds"></a>A tartományvezérlő modul kijelző LED
hello StorSimple eszköz LED hello elsődleges tartományvezérlő és a hello EBOD vezérlő modulok tartalmazza.   

### <a name="monitoring-leds-for-hello-primary-controller"></a>Az elsődleges tartományvezérlő hello LED figyelése
hello alábbi ábrán látható könnyebb legyen azonosítani hello LED hello elsődleges tartományvezérlőn. (Hello összetevők mindegyike helyzetben felsorolt tooaid.)  

   ![Figyelési LED - elsődleges tartományvezérlő][4]

Hello vezérlő modul megfelelően működik-e a következő tábla toodetermine hello használata.  

### <a name="controller-indicator-leds"></a>A tartományvezérlő kijelző LED
| LED | Leírás |
| --- | --- |
| Azonosító LED-jét (kék) |Azt jelzi, hogy hello modul azonosít a rendszer. Ha hello kék LED van villogó egy futó tartományvezérlő, majd hello vezérlő hello aktív vezérlő pedig hello egy másikra hello készenléti vezérlő. További információkért lásd: [azonosítása hello aktív vezérlő az eszközön](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device). |
| Tartalék LED-jét (sárga) |Azt jelzi, hogy az hello controller hibát. |
| OK LED-jét (zöld) |Állandó zöld azt jelzi, hogy hello vezérlő OK gombra. A vezérlő VPD konfigurációs hiba zöld jelzi. |
| SAS-tevékenység LED (zöld) |Állandó zöld állapot azt jelzi, hogy nincs aktuális tevékenység kapcsolatot. Zöld azt jelzi, hello kapcsolat folyamatban lévő tevékenységek. |
| Ethernet állapot LED |Jobb oldali hivatkozást és a hálózati tevékenységet jelez: (állandó zöld) hivatkozás aktív, (zöld villogó) hálózati tevékenységet. Bal oldali jelzi a hálózati sebesség: (sárga) 1000 Mb/s (zöld) 100 Mb/s és (KIKAPCSOLT) 10 Mb/s. Attól függően, hogy hello összetevő modell a ritka villogni előfordulhat, hogy akkor is, ha hello hálózati illesztő nem engedélyezett. |
| POST LED |Azt jelzi, hello rendszerindítási folyamat, amikor hello vezérlő be van kapcsolva. Ha hello StorSimple eszköz tooboot nem sikerül, a LED Microsoft Support hello rendszerindítási folyamat számára, amellyel hello hiba történt a hello pontot azonosító segítségével. |

> [!IMPORTANT]
> Hello tartalék LED bekapcsolásával van, ha probléma van, előfordulhat, hogy orvosolható hello vezérlő újraindításával hello vezérlő modullal. Ha újraindítás hello vezérlő nem oldja meg a probléma, forduljon a Microsoft Support.  
> 
> 

### <a name="monitoring-leds-for-hello-ebod-ebod-enclosure"></a>Figyelés LED hello EBOD (EBOD ház)
Egyes hello 6 Gb/s SAS EBOD vezérlők annak állapotát jelző, ahogy az ábra a következő hello LED rendelkezik.  

  ![Figyelési LED - EBOD ház][5]

E hello EBOD vezérlő modul megfelelően működik-e a következő tábla toodetermine hello használata.  

### <a name="ebod-controller-module-indicator-leds"></a>EBOD vezérlő modul kijelző LED
| status | I/o-modul OK (zöld) | I/o-modul hibát (sárga) | Host port tevékenység (zöld) |
| --- | --- | --- | --- |
| A tartományvezérlő modul OK |ON |KIKAPCSOLVA |- |
| A tartományvezérlő modul hiba |KIKAPCSOLVA |ON |- |
| Külső host port kapcsolat |- |- |KIKAPCSOLVA |
| Külső host port kapcsolat – nincs tevékenység |- |- |ON |
| Külső host port kapcsolat - tevékenység |- |- |Villogó |
| A tartományvezérlő metaadatainak modulhiba |Villogó |- |- |

## <a name="disk-drive-indicator-leds-for-hello-primary-enclosure-and-ebod-enclosure"></a>Lemezmeghajtó kijelző LED hello elsődleges ház és EBOD ház
hello StorSimple eszköz hello elsődleges ház és hello EBOD házhoz, és meghajtó van. Egyes meghajtókon tartalmaz figyelési kijelző LED, ebben a szakaszban leírtak szerint. 

Hello lemezmeghajtókhoz, hello meghajtó állapotát jelzi egy zöld LED-jét, és a piros-sárga LED csatlakoztatott meghajtó szolgáltatónként modulokhoz hello elején. hello alábbi ábrán ezek LED.

  ![Lemezmeghajtó LED][6]

A következő tábla toodetermine hello állapotát minden meghajtó, amely viszont hatással van a teljes előlapja LED állapot hello hello használata.  

### <a name="disk-drive-indicator-leds-for-hello-ebod-enclosure"></a>Lemezmeghajtó mutató LED a hello EBOD ház
| status | Tevékenység OK LED (zöld) | Tartalék LED-jét (piros-sárga) | Ops panel LED társított |
| --- | --- | --- | --- |
| Nincs telepítve meghajtó |KIKAPCSOLVA |KIKAPCSOLVA |None |
| Telepített és működő meghajtó |A tevékenység be-és kikapcsolása villogó |X |None |
| SCSI ház szolgáltatások (SES) eszközt identitás beállítása |ON |A/1 második ki 1 másodperc villogó |None |
| SES eszköz tartalék bit beállítása |ON |ON |Logikai hiba (piros) |
| Áramszünet vezérlő áramkör |KIKAPCSOLVA |ON |A modul hibát (piros) |

## <a name="audible-alarms"></a>Hallható riasztások
A StorSimple eszköz hello elsődleges ház és a hello EBOD ház kapcsolódó hallható riasztásokat tartalmazza. Hallható figyelmeztető hello előlapja (más néven hello ops panel) mindkét házban található. hello hallható riasztás azt jelzi, ha jelen-e a hibaállapot esetén. a következő feltételek hello aktiválódik hello riasztás:  

* Ventilátor hiba vagy probléma
* Engedélyezett tartományon kívül esik feszültségérzékelő
* Felett vagy alatt hőmérséklet feltétel
* Hőmérséklet-túllépés
* Rendszer-hiba
* Logikai hiba
* Hálózati ellátási hiba
* A modul (PCM) hűtési teljesítménye eltávolítása  

hello következő táblázatban található hello különböző riasztás állapota.  

### <a name="alarm-states"></a>Riasztó állapotait
| Riasztási állapot | Műveletek | A művelet némító gombokra |
| --- | --- | --- |
| S0 |Normál mód: beavatkozás nélküli |E sípoló kétszer |
| S1 |Tartalék mód: 1 másodperc a/1 második kikapcsolása |Átmenet tooS2 vagy S3 (lásd: a Megjegyzés) |
| S2 |Emlékeztesse mód: időszakos hangjelzés |None |
| S3 |Elfedett mód: beavatkozás nélküli |None |
| S4 |Kritikus hiba mód: folyamatos riasztás |Nem érhető el: csend nem aktív |

> [!NOTE]
> * A riasztás állapota S1 Ha a nem gombra kattint némító 2 percen belül hello állapot automatikusan átkerül tooS2 vagy S3.  
> * Riasztó állapotait S1 tooS4 vissza tooS0 után hello hibaállapot esetén nincs bejelölve.  
> * Kritikus hiba állapot S4 más állapotból adható meg.  


Hello hallható riasztás is elnémíthatja hello ops panelen hello némító gomb lenyomásával. Automatikus némítása megtörténik a két perc után ha hello némító kapcsoló manuálisan nem működik. Amikor hello riasztás némított, továbbra is toosound a rövid időszakos hangjelzés tooindicate, hogy a probléma továbbra is fennáll. hello riasztás csendes lesz, ha az összes hello probléma adatútvonalai törlődtek.

hello következő táblázatban található hello különböző feltételek riasztás.

### <a name="alarm-conditions"></a>Riasztási feltételek
| status | Súlyosság | Riasztás | OPS LED panel |
| --- | --- | --- | --- |
| PCM riasztás – DC áramkimaradás egyetlen PCM |Hiba – a redundancia adatvesztés nélkül |S1 |A modul hiba |
| PCM riasztás – DC áramkimaradás egyetlen PCM |Hiba – a redundancia adatvesztés |S1 |A modul hiba |
| Sikertelen a PCM ventilátor |Hiba – a redundancia adatvesztés |S1 |A modul hiba |
| SBB modul PCM hiba észlelhető |Hiba |S1 |A modul hiba |
| PCM eltávolítása |Hiba történt a konfiguráció |None |A modul hiba |
| Ház konfigurációs hiba |Hiba – kritikus |S1 |A modul hiba |
| Alacsony hőmérséklet vonatkozó figyelmeztetés |Figyelmeztetés |S1 |A modul hiba |
| Magas hőmérséklet vonatkozó figyelmeztetés |Figyelmeztetés |S1 |A modul hiba |
| Hőmérséklet-riasztás keresztül |Hiba – kritikus |S1 |A modul hiba |
| I2C bus hiba |Hiba – a redundancia adatvesztés |S1 |A modul hiba |
| OPS kommunikációs hiba történt (I2C) panelen |Hiba – kritikus |S1 |A modul hiba |
| A tartományvezérlő hiba |Hiba – kritikus |S1 |A modul hiba |
| SBB felület modul hiba |Hiba – kritikus |S1 |A modul hiba |
| SBB felület modul hiba – nem működő modulok fennmaradó |Hiba – kritikus |S4 |A modul hiba |
| SBB felület modul eltávolítása |Figyelmeztetés |None |A modul hiba |
| Meghajtó hálózati vezérlő hiba |Figyelmeztetés – meghajtó power adatvesztés nélkül |S1 |A modul hiba |
| Meghajtó hálózati vezérlő hiba |Hiba – kritikus; meghajtó áramkimaradás |S1 |A modul hiba |
| Eltávolított meghajtó |Figyelmeztetés |None |A modul hiba |
| Nincs elegendő rendelkezésre álló energiagazdálkodási |Figyelmeztetés |Egyik sem |A modul hiba |

## <a name="next-steps"></a>Következő lépések
További információ [StorSimple hardverösszetevők és állapot](storsimple-8000-monitor-hardware-status.md).

[1]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE01.png
[2]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE02.png
[3]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE03.png
[4]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE04.png
[5]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE05.png
[6]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE06.png


