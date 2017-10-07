---
title: "Gyakori kérdések (GYIK) – az Azure AD B2C |} Microsoft Docs"
description: "Azure Active Directory B2C-vel kapcsolatos gyakori kérdések"
services: active-directory-b2c
documentationcenter: 
author: saeeda
manager: krassk
editor: bryanla
ms.assetid: ed33c2ca-76d0-442a-abb1-8b7b7bb92d6a
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeeda
ms.openlocfilehash: f7857299bc3cb9d5fbe58e047818ec56741e0740
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-frequently-asked-questions-faq"></a>Az Azure AD B2C: Gyakori kérdések (GYIK) 
Ezen a lapon hello Azure Active Directory (Azure AD) B2C kapcsolatos gyakori kérdésekre. Tartsa biztonsági frissítések keresése.

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>Használható az Azure AD B2C funkciók a a meglévő, alkalmazott-alapú Azure AD-bérlő?
Az Azure AD és az Azure AD B2C külön termékajánlatokat, és nem szerepelhet egyszerre a(z) hello azonos bérlő.  Az Azure AD-bérlő szervezet jelöli.  Azure AD B2C-bérlő a függő entitások alkalmazásainak használt identitások toobe gyűjteményét képviseli.  Egyéni házirendek segítségével (a nyilvános előzetes verzió) az Azure AD B2C is összevonni tooAzure AD a szervezetben lévő alkalmazottakkal lehetővé tevő hitelesítését.

### <a name="can-i-use-azure-ad-b2c-tooprovide-social-login-facebook-and-google-into-office-365"></a>Office 365-be is használható az Azure AD B2C tooprovide közösségi bejelentkezés (Facebook és Google +)?
Az Azure AD B2C használt tooauthenticate felhasználók a Microsoft Office 365 nem lehet.  Az Azure AD alkalmazott hozzáférés tooSaaS alkalmazásokat kezeléséhez a Microsoft-megoldás és nem rendelkezik, például a licencelési és a feltételes hozzáférések erre a célra tervezett szolgáltatások.  Az Azure AD B2C egy identitás- és hozzáférés-kezelési platformot kínál webes és mobilalkalmazásokhoz készítéséhez.  Konfigurált toofederate tooan Azure AD-bérlő az Azure AD B2C esetén hello Azure AD-bérlő kezeli az Azure AD B2C támaszkodó alkalmazott hozzáférés tooapplications.

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>Mik azok a helyi fiókok az Azure AD B2C? Hogyan vannak különbözik a munkahelyi vagy iskolai fiókok Azure AD-ben?
Az Azure AD-bérlő toohello bérlőhöz tartozó felhasználók bejelentkezés hello űrlap e-mail címmel rendelkező `<xyz>@<tenant domain>`.  Hello `<tenant domain>` hello egyik ellenőrizve hello tartományok bérlői vagy hello kezdeti `<...>.onmicrosoft.com` tartomány. Ilyen típusú fiókok munkahelyi vagy iskolai fiókkal.

Azure AD B2C-bérlő, a legtöbb alkalmazást szeretne hello felhasználói toosign-bármilyen tetszőleges e-mail címmel (például joe@comcast.net, bob@gmail.com, sarah@contoso.com, vagy jim@live.com). Ez a fiók típus egy helyi fiókot.  Tetszőleges felhasználónevek helyi fiókok (például joe, bob, sarah vagy jim) is támogatja. Az említett két helyi fiók beállítása az Azure AD B2C hello Azure-portálon szerint is választhat.

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-toosupport-in-hello-future"></a>Mely közösségi identitás-szolgáltatóktól támogatják a most? Melyik elvégzi a jövőbeli hello toosupport megtervezése?
Jelenleg támogatott Facebook, Google +, LinkedIn, Amazon, Twitter (előzetes verzió), WeChat (előzetes verzió), Weibo (előzetes verzió) és Gyorsműveletek (előzetes verzió). A Microsoft támogatni fogják a többi ügyfél igény szerint népszerű közösségi identitás-szolgáltatóktól.

Az Azure AD B2C is hozzá van adva támogatása [egyéni házirendek](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-overview-custom).  Ezek [egyéni házirendek](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-overview-custom) egy fejlesztői toocreate engedélyezése, hogy bármely identitásszolgáltatóval, amely támogatja a saját házirend [OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) vagy SAML. 

Ismerkedés az egyéni házirendek kiveszi a [egyéni házirend alapszintű csomag](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack).

### <a name="can-i-configure-scopes-toogather-more-information-about-consumers-from-various-social-identity-providers"></a>Beállítható hatókörök toogather további információ a különböző közösségi identitás-szolgáltatóktól származó fogyasztók?
Nem, de ez a funkció megtalálható a terv. a közösségi identitás-szolgáltatóktól támogatott számú használt hello alapértelmezett hatókörök a következők:

* Facebook: e-mailek
* Google +: e-mailek
* Microsoft-fiókot: e-mail-profil openid
* Amazon: profil
* LinkedIn: r_emailaddress, r_basicprofile

