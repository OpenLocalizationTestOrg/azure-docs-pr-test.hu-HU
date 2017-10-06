---
title: "aaaGet lépések az Azure AD integrálása az alkalmazásokkal |} Microsoft Docs"
description: "Ez a cikk egy első lépésekről szóló útmutatót az Azure Active Directory (AD) integrálása a helyszíni alkalmazások és a felhőalapú alkalmazásokhoz."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: db6d210d-c970-49e9-bd20-36d984bcd1c3
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: asteen
ms.openlocfilehash: 5a7a851e8418083fee72ab58477a9cab75d0d4bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>Azure Active Directory integrálása alkalmazások első lépések útmutató
## <a name="overview"></a>Áttekintés
Ez a témakör megfelelő toogive, az alkalmazások az Azure Active Directory (AD) integrálása a terv. Egyes hello lentebbi tartalmaz részletesebb témakör röviden ismerteti, hogy melyik az első lépésekről szóló útmutatót részei vonatkozó tooyou.  Mélyebb bemutatója hello hivatkozások követése egyes témák.

## <a name="before-you-begin-take-inventory"></a>Mielőtt elkezdené, leltározása
De ugorhat toointegrating alkalmazásokban az Azure ad-vel, mielőtt fontos tooknow is, ahol Ön, és amennyiben azt szeretné, hogy toogo.  hello alábbi kérdéseket tervezett toohelp, gondolja át az Azure AD-integrációs projektet.

### <a name="application-inventory"></a>Alkalmazásleltár
* Hol találhatók az alkalmazásokat? Azok ki tulajdonosa?
* Milyen típusú hitelesítés van szükség az alkalmazások?
* Ki kell toowhich alkalmazásokat?
* Szeretné toodeploy egy új alkalmazást?
  * Fog összeállítani, belső fejlesztésű-telepítheti az Azure compute példányhoz?
  * Fog használni, amely elérhető hello Azure Application Gallery?

### <a name="user-and-group-inventory"></a>Felhasználó- és szoftverleltár
* Hol találhatók a felhasználói fiókjait?
  * Helyszíni Active Directory
  * Azure AD
  * Egy különálló alkalmazás adatbázis saját belül
  * Jóvá nem hagyott alkalmazások
  * Az összes fenti hello
* Milyen engedélyeket és a szerepkör-hozzárendelések egyes felhasználók jelenleg rendelkeznek? Szüksége van tooreview hozzáférésüket vagy biztos, hogy a felhasználói hozzáférés és a szerepkör-hozzárendelések is megfelelő most?
* Csoportok már meghatározott a helyszíni Active Directoryban?
  * Hogyan vannak rendszerezve a csoportok?
  * Kik tartoznak hello csoporttagok?
  * Milyen engedélyekkel/szerepkör-hozzárendelések hello csoportok jelenleg rendelkeznek?
* Szükség lesz-felhasználó vagy csoport adatbázisok tooclean integrálása előtt?  (Ez a közérthető fontos kérdésre. Szemétgyűjtési a szemétgyűjtési ki.)

### <a name="access-management-inventory"></a>Access felügyeleti készlet
* Hogyan jelenleg felügyel felhasználói hozzáférés tooapplications? Nem, amelyeknek szükségük van toochange?  Akkor tekintette egyéb módjai toomanage hozzáférés, többek között a [RBAC](role-based-access-control-configure.md) például?
* Hozzáférés toowhat kell?

Lehet, hogy nincs hello válaszok tooall ezeket a kérdéseket előre, de ez nem probléma.  Ez az útmutató segítséget nyújt válaszolja meg a fenti kérdések közül, és néhány kérdésekre vonatkozó döntések meghozatalában.

## <a name="prerequisites"></a>Előfeltételek
* Azure-előfizetés és Azure Active Directory címtárhoz.  Ha még nem rendelkezik Azure-előfizetéssel, kipróbálhatja Azure ingyenes 30 napig. [Próbálja ki!](https://azure.microsoft.com/trial/get-started-active-directory/)

## <a name="application-integration-with-azure-ad"></a>Alkalmazások integrálása az Azure ad szolgáltatással
### <a name="finding-unsanctioned-cloud-applications-with-cloud-app-discovery"></a>A Cloud App Discovery megállapítás nem engedélyezett a felhőalapú alkalmazásokhoz
Fent említett, alkalmazásokat, amelyek még nem lett kezeli a szervezet eddig lehet.  Hello hardverleltározási folyamatának részeként is lehetséges toofind nem engedélyezett a felhőalapú alkalmazásokhoz. Lásd: [jóvá nem hagyott alkalmazások a Cloud App Discovery keresése](active-directory-cloudappdiscovery-whatis.md).

### <a name="authentication-types"></a>Hitelesítési típusok
Az alkalmazások előfordulhat, hogy különböző hitelesítési követelményekkel rendelkező. Az Azure AD aláíró tanúsítványok használható SAML 2.0, WS-Federation, vagy OpenID Connect protokollok, valamint jelszó egyszeri bejelentkezést használó alkalmazásokat. Alkalmazással kapcsolatos további információk az Azure AD hitelesítési típus: [tanúsítványok kezelése az összevont egyszeri bejelentkezés az Azure Active Directoryban](active-directory-sso-certs.md) és [jelszó alapján egyszeri bejelentkezési](active-directory-appssoaccess-whatis.md).

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>Az Azure AD alkalmazás Proxy egyszeri bejelentkezés engedélyezése
A Microsoft Azure AD-alkalmazásproxy megadhatja a hozzáférés tooapplications található a magánhálózaton belülről biztonságosan, bárhonnan és bármilyen eszközről. Miután telepítette az alkalmazásproxy-összekötő a környezetben, könnyen beállítható az Azure ad-val.

### <a name="integrating-applications-with-azure-ad"></a>Alkalmazások integrálása az Azure ad szolgáltatással
hello alábbi cikkekben hello különböző módokon alkalmazások integrálása az Azure ad-val, és néhány útmutatást nyújtanak.

* [Mely az Active Directory toouse meghatározása](active-directory-administer.md)
* [Alkalmazások használata az hello Azure alkalmazáskatalógusában](active-directory-appssoaccess-whatis.md)
* [Integrálása SaaS-alkalmazások oktatóanyagok listáját](active-directory-saas-tutorial-list.md)

## <a name="managing-access-tooapplications"></a>Hozzáférés tooapplications kezelése
hello következő cikkek ismertetik, hogyan kezelheti a hozzáférést tooapplications után az Azure AD-összekötők használata az Azure AD és az Azure AD integrált.

* [Az Azure AD hozzáférési tooapps kezelése](active-directory-managing-access-to-apps.md)
* [Az Azure AD-összekötők automatizálása](active-directory-saas-app-provisioning.md)
* [Felhasználók hozzárendelése tooan alkalmazás](active-directory-applications-guiding-developers-assigning-users.md)
* [Csoportok tooan alkalmazás hozzárendelése](active-directory-applications-guiding-developers-assigning-groups.md)
* [Fiókok megosztása](active-directory-sharing-accounts.md)

## <a name="integrating-custom-applications"></a>Egyéni alkalmazások integrálása
Ha új alkalmazások írása, és tooassist fejlesztők a hello power az Azure AD kihasználva, lásd: [Guiding fejlesztők](active-directory-applications-guiding-developers-for-lob-applications.md).

Ha azt szeretné, tooadd az egyéni alkalmazás toohello Azure Application Gallery, lásd: ["Állapotba az alkalmazásba" Azure AD önkiszolgáló SAML konfigurációval](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

## <a name="see-also"></a>Lásd még:
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)

