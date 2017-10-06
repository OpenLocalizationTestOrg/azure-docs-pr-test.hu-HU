---
title: "aaaConfigurable jogkivonat élettartamát az Azure Active Directoryban |} Microsoft Docs"
description: "Ismerje meg, hogyan jogkivonatok tooset élettartamai kiadott Azure ad."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 06f5b317-053e-44c3-aaaa-cf07d8692735
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: billmath
ms.custom: aaddev
ms.reviewer: anchitn
ms.openlocfilehash: 0d4c8545981c5463cc7c95f669167bbc38230123
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configurable-token-lifetimes-in-azure-active-directory-public-preview"></a>Konfigurálható jogkivonat élettartamát az Azure Active Directoryban (nyilvános előzetes verzió)
Azure Active Directory (Azure AD) által kiadott tokennek hello élettartama adhatja meg. A szervezet jogkivonat élettartamát a szervezet összes alkalmazást, egy több-bérlős (több szervezet) alkalmazáshoz, vagy egy adott szolgáltatás egyszerű állíthatja be.

> [!NOTE]
> Ez a funkció jelenleg nyilvános előzetes verziójához. Készüljön toorevert, vagy távolítsa el a módosításokat. egyetlen Azure Active Directory-előfizetéshez nyilvános előzetes hello szolgáltatás érhető el. Azonban amikor hello szolgáltatás általánosan elérhetővé válik, a hello szolgáltatás egyes funkcióit szükség lehet egy [Azure Active Directory Premium](active-directory-get-started-premium.md) előfizetés.
>
>

Az Azure AD egy csoportházirend-objektum csomagok olyan szabályokat, amelyek az egyes alkalmazásokra, illetve a szervezeten belüli összes alkalmazás lépnek érvénybe. Minden házirendtípus struktúrája egy egyedi, vannak beállítva, amelyek az alkalmazott tooobjects toowhich hozzájuk rendelve.

Kijelölhet egy házirend hello alapértelmezett házirendet a szervezet számára. hello házirend hello szervezetben alkalmazott tooany alkalmazás, mindaddig, amíg nem bírálják felül a magasabb prioritású házirend. Is hozzárendelhet egy házirend toospecific alkalmazások. házirend típusa függ a prioritásuk szerinti sorrendben hello.


## <a name="token-types"></a>Tokentípusokat

A frissítési jogkivonatokat, a hozzáférési jogkivonatok, a munkamenet-jogkivonatokat és az azonosító-jogkivonatokat a jogkivonatok élettartama házirendek állíthatja be.

### <a name="access-tokens"></a>Hozzáférési jogkivonatok
Ügyfelek használata hozzáférési jogkivonatok tooaccess védett erőforrás. Olyan hozzáférési jogkivonatot csak olyan felhasználó, az ügyfél és az erőforrás egyedi kombinációja használható. Hozzáférési jogkivonatok nem vonható vissza, és azok lejártáig érvényesek. Egy rosszindulatú szereplő, amely egy hozzáférési jogkivonatot kapott használhatja az élettartamuk mértékét. Hello élettartamának beállítása egy hozzáférési jogkivonat között, a rendszer teljesítményének növelése és a növekvő hello, hogy mennyi ideig hello client access őrzi meg hello felhasználói fiók le van tiltva. Továbbfejlesztett rendszerteljesítmény hello szám, ahányszor egy ügyfél tooacquire egy új jogkivonatot kell csökkentésével érhető el.

### <a name="refresh-tokens"></a>Frissítési jogkivonatok
Létrejöttekor egy hozzáférési jogkivonat tooaccess védett erőforrás, hello ügyfél megkapja egy frissítési jogkivonat és a hozzáférési tokent. hello frissítési jogkivonat használt tooobtain új hozzáférési/frissítési jogkivonat párok esetén hello aktuális hozzáférési jogkivonat lejár. A frissítési token felhasználói és az ügyfél kötött tooa kombinációját. A frissítési jogkivonat visszavonhatók, és hello token érvényességi be van jelölve, minden alkalommal, amikor hello jogkivonat.

Fontos fontos toomake bizalmas és nyilvános ügyfelek közötti különbséget. További információ a különböző típusú ügyfelek: [RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.1).

#### <a name="token-lifetimes-with-confidential-client-refresh-tokens"></a>A frissítési jogkivonatokat bizalmas ügyfél token élettartama
Bizalmas ügyfelek olyan alkalmazások, amelyek tudja biztonságosan tárolni ügyfél jelszót (titkos). Hogy a rendszer tudja, hogy érkeznek hello ügyfélalkalmazástól, és nem egy rosszindulatú szereplő. Például a webes alkalmazás nem bizalmas ügyfél, mert egy ügyfélkulcsot hello webkiszolgálón képes tárolni. Nem lesz közzétéve. Mivel ezek a folyamatok biztonságosabb, hello alapértelmezett élettartama a frissítési jogkivonatokat kiállított toothese adatfolyamok van `until-revoked`házirend használatával nem módosítható és nem lehet visszavonni a önkéntes jelszó alaphelyzetbe állítását.

#### <a name="token-lifetimes-with-public-client-refresh-tokens"></a>A nyilvános ügyfél frissítési jogkivonatokat token élettartama

