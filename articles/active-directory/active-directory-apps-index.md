---
title: "az Alkalmazáskezelés az Azure Active Directoryban Index aaaArticle |} A Microsoft Azure"
description: "Ismerje meg, hogyan toocustomize hello lejárati dátum összevonási tanúsítványait, és hogyan toorenew tanúsítványokat, amelyeket hamarosan lejár."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 5321b8e4-2afa-4dfe-8d53-4add7abb5ec8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: asteen
ms.openlocfilehash: f8a584baa94dc50e279899074f50160978256559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="article-index-for-application-management-in-azure-active-directory"></a>Article Index for Application Management in Azure Active Directory (Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke)
Ezen a lapon minden egyes dokumentum írása hello kapcsolatos különféle-alkalmazásokhoz kapcsolódó szolgáltatások az Azure Active Directory (Azure AD) átfogó listáját tartalmazza.

Nincs, a rövid bevezető tooeach fő szolgáltatásterület, valamint a mely cikkeket tooread attól függően, hogy milyen információkat keres útmutatást.

## <a name="overview-articles"></a>Cikkek áttekintése
az alábbi hello cikkek megfelelőek kiindulási pontként, akik egyszerűen szeretné az Azure AD alkalmazás-felügyeleti funkciókat rövid leírása.

| Útmutató a következő cikket: |  |
|:---:| --- |
| Egy bevezető toohello alkalmazás felügyelettel kapcsolatos problémák, amelyek az Azure AD megoldja |[Alkalmazások kezelése az Azure Active Directory (AD)](active-directory-enable-sso-scenario.md) |
| Hello áttekintése az Azure AD különböző funkcióival kapcsolatos tooenabling egyszeri bejelentkezést, kik meghatározása tooapps, és hogyan indítsa el a felhasználók az alkalmazások eléréséhez |[Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban](active-directory-appssoaccess-whatis.md) |
| Hello különböző lépéseit Ha alkalmazások integrálása az Azure AD meg |[Az Azure Active Directory integrálása alkalmazások](active-directory-integrating-applications-getting-started.md)<br /><br />[Egyszeri bejelentkezés tooSaaS alkalmazások engedélyezése](active-directory-sso-integrate-saas-apps.md)<br /><br />[Hozzáférés tooApps kezelése](active-directory-managing-access-to-apps.md) |
| Hogyan vannak megadva az alkalmazások az Azure AD technikai magyarázata |[Útmutató és érvek alkalmazások felvétele AD tooAzure](active-directory-how-applications-are-added.md) |

## <a name="troubleshooting-articles"></a>Hibaelhárítási cikkek
Ez a szakasz a toorelevant hibaelhárítási útmutatók gyors hozzáférést biztosít. További információ az egyes szolgáltatási terület hello részeinek ezen a lapon található.

| Szolgáltatási terület |  |
|:---:| --- |
| Összevont egyszeri bejelentkezést. |[Hibaelhárítási SAML-alapú egyszeri bejelentkezést.](active-directory-saml-debugging.md) |
| Jelszó-alapú egyszeri bejelentkezést. |[Az Internet Explorer hello hozzáférési Panel bővítmény hibaelhárítása](active-directory-saas-ie-troubleshooting.md) |
| Application Proxy |[Alkalmazás Proxy hibaelhárítási útmutatója](active-directory-application-proxy-troubleshoot.md) |
| Egyszeri bejelentkezés közötti helyszíni AD és az Azure AD |[A jelszó-szinkronizálás hibaelhárítása](connect/active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization)<br /><br />[A Jelszóvisszaírás hibaelhárítása](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) |
| Dinamikus csoporttagság |[Dinamikus csoporttagság hibaelhárítása](active-directory-accessmanagement-troubleshooting.md) |

## <a name="single-sign-on-sso"></a>Egyszeri bejelentkezés (SSO)
### <a name="federated-single-sign-on-sign-into-many-apps-using-one-identity"></a>Összevont egyszeri bejelentkezés: Jelentkezzen be a egy identitásával sok alkalmazás
Egyszeri bejelentkezés lehetővé teszi felhasználók tooaccess a különböző alkalmazások és szolgáltatások csak egy hitelesítő adatainak használatával. Összevonási a, amelyekkel engedélyezheti az egyszeri bejelentkezés módszerrel. Ha a felhasználó toosign összevont alkalmazásokba, átirányított tootheir szervezet hivatalos bejelentkezési oldal Azure Active Directoryban, és azok majd átirányított hátsó toohello alkalmazás sikeres hitelesítés után által megjelenített fogja használni.

