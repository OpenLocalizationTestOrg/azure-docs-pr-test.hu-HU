---
title: "Az Azure Active Directory B2C: Hivatkozás - keretrendszerek megbízható |} Microsoft Docs"
description: "Azure Active Directory B2C egyéni házirendek és hello identitás élmény keretrendszer témája"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: joroja
ms.openlocfilehash: d9634da72cb136ac165dd32e735622b5d0e22ec3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="define-trust-frameworks-with-azure-ad-b2c-identity-experience-framework"></a>Az Azure AD B2C identitás élmény keretrendszerrel megbízhatósági keretrendszerek meghatározása

Az Azure Active Directory B2C hello identitás élmény keretrendszer használó egyéni házirendekkel (az Azure AD B2C), adja meg a szervezet egy központosított szolgáltatással. Ez a szolgáltatás egyik fontos a nagy közösségi identitás-összevonási hello összetettsége csökkenti. hello összetettségét csökkentett tooa egyetlen megbízhatósági kapcsolat egy egyetlen metaadatok exchange.

Hello identitás élmény keretrendszer tooenable használó Azure AD B2C egyéni házirendeket akkor tooanswer hello a következő kérdésekre:

- Mik azok a hello jogi, biztonsági, adatvédelmi és, hogy be kell tartani, adatvédelmi szabályzatok?
- Kik tartoznak hello névjegyeket és egy akkreditált résztvevő csatlakozási hello folyamatok Mik?
- Aki hello akkreditált Identitásszolgáltatók információkat (más néven: "jogcímek szolgáltatók"), és milyen lehetőségek kínálnak?
- Függő entitások számára, akik hello akkreditált (és szükség esetén mi szükséges a)?
- Mik azok a hello műszaki "a"hello keresztülhaladnak a hálózaton a résztvevők együttműködési követelményeket?
- Mik azok a digitális identitásokat információcsere kell kényszeríthető hello működési "futtatókörnyezet" szabályokat?

tooanswer összes ezeket a kérdéseket, az Azure AD B2C egyéni házirendek hello identitás élmény keretrendszer használata hello megbízható keretrendszer (TF) használó összeállításához. Mérlegeljük, ez a szerkezet és akkor biztosít.

## <a name="understand-hello-trust-framework-and-federation-management-foundation"></a>Hello és összevonási megbízhatósági keretében felügyeleti foundation ismertetése

hello megbízható keretrendszer hello azonosító, biztonsági, adatvédelmi és adatok adatvédelmi szabályzatok meg kell felelnie a érdeklő közösségi toowhich résztvevők írásbeli meghatározásához.

Összevont identitás alap biztosít végfelhasználói identitás megbízhatósági Internet léptékű elérésében. Identity management toothird felek delegálásával, a felhasználó egyetlen digitális identitásokat újrahasználhatók több függő entitást.  

Identitás megbízhatósági igényel, identitás-szolgáltatóktól (IdPs) és az attribútum szolgáltatók (AtPs) betartsák toospecific biztonsági, adatvédelmi, és működési házirendek és eljárásokat.  Ha azok nem hajtható végre a közvetlen ellenőrzések, függő entitások (RPs) ki kell dolgoznia megbízhatósági kapcsolata hello IdPs és AtPs toowork a választ.  

Fogyasztók és digitális azonosító adatok szolgáltatói hello számának növekedésével nehéz toocontinue páros felügyeleti ezek megbízhatósági kapcsolatban, vagy akár hello páros exchange hello műszaki metaadatot, amely a hálózati kapcsolathoz szükséges .  Összevonási hubok értek csak korlátozott sikeres, a problémák megoldásához.

### <a name="what-a-trust-framework-specification-defines"></a>Milyen a keretrendszer megbízhatósági specifikációval meghatározása
A TFs rendszer hello linchpins hello nyitott identitás Exchange (OIX) megbízható keretrendszer modell, ahol minden közösségi iránt, egy adott TF specifikációja szabályozzák. Meghatározza, hogy ilyen TF leírást:

- **hello biztonsági és adatvédelmi metrikáját hello közösségi érdeklő hello definícióját:**
    - hello a megbízhatósági szintek (LOA), amelyek a résztvevők; által kínált/szükséges Ha például egy digitális azonosító adatok hitelességének hello abban, hogy be a besorolások rendezett halmazát.
    - hello szintű védelmet (LOP), amelyek a résztvevők; által kínált/szükséges például egy rendezett halmazát abban, hogy minősítését hello védelmét digitális azonosító adatok, amelyek érdeklő hello Közösségben résztvevő kezeli.

- **hello digitális azonosító adatok, amely rendelkezik felajánlott/szükséges résztvevők által leírása hello**.

