---
title: "aaaAzure CDN szabályok motor szolgáltatások |} Microsoft Docs"
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
ms.openlocfilehash: c10b8ef58e3d209b12fbb0ac2173e1ca51ff7538
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cdn-rules-engine-features"></a>Az Azure CDN szabályok motor-funkciók
Ez a témakör részletes leírását tartalmazza hello elérhető szolgáltatásokat az Azure Content Delivery Network (CDN) [szabálymotor](cdn-rules-engine.md).

hello harmadik szabály része hello szolgáltatást. Egy szolgáltatás határozza meg a műveletet hello típusú feltételek egyeznek kérelem toohello típusú alkalmazza.

## <a name="access"></a>Hozzáférés

Ezek a szolgáltatások akkor tervezett toocontrol hozzáférés toocontent.


Név | Cél
-----|--------
Hozzáférés megtagadása | Meghatározza, hogy minden kérésnél 403 Tiltott választ utasítja el.
Jogkivonat hitelesítési | Meghatározza, hogy jogkivonat-alapú hitelesítés alkalmazott tooa kérelem legyen.
Jogkivonat hitelesítési Megtagadás kódot | Meghatározza a válaszban visszaadott tooa felhasználói kérelem miatt tooToken-alapú hitelesítés megtagadása hello típusát.
Jogkivonat hitelesítési nagybetűket URL-címe | Meghatározza, hogy jogkivonat-alapú hitelesítés által készített URL-cím összehasonlítás legyen kis-és nagybetűket.
Jogkivonat hitelesítési paraméter | Határozza meg, hogy van-e hello jogkivonat-alapú hitelesítés lekérdezési karakterlánc paraméter átnevezték.

### <a name="deny-access"></a>Hozzáférés megtagadása
**Cél**: meghatározza, hogy minden kérésnél 403 Tiltott választ utasítja el.

Érték | eredménye
------|-------
Engedélyezve| Hatására az összes kérelmet, amely megfelel a hello 403 Tiltott választ elutasított folyamatmegfeleltetési feltételek toobe.
Letiltva| Visszaállítja a hello alapértelmezett működését. hello alapértelmezés lesz tooallow hello származási server toodetermine hello válaszának típusa, amelyet adja vissza.

**Alapértelmezett viselkedés**: letiltva

> [!TIP]
   > Egy lehetséges a funkció használata a kérelem fejléc egyezik meg a feltétel tooblock hozzáférés tooHTTP hivatkozó kérelmei által használt beágyazott hivatkozások tooyour tartalom tooassociate.

### <a name="token-auth"></a>Jogkivonat hitelesítési
**Cél:** meghatározza, hogy jogkivonat-alapú hitelesítés alkalmazott tooa kérelem legyen.

Ha a jogkivonat-alapú hitelesítés engedélyezve van, adjon meg egy titkosított jogkivonatot, és felel meg, hogy a jogkivonat megadott toohello követelményeket csak kérelmek lesz érvényes.

hello titkosítási kulcs, amely használt tooencrypt és visszafejtése lexikális elemek értékének lesz az elsődleges kulcs és a biztonsági mentés kulcs jogkivonat hitelesítési lapján található beállítások határozzák meg. Ne feledje, hogy a titkosítási kulcsok platform-specifikus.

Érték | eredménye
------|---------
Engedélyezve | Védi a hello a kért tartalom jogkivonat-alapú hitelesítéssel. Csak ad meg egy érvényes tokent, és megfeleljenek az ügyfelek kérelmeinek szembeni szerződéses kötelezettségeket. FTP-tranzakciók jogkivonat-alapú hitelesítés nem tartoznak.
Letiltva| Visszaállítja a hello alapértelmezett működését. alapértelmezett viselkedés hello van a a hitelesítési jogkivonat-alapú konfigurációs toodetermine tooallow e kérelem védve legyenek.

**Alapértelmezés:** letiltva.

###<a name="token-auth-denial-code"></a>Jogkivonat hitelesítési Megtagadás kódot
**Cél:** visszaadott tooa felhasználói kérelem miatt tooToken-alapú hitelesítés megtagadása válaszának hello típusa határozza meg.

hello elérhető válaszkódot alább láthatók.

Válaszkód|Válasz neve|Leírás
----------------|-----------|--------
301|Végleg áthelyezése|Ez az állapot kód jogosulatlan felhasználók toohello URL-cím helye fejlécben megadott irányítja át.
302|Található|Ez az állapot kód jogosulatlan felhasználók toohello URL-cím helye fejlécben megadott irányítja át. Ez az állapot kód hello iparági szabványos módjáról irányítja át a felhasználókat a rendszer.
307|Ideiglenes átirányítás|Ez az állapot kód jogosulatlan felhasználók toohello URL-cím helye fejlécben megadott irányítja át.
401|Nem engedélyezett|Ezzel az állapotkóddal kombinálás a WWW-Authenticate válaszfejléc lehetővé teszi a tooprompt a felhasználó a hitelesítéshez.
403|Tiltott|Ez az hello szabványos 403-as tiltott állapotüzenetet, hogy a jogosulatlan felhasználók megjelenik, ha a védett tartalmak tooaccess közben.
404|A fájl nem található|Ezzel az állapotkóddal azt jelzi, hogy hello HTTP-ügyfél képes toocommunicate hello kiszolgálóval, de hello a kért tartalom nem található.

#### <a name="url-redirection"></a>Átirányítási URL-címe

Ez a szolgáltatás URL-cím átirányítási tooa felhasználói URL-címet támogatnak konfigurált tooreturn 3xx állapotkódot esetén. A felhasználó által definiált URL-cím adható meg hello lépések végrehajtásával:

1. Válassza ki a 3xx válaszkód hello hitelesítés megtagadása Tokenkódot szolgáltatáshoz.
2. "Hely" Válassza ki a kötelező fejlécet beállítást.
3. A Fejlécérték nem kötelező beállítás szükséges toohello URL-Címének beállítása.

Ha az egy URL-címe nincs definiálva a következő 3xx állapotkódot, majd hello szabványos válasz lap egy 3xx status kód visszatér toohello felhasználó.

Átirányítási URL-címe esetén csak 3xx válaszkódot alkalmazható.

A választható állomásfejléc-érték a beállítás lehetővé teszi, alfanumerikus karaktereket, idézőjelek és szóközöket.

#### <a name="authentication"></a>Authentication

Ez a funkció támogatja a hello funkció tooinclude a WWW-Authenticate fejlécet, ha tooan jogosulatlan kérelmet a jogkivonat-alapú hitelesítés által védett tartalom válaszol. Ha a WWW-Authenticate fejléc túl "basic" konfigurációs van beállítva, majd hello illetéktelen felhasználó bekéri a fiók hitelesítő adatait.

konfigurációs fent hello elérhető hello lépések végrehajtásával:

1. Válassza ki a "401-es" hello válaszkód hello hitelesítés megtagadása Tokenkódot funkció szerint.
2. Válassza ki a "WWW-Authenticate" a Fejlécnév nem kötelező beállítás.
3. Beállítása választható állomásfejléc-érték túl "alapszintű."

A WWW-Authenticate fejléc értéke csak a 401-es válaszkódot.

### <a name="token-auth-ignore-url-case"></a>Jogkivonat hitelesítési nagybetűket URL-címe
**Cél:** meghatározza, hogy jogkivonat-alapú hitelesítés által készített URL-cím összehasonlítás legyen kis-és nagybetűket.

Ez a szolgáltatás által érintett hello paraméterek a következők:

- ec_url_allow
- ec_ref_allow
- ec_ref_deny

Érvényes értékek a következők:

Érték|eredménye
---|----
Engedélyezve|Hatására a peremhálózati kiszolgáló tooignore eset URL-címek jogkivonat-alapú hitelesítési paraméterek összehasonlításakor.
Letiltva|Visszaállítja a hello alapértelmezett működését. hello alapértelmezés lesz az URL-összehasonlítás tokent használó hitelesítés toobe kis-és nagybetűket.

**Alapértelmezés:** letiltva.
 
### <a name="token-auth-parameter"></a>Jogkivonat hitelesítési paraméter
**Cél:** határozza meg, hogy van-e hello jogkivonat-alapú hitelesítés lekérdezési karakterlánc paraméter átnevezték.

Kapcsolatos információkat:

- Az érték opció meghatározza hello lekérdezési karakterlánc paraméter neve keresztül, ami egy token adható meg.
- A beállítás értéke túl nem állítható be "ec_token."
- Győződjön meg arról, hogy az érték lehetőséget csak meghatározott hello neve 
- URL-cím érvénytelen karaktereket tartalmaz.

Érték|eredménye
----|----
Engedélyezve|Az érték opció meghatározza hello lekérdezési karakterlánc paraméter neve keresztül, amely a jogkivonatok definiálni kell.
Letiltva|A jogkivonat hello kérelem URL-címében nem definiált lekérdezési karakterlánc paraméterként adható meg.

**Alapértelmezés:** letiltva. A jogkivonat hello kérelem URL-címében nem definiált lekérdezési karakterlánc paraméterként adható meg.

## <a name="caching"></a>Gyorsítótárazás

Ezek a szolgáltatások akkor tervezett toocustomize, mikor és hogyan tartalom található a gyorsítótárban.

