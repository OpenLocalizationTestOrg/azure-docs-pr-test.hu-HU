---
title: "Alkalmazások vonatkozó irányelvek aaaBranding |} Microsoft Docs"
description: "Egy átfogó toodeveloper készített erőforrásairól útmutató az Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: skwan
manager: mbaldwin
editor: 
ms.assetid: 72f4e464-1352-4a49-a18f-c37f58e7d5c4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: skwan
ms.custom: aaddev
ms.openlocfilehash: e43f884c736a0dcb2e6e51293962ef1e2636ad70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="branding-guidelines-for-applications"></a>Az alkalmazások arculati útmutatóját
Ebben a témakörben ismertetett hello branding irányelveket kell használnia, az Azure Active Directoryval (Azure AD) alkalmazások fejlesztése során. Ezeket az irányelveket az ügyfelek közvetlen, ha szeretnék toouse a munkahelyi vagy iskolai fiókját, kezeli az Azure ad-ben, vagy a személyes fiókra regisztráció és bejelentkezés tooyour alkalmazás segítségével.

## <a name="personal-accounts-vs-work-or-school-accounts-from-microsoft"></a>Személyes fiókokat és a munkahelyi vagy iskolai fiókjait a Microsoft
A Microsoft kezeli a felhasználói fiókok kétféle:

* **Személyes fiókok** (korábbi nevén Windows Live ID). Ezek a fiókok hello kapcsolatát képviseli *egyedi* felhasználók és a Microsoft, használják őket tooaccess fogyasztói eszközök és a Microsoft-szolgáltatásokat. Ezek a fiókok személyes használatra lettek tervezve.
* **Munkahelyi vagy iskolai fiókját.** Ezeket a fiókokat a Microsoft által felügyelt Azure Active Directory címtárszolgáltatást használó szervezeteknek nevében. Ezek a fiókok tooOffice 365 és a Microsoft más üzleti szolgáltatásaiban használt toosign.

Munkahelyi vagy iskolai fiókok használata általában Microsoft tooend felhasználók (az alkalmazottak, a diákok, szövetségi alkalmazottak) kiosztotta a vállalatuk (munkahelyi, iskolai, kormányzati Ügynökség). Ezek a fiókok, vagy közvetlenül a hello felhő hello Azure AD platform, vagy egy helyszíni címtár, például Windows Server Active Directory a szinkronizált tooAzure AD értékűre. A Microsoft hello *konfigurációelem gondnoka képes legyen* hello munkahelyi vagy iskolai fiókjait, de hello fiókok tulajdonában lévő és hello szervezet vezérli.

## <a name="referring-tooazure-ad-accounts-in-your-application"></a>Az alkalmazás hivatkozó tooAzure AD-fiókok
A Microsoft nem teszi közzé végfelhasználók toohello Azure vagy hello Active Directory védjegyek, és nem kell azt.

* Ha felhasználó jelentkezik be, hello szervezet nevét és a lehető legnagyobb mértékben embléma kell használnia. Ez jobb, mint például a "a szervezet" általános kifejezések használatával.
* Felhasználó nem jelentkezik be, amikor tootheir fiókok, olvassa el "munkahelyi vagy iskolai fiók" és a használata hello Microsoft embléma tooconvey, hogy ezeket a fiókokat a Microsoft által felügyelt. Ne használja a "vállalati fiók", "üzleti fiók" vagy "vállalati fiók" feltételek, amelyek felhasználói zavart okozhat.

## <a name="user-account-pictogram"></a>Felhasználói fiók piktogramot
Ezeket az irányelveket korábbi verzióiban javasoljuk, használja a "kék jelvény" piktogramot. Felhasználói és a fejlesztői visszajelzései alapján, most ajánlott hello Microsoft embléma hello használja helyette. Ez segít megérteni, hogy az Office 365 vagy egyéb Microsoft üzleti szolgáltatások toosign tooyour alkalmazásban használata hello fiók felhasználhatja a felhasználóknak.

## <a name="signing-up-and-signing-in-with-azure-ad"></a>Iratkozik fel, és az Azure AD bejelentkezés
Az alkalmazás-előfizetési és a bejelentkezési külön elérési jelenthet, és a következő szakaszok hello visual útmutatás nyújtása a mindkét forgatókönyvet.

**Ha az alkalmazás támogatja a végfelhasználó regisztrációt (pl.: szabad tootrial vagy freemium modell)**: megjelenítheti a **bejelentkezési** gomb, amely lehetővé teszi, hogy a felhasználók tooaccess az alkalmazás a munkahelyi fiókkal vagy személyes fiókkal. Az Azure AD egy hozzájárulás kérése hello elérésekor az alkalmazás első alkalommal megjelenik.