Nyilvános ügyfelek nem tudja biztonságosan tárolni (titkos) ügyfél jelszót. Például az iOS vagy Android-alkalmazások nem takarják hello erőforrás tulajdonosa, a titkos kulcs, egy nyilvános ügyfél akkor tekinthető. Között állítható be házirendek erőforrások tooprevent frissítési jogkivonatokat nyilvános ügyfelek régebbi, mint a megadott időszak hozzáférési/frissítési jogkivonat párokat beszerezni. (toodo a, használjon hello frissítési jogkivonat maximális tétlenség ideje tulajdonság.) Adott időszakban, mely hello frissítési jogkivonatokat túl már nem fogad házirendek tooset is használhatja. (toodo a, használjon hello frissítési jogkivonat maximális életkora tulajdonság.) Beállíthatja, hogy mikor és milyen gyakran hello felhasználó szükség tooreenter hitelesítő adatait, éppen csendes hitelesíteni, egy nyilvános ügyfélalkalmazás használata helyett a frissítési token toocontrol hello élettartamát.

### <a name="id-tokens"></a>Azonosító-jogkivonatokat
Azonosító-jogkivonatokat átadott toowebsites és natív ügyfelek. Azonosító-jogkivonatokat a felhasználói profil adatait tartalmazzák. Egy azonosító token felhasználói és az ügyfél egyedi kombinációja kötött tooa. Azonosító-jogkivonatokat érvényesek a lejártáig. Általában egy webes alkalmazás megfelel a felhasználó által hello alkalmazás toohello élettartama hello azonosító jogkivonat a munkamenetek élettartamát kiadott hello felhasználó. Egy azonosító token toocontrol milyen gyakran hello webalkalmazás hello alkalmazás munkamenetet, és milyen gyakran szükség van hello felhasználói toobe hitelesíteni az Azure ad-val (csendes vagy interaktív) lejár hello élettartama módosíthatja.

### <a name="single-sign-on-session-tokens"></a>Egyszeri bejelentkezés munkamenetet jogkivonatok
Amikor egy felhasználó hitelesíti az Azure AD-val, és kiválasztja a hello **bejelentkezve szeretnék maradni** jelölőnégyzetet, egy egyszeri bejelentkezés (SSO) munkamenet és hello felhasználói böngésző és az Azure AD. hello SSO jogkivonat, a cookie-k hello formájában ehhez a munkamenethez tartalmazza. Vegye figyelembe, hogy hello SSO munkameneti jogkivonat nem kötött tooa adott erőforrás/ügyfélalkalmazást. Egyszeri bejelentkezési munkamenet jogkivonatok visszavonhatók, és azok érvényességi be van jelölve, minden alkalommal, amikor a segítségükkel.

Az Azure AD használ egyszeri bejelentkezési munkamenet jogkivonatok kétféle: állandó, és nem állandó. Állandó munkamenet jogkivonatok állandó cookie-k hello böngésző tárolódnak. Nem állandó munkamenet jogkivonatok munkamenet cookie-kat, tárolódnak. (A munkamenet cookie-k megsemmisülnek hello böngésző bezárásakor.)

Nem állandó munkamenet jogkivonatok élettartama pedig 24 óra. Állandó jogkivonatok élettartama 180 nap. Egy egyszeri bejelentkezési munkamenet jogkivonat az érvényességi idején belül bármikor hello érvényességi időszaka másik 24 óra vagy 180 nap elteltével hello token típusától függően. Ha egy egyszeri bejelentkezési munkamenet-azonosító nem használják az érvényességi idején belül, lejárt, és már nem elfogadható minősül.

Egy házirend tooset hello időt, miután hello első munkameneti jogkivonat adta ki, melyik hello túl munkameneti jogkivonat nem elfogadható használhatja. (toodo a, használjon hello munkamenet Token maximális életkora tulajdonság.) Beállíthatja, hogy mikor és milyen gyakran egy felhasználó-e a szükséges tooreenter helyett hitelesítő adatokat, csendes hitelesített, a webes alkalmazás használata a munkamenet token toocontrol hello élettartama.

### <a name="token-lifetime-policy-properties"></a>A jogkivonatok élettartama házirend tulajdonságai
A jogkivonatok élettartama házirend a csoportházirend-objektum, amely tartalmazza a jogkivonatok élettartama szabályok típusú. Használjon hello tulajdonságainak hello házirend toocontrol megadott jogkivonat élettartamát. Ha nincs házirend van beállítva, a hello rendszer hello alapértelmezett élettartamértékénél érvénybe lépteti.

### <a name="configurable-token-lifetime-properties"></a>A jogkivonatok élettartama konfigurálható tulajdonságai
| Tulajdonság | Házirend tulajdonság karakterlánc | Érint | Alapértelmezett | Minimális | Maximális |
| --- | --- | --- | --- | --- | --- |
| Hozzáférés a jogkivonatok élettartama |AccessTokenLifetime |Hozzáférési jogkivonatok, azonosító-jogkivonatokat, egy SAML2 jogkivonatok |1 óra |10 perc |1 nap |
| Frissítse a Token maximális tétlenség ideje |MaxInactiveTime |Frissítési jogkivonatok |14 napja |10 perc |90 nap |
| Single-tényező frissítési jogkivonat maximális életkora |MaxAgeSingleFactor |Frissítési jogkivonatok (a felhasználók számára) |Amíg visszavonása |10 perc |Amíg visszavont<sup>1</sup> |
| Multi-factor Authentication frissítési jogkivonat maximális életkora |MaxAgeMultiFactor |Frissítési jogkivonatok (a felhasználók számára) |Amíg visszavonása |10 perc |Amíg visszavont<sup>1</sup> |
| Single-tényező munkamenet Token maximális életkora |MaxAgeSessionSingleFactor<sup>2</sup> |Munkamenet-jogkivonatok (állandó és nem állandó) |Amíg visszavonása |10 perc |Amíg visszavont<sup>1</sup> |
| Multi-factor Authentication munkamenet Token maximális életkora |MaxAgeSessionMultiFactor<sup>3</sup> |Munkamenet-jogkivonatok (állandó és nem állandó) |Amíg visszavonása |10 perc |Amíg visszavont<sup>1</sup> |

