---
title: "aaaStorSimple műszaki specifikációk |} Microsoft Docs"
description: "Hello műszaki specifikációk és szabályozási előírások megfelelőségi információit hello StorSimple hardverösszetevők ismerteti."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 17ebf6b3-0872-4332-ac6e-074cc20a2b8e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: bb73661105dee7f6020a91f8c4f5abd6583023ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="technical-specifications-and-compliance-for-hello-storsimple-device"></a>Műszaki specifikációk és hello StorSimple eszköz megfelelőségét
## <a name="overview"></a>Áttekintés
a Microsoft Azure StorSimple eszköz hello hardverösszetevők toohello műszaki adatok és a cikkben ismertetett szabályozási előírások igazodik. hello műszaki specifikációk hello energia- és hűtési modulok (PCMs) meghajtók, a tárolási kapacitás, és házakat leírása hello megfelelőségi információk nemzetközi szabványok, a biztonsági és a kibocsátás és a kábelezést is vonatkozik.

## <a name="power-and-cooling-module-specifications"></a>Teljesítmény- és hűtési modul specifikációk
hello StorSimple eszköz két 100-240V kettős ventilátor SBB-kompatibilis Power hűtési modulok (PCMs) rendelkezik. Ez lehetővé teszi egy redundáns power konfigurációt. Egy PCM meghibásodásakor hello eszköz toooperate a szokásos módon folytatódik a hello más PCM, amíg hello modul nem cseréli le.  

hello EBOD ház egy 580 W PCM, és elsődleges ház egy 764 W PCM használja. hello alábbi táblázatokban lista hello műszaki specifikációk hello PCMs társított.

| Meghatározása | 580 W PCM (EBOD) | 764 W PCM (elsődleges) |
| --- | --- | --- |
| Maximális kimenő teljesítménye |580 W |764 |
| gyakoriság |50/60 Hz |50/60 Hz |
| A feszültség tartomány kiválasztása |Automatikus beállításnak: 90 – 264 V AC, 47/63 Hz |Automatikus beállításnak: 90-264 V AC, 47/63 Hz |
| Aktuális maximális behatolása |20 A |20 A |
| Energiagazdálkodási tényező javítása |> 95 % névleges bemeneti feszültségtartomány |> 95 % névleges bemeneti feszültségtartomány |
| Harmonikus |Megfelel-e EN61000-3-2 |Megfelel-e EN61000-3-2 |
| Kimenet |2.0 A @ 5V készenléti feszültségérzékelő |2.7 A @ 5V készenléti feszültségérzékelő |
| + 5 V 42 A |+ 5 V 40 A | |
| + @ 38 12 A |+ @ 38 12 A | |
| Moduláris közbeni |Igen |Igen |
| Kapcsolók és LED |Be-és AC kapcsoló és a négy állapotjelzője LED |Be-és AC kapcsoló és a hat állapotjelzője LED |
| Ház hűtési |Tengelyirányú hűtési változó ventilátorvezérlésre sebessége a ventilátor |Tengelyirányú hűtési változó ventilátorvezérlésre sebessége a ventilátor |

## <a name="power-consumption-statistics"></a>Energiagazdálkodási felhasználási statisztikát
a következő táblázat hello jeleníti meg tipikus energiafogyasztási adatokkal hello (tényleges értékek eltérőek lehetnek közzétéve hello) hello StorSimple eszköz különböző modellek. 

| Feltételek | 240 V AC | 240 V AC | 240 V AC | 110 V AC | 110 V AC | 110 V AC |
| --- | --- | --- | --- | --- | --- | --- |
|  Üresjárati lassú, meghajtók ventilátor |1.45 A |0.31 kW |1057.76 BTU/hr |3.19. A |0.34 kW |1160.13 BTU/hr |
|  Ventilátorok lassú, meghajtók használata |1.54 A |0.33 kW |1126.01 BTU/hr |3.27 A |0.36 kW |1228.37 BTU/hr |
|  Ventilátorok gyors, meghajtók üresjárati, két PSUs kapcsolva |2.14 A |0.49 kW |1671.95 BTU/hr |4.99 A |0.54 kW |1842.56 BTU/hr |
|  Gyors és ventilátorok meghajtók üresjárati, egy PSU kapcsolva egy inaktív |2.05 A |0.48 kW |1637.83 BTU/hr |4.58 A |0,50 kW |1706.07 BTU/hr |
|  Ventilátorok fér hozzá, az alkalmazás bekapcsolja két PSUs gyors és meghajtók |2.26 PONTBAN A |0,51 kW |1740.19 BTU/hr |4.95 A |0.54 kW |1842.56 BTU/hr |
|  Gyors ventilátorok meghajtók fér hozzá, egy PSU kapcsolva egy inaktív |2.14 A |0.49 kW |1671.95 BTU/hr |4.81 A |0.53 kW |1808.44 BTU/hr |

