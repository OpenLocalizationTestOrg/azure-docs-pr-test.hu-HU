---
title: "A jelszóvisszaíró az Azure AD SSPR |} Microsoft Docs"
description: "Az Azure AD és az Azure AD Connect toowriteback jelszavak tooon helyszíni könyvtár"
services: active-directory
keywords: "Az Active directory-jelszókezelés, jelszókezelés, az Azure AD self service jelszó alaphelyzetbe állítása"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 6a1dd964a51b4f3b5b0be303b722ab6deb4a6110
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="password-writeback-overview"></a>Jelszó visszaírási áttekintése

A jelszóvisszaírás lehetővé teszi, hogy az Azure AD tooconfigure toowrite jelszavak biztonsági tooyou a helyszíni Active Directory. Eltávolít hello kell tooset fel, és egy összetett helyszíni önkiszolgáló jelszó-visszaállítási megoldás kezelése, és biztosít felhőalapú kényelmesen a felhasználók tooreset helyszíni jelszavukat bárhol legyenek is. A jelszóvisszaírás összetevő [Azure Active Directory Connect](./connect/active-directory-aadconnect.md) , hogy engedélyezve van, és támogatás az aktuális előfizetők által használt [Azure Active Directory-kiadások](active-directory-editions.md).

A jelszóvisszaírás biztosít a következő funkciók hello

