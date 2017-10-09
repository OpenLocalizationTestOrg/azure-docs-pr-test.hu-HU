---
title: "Házirend: Azure AD SSPR |} Microsoft Docs"
description: "Az Azure AD az önkiszolgáló jelszó-változtatási házirend-beállításokban"
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
ms.openlocfilehash: af7cb13794bf3a9fee91d355f788aa5c2246e57c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Jelszóházirendek és -korlátozások az Azure Active Directoryban

Ez a cikk ismerteti a hello jelszóházirendek és az Azure AD-bérlő tárolt felhasználói fiókok társított bonyolultsági követelményeknek.

## <a name="administrator-password-policy-differences"></a>Rendszergazdai jelszó házirend különbségek

A Microsoft új jelszó kérésére vonatkozó erős alapértelmezett **két kapus** szabályzat alkalmazását írja elő minden Azure rendszergazdai szerepkörhöz (például: globális rendszergazda, ügyfélszolgálati rendszergazda, jelszórendszergazda stb.)

Ezzel letiltja a rendszergazdák a biztonsági kérdések, és érvénybe lépteti a hello következő.

Két kapu házirend igénylő kétféle hitelesítési adatok (e-mail cím **és** telefonszám), a következő körülmények között hello vonatkozik

* Az összes Azure-rendszergazdai szerepkörök
  * Segélyszolgálat rendszergazda
  * Szolgáltatás-rendszergazda
  * Számlázási rendszergazda
  * Partner Tier1 támogatása
  * Partner Tier2 támogatása
  * Exchange szolgáltatás-rendszergazda
  * Lync szolgáltatás-rendszergazda
  * Felhasználói fiók rendszergazdája
  * Directory írók
  * Globális rendszergazda/vállalati rendszergazda
  * A SharePoint szolgáltatás-rendszergazda
  * Megfelelőségi rendszergazda
  * Alkalmazás-rendszergazda
  * Biztonsági rendszergazda
  * Kiemelt szerepkör rendszergazda
  * Intune szolgáltatás-rendszergazda
  * Application Proxy szolgáltatás-rendszergazda
  * CRM szolgáltatás-rendszergazda
  * A Power BI szolgáltatás-rendszergazda
  
* 30 nap eltelt a próbaverzió **vagy**
* Megtalálható a személyes tartomány (contoso.com) **vagy**
* Az Azure AD Connect visszaszinkronizálja a helyszíni címtárban szereplő identitásokat

### <a name="exceptions"></a>Kivételek
Egy kapu házirend igénylő hitelesítési adatok egy részét (e-mail cím **vagy** telefonszám), a következő körülmények között hello vonatkozik

* A próbaverzió első 30 napnyi **vagy**
* Személyes tartomány nincs jelen (*. onmicrosoft.com) **és** az Azure AD Connect nem szinkronizál identitások


## <a name="userprincipalname-policies-that-apply-tooall-user-accounts"></a>UserPrincipalName házirend alkalmazása tooall felhasználói fiókok

Összes felhasználói fiók, amelyet a tooAzure AD a toosign rendelkeznie kell egy egyedi felhasználói egyszerű felhasználónév (UPN) attribútum értéke annak a fióknak. hello házirendek vázol fel, tooboth alkalmazó az alábbi hello táblázatban a helyszíni Active Directory felhasználói fiókoknak toohello felhő- és csak toocloud felhasználói fiókok szinkronizálása.

| Tulajdonság | UserPrincipalName követelmények |
| --- | --- |
| Karakterből állhat |<ul> <li>A – Z</li> <li>a - z-ig</li><li>0 – 9</li> <li> . - \_ ! \# ^ \~</li></ul> |
| Karakterek nem használhatók. |<ul> <li>A "@" karakter, amely nem az elválasztó hello felhasználónév hello tartományból.</li> <li>A pont karakter nem tartalmazhat "." közvetlenül megelőző hello "@" szimbólummal</li></ul> |
| Hossz megkötések |<ul> <li>Teljes hossza nem haladhatja meg a 113 karaktert</li><li>64 karakter hosszúságú lehet, mielőtt hello "@" szimbólummal</li><li>48 karakter után hello "@" szimbólummal</li></ul> |

