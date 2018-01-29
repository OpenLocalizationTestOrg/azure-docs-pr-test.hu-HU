---
title: "Feltételes hozzáférés az Azure Active Directory B2B együttműködés felhasználók |} Microsoft Docs"
description: "Az Azure Active Directory B2B együttműködés többtényezős hitelesítés (MFA) támogatja a szelektív hozzáférést biztosít a vállalati alkalmazásokhoz"
services: active-directory
documentationcenter: 
author: sasubram
manager: mtillman
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 09/11/2017
ms.author: sasubram
ms.openlocfilehash: 2f2cfc351d372d665aac054d52d6e1520e1ffe48
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/11/2017
---
# <a name="conditional-access-for-b2b-collaboration-users"></a>Feltételes hozzáférés a B2B együttműködés felhasználók

## <a name="multi-factor-authentication-for-b2b-users"></a>A B2B felhasználók a többtényezős hitelesítést
Az Azure AD B2B együttműködés a szervezetek kényszerítheti a többtényezős hitelesítés (MFA) házirendek a B2B felhasználók számára. Ezek a házirendek kényszerítheti a bérlői, alkalmazás vagy egyéni felhasználói szintjén, a megszokott módon, hogy a teljes munkaidejű alkalmazottak és a szervezet a engedélyezve vannak. Többtényezős hitelesítési házirendek érvényben vannak, az erőforrás-szervezetben.

Példa:
1. A vállalat rendszergazdai vagy információk munkavégző meghívja a vállalat B egy alkalmazás felhasználói *PEL* vállalat azonosítójához.
2. Alkalmazás *PEL* vállalat A többtényezős Hitelesítést a hozzáférési úgy van beállítva.
3. Ha a vállalat B felhasználó megpróbálja elérni az alkalmazás *PEL* A bérlők a vállalatnál is megkéri őket végezze el az MFA-kérdést.
4. A felhasználó az MFA legyenek A vállalat állíthat be, és úgy dönt, a többtényezős hitelesítés lehetőséget.
5. Ebben a forgatókönyvben minden identitás működik (az Azure AD vagy az msa-t, például, ha a felhasználók a vállalati B azonosítania társadalombiztosítási azonosító)
6. A vállalat elegendő az Azure AD Premium licenc, amely támogatja a többtényezős Hitelesítést kell rendelkeznie. A felhasználó a vállalati B igényel, a vállalat A. licenc

Hívja fel a bérlet felelős mindig a multi-factor Authentication a felhasználókat az erőforráspartner szervezetnél, akkor is, ha a fiókpartner-szervezet MFA képességekkel rendelkezik.

### <a name="setting-up-mfa-for-b2b-collaboration-users"></a>Többtényezős hitelesítés beállítása B2B együttműködés felhasználók
Annak megállapításához, hogy milyen egyszerűen B2B együttműködés felhasználók többtényezős hitelesítés beállítása, tekintse meg az alábbi videóban módját:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-conditional-access-setup/Player]

### <a name="b2b-users-mfa-experience-for-offer-redemption"></a>Többtényezős hitelesítés élményt a B2B felhasználók kínálnak érvényesítési
Tekintse meg az érvényesítési megtekintéséhez a következő animáció élményt:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/MFA-redemption/Player]

### <a name="mfa-reset-for-b2b-collaboration-users"></a>B2B együttműködés felhasználóknak az új MFA
Jelenleg a rendszergazda megkövetelheti B2B együttműködés felhasználók igazolása be újra csak a következő PowerShell-parancsmagok használatával:

1. Összekapcsolás az Azure AD-vel

  ```
  $cred = Get-Credential
  Connect-MsolService -Credential $cred
  ```
2. Minden felhasználó lekérdezni módszereket igazolása

  ```
  Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```
  Például:

  ```
  Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```

3. Állítsa vissza a multi-factor Authentication módszer egy adott felhasználó, hogy a B2B együttműködés felhasználó igazolása felfelé módszerek újból beállítani. Példa:

  ```
  Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName gsamoogle_gmail.com#EXT#@ WoodGroveAzureAD.onmicrosoft.com
  ```

### <a name="why-do-we-perform-mfa-at-the-resource-tenancy"></a>Miért most elvégezni a többtényezős Hitelesítést, az erőforrás-bérlőhöz?

A jelenlegi kiadásban MFA mindig van kapcsolva az erőforrás-bérlőhöz, kiszámíthatóságot miatt. Például tegyük fel, a Contoso felhasználó (Sally) felkérik, hogy a Fabrikam és Fabrikam engedélyezte a többtényezős hitelesítés a B2B felhasználók számára.

