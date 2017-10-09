---
title: "az Azure AD aaaSharing fiókok |} Microsoft Docs"
description: "Ismerteti, hogyan Azure Active Directory lehetővé teszi, hogy a szervezetek toosecurely megosztás fiókok a helyszíni alkalmazások és a fogyasztói felhőszolgáltatások."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e2d77104-d978-46a3-bfea-03ffdf3b61e6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 9f98bfa97a6c9ba1566d3f921c1b676d5f3c2a88
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sharing-accounts-with-azure-ad"></a>Fiókok megosztása az Azure ad szolgáltatással
## <a name="overview"></a>Áttekintés
Egyes esetekben a szervezetek kell toouse egyetlen felhasználónév és jelszó számára több. Ez általában akkor fordul elő két esetben:

* Ha mindegyik felhasználóhoz egyedi felhasználónevét és jelszavát igénylő alkalmazásokhoz való hozzáférés, hogy a helyszíni alkalmazások vagy a felhasználó a felhőalapú szolgáltatások (pl. vállalati közösségi fiókok).
* Többfelhasználós környezet létrehozásakor. Előfordulhat, hogy emelt szintű jogosultságokkal egyetlen, a helyi fiókkal rendelkezik, és lehet használt toodo core telepítés, a felügyelet és a helyreállítási tevékenységeket (pl. hello helyi "globális rendszergazda" fiók az Office 365-öt vagy hello root fiókjának a Salesforce-ban).

Hagyományosan ezek a fiókok volna közösen terjesztése hello hitelesítő adatok (felhasználónév/jelszó) toohello jobb egyéni felhasználók számára, vagy tárolja őket egy megosztott hely, ahol több megbízható ügynökök érhetik el azokat.

hello hagyományos megosztási modellnek több hátrányai:

* Hozzáférés toonew alkalmazások engedélyezése szükséges toodistribute hitelesítő adatok tooeveryone, amely engedéllyel kell rendelkeznie.
* Minden megosztott alkalmazás a saját egyedi készletének megosztott hitelesítő adatokat igénylő felhasználók tooremember több hitelesítőadat-készletek lehet szükség. Ha a felhasználók tooremember sok hitelesítő adatokat, hello kockázati növeli a, hogy azok fog igénybe toorisky eljárásokat. (pl. írás a jelszavak le).
* Nem sikerült megállapítani, kik access tooan alkalmazást.
* Nem sikerült megállapítani a kik *elért* kérelmet.
* Amennyiben tooremove access tooan alkalmazás van szüksége, tooupdate hello hitelesítő adatokat, és újra ossza ki őket tooeveryone elérő toothat alkalmazás van szüksége.

## <a name="azure-active-directory-account-sharing"></a>Az Azure Active Directory fiók megosztása
Az Azure AD egy új megközelítését ismerteti, amely kiküszöböli a hátrányai megosztott toousing fiókok.

hello Azure AD rendszergazda úgy konfigurálja, hogy mely alkalmazásokat, egy felhasználó hozzáférhessen a hozzáférési Panel hello használatával, és az egyszeri bejelentkezés az alkalmazás számára leginkább megfelelő hello kiválasztása. Egy adott típusú *jelszó-alapú egyszeri bejelentkezést*, lehetővé teszi, hogy az Azure AD egy "broker" típusú összekötőként az adott alkalmazáshoz hello bejelentkezési folyamat során.

Felhasználói bejelentkezés egyszer a szervezeti fiókjukkal. Ez a hello ugyanazt a fiókot, hogy rendszeresen használata tooaccess az asztalon vagy e-mailt. Ugyanakkor felderítése és hozzáférési csak ezeket az alkalmazásokat, amelyek vannak rendelve. A megosztott-fiókokkal ebben a listában, az alkalmazások tartalmazhatnak tetszőleges számú megosztott hitelesítési adatok. hello végfelhasználói nem kell tooremember, vagy írja fel hello különböző fiókok használják.

Megosztott fiókok csak nem növeli a felügyeletet, valamint javítja a használhatóság, akkor is a megfelelő biztonság érdekében. Engedélyek toouse hello hitelesítő adatokkal rendelkező felhasználók hello megosztott jelszó nem látható, de ahelyett, hogy egy vezénylését hitelesítési folyamat részeként a engedélyek toouse hello jelszó beolvasása. Emellett egyes jelszó SSO alkalmazások, az informatikai részleg hello beállítás toohave az Azure AD rendszeres időközönként (frissítés) helyettesítő hello jelszó használatával nagy, összetett jelszót, hello fiók biztonsági növelését. hello a rendszergazdák egyszerűen adja meg vagy access tooan alkalmazás visszavonása és is tudja, akik hozzáférést toohello fiókkal rendelkezik, és ki fért hozzá az elmúlt hello.

Az Azure AD támogatja a megosztott fiókokat a bármely nagyvállalati mobilitási csomag (EMS), az alkalmazások aláírásához prémium vagy alapszintű licenccel rendelkező felhasználók, az egyszeri jelszó összes típusa. Hozzáadhatja a saját jelszó hitelesítése alkalmazás pedig bármelyik több ezer előre integrált alkalmazások hello alkalmazáskatalógusában fiókok megosztott [egyéni SSO alkalmazások](active-directory-sso-integrate-saas-apps.md).

Az Azure AD-funkciókat, amelyek lehetővé teszik a fiók megosztása a következők:

* [Jelszó egyszeri bejelentkezést.](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
* Jelszó egyszeri bejelentkezés ügynök
* [Csoport-hozzárendelés](active-directory-accessmanagement-self-service-group-management.md)
* Egyéni jelszó alkalmazások
* [Alkalmazás használati irányítópult/jelentések](active-directory-passwords-get-insights.md)
* Felhasználói hozzáférés portálok
* [Alkalmazás proxy](active-directory-application-proxy-get-started.md)
* [Active Directory Marketplace-ről](https://azure.microsoft.com/marketplace/active-directory/all/)

## <a name="sharing-an-account"></a>Egy fiók megosztása
az Azure AD toouse tooshare fiók szüksége lesz:

* Alkalmazás hozzáadása [alkalmazásgyűjtemény](https://azure.microsoft.com/marketplace/active-directory/) vagy [egyéni alkalmazás](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)
* Állítsa be alkalmazását hello jelszó egyszeri bejelentkezés (SSO)
* Használjon [csoport-alapú hozzárendelés](active-directory-accessmanagement-group-saasapps.md) és select hello beállítás tooenter megosztott hitelesítő adatokat
* Választható lehetőség: az egyes alkalmazások, például a Facebook, a Twitter és a LinkedIn, engedélyezheti hello beállítása [az Azure AD automatikus jelszó során](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx)

Végezhet is megosztott fiókja biztonságát erősítik a multi-factor Authentication (MFA) (További információ [biztonságossá tétele az alkalmazások az Azure ad-val](../multi-factor-authentication/multi-factor-authentication-get-started.md)) és delegálhatja hello képességét toomanage rendelkező hozzáférés toohello alkalmazás használatával [Az azure AD-önkiszolgáló](active-directory-accessmanagement-self-service-group-management.md) felügyeleti csoportban.

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Feltételes hozzáféréssel rendelkező alkalmazások védelme](active-directory-conditional-access.md)
* [Önkiszolgáló csoportkezelési felügyeleti/SSAA](active-directory-accessmanagement-self-service-group-management.md)

