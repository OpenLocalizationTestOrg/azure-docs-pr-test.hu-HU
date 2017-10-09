---
title: az Azure Active Directoryval aaaHow tooIntegrate |} Microsoft Docs
description: "Egy útmutató toobenefits, és az Azure Active Directory integrációja a erőforrásokat."
services: active-directory
documentationcenter: dev-center-name
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: d13bba54-96bd-4b81-bee9-c8025ffa1648
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: 4542965ae4a7756eda9f57e9e895f8044892f20e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrating-with-azure-active-directory"></a>Az Azure Active Directory integrálása
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Az Azure Active Directory biztosít a vállalati szintű felhőalapú alkalmazások Identitáskezelés szervezetek számára.  Az Azure AD-integrációs a felhasználók egy zökkenőmentes bejelentkezési élményt nyújt, és lehetővé teszi az alkalmazás tooIT házirend felel meg.

## <a name="how-toointegrate"></a>Hogyan tooIntegrate
Az Azure AD-val a alkalmazás toointegrate számos módja van.  Számos, vagy kevés ezek a forgatókönyvek az alkalmazás megfelelő előnyeinek kihasználása.

### <a name="support-azure-ad-as-a-way-toosign-in-tooyour-application"></a>Támogatja az Azure AD szolgáltatásba egy módon tooSign tooYour alkalmazás
**Csökkentse a súrlódás bejelentkezés, és csökkentheti a támogatási költségeket.** Az Azure AD toosign tooyour alkalmazás segítségével a felhasználók egy további nevét és jelszavát tooremember nem lesz.  Fejlesztői lesz egy kisebb jelszó toostore rendelkezik, és védelmét.  Nem rendelkezik toohandle elfelejtett jelszó-átállításra, lehet, hogy csak egy jelentős megtakarítást.  Az Azure AD bejelentkezés hello world legnépszerűbb felhőalapú alkalmazások, beleértve az Office 365 és a Microsoft Azure részénél megoldásaira épül.  A száz millió felhasználók szervezetek több millió, valószínűleg a felhasználó már bejelentkezett tooAzure AD.  További információ [az Azure AD bejelentkezés támogatását](active-directory-authentication-scenarios.md).

