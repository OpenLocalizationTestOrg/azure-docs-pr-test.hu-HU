---
title: "a Szolgáltatottszoftver-alkalmazásoknál Azure Active Directoryval az egyszeri bejelentkezés aaaIntegrate |} Microsoft Docs"
description: "Engedélyezze az egyszeri bejelentkezés hitelesítést és a felhasználók átadásához központi hozzáféréskezelést az SaaS-alkalmazásokhoz az Azure Active Directoryban. Hogyan toointegrate Azure Active Directory tooSaaS alkalmazásokat."
services: active-directory
keywords: "Azure AD integrálása SaaS-alkalmazásokhoz"
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 53b9d341-d1fc-4bbb-ac7c-3f4c68fcf00a
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/17/2017
ms.author: curtand
ms.reviewer: aaronsm
ms.openlocfilehash: fe621a30429c81c32470635d105ae3e95184efa1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-active-directory-single-sign-on-with-saas-apps"></a>Azure Active Directoryval az egyszeri bejelentkezés integrálása SaaS-alkalmazásokhoz
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-enterprise-apps-manage-sso.md)
> * [klasszikus Azure portál](active-directory-sso-integrate-saas-apps.md)
>
>

[!INCLUDE [active-directory-sso-use-case-intro](../../includes/active-directory-sso-use-case-intro.md)]

egyszeri bejelentkezés egy olyan alkalmazáshoz, az Ön szervezete még üzembe beállítása megkezdődött tooget, fog használni egy létező könyvtár az Azure Active Directory (Azure AD). A Microsoft Azure, az Office 365 vagy a Windows Intune beszerezni Azure AD-címtár is használhatja. Ha ezek közül legalább kettő, olvassa el [az Azure AD-címtár felügyelete](active-directory-administer.md) toodetermine melyik egy toouse.

> [!IMPORTANT]
> A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon. Hogyan center tooassign rendszergazdai szerepkörök az Azure AD Üdvözöljük a rendszergazdákat, lásd: [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory](active-directory-enterprise-apps-manage-sso.md).

## <a name="authentication"></a>Authentication
Az alkalmazások, amelyek támogatják az WS-Federation, SAML 2.0 hello vagy protokollok OpenID Connect Azure Active Directory által használt aláírási tanúsítványokat tooestablish megbízhatósági kapcsolatokat. További információ: [tartozó tanúsítványok összevont egyszeri bejelentkezést](active-directory-sso-certs.md).