## <a name="disk-drive-specifications"></a>Lemezmeghajtó specifikációk
A StorSimple eszköz legfeljebb too12 3,5 hüvelykes űrlap tényező soros csatlakozású SCSI (SAS) meghajtók támogat. hello tényleges meghajtókat is szerepeljenek SSD-meghajtót (SSD) vagy a merevlemezes (HDD) meghajtók, hello termék konfigurációjától függően. 12 tárhelyek hello a lemezmeghajtó elé hello ház 3 által 4 konfigurációban található. További tárhely az egy másik 12 lemezmeghajtók hello EBOD ház teszi lehetővé. Ezek a HDD-k mindig.  

## <a name="storage-specifications"></a>Tárolási specifikációk
hello StorSimple eszközön merevlemez-meghajtók és mindkét hello 8100 és 8600 SSD-meghajtó van. teljes használható kapacitás hello hello 8100 és 8600 a rendszer körülbelül 15 TB és 38 TB kulcsattribútumokkal. a következő táblázat hello dokumentumok felhő kapacitása a StorSimple megoldás kapacitás hello hello környezetében, SSD és HDD hello részleteit.

| Eszközmodell / kapacitás | 8100 | 8600 |
| --- | --- | --- |
| Merevlemezes (HDD) meghajtók száma |8 |19 |
| SSD-meghajtót (SSD-k) száma |4 |5 |
| Egyetlen HDD-kapacitás |4 TB |4 TB |
| Egyetlen SSD kapacitása |400 GB |800 GB |
| Tartalék kapacitás |4 TB |4 TB |
| Használható HDD-kapacitás |14 TB |36 TB |
| Használható SSD kapacitása |800 GB |2 TB |
| Teljes használható kapacitás * |~ 15 TB |~ 38 TB |
| Megoldás maximális kapacitás (például felhő) |200 TB |500 TB |

<sup>* </sup>- *hello használható kapacitásáig hello elérhető adatokat, metaadatokat, és kapacitás pufferek tartalmazza.*

## <a name="enclosure-dimensions-and-weight-specifications"></a>Ház dimenziókat és súlyozás specifikációk
a következő táblák listában hello hello különböző ház specifikációk dimenziók és a súlyozást.  

### <a name="enclosure-dimensions"></a>Ház dimenziók
hello következő táblázatban hello ház milliméterben és hüvelyk hello méretei.

| Ház | Milliméterben | Hüvelyk |
| --- | --- | --- |
| Magassága |87.9 |3.46 |
| Szélesség bordában között |483 |19.02 |
| Ház törzsét között szélessége |443 |17.44 |
| Az első csatlakoztatását a nyomkarima tooextremity ház törzse mélysége |577 |22.72 |
| Mélysége a Műveletek panel ház toofurthest vége |630.5 |24.82 |
| Ház nyomkarima toofurthest részén csatlakoztassa mélysége |603 |23.74 |

### <a name="enclosure-weight"></a>Ház súlyozás
Hello konfigurációjától függően a teljes betöltése elsődleges ház is mérjük a 21 too33 kgs- és két személyek toohandle igényli azt. 

| Ház | Súlyozás |
| --- | --- |
| Maximális súly (hello konfigurációjától függ) |30 kg – 33 kg |
| Üres (nincs felszerelt meghajtó) |21 – 23 kg |

## <a name="enclosure-environment-specifications"></a>Ház környezet specifikációk
Ez a szakasz hello specifikációk kapcsolódó toohello ház környezet. ebbe a kategóriába tartozó vizsgálja hello hőmérséklet, páratartalom, magasság, ütés, vibráció, tájolás, biztonsági és elektromágneses kompatibilitási (EMC).  