**Egyszerűbbé teszik a jelentkezzen be az alkalmazáshoz.**  Alatt a regisztrációt az alkalmazás az Azure AD, hogy előre töltse ki a regisztrációs űrlap vagy kiküszöbölheti a teljesen küldhet egy felhasználó alapvető adatait.  Felhasználók regisztrálhatnak az alkalmazás használatával az Azure AD-fiókot egy jól ismert hozzájárulási élmény hasonló toothose található a közösségi média és a mobilalkalmazások keresztül.  Bármely felhasználó regisztráljon, és jelentkezzen be tooan alkalmazás, amely integrálva van az Azure AD informatikai beavatkozás nélkül.  További információ [aláíró létrehozása az Azure AD-fiók bejelentkezési alkalmazását](../../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

### <a name="browse-for-users-manage-user-provisioning-and-control-access-tooyour-application"></a>Tallózással keresse meg a felhasználók, a felhasználók átadása kezelése és elérés tooYour alkalmazás
**Keresse meg a felhasználók hello könyvtárban.**  Felhasználhatja a hello Graph API toohelp felhasználók keresési és tallózási mások vállalatukon belüli meghívása mások, vagy a hozzáférést, ahelyett, hogy azok tootype e-mail-címet.  Felhasználók tallózhassanak a megszokott cím könyv stílus felületen keresztül, beleértve a szervezeti hierarchia hello hello részleteinek megtekintése.  További tudnivalók hello [Graph API](active-directory-graph-api.md).

**Active Directory-csoportok és az ügyfél már kezel terjesztési listák újra felhasználhatja.**  Az Azure AD, hogy az ügyfél már e-mailek terjesztése használja, és hozzáférés-kezelés hello csoportokat tartalmazza.  Hello Graph API használatával, újra felhasználhatja ezeket a csoportokat, ahelyett, hogy az ügyfél toocreate, és külön az az alkalmazás csoportok kezelése.  Felügyeleticsoport-információkat is küldhetők tooyour alkalmazás jelentkezzen be a jogkivonatokat.  További tudnivalók hello [Graph API](active-directory-graph-api.md).

**Az Azure AD toocontrol rendelkező access tooyour alkalmazás használja.**  A rendszergazdák és az Azure AD alkalmazás-tulajdonosok rendelhet hozzá hozzáférési tooapplications toospecific felhasználókat és csoportokat.  Hello Graph API használatával, olvassa el ebben a listában, és használják toocontrol üzembe helyezést és megszüntetést, az erőforrások és az alkalmazáson belül hozzáférni.

**Az Azure AD-szerepkörök alapuló hozzáférés-vezérlést.**  A rendszergazdák és alkalmazástulajdonosok rendelhet hozzá felhasználók és csoportok tooroles regisztrálnia az alkalmazást az Azure AD meg.  Szerepkör-információ tooyour alkalmazás küldött jogkivonatokat bejelentkezés, és is olvasható hello Graph API használatával.  További információ [Azure AD használata a hitelesítéshez](http://blogs.technet.com/b/ad/archive/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles.aspx).

### <a name="get-access-toousers-profile-calendar-email-contacts-files-and-more"></a>Hozzáférés tooUser profil, naptár, e-mailek, névjegyek, fájlok és több
**Az Azure AD az Office 365 hello engedélyezési kiszolgáló- és egyéb Microsoft üzleti szolgáltatásokat.**  Ha tooyour alkalmazás vagy a jelenlegi felhasználói fiókok tooAzure AD felhasználói fiókok összekapcsolása OAuth 2.0 használatával támogatási bejelentkezéshez támogatja az Azure AD, olvasási és írási hozzáférés tooa felhasználói profil, naptár, e-mailek, névjegyek, fájlok és egyéb információkat kérhet.  Akkor is zökkenőmentesen események toouser naptár, írási és olvasási vagy írási fájlok tootheir onedrive vállalati verzió.  További információ [Office 365 API-k használata hello](https://msdn.microsoft.com/office/office365/howto/platform-development-overview).

### <a name="promote-your-application-in-hello-azure-and-office-365-marketplaces"></a>Hello Azure az alkalmazás és az Office 365 piacterek előléptetése
**A szervezetek számára már használja az Azure AD alkalmazás toohello több millió lépteti elő.**  Felhasználók keresése, és keresse meg a piactér már használja egy vagy több felhőszolgáltatások, így azok minősített felhőalapú szolgáltatás felhasználói.  További információkat az alkalmazás [hello Azure piactér](https://azure.microsoft.com/marketplace/partner-program/).

**Amikor a felhasználók az alkalmazáshoz, az Azure AD hozzáférési panel és Office 365 alkalmazás indító fog megjelenni.**  Felhasználók fognak tudni tooquickly és később könnyen visszatérési tooyour alkalmazás felhasználói javítása.  További tudnivalók hello [Azure AD hozzáférési panel](../active-directory-saas-access-panel-introduction.md).

### <a name="secure-device-to-service-and-service-to-service-communication"></a>Eszköz-to-Service és a szolgáltatások közötti kommunikáció védelméhez
**Az Azure AD identity Management szolgáltatások és eszközök csökkenti a hello kód toowrite kell, és lehetővé teszi a IT toomanage számára.**  Szolgáltatások és eszközök jogkivonatok lekérni az OAuth protokollt használó Azure AD, és ezek jogkivonatok tooaccess webes API-k használata.  Az Azure AD használatával elkerülheti a programozás összetett hitelesítést.  Hello szolgáltatások és eszközök hello identitások tárolási az Azure ad-ben, mert informatikai kulcsok és a visszavont tanúsítványok, így ahelyett hogy toodo ez külön-külön az alkalmazás egy helyen kezelhetők.

## <a name="benefits-of-integration"></a>Integráció előnyei
Az Azure Active Directoryval való integrációhoz tartalmaz, amely nincs szükség további kódokra toowrite előnyeit.

### <a name="integration-with-enterprise-identity-management"></a>Integráció a vállalati identitás kezelése
**Az IT-házirendeknek megfelelő alkalmazás segítségével.**  A szervezetek a vállalati identitás felügyeleti rendszerek integrálása az Azure AD, ha egy személy elhagyja a szervezet, automatikusan elvesztik access tooyour alkalmazás nélkül informatikai igénylő édesvizek tootake további lépéseket.  Informatikai kezelheti az alkalmazás elérését és határozni, hogy milyen hozzáférési házirendek szükségesek – példa multi-factor Authentication - csökkenti a szükséges toowrite kód toocomply összetett vállalati házirendek.  Az Azure AD segítségével a rendszergazdák, akik aláírt tooyour alkalmazásban tehát részletes naplózást informatikai használat nyomon követheti.

**Az Azure AD kiterjeszti az Active Directory toohello felhő, hogy az alkalmazás integrálható az Active.**  Hello világ számos szervezet Active Directory használata a fő be- és identitás felügyeleti rendszer, és igényelnek az alkalmazások toowork az ad-val.  Az alkalmazás az Azure AD integrálása integrálható az Active Directory.

### <a name="advanced-security-features"></a>A speciális biztonsági szolgáltatások
**Többtényezős hitelesítés.**  Az Azure AD natív többtényezős hitelesítést biztosít.  A rendszergazdák lehet szükség a multi-factor authentication tooaccess az alkalmazást, hogy nem rendelkezik toocode a támogatás saját maga.  További információ [multi-factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/).

**Rendellenes bejelentkezési észlelése.**  Az Azure AD dolgozza fel a több mint egymilliárd bejelentkezések a nap, a gép tanulási algoritmusok toodetect gyanús tevékenységet, és értesítenie kell a lehetséges okok a rendszergazdák.  Az Azure AD-bejelentkezés támogatásával az alkalmazás lekérdezi hello előnye, hogy ez a védelem. További információ [Azure Active Directory hozzáférési jelentés megtekintése](../active-directory-view-access-usage-reports.md).

**Feltételes hozzáférés.**  Toomulti-factor authentication rendszergazdák is követelhetnek meg bizonyos feltételeknek teljesülniük előtt felhasználók is bejelentkezhet tooyour alkalmazás.  Beállítható feltétel a hello IP-címtartomány ügyféleszközök, a tagságot a megadott csoportokra és hello eszköz eléréshez használt hello állapotának.  További információ [Azure Active Directory feltételes hozzáférés](../active-directory-conditional-access.md).

### <a name="easy-development"></a>Könnyű fejlesztés
**Iparági szabványos protokollt használják.**  Microsoft véglegesített toosupporting ipari szabványok.  Az Azure AD hello SAML 2.0, az OpenID Connect 1.0, az OAuth 2.0 és a WS-Federation 1.2-es hitelesítési protokollt támogat.  a Graph API hello OData 4.0-s megfelelő.  Ha az alkalmazás már támogatja az összevont bejelentkezéshez hello SAML 2.0-s vagy OpenID Connect 1.0 protokoll, az Azure AD-támogatás hozzáadása is egyszerű lehet.  További információ [az Azure AD hitelesítési protokollok támogatott](active-directory-authentication-protocols.md).

**Nyílt forráskódú szalagtárak.**  A Microsoft teljes mértékben támogatott nyílt forráskódú szalagtárak népszerű nyelvekhez és platformokhoz toospeed fejlesztési biztosít.  Apache 2.0 licencbe hello forráskódot, és szabad toofork és hátsó toohello projektek közre.  További információ [az Azure AD-hitelesítési kódtárakkal](active-directory-authentication-libraries.md).

### <a name="worldwide-presence-and-high-availability"></a>Globális jelenlét és magas rendelkezésre állás
**Az Azure AD hello világ különböző adatközpontokban van telepítve van és kezelhető és figyelhető hello óra körül is.**  Az Azure AD hello identitás a Microsoft Azure és az Office 365 felügyeleti rendszer, és telepítve van az hello világ 28 adatközpontokban.  Címtáradatok garantáltan replikált toobe tooat legalább három adatközpontokban.  Globális terheléselosztók győződjön meg arról, a felhasználók férhetnek hozzá a hello legközelebbi úgy, hogy az adatok az Azure AD példányát, és automatikusan újra útvonal-kérelmek tooother adatközpontok Ha probléma merül fel.

## <a name="next-steps"></a>Következő lépések
[Első lépések programozás](active-directory-developers-guide.md#get-started).

[Az Azure AD használatával a felhasználók](active-directory-authentication-scenarios.md)

