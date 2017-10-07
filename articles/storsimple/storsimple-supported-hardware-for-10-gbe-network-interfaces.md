---
title: "StorSimple 10 GbE-illesztőkre vonatkozó aaaHardware |} Microsoft Docs"
description: "Hello támogatott kis méretet moduláris (SFP) adó, kábelek és kapcsolók hello 10 GbE hálózati adapterek a ismerteti a StorSimple eszköz."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: df8d40c7-f5ad-4f84-93eb-779fbd5f7243
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/21/2016
ms.author: alkohli
ms.openlocfilehash: 3e6769380040ab08d9a57ef7221bf62c6c855137
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="supported-hardware-for-hello-10-gbe-network-interfaces-on-your-storsimple-device"></a>Támogatott hardverek hello 10 GbE hálózati adapterek a StorSimple eszköz
## <a name="overview"></a>Áttekintés
Ez a cikk tájékoztatást ad azokról a kiegészítő hardver, amely kompatibilis a Microsoft Azure StorSimple eszközt.

## <a name="list-of-devices-tested-by-microsoft"></a>A Microsoft tesztelte eszközök listája
A Microsoft tesztelte a következő kis méretet moduláris (SFP) adó, kábelek és kapcsolók tooensure, hogy akkor működik optimálisan eszközök hello. (a következő táblák hello frissíti, az új hardver lett tesztelve.)

### <a name="sfp-transceivers"></a>SFP + adó
| Ellenőrizze | Modell |
| --- | --- |
| Cisco |SFP-10G-SR |

### <a name="cables"></a>Kábelek.
| S. Nem. | Ellenőrizze | Modell |
| --- | --- | --- |
| 1. |Cisco |SFP-H10GB-CU1M |
| 2. |Cisco |SFP-H10GB-CU2M |
| 3. |Cisco |SFP-H10GB-CU3M |
| 4. |Tripp Lite |N820 - 05M (OM3) |

### <a name="switches"></a>Kapcsolók
| S. Nem. | Ellenőrizze | Modell |
| --- | --- | --- |
| 1. |Cisco |N3K-C3172PQ-10GE |
| 2. |Cisco |N3K-C3048-ZM-F |
| 3. |Cisco |N5K-C5596UP-BE |

## <a name="list-of-devices-tested-in-hello-field"></a>Az eszközök listájának hello mezőben tesztelése
Ez a szakasz az eszközöket, amelyek sikeresen telepítették hello mezőben StorSimple ügyfelek hello listája. Ezek a Microsoft által nem tesztelt, de valószínűleg toowork a StorSimple eszközt a rendszer.

| Paraméter | Érték |
| --- | --- |
| Ellenőrizze a kapcsoló |Juniper |
| Kapcsoló modell |ex4550-32F |
| Kapcsoló operációs rendszer verziója |JunOS 12.3R9.4 |
| Panel modell |Portok bevezetésében (PIC 0) |
| Adó ellenőrizze |Juniper |
| Adó modell |740-021308 cikkszám <br></br> 740-030658 cikkszám |
| Adó belső vezérlőprogram-verziója |Rev 01 verzió 0,0 (jelentett) |
| Kábel modell |Duplex átkötés LC/LC 50/125µ, OM3, LSZH |
| StorSimple-modell |8600 |
| StorSimple szoftver verziója |6.3.9600.17491 |

## <a name="list-of-devices-tested-by-oem-provider-mellanox"></a>OEM-szolgáltató (Mellanox) tesztelte eszközök listája
Mellanox kis méretet moduláris (SFP) adó, kábelek és kapcsolók tooensure, hogy akkor működik optimálisan Mellanox hálózati felületek, például hello 10 GbE hálózati adapterek a StorSimple eszközt a következő hello tesztelte.

### <a name="cables-and-modules-supported-by-mellanox"></a>Kábelek és Mellanox által támogatott modulok
hello következő táblázatban hello kábelek és modulok Mellanox által támogatott. Ezek a Microsoft által nem tesztelt, de valószínűleg toowork a StorSimple eszközt a rendszer.