Név | Cél
-----|--------
Sávszélesség-paraméterek | Meghatározza, hogy a sávszélesség-szabályozási paraméterek (értsd ec_rate és ec_prebuf) aktív legyen.
Sávszélesség-szabályozás | Azelőtt gyorsítja fel a peremhálózati kiszolgáló által biztosított hello válasz hello sávszélesség.
Gyorsítótár megkerülése | Meghatározza, hogy hello kérelem kihasználhatják a gyorsítótárazási technológiánk.
A Cache-Control fejléc kezelése | Vezérlők aktív külső maximális-életkora szolgáltatás esetén hello hello peremhálózati kiszolgáló által a Cache-Control fejlécek előállítását.
Gyorsítótár-kulcs lekérdezési karakterlánc | Meghatározza, hogy hello gyorsítótár-kulcsot fogja vagy kizárja a lekérdezési karakterlánc paramétereinek a kérelemhez társított.
Gyorsítótár-kulcs átdolgozás | Hello gyorsítótár-kulcs a kérelemhez társított újraírja.
Fejezze be a gyorsítótár kitöltés | Meghatározza, hogy mi történik, amikor egy kérelem egy részleges gyorsítótár-tévesztései eredményez az egy biztonsági kiszolgálót.
Fájltípusok tömörítése | Meghatározza a hello formátumú fájlok tömörítése hello kiszolgálón.
Alapértelmezett belső maximális életkora | Meghatározza, hogy hello alapértelmezett maximális-életkora időköze peremhálózati kiszolgáló tooorigin kiszolgáló gyorsítótár ismételt érvényesítése.
Fejléc kezelés lejár | Vezérlők hello Expires fejléc generációja által egy biztonsági kiszolgálót, aktív hello külső maximális-életkora szolgáltatás esetén.
Külső maximális-kor | Meghatározza a böngésző tooedge kiszolgáló gyorsítótár ismételt érvényesítése hello maximális-életkora intervallumát.
Kényszerített belső maximális életkora | Határozza meg a peremhálózati kiszolgáló tooorigin kiszolgáló gyorsítótár ismételt érvényesítése hello maximális-életkora intervallumát.
H.264 támogatási (HTTP a progresszív letöltés) | Meghatározza, hogy H.264 hello típusú fájlformátumokat, amelyek esetleg használt toostream tartalom.
No-Cache kérés elfogadása | Meghatározza, hogy egy HTTP no-cache ügyfélkéréseket a rendszer továbbítja toohello eredeti kiszolgálóra.
Figyelmen kívül hagyja az eredeti No-Cache | Meghatározza, hogy a CDN egyes egy forráskiszolgálóról kiszolgált irányelvek figyelmen kívül hagyja.
Hagyja figyelmen kívül Unsatisfiable tartományok | Meghatározza, hogy hello válaszban visszaadott tooclients amikor egy kérelem egy 416 kért tartomány nem teljesíthető állapotkódot állítja elő.
Belső maximális elavult | Meghatározza, mennyi ideig kezdeményezheti a gyorsítótárazott eszköz hello normál lejárati ideje korábbi egy biztonsági kiszolgálót a hello biztonsági kiszolgáló esetén nem toorevalidate hello hello eredeti kiszolgálóra a gyorsítótárazott objektum.
A részleges gyorsítótári megosztása | Azt határozza meg, hogy egy kérelem hozhat létre a részlegesen gyorsítótárazott tartalmat.
Gyorsítótárazott tartalom prevalidate | Meghatározza, hogy a gyorsítótárazott tartalmat legyen abban az esetben jogosult a korai ismételt érvényesítése a TTL lejárata előtt.
Nulla bájtos gyorsítótárban levő fájlok frissítése | Meghatározza, hogy a 0 bájtos gyorsítótár eszköz egy HTTP-ügyfél kérelmet a rendszer hogyan kezelje a peremhálózati kiszolgáló.
Állítsa be a gyorsítótárazható állapotkódok | Meghatározza a gyorsítótárazott tartalom eredményező állapotkódok hello készletét.
Hiba történt a régi Tartalomkézbesítési | Meghatározza, hogy lejárt gyorsítótárazott tartalmát kézbesíti a rendszer hiba esetén a gyorsítótár ismételt érvényesítése során, vagy ha a kért tartalom lekérése során hello hello ügyfél forráskiszolgálóról.
Elavult Revalidate közben | Így a tooserve elavult ügyfél toohello kérelmező peremhálózati kiszolgálókról során kerül sor az ismételt érvényesítése javítja a teljesítményt.
Megjegyzés | hello Megjegyzés funkció lehetővé teszi, hogy a Megjegyzés toobe hozzáadva a szabályban.

###<a name="bandwidth-parameters"></a>Sávszélesség-paraméterek
**Cél:** meghatározza, hogy a sávszélesség-szabályozási paraméterek (értsd ec_rate és ec_prebuf) aktív legyen.

Sávszélesség-szabályozási paraméter határozza meg, hogy hello adatátviteli sebesség az ügyfélkérések lesz-e a korlátozott tooa egyéni sebessége.

Érték|eredménye
--|--
Engedélyezve|Lehetővé teszi, hogy a peremhálózati kiszolgálóinak toohonor a sávszélesség-szabályozási kérelmeket.
Letiltva|A peremhálózati kiszolgálóinak tooignore a sávszélesség-szabályozási paraméterek okoz. a kért hello tartalmat szolgáltató általában (azaz nélkül sávszélesség-szabályozás).

**Alapértelmezés:** engedélyezve van.

###<a name="bandwidth-throttling"></a>Sávszélesség-szabályozás
**Cél:** szabályozások hello hello válasz peremhálózati kiszolgálókról által biztosított sávszélesség.

Sávszélesség-szabályozás beállítása meghatározott tooproperly mindkét alábbi beállítások hello kell lennie.

Beállítás|Leírás
--|--
Kilobájt / másodperc|Állítsa be ezt a beállítást toohello maximális sávszélesség (Kb / s), amelyek esetleg használt toodeliver hello válasz.
Prebuf másodpercben|Állítsa be ezt a beállítást toohello peremhálózati kiszolgálókról megvárja a sávszélesség szabályozási másodpercek számát. hello ebben az időszakban nem korlátozott sávszélesség célja egy akadozás vagy pufferelés toobandwidth szabályozás miatt problémákat tapasztalhat a media player tooprevent.

**Alapértelmezés:** letiltva.

###<a name="bypass-cache"></a>Gyorsítótár megkerülése
**Cél:** határozza meg, hogy hello kérelem kihasználhatják a gyorsítótárazási technológiánk.

Érték|eredménye
--|--
Engedélyezve|Hatására az összes kérelmek toofall toohello forrás-kiszolgálón keresztül, még akkor is, ha hello tartalom korábban a peremhálózati kiszolgálóinak kerül a gyorsítótárba.
Letiltva|Hatására a peremhálózati kiszolgálóinak toocache eszközök toohello gyorsítótár házirend lett meghatározva, a saját válaszfejlécek szerint.

**Alapértelmezés:**

- **A HTTP nagy:** letiltva

<!---
- **ADN:** Enabled
--->

###<a name="cache-control-header-treatment"></a>Gyorsítótár vezérlő fejléce kezelése
**Cél:** Cache-Control fejlécek hello generációja meghatározza a hello peremhálózati kiszolgáló aktív külső maximális-életkora szolgáltatás esetén.

Ez a konfiguráció típus tooplace hello külső maximális-kor és hello Cache-Control fejléc-kezelés szolgáltatások legegyszerűbb módja tooachieve hello hello ugyanabban az utasításban.

Érték|eredménye
--|--
Felülírása|Biztosítja, hogy a következő műveletek hello akkor kerül sor:<br/> -Felülírja a Cache-Control-fejlécet hello eredeti kiszolgálón állítja elő. <br/>-A Cache-Control-fejlécet, külső maximális-életkora szolgáltatás toohello válasz hello által előállított ad.
Továbbítása|Biztosítja a Cache-Control-fejlécet hello külső maximális-életkora szolgáltatás által előállított soha nem adja hozzá a toohello válasz. <br/> Ha a forráskiszolgáló hello Cache-Control fejléc, akkor továbbítja a toohello végfelhasználói. <br/> Ha hello eredeti kiszolgálóra nem hozhatók létre a beállítást, akkor a Cache-Control fejléc OK hello válasz fejléce toonot tartalmazhat egy Cache-Control-fejlécet.
Ha hiányoznak hozzáadása|A Cache-Control fejléc nem érkezett hello forráskiszolgálóról, ha ezt a beállítást a Cache-Control-fejlécet hello külső maximális-életkora szolgáltatás által létrehozott ad hozzá. Ez a beállítás akkor hasznos, annak biztosítására, hogy az összes eszköz hozzá lesz rendelve a Cache-Control-fejlécet.
Eltávolítás| Ez a beállítás biztosítja, hogy a Cache-Control fejléc nem hello fejléc válasz tartalmazza. Ha már használja egy Cache-Control-fejlécet, majd azt fogja lehet üres karaktert törölni a hello válasz fejrészét.

**Alapértelmezés:** felülírásához.

###<a name="cache-key-query-string"></a>Gyorsítótár-kulcs lekérdezési karakterlánc
**Cél:** beállítja, hogy a gyorsítótár-kulcs tartalmaznak, vagy zárja ki a lekérdezési karakterlánc paramétereinek a kérelemhez társított.

Kapcsolatos információkat:

- Adjon meg egy vagy több lekérdezési karakterlánc paraméter neve. Minden egyes paraméternév kell tagolt, egy szóközzel.
- Ez a szolgáltatás határozza meg, hogy lekérdezési karakterlánc paramétereket tartalmazza vagy kizárását hello gyorsítótár-kulcs. További információ az egyes lehetőségek közül.

Típus|Leírás
--|--
 Tartalmazza|  Azt jelzi, hogy minden egyes megadott paraméter hello gyorsítótár-kulcsot kell foglalni. Egyedi gyorsítótár-kulcs jön létre minden olyan kérelmet, amely a lekérdezési karakterlánc paraméterként, ez a szolgáltatás definiált egyedi értéket tartalmaz. 
 Az összes  |Azt jelzi, hogy egyedi gyorsítótár-kulcsot hoz létre minden egyes kérelem tooan eszköz, amely egy egyedi lekérdezési karakterláncot tartalmaz. Az ilyen típusú konfigurációs általában nem javasolt, mert azt eredményezheti, találatot eredményező gyorsítótárbeli kereséseinek tooa kis százalékát. Ez növeli hello terhelése hello eredeti kiszolgálóra, mivel kell tooserve, akkor további kérelmeket. Ez a konfiguráció "egyedi-gyorsítótár" a lekérdezési karakterlánc-gyorsítótár oldalon néven gyorsítótárazásának hello lehet azonos. 
 Kizárása | Azt jelzi, hogy csak hello megadott paraméterek nem kerülnek bele az hello gyorsítótár-kulcs. Minden más lekérdezési karakterlánc paraméter szerepelni fog a hello gyorsítótár-kulcs. 
 Az összes kihagyása  |Azt jelzi, hogy az összes lekérdezési karakterlánc paraméterei nem kerülnek bele az hello gyorsítótár-kulcs. Ez a konfiguráció ismétlődések hello alapértelmezett gyorsítótárazásának működése, a lekérdezési karakterlánc-gyorsítótár oldalon "standard-gyorsítótár" néven ismert. 

HTTP szabályok motor hello teljesítménye lehetővé teszi a toocustomize hello módon, amelyben lekérdezési karakterláncok gyorsítótárazása történik. Megadhatja például, hogy az egyes helyek vagy fájltípusok lekérdezési karakterláncok gyorsítótárazása csak végezhető.

Ha szeretné tooduplicate hello lekérdezési karakterláncok gyorsítótárazásának működése, úgynevezett "no-cache" a lekérdezési karakterlánc-gyorsítótár lapon, akkor szüksége lesz egy szabályt, amely egy URL-cím lekérdezés helyettesítő egyezés feltételt, valamint a Mellőzés gyorsítótár szolgáltatás tartalmaz toocreate. hello URL-cím lekérdezés helyettesítő egyezés feltétel tooan csillag (*) kell beállítani.