### <a name="does-my-application-have-toobe-run-on-azure-for-it-work-with-azure-ad-b2c"></a>Az alkalmazás rendelkezik toobe az Azure-on futtatása az Azure AD B2C használata?
Nem, az alkalmazást tetszőleges (hello felhőalapú vagy helyszíni) tárolhatja. Azure AD B2C toointeract kell csak képességét toosend hello és nyilvánosan elérhető végponton HTTP-kérelmek fogadásához.

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-hello-azure-portal"></a>Van több Azure AD B2C-bérlő. Hogyan kezelhetők azok a hello Azure-portálon?
Mielőtt megnyitná "Azure AD B2C" hello Azure portál bal oldali menüjében hello, át kell váltania toomanage kívánt hello könyvtárba.  Váltson a könyvtárak hello jobb felső sarkában hello Azure-portálon identitásra kattintva, majd válasszon egy könyvtárat a hello legördülő lista, amely akkor jelenik meg.  A részletes lemezképekkel, lásd: [keresse meg a tooAzure AD B2C beállítások](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).

### <a name="how-do-i-customize-verification-emails-hello-content-and-hello-from-field-sent-by-azure-ad-b2c"></a>Hogyan ellenőrző e-mailek testre (tartalom hello és hello "származó:" mező) az Azure AD B2C által küldött?
Használhatja a hello [vállalati arculat megjelenítése a szolgáltatás](../active-directory/active-directory-add-company-branding.md) toocustomize hello tartalma ellenőrző e-maileket. Pontosabban a két elem hello e-mailek testre szabható:

* **A szalagcím emblémája**: hello jobb alsó látható.
* **Háttérszín**: hello tetején látható.

    ![Képernyőfelvétel a testreszabott ellenőrző e-mailt](./media/active-directory-b2c-faqs/company-branded-verification-email.png)

hello e-mail aláírás hello B2C bérlő kezdeti létrehozásakor megadott hello B2C bérlő neve tartalmazza. Módosíthatja a jelen útmutatást hello nevét:

1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com/) hello előfizetési rendszergazda.
1. Keresse meg a tooyour B2C-bérlő.
1. Kattintson a hello **konfigurálása** fülre.
1. Változás hello **neve** hello eleménél **Directory tulajdonságok** szakasz.
1. Kattintson a **mentése** hello lap hello alján.

Jelenleg nincs módja toochange hello "származó:" hello e-mail található. Szavazhatnak [feedback.azure.com](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/15334335-fully-customizable-verification-emails) érdekli testreszabása hello megerősítési e-mailt hello törzsét.

### <a name="how-can-i-migrate-my-existing-user-names-passwords-and-profiles-from-my-database-tooazure-ad-b2c"></a>Hogyan tudom áttelepítheti a meglévő felhasználóneveket, jelszavakat és profilok saját adatbázis tooAzure AD B2C?
Hello Azure AD Graph API toowrite az áttelepítési eszköz használható. Lásd: hello [Graph API-mintában](active-directory-b2c-devquickstarts-graph-dotnet.md) részleteiről.