**Ha az alkalmazás, amely csak a rendszergazdák is hozzájárul engedélyekkel kell rendelkeznie, vagy ha az alkalmazás használatához szervezeti licencelési**: a felhasználói bejelentkezési admin beszerzési kell egymástól. Hello **"beolvasása az alkalmazás" gomb** a program átirányítja a rendszergazdák toosign, majd kérje meg a szervezetben lévő felhasználók nevében toogrant jóváhagyását. Ez rendelkezik hello letilthatja a végfelhasználók beleegyezést kér tooyour app igénybe.

## <a name="visual-guidance-for-app-acquisition"></a>Visual-útmutató a alkalmazás beszerzése
A "hello app get" hivatkozásra irányíthatja át hello felhasználói toohello az Azure AD-hozzáférés (engedélyezése) lap, tooallow egy szervezet rendszergazdája tooauthorize az alkalmazás toohave adatelérés tootheir szervezete a Microsoft által futtatott. Hogyan toorequest hozzáférés hello esik szó részleteinek [alkalmazások integrálása az Azure Active Directoryval](active-directory-integrating-applications.md) cikk.

Után a rendszergazdák hozzájárulás tooyour alkalmazást, akkor választhat, tooadd azt tootheir felhasználók Office 365 felhasználói indítója élményt (érhető el, amelyek hello waffle, vagy olyan [https://portal.office.com/myapps](https://portal.office.com/myapps)). Ha tooadvertise ezt a képességet, használhatja a feltételek, például a "Hozzáadás ezen alkalmazás tooyour szervezet", és egy ehhez hasonló gomb megjelenítése:

![Alkalmazástípusok és forgatókönyvek](./media/active-directory-branding-guidelines/add-to-my-org.png)

Azonban azt javasoljuk, hogy ahelyett, hogy a gombok magyarázó szöveget írni. Példa:

> *Ha már az Office 365 vagy a Microsoft egyéb üzleti szolgáltatás, egyszerűen < your_app_name > hozzáférési tooyour szervezeti adatok megadásához. Ez lehetővé teszi a felhasználók tooaccess < your_app_name > meglévő munkahelyi fiókkal.*
> 
> 

## <a name="visual-guidance-for-sign-in"></a>Visual-útmutató a bejelentkezés
Az alkalmazás a bejelentkezési gomb, amely átirányítja a felhasználókat toohello bejelentkezési végpont, amely megfelel a toohello protokoll toointegrate használhatja az Azure AD meg. a következő szakasz hello részleteit mi gombot hasonlóan kell kinéznie.

### <a name="pictogram-and-sign-in-with-microsoft"></a>Piktogramot és az "Jelentkezzen be Microsoft"
Azt a hello társítás, többek között a más identitásszolgáltatók, az alkalmazás esetleg támogatja az Azure AD egyedileg jelölő hello Microsoft embléma és hello "Sign in with Microsoft" feltételeket. Ha nincs elég hely a "Sign in with Microsoft", akkor ok tooshorten azt túl "bejelentkezés".

![Alkalmazástípusok és forgatókönyvek](./media/active-directory-branding-guidelines/sign-in-with-microsoft-light.png)

![Alkalmazástípusok és forgatókönyvek](./media/active-directory-branding-guidelines/sign-in-light.png)

Sötét színsémát hello gombok is használható.

![Alkalmazástípusok és forgatókönyvek](./media/active-directory-branding-guidelines/sign-in-with-microsoft-dark.png)

![Alkalmazástípusok és forgatókönyvek](./media/active-directory-branding-guidelines/sign-in-dark.png)

## <a name="branding-dos-and-donts"></a>Márkajelzési helyes, és nem ajánlott műveletek
**Tegye** használata "munkahelyi vagy iskolai fiók" hello "Sign in with Microsoft" gomb tooprovide további magyarázat toohelp végfelhasználók együtt ismeri fel, hogy használhatják azt. **Nem** használjon más feltételek, például a "vállalati fiók", "üzleti fiók" vagy "vállalati fiók."

**Nem** "Office 365 ID" vagy "Azure ID". Az Office 365 esetén is hello felhasználóinak ajánlat a Microsoft, amely nem használja az Azure AD-hitelesítéshez.

**Nem** alter hello Microsoft embléma.

**Nem** elérhetővé tenni a végfelhasználók toohello Azure vagy az Active Directory márkákat. Ez így megfelelő azonban toouse ezeket a feltételeket a fejlesztők számára, az informatikai szakemberek és a rendszergazdák.

## <a name="navigation-dos-and-donts"></a>Navigációs javaslatok és nem ajánlott műveletek
**Tegye** teszik lehetővé a felhasználók toosign ki, és váltson tooanother felhasználói fiókot. Amíg a legtöbben ugyanazt a személyes fiókot a Microsoft/Facebook/Google/Twitter, személyek társított gyakran több szervezet. Több bejelentkezett felhasználó támogatása hamarosan elérhető.

