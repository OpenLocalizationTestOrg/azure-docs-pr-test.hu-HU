---
title: "a forgatókönyvnek Azure tartalomkézbesítési aaaOptimize"
description: "Hogyan toooptimize kézbesítési a tartalom az adott forgatókönyveket"
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: v-semcev
ms.openlocfilehash: 922a92fdbf7e6e21f2b5ae9a2fb4ac32735fc15a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-azure-content-delivery-for-your-scenario"></a>A forgatókönyvnek Azure tartalomkézbesítési optimalizálása

Tartalom tooa hatalmas, globális közönségek átadná esetén kritikus tooensure optimalizált hello kézbesítési tartalmat. hello Azure tartalom Delivery Network hello kézbesítési élményt hello típusú rendelkezik tartalom is optimalizálhatja. A tartalom lehet egy webhely, élő adatfolyam, videó, vagy letölthető nagy fájlok. Amikor létrehoz egy tartalomkézbesítési hálózat (CDN) végpont, a hello adjon meg egy olyan forgatókönyvet **optimalizálva** lehetőséget. A kiválasztott meghatározza, hogy mely optimalizálási alkalmazott toohello tartalom szállított hello CDN-végpontot.

Optimalizálás használhatók tervezett toouse bevált gyakorlat viselkedések tooimprove tartalomkézbesítési teljesítményét, és jobb eredetű-kiszervezés. A forgatókönyvben a választott részleges gyorsítótárazás objektum adattömbösítő és hello származási hiba újrapróbálkozási házirendje konfigurációi módosításával hatása a teljesítményre. 

Ez a cikk áttekintése különböző optimalizálási funkcióját, és ha kell használnia. A szolgáltatások és korlátozások további információkért lásd: hello megfelelő cikkek minden egyes optimalizálási típus.

> [!NOTE]
> A **optimalizálva** beállításokat választja hello szolgáltató függően változhat. CDN-szolgáltatók a fejlesztés hello forgatókönyvtől függően különböző módon alkalmazni. 

## <a name="provider-options"></a>Szolgáltatói beállítása

támogatja az Azure Content Delivery Network Akamai hello:

* Általános webes kézbesítés 

* Általános médiaadatfolyam-továbbítást

* Videotartalom médiaadatfolyam-továbbítást

* Nagy méretű fájl letöltése

* Dinamikus gyorsulás 

hello Azure Content Delivery Network verizon csak általános webes kézbesítési támogatja. Igény szerint és a nagy méretű fájlok letöltési videót használható. Tooselect egy optimalizálási típus nem rendelkezik.

Erősen ajánlott, hogy tesztelje teljesítmény változata különböző szolgáltatók tooselect hello optimális szolgáltató a kézbesítésre között.

## <a name="select-and-configure-optimization-types"></a>Válasszon ki és konfiguráljon optimalizálási típusok

toocreate új végpont, válassza ki az optimalizálás típusa, amely a legjobban illik a hello forgatókönyv végpont toodeliver hello kívánt tartalom típusa. **Általános webes kézbesítési** hello alapértelmezett beállítás. Hello optimalizálási beállítás bármely létező Akamai végpont bármikor frissítheti. Ez a változás nem szakítsa meg a hello CDN kézbesítési. 

1. Válasszon ki egy végpontot, egy Standard Akamai profilon belül.

    ![Végpont kiválasztása ](./media/cdn-optimization-overview/01_Akamai.png)

2. A **beállítások**, jelölje be **optimalizálási**. Jelölje ki a kívánt hello a **optimalizálva** legördülő listából.

    ![Optimalizálási és típus kiválasztása](./media/cdn-optimization-overview/02_Select.png)

## <a name="optimization-for-specific-scenarios"></a>Az adott forgatókönyveket optimalizálása

CDN-végpont hello valamelyik a következő forgatókönyvek hello is optimalizálhatja. 

### <a name="general-web-delivery"></a>Általános webes kézbesítés

Általános webes kézbesítési beállítás hello leggyakoribb optimalizálás. Általános webes tartalom optimalizálásra, például a weboldalakon és webes alkalmazásokhoz tervezték. Az optimalizálás is használható fájlt, és a videó tölti le.

Egy tipikus webhely statikus és dinamikus tartalom tartalmazza. Statikus tartalom magában foglalja a lemezképek, a JavaScript szalagtárak és a stíluslapok, amely a gyorsítótárban, és toodifferent felhasználók kézbesíteni. Dinamikus tartalom egy adott felhasználó személyre szabhatja például hírek elemek, amelyek különböző tooa felhasználói profilt. Dinamikus tartalom nincs gyorsítótárazva, mert egyedi tooeach felhasználó, például vásárlás bevásárlókocsiból tartalmát. Általános webes kézbesítési optimalizálhatja a teljes webhelyet. 

