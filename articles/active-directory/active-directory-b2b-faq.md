---
title: "aaaAzure Active Directory B2B együttműködés – gyakori kérdések |} Microsoft Docs"
description: "Microsoft Azure Active Directory B2B együttműködés kapcsolatban felmerülő kérdések válaszok toofrequently beolvasása."
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
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: 4412bbc65274ff01782db81dfcc8818a6362ea7c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2b-collaboration-faqs"></a>Az Azure Active Directory B2B együttműködés – gyakori kérdések

Azure Active Directory (Azure AD) üzleti vállalatközi (B2B) együttműködés kapcsolatos gyakori kérdések (GYIK) olyan rendszeresen frissített tooinclude új témaköröket.

### <a name="is-azure-ad-b2b-collaboration-available-in-hello-azure-classic-portal"></a>Az Azure AD B2B együttműködés hello a klasszikus Azure portálon elérhető?
Nem. Az Azure AD B2B együttműködés funkciók érhetők el csak hello [Azure-portálon](https://portal.azure.com) és hello [hozzáférési Panel](https://myapps.microsoft.com/). 

### <a name="can-we-customize-our-sign-in-page-so-it-is-more-intuitive-for-our-b2b-collaboration-guest-users"></a>A Microsoft testre az bejelentkezési oldalra, hogy a a B2B együttműködés vendégfelhasználók intuitívabb?
Feltétlenül! Tekintse meg a [erről a szolgáltatásról blogbejegyzés](https://blogs.technet.microsoft.com/enterprisemobility/2017/04/07/improving-the-branding-logic-of-azure-ad-login-pages/). További információ hogyan toocustomize a szervezete bejelentkezési lapon című [vállalati arculat megjelenítése a toosign és a hozzáférési Panel oldalakon](active-directory-add-company-branding.md).

### <a name="can-b2b-collaboration-users-access-sharepoint-online-and-onedrive"></a>SharePoint Online és onedrive vállalati verzió hozzáférhet B2B együttműködés felhasználók?
Igen. Hello képességét toosearch a meglévő vendégfelhasználók a SharePoint Online hello személyek objektumválasztó használatával azonban **ki** alapértelmezés szerint. a hello beállítás toosearch a meglévő vendégfelhasználók tooturn beállítása **ShowPeoplePickerSuggestionsForGuestUsers** túl**a**. Ha bekapcsolja ezt a beállítást a hello bérlői szintjén vagy hello webhelycsoport szintjén. Ezt a beállítást módosíthatja hello Set-SPOTenant és a Set-SPOSite parancsmagjainak használatával. Ezeket a parancsmagokat a tagok összes meglévő vendégfelhasználók kereshet hello könyvtárban. Hello bérlői hatókörhöz változásai nem befolyásolják a SharePoint Online-webhelyhez, amely már megtörtént.

### <a name="is-hello-csv-upload-feature-still-supported"></a>Fürt megosztott kötetei szolgáltatás töltse fel a szolgáltatás továbbra is támogatott hello van?
Igen. Hello .csv fájl feltöltési szolgáltatás használatával kapcsolatos további információkért lásd: [a PowerShell-példa](active-directory-b2b-code-samples.md).

### <a name="how-can-i-customize-my-invitation-emails"></a>Hogyan szabható testre a meghívó e-mailt?
Hello segítségével testreszabhatja hello meghívó folyamattal kapcsolatos szinte minden [B2B meghívó API-k](active-directory-b2b-api.md).

### <a name="can-an-invited-external-user-leave-hello-organization-after-being-invited"></a>A meghívott külső felhasználó elhagyja hello szervezet meghívásának után?
hello hívja fel szervezet rendszergazdája B2B együttműködés Vendég felhasználó törlése a könyvtárból, de hello Vendég felhasználó nem léphet ki a szervezet címtárához meghívása önmagában hello. 

### <a name="can-guest-users-reset-their-multi-factor-authentication-method"></a>Visszaállíthatja a vendégfelhasználók a többtényezős hitelesítést?
Igen. Vendégfelhasználók alaphelyzetbe állíthatja a többtényezős hitelesítési módszer hello azonos módon, hogy rendszeres felhasználók tegye.

### <a name="which-organization-is-responsible-for-multi-factor-authentication-licenses"></a>Melyik szervezetben felelős a multi-factor authentication licenceket?
hello hívja fel szervezet multi-factor Authentication hitelesítést hajt végre. szervezet meghívása hello kell győződjön meg arról, hogy rendelkezik-e elegendő a B2B többtényezős hitelesítést használó felhasználók licenceinek hello szervezet.

### <a name="what-if-a-partner-organization-already-has-multi-factor-authentication-set-up-can-we-trust-their-multi-factor-authentication-and-not-use-our-own-multi-factor-authentication"></a>Mi történik, ha egy fiókpartner-szervezet már rendelkezik állítson be többtényezős hitelesítést? Is azt a többtényezős hitelesítés megbízható, és nem a saját többtényezős hitelesítés használata?
Ez a szolgáltatás egy későbbi kiadásban tervezett úgy, hogy akkor is kijelölhet egyes partnerek tooexclude (hello hívja fel vállalata) a multi-factor authentication.

### <a name="how-can-i-use-delayed-invitations"></a>Hogyan használhatók késleltetett meghívókat?
Egy szervezet előfordulhat, hogy tooadd B2B együttműködés a felhasználóknak, azok tooapplications szükség szerint oszthatják ki, és küldje meghívókat. Hello B2B együttműködés meghívó API toocustomize hello bevezetési munkafolyamat is használhat.

### <a name="can-i-make-a-guest-user-a-limited-administrator"></a>Tehetők a Vendég felhasználó korlátozott jogokkal rendelkező rendszergazda?
Abszolút. További információkért lásd: [hozzáadása vendég felhasználók tooa szerepkör](active-directory-users-assign-role-azure-portal.md).

### <a name="does-azure-ad-b2b-collaboration-allow-b2b-users-tooaccess-hello-azure-portal"></a>Azure AD B2B együttműködés lehetővé teszi a B2B felhasználók tooaccess hello Azure-portálon?
Kivéve, ha a felhasználó szerepét hello korlátozott jogokkal rendelkező rendszergazda vagy a globális rendszergazda, a B2B együttműködés felhasználók nem szükséges hozzáférési toohello Azure-portálon. Korlátozott vagy globális rendszergazdája hello szerepe hozzárendelt B2B együttműködés felhasználók azonban hello portál eléréséhez. Ha nincs hozzárendelve a rendszergazdai szerepkörök valamelyikét egy Vendég felhasználó hozzáfér a hello portal, hello felhasználói előfordulhat, hogy is képes tooaccess egyes részeinek hello élmény. hello Vendég felhasználói szerepkör hello directory bizonyos jogokkal rendelkezik.

### <a name="can-i-block-access-toohello-azure-portal-for-guest-users"></a>Letilthatom az Azure-portál a vendégfelhasználók hozzáférés toohello?
Igen! Ez a házirend beállításakor lehet gondos tooavoid véletlenül blokkoló hozzáférés toomembers és a rendszergazdák.
tooblock a Vendég felhasználó hozzáférési toohello [Azure-portálon](https://portal.azure.com), használja a feltételes hozzáférési házirend a Windows Azure klasszikus telepítési modell API hello:
1. Módosítsa a hello **minden felhasználó** csoportot, hogy csak a tagot tartalmaz.
  ![képernyőfelvétel a hello csoport módosítása](media/active-directory-b2b-faq/modify-all-users-group.png)
2. Hozzon létre egy dinamikus csoportot, amely tartalmazza a vendégfelhasználók számára.
  ![képernyőfelvétel a csoport létrehozása](media/active-directory-b2b-faq/group-with-guest-users.png)
3. Beállítása egy feltételes hozzáférési házirend tooblock vendégfelhasználók hello portal hozzáférését, látható módon hello következő videót:
  
  > [!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-block-guest-user/Player] 

### <a name="does-azure-ad-b2b-collaboration-support-multi-factor-authentication-and-consumer-email-accounts"></a>Többtényezős hitelesítés és a fogyasztói e-mail fiókok támogatja Azure AD B2B együttműködés?
Igen. Többtényezős hitelesítés és a fogyasztói e-mail fiókok is támogatott az Azure AD B2B együttműködés.

### <a name="do-you-plan-toosupport-password-reset-for-azure-ad-b2b-collaboration-users"></a>Tervezi az új Azure AD B2B együttműködés felhasználók toosupport jelszó?
Igen. Az alábbiakban az önkiszolgáló jelszó-visszaállításból (SSPR) B2B felhasználó számára a fiókpartner-szervezet felkérik hello fontos részleteket:
 
* SSPR csak hello B2B felhasználói identitás bérlő hello történik.
* Ha hello identitás bérlői Microsoft-fiókkal, Microsoft-fiók SSPR mechanizmus hello szolgál.
* Ha hello identitás bérlői egy közvetlenül az igény (szerinti JIT) vagy a "ugrásszerű" bérlői, a jelszó alaphelyzetbe állítása e-mailt küld el.
* Más bérlők hello normál SSPR folyamat B2B felhasználók követi. SSPR tag B2B felhasználók hello környezetben hello erőforrás, például bérleti le van tiltva. 

### <a name="is-password-reset-available-for-guest-users-in-a-just-in-time-jit-or-viral-tenant-who-accepted-invitations-with-a-work-or-school-email-address-but-who-didnt-have-a-pre-existing-azure-ad-account"></a>Jelszó alaphelyzetbe áll rendelkezésre a vendégfelhasználók a közvetlenül az igény (szerinti JIT) a, vagy "ugrásszerű" bérlői elfogadó meghívókat munkahelyi vagy iskolai e-mail címet, de nem volt, akik egy már meglévő Azure AD-fiókot?
Igen. A jelszó alaphelyzetbe állítása e-mail küldhető, amely lehetővé teszi, hogy egy felhasználó tooreset jelszavukat hello JIT-bérlőhöz.

### <a name="does-microsoft-dynamics-crm-provide-online-support-for-azure-ad-b2b-collaboration"></a>Nem Microsoft Dynamics CRM online támogatást nyújthat olyan Azure AD B2B együttműködés?
Jelenleg Microsoft Dynamics CRM nem online támogatást nyújt az Azure AD B2B együttműködés. Azonban tervezzük toosupport Ez a jövőbeni hello.

### <a name="what-is-hello-lifetime-of-an-initial-password-for-a-newly-created-b2b-collaboration-user"></a>Mi az a hello élettartama egy kezdeti jelszó B2B együttműködés újonnan létrehozott felhasználó számára?
Az Azure AD tartozik egy rögzített karakter, a jelszó erősségét és a fiók zárolása vonatkozó követelmények egyaránt tooall az Azure AD felhőalapú felhasználói fiókokhoz. Felhő felhasználói fiókok azok a fiókok, a rendszer nem összevont egy másik identitásszolgáltatóval, például a 
* Microsoft-fiók
* Facebook
* Az Active Directory összevonási szolgáltatások
* Egy másik felhőben bérlőt (B2B együttműködés)

Az összevont partnerek jelszóházirend hello helyszíni bérleti és hello felhasználó Microsoft-fiókjának beállításai alkalmazott hello házirend függ.

### <a name="an-organization-might-want-toohave-different-experiences-in-their-applications-for-tenant-users-and-guest-users-is-there-standard-guidance-for-this-is-hello-presence-of-hello-identity-provider-claim-hello-correct-model-toouse"></a>Egy szervezet érdemes különböző toohave észlel, az alkalmazásokban a bérlő felhasználók és a vendégfelhasználók számára. A szokásos útmutató van? Az hello identity provider jogcím hello megfelelő modell toouse hello jelenléte?
 A Vendég felhasználó bármely identity provider tooauthenticate használhatja. További információkért lásd: [B2B együttműködés felhasználó tulajdonságainak](active-directory-b2b-user-properties.md). Használjon hello **UserType** tulajdonság toodetermine felhasználói élményt. Hello **UserType** jogcím jelenleg nem része a hello jogkivonat. Alkalmazások a hello felhasználói és tooget hello UserType hello Graph API tooquery hello directory használata ajánlott.

### <a name="where-can-i-find-a-b2b-collaboration-community-tooshare-solutions-and-toosubmit-ideas"></a>Hol található a B2B együttműködés közösségi tooshare megoldások és toosubmit ötleteket?
Azt a most tooyour visszajelzést, tooimprove B2B együttműködés folyamatosan figyeli. Azt, tooshare a felhasználó meghívása forgatókönyvek, ajánlott eljárások és például Azure AD B2B együttműködés kapcsolatban. Hello vitafórum csatlakozott hello [a Microsoft technikai közösségi](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).
 
Azt is meghívót, hogy toosubmit az ötletek és a jövőbeli szolgáltatásokat szavazzon [B2B együttműködés ötleteket](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B-Ideas/idb-p/AzureAD_B2B_Ideas).

### <a name="can-we-send-an-invitation-that-is-automatically-redeemed-so-that-hello-user-is-just-ready-toogo-or-does-hello-user-always-have-tooclick-through-toohello-redemption-url"></a>Nem elküldeni, amely automatikusan beváltott, úgy, hogy a felhasználó hello értéke csak "kész toogo" meghívót? Vagy nem hello felhasználó mindig rendelkezik tooclick keresztül toohello érvényesítési URL-címet?
A szervezet számára is a tagja a fiókpartner-szervezet hello meghívása hello felhasználó által küldött meghívót nem igényelnek érvényesítési hello B2B felhasználó.

Azt javasoljuk, hogy egy felhasználó hello partner szervezet toojoin hello felkéri a szervezet a meghívott. [A felhasználó toohello Vendég meghívó szerepkör hozzáadása hello erőforrás-szervezetben](active-directory-b2b-add-guest-to-role.md). Ez a felhasználó hello fiókpartner-szervezet belüli más felhasználók is meghívhatja hello bejelentkezés használata felhasználói felületén, a PowerShell-parancsfájlok, vagy API-t. Ezt követően B2B együttműködés felhasználóit, hogy a szervezet nem szükséges tooredeem a meghívást.

### <a name="how-does-b2b-collaboration-work-when-hello-invited-partner-is-using-federation-tooadd-their-own-on-premises-authentication"></a>Hogyan működik a B2B együttműködés a meghívott hello partner van összevonási tooadd a saját helyszíni hitelesítés használatakor?
Ha hello partner, amely össze van vonva az Azure AD-bérlő toohello helyszíni hitelesítési infrastruktúráját, helyszíni egyszeri bejelentkezés (SSO) automatikusan érhető el. Ha a hello partner Azure AD-bérlő nem rendelkezik, az Azure AD-fiókot az új felhasználók jön létre. 

### <a name="i-thought-azure-ad-b2b-didnt-accept-gmailcom-and-outlookcom-email-addresses-and-that-b2c-was-used-for-those-kinds-of-accounts"></a>I-re Azure AD B2B nem fogadta el a gmail.com és Outlook.com-os e-mail címet, és a B2C használatát az ilyen típusú fiókok?
Megszüntetjük hello különbségei B2B és üzleti-vállalati (B2C) együttműködés szempontjából identitások támogatottak. hello identitását nincs a helyes OK toochoose B2B vagy B2C használatával között. Az együttműködési lehetőség kiválasztásával kapcsolatos információkért lásd: [összehasonlítása B2B együttműködés és az Azure Active Directory B2C](active-directory-b2b-compare-b2c.md).

### <a name="what-applications-and-services-support-azure-b2b-guest-users"></a>Milyen alkalmazások és szolgáltatások támogatják az Azure B2B vendégfelhasználók?
Minden Azure Active Directoryba integrált alkalmazások Azure B2B vendég felhasználók támogatására. 

### <a name="can-we-force-multi-factor-authentication-for-b2b-guest-users-if-our-partners-dont-have-multi-factor-authentication"></a>A Microsoft kényszerítheti a többtényezős hitelesítést a B2B vendégfelhasználók Ha partnereink nem rendelkezik a multi-factor authentication?
Igen. További információkért lásd: [feltételes hozzáférés a B2B együttműködés felhasználók](active-directory-b2b-mfa-instructions.md).

### <a name="in-sharepoint-you-can-define-an-allow-or-deny-list-for-external-users-can-we-do-this-in-azure"></a>A SharePoint definiálhat egy külső felhasználók "engedélyezése" vagy "Elutasítás" listája. A Microsoft ehhez az Azure-ban?
Igen. Az Azure AD B2B együttműködés támogatja engedélyezési listák, és letiltási listáját. 

### <a name="what-licenses-do-we-need-toouse-azure-ad-b2b"></a>Licencek mire igazolnia kell, hogy Azure AD B2B toouse?
Mi licencek, a szervezet információt szükség van az Azure AD B2B toouse, lásd: [útmutatást licencelési Azure Active Directory B2B együttműködés](active-directory-b2b-licensing.md).

### <a name="next-steps"></a>Következő lépések

Ismerje meg az Azure AD B2B együttműködés további cikkeit:

* [Mi az az Azure AD B2B együttműködés?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Hogyan rendszergazdák az Azure AD B2B együttműködés felhasználók hozzá?](active-directory-b2b-admin-add-users.md)
* [Hogyan hozzá az információkkal dolgozó szakemberek B2B együttműködés felhasználók?](active-directory-b2b-iw-add-users.md)
* [hello B2B együttműködés meghívó e-mail hello elemei](active-directory-b2b-invitation-email.md)
* [B2B együttműködés meghívó érvényesítési](active-directory-b2b-redemption-experience.md)
* [Az Azure AD B2B együttműködés licencelés](active-directory-b2b-licensing.md)
* [Hibaelhárítás az Azure AD B2B együttműködés](active-directory-b2b-troubleshooting.md)
* [Az Azure AD B2B együttműködés API és a Testreszabás](active-directory-b2b-api.md)
* [Többtényezős hitelesítés a B2B-együttműködés felhasználói számára](active-directory-b2b-mfa-instructions.md)
* [Adja hozzá a B2B együttműködés felhasználók nélkül](active-directory-b2b-add-user-without-invite.md)
* [Alkalmazások kezelése az Azure AD-cikk indexe](active-directory-apps-index.md)