* <sup>1</sup>365 nap hello maximális hosszúságát, hogy ezek az attribútumok állítható be.
* <sup>2</sup>Ha **MaxAgeSessionSingleFactor** nincs beállítva, ezt az értéket veszi hello **MaxAgeSingleFactor** érték. Ha sem a paraméter értéke, a hello tulajdonság hello alapértelmezett érték (amíg visszavont) vesz igénybe.
* <sup>3</sup>Ha **MaxAgeSessionMultiFactor** nincs beállítva, ezt az értéket veszi hello **MaxAgeMultiFactor** érték. Ha sem a paraméter értéke, a hello tulajdonság hello alapértelmezett érték (amíg visszavont) vesz igénybe.

### <a name="exceptions"></a>Kivételek
| Tulajdonság | Érint | Alapértelmezett |
| --- | --- | --- |
| Frissítse a Token maximális életkora (összevont rendelkező felhasználók számára megfelelő visszavonási információ kiadott<sup>1</sup>) |Frissítési jogkivonatok (összevont rendelkező felhasználók számára megfelelő visszavonási információ kiadott<sup>1</sup>) |12 óra |
| Frissítse a Token maximális tétlenség ideje (bizalmas ügyfelek ki) |Frissítési jogkivonatok (bizalmas ügyfelek ki) |90 nap |
| Frissítse a Token maximális életkora (bizalmas ügyfelek ki) |Frissítési jogkivonatok (bizalmas ügyfelek ki) |Amíg visszavonása |

* <sup>1</sup>közé tartozik a felhasználók, akik nem rendelkeznek elegendő visszavonási információ rendelkező külső felhasználók hello "LastPasswordChangeTimestamp" attribútum szinkronizálva. Ezek a felhasználók kapnak e rövid maximális életkora, mivel aad-ben nem tooverify toorevoke jogkivonatokat kötött tooan régi hitelesítőadat-(például egy jelszót, amely megváltozott), és újra a gyakrabban tooensure ellenőriznie kell, hogy a felhasználó hello és társított tokenek továbbra is: a jó állandó. a felhasználói élmény, a bérlői rendszergazdák győződjön meg arról, hogy azok szinkronizálását végzi hello "LastPasswordChangeTimestamp" attribútumot (Ez állítható hello user objektum Powershell használatával vagy az AADSync keresztül) tooimprove.

### <a name="policy-evaluation-and-prioritization"></a>Házirend kiértékelése és rangsorolási
Hozzon létre, és hozzárendelheti a jogkivonatok élettartama házirend tooa az adott alkalmazást, a tooyour szervezetét és a tooservice rendszerbiztonsági tagok. Több házirendet is vonatkozhatnak tooa az adott alkalmazást. hello a jogkivonatok élettartama házirend érvénybe ezeket a szabályokat követi:

* Ha egy házirend explicit módon hozzárendelt toohello egyszerű, a rendszer érvényesíti.
* Ha nincs házirend explicit módon hozzárendelt toohello egyszerű, egy explicit módon hozzárendelt toohello szülő szervezeti hello szolgáltatás rendszerbiztonsági tag lesz alkalmazva,
* Ha nincs házirend explicit módon hozzárendelt toohello egyszerű vagy toohello szervezet, hello toohello alkalmazás hozzárendelése lesz alkalmazva,
* Ha nincs házirend társított toohello szolgáltatás egyszerű, hello szervezet vagy hello alkalmazásobjektum, hello alapértelmezett értékek van kényszerítve. (Lásd a táblázat hello [konfigurálható a jogkivonatok élettartama tulajdonságok](#configurable-token-lifetime-properties).)

Alkalmazás és szolgáltatás egyszerű objektumok hello kapcsolatát kapcsolatos további információkért lásd: [alkalmazás és szolgáltatás egyszerű objektumok az Azure Active Directoryban](active-directory-application-objects.md).

A token érvényességi hello jogkivonat hello időben értékeli. hello legmagasabb prioritású hello alkalmazás, amely használatban van a hello házirend érvénybe lép.

> [!NOTE]
> Íme egy példa.
>
> A felhasználó szeretne-e tooaccess két webes alkalmazások: A webalkalmazás és a webes alkalmazás a b kiszolgálóra.
> 
> Tényezői:
> * Mindkét webalkalmazások vannak hello megegyezik a szülő szervezet.
> * A munkamenet biztonságijogkivonat maximális életkora nyolc óra jogkivonat élettartamát házirend 1 hello szülő szervezeti alapértelmezés állítja be.
> * A webalkalmazás rendszeres használható webalkalmazás, és nem csatolt tooany házirendek.
> * Webes alkalmazás B szigorúan bizalmas folyamatok szolgál. Az egyszerű szolgáltatásnév a csatolt tooToken élettartama házirend 2, amelynek a munkamenet biztonságijogkivonat maximális életkora 30 perc.
>
> 12:00 PM hello felhasználó elindítja a új böngésző-munkamenet és záma tooaccess webes alkalmazás újraindul hello felhasználói átirányított tooAzure AD, és a toosign kéri. Ezzel létrehoz egy cookie-t, amely rendelkezik a munkameneti jogkivonat hello böngészőben. hello felhasználó az átirányított hátsó tooWeb Application egy, amely lehetővé teszi, hogy a felhasználó tooaccess hello hello alkalmazás azonosítója tokenhez.
>
> A 12:15 előtti a hello felhasználó megpróbál tooaccess webes alkalmazás b hello böngésző átirányítások tooAzure AD, amely észleli az hello munkamenetcookie-t. Webes alkalmazás B szolgáltatás egyszerű csatolt tooToken élettartama házirend 2, de azt is hello szülő szervezet, az alapértelmezett jogkivonat élettartamát házirend 1 részét. Jogkivonat élettartamát házirend 2 lép érvénybe, mert házirendek csatolt tooservice rendszerbiztonsági tagok magasabb prioritású, mint a szervezet alapértelmezett házirendeket. hello munkameneti jogkivonat kiadástól hello belül elmúlt 30 percben, így érvényes tekinthető. hello felhasználói Azonosítót jogkivonatban hozzáférést biztosít az átirányított hátsó tooWeb alkalmazás B.
>
> 1:00 PM hello záma tooaccess webes alkalmazás újraindul hello felhasználóhoz átirányított tooAzure AD. Web Application egy nem csatolt tooany házirendek, de mivel alapértelmezett jogkivonat élettartamát házirend 1 rendelkező szervezeteknél, a házirend érvénybe lép. a rendszer észlelt hello munkamenetcookie-t, korábban kiadott eredetileg belül hello utolsó nyolc óra. hello felhasználói beavatkozás nélkül átirányított hátsó tooWeb Application egy új azonosítója jogkivonatok. hello felhasználó nincs szükség tooauthenticate.
>
> Azonnal a későbbiekben hello felhasználó akkor próbálja meg a tooaccess webes alkalmazás b hello felhasználó az átirányított tooAzure AD. Mivel előtt, a jogkivonat élettartamát házirend 2 lép érvénybe. Hello token ki több, mint harminc perccel ezelőtt történt, mert hello felhasználó-e a kért tooreenter a bejelentkezési hitelesítő adataikat. Egy vadonatúj munkamenet és azonosító jogkivonatot adják ki. hello felhasználói ezután hozzáférhetnek a webes alkalmazás a b kiszolgálóra.
>
>

## <a name="configurable-policy-property-details"></a>Konfigurálható szabályzatot tulajdonság részletei
### <a name="access-token-lifetime"></a>Hozzáférés a jogkivonatok élettartama
**Karakterlánc:** AccessTokenLifetime

**Érinti:** hozzáférési jogkivonatok, azonosító-jogkivonatokat

**Összefoglalás:** Ez az irányelv szabályozza, hogy mennyi ideig hozzáférési és azonosító-jogkivonatokat ehhez az erőforráshoz érvényesek. Hello hozzáférési jogkivonatok élettartama tulajdonság csökkentése csökkenti a hello kockázatot hozzáférési jogkivonat vagy azonosító token idő hosszú időn keresztül a rosszindulatú szereplő használják. (Ezeket a jogkivonatokat nem vonható vissza.) hello kompromisszum oka az, hogy a teljesítményt hátrányosan érinti, hello gyakrabban cseréje toobe lehet.

### <a name="refresh-token-max-inactive-time"></a>Frissítse a Token maximális tétlenség ideje
**Karakterlánc:** MaxInactiveTime

**Érinti:** frissítési jogkivonatok

**Összefoglalás:** Ez az irányelv szabályozza, hogy hány éves frissítési jogkivonat lehet, mielőtt egy ügyfél többé ne használhassa az tooretrieve hozzáférési/frissítési jogkivonat párokat tooaccess ehhez az erőforráshoz megkísérlése során. Egy új frissítési jogkivonat általában ad vissza egy frissítési jogkivonat használata esetén, mert ez a házirend megakadályozza Ha hello-ügyfél megkísérli tooaccess során hello hello jelenlegi frissítési jogkivonat használatával bármilyen olyan erőforrás a megadott időn belül.

Ezzel a házirend-kényszeríti a felhasználók, akik a saját ügyfél tooreauthenticate tooretrieve egy új frissítési jogkivonat nem használtak.

alacsonyabb értékeket tooa hello egyetlen tényezős Token maximális életkora és hello többtényezős Token maximális frissítése életkora tulajdonságok hello frissítési jogkivonat maximális tétlenség ideje tulajdonság kell beállítani.

### <a name="single-factor-refresh-token-max-age"></a>Single-tényező frissítési jogkivonat maximális életkora
**Karakterlánc:** MaxAgeSingleFactor

**Érinti:** frissítési jogkivonatok

**Összefoglalás:** a házirend vezérlőelemek milyen hosszú a felhasználó használhatja a frissítési token tooget új hozzáférési/frissítési jogkivonat pár azok legutóbbi hitelesítése után sikeresen csak tényező használatával. Miután egy felhasználó hitelesíti, és egy új frissítési jogkivonat kap, a hello felhasználó használhatja-e a hello frissítési jogkivonat-folyamat a megadott hello időszakának idő. (Ez igaz, amíg a hello jelenlegi frissítési jogkivonat nem vonták vissza, és nem hagyják hosszabb hello inaktív ideje nem használt.) Ezen a ponton a hello felhasználói kényszerített tooreauthenticate tooreceive egy új frissítési jogkivonat.

Felhasználók tooauthenticate hello maximális életkora csökkentése gyakrabban kényszeríti. Mert minősül, hogy kevésbé biztonságos, mint a multi-factor authentication hitelesítést, azt javasoljuk, hogy állítsa a tooa tulajdonságérték, amely egyenlő tooor hello többtényezős Token maximális frissítése életkora tulajdonság-nál kisebb.

### <a name="multi-factor-refresh-token-max-age"></a>Multi-factor Authentication frissítési jogkivonat maximális életkora
**Karakterlánc:** MaxAgeMultiFactor

**Érinti:** frissítési jogkivonatok

**Összefoglalás:** a házirend vezérlőelemek milyen hosszú a felhasználó használhatja a frissítési token tooget új hozzáférési/frissítési jogkivonat pár azok legutóbbi hitelesítése után sikeresen több tényezővel használatával. Miután egy felhasználó hitelesíti, és egy új frissítési jogkivonat kap, a hello felhasználó használhatja-e a hello frissítési jogkivonat-folyamat a megadott hello időszakának idő. (Ez igaz, amíg a hello jelenlegi frissítési jogkivonat nem vonták vissza, és nem hosszabb hello inaktív ideje nem használt.) Ezen a ponton felhasználók kényszerítve vannak-e tooreauthenticate tooreceive egy új frissítési jogkivonat.

Felhasználók tooauthenticate hello maximális életkora csökkentése gyakrabban kényszeríti. Mert minősül, hogy kevésbé biztonságos, mint a multi-factor authentication hitelesítést, azt javasoljuk, hogy állítsa a tooa tulajdonságérték, amely nagyobb, mint hello egyetlen tényezős Token maximális frissítése életkora tulajdonság egyenlő tooor.

### <a name="single-factor-session-token-max-age"></a>Single-tényező munkamenet Token maximális életkora
**Karakterlánc:** MaxAgeSessionSingleFactor

**Érinti:** munkamenet jogkivonatok (állandó és nem állandó)

**Összefoglalás:** a házirend vezérlőelemek mennyi ideig egy felhasználó használhatja-e a munkamenet biztonságijogkivonat tooget egy új Azonosítót és egy munkamenet-token azok legutóbbi hitelesítése után sikeresen csak tényező használatával. Miután egy felhasználó hitelesíti, és kap egy új munkamenet-azonosító, a hello felhasználó használhatja-e a hello munkamenet jogkivonat-folyamat a megadott hello időszakának idő. (Ez igaz, amíg a hello aktuális munkameneti jogkivonat nem vonták vissza és nem járt le.) Miután hello megadott időn belül, hello felhasználó kényszerített tooreauthenticate tooreceive egy új munkamenet-azonosító.

Felhasználók tooauthenticate hello maximális életkora csökkentése gyakrabban kényszeríti. Mert minősül, hogy kevésbé biztonságos, mint a multi-factor authentication hitelesítést, azt javasoljuk, hogy állítsa a tooa tulajdonságérték, amely egyenlő hello-nál kisebb tooor többtényezős munkamenet Token maximális életkora tulajdonság.

### <a name="multi-factor-session-token-max-age"></a>Multi-factor Authentication munkamenet Token maximális életkora
**Karakterlánc:** MaxAgeSessionMultiFactor

**Érinti:** munkamenet jogkivonatok (állandó és nem állandó)

**Összefoglalás:** a házirend vezérlőelemek mennyi ideig egy felhasználó használhatja-e a munkamenet egy új Azonosítót és egy munkamenet lexikális elem után token tooget hello legutóbbi azok sikeresen hitelesített több tényezővel használatával. Miután egy felhasználó hitelesíti, és kap egy új munkamenet-azonosító, a hello felhasználó használhatja-e a hello munkamenet jogkivonat-folyamat a megadott hello időszakának idő. (Ez igaz, amíg a hello aktuális munkameneti jogkivonat nem vonták vissza és nem járt le.) Miután hello megadott időn belül, hello felhasználó kényszerített tooreauthenticate tooreceive egy új munkamenet-azonosító.

Felhasználók tooauthenticate hello maximális életkora csökkentése gyakrabban kényszeríti. Mert minősül, hogy kevésbé biztonságos, mint a multi-factor authentication hitelesítést, azt javasoljuk, hogy állítsa a tooa tulajdonságérték, amely nagyobb, mint hello egyetlen tényezős munkamenet Token maximális életkora tulajdonság egyenlő tooor.

## <a name="example-token-lifetime-policies"></a>A házirendek például a jogkivonatok élettartama
Számos forgatókönyv is előfordulhatnak, ha akkor hozhat létre és kezelhet jogkivonat élettartamát az alkalmazásokkal, szolgáltatásnevekről és a teljes szervezet Azure AD-ben. Ez a szakasz azt bízná néhány olyan gyakori házirend forgatókönyvet, amelyik segíthet a maximálásához új szabályokat is:

* A jogkivonatok élettartama
* Inaktív token maximális idő
* Token maximális életkora

Hello példák megtudhatja hogyan:

* A szervezetek alapértelmezett házirend kezelése
* Webalkalmazás-bejelentkezés házirend létrehozása
* Hozzon létre egy házirendet, amely behívja a webes API-k natív alkalmazások
* Egy speciális házirend kezelése

### <a name="prerequisites"></a>Előfeltételek
A következő példák hello akkor létrehozása, frissítése, hivatkozás és törölje az alkalmazásokkal, szolgáltatásnevekről és a teljes szervezet házirendeket. Ha új tooAzure AD, azt javasoljuk, hogy Ismerkedjen meg kapcsolatos [hogyan tooget az Azure AD bérlői](active-directory-howto-tenant.md) ezekben a példákban folytatása előtt.  

elindult, tooget hello a következő lépéseket:

1. Töltse le a hello legújabb [az Azure AD PowerShell modul nyilvános előzetes verzió](https://www.powershellgallery.com/packages/AzureADPreview).
2. Futtassa a hello `Connect` parancs toosign a tooyour az Azure AD rendszergazdai fiókot. Futtassa ezt a parancsot minden alkalommal, amikor új munkamenet indításához.

    ```PowerShell
    Connect-AzureAD -Confirm
    ```

3. a parancs, amelyet a szervezetében, futtassa a következő hello létrehozott összes házirend toosee. Futtassa ezt a parancsot a következő forgatókönyvek hello legtöbb műveletek után. Emellett segítséget nyújt hello hello parancs futtatása ** ** a házirendek.

    ```PowerShell
    Get-AzureADPolicy
    ```

### <a name="example-manage-an-organizations-default-policy"></a>Példa: Egy szervezet alapértelmezett házirend kezelése
Ebben a példában létrehozhat olyan házirendet, amely lehetővé teszi, hogy a felhasználók ritkábban jelentkezzen be a teljes szervezetben. toodo, egyetlen tényezős frissítési jogkivonatokat, a szervezetben alkalmazott a jogkivonatok élettartama házirend létrehozása. hello házirend a szervezet és a tooeach egyszerű szolgáltatást, amely még nem rendelkezik a beállított házirend alkalmazott tooevery alkalmazást.

1. A jogkivonatok élettartama házirend létrehozása.

    1.  Beállítása Single-tényező frissítési jogkivonat hello túl "amíg visszavont." hello jogkivonat nem jár le, amíg hozzáférést visszavonva. Hozza létre a következő házirend-definíció hello:

        ```PowerShell
        @('{
            "TokenLifetimePolicy":
            {
                "Version":1,
                "MaxAgeSingleFactor":"until-revoked"
            }
        }')
        ```

    2.  toocreate hello házirend, futtassa a következő parancs hello:

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    3.  toosee az új házirend, és tooget hello házirend **ObjectId**- ben futtassa hello következő parancsot:

        ```PowerShell
        Get-AzureADPolicy
        ```

2. Hello házirend frissítése.

    Dönthet úgy, hogy hello első házirend, ebben a példában nincs-e a szolgáltatás megköveteli a szigorú. tooset a Single-tényező frissítési jogkivonat tooexpire két nap múlva futtassa hello a következő parancsot:

    ```PowerShell
    Set-AzureADPolicy -Id <ObjectId FROM GET COMMAND> -DisplayName "OrganizationDefaultPolicyUpdatedScenario" -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"2.00:00:00"}}')
    ```

### <a name="example-create-a-policy-for-web-sign-in"></a>Példa: A webalkalmazás-bejelentkezés házirend létrehozása

Ebben a példában a webalkalmazáson belüli gyakrabban felhasználók tooauthenticate igénylő házirend létrehozása. Ezzel a házirend-hello élettartama hello hozzáférési/azonosító-jogkivonatokat és a webalkalmazás többtényezős munkamenet token toohello szolgáltatásnevet maximális életkora hello beállítása.

1. A jogkivonatok élettartama házirend létrehozása.

    Ezt a házirendet, a webes bejelentkezéskor a hello hozzáférés vagy-azonosító a jogkivonatok élettartama és hello maximális egyetlen tényezős jogkivonat élettartamát tootwo üzemidő állítja be.

    1.  toocreate hello házirend, futtassa a parancsot:

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"AccessTokenLifetime":"02:00:00","MaxAgeSessionSingleFactor":"02:00:00"}}') -DisplayName "WebPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    2.  toosee az új házirend, és tooget hello házirend **ObjectId**- ben futtassa hello következő parancsot:

        ```PowerShell
        Get-AzureADPolicy
        ```

2.  Rendelje hozzá a hello házirend tooyour egyszerű szolgáltatást. Szükség tooget hello **ObjectId** a szolgáltatás rendszerbiztonsági tag. 

    1.  toosee lekérheti a szervezet szolgáltatásnevekről, [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity). Vagy a [Azure AD Graph Explorer](https://graphexplorer.cloudapp.net/), jelentkezzen be tooyour Azure AD-fiókot.

    2.  Ha rendelkezik hello **ObjectId** a szolgáltatás egyszerű futnia hello a következő parancsot:

        ```PowerShell
        Add-AzureADServicePrincipalPolicy -Id <ObjectId of hello ServicePrincipal> -RefObjectId <ObjectId of hello Policy>
        ```


### <a name="example-create-a-policy-for-a-native-app-that-calls-a-web-api"></a>Példa: Egy natív alkalmazás, amely behívja a webes API-k házirend létrehozása
Ebben a példában hozzon létre egy házirendet, amely kevesebb felhasználói tooauthenticate gyakran igényel. hello házirend is hosszabb lesz a hello időn egy felhasználó lehet inaktív, mielőtt hello felhasználói ismét hitelesítenie kell magát. hello házirend alkalmazott toohello webes API. Ha natív alkalmazás hello hello webes API-erőforrásként kér, a házirend érvényben van.

1. A jogkivonatok élettartama házirend létrehozása.

    1.  webes API-k, futtassa a következő parancs hello szigorú házirend toocreate:

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"30.00:00:00","MaxAgeMultiFactor":"until-revoked","MaxAgeSingleFactor":"180.00:00:00"}}') -DisplayName "WebApiDefaultPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    2.  toosee az új házirend, és tooget hello házirend **ObjectId**- ben futtassa hello következő parancsot:

        ```PowerShell
        Get-AzureADPolicy
        ```