#### <a name="sample-scenarios"></a>Mintaforgatókönyvek

Példa a szolgáltatáshoz tartozó lejjebb tekinthetők meg. Kérelemmintát és hello alapértelmezett gyorsítótár-kulcs alatt találhatók.

- **Mintakérelem:** http://wpc.0001.&lt; Tartomány&gt;/800001/Origin/folder/asset.htm?sessionid=1234 & nyelvi = EN & userid = 01
- **Alapértelmezett gyorsítótár-kulcs:** /800001/Origin/folder/asset.htm

##### <a name="include"></a>Tartalmazza

Mintakonfiguráció:

- **Típus:** tartalmazza
- **Paraméterek:** nyelv

Az ilyen típusú konfigurációs hoz létre a lekérdezési karakterlánc paraméter gyorsítótár-kulcsot következő hello:

    /800001/Origin/folder/asset.htm?language=EN

##### <a name="include-all"></a>Az összes

Mintakonfiguráció:

- **Típus:** az összes

Az ilyen típusú konfigurációs hoz létre a lekérdezési karakterlánc paraméter gyorsítótár-kulcsot következő hello:

    /800001/Origin/folder/asset.htm?sessionid=1234&language=EN&userid=01

##### <a name="exclude"></a>Kizárása

Mintakonfiguráció:

- **Típus:** kizárása
- **Paraméterek:** sessionid userid

Az ilyen típusú konfigurációs hoz létre a lekérdezési karakterlánc paraméter gyorsítótár-kulcsot következő hello:

    /800001/Origin/folder/asset.htm?language=EN

##### <a name="exclude-all"></a>Az összes kihagyása

Mintakonfiguráció:

- **Típus:** összes kizárása

Az ilyen típusú konfigurációs hoz létre a lekérdezési karakterlánc paraméter gyorsítótár-kulcsot következő hello:

    /800001/Origin/folder/asset.htm

###<a name="cache-key-rewrite"></a>Gyorsítótár-kulcs átdolgozás
**Cél:** Újraírások hello a kérelemhez társított gyorsítótár-kulcs.

A gyorsítótár kulcsának hello relatív elérési utat, amely azonosítja a gyorsítótárazás hello alkalmazásában eszközként. Más szóval a kiszolgálóink ellenőrzi, hogy egy eszköz tooits elérési útja, a gyorsítótár-kulcs által meghatározottak szerint a gyorsítótárazott verzió.

Ez a szolgáltatás meghatározása mindkét alábbi beállítások hello konfigurálása:

Beállítás|Leírás
--|--
Eredeti elérési útja| Adja meg a hello relatív elérési út toohello típusú kérelmet, amelynek gyorsítótárkulcshoz felülíródik. Relatív elérési útnak egy alap forrás elérési útvonalának kiválasztásával, és egy Reguláriskifejezés-mintának majd meghatározása definiálhatók.
Új elérési útja|Hello relatív elérési útnak hello új gyorsítótár-kulcs megadása. Relatív elérési útnak egy alap forrás elérési útvonalának kiválasztásával, és egy Reguláriskifejezés-mintának majd meghatározása definiálhatók. A relatív elérési út dinamikusan lehet létrehozni a HTTP-változók hello használata
**Alapértelmezés:** hello kérelem URI-azonosítója egy kérelem gyorsítótár-kulcs határozza meg.

###<a name="complete-cache-fill"></a>Fejezze be a gyorsítótár kitöltés
**Cél:** határozza meg, mi történik, amikor egy kérelem egy részleges gyorsítótár-tévesztései egy biztonsági kiszolgálót a eredményez.

A részleges gyorsítótár-tévesztései hello gyorsítótár állapotát, amely nem volt teljesen letöltött tooan biztonsági kiszolgáló eszköz ismerteti. Ha az eszköz csak részben gyorsítótárában van egy biztonsági kiszolgálót, majd hello az adott eszköz számára a következő kérés továbbítja újra toohello eredeti kiszolgálóra.
<!---
This feature is not available for hello ADN platform. hello typical traffic on this platform consists of relatively small assets. hello size of hello assets served through these platforms helps mitigate hello effects of partial cache misses, since hello next request will typically result in hello asset being cached on that POP.
--->
A részleges gyorsítótár-tévesztései rendszerint azért fordul elő, miután a felhasználó megszakít egy letölthető vagy eszközök kizárólag kért tartomány HTTP-kérelmek használatával. Ez rendkívül nagy eszközök esetén a leghasznosabb ahol felhasználók nem általában letöltik azokat a start toofinish (például videók). Ennek köszönhetően ez a funkció alapértelmezés szerint engedélyezve van hello HTTP nagy platformon. Le van tiltva az összes többi platformon.

Ajánlott tooleave hello alapértelmezett konfigurációjának hello HTTP nagy platform, mivel a felhasználói származási kiszolgáló hello terhelésének csökkentése, és az ügyfelek töltse le a tartalmat hello sebesség növelése.

Lejáró toohello módon mely gyorsítótárában beállítások nyomon követi, ez a funkció nem rendelhető hozzá a következő feltételek egyeznek hello: peremhálózati Cname, kérelem fejléc literális, kérelem fejléc helyettesítő, URL-cím lekérdezés literális és URL-cím lekérdezés helyettesítő.

Érték|eredménye
--|--
Engedélyezve|Visszaállítja a hello alapértelmezett működését. hello alapértelmezés tooforce hello peremhálózati kiszolgáló tooinitiate hello forráskiszolgálóról hello eszköz a háttérben történő lesz. Amely után a helyi gyorsítótárban hello biztonsági kiszolgáló hello eszköz lesz.
Letiltva|Megakadályozza, hogy egy biztonsági kiszolgálót az hello eszköz a háttérben történő végrehajtásához. Ez azt jelenti, hogy hello következő kérés, az adott eszközre, régió, akkor a peremhálózati kiszolgáló toorequest hello ügyfél forráskiszolgálóról azt.

**Alapértelmezés:** engedélyezve van.

###<a name="compress-file-types"></a>Fájltípusok tömörítése
**Cél:** határozza meg, amelyek tömörítése hello fájlformátumok hello kiszolgálón.

Egy fájl formátum használatával az Internet az adathordozó típusát (pl., Content-Type) adható meg. Internet adathordozó-típus, amely lehetővé teszi a kiszolgálók tooidentify hello fájlformátum, ha egy adott eszköz platformfüggetlen metaadatok. Közös Internet adathordozó-típusok listája lejjebb tekinthetők meg.

Internet adathordozó-típus|Leírás
--|--
egyszerű szöveg|Egyszerű szöveges fájlokról
text/html| HTML-fájlok
Text/css|Egymásra épülő stíluslap (CSS)
alkalmazás/x-javascript|Javascript
alkalmazás/javascript|Javascript
Kapcsolatos információkat:

- Adjon meg egy szóköz egyenként a határoló többféle adathordozó-típust. 
- Ez a funkció csak ügy módosul, amelynek mérete 1 MB-nál kevesebb eszközök. A kiszolgálók nagyobb eszközök nem lesznek tömörítve.
- Bizonyos típusú tartalmakra, például a video-lemezképek és lejátszása eszközök (pl., JPG, MP3, MP4 stb.), már tömörített. Ilyen típusú eszközök további tömörítés nem jelentősen romolhat méretét. Ezért ajánlott, hogy nem engedélyezi az ilyen típusú eszközök tömörítést.
- Helyettesítő karakterek, például a csillag, nem támogatottak.
- Ez a szolgáltatás tooa szabály hozzáadásának előtt győződjön meg arról, hogy tooset hello platform toowhich Ez a szabály lesz alkalmazva a tömörítési oldal tömörítési letiltott beállítás.

###<a name="default-internal-max-age"></a>Alapértelmezett belső maximális életkora
**Cél:** megállapítja hello alapértelmezett maximális-életkora időköze peremhálózati kiszolgáló tooorigin kiszolgáló gyorsítótár ismételt érvényesítése. Más szóval hello időtartam egy biztonsági kiszolgálót ellenőrzi, hogy egy gyorsítótárazott eszköz megfelel-e a hello eszköz hello forrás kiszolgálón tárolt előtt adja meg, hogy.

Kapcsolatos információkat:

- Ez a művelet csak akkor kerül sor a válaszok egy forráskiszolgálóról, amely nem társít egy maximális-életkora arra utal, hogy a Cache-Control vagy Expires fejléc található.
- Ez a művelet nem kerül sor eszközök fontosságúnak ítélt nem gyorsítótárazható.
- Ez a művelet nem befolyásolja a böngésző tooedge kiszolgáló gyorsítótár revalidations. Ilyen típusú revalidations határozza meg a Cache-Control vagy Expires fejléc küldése toohello browser bővítményt, amely testre szabható a külső maximális-életkora szolgáltatással.
- Ez a művelet eredményét hello nem hatással van a megfigyelhető hello válaszfejlécek és hello tartalmat adott vissza a tartalom peremhálózati kiszolgálókról, de hello mennyisége peremhálózati kiszolgálók tooyour forráskiszolgálóról adatforgalom ismételt érvényesítése hatással lehet.
- Ez a szolgáltatás által konfigurálása:
    - Egy alapértelmezett belső maximális-életkora alkalmazhassa hello állapotkód kiválasztása.
    - Adja meg egy egész számot, és jelölje be hello kívánt időegység (azaz, másodperc, perc, óra, stb.). Ez az érték határozza meg a hello alapértelmezett belső maximális-életkora időköz.

- Beállítás hello időegység túl "Off" hozzárendel egy alapértelmezett belső maximális-életkora 7 napos időszak a kéréseket, amely nincs hozzárendelve egy maximális-életkora arra utal, hogy a Cache-Control vagy Expires fejléc.
- Lejáró toohello módon mely gyorsítótárában beállítások nyomon követi ez a funkció nem rendelhető hozzá a következő feltételek egyeznek hello: 
    - Peremhálózati 
    - CNAME
    - Kérelem fejléc szövegkonstans
    - Kérelem fejléc helyettesítő
    - Kérési módszer
    - Lekérdezés-szövegkonstans URL-címe
    - Lekérdezés helyettesítő URL-címe

**Alapértelmezett érték:** 7 nap

###<a name="expires-header-treatment"></a>Fejléc kezelés lejár
**Cél:** által egy biztonsági kiszolgálót hello generációs Expires fejléc szabályozza, ha a külső maximális-életkora szolgáltatás aktív.

