---
title: "Részletesen: Azure AD SSPR |} Microsoft Docs"
description: "Az Azure AD az önkiszolgáló jelszó-átállítási részletes bemutatója"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 618c5908-5bf6-4f0d-bf88-5168dfb28a88
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: c177192bbe69d179a25d174b06a0813ec28e2615
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="self-service-password-reset-in-azure-ad-deep-dive"></a>Az önkiszolgáló jelszó-változtatási az Azure AD részletes bemutatója

Hogyan működik az önkiszolgáló jelszó-Változtatási? Mi ez a lehetőség jelent hello felületen? Továbbra is olvasása toofind további információk az Azure AD az önkiszolgáló jelszó alaphelyzetbe állítása.

## <a name="how-does-hello-password-reset-portal-work"></a>Hogyan alaphelyzetbe állítja a jelszót hello a munkahelyi portál

Amikor a felhasználók megnyitják a jelszó-változtatási portál toohello, akkor a munkafolyamat van kezdődött el toodetermine:

   * Hogyan lehet honosított hello lap?
   * Az érvényes hello felhasználói fiók?
   * Milyen szervezet hello felhasználó tartozik?
   * Ha hello jelszó felügyelt?
   * Az hello felhasználói licencelt toouse hello szolgáltatás?


Olvassa végig toolearn hello logika hello jelszó alaphelyzetbe állítása oldal mögött kapcsolatos alábbi hello lépéseket.

1. Felhasználó kattint hello nem tudja elérni a fiókot hivatkozásra, vagy túl közvetlenül kerül[https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com).
2. Alapú hello böngésző területi hello a felhasználói élmény hello megfelelő nyelven lett megjelenítve. hello jelszó alaphelyzetbe állítása élmény van honosítva hello azonos nyelveket támogatja az Office 365.
3. Felhasználó beírja egy felhasználói azonosítót, és továbbítja a captcha.
4. Az Azure AD ellenőrzi, hogy hello felhasználó képes toouse-e a szolgáltatás hello következő tevékenységek végrehajtásával:
   * Ellenőrzi, hogy hello felhasználó rendelkezik ezzel a funkcióval és az Azure AD-licenccel.
     * Hello felhasználó nem rendelkezik ezzel a funkcióval vagy egy licenccel, hogy van-e a hello felhasználói toocontact a rendszergazda tooreset kéri a jelszót.
   * Ellenőrzi, hogy hello felhasználó rendelkezik-e a rendszergazda nyilatkozatnak fiókjuk definiált hello jobb oldali kérdés adatok.
     * Ha a házirend csak egy ellenőrző igényel, majd biztosítható, hogy hello felhasználó rendelkezik-e legalább az egyik hello kihívást hello rendszergazdai házirend által engedélyezett definiált hello megfelelő adatok.
       * Ha hello felhasználó nincs konfigurálva, akkor a felhasználó hello van elkeverni toocontact a rendszergazda tooreset a jelszavát.
     * Ha hello házirend két kihívást igényel, majd biztosítható, hogy hello felhasználó rendelkezik-e legalább két hello kihívást hello rendszergazdai házirend által engedélyezett definiált hello megfelelő adatok.
       * Ha hello felhasználó nincs konfigurálva, akkor azt a felhasználói hello elkeverni toocontact a rendszergazda tooreset van a jelszavát.
   * Ellenőrzi, hogy hello jelszó (összevont vagy Jelszókivonat szinkronizálása) helyszínen felügyelt.
     * Ha visszaírási van telepítve, és a helyszínen felügyelt hello jelszó, hello felhasználói tooproceed tooauthenticate engedélyezett, és a jelszó alaphelyzetbe állítása.
     * Ha visszaírási nincs telepítve a helyszínen felügyelt hello felhasználó jelszavát, majd hello felhasználó toocontact a rendszergazda tooreset kéri a jelszót.
5. Ha megállapította, hogy hello felhasználó toosuccessfully jelszó visszaállítása, majd a rendszer végigvezeti hello a felhasználót hello visszaállítási folyamat.

## <a name="authentication-methods"></a>Hitelesítési módszerek