Csak HTML űrlapos bejelentkezés támogató alkalmazások esetében az Azure Active Directory által használt "jelszótárolást" tooestablish bizalmi kapcsolatok. Ez lehetővé teszi, hogy a szervezet toobe hello felhasználók automatikusan megtörténik a tooa hello felhasználói fiók adatai alapján hello SaaS-alkalmazás az Azure ad SaaS-alkalmazáshoz. Az Azure AD gyűjt, és biztonságos helyen tárolja a hello felhasználói fiókkal kapcsolatos információk és hello kapcsolódó jelszót. További információkért lásd: [jelszó-alapú egyszeri bejelentkezést](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

## <a name="authorization"></a>Engedélyezés
Kiépített fiók lehetővé teszi, hogy egy felhasználó jogosult toobe toouse alkalmazás keresztül egyszeri bejelentkezés hitelesített után. A felhasználók átadása elvégezhető manuálisan, vagy bizonyos esetekben adhat hozzá és felhasználói adatok eltávolítása hello SaaS-alkalmazás az Azure Active Directoryban végzett módosítások alapján. A meglévő Azure AD-összekötőt használja, az automatikus üzembe helyezését további információkért lásd: [felhasználói üzembe helyezést és megszüntetést, az SaaS-alkalmazásokhoz automatikus](active-directory-saas-app-provisioning.md).

Ellenkező esetben ezt is manuálisan adja hozzá a felhasználói adatokat tooan app, vagy más kiépítési megoldás hello piactér rendelkezésre álló.

## <a name="access"></a>Hozzáférés
Az Azure AD többféleképpen testre szabható toodeploy alkalmazások tooend felhasználókat a szervezetben. Bármely adott központi telepítés vagy a hozzáférési megoldás, nem záródik. Használhat [igényeinek leginkább megfelelő megoldást hello](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

## <a name="additional-considerations-for-applications-already-in-use"></a>További szempontok az alkalmazások már használatban van
Egy másik folyamat az új fiókot a következő új alkalmazás létrehozásának folyamatán hello beállítását egyszeri bejelentkezés egy alkalmazás már a szervezet által használt. Az első lépések, beleértve néhány: hello alkalmazás tooAzure AD identitások felhasználói identitások mapping, és megértése, hogyan felhasználók fog tapasztalni tooan alkalmazás bejelentkezés után van.

> [!NOTE]
> tooset be egyszeri Bejelentkezést egy meglévő alkalmazáshoz, toohave globális rendszergazdai jogosultsággal kell rendelkeznie mindkét Azure AD-ben, és hello SaaS-alkalmazáshoz.
>
>

### <a name="mapping-user-accounts"></a>Felhasználói fiókok hozzárendelése
A felhasználók személyazonossága általában egy egyedi azonosítója, amely lehet egy e-mail címet, vagy az egyszerű felhasználónév (UPN) rendelkezik. Szüksége lesz a toolink (leképezés) tootheir saját Azure AD identity minden felhasználó alkalmazásidentitás. Többféle módon tooaccomplish, ez attól függően, hogy hogyan hello követelmény az alkalmazások hitelesítést.

Alkalmazás identitásokat az Azure AD-identitások leképezési kapcsolatos további információkért lásd: [hello SAML-jogkivonat kiállított jogcímek testreszabása](http://social.technet.microsoft.com/wiki/contents/articles/31257.azure-active-directory-customizing-claims-issued-in-the-saml-token-for-pre-integrated-apps.aspx) és [attribútum-leképezésekhez kialakítási testreszabása](active-directory-saas-customizing-attribute-mappings.md).

### <a name="understanding-hello-users-log-in-experience"></a>Hello felhasználói bejelentkezéshez ismertetése
Ha integrálja az egyszeri bejelentkezés egy alkalmazás, amely már használatban van, fontos, amely a felhasználói élmény hello toorealize fog vonatkozni. Minden olyan alkalmazásnál felhasználók indul el az Azure AD hitelesítő adatait toosign a használatával. Annak oka is lehet, hogy egy másik portál tooaccess hello alkalmazások kell használniuk.

Egyes alkalmazások SSO hello alkalmazás bejelentkezési felületen végezhető, de más alkalmazások hello felhasználói kell toogo központi portálon keresztül, mint ([saját alkalmazások](http://myapps.microsoft.com) vagy [Office365](http://portal.office.com/myapps)) a toosign. További tudnivalók az egyszeri bejelentkezés és a felhasználói élmény, a különböző típusú hello [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

Egy másik értékes erőforrás *felhasználói hozzájárulás kikapcsolása* a hello [Guiding fejlesztők](active-directory-applications-guiding-developers-for-lob-applications.md) cikk.

## <a name="next-steps"></a>Következő lépések
Hello Alkalmazásgyűjtemény talált SaaS-alkalmazásokhoz, az Azure AD számos [kapcsolatos bemutatók toointegrate SaaS-alkalmazások](active-directory-saas-tutorial-list.md).

Ha az alkalmazás nem szerepel gyűjtemény, akkor [adja hozzá az Azure AD-Alkalmazásgyűjtemény toohello egyéni alkalmazás](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

Sokkal részletesebben összes kezdve hello Azure.com webhelyre könyvtárban problémák van [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).

Továbbá, nem hagy ki hello [cikk Index az Azure Active Directoryban Alkalmazáskezelés](active-directory-apps-index.md).
