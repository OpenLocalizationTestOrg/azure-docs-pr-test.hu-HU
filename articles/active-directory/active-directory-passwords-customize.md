---
title: "Testreszabás: Azure AD SSPR |} Microsoft Docs"
description: "Önkiszolgáló jelszóváltoztatás szolgáltatás az Azure AD beállítások testreszabása"
services: active-directory
keywords: 
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
ms.openlocfilehash: 4762fffef040f9b409355f9ee0e8cc593e3eea0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customize-azure-ad-functionality-for-self-service-password-reset"></a>Az Azure AD-funkciók testreszabása az önkiszolgáló jelszó-változtatási

Informatikai szakemberek számára toodeploy önkiszolgáló jelszóváltoztatás keresése hello élmény toomatch szabhatja testre a felhasználók.

## <a name="customize-hello-contact-your-administrator-link"></a>Testreszabás hello kérje a rendszergazda hivatkozás

Akkor is, ha önkiszolgáló jelszó-Változtatási nem engedélyezett felhasználók továbbra is egy "forduljon a rendszergazdához" hivatkozásra kattintva hello jelszó-visszaállítási portál.  Ez a hivatkozás e-mailek a rendszergazdák a hello jelszó módosítása segítségét kéri. Az e-mailt küld a következő sorrend hello címzetteket toohello:

1. Ha hello **jelszókezelő** szerepkör van hozzárendelve, akkor kapnak értesítést, ezzel a szerepkörrel rendelkező rendszergazdák
2. Ha nincs jelszókezelők jelszavát állíthatják rendelt, majd a rendszergazda hello **felhasználó rendszergazda** szerepkör értesítést kap
3. Ha hello előző szerepkörök egyike sem volt rendelve, majd **globális rendszergazdák** értesítést kap

Minden esetben értesítést kap egy legfeljebb 100 címzetteknek.

További információk a hello másik, rendszergazdai szerepkörök és hogyan tooassign őket lásd: hello dokumentum toofind [rendszergazdai szerepkörök hozzárendelése az Azure Active Directoryban](active-directory-assign-admin-roles.md)

### <a name="disable-contact-your-administrator-emails"></a>Tiltsa le kérje a rendszergazda e-mailek

Ha a szervezet nem szeretné a rendszergazdák, értesítse a jelszó alaphelyzetbe állítása kérelmeket, hello következő konfigurációs engedélyezhető

* Az önkiszolgáló jelszó-változtatási minden végfelhasználói engedélyezése. Ez a beállítás akkor a **jelszó-átállítási > Tulajdonságok**.
    * Ha nem szeretné felhasználók tooreset saját jelszavukat, hatókörét megadhatja a hozzáférés tooan üres csoport **ezt a beállítást nem ajánlott**.
* A webalkalmazás URL-cím és a mailto hello segélyszolgálat hivatkozás tooprovide testreszabásához: címét, hogy a felhasználók használhatnak tooget segítséget. Ez a beállítás akkor a **jelszó-átállítási > Testreszabás > egyéni segélyszolgálat e-mail vagy URL-címe**.

## <a name="customize-adfs-sign-in-page-for-sspr"></a>Sspr az AD FS bejelentkezési oldal testreszabása

Az AD FS-rendszergazdák adhat hozzá egy hivatkozást tootheir bejelentkezési oldal hello cikke hello útmutatásának [bejelentkezésilap-leírás hozzáadása](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description).

Hello parancsot, amely az AD FS-kiszolgálón a következő ad a hivatkozás toohello az AD FS bejelentkezési oldal így közvetlenül a felhasználók tooenter hello önkiszolgáló jelszó-visszaállítási munkafolyamat.

``` Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href=’https://passwordreset.microsoftonline.com’>Can’t access your account?</A></p>" ```

## <a name="customize-hello-sign-in-and-access-panel-look-and-feel"></a>Hello bejelentkezési és a hozzáférési panel megjelenését és működését testreszabása

Amikor a felhasználók hello bejelentkezési oldal el, testre szabhatja hello bejelentkezési oldal kép toofit együtt jelenik meg a vállalati védjegyadatoknak hello embléma.

Ezek a képek a következő körülmények között hello láthatók:

* A felhasználó után meg kell adnia a felhasználóneve
* Felhasználó fér hozzá az egyéni URL-címe
    * Által átadott hello "whr" paraméter toohello jelszó-átállítási lap, például a "https://login.microsoftonline.com/?whr=contoso.com"
    * Által átadott hello "felhasználónév" paraméter toohello jelszó-átállítási lap, például "https://login.microsoftonline.com/?username=admin@contoso.com"

### <a name="graphics-details"></a>Grafikus részletek

hello következő beállításai lehetővé teszik toochange hello látható jellemzőit hello bejelentkezési oldal és alatt található **Azure Active Directory**, **vállalati arculat**, **vállalati szerkesztése védjegyek**