## <a name="password-policies-that-apply-only-toocloud-user-accounts"></a>Jelszó-házirenddel, csak a toocloud felhasználói fiókok

a következő táblázat hello hello rendelkezésre álló jelszó házirend-beállításokat ismerteti, amelyek létrehozása és kezelése az Azure AD alkalmazott toouser fiókok lehetnek.

| Tulajdonság | Követelmények |
| --- | --- |
| Karakterből állhat |<ul><li>A – Z</li><li>a - z-ig</li><li>0 – 9</li> <li>@ # $ % ^ & * - _ ! + = [ ] { } &#124; \ : ‘ , . ? / ` ~ “ ( ) ;</li></ul> |
| Karakterek nem használhatók. |<ul><li>Unicode-karaktereket</li><li>Tárolóhelyek</li><li> **Csak az erős jelszavak**: egy karaktersor nem tartalmazhat "." közvetlenül megelőző hello "@" szimbólummal</li></ul> |
| Jelszó-korlátozások |<ul><li>minimum 8 karakter, és legfeljebb 16 karakter</li><li>**Csak az erős jelszavak**: igényel 3 hello alábbi 4-ből:<ul><li>Kisbetűk</li><li>Nagybetűk</li><li>Számok (0-9)</li><li>Szimbólumok (lásd a fenti jelszó korlátozások)</li></ul></li></ul> |
| Jelszavak érvényességi időtartamát |<ul><li>Alapértelmezett érték: **90** nap </li><li>Érték használatával lehet konfigurálni az Active Directory modul Windows Powershellhez készült Azure hello hello Set-MsolPasswordPolicy parancsmagjával.</li></ul> |
| Jelszó lejáratáról szóló értesítés |<ul><li>Alapértelmezett érték: **14** napig (lejárt jelszó)</li><li>A értéke konfigurálható hello Set-MsolPasswordPolicy parancsmagjával.</li></ul> |
| Jelszó lejárata |<ul><li>Alapértelmezett érték: **hamis** nap (azt jelzi, hogy a jelszó lejárati engedélyezve van) </li><li>Érték felhasználói fiókok egyesével történő hello Set-MsolUser parancsmag használatával konfigurálhatók. </li></ul> |
| Jelszó **módosítása** előzmények |Utolsó jelszó **nem** használható újra amikor **módosítása** jelszót. |
| Jelszó **alaphelyzetbe** előzmények | Utolsó jelszó **előfordulhat, hogy** használható újra amikor **alaphelyzetbe állítása** elfelejtett jelszavát. |
| A Fiókzárolásra |10 sikertelen bejelentkezési kísérlet után (helytelen jelszót) hello felhasználói rendszer zárolja egy percig. További helytelen bejelentkezési kísérletek hello felhasználói zárolás időtartamának növelése. |

## <a name="set-password-expiration-policies-in-azure-active-directory"></a>Jelszó lejárati házirendek beállítása az Azure Active Directoryban

Egy Microsoft felhőszolgáltatásra globális rendszergazdája használhatja hello Microsoft Active Directory modul Windows Powershellhez készült Azure tooset felhasználói jelszavak nem tooexpire fel. A Windows PowerShell parancsmagok tooremove hello-véglegesek konfiguráció, vagy toosee mely felhasználói jelszavak beállítása nem tooexpire is használható. Ez az útmutató tooother szolgáltatók például a Microsoft Intune és az Office 365, amely identitás- és a directory Services Microsoft Azure Active Directory is támaszkodjon vonatkozik.

> [!NOTE]
> Csak jelszavak keresztül a címtár-szinkronizálás nem szinkronizált felhasználói fiókok konfigurálható toonot lejár. További információ a címtár-szinkronizálás:[AD az Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).
>
>