Ha az önkiszolgáló jelszó alaphelyzetbe állítása (SSPR) engedélyezve van, ki kell választania legalább az alábbi hitelesítési módszerek beállítások hello. Erősen ajánlott legalább két hitelesítési módszer kiválasztása, hogy a felhasználók több beleszólása van.

* E-mail cím
* Mobiltelefon
* Irodai telefon
* Biztonsági kérdések

### <a name="what-fields-are-used-in-hello-directory-for-authentication-data"></a>Milyen mezők használ a hello directory hitelesítési adatok

* Irodai telefon tooOffice Phone felel meg.
    * Felhasználók maguk meg kell határozni egy rendszergazda ebben a mezőben nem tooset
* Mobiltelefon felel meg a hitelesítéshez megadott telefonját (nem nyilvánosan láthatóvá) vagy a mobiltelefonjára (nyilvánosan láthatóvá) tooeither
    * hello szolgáltatást először megkeresi a hitelesítéshez megadott telefonját, majd visszavált tooMobile telefon, ha nem jelent-e
* Másodlagos E-mail-cím megfelel-e tooeither hitelesítési E-mail (nem nyilvánosan láthatóvá) vagy a másodlagos E-mail
    * hello szolgáltatás hitelesítési E-mail először megkeresi, az e-mailek hátsó tooAlternate sikertelen

Alapértelmezés szerint csak hello jellemzői irodai telefon és mobiltelefonra szinkronizálja a hitelesítési adatok a helyi címtárban lévő tooyour felhőalapú címtárban.

A felhasználók csak a jelszavát, ha telepítve adatok találhatók az hello hitelesítési módszereket, hogy hello rendszergazda engedélyezte a visszaállítási és igényel.

