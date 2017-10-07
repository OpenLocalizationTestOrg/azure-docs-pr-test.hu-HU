---
title: "aaaAzure CDN szabályok motor egyezés feltételek |} Microsoft Docs"
description: "Az Azure CDN referenciadokumentációt szabályok motor egyezés feltételek és a szolgáltatások."
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: 5e79f8f0c75a646e13bf315c492b9f2a9defc396
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cdn-rules-engine-match-conditions"></a>Az Azure CDN szabálymotor feltételek egyeznek
Ez a témakör listák részletes leírását, az Azure Content Delivery Network (CDN) feltételek egyeznek hello [szabálymotor](cdn-rules-engine.md).

hello második szabály része hello egyezés feltétel. Egy egyezés feltétel azonosítja az adott típusú kérelmet, amely funkciókat érvényesül.

Például lehet használt toofilter tartalomkérelmeit egy adott helyen létrehozott, egy adott IP-cím vagy ország vagy a fejléc-információ kérelmek.

## <a name="always"></a>Mindig

hello mindig egyezés feltétele egy alapértelmezett beállítása a szolgáltatások tooall kérelmek tervezett tooapply.

## <a name="device"></a>Eszköz

hello eszköz egyezés feltétel kérelmek egy mobileszközről tulajdonságai alapján azonosítja.  Mobil eszköz észlelési sorrendekben [WURFL](http://wurfl.sourceforge.net/).  WURFL képességek és a CDN szabálymotor változók az alábbiak.

> [!NOTE] 
> az alábbi hello változók hello által támogatott **ügyfél kérelem fejléc módosítása** és **ügyfél válaszfejléc módosítása** szolgáltatások.

Képesség | Változó | Leírás | A minta érték(ek)
-----------|----------|-------------|----------------
Márka neve | a(z) % {wurfl_cap_brand_name} | Egy karakterlánc, amely hello eszköz hello márkáját jelöli. | Samsung
Az eszköz operációs rendszere | a(z) % {wurfl_cap_device_os} | Egy karakterlánc, amely azt jelzi, hello operációs rendszer hello eszközön van telepítve. | IOS
Eszköz operációs rendszerének verziója | a(z) % {wurfl_cap_device_os_version} | Egy karakterlánc, amely azt jelzi, hello hello hello eszközre telepített operációs rendszer verziószáma. | 1.0.1
Kettős tájolását | a(z) % {wurfl_cap_dual_orientation} | Egy logikai érték, amely jelzi, hogy hello eszköz támogatja-e kettős tájolását. | Igaz
HTML előnyben részesített DTD | a(z) % {wurfl_cap_html_preferred_dtd} | Karakterlánc, amely azt jelzi, hello mobil eszköz előnyben részesített dokumentumtípusdefiníció (DTD) a HTML-tartalmakat. | Egyik sem<br/>xhtml_basic<br/>HTML5
A kép Inlining | a(z) % {wurfl_cap_image_inlining} | Olyan logikai érték, amely jelzi, hogy hello eszköz támogatja-e a Base64 kódolású képek. | hamis
Az Android | a(z) % {wurfl_vcap_is_android} | Egy logikai érték, amely azt jelzi, hogy hello eszköz hello Android operációs rendszer használja-e. | Igaz
IOS | a(z) % {wurfl_vcap_is_ios} | Egy logikai érték, amely azt jelzi, hogy hello eszköz iOS használ-e. | hamis
Az intelligens TV | a(z) % {wurfl_cap_is_smarttv} | Egy logikai érték, amely azt jelzi, hogy hello eszköz intelligens TV. | hamis
Smartphone van | a(z) % {wurfl_vcap_is_smartphone} | Egy logikai érték, amely azt jelzi, hogy hello eszköz okostelefont. | Igaz
Tábla van | a(z) % {wurfl_cap_is_tablet} | Egy logikai érték, amely azt jelzi, hogy hello eszköz táblagép. Ez az az operációs rendszer független leírást. | Igaz
Vezeték nélküli eszköz | a(z) % {wurfl_cap_is_wireless_device} | Egy logikai érték, amely azt jelzi, hogy hello eszköz vezeték nélküli eszköz számít. | Igaz
Marketing neve | a(z) % {wurfl_cap_marketing_name} | Egy karakterlánc, amely hello eszköz marketing nevét jelöli. | BlackBerry 8100 Pearl
Mobil böngésző | a(z) % {wurfl_cap_mobile_browser} | Karakterlánc, amely azt jelzi, hello böngésző használt toorequest tartalom hello eszközről. | Chrome
Mobil böngészőverzió | a(z) % {wurfl_cap_mobile_browser_version} | Hello böngésző hello verzióját jelző karakterlánc használt toorequest tartalom hello eszközről. | 31
Modell neve | a(z) % {wurfl_cap_model_name} | Egy karakterlánc, amely azt jelzi, hello eszköz modell neve. | S3
Progresszív letöltés | a(z) % {wurfl_cap_progressive_download} | Egy logikai érték, amely jelzi, hogy hello eszköz támogatja-e a hang-és videófolyamot hello lejátszását, miközben továbbra is letöltése. | Igaz
Kiadás dátuma | a(z) % {wurfl_cap_release_date} | Karakterlánc, amely azt jelzi, mely hello eszköz volt hónap és év hello toohello WURFL adatbázis hozzá.<br/><br/>Formátum:`yyyy_mm` | 2013_december
Megoldási magassága | a(z) % {wurfl_cap_resolution_height} | Hello eszköz magassága képpontban jelző egész számot. | 768
Megoldási szélessége | a(z) % {wurfl_cap_resolution_width} | Hello eszköz szélességét képpontban jelző egész számot. | 1024


## <a name="location"></a>Hely

Ezek a feltételek egyeznek tervezett tooidentify kérelem hello kérelmező helye alapján.

Név | Cél
-----|--------
SZÁMOT | Egy adott hálózati kérelmekkel azonosítja.
Ország | A megadott hello kérelmekkel azonosítja országokban.


## <a name="origin"></a>Forrás

Ezek a feltételek egyeznek tervezett tooidentify kérelmek vannak pont tooCDN tárolási vagy egy ügyfél eredeti kiszolgálóra.

Név | Cél
-----|--------
CDN-forrása | A CDN-tárolón tárolt tartalmak iránti kérelmek azonosítja.
Ügyfél forrása | Egy adott ügyfélhez forrás kiszolgálón lévő tartalomhoz azonosítja.


## <a name="request"></a>Kérés

Ezek a feltételek egyeznek tervezett tooidentify kérelem tulajdonságaik alapján.

Név | Cél
-----|--------
Ügyfél IP-címe | Egy adott IP-címről kérelmekkel azonosítja.
Cookie-k paraméter | Ellenőrzések hello cookie-k a megadott hello minden kérelemhez társított értéket.
Cookie-k paraméter Regex | Ellenőrzi, hogy minden egyes kérelem hello társított hello cookie-k a megadott reguláris kifejezés.
Peremhálózati Cname | Azonosítja a kérelmeket, amelyek tooa adott peremhálózati CNAME.
Hivatkozó tartomány | Az említett kérelmek azonosítja hello megadott hostname(s).
Kérelem fejléc szövegkonstans | A megadott hello tartalmazó kérések azonosítja fejléc set tooa megadott értékeket.
Kérelem fejléc Regex | A megadott hello tartalmazó kérések azonosítja set tooa Fejlécérték hello megfelelő a megadott reguláris kifejezésnek.
Kérelem fejléc helyettesítő | A megadott hello tartalmazó kérések azonosítja fejléc tooa érték, amely megfelel a megadott minta hello beállítása.
Kérési módszer | A HTTP-metódus azonosítja a kérelmeket.
Kérelem séma | A HTTP protokoll azonosítja a kérelmeket.

## <a name="url"></a>URL-CÍME

Ezek a feltételek egyeznek az URL-címek alapján tervezett tooidentify kérelem.

Név | Cél
-----|--------
URL-cím elérési út könyvtár | Azonosítja a kéréseket a relatív elérési útja.
URL-cím elérési út bővítmény | A fájlnév-kiterjesztés azonosítja a kérelmeket.
URL-cím elérési út fájlnév | A fájlnév azonosítja a kérelmeket.
URL-cím elérési út szövegkonstans | Egy kérelem összehasonlítja érték megadott relatív elérési út toohello.
URL-cím elérési út Regex | Egy kérelem összehasonlítja reguláris kifejezéssel megadott relatív elérési út toohello.
URL-cím elérési út helyettesítő karakter | A kérelem relatív elérési út toohello megadott minta hasonlítja össze.
Lekérdezés-szövegkonstans URL-címe | Egy kérelem összehasonlítja érték megadott lekérdezési karakterlánc toohello.
URL-cím lekérdezési paraméter | Hello megadott lekérdezési karakterlánc-paraméter beállítása, amely megfelel a megadott minta tooa értéket tartalmazó kérések azonosítja.
Lekérdezés Regex URL-címe | Hello megadott lekérdezési karakterlánc-paraméter beállítása tooa érték, amely megfelel a megadott reguláris kifejezést tartalmazó kérések azonosítja.
Lekérdezés helyettesítő URL-címe | Összehasonlítja hello megadott érték(ek) hello lekérdezési karakterlánc ellen.


## <a name="next-steps"></a>Következő lépések
* [Az Azure CDN áttekintése](cdn-overview.md)
* [Szabályok motor referencia](cdn-rules-engine-reference.md)
* [Szabályok motor feltételes kifejezések](cdn-rules-engine-reference-conditional-expressions.md)
* [Szabályok adatbázismotor-szolgáltatások](cdn-rules-engine-reference-features.md)
* [Alapértelmezett HTTP működés használata hello szabályok felülbírálása](cdn-rules-engine.md)