| S. Nem. | Gyorsaság | Modell | Leírás | Ellenőrizze |
| --- | --- | --- | --- | --- |
| 1. |10 GbE |CAB-SFP-SFP - 1M |passzív réz kábel SFP + 10 Gb/s-1m |Arista |
| 2. |10 GbE |CAB-SFP-SFP - 2M |passzív réz kábel SFP + 10 Gb/s 2m |Arista |
| 3. |10 GbE |CAB-SFP-SFP - 3M |passzív réz kábel SFP + 10 Gb/s 3m |Arista |
| 4. |10 GbE |CAB-SFP-SFP - 5M |passzív réz kábel SFP + 10 Gb/s 5m |Arista |
| 5. |10 GbE |Cisco SFP-H10GBCU1M |Cisco SFP + kábel |Cisco |
| 6. |10 GbE |Cisco SFP-H10GBCU3M |Cisco SFP + kábel |Cisco |
| 7. |10 GbE |Cisco SFP-H10GBCU5M |Cisco SFP + kábel |Cisco |
| 8. |10 GbE |J9281B HP X242 10 G |SFP + tooSFP + 1m közvetlen csatlakoztatása réz kábel |HP |
| 9. |10 GbE |HP BLc 455883-B21 |10 GB-os SR SFP + Opt |HP |
| 10. |10 GbE |HP BLc 455886-B21 |10 GB-os LR SFP + Opt |HP |
| 11. |10 GbE |HP BLc 487649-B21 |SFP + 0,5 m 10GbE réz kábel |HP |
| 12. |10 GbE |HP BLc 487652-B21 |1m 10GbE SFP + réz kábel |HP |
| 13. |10 GbE |HP BLc 487655-B21 |SFP + 3m 10GbE réz kábel |HP |
| 14. |10 GbE |HP BLc 487658-B21 |SFP + 7m 10GbE réz kábel |HP |
| 15. |10 GbE |HP BLc 537963-B21 |SFP + 5m 10GbE réz kábel |HP |
| 16. |10 GbE |AP784A HP |3m C-sorozat passzív réz SFP + kábel |HP |
| 17. |10 GbE |AP785A HP |5m C-sorozat passzív réz SFP + kábel |HP |
| 18. |10 GbE |AP818A HP |1m B sorozatú aktív réz SFP + kábel |HP |
| 19. |10 GbE |AP819A HP |B sorozatú aktív réz SFP + kábel 3m |HP |
| 20. |10 GbE |J9150A HP |X132 10 G SFP + LC SR adó |HP |
| 21. |10 GbE |J9151A HP |X132 10 G SFP + LC LR adó |HP |
| 22. |10 GbE |J9283B HP |X242 10 G SFP + SFP + 3 m DAC kábel |HP |
| 23. |10 GbE |J9285B HP |X242 10 G SFP + SFP + 7 m DAC kábel |HP |
| 24. |10 GbE |JD095B HP |X240 10 G SFP + SFP + 0,65 m DAC kábel |HP |
| 25. |10 GbE |JD096B HP |X240 10 G SFP + SFP + 1,2 millió DAC kábel |HP |
| 26. |10 GbE |JD097B HP |X240 10 G SFP + SFP + 3 m DAD kábel |HP |
| 27. |10 GbE |MAM1Q00A-QSA Mellanox |QSFP tooSFP + Adapter |Mellanox technológiák |
| 28. |10 GbE |MC2309124-006 Mt |Passzív réz kábel 1 x SFP + tooQSFP 10Gb/s 24awg 7m |Mellanox technológiák |
| 29. |10 GbE |MC2309124-007 Mt |Passzív réz kábel 1 x SFP + tooQSFP 10Gb/s 24awg 7m |Mellanox technológiák |
| 30. |10 GbE |MC2309130-003 Mt |Passzív réz kábel 1 x SFP + tooQSFP 10Gb/s 30awg 3m |Mellanox technológiák |
| 31. |10 GbE |MC2309130-00A Mt |Passzív réz kábel 1 x SFP + tooQSFP 10Gb/s 30awg 0,5 m |Mellanox technológiák |
| 32. |10 GbE |MC3309124-005 Mt |Passzív réz Bekábelezése 1 x SFP+ 10 Gb/s 24awg 5 m |Mellanox technológiák |
| 33. |10 GbE |MC3309124-007 Mt |Passzív réz Bekábelezése 1 x SFP+ 10 Gb/s 24awg 7 m |Mellanox technológiák |
| 34. |10 GbE |MC3309130-003 Mt |Passzív réz Bekábelezése 1 x SFP+ 10 Gb/s 30awg 3 m |Mellanox technológiák |
| 35. |10 GbE |MC3309130-00A Mt |Passzív réz Bekábelezése 1 x SFP+ 10 Gb/s 30awg 0,5 m |Mellanox technológiák |

### <a name="switches-supported-by-mellanox"></a>Mellanox által támogatott kapcsolók
a következő táblázat hello kapcsolókat hello Mellanox által támogatott. Ezek a Microsoft által nem tesztelt, de valószínűleg toowork a StorSimple eszközt a rendszer.

| S. Nem. | Gyorsaság | Modell | Leírás | Ellenőrizze |
| --- | --- | --- | --- | --- |
| 1. |10GbE |516733-B21 |HP ProCurve 6120XG 10GbE Ethernet panel kapcsoló |HP |
| 2. |10GbE |538113-B21 |HP 10GbE csatlakoztatott modul (PTM) |HP |
| 3. |10GbE |EN4093 |IBM PureFlex rendszer háló EN4093 10 gigabites méretezhető kapcsoló modul |IBM |
| 4. |1 gbe |3020 |Cisco katalizátor 3020 1 gbe kapcsoló panel |Cisco |
| 5. |1 gbe |3020 X |Cisco katalizátor 3020 X 1 gbe kapcsoló panel |Cisco |
| 6. |1 gbe |438030-B21 |HP 1 gbe kapcsoló modul - GbE2c 2 vagy 3. rétegbeli Ethernet panel Váltás |HP |
| 7. |1 gbe |6120G |HP ProCurve 6120G/XG 1 gbe kapcsoló panel |HP |

## <a name="next-steps"></a>Következő lépések
[Ismerje meg, további információ a StorSimple hardverösszetevők és állapot](storsimple-monitor-hardware-status.md).

