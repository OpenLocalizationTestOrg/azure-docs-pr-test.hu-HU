---
title: "aaaLarge fájl letöltése optimalizálási hello Azure Content Delivery Network keresztül"
description: "Nagy fájlok letöltése, tekintse meg a mélység optimalizálása"
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
ms.openlocfilehash: 2646979bfb38e997037bcff5b1cdda34e22c394a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="large-file-download-optimization-via-hello-azure-content-delivery-network"></a>Nagy méretű fájlok letöltési optimalizálási hello Azure Content Delivery Network keresztül

A hello interneten keresztül továbbított tartalom méretének toogrow fizetési tooenhanced funkciót, a továbbfejlesztett grafikus és multimédiás tartalom továbbra is. A növekedési célja a számos tényező közé tartoznak: szélessávú behatolást vagy a biztonság, a nagyobb alacsony költségű tárolóeszközök, a széles körű nő az nagy felbontású videók és internetkapcsolattal rendelkező eszközök (IoT). A nagy fájlok gyors és hatékony mechanizmus kritikus tooensure egy élvezetesebbé és zökkenőmentes felhasználói élmény.

Nagy fájlok kézbesítését számos kihívást rendelkezik. Első lépésként hello átlagos idő toodownload nagy fájlok jelentős lehet mivel az alkalmazások előfordulhat, hogy nem töltik le az összes adat egymás után. Alkalmazások bizonyos esetekben előfordulhat, hogy töltse le a hello hello első rész előtt a fájl utolsó része. Amikor egy fájl csak egy kis mennyiségű van szükség, vagy a felhasználó megszakítja a letöltési, hello letöltése sikertelen lehet. hello letöltési is előfordulhat, hogy később, amíg után hello tartalomkézbesítési hálózat (CDN) hello teljes fájl kikeresi hello eredeti kiszolgálóra. 

A felhasználó számítógépén és hello fájl között hello késés, hello sebesség, amellyel akkor megtekintheti a tartalom határozza meg. Hálózati torlódás és a kapacitás problémák emellett is átviteli hatással vannak. Kiszolgálók és a felhasználók nagyobb távolságát további lehetőségek a csomag adatvesztés toooccur, ami csökkenti a minőségi létrehozása. minőségi hello csökkenése korlátozott átviteli okozta, és nagyobb csomagvesztés növelheti a fájl letöltése toofinish hello várakozási ideje. 

Harmadik sok nagy fájlt nem érkeznek meg teljes egészében. Felhasználók előfordulhat, hogy egy másik keresztül letölthető megszakítja, vagy tekintse meg a csak hello egy hosszú MP4 videó az első néhány percig. Ezért szoftverét és adathordozóit kézbesítési vállalatok szeretné toodeliver csak hello része egy fájlt, amely van szükség. A kért hello hatékony terjesztése részei hello forráskiszolgálóról csökkenti a hello kilépő forgalmat. Hatékony terjesztési is csökkenti a hello memória és hello forrás kiszolgálón i/o-terhelés. 

hello Azure Content Delivery Network Akamai kínál: a szolgáltatás letölti a nagy fájlok hatékonyan toousers hello földgömb léptékű között. hello szolgáltatás csökkenti késések fordulnak elő, mert hello származási kiszolgálók hello terhelése csökkenti. Ez a szolgáltatás hello Standard Akamai tarifacsomag érhető el.

## <a name="configure-a-cdn-endpoint-toooptimize-delivery-of-large-files"></a>A CDN végpont toooptimize kézbesítési nagy méretű fájl konfigurálása

A CDN-végpont toooptimize hello Azure-portálon keresztül nagy fájlok esetében kézbesítési konfigurálhatja. Használhatja a REST API-kat is, vagy bármely ez hello ügyfél SDK-k toodo. hello következő lépések bemutatják hello folyamat hello Azure-portálon keresztül.

1. egy új végponton, hello tooadd **CDN-profil** lapon jelölje be **végpont**.

    ![Új végpont](./media/cdn-large-file-optimization/01_Adding.png)  
 
2. A hello **optimalizálva** legördülő listában válassza **nagy méretű fájlok letöltési**.

    ![Kijelölt nagy méretű fájlok optimalizálása](./media/cdn-large-file-optimization/02_Creating.png)


Hello CDN-végpont létrehozása után hello nagy méretű fájlok optimalizálás feltételeknek megfelelő összes fájl vonatkozik. a következő szakasz hello ezt a folyamatot ismerteti.

## <a name="optimize-for-delivery-of-large-files-with-hello-azure-content-delivery-network-from-akamai"></a>Nagy fájlok az Azure Content Delivery Network hello kézbesítését Akamai optimalizálás

