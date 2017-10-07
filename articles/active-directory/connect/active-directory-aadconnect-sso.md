---
title: "Az Azure AD Connect: Zökkenőmentes egyszeri bejelentkezést |} Microsoft Docs"
description: "Ez a témakör ismerteti az Azure Active Directory (Azure AD) zökkenőmentes egyszeri bejelentkezést, és hogyan lehetővé teszi a akkor tooprovide valódi egyszeri bejelentkezést a vállalati hálózaton belüli vállalati asztali felhasználók."
services: active-directory
keywords: "Mi az Azure AD Connect telepítés Active Directory szükséges összetevőket az Azure AD, SSO, egyszeri bejelentkezést."
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: billmath
ms.openlocfilehash: 11c778c37ee39866dcc3a14ab648cfcd8e322e47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-seamless-single-sign-on"></a>Az Azure Active Directory zökkenőmentes egyszeri bejelentkezést.

## <a name="what-is-azure-active-directory-seamless-single-sign-on"></a>Mi az az Azure Active Directory zökkenőmentes egyszeri bejelentkezést?

Azure Active Directory zökkenőmentes egyszeri bejelentkezést (az Azure AD zökkenőmentes SSO) Ha vannak a vállalati eszközök csatlakoztatott tooyour vállalati hálózaton lévő felhasználók bejelentkezésekor automatikusan. Ha engedélyezve van, felhasználók nem kell a jelszavak toosign a tooAzure AD a tootype, és általában, akkor írja be a felhasználónevek. Ez a szolgáltatás tooyour felhőalapú alkalmazások biztosít a felhasználók egyszerűen hozzáférhetnek további helyszíni összetevők anélkül.

Zökkenőmentes SSO kombinálva vagy hello [Jelszókivonat-szinkronizálást](active-directory-aadconnectsync-implement-password-synchronization.md) vagy [áteresztő hitelesítés](active-directory-aadconnect-pass-through-authentication.md) bejelentkezési módszerek.

![Zökkenőmentes egyszeri bejelentkezést.](./media/active-directory-aadconnect-sso/sso1.png)

>[!IMPORTANT]
>Zökkenőmentes SSO jelenleg előzetes verzió. Ez a szolgáltatás _nem_ alkalmazható tooActive Directory összevonási szolgáltatások (ADFS).

## <a name="key-benefits"></a>Főbb előnyök

- *Kiváló felhasználói élmény*
  - Felhasználók automatikusan bejelentkezett mind a helyszíni és felhőalapú alkalmazásokat.
  - Felhasználó nem rendelkezik tooenter jelszavukat ismételten.
- *Egyszerű toodeploy & felügyelete*
  - További összetevők szükséges a helyszíni toomake ezt a munkát.
  - A felhőalapú hitelesítés - bármely metódusát együttműködve [Jelszókivonat-szinkronizálást](active-directory-aadconnectsync-implement-password-synchronization.md) vagy [áteresztő hitelesítés](active-directory-aadconnect-pass-through-authentication.md).
  - Is toosome vagy csoportházirend segítségével minden felhasználó ki lesz vonva.
  - Windows 10-eszközök regisztrálása nélkül hello bármely AD FS infrastruktúra Azure AD-val. Ez a funkció arra kéri, toouse verzió 2.1-es vagy újabb hello [munkahelyhez ügyfél](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="feature-highlights"></a>A szolgáltatás kiemelések

- Bejelentkezési felhasználónév vagy hello helyszíni alapértelmezett felhasználónév lehet (`userPrincipalName`) vagy az Azure AD Connect konfigurált egy másik attribútum (`Alternate ID`). Mindkét esetben a munkahelyi használható, mert a zökkenőmentes egyszeri Bejelentkezést használ hello `securityIdentifier` jogcím hello Kerberos jegy toolook hello megfelelő felhasználói objektum mentése az Azure ad-ben.
- Zökkenőmentes SSO az alkalmi szolgáltatása. Ha bármilyen okból nem sikerül, hello felhasználói bejelentkezési élmény Visszalépés tooits rendszeres viselkedés - Egytényezős, hello felhasználó számára szükséges tooenter hello bejelentkezési oldala a jelszavukat.
- Ha egy alkalmazás továbbítja a `domain_hint` (OpenID Connect) vagy `whr` (SAML) paraméter - azonosító a bérlő vagy `login_hint` paraméter - hello felhasználói azonosító az Azure AD bejelentkezési kérelem, a felhasználók automatikusan bejelentkeztetjük ezeket nem Írja be a felhasználónevet és jelszót.
- Az Azure AD Connect használatával engedélyezhető.
- Egy szabad szolgáltatást, és nem kell minden Azure AD toouse fizetős verziója azt.
- A webes webböngésző-alapú ügyfelek és a támogató Office-ügyfelek támogatott [modern hitelesítést](https://aka.ms/modernauthga) platformok és böngészők képes a Kerberos-hitelesítés:

| OS\Browser |Az Internet Explorer|Peremhálózati|Google Chrome|Mozilla Firefox|Safari|
| --- | --- |--- | --- | --- | -- 
|Windows 10|Igen|Nem|Igen|igen\*|N/A
|Windows 8.1|Igen|N/A|Igen|igen\*|N/A
|Windows 8|Igen|N/A|Igen|igen\*|N/A
|Windows 7|Igen|N/A|Igen|igen\*|N/A
|Mac OS X|N/A|N/A|igen\*|igen\*|igen\*

\*Szükséges [további konfigurációs](active-directory-aadconnect-sso-quick-start.md#browser-considerations)

>[!IMPORTANT]
>A Microsoft nemrég visszaállítása peremhálózati tooinvestigate támogatása ügyfél jelentett problémák.

>[!NOTE]
>Windows 10-es hello ajánljuk, toouse [az Azure AD Join](../active-directory-azureadjoin-overview.md) hello optimális egyszeri bejelentkezéses felhasználói élmény biztosítása az Azure AD számára.

## <a name="next-steps"></a>Következő lépések

- [**Gyors üzembe helyezési** ](active-directory-aadconnect-sso-quick-start.md) - létrehozásához, és az Azure AD zökkenőmentes SSO futtatása.
- [**Műszaki mélyreható** ](active-directory-aadconnect-sso-how-it-works.md) – Ez a funkció működésének megismerése.
- [**Gyakran ismételt kérdések** ](active-directory-aadconnect-sso-faq.md) -kérdések toofrequently válaszol.
- [**Hibaelhárítás** ](active-directory-aadconnect-troubleshoot-sso.md) -megtudhatja, hogyan tooresolve közös hello szolgáltatást érintő problémákat.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – új funkciókérések tárolásához.