Ez a konfiguráció típus tooplace hello külső maximális-kor és hello lejár fejléc-kezelés szolgáltatások legegyszerűbb módja tooachieve hello hello ugyanabban az utasításban.

Érték|eredménye
--|--
Felülírása|Biztosítja, hogy a következő műveletek hello akkor kerül sor:<br/>-Felülírja a Expires fejléc hello eredeti kiszolgálón állítja elő.<br/>-A hello külső maximális-életkora szolgáltatás toohello válasz által előállított Expires fejléc hozzáadása.
Továbbítása|Biztosítja a hello külső maximális-életkora szolgáltatás által előállított Expires fejléc soha nem adja hozzá a toohello válasz. <br/> Ha a forráskiszolgáló hello egy Expires fejléc, akkor továbbítja a toohello végfelhasználói. <br/>Ha a forráskiszolgáló hello nem hozhatók létre ezt a beállítást, akkor egy Expires fejléc OK hello válasz fejléce toonot tartalmazhat egy Expires fejléc.
Ha hiányoznak hozzáadása| Nem érkezett meg egy Expires fejléc hello forráskiszolgálóról, ha ez a beállítás nagyobb a hello külső maximális-életkora szolgáltatás által előállított Expires fejléc. Ez a beállítás akkor hasznos, annak biztosítására, hogy az összes eszköz kioszt egy Expires fejléc.
Eltávolítás| Ellenőrzi, hogy egy Expires fejléc nem hello fejléc válasz tartalmazza. Ha már használja egy Expires fejléc, majd azt fogja lehet üres karaktert törölni a hello fejléc válaszban.

**Alapértelmezés:** felülírása

###<a name="external-max-age"></a>Külső maximális-kor
**Cél:** megállapítja hello maximális-életkora intervallumát böngésző tooedge kiszolgáló gyorsítótár ismételt érvényesítése. Más szóval a hello ennyi idő alatt, a böngésző előtt adja meg, hogy egy eszköz egy biztonsági kiszolgálót az új verzióhoz tartozó ellenőrizheti.

A funkció engedélyezése hoz létre gyorsítótár-vezérlő: maximális-kor és lejár, a peremhálózati kiszolgálókról fejlécek és elküldhesse az összeset toohello HTTP-ügyfél. Alapértelmezés szerint ezek a fejlécek hello származási kiszolgáló által létrehozott felülírja. Azonban a Cache-Control fejléc kezelését és a lejárati fejléc-kezelés szolgáltatások is használt tooalter kell ezt a viselkedést.

Kapcsolatos információkat:

- Ez a művelet nem befolyásolja a peremhálózati kiszolgáló tooorigin kiszolgáló gyorsítótár revalidations. Az ilyen típusú revalidations határozza meg a gyorsítótár-Control vagy Expires fejléc hello származási kiszolgálótól kapott, és testre szabható az alapértelmezett belső maximális-kor és a kényszerített belső maximális életkora szolgáltatásokkal.
- Adja meg egy egész számot, majd válassza a hello kívánt idő egysége (azaz, másodperc, perc, óra, stb.) Ez a szolgáltatás konfigurálása
- Ez a szolgáltatás tooa negatív érték beállítása után a peremhálózati kiszolgáló toosend a gyorsítótár-vezérlő: nincs-gyorsítótárat, és egy Expires idő hello a múltbeli minden válasz toohello böngészővel. Bár a HTTP-ügyfél nem gyorsítótárazhatják az hello választ, ez a beállítás nem lesz hatással a peremhálózati kiszolgálóinak képességét toocache hello válasz hello forráskiszolgálóról.
- Beállítás hello időegység túl "Off" letiltja ezt a szolgáltatást. A gyorsítótár-Control vagy Expires fejléc hello válasz hello származási kiszolgáló gyorsítótárazott toohello böngésző fog továbbítani.

**Alapértelmezés:** kikapcsolása

###<a name="force-internal-max-age"></a>Kényszerített belső maximális életkora
**Cél:** megállapítja hello maximális-életkora intervallumát peremhálózati kiszolgáló tooorigin kiszolgáló gyorsítótár ismételt érvényesítése. Más szóval hello időtartam egy biztonsági kiszolgálót ellenőrizheti, hogy a gyorsítótárazott eszköz megfelel-e a hello forrás kiszolgálón tárolt hello eszköz előtt adja meg, hogy.

Kapcsolatos információkat:

- Ez a szolgáltatás felülírják hello maximális-életkora időköze a Cache-Control vagy Expires fejléc jön létre egy forráskiszolgálóról van megadva.
- Ez a funkció nem befolyásolja a böngésző tooedge kiszolgáló gyorsítótár revalidations. Az ilyen típusú revalidations határozza meg a Cache-Control vagy Expires fejléc küldése toohello böngésző.
- Ez a szolgáltatás nem rendelkezik a peremhálózati kiszolgáló toohello kérelmező kézbesítette hello válasz megfigyelhető hatással. Azonban a forráskiszolgálóról a peremhálózati kiszolgáló toohello ismételt érvényesítése adatforgalom mennyisége hello hatása lehet.
- Ez a szolgáltatás által konfigurálása:
    - Amely egy belső maximális életkora alkalmazza hello állapotkód kiválasztása.
    - Adja meg egy egész számot, és kiválasztja hello kívánt időegység (azaz, másodperc, perc, óra, stb.). Ez az érték határozza meg a hello kérelem maximális-életkora időköz.

- Beállítás hello időegység túl "Off" letiltja a szolgáltatást. Belső maximális-életkora időközönkénti nem lesz hozzárendelve toorequested eszközök. Ha hello eredeti fejléc nem tartalmaz a gyorsítótárazási információkat, majd hello eszköz a gyorsítótárba fognak kerülni toohello aktív beállítás az alapértelmezett belső maximális-életkora funkció szerint.
- Lejáró toohello módon mely gyorsítótárában beállítások nyomon követi ez a funkció nem rendelhető hozzá a következő feltételek egyeznek hello: 
    - Peremhálózati 
    - CNAME
    - Kérelem fejléc szövegkonstans
    - Kérelem fejléc helyettesítő
    - Kérési módszer
    - Lekérdezés-szövegkonstans URL-címe
    - Lekérdezés helyettesítő URL-címe

**Alapértelmezés:** kikapcsolása

###<a name="h264-support-http-progressive-download"></a>H.264 támogatási (HTTP a progresszív letöltés)
**Cél:** H.264 hello típusú megállapítja a fájlformátumokat, amelyek esetleg használt toostream tartalom.

Kapcsolatos információkat:

- A kívánt fájlkiterjesztéseket beállítás engedélyezett H.264 fájlnév-kiterjesztések egy szóközökkel elválasztott kötetnevek kulcstulajdonságokat határozza meg. A kívánt fájlkiterjesztéseket beállítás felülírja a hello alapértelmezett viselkedését. Ha ezt a beállítást, többek között azokat a fájlkiterjesztéseket karbantartásához MP4 és F4V támogatása. 
- Győződjön meg arról, hogy tooinclude egy időszakot minden fájlnév-kiterjesztés (pl. .mp4 .f4v) megadásakor.

**Alapértelmezés:** alapértelmezés szerint HTTP progresszív letöltés MP4 és F4V media támogatja.

###<a name="honor-no-cache-request"></a>No-cache kérés elfogadása
**Cél:** határozza meg, hogy egy HTTP no-cache ügyfélkéréseket a rendszer továbbítja toohello eredeti kiszolgálóra.

Hello HTTP-ügyfél üzenetet küld egy gyorsítótárban történik a no-cache kérelem-vezérlés: nem-gyorsítótár és/vagy Pragma:no-gyorsítótár fejléc hello HTTP-kérelem.

Érték|eredménye
--|--
Engedélyezve|Lehetővé teszi, hogy egy HTTP-ügyfél no-cache kér toobe továbbított toohello eredeti kiszolgálóra, és a forráskiszolgáló hello visszaadható hello válaszfejlécek és hello törzs hello a biztonsági kiszolgálón keresztül hátsó toohello HTTP-ügyfél.
Letiltva|Visszaállítja a hello alapértelmezett működését. hello alapértelmezés tooprevent no-cache kérelmek továbbítását toohello eredeti kiszolgálóra lesz.

Minden éles forgalom esetén magas ajánlott tooleave Ez a funkció le van tiltva alapértelmezett állapotában. Ellenkező esetben származási kiszolgálók fog védelme nem biztosítható a végfelhasználóktól, akik véletlenül indíthat sok no-cache kérelem weblapok frissítésekor, a hello számos népszerű médialejátszókhoz, amelyek a forráskódban, sem toosend no-cache fejléc minden videó kérelemhez. Azonban ez a szolgáltatás lehet hasznos tooapply toocertain nem éles szakaszhoz, vagy igény szerinti sorrendben tooallow friss tartalom toobe könyvtárak, teszteléséhez lekért hello eredeti kiszolgálóra.

hello gyorsítótár állapota továbbított toobe tooan eredeti kiszolgálóra toothis szolgáltatás miatt TCP_Client_Refresh_Miss engedélyezett kérelmet küldött. A gyorsítótár állapotok jelentést, amely nem érhető el a hello fő jelentéskészítési modul, a gyorsítótár állapot szerint statisztikai adatokat biztosít. Ez lehetővé teszi, hogy tootrack hello száma és a forráskiszolgáló tooan miatt toothis szolgáltatás továbbított kérelmek százalékos aránya.

**Alapértelmezés:** letiltva.

###<a name="ignore-origin-no-cache"></a>Figyelmen kívül hagyja az eredeti no-cache
**Cél:** határozza meg, hogy a CDN figyelmen kívül hagyja a következő irányelvek egy forráskiszolgálóról kiszolgált hello:

- A Cache-Control: titkos
- A Cache-Control: no-tároló
- A Cache-Control: no-cache
- Pragma: no-cache

Kapcsolatos információkat:

- Ez a szolgáltatás konfigurálja, amelynek a fenti irányelvek hello figyelmen kívül állapotkódok szóközökkel elválasztott kötetnevek listáját meghatározásával.
- hello beállítása a szolgáltatáshoz tartozó érvényes állapotkódok vannak: 200, 203, 300, 301, 302, 305, 307, 400, 401-es, 402, 403-as, 404, 405, 406, 407, 408, 409, 410, 411, 412, 413, 414, 415, 416, 417, 500, 501, 502-es, 503-as, 504, és 505.
- Ez a funkció letiltása értékre állításával tooa üres érték.
- Lejáró toohello módon mely gyorsítótárában beállítások nyomon követi ez a funkció nem rendelhető hozzá a következő feltételek egyeznek hello: 
    - Peremhálózati 
    - CNAME
    - Kérelem fejléc szövegkonstans
    - Kérelem fejléc helyettesítő
    - Kérési módszer
    - Lekérdezés-szövegkonstans URL-címe
    - Lekérdezés helyettesítő URL-címe

