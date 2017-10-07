---
title: "a dolgok ajánlott biztonsági eljárások aaaInternet |} Microsoft Docs"
description: "hello a cikk a Microsoft Internet dolgot ajánlott biztonsági eljárások és általános javaslatok válogatott listáját tartalmazza."
services: security
documentationcenter: na
author: TomShinder
manager: StevenPo
editor: TomSh
ms.assetid: 2d5598c5-4c30-481d-b8f4-51ee024ea9a7
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: yurid
ms.openlocfilehash: 7ee31c912e8ac230ffa5efcd5b4c2b0b0713584f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="internet-of-things-security-best-practices"></a>Eszközök internetes dolgot ajánlott biztonsági eljárások
Rendelkezésre áll egy kritikus vállalkozás bárki vállalniuk az IoT-megoldások biztonságossá tétele hello az eszközök internetes hálózatát (IoT) infrastruktúra. Hello érintett eszközök száma és hello miatt ezeken az eszközökön hello hatás biztonsági esemény elosztott jellege kapcsolatos, az IoT-eszközök millióira toocompromise nem triviális, és a széleskörű hatással lehet.

Emiatt az IoT biztonsági kell a biztonsági jellegű megközelítést. Adatok igényeinek toobe biztonságos hello felhőben és, mert privát és nyilvános hálózatokon helyezi át. Metódusok hívására van szükség, hogy a hely toosecurely rendelkezés hello az IoT-eszközök maguk toobe. Minden egyes rétegben, eszköz, toonetwork, toocloud háttér-erős biztonságot biztosítékok kell.

Az IoT-ajánlott eljárások a hello a következő módon csoportosíthatók:

* Az IoT-hardvergyártó vagy integráló
* Az IoT-megoldás fejlesztői
* Az IoT-megoldás deployer
* Az IoT-megoldás operátor

Ez a cikk összefoglalja [Internet a dolgok ajánlott biztonsági eljárások](../iot-suite/iot-security-best-practices.md). További részletes információt toothat cikkben tájékozódhat.

## <a name="iot-hardware-manufacturer-or-integrator"></a>Az IoT-hardvergyártó vagy integráló
Kövesse az alábbi gyakorlati tanácsok hello egy IoT hardver gyártás vagy egy hardveres integráló:

* **Hatókör toominimum hardverkövetelmények**: hello hardver tervezési hello hardver, és semmi további művelethez szükséges minimális funkciók kell tartalmaznia. 
* **Ellenőrizze a hardver igazolása védelmet**: build a mechanizmusok toodetect fizikai illetéktelen módosításának hardverek, például a megnyitása hello eszköz borítóján eltávolítása egy részét hello eszköz stb. 
* **Biztonságos hardveren körül Build**: Ha [ELÁBÉ](https://en.wikipedia.org/wiki/Cost_of_goods_sold) lehetővé teszik, készíthetnek biztonsági funkciókat, például a biztonságos és titkosított tárolási és a platformmegbízhatósági modul TPM-alapú rendszerindítási funkcióit.
* **Frissítések biztonságosabbá teheti**: előbb vagy utóbb elkerülhetetlenné belső vezérlőprogram frissítése hello eszköz élettartama alatt.

## <a name="iot-solution-developer"></a>Az IoT-megoldás fejlesztői
Kövesse az alábbi gyakorlati tanácsok hello egy IoT-megoldás fejlesztők:

* **Hajtsa végre a biztonságos software development módszert**: biztonságos szoftver fejlesztése ground up gondolat biztonsági hello kezdetektől hello projekt összes hello módon tooits végrehajtására, tesztelése és telepítése szükséges.
* **A kiválasztott nyílt forráskódú szoftver körültekintően**: nyílt forráskódú szoftvereket lehetőséget kínál a tooquickly megoldások fejlesztése.
* **Integrálható, így gondossággal járjanak**: hello szoftver biztonsági hiányosságokat számos hello határt a könyvtárakat és API-k lehet létrehozni. 

## <a name="iot-solution-deployer"></a>Az IoT-megoldás deployer
Ha Ön egy IoT-megoldás deployer, kövesse a hello ajánlott eljárásokat az alábbi:

* **Hardver biztonságos telepítése**: IoT-telepítések nem biztonságos helyen, például a nyilvános szóközt vagy felügyeletlen területi telepített hardver toobe lehet szükség.
* **Hitelesítési kulcsok biztonsága**: a telepítés során minden eszköz eszközazonosítókat igényel, és társított hello felhőalapú szolgáltatás által létrehozott hitelesítési kulcsokat. Tartsa meg ezeket a kulcsokat fizikailag biztonságos hello telepítés után is. Feltört kulcs egy meglévő eszközt egy rosszindulatú eszköz toomasquerade is használhatják.

## <a name="iot-solution-operator"></a>Az IoT-megoldás operátor
Ha az IoT-megoldás kezelőként, kövesse a hello ajánlott eljárásokat az alábbi:

* **Az egyes rendszerek toodate be**: Győződjön meg arról, eszköz-operációsrendszerek és az összes eszközillesztőt frissített toohello legújabb verziója. 
* **Rosszindulatú tevékenységhez elleni**: Ha engedélyezi az hello operációs rendszer, helyezze hello legújabb víruskereső és kártevőirtó funkciókat minden eszköz operációs rendszere. 
* **Naplózási gyakran**: naplózás IoT infrastruktúra pedig biztonsági kapcsolatos problémák esetén kulcs toosecurity incidensek válaszol.
* **Fizikailag a hello IoT-infrastruktúra védelméhez**: hello legrosszabb IoT infrastruktúra elleni támadások fizikai hozzáférés toodevices indul el.
* **Felhő hitelesítő adatok védelme**: Felhő hitelesítő adatok konfigurálása és az IoT központi telepítése operációs valószínűleg hello legegyszerűbb módja toogain hozzáférés, és az IoT-rendszerbe. 