### <a name="temperature-and-humidity"></a>Hőmérséklet és a páratartalom
| Ház | Környezeti hőmérséklet-tartomány | Környezeti relatív nedvességtartalma | Maximális nedves tartályának |
| --- | --- | --- | --- |
| Működési |5 C - 35 C (41°-F - 95° F) |80 %-os 20 %-át nem-kondenzációs- |28 C (82° F) |
| Nem működő |-40 C - 70° C (40°-F - 158° F) |5-100 % nem kicsapódó |29 C (84° F) |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Légmozgás, magasság, ütés, vibráció, tájolás, biztonsági és EMC
| Ház | Működési specifikációk |
| --- | --- |
| A légmozgás |Rendszer légmozgás első toorear. Rendszer alacsony nyomású, hátsó-kipufogógáz telepítést kell működtetni. Állvány ajtók és akadályok által létrehozott hátsó nyomás legfeljebb 5 pascalban (0,5 mm vízjel mérőműszer). |
| Működési magasság |-30 mérőszámok too3045 mérőszámok (-100 láb too10, 000 láb) a legnagyobb üzemi hőmérséklet deszerializálni besorolású által 5 C 7000 láb fent. |
| Magasság nem működő |-305 mérőszámok too12, 192 mérőszámok (-1,000 láb too40, 000 láb) |
| Ütés, működési |5 g. 10 ms ½ szinusz |
| Ütés, nem működő |30g 10 ms ½ szinusz |
| Működési vibráció |0.21g RMS 5-500 Hz véletlenszerű |
| Nem működő vibráció |1.04-es g RMS 2-200 Hz véletlenszerű |
| Vibráció, áttelepítése |3g-2-200 Hz szinusz |
| Tájolás és csatlakoztatása |19" állványra szerelheti (2 EIA egység) |
| Állvány sínek |toofit minimális 700 mm (31.50 hüvelyk) mélysége állványok IEC 297 megfelelő |
| Biztonsági és jóváhagyásokat |CE és UL EN 61000-3 IEC 61000-3, UL 61000-3 |
| EMC |EN55022 (CISPR - A), FCC A |

## <a name="international-standards-compliance"></a>Nemzetközi szabványoknak való megfelelés
A Microsoft Azure StorSimple eszköz megfelel a következő nemzetközi szabványok hello:  

* CE - EN 60950-1  
* CB jelentés tooIEC 60950-1  
* UL és cUL tooUL 60950-1  

## <a name="safety-compliance"></a>Biztonsági megfelelőség
A Microsoft Azure StorSimple eszköz megfelel-e a következő safety minősítések hello:  

* Rendszer termék típus jóváhagyási: UL, cUL, CE  
* Biztonsági megfelelőségi: UL 60950, IEC 60950, EN 60950  

## <a name="emc-compliance"></a>EMC megfelelőségi
A Microsoft Azure StorSimple eszköz megfelel-e a következő EMC minősítések hello.  

### <a name="emissions"></a>Kibocsátott
hello eszköz vezetett és kisugárzott kibocsátási EMC-kompatibilis.  

* Vezetett kibocsátott korlátozása szintek: CFR 47 15B osztály A EN55022 osztály A CISPR osztály a.  
* Kisugárzott kibocsátott korlátozása szintek: CFR 47 15B osztály A EN55022 osztály A CISPR osztály a.   

### <a name="harmonics-and-flicker"></a>Harmonikus és Villódzás
hello eszköz EN61000-3-2/3 megfelel.  

### <a name="immunity-limit-levels"></a>Mentesség korlát szintek
hello eszköz EN55024 megfelel.  

## <a name="ac-power-cord-compliance"></a>AC power kábellel megfelelőségi
hello plug és hello teljes power kábellel szerelvény meg kell felelnie a hello szabványoknak megfelelő hello ország mely hello eszköz van használatban, és biztonsági jóváhagyások, amelyek elfogadható ebben az országban kell rendelkezniük. hello alábbi táblázatokban hello amerikai és Európai lista szabványainak.  

### <a name="ac-power-cords---usa-must-be-nrtl-listed"></a>AC tápkábelek - USA (felsorolt NRTL kell lennie)
| Összetevő | Meghatározása |
| --- | --- |
| Kábellel típusa |SV vagy SVT, 18 AWG minimális, 3 vezető 2.0 mérőszámok maximális hossza |
| Plug |NEMA 5-15P résidőalapban-típust mellékletfájl plug besorolású 120 V, 10 A; vagy IEC 320 14, 250 V, A 10 |
| A szoftvercsatorna |IEC 320 C-13, 250 V, 10 A |

### <a name="ac-power-cords---europe"></a>AC tápkábelek - Európa
| Összetevő | Meghatározása |
| --- | --- |
| Kábellel típusa |Összehangolt, H05-VVF-3G1.0 |
| A szoftvercsatorna |IEC 320 C-13, 250 V, 10 A |

## <a name="supported-network-cables"></a>Támogatott hálózati kábel
Hello 10 GbE hálózati adapterek, a DATA 2 és a DATA 3, tekintse meg a toohello [támogatott hálózati kábelek és a modulok listáját](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

## <a name="next-steps"></a>Következő lépések
Most már áll készen toodeploy a StorSimple eszköz az adatközpontban található. További információkért lásd: [központi telepítése a helyszíni eszközök](storsimple-deployment-walkthrough-u2.md).  