**Alapértelmezés:** az alapértelmezett viselkedés toohonor hello irányelvek felett.

###<a name="ignore-unsatisfiable-ranges"></a>Hagyja figyelmen kívül Unsatisfiable tartományok 
**Cél:** meghatározza hello választ visszaadott tooclients amikor egy kérelem egy 416 kért tartomány nem teljesíthető állapotkódot állítja elő.

Alapértelmezés szerint ez az állapot kód érték érkezett vissza hello megadott bájttartomány kérelem nem tud teljesíteni egy biztonsági kiszolgálót, és egy If tartományon kívüli kérelmet fejlécmező nem volt megadva.

Érték|eredménye
-|-
Engedélyezve|Megakadályozza, hogy a kérelemből válaszol tooan bájttartomány érvénytelen egy 416 kért tartomány nem teljesíthető állapotkód: peremhálózati kiszolgálókról. Ehelyett a kiszolgálóink hello kért eszköz biztosítanak, és térjen vissza a 200 OK hello ügyfél.
Letiltva|Visszaállítja a hello alapértelmezett működését. hello alapértelmezett működése a 416 kért tartomány nem teljesíthető állapotkód: toohonor.

**Alapértelmezés:** letiltva.

###<a name="internal-max-stale"></a>Belső maximális elavult
**Cél:** meghatározza, mennyi ideig múltbeli hello normál lejárati ideje a gyorsítótárazott eszköz előfordulhat, hogy szolgálható ki egy biztonsági kiszolgálót, ha hello peremhálózati kiszolgáló nem toorevalidate hello gyorsítótárazott eszköz hello eredetű-kiszolgálóval.

Általában ha az eszköz maximális-életkora időtartam lejár, a hello biztonsági kiszolgáló elküld egy ismételt érvényesítése kérelem toohello eredeti kiszolgálóra. hello eredeti kiszolgálóra majd válaszol, és vagy a 304 biztosítanak hello biztonsági kiszolgálót egy friss bérleti hello gyorsítótárazott eszköz – a 200 OK adja meg a hello peremhálózati kiszolgáló hello gyorsítótárazott eszköz frissített verzióra nem módosultak.

Hello peremhálózati kiszolgáló nem tooestablish ilyen ismételt érvényesítése közben hello forráskiszolgáló kapcsolatot, akkor ez a belső maximális elavult funkció szabályozza, e, és mennyi ideig hello a peremhálózati kiszolgáló továbbra is tooserve hello most már elavult eszköz-e.

Vegye figyelembe, hogy ez alatt az időtartam alatt lejártakor hello eszköz maximális-életkora nem hello sikertelen ismételt érvényesítése akkor fordul elő, amikor elindul. Hello maximális időszak, amely alatt az eszköz sikeres ismételt érvényesítése nélkül szolgáltatható ezért hello időn megadott maximális-kor és max elavult hello kombinációja. Például ha egy eszköz, 9:00 30 perc maximális életkora és 15 perc elteltével maximális elavult gyorsítótárazva lett, majd egy sikertelen ismételt érvényesítése kíséreljék meg a 9:44 eredményezne egy végfelhasználói fogadó hello elavult gyorsítótárazott eszköz, míg egy sikertelen ismételt érvényesítése kísérlet 9:46 eredményezne hello végfelhasználói 504-es számú átjáró időtúllépése fogadásakor.

Bármely ezt a szolgáltatást felváltotta gyorsítótár konfigurált érték-vezérlés: kell-kísérelje meg újra érvényesítését, vagy a gyorsítótár-vezérlés: proxy-kísérelje meg újra érvényesítését fejlécek hello származási kiszolgálótól kapott. Ha ezeket a fejléceket fogadásakor hello forráskiszolgálóról, ha egy eszköz kezdetben gyorsítótárazza, majd hello biztonsági kiszolgáló nem teljesíti a gyorsítótárazott elavult eszköz. Ebben az esetben ha hello biztonsági kiszolgáló hello eredete nem toorevalidate hello eszköz maximális-életkora intervalluma lejárta, majd hello biztonsági kiszolgáló adja vissza 504-es számú átjáró időtúllépése.

Kapcsolatos információkat:

- Ez a szolgáltatás által konfigurálása:
    - Hello állapotkód, amelynek maximális elavult alkalmazandó kiválasztása.
    - Adja meg egy egész számot, és jelölje be hello kívánt időegység (azaz, másodperc, perc, óra, stb.). Ez az érték határozza meg a hello belső maximális elavult alkalmazni fogja őket.

- Beállítás hello időegység túl "Off" letiltja ezt a szolgáltatást. A gyorsítótárazott eszköz nem tudja megjeleníteni a normál lejárati ideje túl.
- Lejáró toohello módon mely gyorsítótárában beállítások nyomon követi ez a funkció nem rendelhető hozzá a következő feltételek egyeznek hello: 
    - Peremhálózati 
    - CNAME
    - Kérelem fejléc szövegkonstans
    - Kérelem fejléc helyettesítő
    - Kérési módszer
    - Lekérdezés-szövegkonstans URL-címe
    - Lekérdezés helyettesítő URL-címe

**Alapértelmezés:** 2 perc

###<a name="partial-cache-sharing"></a>A részleges gyorsítótári megosztása
**Cél:** határozza meg, hogy egy kérelem hozhat létre a részlegesen gyorsítótárazott tartalmat.

A részleges gyorsítótári majd lehet használt toofulfill új kérelmek az adott tartalmakra vonatkozóan, amíg hello a kért tartalom gyorsítótárazva van, teljes mértékben.

Érték|eredménye
-|-
Engedélyezve|Kérelmek hozhat létre a részlegesen gyorsítótárazott tartalmat.
Letiltva|Kérelmek csak egy teljesen gyorsítótárazott hozhat létre a kért tartalom hello verzióját.

**Alapértelmezés:** letiltva.

###<a name="prevalidate-cached-content"></a>Gyorsítótárazott tartalom prevalidate
**Cél:** meghatározza, hogy a gyorsítótárazott tartalmat legyen abban az esetben jogosult a korai ismételt érvényesítése a TTL lejárata előtt.

Adja meg a hello időn hello előzetes toohello lejáratának a kért tartalom TTL során, amely jogosult a korai ismételt érvényesítése lesz.

Kapcsolatos információkat:

- Ismételt érvényesítése tootake hely kijelölésével "Off" hello időegység van szükség, hello gyorsítótárazott tartalom TTL lejárata után. Idő nem adható meg, és figyelmen kívül.

**Alapértelmezés:** ki. Ismételt érvényesítése után a gyorsítótárazott tartalmat TTL lejárt hello csak kerülhet sor.

###<a name="refresh-zero-byte-cache-files"></a>Nulla bájt gyorsítótárban levő fájlok frissítése
**Cél:** határozza meg a 0 bájtos gyorsítótár eszköz egy HTTP-ügyfél kérelmet a rendszer hogyan kezelje a peremhálózati kiszolgáló.

Érvényes értékek a következők:

Érték|eredménye
--|--
Engedélyezve|A peremhálózati kiszolgáló toore-fetch hello eszköz hatására hello forráskiszolgálóról.
Letiltva|Visszaállítja a hello alapértelmezett működését. hello alapértelmezés lesz tooserve be érvényes gyorsítótár eszközöket kérésre.
Ez a funkció nincs szükség a megfelelő gyorsítótárazás és a továbbítási, de hasznos megoldás lehet. Például a dinamikus tartalom generátorokat származási kiszolgálókon véletlenül eredményezhet 0 bájtos válaszok toohello peremhálózati kiszolgálóinak küldi el. Ilyen típusú válaszokat a rendszer jellemzően gyorsítótárzza a peremhálózati kiszolgáló. Ha tudja, hogy, hogy a 0 bájtos válasz soha nem egy érvényes válasz 

az ilyen tartalmat majd ezt a szolgáltatást előfordulhat, hogy ilyen típusú eszközök tooyour ügyfelek kiszolgálását.

**Alapértelmezés:** letiltva.

###<a name="set-cacheable-status-codes"></a>Állítsa be a gyorsítótárazható állapotkódok
**Cél:** meghatározza a gyorsítótárazott tartalom eredményező állapotkódok hello készletét.

Alapértelmezés szerint gyorsítótárazás csak a 200 OK válaszok engedélyezték.

Egy szükséges hello állapotkódok szóközökkel elválasztott kötetnevek a kulcstulajdonságokat határozza meg.

Kapcsolatos információkat:

- Engedélyezze a is az eredeti No-Cache figyelmen kívül hagyása funkció. Ha nincs engedélyezve a szolgáltatást, majd 200 OK válaszok előfordulhat, hogy nem gyorsítótárazza.
- hello beállítása a szolgáltatáshoz tartozó érvényes állapotkódok vannak: 203, 300, 301, 302, 305, 307, 400, 401-es, 402, 403-as, 404, 405, 406, 407, 408, 409, 410, 411, 412, 413, 414, 415, 416, 417, 500, 501, 502-es, 503-as, 504, és 505.
- Ez a funkció nem lehet egy 200 OK állapotkód eredményező válaszok gyorsítótárazásának használt toodisable.

**Alapértelmezés:** 200 OK állapotkód eredményező válaszok esetében csak engedélyezni a gyorsítótárazást.
###<a name="stale-content-delivery-on-error"></a>Hiba történt a régi Tartalomkézbesítési
**Cél:** 

Meghatározza, hogy lejárt gyorsítótárazott tartalmát kézbesíti a rendszer hiba esetén a gyorsítótár ismételt érvényesítése során, vagy ha a kért tartalom lekérése során hello hello ügyfél forráskiszolgálóról.

Érték|eredménye
-|-
Engedélyezve|Elavult tartalmat szolgáltató toohello kérelmező egy kapcsolat tooan eredeti kiszolgálóra során hiba esetén.
Letiltva|hello eredeti kiszolgálóra hiba toohello kérelmező rendszer továbbítja.

**Alapértelmezés:** letiltva

###<a name="stale-while-revalidate"></a>Elavult Revalidate közben
**Cél:** javítja a teljesítményt tételével peremhálózati kiszolgálókról tooserve elavult tartalom toohello kérelmező kerül sor az ismételt érvényesítése közben.

Kapcsolatos információkat:

- a szolgáltatás működését hello függően toohello kijelölt időegység függően változik.
    - **Időegység:** adja meg az az időtartam, és válassza ki a idő (pl., másodperc, perc, óra, stb.) egység tooallow elavult tartalomkézbesítési. Az ilyen típusú telepítés lehetővé teszi, hogy hello CDN tooextend hello, hogy mennyi ideig kérhet tartalom toohello a következő képlet szerint érvényesítés megkövetelése előtt:**TTL** + **elavult közben kísérelje meg újra érvényesítését idő** 
    - **OFF:** elavult tartalom kezdeményezheti a "Off" toorequire ismételt érvényesítése kérelmet előtt válasszon ki.
        - Ne adjon meg az időtartam, mert törlendő, és figyelmen kívül hagyja.

**Alapértelmezés:** ki. Ismételt érvényesítése előtt hello a kért tartalom szolgáltatható kell történnie.

###<a name="comment"></a>Megjegyzés
**Cél:** lehetővé teszi, hogy a Megjegyzés toobe hozzáadva a szabályban.

A funkció használatát tooprovide további tájékoztatást az általános célú hello egy szabály vagy miért egy adott feltétel vagy a szolgáltatás egyezik-e a toohello szabály hozzá lett adva.

Kapcsolatos információkat:

- Legfeljebb 150 karakter adható meg.
- Győződjön meg arról, hogy tooonly alfanumerikus karaktereket használjon.
- Ez a funkció nem befolyásolja a hello szabály hello viselkedését. Egy olyan területre, ahol megadhatja információt későbbi használatra, vagy a segíthet hello szabály hibaelhárítási tooprovide csupán célja.
 
## <a name="headers"></a>Fejlécek

Ezek a szolgáltatások akkor tervezett tooadd, módosítására és törlésére fejlécek hello kérés vagy válasz.

Név | Cél
-----|--------
Kor válaszfejléc | Határozza meg, hogy egy kora válaszfejléc fog toohello kérelmező küldi hello válaszként.
Gyorsítótár válaszfejlécek hibakeresése | Határozza meg, hogy a válasz tartalmazhatják hello X-EK-Debug válaszfejléc amely információt nyújt a kért objektum hello hello gyorsítótár szabályzatát.
Ügyfél fejléc módosítása | Felülírja, hozzáfűzi vagy fejléc töröl egy kérelmet.
Módosítsa az ügyfél válaszfejléc | Felülírja, hozzáfűzi vagy fejléc töröl egy választ.
Ügyfél IP-egyéni fejléc beállítása | Lehetővé teszi a hello kérést küldő ügyfél IP-címe hello toobe hozzáadott toohello kérések egyéni fejléc szerint.

###<a name="age-response-header"></a>Kor válaszfejléc
**Cél**: határozza meg, hogy egy kora válaszfejléc lesz a hello küldött válaszok toohello kérelmező.
Érték|eredménye
--|--
Engedélyezve | hello kora válaszfejléc hello választ küldött a kérelmező toohello fognak szerepelni.
Letiltva | hello kora válaszfejléc nem kerülnek bele a hello választ küldött a kérelmező toohello.

**Alapértelmezett viselkedés**: letiltva.

###<a name="debug-cache-response-headers"></a>Gyorsítótár válaszfejlécek hibakeresése
**Cél:** határozza meg, hogy a válasz lehet, hogy tartalmazzák-e az X-EK-Debug válaszfejléc, amely információval szolgál a kért objektum hello hello gyorsítótár szabályzatát.

Hibakeresési gyorsítótári választ fejlécek szerepelnek hello válasz mindkét hello következő teljesülése esetén:

- hello kívánt kérésre hello Debug gyorsítótár válasz fejlécek funkció engedélyezve van.
- újabb kérés hello hibakeresési gyorsítótár válaszfejlécek, amely szerepelni fog a hello válasz hello csoportját határozza meg.

Gyorsítótár válasz fejlécek többek között a következő fejléc hello kérheti és hello kérelem kívánt irányelvek hello Debug:

X-EK-Debug: _Directive1_,_Directive2_,_DirectiveN_

**Példa**

X-EK-Debug: x-ec-cache,x-ec-check-cacheable,x-ec-cache-key,x-ec-cache-state

Érték|eredménye
-|-
Engedélyezve|Hibakeresési gyorsítótár válaszfejlécek kérelmek visszaadható egy választ, amely az X-EK-Debug fejlécet tartalmaz.
Letiltva|Az X-EK-Debug válaszfejléc nem kerülnek bele a hello válaszban.

**Alapértelmezés:** letiltva.

###<a name="modify-client-response-header"></a>Módosítsa az ügyfél válaszfejléc
**Cél:** minden kérelmet tartalmaz [kérelem fejlécei]() , amely írják le. Ez a szolgáltatás következő lehetőségek közül választhat:

- Hozzáfűzése vagy hozzárendelt tooa fejléc hello érték felülírása. Ha a megadott kérelemfejlécet hello nem létezik, majd ezt a szolgáltatást felveszi toohello kérelmet.
- Törölje a fejléc hello kérelemből.

Kérelmek tooan eredeti kiszolgálóra továbbított hello módosítások ezt a funkciót fogja tartalmazni.

A következő műveletek hello egyik fejléc hajtható végre:

Beállítás|Leírás|Példa
-|-|-
Hozzáfűzés|hello megadott értéket ad hozzá toend a hello meglévő kérelem fejléc értéke.|**A kérelem fejléc értéke (ügyfél):**érték1 <br/> **Kérelem fejléc értéke (HTTP szabálymotor):** érték2 <br/>**Új kérelem fejléc értéke:** Value1Value2
Felülírása|hello kérelem fejléc értéke lesz beállítva toohello megadott érték.|**A kérelem fejléc értéke (ügyfél):**érték1 <br/>**Kérelem fejléc értéke (HTTP szabálymotor):** érték2 <br/>**Új kérelem fejléc értéke:** érték2 <br/>
Törlés|Törli a hello megadott kérelemfejlécet.|**A kérelem fejléc értéke (ügyfél):**érték1 <br/> **Ügyfél fejléc konfigurációjának módosítása:** Delete hello fejléc az adott. <br/>**Eredmény:** hello megadott kérelemfejlécet nem lesznek továbbítva toohello eredeti kiszolgálóra.

Kapcsolatos információkat:

- Győződjön meg arról, hogy hello értéket adtak meg a beállítás a kívánt hello fejléc pontos egyezést.
- Eset nem veszi figyelembe hello célja, hogy a fejléc. Például a Cache-Control fejlécnév változatait a következő hello bármelyike lehet használt tooidentify azt:
    - a cache-control
    - A CACHE-CONTROL
    - a cachE-Control
- Győződjön meg arról, hogy tooonly alfanumerikus karaktereket, kötőjelek és aláhúzásjelek egy állomásfejlécnevet megadásakor használhatók.
- A fejléc törlése megakadályozzák a tooan eredeti kiszolgálóra a peremhálózati kiszolgáló által továbbított.
- hello következő fejlécek fenntartva, ezért ezt a funkciót nem lehet módosítani:
    - továbbított
    - állomás
    - keresztül
    - Figyelmeztetés
    - x-továbbított-számára
    - Az összes fejléc neve az "x-EK" vannak fenntartva.

###<a name="modify-client-response-header"></a>Módosítsa az ügyfél válaszfejléc
Minden válasz tartalmaz [válaszfejlécek]() , amely írják le. Ez a szolgáltatás következő lehetőségek közül választhat:

- Hozzáfűzendő vagy tooa válaszfejléc hozzárendelt hello érték felülírása. Ha a megadott kérelemfejlécet hello nem létezik, majd ezt a szolgáltatást felveszi toohello választ.
- Törölje a válaszfejléc hello válasz.

Alapértelmezés szerint válasz fejléce értékek meg vannak határozva az eredeti kiszolgálóra és a peremhálózati kiszolgálókról.

Hello a következő műveletek egyikét a válaszfejléc hajtható végre:

Beállítás|Leírás|Példa
-|-|-
Hozzáfűzés|hello megadott értéket ad hozzá toend a hello meglévő kérelem fejléc értéke.|**Válasz állomásfejléc-érték (ügyfél):**érték1 <br/> **Válasz állomásfejléc-érték (HTTP szabálymotor):** érték2 <br/>**Új válasz állomásfejléc-érték:** Value1Value2
Felülírása|hello kérelem fejléc értéke lesz beállítva toohello megadott érték.|**Válasz állomásfejléc-érték (ügyfél):**érték1 <br/>**Válasz állomásfejléc-érték (HTTP szabálymotor):** érték2 <br/>**Új válasz állomásfejléc-érték:** érték2 <br/>
Törlés|Törli a hello megadott kérelemfejlécet.|**A kérelem fejléc értéke (ügyfél):** érték1 <br/> **Ügyfél kérelem fejléc konfigurációjának módosítása:** Delete hello válaszfejléc az adott. <br/>**Eredmény:** hello megadott válaszfejlécet nem lesznek továbbítva toohello kérelmező.

Kapcsolatos információkat:

- Győződjön meg arról, hogy hello értéket adtak meg a beállítás a hello kívánt válaszfejléc pontos egyezést. 
- Eset nem veszi figyelembe hello célja, hogy a fejléc. Például a Cache-Control fejlécnév változatait a következő hello bármelyike lehet használt tooidentify azt:
    - a cache-control
    - A CACHE-CONTROL
    - a cachE-Control
- A fejléc törlése megakadályozzák a toohello kérelmező lesznek továbbítva.
- hello következő fejlécek fenntartva, ezért ezt a funkciót nem lehet módosítani:
    - fogadja el-kódolás
    - kora
    - kapcsolat
    - tartalom kódolása
    - tartalom hosszúságú
    - tartalom tartományon.
    - Dátum
    - kiszolgáló
    - pótkocsi
    - Transfer-encoding
    - Frissítés
    - eltérő
    - keresztül
    - Figyelmeztetés
    - Az összes fejléc neve az "x-EK" vannak fenntartva.

###<a name="set-client-ip-custom-header"></a>Ügyfél IP-egyéni fejléc beállítása
**Cél:** egy egyéni fejlécet, amely azonosítja a hello kérést küldő ügyfél IP-cím toohello kérelem ad.

A fejléc neve beállítás hello egyéni kérelemfejléc hello ügyfél IP-cím tárolására szolgáló hello nevét adja meg.

Ez a funkció lehetővé teszi, hogy az ügyfél eredeti kiszolgáló toofind ügyfél IP-címek egyéni fejléc a kimenő. Ha hello kérés van kiszolgálása a gyorsítótárból, majd hello eredeti kiszolgálóra nem értesülnek hello ügyfél IP-cím. Ezért ajánlott, hogy ez a szolgáltatás használható ADN vagy eszközök, amelyek nem kerülnek a gyorsítótárba.