2. Rendelje hozzá a hello házirend tooyour webes API-t. Szükség tooget hello **ObjectId** az alkalmazás. hello legjobb módja toofind az alkalmazás **ObjectId** toouse hello van [Azure-portálon](https://portal.azure.com/).

   Ha rendelkezik hello **ObjectId** az alkalmazás, futtassa a következő parancs hello:

        ```PowerShell
        Add-AzureADApplicationPolicy -Id <ObjectId of hello Application> -RefObjectId <ObjectId of hello Policy>
        ```


### <a name="example-manage-an-advanced-policy"></a>Példa: Egy speciális házirend kezelése
Ebben a példában néhány házirendek, hello prioritás rendszer működése toolearn hoz létre. Azt is is megtudhatja, hogyan toomanage alkalmazott tooseveral objektumok több házirendeket.

1. A jogkivonatok élettartama házirend létrehozása.

    1.  egy szervezet alapértelmezett házirendet, amely beállítja a hello egyetlen tényezős frissítési jogkivonat élettartamát too30 nap, futtassa a következő parancs hello toocreate:

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"30.00:00:00"}}') -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    2.  toosee az új házirend, és tooget hello házirend **ObjectId**- ben futtassa hello következő parancsot:

        ```PowerShell
        Get-AzureADPolicy
        ```