Ha a felhasználó nem szeretné, hogy a mobiltelefon száma toobe hello directory látható, de szeretné még toouse például azt a jelszó alaphelyzetbe állítása, a rendszergazdák nem kell tölteni azt hello könyvtárban, és hello felhasználói majd fel kell töltenie a **hitelesítéshez megadott telefonját**  keresztül hello attribútum [jelszó-változtatási regisztrációs portálra](http://aka.ms/ssprsetup). A rendszergazdák láthatják ezt az információt hello profil, de nem máshol lesz közzétéve. Ha Azure-rendszergazdai fiók regisztrálja a hitelesítéshez használni kívánt telefonszámot, hello mobiltelefon mezőbe fel van töltve, és látható.

### <a name="number-of-authentication-methods-required"></a>Több hitelesítési módszer szükséges

Ez a beállítás határozza meg, hogy hello minimális hello elérhető hitelesítési módszerek egy felhasználó tooreset keresztül kell haladnia vagy feloldása a jelszavát, és 1 vagy 2 tooeither állítható be.

Felhasználók maguk dönthetik toosupply további hitelesítési módszerek Ha hello rendszergazda engedélyezve vannak.

Ha a felhasználó nem rendelkezik a szükséges minimális hello módszerekkel regisztrált, láthatják, amelyek vezeti őket egy rendszergazda tooreset toorequest hibalap a jelszavát.

### <a name="how-secure-are-my-security-questions"></a>Hogy mennyire vannak biztonságban vannak a biztonsági kérdések

Biztonsági kérdések használata, ha ajánlott azokat használja egy másik módszerrel módon el más módszernél kevésbé biztonságos, mivel a felhasználók némelyike előfordulhat, hogy ismeri a hello válaszok tooanother felhasználók kérdések.

> [!NOTE] 
> Biztonsági kérdések a user objektum hello könyvtárban közvetlenül és biztonságos helyen tárolja, és is csak válaszolni felhasználók regisztrálás során. Nincs semmilyen módszer a egy rendszergazda tooread vagy módosíthat egy felhasználói kérdések és válaszok.
>

### <a name="security-question-localization"></a>Biztonsági kérdés honosítása

Hajtsa végre az összes előre definiált kérdésekre az Office 365 nyelvek hello felhasználó a böngésző nyelve alapján teljes készletét hello honosítva vannak.

* Melyik városban ismerkedett meg az első házastársával/párjával?
* Melyik városban találkozak először a szülei?
* Melyik városban lakik a legközelebbi rokona?
* Az édesapja melyik városban született?
* Melyik városban volt az első munkahelye?
* Melyik városban született az édesanyja?
* Hol töltötte 1999 szilveszterét?
* Mi az, hogy magas volt a kedvenc tanárának vezetékneve hello * iskolai?
* Hello keresztneve alkalmazott felsőoktatási intézménybe toobut nem vették fel?
* Mi az a hello neve hello helyet, ahol az első lakodalmát tartották?
* Édesapja második keresztneve?
* Mi a kedvenc étele?
* Anyai nagyapja vezeték- és keresztneve?
* Mi az édesanyja második keresztneve?
* Mi az a legidősebb testvérének születési éve és hónapja? (pl. 1985. November)
* Mi a legidősebb testvérének második keresztneve?
* Apai nagyapja vezeték- és keresztneve?
* Mi a legfiatalabb tesvérének második keresztneve?
* Melyik iskolában végezte el a hatodik osztályt?
* Mi volt első hello, és a gyerekkori legjobb barátjának keresztneve?
* Mi volt első hello- és keresztneve első társának?
* Mi volt a kedvenc általános iskolai tanárának vezetékneve hello?
* Mi volt az első autója vagy motorkerékpárja márkája és típusa hello?
* Mi volt a hello hello első iskolájának neve?
* Mi volt a hello neve hello kórháznak, ahol született?
* Mi volt első gyerekkori otthonának utcaneve hello hello neve?
* Mi volt a kedvenc gyerekkori hőse hello neve?
* Mi volt a kedvenc plüssállatának neve hello?
* Mi volt az első háziállatának neve hello?
* Mi volt a gyerekkori beceneve?
* Mi volt a kedvenc sportja a középiskolában?
* Mi volt az első munkahelye?
* Mi volt hello utolsó négy a gyerekkori telefonszámának számjegye?
* Gyerekkorában, mi szeretett volna toobe Ha majd felnő?
* Ki hello leghíresebb ember, valaha is találkozott?

### <a name="custom-security-questions"></a>Egyéni biztonsági kérdések

Egyéni biztonsági kérdések nem a különböző területi beállításokhoz honosított. Minden egyéni kérdések jelennek meg hello ugyanezen a nyelven bevitel hello rendszergazdai felhasználói felületén akkor is, ha hello felhasználó a böngésző nyelve nem egyezik. Ha honosított kérdése van szüksége, használja az előre megadott hello kérdéseket.

hello egy egyéni biztonsági kérdés hossza legfeljebb 200 karakter lehet.

### <a name="security-question-requirements"></a>Biztonsági kérdés követelmények

* Minimális válasz karakteres korlátot érték 3 karakter
* A válasz maximális karakteres korlátot 40 karakter
* Felhasználók esetleg nem fogadja a hívást hello azonos kérdés egynél többször
* Felhasználók nem rendelkezhetnek hello azonos választ, mint egy kérdést toomore
* Lehet, hogy bármely karakterkészlet használt toodefine kérdések és válaszok, beleértve a Unicode-karaktereket
* hello száma megadott kérdéseket kell nagyobb, mint vagy egyenlő kérdések szükséges tooregister toohello száma

## <a name="registration"></a>Regisztráció

### <a name="require-users-tooregister-when-signing-in"></a>Felhasználók tooregister kérése, amikor a bejelentkezés

A beállítás engedélyezése a felhasználó, aki engedélyezve van a jelszó visszaállítása toocomplete hello a jelszóátállítás regisztrációját, ha azok bejelentkezés használatával, például az alábbiakhoz hajtsa végre az Azure AD toosign tooapplications szükséges:

* Office 365
* Azure Portal
* Hozzáférési panel
* Összevont alkalmazásokhoz
* Egyéni alkalmazások az Azure AD használatával

E funkció letiltása továbbra is lehetővé teszi felhasználók toomanually register kapcsolattartási adatait ellátogatva [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) vagy hello kattintva **regisztrálhatnak a jelszóváltoztatásra** hello alatt profil lap hello hozzáférési panel.

> [!NOTE]
> A felhasználók hello jelszó-visszaállítási portál esetben elvetheti a Mégse gombra kattintva, vagy bezárja az ablakot hello, de a rendszer kéri, minden alkalommal, amikor azok bejelentkezéshez, amíg a regisztrációs művelet befejeződik.
>

### <a name="number-of-days-before-users-are-asked-tooreconfirm-their-authentication-information"></a>Hány nap elteltével a felhasználó információt kér a rendszer tooreconfirm a hitelesítés

Ez a beállítás azt határozza meg, és a hitelesítési adatokat reconfirming közötti hello időszak, és csak érhető el, ha engedélyezi a hello **felhasználók tooregister kérése, amikor bejelentkezik** lehetőséget.

Érvényes értékek: 0-730 tehát ne kérdezzen rá felhasználók tooreconfirm hitelesítési adataikat 0 nap

## <a name="notifications"></a>Értesítések

### <a name="notify-users-on-password-resets"></a>Értesítse a felhasználókat új jelszó kérésekor?

Tooyes állítja a beállítást, ha a jelszó alaphelyzetbe állításával hello felhasználó kap egy e-mailt, amely értesíti őket, hogy a jelszó megváltozott keresztül hello önkiszolgáló jelszó-Változtatási portál tootheir elsődleges és másodlagos e-mail-címet a fájl az Azure ad-ben. A visszaállítási értesítést senki más nem esemény.

### <a name="notify-all-admins-when-other-admins-reset-their-passwords"></a>Az összes rendszergazda értesítése, ha más rendszergazdák a jelszavak alaphelyzetbe állítása

Ha ez a beállítás tooyes, majd **minden rendszergazda** kap egy e-mailek tootheir elsődleges e-mail címét, a fájl az Azure ad-ben, amely értesíti őket, hogy egy másik rendszergazda megváltozott a jelszavát, használja az önkiszolgáló jelszó-Változtatási.

Példa: Nincsenek négy rendszergazdák környezetben. "A" rendszergazda az önkiszolgáló jelszó-Változtatási használatával jelszavának alaphelyzetbe állítása. A rendszergazdák B, C és D azokat, azonban ez a riasztás e-mailt kapni.

## <a name="on-premises-integration"></a>Helyszíni integráció

Ha telepítette, konfigurálva, és engedélyezve van az Azure AD Connect akkor helyszíni Integrációk további beállításokat.

### <a name="write-back-passwords-tooyour-on-premises-directory"></a>Jelszavak tooyour helyszíni címtár visszaírni

Szabályozza-e a jelszóvisszaírás engedélyezve van a könyvtárhoz, és ha visszaírási, hello helyszíni visszaírási szolgáltatás hello állapotát jelzi. Ez akkor hasznos, ha azt szeretné, tootemporarily letiltása hello jelszóvisszaírás nélkül újrakonfigurálása az Azure AD Connect.

* Ha hello kapcsoló nem set tooyes visszaírás engedélyezve van, és összevont és a jelszó szinkronizálva kivonatoló felhasználók fognak tudni tooreset jelszavukat.
* Ha hello kapcsoló nem set toono visszaírás le van tiltva, és összevont és a jelszó szinkronizálva kivonatoló felhasználók nem lesz képes tooreset jelszavukat.

### <a name="allow-users-toounlock-accounts-without-resetting-their-password"></a>Engedélyezze a felhasználók toounlock fiókok nélkül a jelszó alaphelyzetbe állításával

Jelöli meg a felhasználók, akik a Microsoft hello jelszó-változtatási portál adott hello beállítás toounlock nélkül a jelszó alaphelyzetbe állításával fiókok a helyszíni Active Directoryban kell-e. Alapértelmezés szerint az Azure AD lesz mindig fiókok zárolásának feloldása új jelszó létrehozását végrehajtása során, ez a beállítás lehetővé teszi, hogy tooseparate e két művelet. 

* Ha túl állítsa a "yes", akkor a felhasználókat a rendszer adott hello beállítás tooreset a jelszavát, majd hello fiókot, illetve toounlock hello jelszó alaphelyzetbe állításával nélkül is feloldhatják.
* Ha értéke túl "no", akkor a felhasználók csak akkor tudja tooperform egy kombinált jelszó alaphelyzetbe állítása és a fiók a feloldási műveletet.

## <a name="network-requirements"></a>A hálózatra vonatkozó követelmények

### <a name="firewall-rules"></a>Tűzfalszabályok

[A Microsoft Office URL-címei és IP-címek listája](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)

Az Azure AD Connect verzió 1.1.443.0 és felett, hogy a kimenő HTTPS hozzáférés toohello következőkre lesz szüksége
* passwordreset.microsoftonline.com
* servicebus.Windows.NET

Részletesebb hozzáférni, Microsoft Azure Datacenter IP-címtartományok, amely minden szerdán frissítése és kerüljenek a hatás hello következő frissített hello listája megtalálható hétfő [Itt](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="idle-connection-timeouts"></a>Kapcsolat üresjárati időtúllépés

hello Azure AD Connect eszköz rendszeres ping-üzenetek Keep Alive hívás/csomagok küldését tooServiceBus végpontok tooensure, hogy hello kapcsolatok életben maradjon küld. Kell hello eszköz észleli, hogy túl sok a kapcsolat vannak megállítása folyamatban., automatikusan megnöveli ping-üzenetek toohello végpont hello gyakoriságát. hello legalacsonyabb "pingelje intervallumok" elhagyta toois 1 ping 60 másodpercenként, azonban ajánlott, hogy proxyk/tűzfalak engedélyezik-e legalább 2-3 percig inaktív kapcsolatok toopersist. * A régebbi verziókat javasoljuk, hogy akár négy percnél.

## <a name="active-directory-permissions"></a>Active Directory-engedélyek

hello hello Azure AD Connect segédprogram megadott fiók jelszó alaphelyzetbe állítása, jelszó módosítása, lockoutTime írási engedéllyel, és engedélyekkel kell rendelkeznie írni a pwdLastSet, bővített jogosultságokkal vagy hello legfelső szintű objektumának a **tartományonként** az adott erdőben **vagy** hello a felhasználónak a szervezeti egységek sspr hatókörében toobe kívánja.

Ha nem biztos a fenti milyen fiók hello hivatkozik, nyissa meg a hello Azure Active Directory Connect konfigurációs felhasználói Felületet, és kattintson a hello nézet aktuális konfigurációs beállítást. a "Címtárak szinkronizálása" felsorolt tooadd engedély toois kell hello fiók

Ezek az engedélyek beállítása lehetővé teszi, hogy hello MA-szolgáltatásfiókja minden erdő toomanage jelszavak felhasználói fiókok nevében az erdőben lévő. **Ha nem rendeli tooassign ezeket az engedélyeket, majd, annak ellenére, hogy a visszaírási megfelelően konfigurálva toobe jelenik meg, a felhasználók felmerülő problémákat, toomanage hello felhőből helyszíni jelszavukat megkísérlése során.**

> [!NOTE]
> Tooan óráig vagy tovább a könyvtárban ezen engedélyek tooreplicate tooall objektumok eltarthat.
>

tooset hello a megfelelő engedélyeket jelszó visszaírási toooccur mentése

1. Nyisson meg egy hello megfelelő tartományi rendszergazdai engedélyekkel rendelkező fiók Active Directory – felhasználók és számítógépek
2. Hello Nézet menü ellenőrizze, hogy a speciális funkciók be van kapcsolva
3. A hello bal oldali panelen kattintson a jobb gombbal a hello tartomány gyökeréhez hello képviselő hello objektum, és válassza a Tulajdonságok
    * Kattintson a hello Biztonság lap
    * Majd a Speciális gombra.
4. Hello engedélyek lapján kattintson a Hozzáadás gombra.
5. Válasszon hello fiókot, hogy engedélyeket alkalmazásra kerülnek túl (az Azure AD Connect telepítés)
6. Hello hozzárendelési toodrop beállítómező válassza leszármazott felhasználó objektumai
7. Az engedélyek jelölőnégyzeteket hello hello következő
    * Unexpire jelszó
    * Jelszó alaphelyzetbe állítása
    * Jelszó módosítása
    * LockoutTime írása
    * PwdLastSet írása
8. Kattintson az alkalmaz/OK tooapply keresztül, és zárja be minden megnyitott párbeszédpanelen.

## <a name="how-does-password-reset-work-for-b2b-users"></a>Hogyan alaphelyzetbe állítja a jelszót B2B felhasználók számára?
Bármilyen B2B konfigurációjának teljes mértékben támogatottak a jelszó alaphelyzetbe állítása és módosítása.  Olvassa el alább hello három explicit B2B adódó jelszó-visszaállítás által támogatott.

1. **Egy partner szervezet munkatársa, a meglévő Azure AD-bérlő felhasználóit** – Ha Ön együttműködve hello szervezet rendelkezik egy meglévő Azure AD-bérlő azt **tiszteletben tartják a bérlőre engedélyezve vannak függetlenül jelszó alaphelyzetbe állítása házirendjei**. A jelszó alaphelyzetbe állítása toowork, hello partner szervezet csak igények toomake meg arról, hogy az Azure AD SSPR engedélyezve van, amely nem kell külön fizetni az Office 365-ügyfelek, és engedélyezhető a következő hello lépései a [jelszókezelés első lépései ](https://azure.microsoft.com/documentation/articles/active-directory-passwords-getting-started/#enable-users-to-reset-or-change-their-aad-passwords) útmutató.
2. **Felhasználókra iratkozott fel használatával [önkiszolgáló regisztrációs](active-directory-self-service-signup.md)**  – Ha Ön együttműködve hello szervezet használt hello [önkiszolgáló regisztrációs](active-directory-self-service-signup.md) be egy bérlői tooget funkciót, azt hogy azok a alaphelyzetbe hello e-mail regisztrálják azokat.
3. **B2B felhasználók** -hello új használatával létrehozott új B2B felhasználók [Azure AD B2B képességek](active-directory-b2b-what-is-azure-ad-b2b.md) is képes tooreset hello e-mail hello meghívása során regisztrálják azokat a jelszavukat.

tootest e, lépjen toohttp://passwordreset.microsoftonline.com ezen partner felhasználók egyike. Mindaddig, amíg egy másodlagos e-mail vagy a megadott hitelesítési e-mail rendelkeznek, jelszó-átállítási akkor működik megfelelően.

## <a name="next-steps"></a>Következő lépések

a következő hivatkozások hello adja meg a jelszó alaphelyzetbe állítása, az Azure AD használatával kapcsolatos további információk

* [**Gyors üzembe helyezés**](active-directory-passwords-getting-started.md) – Percek alatt üzembe helyezheti az Azure AD önkiszolgáló jelszókezelőjét. 
* [**Licencelés**](active-directory-passwords-licensing.md) – Az Azure AD licencelésének konfigurálása.
* [**Adatok** ](active-directory-passwords-data.md) - szükséges hello adatok megismeréséhez, és hogyan használja fel azokat a jelszókezelés
* [**Bevezetés** ](active-directory-passwords-best-practices.md) -megtervezése és telepítése az önkiszolgáló jelszó-Változtatási tooyour felhasználók hello útmutatást itt talál
* [**Szabályzat**](active-directory-passwords-policy.md) – Megismerheti és beállíthatja az Azure AD jelszószabályzatait.
* [**Jelszóvisszaíró**](active-directory-passwords-writeback.md) – Megtudhatja, hogyan használhatja a jelszóvisszaírót a helyszíni címtárával.
* [**Testre szabhatja** ](active-directory-passwords-customize.md) -testreszabása, önkiszolgáló jelszó-Változtatási élményt a vállalata hello hello megjelenését és működését.
* [**Jelentéskészítés**](active-directory-passwords-reporting.md) – Megtudhatja, mikor és hol érik el a felhasználói az SSPR funkcióit.
* [**Gyakori kérdések**](active-directory-passwords-faq.md) – Hogyan? Hogy miért? Mi? Hová? Ki? Mikor? -Válaszok mindig kívánta tooask tooquestions
* [**Hibaelhárítás** ](active-directory-passwords-troubleshoot.md) -megtudhatja, hogyan tooresolve közös állít ki, hogy az önkiszolgáló jelszó-Változtatási látható