- **hello műszaki házirendek digitális azonosító adatok tartozó, és így LOA és LOP méri. Ezek a házirendek általában írása a következők a következő kategóriák házirendek hello:**
    - Identitás-ellenőrző a házirendek, például: *erősen hogyan van egy személy azonosító adatok vetted?*
    - Biztonsági házirendek, például: *információk sértetlenségét és bizalmasságát védett erősen hogyan vannak?*
    - Adatvédelmi szabályzatok, például: *ellenőrző a felhasználó rendelkezik személyes azonosításra alkalmas adatokat (PII) keresztül*?
    - Túlélést házirendek, például: *Ha egy szolgáltató működése megszűnik, hogyan működik folytonosságának és védelem személyazonosításra alkalmas függvény?*

- **műszaki profilok hello éles üzemi pontjának és digitális azonosító adatok felhasználása. Ezeket a profilokat a következők:**
    - Hatókör felületek, amelyek digitális azonosító adatok esetében érhető el a megadott LOA.
    - A tömörített együttműködési műszaki követelményei.

- **hello leírását hello különböző szerepkörökben, hogy hello Közösségben résztvevő végrehajtása és hello képesítéssel, amelyek a szükséges toofulfill ezeket a szerepköröket.**

Így a TF specifikációval szabályozza, hogyan történik a azonosító adatok hello Közösség érdeklő hello résztvevők között: a függő entitások, identitáskezelési és attribútum szolgáltatók és attribútum hitelesítők.

A TF specifikációval egy vagy több dokumentumok hello irányításhoz hello Közösség, amely szabályozza a hello helyességi feltétel érdeklő és digitális azonosító adatok hello Közösségben fogyasztásának referenciaként szolgálnak. A házirendek dokumentált és eljárások tervezett tooestablish megbízhatóságának hello digitális identitások érdeklő egy Közösség tagjai közötti internetes tranzakciók használt.  

Más szóval a TF specifikációval meghatározza, hogy hello szabályok közösségi életképes összevont identitás-ökoszisztéma létrehozásához.

Jelenleg nincs ilyen megközelítés hello előnye a széles körű szerződést. Nem kétséges, amely megbízhatósági keretrendszer specifikációk digitális identitásokat ökoszisztéma ellenőrizhető biztonsági, megbízhatósági és adatvédelmi jellemzőkkel, ami azt jelenti, hogy felhasználhatók között több csoport egyik fontos hello fejlesztésének elősegítése.

Emiatt hello identitás élmény keretrendszert használó Azure AD B2C egyéni házirendek használ hello specification hello alapjául a saját adatok egy TF toofacilitate együttműködését.  

Az Azure AD B2C egyéni házirendeket használó hello identitás élmény keretrendszer egy TF specifikációjának emberi és géppel olvasható adatok keverékével jelölik. Egyes szakaszok a modell (általában szakaszok több felé irányulnak irányítás) vannak megadva, toopublished biztonsági és adatvédelmi házirend dokumentációját együtt hello hivatkozik kapcsolódó eljárások (ha van ilyen). Más szakaszok ismertetik a részletes hello konfigurációs metaadatai és futásidejű szabályok, amelyek egyszerűbbé teszik a működési automation.

## <a name="understand-trust-framework-policies"></a>Keretrendszer megbízhatósági házirendek ismertetése

Tekintetében végrehajtására hello TF specification áll házirendjei, amelyek lehetővé teszik a teljes felügyeletet gyakorolhat az identitás-működést és a feladatait.  Hello identitás élmény keretrendszert használó Azure AD B2C egyéni házirendeket akkor tooauthor engedélyezése, és hozzon létre saját TF keresztül ilyen deklaratív házirendek meghatározására és konfigurálása:

- hello hivatkozás vagy hivatkozásai, amelyek meghatározzák a hello összevont identitás ökoszisztéma hello Közösség, amely toohello TF vonatkozik. Azok a hivatkozások toohello TF dokumentációját. hello (előre meghatározott) működési "futtatókörnyezet" szabályok vagy hello felhasználói utak, amely automatizálásához és/vagy hello exchange és hello jogcímek használatát szabályozza. Ezek az felhasználói utak társítva egy LOA (és egy LOP). Egy házirend ezért rendelkezhet felhasználói utak rendelkező különböző idő (és LOPs).

- hello identitás- és attribútum szolgáltatók vagy hello jogcím-szolgáltatók, hello közösségi érdeklődési és hello műszaki profilok támogatják hello (a sávon kívüli) LOA/LOP akkreditációs toothem kapcsolódó együtt.

- hello integrálását attribútum hitelesítők vagy jogcímszolgáltatóktól.

- hello függő entitások hello közösségi (által megállapítás).

- résztvevők közötti hálózati kommunikációk létrehozó hello metaadatait. Ezek a metaadatok hello műszaki profilok, a tranzakció tooplumb során használt "a"hello keresztülhaladnak a hálózaton együttműködésével hello függő entitáshoz, és más közösségi résztvevők.

- hello protokoll átalakítás, ha van ilyen (például a SAML-alapú, az OAuth2, WS-Federation, és az OpenID Connect).

- hello hitelesítési követelmények.

- hello többtényezős vezénylési ha van ilyen.