Győződjön meg arról, hogy hello megadott fejléc neve nem egyezik meg a következő hello:

- Standard kérelem nevével. Szokásos fejlécben neveinek listáját található [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).
- Fenntartott nevek:
    - továbbítja a
    - állomás
    - eltérő
    - keresztül
    - Figyelmeztetés
    - x-továbbított-számára
    - Az összes fejléc neve az "x-EK" vannak fenntartva.
 
## <a name="logs"></a>Logs

Ezek a szolgáltatások akkor tervezett toocustomize hello adatok nyers naplófájlokat tárolja.

Név | Cél
-----|--------
1. egyéni mező | Meghatározza, hogy hello formátum és hello tartalom toohello egyéni mező egy nyers naplófájlban hozzá lesz rendelve.
Napló lekérdezési karakterlánc | Meghatározza, hogy egy lekérdezési karakterlánc hello URL hozzáférés naplókban együtt kell tárolni.

###<a name="custom-log-field-1"></a>1. egyéni mező
**Cél:** hello formátum és hello tartalom toohello egyéni mező egy nyers naplófájlban rendelendő határozza meg.

hello fő célja az egyéni mező mögött tooallow toodetermine, mely kérés- és a fejléc értékei fogja tárolni a rendszernapló fájljaiban.

Alapértelmezés szerint hello egyéni mező neve "x-ec_custom-1." Azonban ez a mező nevét hello szabható a [nyers naplózási beállításai oldal]().

formázás, hogy használjon toospecify kérés- és válaszfejlécekről hello van megadva.

Fejléc típusa|Formátumban|Példák
-|-|-
Fejléc|%{[RequestHeader]()}[i]() | A(z) % {elfogadja-kódolás} i <br/> {Hivatkozó} i <br/> A(z) % {engedélyezési} i
Válaszfejléc|%{[ResponseHeader]()}[o]()| A(z) % {kora} o <br/> A(z) % {content-Type} o <br/> A(z) % {cookie-k} o

Kapcsolatos információkat:

- Egy egyéni napló mező fejlécmezők és egyszerű szöveg tetszőleges kombinációját tartalmazhatja.
- Az ebben a mezőben érvényes karakterek a következők hello alábbi: alfanumerikus (azaz a 0-9, a – z és A-Z), kötőjeleket, kettőspontokat, pontosvesszővel kell elválasztani, aposztrófot, vesszővel válassza el egymástól, időszakok, aláhúzásjeleket, egyenlőségjelet, kerek zárójeleket tartalmazhatnak, zárójelek és szóközöket. hello százalékos szimbólum és a kapcsos zárójelek csak engedélyezett mikor toospecify egy fejlécmező használt.
- minden egyes megadott fejlécmező hello helyesírását hello kívánt kérelem/válasz fejléc neve meg kell egyeznie.
- Ha azt szeretné, hogy toospecify több fejlécek, akkor az ajánlott, hogy használjon egy elválasztó tooindicate minden fejléc. Használhat például rövidítése minden fejléc. Szintaxis lejjebb tekinthetők meg.
    - AE: % {elfogadja-kódolás} i A: % {engedélyezési} i ki: % {Content-Type} o 

**Alapértelmezett érték:** -

###<a name="log-query-string"></a>Napló lekérdezési karakterlánc
**Cél:** határozza meg, hogy a lekérdezési karakterlánc hello URL hozzáférés naplókban együtt kell tárolni.

Érték|eredménye
-|-
Engedélyezve|Tárolás hello olyan lekérdezési karakterláncokról, egy hozzáférési napló URL-címek rögzítésekor. Ha egy URL-címe nem tartalmazhat lekérdezési karakterláncot, majd ezt a beállítást nem lesz hatása.
Letiltva|Visszaállítja a hello alapértelmezett működését. hello alapértelmezett viselkedés esetén tooignore lekérdezési karakterláncok URL-címek egy hozzáférési naplóban.

**Alapértelmezés:** letiltva.

<!---
## Optimize

These features determine whether a request will undergo hello optimizations provided by Edge Optimizer.

Name | Purpose
-----|--------
Edge Optimizer | Determines whether Edge Optimizer can be applied tooa request.
Edge Optimizer – Instantiate Configuration | Instantiates or activates hello Edge Optimizer configuration associated with a site.

###Edge Optimizer
**Purpose:** Determines whether Edge Optimizer can be applied tooa request.

If this feature has been enabled, then hello following criteria must also be met before hello request will be processed by Edge Optimizer:

- hello requested content must use an edge CNAME URL.
- hello edge CNAME referenced in hello URL must correspond tooa site whose configuration has been activated in a rule.

This feature requires the ADN platform and hello Edge Optimizer feature.

Value|Result
-|-
Enabled|Indicates that hello request is eligible for Edge Optimizer processing.
Disabled|Restores hello default behavior. hello default behavior is toodeliver content over the ADN platform without any additional processing.

**Default Behavior:** Disabled
 

###Edge Optimizer - Instantiate Configuration
**Purpose:** Instantiates or activates hello Edge Optimizer configuration associated with a site.

This feature requires the ADN platform and hello Edge Optimizer feature.

Key information:

- Instantiation of a site configuration is required before requests toohello corresponding edge CNAME can be processed by Edge Optimizer.
- This instantiation only needs toobe performed a single time per site configuration. A site configuration that has been instantiated will remain in that state until hello Edge Optimizer – Instantiate Configuration feature that references it is removed from hello rule.
- hello instantiation of a site configuration does not mean that all requests toohello corresponding edge CNAME will automatically be processed by Edge Optimizer. The Edge Optimizer feature determines whether an individual request will be processed.

If hello desired site does not appear in hello list, then you should edit its configuration and verify that the Active option has been marked.

**Default Behavior:** Site configurations are inactive by default.
--->

## <a name="origin"></a>Forrás

Ezek a szolgáltatások akkor tervezett toocontrol hogyan kommunikál a hello CDN az eredeti kiszolgálóra.

Név | Cél
-----|--------
Életben tartási kérelmek maximális száma | Hello életben tartási kapcsolat kérelmek maximális számát határozza meg, mielőtt le van zárva.
Proxy különleges fejlécek | CDN-specifikus kérelemfejléc forráskiszolgálóról egy peremhálózati kiszolgáló tooan továbbított hello csoportját határozza meg.


###<a name="maximum-keep-alive-requests"></a>Életben tartási kérelmek maximális száma
**Cél:** hello életben tartási kapcsolat kérelmek maximális számát határozza meg, mielőtt le van zárva.

Tooa alacsony értékre kérelmek maximális száma hello beállítása nem ajánlott, és teljesítménycsökkenést eredményezhet.

Kapcsolatos információkat:

- Adja meg ennek az értéknek egész számként.
- Ne vegyen fel vesszőt és pontot hello megadott értéket.

**Alapértelmezett érték:** 10 000 kérelem

###<a name="proxy-special-headers"></a>Proxy különleges fejlécek
**Cél:** hello csoportját határozza meg [CDN-specifikus kérelemfejléc]() , hogy a rendszer továbbítja forráskiszolgálóról egy peremhálózati kiszolgáló tooan.

Kapcsolatos információkat:

- Minden egyes CDN-specifikus kérelemfejléc, ez a szolgáltatás meghatározott tooan eredeti kiszolgálóra továbbítja.
- CDN-specifikus fejléc megakadályozása továbbítsa tooan forráskiszolgáló eltávolítása a listáról.

**Alapértelmezés:** összes [CDN-specifikus kérelemfejléc]() toohello eredeti kiszolgálóra továbbítja.

## <a name="specialty"></a>Speciális

Ezeket a funkciókat biztosítanak speciális funkcióit, amelyek csak haladó felhasználóknak használják.

Név | Cél
-----|--------
Kérelmeznék HTTP-metódus | Meghatározza, hogy a hálózaton gyorsítótárazható további HTTP-metódus hello készletét.
Kérelmeznék kérelem törzse mérete | Határozza meg a meghatározásához, hogy a FELADÁS egy vagy több válasz gyorsítótárazható hello küszöbértéket.

###<a name="cacheable-http-methods"></a>Kérelmeznék HTTP-metódus
**Cél:** hello halmaza gyorsítótárazható, a hálózat további HTTP-metódus határozza meg.

Kapcsolatos információkat:

- Ez a szolgáltatás azt feltételezi, hogy GET válaszok mindig gyorsítótárazza. Ennek eredményeképpen hello első HTTP-metódus nem lehet megtalálható ez a funkció beállítása során.
- Ez a funkció csak a POST HTTP-metódus hello támogatja. POST válasz úgy, hogy ez a funkció gyorsítótárazás engedélyezése: POST 
- Alapértelmezés szerint csak kérelmeket amelynek törzs 14 Kb-nál kisebb a gyorsítótárba fognak kerülni. A kérelem törzsében kérelmeznék mérete szolgáltatás hello maximális kérelem törzse méretének beállításához használja.

**Alapértelmezés:** csak GET válaszok a gyorsítótárba fognak kerülni.

###<a name="cacheable-request-body-size"></a>Kérelmeznék kérelem törzse mérete

**Cél:** meghatározza hello küszöbérték meghatározásához, hogy a FELADÁS egy vagy több válasz gyorsítótárazható.

Ez a küszöbérték megadása a kérelem maximális mérete határozza meg. Egy nagyobb kérelemtörzset tartalmazó kérelmek nem gyorsítótárazza.

Kapcsolatos információkat:

- A szolgáltatás csak akkor alkalmazható, ha a POST válaszok jogosultak a gyorsítótárazás. Hello kérelmeznék HTTP módszerek funkció használatával engedélyezze a POST-kérések gyorsítótárazása.
- hello kérelemtörzset a figyelembe venni:
    - x-www-form-urlencoded értékek
    - Egyedi gyorsítótár-kulcs biztosítása
- A nagy méretű kérelem maximális mérete meghatározása hatással lehetnek az adatok kézbesítési teljesítményére.
    - **Javasolt érték:** 14 Kb
    - **Minimális érték:** 1 Kb

**Alapértelmezés:** 14 Kb
 
## <a name="url"></a>URL-CÍME

Ezek a szolgáltatások lehetővé egy kérelem toobe átirányítva, vagy írni tooa másik URL-címet.

Név | Cél
-----|--------
Átirányítások követése | Meghatározza, hogy kérelmeket a hello helyet megjelölő fejlécet egy ügyfél eredeti kiszolgáló által visszaadott definiált átirányított toohello állomásnév lehet.
Az átirányítási URL-címe | Kérelmek hello helyet megjelölő fejlécet keresztül irányítja át.
URL-cím átdolgozás  | Újraírja hello kérelem URL-CÍMÉT.