hello nagy méretű fájlok optimalizálási típusa szolgáltatás bekapcsolása hálózati optimalizálást és konfigurációk toodeliver nagy fájlok, gyorsabb és több responsively. Általános webes kézbesítési Akamai a fájlt csak 1,8 GB alá, és alagút (nem a gyorsítótárban) is fájlok mentése too150 GB. Nagy méretű fájlok optimalizálási gyorsítótárazza a fájlok mentése too150 GB.

Nagy méretű fájlok optimalizálási bizonyos feltételek teljesülése esetén hatékony. Feltételei közé tartozik a hello eredeti kiszolgálóra működését és a hello méretek és a hello fájlok kért típusú. Mielőtt e témákban részleteinek kapjuk, tisztában kell hello optimalizálási működése. 

### <a name="object-chunking"></a>Az objektum adattömbösítő 

hello Azure Content Delivery Network Akamai objektum adattömbösítő elnevezésű technikát használja. Ha van szükség a nagy fájlok, a hello CDN lekérdezi a hello fájl kisebb kódrészletek hello forrásból. Hello CDN kiszolgáló peremhálózati/POP teljes vagy bájttartomány fájl kérelmet kap, miután ellenőrzi, hogy az optimalizálás hello fájltípus esetén támogatott. Azt is ellenőrzi, hogy hello fájltípus megfelel hello fájl méretét. Ha hello fájl mérete nagyobb, mint 10 MB, a hello CDN peremhálózati kiszolgáló lekéri a hello fájl 2 MB méretű adattömböket a hello a forrásból. 

Hello adatrészlet hello CDN peremhálózati megérkezik, miután a gyorsítótárba, és azonnal kiszolgált toohello felhasználó. hello CDN majd prefetches hello következő adatrészlet párhuzamosan. A lehívott biztosítja, hogy hello tartalom marad egy adatrészlet előre hello felhasználói, amely csökkenti a késést. A művelet akkor amíg teljes hello le (ha szükséges), minden bájttartományok érhetők el (ha szükséges), vagy hello ügyfél megszakítja hello kapcsolatot. 