## <a name="set-or-check-password-policies-using-powershell"></a>Állítsa be, vagy ellenőrizze a jelszóházirendek PowerShell használatával

tooget elindult, kell túl[töltse le és telepítse az Azure AD PowerShell modult hello](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0). Ha már van telepítve, a lépésekkel hello tooconfigure alatt minden mező.

### <a name="how-toocheck-expiration-policy-for-a-password"></a>Hogyan toocheck jelszó-elévülési szabályzatának
1. Csatlakozás tooWindows PowerShell vállalati rendszergazdai hitelesítő adataival.
2. Végrehajtás hello a következő parancsok egyikét:

   * toosee hogy egyetlen felhasználó jelszavának beállítása toonever lejár, futtassa a következő parancsmag használatával hello egyszerű felhasználónév (UPN) hello (például aprilr@contoso.onmicrosoft.com) vagy a felhasználói azonosító hello hello felhasználó toocheck szeretne:`Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`
   * toosee hello "Jelszó sohasem jár le" beállítást az összes felhasználó számára, futtassa a következő parancsmag hello:`Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

### <a name="set-a-password-tooexpire"></a>A jelszó tooexpire beállítása

1. Csatlakozás tooWindows PowerShell vállalati rendszergazdai hitelesítő adataival.
2. Végrehajtás hello a következő parancsok egyikét:

   * tooset hello egy felhasználó jelszavát, hogy hello jelszava lejár, futtassa a következő parancsmag használatával hello egyszerű felhasználónév (UPN) hello, vagy felhasználó azonosítója hello hello:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`
   * tooset hello jelszavak az összes olyan felhasználó hello szervezet, hogy a várósorban, használja a következő parancsmag hello:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

### <a name="set-a-password-toonever-expire"></a>A jelszó toonever lejár beállítása

1. Csatlakozás tooWindows PowerShell vállalati rendszergazdai hitelesítő adataival.
2. Végrehajtás hello a következő parancsok egyikét:

   * egy felhasználó toonever tooset hello jelszava lejár, futtassa a következő parancsmag használatával hello egyszerű felhasználónév (UPN) hello, vagy hello hello felhasználó azonosítója:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`
   * egy szervezet toonever hello felhasználók tooset hello jelszavak lejár, futtassa a következő parancsmag hello:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

## <a name="next-steps"></a>Következő lépések

a következő hivatkozások hello adja meg a jelszó alaphelyzetbe állítása, az Azure AD használatával kapcsolatos további információk

* [**Gyors üzembe helyezés**](active-directory-passwords-getting-started.md) – Percek alatt üzembe helyezheti az Azure AD önkiszolgáló jelszókezelőjét. 
* [**Licencelés**](active-directory-passwords-licensing.md) – Az Azure AD licencelésének konfigurálása.
* [**Adatok** ](active-directory-passwords-data.md) - szükséges hello adatok megismeréséhez, és hogyan használja fel azokat a jelszókezelés
* [**Bevezetés** ](active-directory-passwords-best-practices.md) -megtervezése és telepítése az önkiszolgáló jelszó-Változtatási tooyour felhasználók hello útmutatást itt talál
* [**Testre szabhatja** ](active-directory-passwords-customize.md) -testreszabása, önkiszolgáló jelszó-Változtatási élményt a vállalata hello hello megjelenését és működését.
* [**Jelentéskészítés**](active-directory-passwords-reporting.md) – Megtudhatja, mikor és hol érik el a felhasználói az SSPR funkcióit.
* [**Műszaki mélyreható** ](active-directory-passwords-how-it-works.md) -mögött hello függöny toounderstand nyissa meg annak működéséről
* [**Gyakori kérdések**](active-directory-passwords-faq.md) – Hogyan? Hogy miért? Mi? Hová? Ki? Mikor? -Válaszok mindig kívánta tooask tooquestions
* [**Hibaelhárítás** ](active-directory-passwords-troubleshoot.md) -megtudhatja, hogyan tooresolve közös állít ki, hogy az önkiszolgáló jelszó-Változtatási látható