- Az összes rendelkezésre álló hello jogcímeket és hozzárendelések tooparticipants érdeklő közösségi megosztott séma.

- Minden hello jogcímek átalakítások, a környezetben, a toosustain hello exchange és a használati hello jogcímek hello lehetséges minimalizálásáról együtt.

- hello kötési és a titkosítás.

- hello tárolási jogcímek.

### <a name="understand-claims"></a>Jogcímek ismertetése

> [!NOTE]
> Együttesen irányítjuk tooall hello lehetséges típusait, előfordulhat, hogy felcserélhetők "jogcímként" azonosító adatok: a végfelhasználók hitelesítő adatok, a identitás vizsgálata során, a kommunikációs eszköz, a fizikai hely, jogcímek személyes azonosító attribútumok, és így tovább.  
>
> Mivel az online tranzakció, ezen adatok összetevők nincsenek hello függő entitás által közvetlenül ellenőrizhető tények hello kifejezés "jogcímek"--helyett "attribútum"--használjuk. Ahelyett, hogy fontosságúak helyességi feltételek vagy a jogcímeket, mely hello a függő entitás ki kell dolgoznia elegendő abban, hogy toogrant hello végfelhasználó által kért tranzakcióazonosító tények kapcsolatos.  
>
> Azt is használhatja a "jogcímek" mivel hello identitás élmény keretrendszert használó egyéni házirendek az Azure AD B2C toosimplify hello exchange típusú digitális azonosító adatok következetesen függetlenül attól, hogy hello az alapul szolgáló hello kifejezés protokoll a felhasználói hitelesítés vagy attribútum lekérdezés van meghatározva.  Hasonlóképpen a "jogcím-szolgáltatók" toocollectively tekintse meg a tooidentity szolgáltatók, az attribútum szolgáltatók és az attribútum hitelesítők amikor toodistinguish funkcióik között nem szeretnénk hello kifejezés használjuk.   

Így azok szabályozzák, hogyan történik a azonosító adatok egy függő entitás, identitáskezelési és attribútum szolgáltatók és attribútum hitelesítők között. Ezek szabályozhatja, hogy mely identitás és attribútum szolgáltatók a következők: egy függő entitás hitelesítéshez szükséges. Akkor kell tekinteni tartományspecifikus nyelv (DSL), ez azt jelenti, hogy egy számítógép nyelv, amely rendelkezik egy adott alkalmazás tartomány öröklési, kifejezetten *Ha* utasítások, polimorfizmus.

Ezek a házirendek hello TF összeállítani az Azure AD B2C egyéni házirendek hello identitás élmény keretrendszer kihasználva hello géppel olvasható részét alkotják. Minden hello működési adatait, többek között jogcím-szolgáltatók metaadatok és műszaki profilok, jogcímek sémadefiníciók, jogcímek átalakítása funkciók és felhasználói útvonal be, amely ki van töltve a toofacilitate működési vezénylési tartoznak és automatizálási.  

Ezek feltételezhetően toobe *élő dokumentumok* mert van esély arra, hogy azok tartalmát hello aktív résztvevők hello házirendek deklarált vonatkozó időbeli változik. Is van, amely hello használati feltételek rendelkező módosíthatja hello lehetséges.  

Összevonási telepítési és karbantartási jelentősen egyszerűsített folyamatos megbízható és a csatlakozási újrakonfigurálás a függő entitások védelmi különböző jogcím-szolgáltatók/hitelesítők csatlakoztassa, vagy hagyja (által képviselt hello Közösség) által hello házirendeket.

Együttműködési lehetőség egy másik jelentős kihívás. Integrálni kell a további jogcímek szolgáltatók/hitelesítők, mert a függő entitások valószínű toosupport összes hello szükséges protokollokat. Az Azure AD B2C egyéni házirendek szabványos protokollok támogatása a probléma megoldásához, és úgy, hogy alkalmazza a megadott felhasználó utak tootranspose kérelmek nem támogatják a függő entitások és attribútum szolgáltatók hello ugyanazt a protokollt.  

Felhasználói utak közé tartozik a protokoll-profilok és a metaadatokat, amelyek használt tooplumb "a"hello keresztülhaladnak a hálózaton együttműködésével hello függő entitáshoz, és más résztvevői. Alkalmazott tooidentity információs exchange kérelem/válasz üzenetet hello TF specification részeként közzétett házirendekkel való megfelelőség kényszerítése a működési futásidejű szabályokat is vannak. hello meghatározni, hogy a felhasználó utak a kulcs toohello testreszabása hello felhasználói élmény. Azt is világítsa hello rendszer működéséről hello protokoll szintjén.

Ennek alapján függő entitás alkalmazásai és portálokról esik szó, attól függően, hogy a környezetükben, hívhat meg egyéni házirendeket az Azure AD B2C, hogy használja ki az hello identitás élmény keretrendszer átadja egy adott házirend hello nevét, és pontosan hello viselkedést és tudnivalók az Exchange kívánják nélkül muss, fuss, vagy kockázatát.