Ha Contoso többtényezős hitelesítési szabályzat az App1 számítógépen, de nem App2 engedélyezve van, majd ha úgy tekintünk, a Contoso MFA jogcím a jogkivonatot, előfordulhat, hogy látható a következő hibát:

* 1. napja: A felhasználó rendelkezik-e többtényezős hitelesítés Contoso és fér hozzá az App1, akkor nincs további MFA kérdés megjelenik-e a Fabrikam.

* 2. napon: A felhasználó általi App 2 futnak, így most Fabrikam elérésekor, kell regisztrálnia az MFA van.

Ez a folyamat zavaró lehet, és előfordulhat, hogy a bejelentkezési befejezésekhez dobja el.

Továbbá akkor is, ha a Contoso többtényezős hitelesítési funkció, nincs mindig az esetben a Fabrikam volna megbízik a Contoso többtényezős hitelesítési szabályzat.

Végezetül erőforrás bérlőt többtényezős Hitelesítést is működik, az msa-k és társadalombiztosítási azonosítókat és az erőforráspartner-szervezethez, amelyek nem rendelkeznek a többtényezős hitelesítés beállítása.

A javaslat a multi-factor Authentication B2B felhasználók ezért mindig megkövetelő hívja fel a bérlőben. Ez a követelmény előfordulhat, hogy bizonyos esetekben dupla MFA, de hívja fel a bérlő fér hozzá, amikor a végfelhasználói élmény kiszámítható: Sally regisztrálnia kell a multi-factor Authentication hívja fel a bérlő.

### <a name="device-based-location-based-and-risk-based-conditional-access-for-b2b-users"></a>Eszköz, helyét és kockázati-alapú feltételes hozzáférés a B2B felhasználók

Contoso lehetővé teszi, hogy a vállalati adatok eszközalapú feltételes hozzáférési házirendeket, ha hozzáférést nem lehetséges a, amelyek nem a Contoso által felügyelt és nem a Contoso eszköz házirendeknek megfelelő eszközökről.

Ha a B2B felhasználó-eszköz nem a Contoso felügyeli, a B2B felhasználókat az erőforráspartner-szervezetek az le van tiltva a abban a környezetben, ezek a házirendek érvényben vannak. Contoso azonban, hogy kizárja őket az eszközalapú feltételes hozzáférési házirend adott partner felhasználók tartalmazó kizárási listák hozhat létre.

#### <a name="location-based-conditional-access-for-b2b"></a>Feltételes hozzáférés helyalapú B2B

Helyalapú feltételes hozzáférési házirendek kényszerítheti a B2B felhasználók hívja fel a szervezet létre tudja hozni egy megbízható IP-címtartományt, amely meghatározza a fiókpartner-szervezetek esetén is.

#### <a name="risk-based-conditional-access-for-b2b"></a>Kockázati-alapú feltételes hozzáférés a B2B

Jelenleg kockázati-alapú bejelentkezési házirendek nem alkalmazható B2B felhasználók mivel a kockázat kiértékelésekor a B2B felhasználó otthoni szervezet hajtja végre.

## <a name="next-steps"></a>Következő lépések

Ismerje meg az Azure AD B2B együttműködés további cikkeit:

* [Mi az az Azure AD B2B együttműködés?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Hogyan rendszergazdák Azure Active Directory B2B együttműködés felhasználók hozzá?](active-directory-b2b-admin-add-users.md)
* [Hogyan hozzá az információkkal dolgozó szakemberek B2B együttműködés felhasználók?](active-directory-b2b-iw-add-users.md)
* [A B2B együttműködés meghívó e-mail elemei](active-directory-b2b-invitation-email.md)
* [B2B együttműködés meghívó érvényesítési](active-directory-b2b-redemption-experience.md)
* [Az Azure AD B2B együttműködés licencelés](active-directory-b2b-licensing.md)
* [Hibaelhárítás az Azure Active Directory B2B együttműködés](active-directory-b2b-troubleshooting.md)
* [Az Azure Active Directory B2B együttműködés gyakori kérdések (GYIK)](active-directory-b2b-faq.md)
* [Az Azure Active Directory B2B együttműködés API és a Testreszabás](active-directory-b2b-api.md)
* [Adja hozzá a B2B együttműködés felhasználók nélkül](active-directory-b2b-add-user-without-invite.md)
* [Alkalmazások kezelése az Azure Active Directoryban cikk indexe](active-directory-apps-index.md)