> [!NOTE]
> Ha hello Azure Content Delivery Network Akamai használ, érdemes toouse az optimalizálás Ha a 10 MB-nál kisebb fájlok átlagos mérete. Ha a átlagos mérete nagyobb, mint 10 MB, jelölje be **nagy méretű fájlok letöltési** a hello **optimalizálva** legördülő listában.

### <a name="general-media-streaming"></a>Általános médiaadatfolyam-továbbítást

Ha élő adatfolyam-és videotartalom streaming toouse hello végpont van szüksége, ajánlott általános médiaadatfolyam-optimalizálás.

Az idő-és nagybetűket, médiaadatfolyam azért, mert a késői hello ügyfél csomagokat csökkent megtekintését, például a video tartalom Gyakori pufferelés okozhat. Médiaadatfolyam-továbbítást optimalizálási media tartalomkézbesítési hello késését csökkenti, és adatfolyam-továbbítási zökkenőmentes élményt nyújt a felhasználók számára. 

Ez a forgatókönyv esetében gyakori, az Azure media service ügyfelek esetén. Az Azure media services használata esetén nem használható élő és igény szerinti adatfolyam egy streamvégpont kap. Ebben az esetben az ügyfelek sem kell tooswitch tooanother végpont, az élő tooon igényalapú streamelési módosítják. Általános media adatfolyam-továbbítási optimalizálási támogatja ezt a forgatókönyvet.

hello Azure tartalom Delivery Network verizon hello általános webes kézbesítési optimalizálási típusú toodeliver adatfolyam media tartalmat használ.

toolearn médiaadatfolyam-továbbítást optimalizálásával kapcsolatos további információkért lásd: [médiaadatfolyam-továbbítást optimalizálási](cdn-media-streaming-optimization.md).

### <a name="video-on-demand-media-streaming"></a>Videotartalom médiaadatfolyam-továbbítást

Videotartalom media adatfolyam-továbbítási optimalizálási növeli a videotartalom adatfolyamok tartalmát. Ha a végpont videotartalom streaming használja, célszerű ezt a beállítást toouse.

hello Azure tartalom Delivery Network verizon hello általános webes kézbesítési optimalizálási típusú toodeliver adatfolyam media tartalmat használ.

toolearn médiaadatfolyam-továbbítást optimalizálásával kapcsolatos további információkért lásd: [médiaadatfolyam-továbbítást optimalizálási](cdn-media-streaming-optimization.md).

> [!NOTE]
> Ha hello végpont elsősorban videotartalom tartalmat szolgáltat, ez használható optimalizálás. az optimalizálás és az általános media adatfolyam-továbbítási optimalizálási hello jelentős különbség hello, hello kapcsolat újrapróbálkozási időkorlátja. hello időtúllépés értéke sokkal rövidebb toowork élő adatfolyam-továbbítási esetén.

### <a name="large-file-download"></a>Nagy méretű fájl letöltése

Hello Azure Content Delivery Network Akamai használatakor a nagy méretű fájlok letöltési toodeliver 1,8 GB-nál nagyobb fájlokat kell használnia. hello Azure Content Delivery Network verizon nem rendelkezik a fájl egy korlátozás, töltse le az általános webes kézbesítési optimalizálás mérete.

Hello Azure tartalom Delivery Network Akamai használatakor nagy fájlok letöltése tartalma 10 MB-nál nagyobb vannak optimalizálva. Ha a átlagos mérete 10 MB-nál kisebb, érdemes toouse általános webes kézbesítését. Ha a átlagos fájlok következetesen 10 MB-nál nagyobb értékek, hatékonyabb toocreate nagy fájlok esetében a különálló végpont lehet. Például belső vezérlőprogram vagy szoftverfrissítések általában nagy méretű fájlt.

hello Azure tartalom Delivery Network verizon hello általános webes kézbesítési optimalizálási típusú toodeliver adatfolyam media tartalmat használ.

toolearn nagy méretű fájlok optimalizálási kapcsolatos további információkért lásd: [nagy méretű fájlok optimalizálási](cdn-large-file-optimization.md).

### <a name="dynamic-site-acceleration"></a>Dinamikus gyorsulás

 Dinamikus gyorsítás Akamai és a Verizon Content Delivery Network érhető el. Az optimalizálás egy további díj toouse magában foglalja. További információkért lásd: hello árképzést ismertető oldalra.

Dinamikus gyorsítás hello késleltetés és a teljesítmény a dinamikus tartalom előnyeit kihasználó különböző módszereket foglalja magában. Ennek technikái a következők: útvonal és a hálózati optimalizálás, a TCP-optimalizálása és egyebek. 

Az optimalizálás tooaccelerate számos olyan válaszok, amelyek nem gyorsítótárazható tartalmazó webes alkalmazás is használhatja. Többek között a keresési eredmények között, a kivételt tranzakciók vagy a valós idejű adatokat. Folytatás toouse CDN gyorsítótárazási alapképességek statikus adatok. 