| Útmutató a következő cikket: |  |
|:---:| --- |
| Egy bevezető toofederation és más bejelentkezés |[Egyszeri bejelentkezés az Azure ad szolgáltatással](active-directory-appssoaccess-whatis.md) |
| Az Azure AD-val előre integrált Szolgáltatottszoftver-alkalmazásoknál ezer egyszerűsített egyszeri bejelentkezés konfigurációs lépések |[Hello Azure AD application gallery első lépések](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery)<br /><br />[Összevonási támogató előre integrált alkalmazások teljes listája](http://aka.ms/aadfederatedapps)<br /><br />[Hogyan tooAdd Your App toohello Azure AD-Alkalmazásgyűjtemény](active-directory-app-gallery-listing.md) |
| Hogyan tooconfigure egyszeri bejelentkezés alkalmazásokhoz, mint a több mint 150 app oktatóanyagok [Salesforce](active-directory-saas-salesforce-tutorial.md), [ServiceNow](active-directory-saas-servicenow-tutorial.md), [Google Apps](active-directory-saas-google-apps-tutorial.md), [Workday](active-directory-saas-workday-tutorial.md), és sok más |[Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md) |
| Hogyan toomanually beállítása és egyszeri bejelentkezés konfiguráció testreszabása |[Hogyan tooConfigure tooApps összevont egyszeri bejelentkezést, amelyek nem találhatók hello Azure Active Directory Alkalmazáskatalógusában](active-directory-saas-custom-apps.md)<br /><br />[Hogyan tooCustomize jogcím a kiállított hello SAML-jogkivonat Pre-Integrated alkalmazások](active-directory-saml-claims-customization.md) |
| Hibaelhárítási útmutató hello SAML protokoll használó összevont alkalmazásokhoz |[Hibaelhárítási SAML-alapú egyszeri bejelentkezést.](active-directory-saml-debugging.md) |
| Hogyan tooconfigure az alkalmazás tanúsítvány lejárati dátumát, és hogyan toorenew tanúsítványai |[Tanúsítványok kezelése az összevont egyszeri bejelentkezés az Azure Active Directoryban](active-directory-sso-certs.md) |

Összevont egyszeri bejelentkezést érhető el az Azure ad-val tooten alkalmazások minden felhasználóhoz be minden kiadása. [Prémium szintű Azure AD](https://azure.microsoft.com/pricing/details/active-directory/) korlátlan alkalmazásokat támogatja. Ha a szervezete [Azure AD alapvető](https://azure.microsoft.com/pricing/details/active-directory/) vagy [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), akkor [használjon csoportokat tooassign toofederated alkalmazásokat](#managing-access-to-applications).

### <a name="password-based-single-sign-on-account-sharing-and-sso-for-non-federated-apps"></a>Jelszó-alapú egyszeri bejelentkezés: fiók megosztása és az egyszeri bejelentkezés nem összevont alkalmazásokhoz
tooenable egyszeri bejelentkezés tooapplications, amelyek nem támogatják összevonási, az Azure AD ajánlatok jelszó szolgáltatásokat, amelyek biztonságos tárolására jelszavak tooSaaS alkalmazások és a felhasználók automatikus bejelentkezés az alkalmazások. Könnyen terjesztése az újonnan létrehozott fiókhoz tartozó hitelesítő adatok, és a csoporthoz megosztása több személy. Felhasználók nem feltétlenül kell tooknow hello hitelesítő adatok toohello fiókokat, amelyek azokat a hozzáférést kap.

| Útmutató a következő cikket: |  |
|:---:| --- |
| Egy bevezető toohow jelszóalapú SSO works és egy rövid műszaki áttekintése |[Jelszó-alapú egyszeri bejelentkezés az Azure ad szolgáltatással](active-directory-appssoaccess-whatis.md#password-based-single-sign-on) |
| Hello forgatókönyvek kapcsolódó tooaccount megosztása összegzését, és hogyan lehet a problémák megoldani az Azure ad |[Fiókok megosztása az Azure ad szolgáltatással](active-directory-sharing-accounts.md) |
| Egyes alkalmazások rendszeres időközönként automatikusan módosítás hello jelszó |[Automatikus jelszó váltása (előzetes verzió)](https://blogs.technet.microsoft.com/enterprisemobility/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview/) |
| Üzembe helyezési és hibaelhárítási útmutatók az Azure AD-jelszó management bővítmény hello hello Internet Explorer verziója |[Hogyan tooDeploy hello hozzáférési Panel bővítményét az Internet Explorer csoportházirend használatával](active-directory-saas-ie-group-policy.md)<br /><br />[Az Internet Explorer hello hozzáférési Panel bővítmény hibaelhárítása](active-directory-saas-ie-troubleshooting.md) |

Jelszó-alapú egyszeri bejelentkezést érhető el az Azure ad-val tooten alkalmazások minden felhasználóhoz be minden kiadása. [Prémium szintű Azure AD](https://azure.microsoft.com/pricing/details/active-directory/) korlátlan alkalmazásokat támogatja. Ha a szervezete [Azure AD alapvető](https://azure.microsoft.com/pricing/details/active-directory/) vagy [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), akkor [használjon csoportokat tooassign hozzáférés tooapplications](#managing-access-to-applications). Automatikus jelszó váltási van egy [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) szolgáltatás.

### <a name="app-proxy-single-sign-on-and-remote-access-tooon-premises-applications"></a>Alkalmazás-Proxy: Egyszeri bejelentkezés és a távoli hozzáférés tooon helyszíni alkalmazások
Ha vannak olyan alkalmazások, felhasználók és eszközök hello hálózaton kívüli által használt toobe igénylő a magánhálózaton, majd használhatja az Azure AD-alkalmazásproxy tooenable biztonságos távoli elérést toothose alkalmazásokat.

| Útmutató a következő cikket: |  |
|:---:| --- |
| Az Azure AD-alkalmazásproxy és működésének áttekintése |[Tooon helyszíni alkalmazások biztonságos távoli hozzáférést biztosító](active-directory-application-proxy-get-started.md) |
| Kapcsolatos bemutatók tooconfigure proxyval és hogyan toopublish az első alkalmazás |[Hogyan tooSet be Azure AD alkalmazás Proxy](active-directory-application-proxy-enable.md)<br /><br />[TooSilently telepítése hello App alkalmazásproxy-összekötő](active-directory-application-proxy-silent-installation.md)<br /><br />[Hogyan tooPublish alkalmazások App-proxyval](active-directory-application-proxy-publish.md)<br /><br />[Hogyan tooUse saját tartománynév](active-directory-application-proxy-custom-domains.md) |
| Hogyan tooenable egyszeri bejelentkezés és a feltételes hozzáférés az alkalmazások közzétett alkalmazás Proxy |[Single-sign-on a Proxy](active-directory-application-proxy-sso-using-kcd.md)<br /><br />[Feltételes hozzáférés és a Proxy](active-directory-application-proxy-conditional-access.md) |
| Útmutatás a következő forgatókönyvek hello alkalmazásproxy toouse |[Hogyan tooSupport natív alkalmazások](active-directory-application-proxy-native-client.md)<br /><br />[Hogyan tooSupport jogcímbarát alkalmazások](active-directory-application-proxy-claims-aware-apps.md)<br /><br />[Hogyan tooSupport alkalmazások közzétett elkülönülő hálózatokon és helye](active-directory-application-proxy-connectors.md) |
| Alkalmazásproxy hibaelhárítási útmutatója |[Alkalmazás Proxy hibaelhárítási útmutatója](active-directory-application-proxy-troubleshoot.md) |

Minden kiadás tooten alkalmazások minden felhasználóhoz be az Azure ad alkalmazásproxy érhető el. [Prémium szintű Azure AD](https://azure.microsoft.com/pricing/details/active-directory/) korlátlan alkalmazásokat támogatja. Ha a szervezete [Azure AD alapvető](https://azure.microsoft.com/pricing/details/active-directory/) vagy [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), akkor [használjon csoportokat tooassign hozzáférés tooapplications](#managing-access-to-applications).

Bizonyos is érdeklődik [Azure AD tartományi szolgáltatások](../active-directory-domain-services/active-directory-ds-overview.md), így ezeknek az alkalmazásoknak kell a helyszíni alkalmazások tooAzure ugyanakkor továbbra is meg hello identitás toomigrate.

### <a name="enabling-single-sign-on-between-azure-ad-and-on-premises-ad"></a>Egyszeri bejelentkezés az Azure AD között engedélyezése és a helyszíni AD
Ha a szervezet egy Windows Server Active Directory a helyszíni és az Azure Active Directory hello felhőben kezeli, majd érdemes tooenable egyszeri bejelentkezést a két rendszer közötti. Az Azure AD Connect (hello eszközzel együtt egyesíti a két rendszer) egyszeri bejelentkezés beállítása több lehetőséget biztosít: az AD FS vagy egy másik összevonási szolgáltató összevonási létesíthető, vagy a jelszó-szinkronizálás engedélyezése.

| Útmutató a következő cikket: |  |
|:---:| --- |
| Hello egyszeri bejelentkezésre vonatkozó beállításokat érhető el az Azure AD Connect, valamint a hibrid környezetek kezeléséről a áttekintése |[A felhasználói bejelentkezési beállítások az Azure AD Connect](active-directory-aadconnect-user-signin.md) |
| Általános útmutatást mindkét környezetek kezelése a helyszíni Active Directory és az Azure Active Directory |[Az Azure AD hibrid identitáskezelési elrendezésével kapcsolatos szempontok](active-directory-hybrid-identity-design-considerations-overview.md)<br /><br />[A helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md) |
| Útmutatás a jelszó-szinkronizálás tooenable egyszeri bejelentkezés használatával |[Jelszó-szinkronizálás megvalósítása az Azure AD Connect](active-directory-aadconnectsync-implement-password-synchronization.md)<br /><br />[Jelszó-szinkronizálás hibaelhárítása](https://support.microsoft.com/en-us/kb/2855271) |
| Útmutatás a Jelszóvisszaírás tooenable SSO használatával |[Az Azure AD-jelszókezelés első lépések](active-directory-passwords-getting-started.md)<br /><br />[A jelszóvisszaíró hibaelhárítása](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) |
| Útmutatás a harmadik féltől származó identity providers tooenable egyszeri bejelentkezés használatával |[Egyszeri bejelentkezés kompatibilis külső Identity Providers, hogy használható tooEnable listája](https://aka.ms/ssoproviders) |
| Hogyan Windows 10-felhasználók is élvezhessék hello előnyeit az egyszeri bejelentkezés az Azure AD Join keresztül |[Felhőalapú képességek tooWindows 10 eszközöknek az Azure Active Directory csatlakozási kiterjesztése](active-directory-azureadjoin-overview.md) |

Az Azure AD Connect érhető el [Azure Active Directory minden kiadása](https://azure.microsoft.com/pricing/details/active-directory/). Az Azure AD önkiszolgáló jelszó-változtatási érhető el [Azure AD alapvető](https://azure.microsoft.com/pricing/details/active-directory/) és [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/). Jelszó visszaírási tooon helyszínen ad egy [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) szolgáltatás.

### <a name="conditional-access-enforce-additional-security-requirements-for-high-risk-apps"></a>Feltételes hozzáférés: Kényszerítése magas kockázatú alkalmazások további biztonsági követelmények
Beállítása után egyszeri bejelentkezés tooyour alkalmazásokhoz és erőforrásokhoz, majd további biztonságát érzékeny alkalmazások meghatározott biztonsági követelmények minden bejelentkezés toothat alkalmazásában tartat be. Például, hogy az adott alkalmazás összes hozzáférési tooa mindig szükséges többtényezős hitelesítést, függetlenül attól, hogy támogatja-e az alkalmazás innately funkció az Azure AD toodemand is használhatja. Egy másik, ilyenek például a feltételes hozzáférés, hogy a felhasználók fognak csatlakoztatott toohello szervezet toorequire megbízható hálózati rendelés tooaccess a a különösen bizalmas alkalmazásokhoz.

| Útmutató a következő cikket: |  |
|:---:| --- |
| Egy bevezető toohello feltételes hozzáférési képességeket kínál az Azure AD között Office365, és az Intune-ban |[Kockázatkezelés feltételes hozzáférés](active-directory-conditional-access.md) |
| Hogyan tooenable feltételes hozzáférés, a következő típusú erőforrás hello |[Feltételes hozzáférés a Szolgáltatottszoftver-alkalmazásoknál](active-directory-conditional-access-azuread-connected-apps.md)<br /><br />[Feltételes hozzáférés az Office 365-szolgáltatásokhoz](active-directory-conditional-access-device-policies.md)<br /><br />[Feltételes hozzáférés a helyszíni alkalmazások](active-directory-conditional-access.md)<br /><br />[Feltételes hozzáférés a helyszíni alkalmazások közzététele az Azure AD alkalmazás-proxyn keresztül](active-directory-application-proxy-conditional-access.md) |

| Hogyan tooregister eszközök Azure Active Directoryval sorrendben tooenable eszközalapú feltételes hozzáférési házirendek |} [Az Azure Active Directory Eszközregisztráció – áttekintés](active-directory-conditional-access-device-registration-overview.md)<br /><br />[Hogyan tooEnable tartományhoz csatlakoztatott Windows-eszközök automatikus regisztrálásának](active-directory-conditional-access-automatic-device-registration.md)<br />– [Lépéseket a Windows 8.1-eszközök](active-directory-conditional-access-automatic-device-registration-setup.md)<br />– [Lépéseket a Windows 7-eszközök](active-directory-conditional-access-automatic-device-registration-setup.md) |

| Hogyan toouse hello Microsoft Authenticator alkalmazást a kétlépéses ellenőrzéshez |} [Microsoft hitelesítő](../multi-factor-authentication/end-user/microsoft-authenticator-app-how-to.md) |

Feltételes hozzáférés egy [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) szolgáltatás.

## <a name="apps--azure-ad"></a>Alkalmazások és az Azure AD
### <a name="cloud-app-discovery-find-which-saas-apps-are-being-used-in-your-organization"></a>A cloud App Discovery: Található mely SaaS-alkalmazások van használatban a szervezet
A cloud App Discovery segítségével megtudhatja, mely SaaS-alkalmazások vannak használatban hello szervezetben informatikai részlegeket. Azt is mérheti az alkalmazások használatát és a népszerűségét úgy, hogy az informatikai megállapíthatja, hogy mely alkalmazások előnyösek a legtöbbet tudja kihozni alatt hello informatikai vezérlő és integrálva van az Azure AD.

| Útmutató a következő cikket: |  |
|:---:| --- |
| Egy általános működésének áttekintése |[A Cloud App Discovery megállapítás nem engedélyezett a felhőalapú alkalmazásokhoz](active-directory-cloudappdiscovery-whatis.md) |
| Hogyan működik, az adatvédelmi a válaszok tooquestions be egy mélyebb bemutatója |[Biztonsági és adatvédelmi megfontolások](active-directory-cloudappdiscovery-security-and-privacy-considerations.md) |
| Gyakori kérdések |[Gyakori kérdések a Cloud App Discovery modulra](http://social.technet.microsoft.com/wiki/contents/articles/24037.cloud-app-discovery-frequently-asked-questions.aspx) |
| A Cloud App Discovery telepítéséhez oktatóprogramok |[Csoport házirendjének telepítési útmutatója](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx)<br /><br />[A System Center telepítési útmutatója](http://social.technet.microsoft.com/wiki/contents/articles/30968.cloud-app-discovery-system-center-deployment-guide.aspx)<br /><br />[A Proxy kiszolgálókon egyéni portok telepítése](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md) |
| hello frissítések toohello a Cloud App Discovery-ügynök a napló módosítása |[Változásnapló](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx) |

A cloud App Discovery van egy [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) szolgáltatás.

### <a name="automatically-provision-and-deprovision-user-accounts-in-saas-apps"></a>Automatikusan kiépítése és felhasználói fiókok a Szolgáltatottszoftver-alkalmazásoknál kiosztásának megszüntetése
Hello létrehozását, a karbantartási és a felhasználói identitások SaaS-alkalmazásokhoz, például a Dropbox, Salesforce, a ServiceNow és további eltávolítási automatizálásához. Azonos, meglévő identitások az Azure AD közötti szinkronizálása és a Szolgáltatottszoftver-alkalmazásoknál, és automatikusan letiltja a fiókokat, amikor a felhasználók elhagyják a munkahelyet hello elérés.

| Útmutató a következő cikket: |  |
|:---:| --- |
| További tudnivalók: hogyan működik, és válaszok toocommon kérdések |[A felhasználók átadása & megszüntetés tooSaaS alkalmazások automatizálása](active-directory-saas-app-provisioning.md) |
| Hogyan van rendelve az információk között az Azure AD konfigurálása és az SaaS-alkalmazás |[Attribútum-leképezésekhez testreszabása](active-directory-saas-customizing-attribute-mappings.md)<br><br>[Attribútum-leképezésekhez kifejezések írása](active-directory-saas-writing-expressions-for-attribute-mappings.md) |
| Hogyan tooenable automatikus kiépítési tooany app hello SCIM protokollt támogató |[Automatizált Felhasználókiépítése tooany SCIM-Enabled alkalmazás beállítása](active-directory-scim-provisioning.md) |
| Hogyan tooreport meg és hárítsa el a felhasználók átadása |[A felhasználók automatikus átadása jelentések](active-directory-saas-provisioning-reporting.md)<br><br>[Üzembe helyezési értesítések](active-directory-saas-account-provisioning-notifications.md)<br><br>[Hibaelhárítás a felhasználók átadása](active-directory-application-provisioning-content-map.md) |
| Az attribútum értékei alapján kiosztott tooan alkalmazás megkapó korlátot |[Helyezése Hatókörszűrőkkel](active-directory-saas-scoping-filters.md) |

Automatizált felhasználókiépítése Azure ad-val tooten alkalmazások minden felhasználóhoz be összes verziója érhető el. [Prémium szintű Azure AD](https://azure.microsoft.com/pricing/details/active-directory/) korlátlan alkalmazásokat támogatja. Ha a szervezete [Azure AD alapvető](https://azure.microsoft.com/pricing/details/active-directory/) vagy [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/), akkor [mely felhasználók beolvasása kiépített csoportok toomanage használja](#managing-access-to-applications).

### <a name="building-applications-that-integrate-with-azure-ad"></a>Épület alkalmazásokat, amelyekbe beépül az Azure ad szolgáltatással
Ha a szervezet fejleszt, vagy az üzletági (LoB) alkalmazások karbantartása, vagy ha az alkalmazás fejlesztőjének az ügyfelek, akik az Azure Active Directoryt, a következő oktatóanyagok hello segítségével az alkalmazások integrálása az Azure ad-val.

| Útmutató a következő cikket: |  |
|:---:| --- |
| Útmutató mind az informatikai szakemberek, valamint az alkalmazások integrálása az Azure AD az alkalmazásfejlesztők számára |[Alkalmazások fejlesztése az Azure AD hello informatikai szakembereknek tartozó útmutatója](active-directory-applications-guiding-developers-for-lob-applications.md)<br /><br />[az Azure Active Directory hello fejlesztői útmutató](active-directory-developers-guide.md) |
| Hogyan tooapplication szállítók fel az alkalmazások toohello Azure AD-Alkalmazásgyűjtemény |[Az alkalmazás Azure Active Directory Alkalmazáskatalógusában hello listázása](active-directory-app-gallery-listing.md) |
| Hogyan férnek hozzá a toomanage toodeveloped alkalmazások az Azure Active Directoryval |[Hogyan tooEnable fejlesztett alkalmazások felhasználó-hozzárendelése](active-directory-applications-guiding-developers-requiring-user-assignment.md)<br /><br />[Felhasználók tooyour alkalmazás hozzárendelése](active-directory-applications-guiding-developers-assigning-users.md)<br /><br />[Alkalmazás hozzárendelése csoport tooyour](active-directory-applications-guiding-developers-assigning-groups.md) |

Ha az alkalmazások a felhasználók felé néző, előfordulhat, hogy el szeretné használni [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) , hogy nem rendelkezik toodevelop saját identitás rendszer toomanage a felhasználók. [További információk](../active-directory-b2c/active-directory-b2c-overview.md).

## <a name="managing-access-tooapplications"></a>Hozzáférés tooApplications kezelése
### <a name="using-groups-and-self-service-toomanage-who-has-access-toowhich-apps"></a>Csoportok és a hozzáférés toowhich alkalmazások rendelkező önkiszolgáló toomanage használatával
toohelp kezelt erőforrások eléréséhez toowhich rendelkező, Azure Active Directory lehetővé teszi az tooset hozzárendelések és engedélyek léptékű csoportok használatával. Informatikai dönthet tooenable önkiszolgáló szolgáltatásokat, hogy a felhasználók egyszerűen kérhetnek engedély amikor kell azt.

| Útmutató a következő cikket: |  |
|:---:| --- |
| Az Azure AD hozzáférési felügyeleti funkciókat áttekintése |[Bevezetés tooManaging hozzáférés tooApps](active-directory-managing-access-to-apps.md)<br /><br />[Hozzáférés-kezelés az Azure AD működése](active-directory-manage-groups.md)<br /><br />[Hogyan tooUse csoportok tooManage hozzáférés tooSaaS alkalmazások](active-directory-accessmanagement-group-saasapps.md) |
| Alkalmazások és a csoportok a önkiszolgáló felügyeletének lehetővé tételéhez |[Önkiszolgáló alkalmazások kezelése](active-directory-self-service-application-access.md)<br /><br />[Önkiszolgáló csoportkezelés](active-directory-accessmanagement-self-service-group-management.md) |
| Az Azure ad-ben a csoportok beállításával kapcsolatos útmutatás |[Hogyan tooCreate biztonsági csoportok](active-directory-accessmanagement-manage-groups.md)<br /><br />[Hogyan tooDesignate csoportonként](active-directory-accessmanagement-managing-group-owners.md)<br /><br />[Hogyan tooUse hello "minden felhasználók" csoportot](active-directory-accessmanagement-dedicated-groups.md) |
| Használjon dinamikus csoportok tooautomatically Attribútumalapú tagsági szabályokkal csoporttagság feltöltéséhez |[Dinamikus csoporttagság: Speciális szabályok](active-directory-accessmanagement-groups-with-advanced-rules.md)<br /><br />[Dinamikus csoporttagság hibaelhárítása](active-directory-accessmanagement-troubleshooting.md) |

Csoportalapú hozzáférés Alkalmazáskezelés érhető el [Azure AD alapvető](https://azure.microsoft.com/pricing/details/active-directory/) és [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/). Önkiszolgáló csoportfelügyelet, önkiszolgáló Alkalmazáskezelés és a dinamikus csoportok [Azure AD Premium](https://azure.microsoft.com/pricing/details/active-directory/) szolgáltatásokat.

### <a name="b2b-collaboration-enable-partner-access-tooapplications"></a>B2B együttműködés: Partneri hozzáférés tooapplications engedélyezése
Ha a vállalata rendelkezik közösen kötünk más vállalatokkal, valószínű, hogy kell-e toomanage partner tooyour vállalati alkalmazásokat. Az Azure Active Directory B2B együttműködés a partnerekkel együttműködve az alkalmazások egy egyszerű és biztonságos módon tooshare biztosít.

| Útmutató a következő cikket: |  |
|:---:| --- |
| Egy másik Azure AD áttekintést funkciókat, hogy is, mint a külső felhasználók kezelése súgó partnerek, ügyfelek stb. |[Az Azure AD külső identitások kezelésére szolgáló képességeket összehasonlítása](active-directory-b2b-compare-external-identities.md) |
| Egy bevezető tooB2B együttműködés és hogyan tooget |[Egyszerű, biztonságos, az Azure AD Cloud Partner integráció](active-directory-b2b-what-is-azure-ad-b2b.md)<br /><br />[Az Azure Active Directory B2B együttműködés](active-directory-b2b-collaboration-overview.md) |
| Az Azure AD B2B együttműködés a mélyebb bemutatója és hogyan toouse azt |[B2B együttműködés: Hogyan működik?](active-directory-b2b-how-it-works.md)<br /><br />[Az Azure AD B2B együttműködés aktuális korlátozásai](active-directory-b2b-current-limitations.md)<br /><br />[Azure AD B2B együttműködés részletes bemutatója](active-directory-b2b-detailed-walkthrough.md) |
| Hivatkozás cikkek az Azure AD B2B együttműködés működéséről technikai részletei |[A Partner felhasználók hozzáadása a CSV fájlformátum](active-directory-b2b-references-csv-file-format.md)<br /><br />[Az Azure AD B2B együttműködés által érintett felhasználói attribútumok](active-directory-b2b-references-external-user-object-attribute-changes.md)<br /><br />[A Partner felhasználók a felhasználói Token formátuma](active-directory-b2b-references-external-user-token-format.md) |

B2B együttműködés érhető el jelenleg [Azure Active Directory minden kiadása](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="access-panel-a-portal-for-accessing-apps-and-self-service-features"></a>Hozzáférési Panel: Portál alkalmazások és az önkiszolgáló funkciók eléréséhez
Azure AD hozzáférési Panel hello, ahol végfelhasználók is elindíthatja az alkalmazások és a hozzáférési hello önkiszolgáló funkciókat, amelyek lehetővé teszik toomanage az alkalmazások és a csoporttagságokat. Továbbá a toohello hozzáférési panelre, más beállításokat a SSO-kompatibilis alkalmazásokhoz fér hozzá az alábbi listában hello szerepelnek.

| Útmutató a következő cikket: |  |
|:---:| --- |
| Egyszeri bejelentkezés alkalmazásokhoz toousers telepítéséhez rendelkezésre álló hello különböző lehetőségek összehasonlítása |[Az Azure AD integrált alkalmazások tooUsers telepítése](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) |
| A hozzáférési Panel hello és a mobil egyenértékű MyApps áttekintése |[Bevezetés tooAccess Panel és MyApps](active-directory-saas-access-panel-introduction.md)<br />– [iOS](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8)<br />– [Android](https://play.google.com/store/apps/details?id=com.microsoft.myapps) |
| Hogyan tooaccess az Azure AD-alkalmast hello Office 365-webhely |[Office 365 alkalmazás indító hello használata](https://support.office.com/en-us/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a) |
| Hogyan tooaccess az Azure AD-alkalmast hello Intune Managed Browser mobilalkalmazás |[Intune által felügyelt böngészőben](https://technet.microsoft.com/en-us/library/dn878029.aspx)<br />– [iOS](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8)<br />– [Android](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser) |
| Hogyan mélyhivatkozással tooinitiate tooaccess az Azure AD alkalmazások egyszeri bejelentkezés |[Közvetlen bejelentkezés hivatkozások tooYour alkalmazások](active-directory-appssoaccess-whatis.md#direct-sign-on-links-for-federated-password-based-or-existing-apps) |

Hozzáférési Panel érhető el [Azure Active Directory minden kiadása](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="reports-easily-audit-app-access-changes-and-monitor-sign-ins-tooapps"></a>Jelentések: Egyszerűen app változásokat naplózási és bejelentkezések tooapps figyelése
Az Azure Active Directory több jelentéseket biztosít, és riasztást küld toohelp figyelheti a szervezet hozzáférés tooapplications. Szokatlan bejelentkezéseket tooyour alkalmazások riasztásokat kaphat, és nyomon követheti az mikor, és miért érdemes a felhasználói hozzáférés tooan alkalmazás módosult.

| Útmutató a következő cikket: |  |
|:---:| --- |
| Az Azure Active Directory funkciói reporting hello áttekintése |[Ismerkedés az Azure AD jelentéskészítési](active-directory-reporting-getting-started.md) |
| Hogyan toomonitor hello bejelentkezéseket és a felhasználók Alkalmazáshasználat |[A hozzáférési és használati jelentések megtekintése](active-directory-view-access-usage-reports.md) |
| Toowho végrehajtott változások követése érhetik el egy adott alkalmazást |[Az Azure Active Directory Auditnaplójának jelentési eseményei](active-directory-reporting-audit-events.md) |
| Ezen jelentések előnyben részesített tooyour eszközök használatával az adatok exportálása hello hello Reporting API |[Ismerkedés az Azure AD jelentéskészítési API hello](active-directory-reporting-api-getting-started.md) |

mely jelentések érhetők el az Azure Active Directory különböző kiadásait toosee [ide](active-directory-view-access-usage-reports.md).

## <a name="see-also"></a>Lásd még:
[Mi az az Azure Active Directory?](active-directory-whatis.md)

[Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/)

[Az Azure Active Directory tartományi szolgáltatások](https://azure.microsoft.com/services/active-directory-ds/)

[Az Azure többtényezős hitelesítés](https://azure.microsoft.com/services/multi-factor-authentication/)