2. Rendelje hozzá a hello házirend tooa egyszerű szolgáltatást.

    Most hogy egy házirendet toohello egész szervezetre érvényes. Előfordulhat, hogy egy adott szolgáltatás egyszerű toopreserve a 30 napos házirend szeretne, de módosítása hello szervezet alapértelmezett házirend toohello felső korlátja "amíg visszavont."

    1.  toosee lekérheti a szervezet szolgáltatásnevekről, [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity). Vagy a [Azure AD Graph Explorer](https://graphexplorer.cloudapp.net/), jelentkezzen be az Azure AD-fiókot.

    2.  Ha rendelkezik hello **ObjectId** a szolgáltatás egyszerű futnia hello a következő parancsot:

            ```PowerShell
            Add-AzureADServicePrincipalPolicy -Id <ObjectId of hello ServicePrincipal> -RefObjectId <ObjectId of hello Policy>
            ```
        
3. Set hello `IsOrganizationDefault` toofalse jelzőt:

    ```PowerShell
    Set-AzureADPolicy -Id <ObjectId of Policy> -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $false
    ```

4. Hozzon létre egy új szervezeti alapértelmezett házirendet:

    ```PowerShell
    New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "ComplexPolicyScenarioTwo" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
    ```

    Most már rendelkezik hello eredeti házirend csatolt tooyour szolgáltatásnevet, és a szervezet alapértelmezett házirend be van állítva az új házirend hello. Fontos, hogy a házirendeket alkalmaztak tooservice rendszerbiztonsági tagok elsőbbséget élveznek a szervezet alapértelmezett házirendek tooremember.

## <a name="cmdlet-reference"></a>A parancsmagok leírása

### <a name="manage-policies"></a>Házirendek kezelése

A következő parancsmagok toomanage házirendek hello is használhatja.

#### <a name="new-azureadpolicy"></a>Új AzureADPolicy

Létrehoz egy új házirendet.

```PowerShell
New-AzureADPolicy -Definition <Array of Rules> -DisplayName <Name of Policy> -IsOrganizationDefault <boolean> -Type <Policy Type>
```

| Paraméterek | Leírás | Példa |
| --- | --- | --- |
| <code>&#8209;Definition</code> |A tömb stringified JSON, amely tartalmazza az összes hello-szabályzat előírásainak. | `-Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"20:00:00"}}')` |
| <code>&#8209;DisplayName</code> |Hello házirendnév karakterlánc. |`-DisplayName "MyTokenPolicy"` |
| <code>&#8209;IsOrganizationDefault</code> |Igaz értéke esetén hello házirend beállítása hello szervezete alapértelmezett házirendje. Ha hamis, nincs hatása. |`-IsOrganizationDefault $true` |
| <code>&#8209;Type</code> |Házirend típusa. A jogkivonat élettartamát mindig használja az "TokenLifetimePolicy." | `-Type "TokenLifetimePolicy"` |
| <code>&#8209;AlternativeIdentifier</code>[Választható] |Beállítja a hello házirend alternatív azonosítója. |`-AlternativeIdentifier "myAltId"` |

</br></br>

#### <a name="get-azureadpolicy"></a>Get-AzureADPolicy
Minden Azure AD-házirendek vagy a megadott házirend lekérése.

```PowerShell
Get-AzureADPolicy
```

| Paraméterek | Leírás | Példa |
| --- | --- | --- |
| <code>&#8209;Id</code>[Választható] |**Objektumazonosító (Id)** kívánt hello házirend. |`-Id <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadpolicyappliedobject"></a>Get-AzureADPolicyAppliedObject
Lekéri az összes alkalmazás és szolgáltatás rendszerbiztonsági tagok csatolt tooa házirend.

```PowerShell
Get-AzureADPolicyAppliedObject -Id <ObjectId of Policy>
```

| Paraméterek | Leírás | Példa |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Objektumazonosító (Id)** kívánt hello házirend. |`-Id <ObjectId of Policy>` |

</br></br>

#### <a name="set-azureadpolicy"></a>Set-AzureADPolicy
Frissíti a meglévő szabályzatokat.

```PowerShell
Set-AzureADPolicy -Id <ObjectId of Policy> -DisplayName <string>
```

| Paraméterek | Leírás | Példa |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Objektumazonosító (Id)** kívánt hello házirend. |`-Id <ObjectId of Policy>` |
| <code>&#8209;DisplayName</code> |Hello házirendnév karakterlánc. |`-DisplayName "MyTokenPolicy"` |
| <code>&#8209;Definition</code>[Választható] |A tömb stringified JSON, amely tartalmazza az összes hello-szabályzat előírásainak. |`-Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"20:00:00"}}')` |
| <code>&#8209;IsOrganizationDefault</code>[Választható] |Igaz értéke esetén hello házirend beállítása hello szervezete alapértelmezett házirendje. Ha hamis, nincs hatása. |`-IsOrganizationDefault $true` |
| <code>&#8209;Type</code>[Választható] |Házirend típusa. A jogkivonat élettartamát mindig használja az "TokenLifetimePolicy." |`-Type "TokenLifetimePolicy"` |
| <code>&#8209;AlternativeIdentifier</code>[Választható] |Beállítja a hello házirend alternatív azonosítója. |`-AlternativeIdentifier "myAltId"` |

</br></br>

#### <a name="remove-azureadpolicy"></a>Remove-AzureADPolicy
A megadott házirend törlése hello.

```PowerShell
 Remove-AzureADPolicy -Id <ObjectId of Policy>
```

| Paraméterek | Leírás | Példa |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Objektumazonosító (Id)** kívánt hello házirend. | `-Id <ObjectId of Policy>` |

</br></br>

### <a name="application-policies"></a>Alkalmazás-házirendek
A következő parancsmagok az alkalmazás-házirendek hello is használhatja.</br></br>

#### <a name="add-azureadapplicationpolicy"></a>Adja hozzá AzureADApplicationPolicy
Hivatkozások hello megadott házirend tooan alkalmazás.

```PowerShell
Add-AzureADApplicationPolicy -Id <ObjectId of Application> -RefObjectId <ObjectId of Policy>
```

| Paraméterek | Leírás | Példa |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Objektumazonosító (Id)** hello alkalmazás. | `-Id <ObjectId of Application>` |
| <code>&#8209;RefObjectId</code> |**ObjectId** hello házirend. | `-RefObjectId <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadapplicationpolicy"></a>Get-AzureADApplicationPolicy
Lekérdezi a hozzárendelt tooan alkalmazás hello házirend.

```PowerShell
Get-AzureADApplicationPolicy -Id <ObjectId of Application>
```

| Paraméterek | Leírás | Példa |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Objektumazonosító (Id)** hello alkalmazás. | `-Id <ObjectId of Application>` |

</br></br>

#### <a name="remove-azureadapplicationpolicy"></a>Remove-AzureADApplicationPolicy
Egy alkalmazás-házirend eltávolítása.

```PowerShell
Remove-AzureADApplicationPolicy -Id <ObjectId of Application> -PolicyId <ObjectId of Policy>
```

| Paraméterek | Leírás | Példa |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Objektumazonosító (Id)** hello alkalmazás. | `-Id <ObjectId of Application>` |
| <code>&#8209;PolicyId</code> |**ObjectId** hello házirend. | `-PolicyId <ObjectId of Policy>` |

</br></br>

### <a name="service-principal-policies"></a>Szolgáltatás egyszerű házirendjei
A következő parancsmagokat a szolgáltatás egyszerű házirendek hello is használhatja.

#### <a name="add-azureadserviceprincipalpolicy"></a>Adja hozzá AzureADServicePrincipalPolicy
Hivatkozások hello megadott házirend tooa egyszerű szolgáltatást.

```PowerShell
Add-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal> -RefObjectId <ObjectId of Policy>
```

| Paraméterek | Leírás | Példa |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Objektumazonosító (Id)** hello alkalmazás. | `-Id <ObjectId of Application>` |
| <code>&#8209;RefObjectId</code> |**ObjectId** hello házirend. | `-RefObjectId <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadserviceprincipalpolicy"></a>Get-AzureADServicePrincipalPolicy
Lekérdezi a házirend csatolt toohello megadott szolgáltatásnevet.

```PowerShell
Get-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal>
```

| Paraméterek | Leírás | Példa |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Objektumazonosító (Id)** hello alkalmazás. | `-Id <ObjectId of Application>` |

</br></br>

#### <a name="remove-azureadserviceprincipalpolicy"></a>Remove-AzureADServicePrincipalPolicy
Hello házirend eltávolítása hello megadott szolgáltatásnevet.

```PowerShell
Remove-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal>  -PolicyId <ObjectId of Policy>
```

| Paraméterek | Leírás | Példa |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Objektumazonosító (Id)** hello alkalmazás. | `-Id <ObjectId of Application>` |
| <code>&#8209;PolicyId</code> |**ObjectId** hello házirend. | `-PolicyId <ObjectId of Policy>` |
