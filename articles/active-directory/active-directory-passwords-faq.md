---
title: "Gyakran ismételt kérdések: Azure AD SSPR |} Microsoft Docs"
description: "Gyakori kérdések az Azure AD az önkiszolgáló jelszó alaphelyzetbe állítása"
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
ms.openlocfilehash: d04a9efeb3b35421aa605cadb2aa25f656a4d515
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="password-management-frequently-asked-questions"></a>A jelszókezelés gyakran ismételt kérdések

a következő hello néhány gyakori kérdés, minden kapcsolódó toopassword alaphelyzetbe.

Ha az Azure AD általános kérdése van, és az önkiszolgáló jelszó alaphelyzetbe állítása, nem megválaszolandó itt, megkérheti hello közösségi Ha segítségre van szüksége a hello [Azure Ad-fórumok](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD). Hello Közösség tagjai közé tartozik a mérnökök, termék kezelők, MVP és ösztöndíjas informatikai szakemberek számára.

Ez a GYIK a következő szakaszok hello oszlik:

* [**A Jelszóátállítás regisztrációját kapcsolatos kérdések**](#password-reset-registration)
* [**Jelszó alaphelyzetbe állítása kérdések**](#password-reset)
* [**A jelszó módosítása kapcsolatos kérdések**](#password-change)
* [**Tudnivalók a jelszókezelésről kérdések jelentések**](#password-management-reports)
* [**A jelszóvisszaírás kapcsolatos kérdések**](#password-writeback)

## <a name="password-reset-registration"></a>Jelszó-átállítási regisztrációk
* **K: a felhasználók regisztrálhatják saját jelszó alaphelyzetbe állítása adatokat?**

  > **V:** Igen, mindaddig, amíg engedélyezve van a jelszó alaphelyzetbe állítása, és azok licencét, akkor lépjen toohello jelszó-átállítási regisztráció portálon http://aka.ms/ssprsetup tooregister, a hitelesítési adatokat. A felhasználók is regisztrálhatják által toohello hozzáférési panelre lesz a http://myapps.microsoft.com, hello-profil lapon, és kattintson a hello regisztrálása a jelszó alaphelyzetbe állítása lehetőséget.
  >
  >
* **K: jelszó alaphelyzetbe állítása adatok is definiálása a felhasználók nevében?**

  > **V:** Igen, akkor az Azure AD Connect PowerShell, hello [Azure-portálon](https://portal.azure.com), vagy hello Office felügyeleti portálon. További információkért lásd: hello cikk [az Azure AD önkiszolgáló jelszó-változtatási által használt adatok](active-directory-passwords-data.md).
  >
  >
* **K: biztonsági kérdéseket tesz fel a helyszíni adatok szinkronizálása?**

  > **V:** erre nincs lehetőség ma.
  >
  >
* **K: a felhasználók regisztrálhatják adatokat úgy, hogy más felhasználók nem láthatják ezeket az adatokat?**

  > **V:** Igen, ha a felhasználók regisztrálhatnak hello jelszó-változtatási regisztrációs portálra a Mentés személyes hitelesítési mezőkbe, amelyek csak a globális rendszergazdák és hello felhasználó által látható adatokat.
    >
    > [!NOTE]
    > Ha egy **Azure rendszergazdai fiók** regisztrálja a hitelesítéshez használni kívánt telefonszámot is gyűjteménytípust hello mobiltelefon mezőbe, és látható.
    >
  >
  >
* **K: a felhasználók rendelkeznek toobe regisztrálva a jelszó alaphelyzetbe állítása használatához?**

  > **V:** nem, ha elegendő hitelesítési adatokat definiálja a nevében, felhasználó nem rendelkezik tooregister. Jelszó-átállítási működik, amíg a megfelelően formázott adatok hello megfelelő mezőkben hello könyvtárban tárolja.
  >
  >
* **K: I szinkronizálása vagy hello hitelesítéshez megadott telefonját, hitelesítési E-mail vagy alternatív hitelesítéshez megadott telefonját mezők beállítása a felhasználók nevében?**

  > **V:** erre nincs lehetőség ma.
  >
  >
* **K: hogyan nem hello regisztrációs portálon, hogy mely beállítások tooshow a felhasználók?**

  > **V:** hello jelszó-változtatási regisztrációs portálra csak azt mutatja be, hogy a felhasználók számára engedélyezett beállítások hello. Ezek a beállítások hello a könyvtár konfigurálása lap felhasználói jelszó-visszaállítási házirend szakasza alatt találhatók. Például ez azt jelenti, hogy ha nem engedélyezi a biztonsági kérdéseket, majd felhasználók nem tud tooregister az adott beállítási mód.
  >
  >
* **Amikor egy felhasználó tekinthető k: regisztrálni?**

  > **V:** regisztrált sspr, amikor regisztrálják azokat A felhasználó akkor tekinthető legalább hello **módszerek szükséges tooreset száma** hello beállított [Azure-portálon](https://portal.azure.com).
  >
  >
## <a name="password-reset"></a>Új jelszó létrehozása
* **K: mennyi ideig I várjon tooreceive egy e-mailek, SMS vagy a jelszó alaphelyzetbe állítása a telefonhívás?**

  > **V:** e-mailben, SMS-t, és telefonhívásokat egy perc alatt 5-20 másodperc alatt hello normál esetben kell érkeznek.
    >Ha nem ez időkereten belül hello értesítés jelenik meg:
        > * Ellenőrizze a Levélszemét mappát.
        > * Ellenőrzése hello vagy e-mail, amelyhez csatlakozik egy várt hello.
        > * Ellenőrizze, hogy az hello directory hello hitelesítési adatai megfelelően van formázva.
                >     * Példa: "+ 1 4255551234" vagy "user@contoso.com"
  >
  >
* **K: jelszó alaphelyzetbe állítása milyen nyelveket támogatja?**

  > **V:** hello a jelszó alaphelyzetbe állítása felhasználói felület, SMS-t, és hangra vonatkozó hívások honosítva hello ugyanazt az Office 365-ben támogatott nyelvek.
  >
  >
* **K: milyen hello jelszó alaphelyzetbe állítása élmény részei beolvasása vállalati arculattal, szervezeti a címtárban branding meg tartozó konfigurálása lapon?**

  > **V:** hello jelszó-változtatási portál jeleníti meg a szervezeti embléma, és lehetővé teszi a tooconfigure hello kérje a rendszergazda kapcsolat toopoint tooa egyéni e-mail vagy URL-címe. Minden e-mailben küldi el jelszóvisszaállítás üdvözlő e-mail törzsét hello a vállalati embléma, a színeket, a nevét tartalmazza, és testre neve.
  >
  >
* **K: hogyan lehet tájékoztassa a felhasználókat a where toogo tooreset jelszavukat?**

  > **V:** is elküldheti a felhasználók toohttps://passwordreset.microsoftonline.com közvetlenül, vagy utasíthatja őket tooclick hello **nem tudja elérni a fiókot hivatkozás** bármely munkahelyi vagy iskolai bejelentkezési lapján található. Egy hely könnyen hozzáférhető tooyour felhasználók is közzéteheti ezeket a hivatkozásokat.
  >
  >
* **K: használhatok ezen a lapon egy mobileszközről?**

  > **V:** Igen, az ezen a lapon a mobileszközök működik.
  >
  >
* **K: támogatják a feloldásának helyi active directory-fiókokat, amikor a felhasználók visszaállíthassák a jelszavukat?**

  > **V:** Igen, ha a felhasználó visszaállítja a jelszavát, és a jelszóvisszaíró az Azure AD Connect használatával lett telepítve, adott felhasználói fiók esetén automatikusan feloldani a jelszavát.
  >
  >
* **K: hogyan integrálhatók a jelszó alaphelyzetbe állítása közvetlenül a saját felhasználói asztali bejelentkezés során tapasztal élmény?**

  > **V:** Ha az Azure AD Premium ügyfél egy, a Microsoft Identity Manager további költségek nélkül telepítheti és hello helyszíni jelszó alaphelyzetbe állítása megoldás toomeet telepíteni ezt a követelményt.
  >
  >
* **K: beállítása a különböző területi beállításokhoz különböző biztonsági kérdést?**

  > **V:** erre nincs lehetőség ma.
  >
  >
* **K: hogyan több kérdést azt konfigurálható hello biztonsági kérdések hitelesítési lehetőséget?**

  > **V:** meg too20 egyéni biztonsági kérdéseket a hello konfigurálható [Azure-portálon](https://portal.azure.com).
  >
  >
* **K: mennyi ideig lehet, hogy a biztonsági kérdések kell?**

  > **V:** biztonsági kérdések 3 és 200 karakter közötti lehet.
  >
  >
* **K: mennyi ideig toosecurity kérdésekre adott válaszok lehet?**

  > **V:** válaszok 3 too40 karakter hosszú lehet.
  >
  >
* **K: ismétlődő válaszokat toosecurity kérdések utasítja el?**

  > **V:** Igen, azt utasítsa el az ismétlődő válaszokat toosecurity kérdéseket.
  >
  >
* **K: Előfordulhat, hogy a felhasználó regisztrálása hello ugyanazt biztonsági kérdés egynél többször?**

  > **V:** nem, miután a felhasználó regisztrál egy adott kérdést, előfordulhat, hogy nem regisztrálják az adott kérdést, még egyszer.
  >
  >
* **K: az azt a biztonsági kérdések regisztrációs és alaphelyzetbe állítása a minimális korlát lehetséges tooset?**

  > **V:** Igen, egy korlát a regisztrációs és egy másik alaphelyzetbe állítása a állítható be. lehet, hogy 3-5 biztonsági kérdések regisztrációjához szükséges, és lehet, hogy 3-5 alaphelyzetbe állítása szükséges.
  >
  >
* **Kérdés: Ha egy felhasználó több hello maximális kérdések szükséges tooreset regisztrálva van, hogy miként kell a biztonsági kérdések kijelölni alaphelyzetbe állítása során?**

  > **V:** N biztonsági kérdések véletlenszerűen kiválasztott kívül hello teljes száma a felhasználói kérdések regisztrálva van, ahol N az hello **kérdések szükséges tooreset száma**. Például ha egy felhasználó rendelkezik-e regisztrálva 5 biztonsági kérdéseit, de csak 3 szükséges tooreset, hello 5 3 véletlenszerűen és jelenik meg a következő alaphelyzetbe állítása. Hello felhasználó élvezheti hello válaszok toohello kérdések helytelen, ha a hello tanúsítványkiválasztási folyamat tooprevent kérdés beütés jelentkezik.
  >
  >
* **K: el azt, hogy felhasználók jelszó-változtatási rövid időn belül időn belül többször próbálnak?**

  > **V:** Igen, vannak beépítve a jelszó alaphelyzetbe állítása tooprotect visszaélés elleni funkciókat. Előfordulhat, hogy a felhasználók csak megpróbálják 5 jelszó alaphelyzetbe állítása kísérletek 24 óra alatt zárolása előtt egy órán belül. A felhasználók előfordulhat, hogy csak megpróbálják toovalidate telefonszám 5-ször 24 óra alatt zárolása előtt egy órán belül. A felhasználók csak lehet, hogy megpróbálják egy egyetlen hitelesítési módszer 24 óra alatt zárolása előtt egy órán belül 5 alkalommal.
  >
  >
* **K:, hogy mennyi ideig érvényesek hello e-mailek és SMS egyszer használatos jelszót?**

  > **V:** hello munkamenetek élettartamát, jelszó-visszaállításhoz érték 105 perc. Hello hello kezdetén jelszó-átállítási művelet, hello felhasználó rendelkezik 105 perc tooreset a jelszavát. hello e-mailek és SMS egyszer használatos jelszót érvénytelenek ebben az időszakban lejárata után is.
  >
  >

## <a name="password-change"></a>A jelszó módosítása
* **K: kell a felhasználók hová toochange jelszavukat?**

  > **V:** felhasználók előfordulhat, hogy módosítsák jelszavukat bárhol akkor jelenik meg a profil kép vagy ikon (a hello jobb felső sarkában, például a [Office 365](https://portal.office.com) vagy [hozzáférési Panel](https://myapps.microsoft.com) észlel. Felhasználók a jelszavuk változhat hello [hozzáférési Panel profilszerkesztési lap](https://account.activedirectory.windowsazure.com/r#/profile). Felhasználók is lehet, hogy az alkalmazás megkéri toochange automatikusan hello Azure AD bejelentkezési képernyő, a jelszavuk jelszavuk. Végezetül, a felhasználók megnyitják toohello előfordulhat, hogy [Azure AD-jelszó módosítása Portal](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) közvetlenül a jelszavuk alapértékekkel toochange.
  >
  >
* **K: a felhasználók értesítést kaphat az Office portál hello helyszíni jelszavukat lejárati?**

  > **V:** Ez azért lehetséges ma használata az AD FS itt hello utasításokat követve: [küldése jelszó házirend jogcímek az AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396). Ha a Jelszókivonat-szinkronizálást használ, ez nem lehetséges ma. Ennek az az oka, hogy szinkronizálja a helyszíni jelszóházirendek, ezért nem lehetséges az USA toopost lejárati értesítések toocloud észlel. Mindkét esetben is lehetőség túl[értesítse a felhasználókat, amelyeknek a jelszavánál tooexpire kerül a PowerShell használatával](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).
  >
  >

## <a name="password-management-reports"></a>Jelszó-kezelési jelentések
* **K: mennyi időt vesz igénybe az adatok tooshow mentése hello jelszó felügyeleti jelentésekre?**

  > **V:** adatokat meg kell jelennie hello jelszó jelentések 5-10 percen belül. Az egyes példányok tooan óra tooappear igénybe vehet.
  >
  >
* **K: hogyan végezhet hello jelszó jelentések?**

  > **V:** hello jelszó jelentések hello kis nagyítóüveg toohello jobb hello oszlop címkék hello jelentés hello tetején kattintva szűrheti is. Ha azt szeretné, hogy toodo gazdagabb szűrés, hello jelentés tooexcel letöltheti és kimutatás létrehozása.
  >
  >
* **K: Mi az az események maximális száma hello hello jelszó jelentések tárolódnak?**

  > **V:** be too75, 000 jelszó alaphelyzetbe állítása vagy a jelszó alaphelyzetbe állítása regisztrációs események hello jelszó felügyeleti jelentések, készítsen biztonsági másolatot too30 nap átfedés vannak tárolva.  Dolgozunk ennek tooexpand ez tooinclude további események száma.
  >
  >
* **K: milyen távolságban vissza hello jelszó jelentések Ugrás?**

  > **V:** hello jelszókezelés jelentések megjelenítése műveletek hello belül előforduló utolsó 30 nap. A lépést Ha ezek az adatok kell tooarchive meg is letöltheti hello jelentések rendszeres időközönként és mentheti azokat külön.
  >
  >
* **K: van egy hello jelszó jelentések megjeleníthető sorok maximális számát?**

  > **V:** Igen, a 75,000 sorok maximális jelenhetnek meg akár hello jelszó jelentések, hogy azok alatt jelennek meg hello felhasználói felületén vagy a letöltött.
  >
  >
* **K: van egy API tooaccess hello jelszó alaphelyzetbe állítása vagy a regisztráció jelentési adatot?**

  > **V:** Igen, tekintse meg a hello következő dokumentáció toolearn hogyan férhet hozzá a hello jelszó alaphelyzetbe állítása jelentéskészítési adatfolyamban.  [Ismerje meg, hogyan tooaccess jelszó-átállítási jelentési eseményeket programozott módon](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).
  >
  >

## <a name="password-writeback"></a>Jelszóvisszaíró
* **K: hogyan működik a jelszóvisszaírás hello háttérben?**

  > **V:** lásd [jelszóvisszaírás működése](active-directory-passwords-writeback.md) az annak magyarázatát, mi történik, ha engedélyezi a jelszóvisszaírás, és hogyan adatáramlás hello rendszeren keresztül vissza a helyszíni környezetbe.
  >
  >
* **K: mennyi ideig tart a jelszóvisszaírás toowork?  A szinkronizálás késleltetés, például a Jelszókivonat-szinkronizálás van?**

  > **V:** jelszóvisszaírás azonnali. Egy szinkron folyamatot, amely a Jelszókivonat-szinkronizálást alapvetően eltérően működik. A jelszóvisszaírás engedélyezése a felhasználók tooget valós idejű visszajelzést hello sikeres, a jelszó alaphelyzetbe állítása, vagy módosítsa a műveletet. hello átlagos jelszó sikeres visszaírása ideje alatt 500 ms.
  >
  >
* **K:, ha a helyi fiók le van tiltva, milyen hatással a van a felhő fiókelérést?**

  > **V:** a helyszíni-azonosítója le van tiltva, ha a felhő azonosítója/hozzáférést is le lesz tiltva hello tovább szinkronizálás gyakorisága byt alapértelmezett AAD-csatlakozás keresztül ez az 30 percenként.
  >
  >
* **K:, ha egy a helyszíni Active Directory jelszóházirend korlátozza a helyi fiók, önkiszolgáló jelszó-Változtatási felel meg a házirend hello jelszó módosításakor?**

  > **V:** Igen, önkiszolgáló jelszó-Változtatási alapul, és aláveti hello a helyszíni AD-jelszóházirendet, beleértve a tipikus AD tartományi jelszóházirend, valamint bármely meghatározott részletes jelszóházirendek felhasználói tooa céloz.
  >
  >
* **K: milyen típusú fiókok jelszóvisszaírás működik?**

  > **V:** jelszóvisszaírás külső és a jelszó kivonatát szinkronizált felhasználók működik.
  >
  >
* **K: jelszóvisszaírás jelszóházirendjeinek érvényesítéséhez a tartományomat?**

  > **V:** Igen, érvénybe lépteti a jelszóvisszaírást a jelszó élettartama, előzmények, összetettségét, szűrők és helyezhet jelszavak helyen vannak a helyi tartományban lévő bármely más korlátozás.
  >
  >
* **K: az jelszóvisszaírás biztonságos?  Hogyan lehet, hogy I nem fognak első megtámadott?**

  > **V:** Igen, a jelszóvisszaírás biztonságos-e. További információ a négy hello biztonsági réteg alkalmazása tooread hello jelszó visszaírási szolgáltatás által megvalósított, tekintse meg a hello [jelszó visszaírási biztonsági modell](active-directory-passwords-writeback.md#password-writeback-security-model) jelszóvisszaírás működése című szakasza.
  >
  >

## <a name="next-steps"></a>Következő lépések

a következő hivatkozások hello adja meg a jelszó alaphelyzetbe állítása, az Azure AD használatával kapcsolatos további információk

* [**Gyors üzembe helyezés**](active-directory-passwords-getting-started.md) – Percek alatt üzembe helyezheti az Azure AD önkiszolgáló jelszókezelőjét. 
* [**Licencelés**](active-directory-passwords-licensing.md) – Az Azure AD licencelésének konfigurálása.
* [**Adatok** ](active-directory-passwords-data.md) - szükséges hello adatok megismeréséhez, és hogyan használja fel azokat a jelszókezelés
* [**Bevezetés** ](active-directory-passwords-best-practices.md) -megtervezése és telepítése az önkiszolgáló jelszó-Változtatási tooyour felhasználók hello útmutatást itt talál
* [**Testre szabhatja** ](active-directory-passwords-customize.md) -testreszabása, önkiszolgáló jelszó-Változtatási élményt a vállalata hello hello megjelenését és működését.
* [**Jelentéskészítés**](active-directory-passwords-reporting.md) – Megtudhatja, mikor és hol érik el a felhasználói az SSPR funkcióit.
* [**Szabályzat**](active-directory-passwords-policy.md) – Megismerheti és beállíthatja az Azure AD jelszószabályzatait.
* [**Jelszóvisszaíró**](active-directory-passwords-writeback.md) – Megtudhatja, hogyan használhatja a jelszóvisszaírót a helyszíni címtárával.
* [**Műszaki mélyreható** ](active-directory-passwords-how-it-works.md) -mögött hello függöny toounderstand nyissa meg annak működéséről
* [**Hibaelhárítás** ](active-directory-passwords-troubleshoot.md) -megtudhatja, hogyan tooresolve közös állít ki, hogy az önkiszolgáló jelszó-Változtatási látható