* Bejelentkezési lapot a lemezkép PNG, JPG vagy fájl 1420 x 1200 képpont, és nem lehet nagyobb 500KB-nál. Ajánlott toobe körülbelül 200 KB a legjobb eredmények elérése érdekében.
* Bejelentkezési oldal háttérszíne használatakor a nagy késleltetésű kapcsolat, és hello RGB hexadecimális formátumban kell megadni.
* Transzparens lemezkép PNG, JPG vagy fájl 60 x 280 képpont, és nem lehet nagyobb, mint 10 KB.
* Négyzetes embléma (normál és a sötét téma) PNG vagy JPG 240 x 240 (méretezhető) nem lehet nagyobb 10 KB.

### <a name="sign-in-text-options"></a>Bejelentkezési beállítások

a következő beállítások hello tooadd szöveg toohello bejelentkezési oldal megfelelő tooyour szervezet engedélyezése. Ezek a beállítások alatt található **Azure Active Directory**, **vállalati arculat**, **Szerkesztés vállalati arculat megjelenítése**

* **Felhasználói név mutató** cserél hello példa szövege someone@example.com toobe bal alapértelmezett valami jobban megfelelő, a felhasználók számára, az ajánlott, ha a belső és külső felhasználók
* **Bejelentkezési oldal szövege** legfeljebb 256 karakter hosszúságú. Ez a szöveg jelenik meg bárhol a felhasználók bejelentkezési online, és az Azure AD Join élmény a Windows 10 hello. Használja ezt a szöveget használati feltételeinek, utasításokat, és tippek a felhasználók számára. **Bárki, aki a bejelentkezési oldal, nem adja meg a bizalmas információk itt tekintheti meg.**

### <a name="keep-me-signed-in-disabled"></a>A Bejelentkezve szeretnék maradni beállítás le van tiltva

hello beállítás "bejelentkezve szeretnék maradni letiltva" lehetővé teszi, hogy a felhasználók tooremain bejelentkezett zárja be, és nyissa meg a böngészőablakban. Ez a beállítás nem befolyásolja a munkamenet élettartama. Ez a beállítás alatt található **Azure Active Directory > Vállalati arculat > Szerkesztés Védjegyadatok**.

Néhány funkciójának Office 2010 és SharePoint Online függőség rendelkezzen a felhasználók képesek toocheck ebben a mezőben. Elrejti ezt a beállítást, ha a felhasználók kaphat további és váratlan bejelentkezési megjelenő utasításokat.

### <a name="directory-name"></a>Könyvtár neve

Módosíthatja a hello name attribútum alapján **Azure Active Directory > Tulajdonságok** tooshow felhasználóbarát cégnév hello portálon láthatók, és automatikus kommunikáció. Ez a beállítás akkor legjobban látható automatikus e-mailek, az alábbi hello űrlapjain hello formájában

* E-mailben "CONTOSO bemutató nevében Microsoft" felhasználóbarát neve
* E-mailben "CONTOSO bemutató fiók e-mail ellenőrző kód" tárgy

## <a name="next-steps"></a>Következő lépések

a következő hivatkozások hello adja meg a jelszó alaphelyzetbe állítása, az Azure AD használatával kapcsolatos további információk

* [**Gyors üzembe helyezés**](active-directory-passwords-getting-started.md) – Percek alatt üzembe helyezheti az Azure AD önkiszolgáló jelszókezelőjét. 
* [**Licencelés**](active-directory-passwords-licensing.md) – Az Azure AD licencelésének konfigurálása.
* [**Adatok** ](active-directory-passwords-data.md) - szükséges hello adatok megismeréséhez, és hogyan használja fel azokat a jelszókezelés
* [**Bevezetés** ](active-directory-passwords-best-practices.md) -megtervezése és telepítése az önkiszolgáló jelszó-Változtatási tooyour felhasználók hello útmutatást itt talál
* [**Szabályzat**](active-directory-passwords-policy.md) – Megismerheti és beállíthatja az Azure AD jelszószabályzatait.
* [**Jelszóvisszaíró**](active-directory-passwords-writeback.md) – Megtudhatja, hogyan használhatja a jelszóvisszaírót a helyszíni címtárával.
* [**Jelentéskészítés**](active-directory-passwords-reporting.md) – Megtudhatja, mikor és hol érik el a felhasználói az SSPR funkcióit.
* [**Műszaki mélyreható** ](active-directory-passwords-how-it-works.md) -mögött hello függöny toounderstand nyissa meg annak működéséről
* [**Gyakori kérdések**](active-directory-passwords-faq.md) – Hogyan? Hogy miért? Mi? Hová? Ki? Mikor? -Válaszok mindig kívánta tooask tooquestions
* [**Hibaelhárítás** ](active-directory-passwords-troubleshoot.md) -megtudhatja, hogyan tooresolve közös állít ki, hogy az önkiszolgáló jelszó-Változtatási látható