### <a name="what-password-policy-is-used-for-local-accounts-in-azure-ad-b2c"></a>Milyen jelszóházirend az Azure AD B2C helyi fiókok esetében használatos?
helyi felhasználói fiókok Azure AD B2C jelszóházirend hello hello házirend alapján az Azure AD. Az Azure AD B2C-előfizetés, a regisztráció vagy bejelentkezés és a jelszó alaphelyzetbe állítása a házirendek által használt hello "erős" jelszó erőssége, és nem jár le a jelszavakat. Olvasási hello [az Azure AD-jelszóházirendet](https://msdn.microsoft.com/library/azure/jj943764.aspx) további részleteket.

### <a name="can-i-use-azure-ad-connect-toomigrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-tooazure-ad-b2c"></a>Használható az Azure AD Connect toomigrate felhasználói identitásokat a helyszíni Active Directory tooAzure AD B2C tárolt?
Nem, az Azure AD Connect nem az Azure AD B2C tervezett toowork. Érdemes lehet hello [Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md) felhasználói áttelepítés.

### <a name="can-my-app-open-up-azure-ad-b2c-pages-within-an-iframe"></a>Saját alkalmazás megnyitása is az Azure AD B2C lapok iFrame belül?
Nem, biztonsági okokból az Azure AD B2C-lap nem nyitható iFrame belül.  A szolgáltatás hello böngésző tooprohibit IFRAME kommunikál.  biztonsági közösségi általában hello és hello OAUTH2 megadását, IFRAME használatának mellőzését a identitással kapcsolatos műveletet, kattintson-emelési toohello kockázata miatt javasoljuk.

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Az Azure AD B2C például a Microsoft Dynamics CRM rendszerrel működik?
Integráció a Microsoft Dynamics 365 Portal érhető el.  Lásd: [Dynamics 365 portál konfigurálása toouse az Azure AD B2C-hitelesítéshez](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/portals/azure-ad-b2c).

### <a name="does-azure-ad-b2c-work-with-sharepoint-on-premises-2016-or-earlier"></a>Az Azure AD B2C does munkahelyi SharePoint helyszíni 2016 vagy korábbi?
Az Azure AD B2C nem célja hello SharePoint külső partner-megosztási forgatókönyvhöz; Lásd: [Azure AD B2B](http://blogs.technet.com/b/ad/archive/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview.aspx) helyette.

### <a name="should-i-use-azure-ad-b2c-or-b2b-toomanage-external-identities"></a>Az Azure AD B2C vagy B2B toomanage külső identitások érdemes használni?
Olvassa el ebben a cikkben [külső identitások](../active-directory/active-directory-b2b-compare-external-identities.md) hello alkalmazásával kapcsolatos megfelelő toolearn tooyour külső identitások forgatókönyvek szolgáltatásokat.

### <a name="what-reporting-and-auditing-features-does-azure-ad-b2c-provide-are-they-hello-same-as-in-azure-ad-premium"></a>Milyen jelentéskészítési és a naplózási szolgáltatások az Azure AD B2C nyújt? , Azokat hello ugyanaz, mint a prémium szintű Azure AD?
Az Azure AD B2C nem, nem támogatja a hello azonos állítja be a jelentések Azure AD Premium. Vannak azonban sok commonalities:

* hello bejelentkezési jelentései a minden egyes bejelentkezés csökkentett adatokkal rekordja.
* Naplózási jelentések érhetők el az Azure portál, Azure Active Directory hello > tevékenység-Naplók > válassza a B2C és alkalmazni kívánt szűrőket. A felügyeleti tevékenység, valamint alkalmazástevékenységhez tartoznak. 
* Azon felhasználók számát, a bejelentkezések száma és a kötet MFA vonatkozó használati jelentés érhető el: [használati Reporting API](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-usage-reporting-api)

### <a name="can-i-localize-hello-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>Az Azure AD B2C által kiszolgált lap felhasználói felületének hello is localize? Milyen nyelveket támogatja?
Igen!  További információ a [nyelvi testreszabási](active-directory-b2c-reference-language-customization.md), amely nyilvános előzetes verzió van.  Fordítások 36 nyelvhez nyújtunk, és lehet felülbírálni bármilyen karakterlánc toosuit igényeinek.

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-hello-url-from-loginmicrosoftonlinecom-toologincontosocom"></a>Használható önállóan URL-címeket a regisztrációs és a bejelentkezési oldalon, az Azure AD B2C által szolgáltatott? Például módosítható hello URL-címet a login.microsoftonline.com toologin.contoso.com?
Jelenleg nem. Ez a funkció a terv van. A tartomány hello ellenőrzése **tartományok** hello klasszikus Azure portálra a lap nem ezen cél megvalósításához.

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>Hogyan törli a saját Azure AD B2C-bérlő?
Kövesse ezeket a lépéseket toodelete az Azure AD B2C-bérlő:

1. Kövesse az alábbi lépéseket túl[keresse meg a tooAzure AD B2C beállítások](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) a hello Azure-portálon.
1. Keresse meg a toohello **alkalmazások**, **identitás-szolgáltatóktól**, és **házirend** és törölje az összes hello bejegyzéseket hozzájuk.
1. Most már bejelentkezhet toohello [a klasszikus Azure portálon](https://manage.windowsazure.com/) hello előfizetési rendszergazda. (Használja a hello azonos munkahelyi vagy iskolai fiókkal vagy hello toosign az Azure szolgáltatáshoz használt Microsoft-fiók.)
1. Keresse meg az Active Directory-bővítményt toohello hello bal oldalon, és kattintson a B2C bérlőre.
1. Kattintson a hello **felhasználók** fülre.
1. Jelölje ki minden egyes felhasználó bekapcsolása (kizárási hello előfizetési rendszergazda felhasználó Ön jelenleg felhasználóként van bejelentkezve). Kattintson a **törlése** hello lap, és kattintson a hello alján **Igen** megjelenésekor.
1. Kattintson a hello **alkalmazások** fülre.
1. Válassza ki **a vállalat tulajdonában lévő alkalmazások** a hello **megjelenítése** legördülő mezőben, majd kattintson a pipa jelre hello.
1. Egy alkalmazást, **b2c-bővítmények-alkalmazás**. Kattintson a **törlése** hello lap, és kattintson a hello alján **Igen** megjelenésekor.
1. Nyissa meg újra toohello Active Directory-bővítményt, és válassza ki a B2C-bérlő.
1. Kattintson a **törlése** hello lap hello alján. toocomplete hello folyamat, kövesse az utasításokat hello hello képernyőn.

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>Az Azure AD B2C nagyvállalati mobilitási csomag részeként szerezhető?
Az Azure AD B2C nem, a használatalapú fizetés Azure-szolgáltatás, és nem része a nagyvállalati mobilitási csomag.

### <a name="how-do-i-report-issues-with-azure-ad-b2c"></a>Azure AD B2C-vel kapcsolatos problémák jelentése?
Lásd: [fájl támogatási kérelmek az Azure Active Directory B2C](active-directory-b2c-support.md).