További információ a hello bájttartomány kérelemre: [RFC 7233](https://tools.ietf.org/html/rfc7233).

hello CDN gyorsítótárazza a bármely adattömböket, mivel érkezett. hello teljes fájl toobe gyorsítótárazott a hello CDN gyorsítótár nem rendelkezik. A fájl vagy bájt tartományoknál hello kérelmeknél hello CDN gyorsítótár a szolgáltatott. Nem minden hello adattömbök hello CDN gyorsítótárazza, előzetes betöltési esetén használt toorequest adattömbök hello a forrásból. Az optimalizálás hello származási kiszolgálói toosupport bájttartomány kérelmek hello képességét támaszkodik. _Hello származási kiszolgáló nem támogatja a bájttartomány kérelmeket, ha az optimalizálás nem hatékony._ 

### <a name="caching"></a>Gyorsítótárazás
Nagy méretű fájlok optimalizálási más alapértelmezett gyorsítótár lejárati idővel az általános webes kézbesítési használja. Pozitív és negatív gyorsítótárazást a HTTP válaszkódot alapján közötti különbséget tesz. Ha hello eredeti kiszolgálóra a cache-control keresztül lejárati időt határozza meg, vagy egy fejléc a következő hello válasz lejár, a hello CDN eleget tegyen ezt az értéket. Ha hello forrás nem ad meg, és hello fájl hello típusú és bármekkora méretű feltételek optimalizálási típus megfelel, hello CDN hello alapértelmezett értékeket használja, a nagy méretű fájlok optimalizálásához. Hello CDN egyébként általános webes kézbesítésre használja az alapértelmezett értékeket.


|    | Általános webes | Nagy méretű fájlok optimalizálása 
--- | --- | --- 
Gyorsítótárazás: pozitív <br> 200-AS, 203, 300, HTTP <br> 301, 302, és 410 | 7 nap |1 nap  
Gyorsítótárazás: negatív. <br> HTTP 204, 305, 404, <br> és 405 | None | 1 másodperc 

### <a name="deal-with-origin-failure"></a>Az eredeti hiba kezelésére

hello származási olvasás-időtúllépés növekszik általános webes kézbesítési tootwo percig hello nagy optimalizálási fájltípus két másodperc. Ezzel a növekedéssel számlák hello nagyobb fájl méretét tooavoid korai időtúllépés kapcsolatot.

Ha a kapcsolat időtúllépés miatt megszakadt, hello CDN újrapróbálja többször "504 - átjáró időtúllépése" hiba toohello ügyfél küldése előtt. 

### <a name="conditions-for-large-file-optimization"></a>Nagy méretű fájlok optimalizálási feltételei

a következő táblázat hello feltételek toobe elégedett a nagy méretű fájlok optimalizálásához hello készletét tartalmazza:

Az állapot | Értékek 
--- | --- 
Támogatott fájltípusok | 3g, 2, 3gp, az ASP, avi, bz2, dmg, exe, f4v, flv, <br> GZ, hdp, iso, jxr, m4v, mkv, mov, mp4, <br> MPEG, mpg, mts, pkg, qt, erőforrás-kezelő, swf, bont, <br> TGZ, wdp, webm, webp, wma, wmv, zip  
Minimális mérete | 10 MB 
Maximális fájlméret | 150 GB 
Forrás server jellemzői | Támogatnia kell a bájttartomány kérelmek 

## <a name="optimize-for-delivery-of-large-files-with-hello-azure-content-delivery-network-from-verizon"></a>Nagy fájlok az Azure Content Delivery Network hello kézbesítését verizon optimalizálás

hello Azure Content Delivery Network verizon nagy fájlok nélkül a maximális fájlméret nyújt. További funkciók szerint vannak kapcsolva nagy fájlok alapértelmezett toomake kézbesítését gyorsabb.

### <a name="complete-cache-fill"></a>Teljes gyorsítótár kitöltés

hello alapértelmezett teljes gyorsítótár kitöltés funkcióval hello CDN toopull fájl hello gyorsítótárba egy kezdeti kérelem elhagyott vagy elveszett. 

Teljes gyorsítótár kitöltés esetén a leghasznosabb nagy eszközök. Általában felhasználók nem letöltheti start toofinish. Progresszív letöltés használnak. hello alapértelmezett viselkedés hello peremhálózati kiszolgáló tooinitiate hello forráskiszolgálóról hello eszköz a háttérben történő kényszeríti. A későbbiekben hello eszköz hello biztonsági kiszolgáló helyi gyorsítótárában van. Miután hello teljes objektum hello gyorsítótár, a hello biztonsági kiszolgáló teljesíti bájttartomány kérelmek toohello CDN hello gyorsítótárazott objektumhoz.

hello alapértelmezett viselkedés hello szabálymotor hello Verizon Premium szint a keresztül letiltható.

### <a name="peer-cache-fill-hot-filing"></a>Társ-gyorsítótárazás töltse ki a közbeni bejelentés

hello alapértelmezett társközi gyorsítótár kitöltés közbeni bejelentés szolgáltatás kifinomult jogvédett algoritmust használ. További peremhálózati gyorsítótár-alapú sávszélesség kiszolgálók használ, és összesítés kérelmek metrikák toofulfill nagy, nagy népszerű objektumok az ügyfélkéréseket. Ez a szolgáltatás megakadályozza, hogy olyan helyzet, amelyben további kérések küldése tooa felhasználói eredeti kiszolgálóra. 

### <a name="conditions-for-large-file-optimization"></a>Nagy méretű fájlok optimalizálási feltételei

hello optimalizálási funkciók a Verizon alapértelmezés szerint vannak kapcsolva. Nincsenek nem határoz meg a maximális fájlméretet. 

## <a name="additional-considerations"></a>Néhány fontos megjegyzés

Vegye figyelembe a következő további szempontok optimalizálási típus hello.
 
### <a name="azure-content-delivery-network-from-akamai"></a>Akamai Azure Tartalomkézbesítési hálózat

- folyamat adattömbösítő hello további kérelmeket toohello eredeti kiszolgálóra állít elő. Hello teljes hello forrásból kézbesíteni adatok mennyiségét azonban jóval kisebb. Csonkolási eredményez jobb: hello CDN gyorsítótárazási jellemzőit.

- Memória és I/O nyomás csökkennek hello forrása, mert kisebb kódrészletek hello fájl érkeznek.

- A gyorsítótárazott hello CDN: adattömböt nincsenek további kérelmek toohello származási hello tartalom lejárati vagy hello gyorsítótárból ki van zárva.

- Tartomány végezhetnek toohello CDN kér, és van, mint bármely normál fájl kezeli azokat. Optimalizálás csak akkor, ha egy érvényes fájltípust és a 10 MB és 150 GB között van hello bájttartomány vonatkozik. 10 MB-nál kisebb hello átlagos mérete a kért esetén érdemes toouse általános webes kézbesítési helyette.

### <a name="azure-content-delivery-network-from-verizon"></a>Verizon Azure Tartalomkézbesítési hálózat

hello általános webes optimalizálási típusú biztosíthat nagy méretű fájlt.