* **Késleltetés visszajelzés nulla** -jelszóvisszaírás egy aszinkron művelet. A felhasználók arról azonnal a jelszavát nem felelt meg a házirend, vagy nem tudja toobe alaphelyzetbe állítása vagy bármilyen okból megváltozott.
* **Támogatja az AD FS vagy más összevonási technológiákat használó felhasználók számára jelszavak alaphelyzetbe állítását** -jelszóvisszaírás, mindaddig, amíg hello összevont a felhasználói fiókok szinkronizálása az Azure AD-bérlő be képes toomanage a helyszíni AD jelszavak hello felhőből.
* **Támogatja a felhasználók a jelszavak alaphelyzetbe állítását [Jelszókivonat-szinkronizálás](./connect/active-directory-aadconnectsync-implement-password-synchronization.md)**  – Ha hello jelszó alaphelyzetbe állítása szolgáltatás észleli, hogy a szinkronizált felhasználói fiók engedélyezve van a Jelszókivonat-szinkronizálás, a rendszer visszaállítja, mind a fiók a helyszíni és felhőalapú egyidejűleg jelszó.
* **Támogatja a jelszavak módosítása a hello hozzáférési panel és az Office 365** – Ha összevont vagy a jelszó szinkronizálva felhasználók származnak toochange lejárt vagy nem lejárt jelszavukat, azt írni ezeket jelszavak vissza tooyour helyi AD-környezet.
* **Támogatja vissza jelszavakat írása, amikor egy rendszergazda állíthatja őket az Azure-portálon hello** – amikor egy rendszergazda visszaállítja a jelszó a hello [Azure-portálon](https://portal.azure.com), ha a felhasználónak össze van vonva, vagy a jelszó szinkronizálva lesznek állítva hello Üdvözöljük a rendszergazdákat jelszót a helyi Active Directory, valamint a választja ki. Ez jelenleg nem támogatott a hello Office felügyeleti portálon.
* **Kikényszeríti a helyszíni AD jelszóházirendek** – Ha a felhasználó alaphelyzetbe állítja a jelszót, azt győződjön meg arról, hogy megfelel-e a helyszíni AD-házirend előtt véglegesítése azt toothat könyvtár. Ez magában foglalja az előzmények, összetettségét, kor, jelszószűrők és egyéb jelszó korlátozásokat a helyi AD-ben definiált.
* **Nincs szükség a bejövő tűzfalszabályokat** -jelszóvisszaíró az Azure Service Bus relay használja, mint a mögöttes kommunikációs csatornát, ami azt jelenti, hogy nincs tooopen bejövő portra a tűzfalon ahhoz, hogy ez a szolgáltatás toowork.
* **Védett csoportok a helyszíni Active Directoryban található felhasználói fiókok esetében nem támogatott** – védett csoportokkal kapcsolatos további információért tekintse meg az [védett fiókok és csoportok Active Directory](https://technet.microsoft.com/library/dn535499.aspx).

## <a name="how-password-writeback-works"></a>A jelszóvisszaírás működése

Ha összevont vagy a jelszó kivonatát a szinkronizált felhasználói tooreset származik, vagy jelszavukat hello felhőben hello következő történik:

1. Toosee ellenőrizzük, milyen típusú jelszó hello felhasználó rendelkezik.
    * Ha látható a helyszínen felügyelt hello jelszó
        * A Microsoft ellenőrizze, hogy hello visszaírási szolgáltatás működik-e és fut, ha, azt, hogy hello felhasználó folytatni
        * Hello visszaírási szolgáltatás nem működik-e, ha a Microsoft hello felhasználó értesítése, hogy a jelszavát most nem állítható alaphelyzetbe
2. A következő hello felhasználói hello megfelelő hitelesítési kapui továbbítja, és hello alaphelyzetbe állítása jelszóképernyő eléri.
3. hello felhasználó kiválaszt egy új jelszót, és megerősíti, hogy azt.
4. Esetén a Küldés gombra kattintva azt titkosítást hello titkosítatlan szöveges jelszót a hello visszaírási telepítési folyamat során létrehozott szimmetrikus kulcsot.
5. Hello jelszó titkosítása, után azt foglalja bele a hasznos adatok között, amely egy HTTPS csatorna tooyour bérlői-specifikus service bus-továbbító (beállított azt is meg hello visszaírási beállítási folyamata során) keresztül küldi. A továbbító védi, hogy csak a helyszíni telepítési ismeri véletlenszerűen létrehozott jelszót.
6. Ha üdvözlőüzenetére elérte a service bus, hello jelszó alaphelyzetbe állítása végpont automatikusan felébred és látja, hogy rendelkezik-e a visszaállítási kérelem függőben.
7. hello szolgáltatás majd keresi hello felhasználó az adott felhő horgonyattribútum hello segítségével. Az a keresési toosucceed:

    * hello felhasználói objektum már léteznie kell hello AD kapcsolódási térbe
    * hello felhasználói objektum csatolt toohello megfelelő MV-objektum kell lennie.
    * hello felhasználói objektum csatolt toohello megfelelő AAD connector objektumnak kell lennie.
    * hello mutató hivatkozást AD összekötő objektum tooMV rendelkeznie kell hello szinkronizálási szabály `Microsoft.InfromADUserAccountEnabled.xxx` hello hivatkozásra. <br> <br>
    Hello hívás hello felhőből érkezik, amikor a hello szinkronizálási motor által használt hello cloudAnchor attribútum toolook hello AAD connector terület objektumát, hello hivatkozás hátsó toohello MV-objektum a következő és hello hátsó toohello AD objektum majd követi. Mivel lehet több Active Directory-objektumok (Többerdős) az ugyanahhoz a felhasználóhoz, hello szinkronizálási motor támaszkodik hello hello `Microsoft.InfromADUserAccountEnabled.xxx` hivatkozás toopick hello javítsa ki egyet.

    > [!Note]
    > A logika miatt az Azure AD Connect a PDC-emulátor hello a jelszó visszaírási toowork képes toocommunicate kell lennie. Ha ilyen manuálisan kell tooenable, az Azure AD Connect toohello PDC-emulátor kapcsolódhatnak kattintson a jobb gombbal a hello **tulajdonságok** hello Active Directory szinkronizálási összekötő, jelölje be **konfigurálása a címtárparticiók**. Ott, keresse meg hello **tartományvezérlő kapcsolat beállításai** szakaszt, és című hello jelölőnégyzetet **csak használja az elsődleges tartományvezérlő**. Akkor is, ha hello elsődleges tartományvezérlő nem a PDC-emulátor, az Azure AD Connect tooconnect toohello PDC a jelszóvisszaírás kísérli meg.

8. Miután hello felhasználói fiók található, azt közvetlenül a megfelelő Active Directory-erdőben hello tooreset hello jelszó történt kísérlet.
9. Ha hello jelszó set művelet sikeres, a Microsoft hello felhasználó értesítése a jelszó megváltozott.
    > [!NOTE]
    > Hello esetben hello felhasználói jelszó esetén szinkronizált tooAzure AD, hogy hello helyszíni jelszóházirend gyengébb mint hello felhő jelszóházirend esély van jelszó-szinkronizálás használatával. Ebben az esetben azt még kényszerítése bármilyen hello a helyi házirend, és ehelyett lehetővé teszi jelszó kivonatoló szinkronizálási toosynchronize hello kivonatát, hogy a jelszó. Ez biztosítja, hogy a helyi házirend van érvényben hello felhőben, attól függetlenül használatakor a jelszó-szinkronizálás vagy az összevonási tooprovide egyszeri bejelentkezés.

10. Ha hello jelszó művelet sikertelen lesz, azt térjen vissza hello hiba toohello felhasználói, és hogy azok próbálja meg újból.
    * hello művelet miatt hello következő sikertelen lehet
        * hello szolgáltatás le lett
        * hello jelszót állított nem felelt meg a szervezet házirendek
        * Nem található a helyi AD hello hello felhasználó

    Egy adott üzenet számos ezekben az esetekben, és mit tehet hello felhasználó értesítése tudunk tooresolve hello probléma.

## <a name="configuring-password-writeback"></a>A jelszóvisszaírás konfigurálása

Azt javasoljuk, hogy használja-e az automatikus frissítési szolgáltatása hello [az Azure AD Connect](./connect/active-directory-aadconnect-get-started-express.md) Ha azt szeretné, hogy toouse jelszóvisszaírás.

A DirSync és az Azure AD Sync már nem támogatott azt jelenti, hogy a jelszó visszaírási hello cikk [frissíthet a DirSync és Azure AD Sync](connect/active-directory-aadconnect-dirsync-deprecated.md) további információt toohelp az átállás rendelkezik.

hello az alábbi lépések azt feltételezik már konfigurálta az Azure AD Connect használatával hello környezetében [Express](./connect/active-directory-aadconnect-get-started-express.md) vagy [egyéni](./connect/active-directory-aadconnect-get-started-custom.md) beállításait.

1. tooconfigure és engedélyezze a jelszóvisszaírás tooyour az Azure AD Connect-kiszolgáló-e jelentkezni, és indítsa el a hello **az Azure AD Connect** konfigurációs varázsló.
2. Hello üdvözlőképernyőn kattintson **konfigurálása**.
3. A további hello feladatok képernyőn kattintson **testre szabhatja a szinkronizálási beállítások** majd **következő**.
4. Hello csatlakozás tooAzure AD képernyőn adjon meg egy globális rendszergazdai hitelesítő adatok, és válassza a **következő**.
5. Hello csatlakoztassa a címtárakat és a tartományt, és szervezeti egység kérelemszűrés megvizsgálja választhatja **következő**.
6. A hello választható funkciókat felsoroló képernyőjén jelölőnégyzetet hello mellett túl**jelszóvisszaírás** kattintson **következő**.
   ![Az Azure AD Connectben a jelszóvisszaírás engedélyezése][Writeback]
7. A hello készen tooconfigure képernyőn kattintson **konfigurálása** és hello folyamat toocomplete várja.
8. Amikor megjelenik a konfiguráció befejezését követően kattinthat **Kilépés**

## <a name="licensing-requirements-for-password-writeback"></a>A jelszóvisszaírás licencelési követelményei

Licencelési, lásd: kapcsolatos információk hello témakör [jelszóvisszaírás szükséges licencek](active-directory-passwords-licensing.md#licenses-required-for-password-writeback) vagy hello a következő helyek

* [Az Azure Active Directory árképzési hely](https://azure.microsoft.com/pricing/details/active-directory/)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Biztonságos hatékony Enterprise](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

### <a name="on-premises-authentication-modes-supported-for-password-writeback"></a>Helyszíni hitelesítési módot támogat. a jelszó visszaírásához.

A jelszóvisszaírás működik, a következő felhasználói jelszó típusai hello:

* **Csak felhőalapú felhasználók**: a jelszóvisszaírás nem vonatkozik ebben a helyzetben, mert nem a helyszíni jelszó
* **Jelszó-szinkronizált felhasználók**: a jelszóvisszaírás támogatott
* **Összevont felhasználók**: a jelszóvisszaírás támogatott
* **Áteresztő hitelesítés felhasználók**: a jelszóvisszaírás támogatott

### <a name="user-and-admin-operations-supported-for-password-writeback"></a>A jelszóvisszaíró támogatja felhasználói és felügyeleti műveletek

Jelszavak írt vissza a következő helyzetekben összes hello:

* **Végfelhasználói támogatott műveletek**
  * A végfelhasználói önkiszolgáló önkéntes jelszó művelet módosítása
  * A végfelhasználói önkiszolgáló kényszerített jelszó műveletet (például a jelszó lejárati idejét)
  * A végfelhasználói önkiszolgáló jelszó-átállítási hello származó [jelszó-változtatási portál](https://passwordreset.microsoftonline.com)
* **Rendszergazda támogatott műveletek**
  * A rendszergazda önkiszolgáló önkéntes jelszó művelet módosítása
  * Minden rendszergazda kényszerített önkiszolgáló jelszó műveletet (például a jelszó lejárati idejét)
  * A rendszergazda az önkiszolgáló jelszó-átállítási hello származó [jelszó-változtatási portál](https://passwordreset.microsoftonline.com)
  * A rendszergazda által kezdeményezett végfelhasználói jelszó-változtatási a hello [a klasszikus Azure portálon](https://manage.windowsazure.com)
  * A rendszergazda által kezdeményezett végfelhasználói jelszó-változtatási a hello [Azure-portálon](https://portal.azure.com)

### <a name="user-and-admin-operations-not-supported-for-password-writeback"></a>Nem támogatott a jelszóvisszaírás felhasználói és felügyeleti műveletek

A jelszavak **nem** vissza a következő helyzetekben hello bármelyikét nyelven írt:

* **Nem támogatott a végfelhasználói műveletek**
  * A végfelhasználók a saját PowerShell v1, v2 és hello Azure AD Graph API jelszó alaphelyzetbe állításával
* **Nem támogatott rendszergazdai műveletek**
  * A rendszergazda által kezdeményezett végfelhasználói jelszó-változtatási a hello [Office felügyeleti portálon](https://portal.office.com)
  * A rendszergazda által kezdeményezett végfelhasználói jelszó-változtatási PowerShell v1, v2 vagy hello Azure AD Graph API

Dolgozunk ennek tooremove ezekkel a korlátozásokkal, amíg nincs egy meghatározott ütemterv még is osztjuk.

## <a name="password-writeback-security-model"></a>Jelszó visszaírási biztonsági modell

A jelszóvisszaírás egy olyan rendkívül biztonságos szolgáltatás.  az adatok védelméről is gondoskodik tooensure, engedélyezzük a 4-rétegzett biztonsági modell, amely az alábbiakban olvasható.

* **Bérlői-specifikus service-bus-továbbító**
  * Hello szolgáltatást beállítani, beállítjuk a bérlő-specifikus service bus-továbbító, amely Microsoft soha nem rendelkezik hozzáféréssel véletlenszerűen létrehozott erős jelszóval védett.
* **Zárolt, dokumentumtitkosítási erős jelszó-titkosítási kulcs**
  * Hello service bus relay létrehozása után létrehozhatunk egy szimmetrikus kulcs erős tooencrypt hello jelszót használunk, ismét hello hálózaton keresztül. Ez a kulcs él, csak a vállalat titkos tároló hello felhőben, ami erősen zárolva, és naplózva, csakúgy, mint bármely jelszó hello könyvtárban.
* **Iparági szabványos TLS**
  1. A jelszó alaphelyzetbe állítása vagy módosításakor a felhőben hello a művelethez, azt hello titkosítatlan szöveges jelszó igénybe vehet, és a nyilvános kulcs titkosításához.
  2. A Microsoft helyezzen, amely egy HTTPS üzenet, amely a Microsoft SSL Tanúsítványos tooyour service bus relay használatával egy titkosított csatornán keresztül zajlik.
  3. Miután hello üzenet érkezik a Service Bus, a helyszíni ügynök felébred fel, és ezzel hitelesíti tooService busz használatával hello erős jelszót, amely korábban jött létre.
  4. A helyszíni ügynök szerzi be titkosított üdvözlőüzenetére, visszafejti hello azt a létrehozott titkos kulccsal.
  5. A helyszíni ügynök majd megpróbál tooset hello jelszó hello AD DS SetPassword API használatával.
     * Ez a lépés nem mi kiválaszthatjuk tooenforce az AD a helyi jelszóházirend (összetettségét, kor, előzmények, szűrők, stb.) hello felhőben.
* **Üzenet lejárati házirendek** 
  * A Service Bus üdvözlőüzenetére helyezkedik el, mert a helyszíni szolgáltatással nem működik, ha a lejár, és több percig tooincrease biztonsági még tovább után el kell távolítani.

### <a name="password-writeback-encryption-details"></a>Jelszó visszaírási titkosítási részletek

a jelszó-visszaállítási folyamatot végighalad az elküldés után, a helyszíni környezetben, tooensure maximális szolgáltatás megbízhatóságát, és a biztonsági megérkezése előtt hello titkosítás lépései az alábbiakban található.

* **1. lépés – jelszó-titkosítási 2048 bites RSA-kulcsával** – Miután a felhasználó elküld egy jelszó toobe visszaírását tooon helyszíni első, hello beküldött jelszó magát egy 2048 bites RSA kulcs van titkosítva.
* **2. lépés - csomag szintű titkosítást AES-GCM-mel** -teljes csomag hello (jelszó + szükséges metaadatok) titkosított AES-GCM szolgáltatással, majd. Ez megakadályozza, hogy bárki, aki közvetlen hozzáférést toohello alapul szolgáló Szolgáltatásbusz csatorna megjelenítése/illetéktelenül hello tartalmát.
* **Lépés a 3 - TLS keresztül történik az összes kommunikáció / SSL** -Szolgáltatásbusz minden hello kommunikációs történik, egy SSL/TLS-csatorna. Ez biztosítja a hello tartalma nem engedélyezett 3. fél.
* **Automatikus kulcsváltást hathavonta** – automatikusan minden hatodik hónapban, vagy minden alkalommal, amikor a jelszóvisszaírás van letiltva / újra engedélyezik az Azure AD Connect jelenleg helyettesítő ezen kulcsok tooensure maximális szolgáltatás biztonsági és biztonsági.

### <a name="password-writeback-bandwidth-usage"></a>Jelszó visszaírási sávszélesség-használat

A jelszóvisszaírás egy kis sávszélességű szolgáltatás által küldött kérelmek biztonsági toohello helyszíni ügynök csak a következő körülmények között hello alatt:

1. Két üzeneteket küldhet, ha engedélyezésével vagy letiltásával hello szolgáltatást az Azure AD Connect használatával.
2. Mindaddig, amíg hello szolgáltatás fut egy üzenetet küld 5 percenként egyszer, a szolgáltatás szívverést.
3. Két üzenetek küldése történik, minden alkalommal új jelszó küldése
    * Első üzenetet, egy kérelem tooperform hello művelet
    * Második üzenetet, amely tartalmaz hello hello művelet eredményét, és küldése a következő körülmények között hello:
        * Minden alkalommal, amikor a felhasználó önkiszolgáló jelszó alaphelyzetbe állítása során egy új jelszó küldése.
        * Minden alkalommal, amikor egy új jelszó küldése a felhasználói jelszó-változtatási művelet során.
        * Minden alkalommal, amikor egy új jelszó küldése során egy rendszergazda által kezdeményezett felhasználói jelszó alaphelyzetbe állítása (a hello Azure felügyeleti portálon).

#### <a name="message-size-and-bandwidth-considerations"></a>Üzenet mérete és a sávszélesség kapcsolatos szempontok

a fent leírt üdvözlőüzenetére mindegyikének hello mérete általában a 1 kb szélsőséges terhelés alatt is, hello jelszó visszaírási szolgáltatás nem használ-e a sávszélesség néhány kilobit / másodperc. Mert minden egyes üzenettel valós időben csak akkor, ha a jelszó-frissítési művelet által igényelt, és hello üzenet mérete ezért kicsi, mert hello sávszélesség-használat hello visszaírási funkció hatékonyan túl kicsi toohave bármilyen valós mérhető hatással.

## <a name="next-steps"></a>Következő lépések

a következő hivatkozások hello adja meg a jelszó alaphelyzetbe állítása, az Azure AD használatával kapcsolatos további információk

* [**Gyors üzembe helyezés**](active-directory-passwords-getting-started.md) – Percek alatt üzembe helyezheti az Azure AD önkiszolgáló jelszókezelőjét. 
* [**Licencelés**](active-directory-passwords-licensing.md) – Az Azure AD licencelésének konfigurálása.
* [**Adatok** ](active-directory-passwords-data.md) - szükséges hello adatok megismeréséhez, és hogyan használja fel azokat a jelszókezelés
* [**Bevezetés** ](active-directory-passwords-best-practices.md) -megtervezése és telepítése az önkiszolgáló jelszó-Változtatási tooyour felhasználók hello útmutatást itt talál
* [**Testre szabhatja** ](active-directory-passwords-customize.md) -testreszabása, önkiszolgáló jelszó-Változtatási élményt a vállalata hello hello megjelenését és működését.
* [**Szabályzat**](active-directory-passwords-policy.md) – Megismerheti és beállíthatja az Azure AD jelszószabályzatait.
* [**Jelentéskészítés**](active-directory-passwords-reporting.md) – Megtudhatja, mikor és hol érik el a felhasználói az SSPR funkcióit.
* [**Műszaki mélyreható** ](active-directory-passwords-how-it-works.md) -mögött hello függöny toounderstand nyissa meg annak működéséről
* [**Gyakori kérdések**](active-directory-passwords-faq.md) – Hogyan? Hogy miért? Mi? Hová? Ki? Mikor? -Válaszok mindig kívánta tooask tooquestions
* [**Hibaelhárítás** ](active-directory-passwords-troubleshoot.md) -megtudhatja, hogyan tooresolve közös állít ki, hogy az önkiszolgáló jelszó-Változtatási látható

[Writeback]: ./media/active-directory-passwords-writeback/enablepasswordwriteback.png "Az Azure AD Connectben a jelszóvisszaírás engedélyezése"