###<a name="follow-redirects"></a>Átirányítások követése
**Cél:** meghatározza, hogy a kérelmek képes legyen a hely egy ügyfél eredeti kiszolgáló által visszaadott fejlécében megadott átirányított toohello állomásnév.

Kapcsolatos információkat:

- Kérelmek csak lehet, hogy toohello átirányított tooedge CNAME ugyanahhoz a platformhoz.

Érték|eredménye
-|-
Engedélyezve|Kérelmek átirányíthatók.
Letiltva|Kérelmek nem irányítja át.

**Alapértelmezés:** letiltva.
###<a name="url-redirect"></a>Az átirányítási URL-címe
**Cél:** átirányítja a felhasználókat a hely fejléce kérelmeket.

Ez a szolgáltatás konfigurációját hello szükséges beállítása az alábbi beállítások hello:

Beállítás|Leírás
-|-
Kód|Válassza ki a toohello kérelmező visszaadott hello válaszkódot.
Forrás & minta| Ezek a beállítások megadása a kérelem URI mintát, amely azonosítja, hogy előfordulhat, hogy átirányítja kérelem hello típusát. A rendszer átirányítja az URL-cím megfelel a következő feltételek hello mindegyikét csak kérelmeket: <br/> <br/> **Forrás:** (vagy a tartalom-hozzáférési pont) válasszon, amely azonosítja az eredeti kiszolgálóra relatív elérési utat. Ez az hello "/XXXX/" szakaszban és a végpont neve. <br/> **Forrás (minta):** egy mintát, amely azonosítja a kérelmek relatív elérési utat kell megadni. A reguláris kifejezési minta definiálnia kell egy elérési utat, amely közvetlenül elindul, miután hello korábban kiválasztott tartalom-hozzáférési pont (lásd fent). <br/> – Ellenőrizze, hogy fent definiált hello kérelem URI feltételek (például a forrás & mintát) a szolgáltatáshoz tartozó bármely egyezés egyiknek nem ütközik. <br/> -Győződjön meg arról, hogy toospecify egy mintát. Hello mintát üres érték használata csak azokkal a kérelmek toohello hello kijelölt eredeti kiszolgálóra (pl. http://cdn.mydomain.com/) gyökérmappájában.
Cél| Hello URL-cím megadása toowhich hello fent kérelmek irányítja. <br/> Az URL-cím használatával dinamikusan össze: <br/> -A reguláris kifejezési minta <br/>-HTTP változók <br/> Hello cél mintát $ használatával történő hello forrás mintában rögzített hello értékeket helyettesítse _n_  ahol  _n_  hello sorrendje akkor rögzített érték azonosítja. $1 például hello rögzített hello forrás mintát, amíg $2 hello második értékét jelöli első értékét jelöli. <br/> 
Nagyon fontos ajánlott toouse egy abszolút URL-CÍMÉT. hello használata egy relatív URL-cím lehet, hogy átirányítási CDN URL-címek tooan érvénytelen elérési út.

**Mintaforgatókönyv**

Ebben a példában bemutatjuk, hogyan tooredirect CNAME URL-címet feloldó toothis él kiinduló CDN URL-cím: http://marketing.azureedge.net/brochures

Megfelelő kérelmek alap peremhálózati CNAME URL-cím lesz átirányított toothis: http://cdn.mydomain.com/resources

Az URL-cím átirányítást a következő konfigurációs hello keresztül valósítható meg:![](./media/cdn-rules-engine-reference/cdn-rules-engine-redirect.png)

**Kulcs mutat:**

- hello átirányítási URL-cím szolgáltatás hello határozza meg a kérelem URL-címek, amelyek irányítja. Ennek eredményeképpen további egyezés feltételek esetén nincs szükség. "Always" hello egyezés feltétel van definiálva, de csak kérelmek adott pont toohello "brosúrák" hello "marketing" felhasználói származási mappájába irányítja. 
- Minden egyező kérelmek átirányított toohello szélén CNAME URL-címet a cél beállítás lesz. 
    - A minta #1. forgatókönyv: 
        - Mintakérelem (CDN URL): http://marketing.azureedge.net/brochures/widgets.pdf 
        - Kérelem URL-CÍMÉT (után átirányítási): http://cdn.mydomain.com/resources/widgets.pdf  
    - A minta #2. forgatókönyv: 
        - Mintakérelem (peremhálózati CNAME URL): http://marketing.mydomain.com/brochures/widgets.pdf 
        - Kérelem URL-CÍMÉT (után átirányítási): http://cdn.mydomain.com/resources/widgets.pdf mintaforgatókönyv
    - A minta #3. forgatókönyv: 
        - Mintakérelem (peremhálózati CNAME URL): http://brochures.mydomain.com/campaignA/final/productC.ppt 
        - Kérelem URL-CÍMÉT (után átirányítási): http://cdn.mydomain.com/resources/campaignA/final/productC.ppt  
- hello kérelem rendszer (% {séma}) változó lett kihasználhatók a cél beállítás. Ez biztosítja, hogy hello kérelem séma változatlan marad az átirányítást követően.
- hello kérelemből rögzítette hello URL-szegmensek hozzáfűzött toohello új URL-Címéhez révén "$1."
 
###<a name="url-rewrite"></a>URL-cím átdolgozás
**Cél:** újraírja hello kérelem URL-CÍMÉT.

Kapcsolatos információkat:

- Ez a szolgáltatás konfigurációját hello szükséges beállítása az alábbi beállítások hello:

Beállítás|Leírás
-|-
 Forrás & minta | Ezek a beállítások megadása a kérelem URI mintát, amely azonosítja, hogy előfordulhat, hogy be kell írni kérelem hello típusát. URL-cím megfelel a következő feltételek hello mindegyikét csak kérelmek felülíródik: <br/>     - **Forrás (vagy a tartalom-hozzáférési pont):** válasszon, amely azonosítja az eredeti kiszolgálóra relatív elérési utat. Ez az hello "/XXXX/" szakaszban és a végpont neve. <br/> - **Forrás (minta):** egy mintát, amely azonosítja a kérelmek relatív elérési utat kell megadni. A reguláris kifejezési minta definiálnia kell egy elérési utat, amely közvetlenül elindul, miután hello korábban kiválasztott tartalom-hozzáférési pont (lásd fent). <br/> Győződjön meg arról, hogy hello kérelem URI (pl., forrás & mintát) megadott feltételek fent nem ütközik a szolgáltatáshoz tartozó definiált hello egyezés feltételek. Győződjön meg arról, hogy toospecify egy mintát. Hello mintát üres érték használata csak azokkal a kérelmek toohello hello kijelölt eredeti kiszolgálóra (pl. http://cdn.mydomain.com/) gyökérmappájában. 
 Cél  |Hello relatív URL-cím megadása toowhich hello fent kérelmek felülíródik szerint: <br/>    1. A tartalom-hozzáférési pont, amely azonosítja az eredeti kiszolgálóra kiválasztása. <br/>    2. Annak meghatározása, egy relatív elérési út használatával: <br/>        -A reguláris kifejezési minta <br/>        -HTTP változók <br/> <br/> Hello cél mintát $ használatával történő hello forrás mintában rögzített hello értékeket helyettesítse _n_  ahol  _n_  hello sorrendje akkor rögzített érték azonosítja. $1 például hello rögzített hello forrás mintát, amíg $2 hello második értékét jelöli első értékét jelöli. 
 Ez a funkció lehetővé teszi, hogy a peremhálózati kiszolgálóinak toorewrite hello URL-cím egy hagyományos átirányítás végrehajtása nélkül. Ez azt jelenti, hogy hello kérelmező ugyanaz a válasz code, mintha átírása hello URL-cím kért hello fog kapni.

**Példa: 1. forgatókönyv**

Ebben a példában bemutatjuk, hogyan tooredirect CNAME URL-címet feloldó toothis él kiinduló CDN URL-cím: http://marketing.azureedge.net/brochures/

Megfelelő kérelmek alap peremhálózati CNAME URL-cím lesz átirányított toothis: http://MyOrigin.azureedge.net/resources/

Az URL-cím átirányítást a következő konfigurációs hello keresztül valósítható meg:![](./media/cdn-rules-engine-reference/cdn-rules-engine-rewrite.png)

**Mintaforgatókönyv 2**

Ebben a példában fogjuk mutatni, hogyan tooredirect CNAME URL-cím él a nagybetűs toolowercase reguláris kifejezésekkel.

Az URL-cím átirányítást a következő konfigurációs hello keresztül valósítható meg:![](./media/cdn-rules-engine-reference/cdn-rules-engine-to-lowercase.png)


**Kulcs mutat:**

- hello URL-újraíró szolgáltatás hello határozza meg a kérelem URL-címek, amelyek felülíródik. Ennek eredményeképpen további egyezés feltételek esetén nincs szükség. "Always" hello egyezés feltétel van definiálva, de csak kéri, hogy pont toohello "brosúrák" hello "marketing" felhasználói forrás mappa felülíródik.

- hello kérelemből rögzítette hello URL-szegmensek hozzáfűzött toohello új URL-Címéhez révén "$1."



###<a name="compatibility"></a>Kompatibilitás

Ez a funkció tartalmazza, amelyeknek teljesülniük kell ahhoz, azok alkalmazott tooa kérelem feltételeknek. Az ütköző feltételek rendelés tooprevent beállítása, ez a funkció nem kompatibilis a következő feltételek egyeznek hello:

- SZÁMOT
- CDN-forrása
- Ügyfél IP-címe
- Ügyfél forrása
- Kérelem séma
- URL-cím elérési út könyvtár
- URL-cím elérési út bővítmény
- URL-cím elérési út fájlnév
- URL-cím elérési út szövegkonstans
- URL-cím elérési út Regex
- URL-cím elérési út helyettesítő karakter
- Lekérdezés-szövegkonstans URL-címe
- URL-cím lekérdezési paraméter
- Lekérdezés Regex URL-címe
- Lekérdezés helyettesítő URL-címe


## <a name="next-steps"></a>Következő lépések
* [Szabályok motor referencia](cdn-rules-engine-reference.md)
* [Szabályok motor feltételes kifejezések](cdn-rules-engine-reference-conditional-expressions.md)
* [Szabályok motor egyezés feltételek](cdn-rules-engine-reference-match-conditions.md)
* [Alapértelmezett HTTP működés használata hello szabályok felülbírálása](cdn-rules-engine.md)
* [Az Azure CDN áttekintése](cdn-overview.md)
