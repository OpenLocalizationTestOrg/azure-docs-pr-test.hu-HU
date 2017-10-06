---
title: "Azure Active Directory B2B együttműködés felhasználók hozzáférésének aaaConditional |} Microsoft Docs"
description: "Az Azure Active Directory B2B együttműködés többtényezős hitelesítés (MFA) támogatja a szelektív hozzáférést tooyour vállalati alkalmazások"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/24/2017
ms.author: sasubram
ms.openlocfilehash: 3a05be4393f74ff8e87f32432a222a5fbac9af62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="conditional-access-for-b2b-collaboration-users"></a>Feltételes hozzáférés a B2B együttműködés felhasználók

## <a name="multi-factor-authentication-for-b2b-users"></a>A B2B felhasználók a többtényezős hitelesítést
Az Azure AD B2B együttműködés a szervezetek kényszerítheti a többtényezős hitelesítés (MFA) házirendek a B2B felhasználók számára. Ezek a házirendek kényszerítheti a hello bérlői, alkalmazás vagy egyéni felhasználói szintjén, hello azonos módon, hogy a teljes munkaidejű alkalmazottak és a szervezet hello engedélyezve vannak. Többtényezős hitelesítési házirendek érvényben vannak hello erőforrás-szervezetben.

Példa:
1. A vállalat rendszergazdai vagy információk munkavégző felkéri vállalati B tooan alkalmazás felhasználói *PEL* vállalat azonosítójához.
2. Alkalmazás *PEL* vállalat A konfigurált toorequire MFA hozzáférést.
3. Amikor a vállalat B hello felhasználó kísérli meg tooaccess app *PEL* hello vállalati A bérlőt, a rendszer kéri toocomplete az MFA-kérdést.
4. hello felhasználó állíthat be az MFA legyenek A vállalat, majd a többtényezős hitelesítés lehetőséget.
5. Ebben a forgatókönyvben minden identitás működik (az Azure AD vagy az msa-t, például, ha a felhasználók a vállalati B azonosítania társadalombiztosítási azonosító)
6. A vállalat elegendő az Azure AD Premium licenc, amely támogatja a többtényezős Hitelesítést kell rendelkeznie. hello felhasználói vállalat B igényel, a vállalat A. licenc

hello hívja fel bérleti feladata mindig a multi-factor Authentication hello erőforráspartner szervezetnél, a felhasználók akkor is, ha a fiókpartner-szervezet hello MFA képességekkel rendelkezik.

### <a name="setting-up-mfa-for-b2b-collaboration-users"></a>Többtényezős hitelesítés beállítása B2B együttműködés felhasználók
milyen egyszerűen B2B együttműködés a felhasználóknak többtényezős hitelesítés be tooset toodiscover lásd: hogyan a következő videó hello:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-conditional-access-setup/Player]

### <a name="b2b-users-mfa-experience-for-offer-redemption"></a>Többtényezős hitelesítés élményt a B2B felhasználók kínálnak érvényesítési
Tekintse meg a következő animáció toosee hello érvényesítési élmény hello:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/MFA-redemption/Player]

### <a name="mfa-reset-for-b2b-collaboration-users"></a>B2B együttműködés felhasználóknak az új MFA
Jelenleg Üdvözöljük a rendszergazdákat a jelszó használata kötelezővé B2B együttműködés felhasználók tooproof be újra csak hello a következő PowerShell-parancsmagok használatával:

1. TooAzure AD Connect

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
  PS C:\Users\tjwasserGet-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```

3. Hello többtényezős hitelesítési módszer egy adott felhasználó toorequire hello B2B együttműködés felhasználói tooset igazolása felfelé módszerek újra átállítani. Példa:

  ```
  Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName gsamoogle_gmail.com#EXT#@ WoodGroveAzureAD.onmicrosoft.com
  ```

### <a name="why-do-we-perform-mfa-at-hello-resource-tenancy"></a>Miért most elvégezni az MFA a hello erőforrás bérleti:

Hello jelenlegi kiadásban MFA mindig van hello erőforrás bérlőhöz, kiszámíthatóságot miatt. Például tegyük fel, a Contoso a felhasználó (Sally) meghívott tooFabrikam és Fabrikam engedélyezte a többtényezős hitelesítés B2B felhasználók számára.

Ha Contoso többtényezős hitelesítési szabályzat az App1 számítógépen, de nem App2 engedélyezve van, majd úgy tekintünk hello Contoso MFA jogcím hello tokent, ha azt tapasztalhatja hello probléma a következő:

* 1. napja: A felhasználó rendelkezik-e többtényezős hitelesítés Contoso és fér hozzá az App1, akkor nincs további MFA kérdés megjelenik-e a Fabrikam.

* 2. napon: hello felhasználó általi App 2 futnak, így most Fabrikam elérésekor, kell regisztrálnia az MFA van.

Ez a folyamat zavaró lehet, és a bejelentkezési befejezésekhez toodrop vezethet.

Továbbá akkor is, ha a Contoso többtényezős hitelesítési funkció, nincs mindig hello eset hello Fabrikam volna megbízható hello Contoso többtényezős hitelesítési szabályzat.

Végezetül erőforrás bérlőt többtényezős Hitelesítést is működik, az msa-k és társadalombiztosítási azonosítókat és az erőforráspartner-szervezethez, amelyek nem rendelkeznek a többtényezős hitelesítés beállítása.

Hello ajánlott az MFA szolgáltatásra B2B felhasználók ezért tooalways megkövetelő a bérlő meghívása hello. Ez a követelmény toodouble MFA bizonyos esetekben előfordulhat, de hello hívja fel bérlői fér hozzá, amikor hello végfelhasználói élmény kiszámítható: Sally regisztrálnia kell a multi-factor Authentication hello hívja fel bérlő.

### <a name="device-based-location-based-and-risk-based-conditional-access-for-b2b-users"></a>Eszköz, helyét és kockázati-alapú feltételes hozzáférés a B2B felhasználók

Contoso lehetővé teszi, hogy a vállalati adatok eszközalapú feltételes hozzáférési házirendeket, amikor a rendszer megakadályozza hozzáférési és hello Contoso eszköz házirendeknek nem megfelelő, a Contoso nem kezelt eszközök.

Ha hello B2B felhasználó-eszköz nem a Contoso felügyeli, a B2B felhasználókat az erőforráspartner-szervezetek hello le van tiltva a abban a környezetben, ezek a házirendek érvényben vannak. Contoso azonban adott partner felhasználók tooexclude tartalmazó listák azokat hello eszközalapú feltételes hozzáférési házirend kizárási hozhat létre.

#### <a name="location-based-conditional-access-for-b2b"></a>Feltételes hozzáférés helyalapú B2B

Helyalapú feltételes hozzáférési szabályzatokat a B2B felhasználók kényszeríthető, ha hello hívja fel szervezet képes toocreate egy megbízható IP-címtartományt, amely meghatározza a fiókpartner-szervezetek.

#### <a name="risk-based-conditional-access-for-b2b"></a>Kockázati-alapú feltételes hozzáférés a B2B

Jelenleg kockázati-alapú bejelentkezési házirendek nem lehet alkalmazott tooB2B felhasználók, mert hello B2B felhasználó otthoni szervezet hello kockázat kiértékelésekor történik.

## <a name="next-steps"></a>Következő lépések

Ismerje meg az Azure AD B2B együttműködés további cikkeit:

* [Mi az az Azure AD B2B együttműködés?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Hogyan rendszergazdák Azure Active Directory B2B együttműködés felhasználók hozzá?](active-directory-b2b-admin-add-users.md)
* [Hogyan hozzá az információkkal dolgozó szakemberek B2B együttműködés felhasználók?](active-directory-b2b-iw-add-users.md)
* [hello B2B együttműködés meghívó e-mail hello elemei](active-directory-b2b-invitation-email.md)
* [B2B együttműködés meghívó érvényesítési](active-directory-b2b-redemption-experience.md)
* [Az Azure AD B2B együttműködés licencelés](active-directory-b2b-licensing.md)
* [Hibaelhárítás az Azure Active Directory B2B együttműködés](active-directory-b2b-troubleshooting.md)
* [Az Azure Active Directory B2B együttműködés gyakori kérdések (GYIK)](active-directory-b2b-faq.md)
* [Az Azure Active Directory B2B együttműködés API és a Testreszabás](active-directory-b2b-api.md)
* [Adja hozzá a B2B együttműködés felhasználók nélkül](active-directory-b2b-add-user-without-invite.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
