---
title: "Active Directory koncepció alkalmazástervezési megvalósítási aaaAzure |} Microsoft Docs"
description: "Vizsgálatát, és gyorsan végrehajthatja az identitás- és kezelési helyzetek"
services: active-directory
keywords: "az Azure active directory, a forgatókönyv, a koncepció igazolása, PoC"
documentationcenter: 
author: dstefanMSFT
manager: asuthar
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/12/2017
ms.author: dstefan
ms.openlocfilehash: 4d61f1432d5f1c15cd88fda4824cf1c1de64c712
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-implementation"></a>Az Azure Active Directory igazolása koncepció forgatókönyv: végrehajtása

## <a name="foundation---syncing-ad-tooazure-ad"></a>Foundation - AD tooAzure AD szinkronizálása 

A hibrid identitás szolgáltatás hello vállalati felhasználók, akiknek már van egy helyszíni címtárat a legtöbb hello alapja. hello itt célja toointentionally szerint kevesebb időt itt hello tényleges identitások és hozzáférések forgatókönyvek lehetséges tooshow hello értékként. 

| Forgatókönyv | Építőelemek| 
| --- | --- |  
| [Kiterjeszti a helyszíni identitás toohello felhő](#extending-your-on-premises-identity-to-the-cloud) | [A címtár-szinkronizálás - Jelszókivonat-szinkronizálás](active-directory-playbook-building-blocks.md#directory-synchronization---password-hash-sync-phs---new-installation) <br/>**Megjegyzés:**: Ha már rendelkezik a DirSync/ADSync vagy az Azure AD Connect korábbi verzióiban, ez a lépés nem kötelező megadni. Bizonyos esetekben az útmutató az Azure AD Connect újabb verzióra lehet szükség.  <br/>[Védjegyek](active-directory-playbook-building-blocks.md#branding) | 
| [Csoportok használata az Azure AD-licencek hozzárendelése](#assigning-azure-ad-licenses-using-groups) | [Csoport alapú licencelés](active-directory-playbook-building-blocks.md#group-based-licensing) |


### <a name="extending-your-on-premises-identity-toohello-cloud"></a>Kiterjeszti a helyszíni identitás toohello felhő 

1. Bob hello Active Directory rendszergazdája a Contoso. Azon felhasználók szolgáltatásként kapjuk hello követelmény tooenable identitás. Az Azure AD Connect varázsló végrehajtását követően hello hello azon felhasználók megcélzása elérhető hello felhőben identitását. 
2. Bob Susie, egy hello azon felhasználók megcélzása, megkérdezi, tooaccess hello Azure Active Directory hozzáférési panelre, és győződjön meg arról, hogy ő hitelesítheti. Susie megtekinti egy vállalati arculattal ellátott bejelentkezési oldal és egy üres hozzáférési panel, amely készen áll a jövőbeli alkalmazás-hozzáférés engedélyezése.

### <a name="assigning-azure-ad-licenses-using-groups"></a>Csoportok használata az Azure AD-licencek hozzárendelése 

1. Bob hello Azure AD globális rendszergazda, és szeretne tooallocate az Azure AD licencet tooa a felhasználók adott csoportja hello kezdeti Bevezetés az Azure AD részeként.
2. Bob csoportot hoz létre hello a próbaüzem felhasználóinak. 
3. Bob hello licencek toohello csoporthoz rendeli hozzá.
4. Susie, egy hello információkkal dolgozó szakemberek saját munkakörök részeként kerül toohello biztonsági csoport
5. Némi várakozás után Susie hozzáférés toohello az Azure AD premium licenccel rendelkezik. Ezzel a lépéssel engedélyezi hello Koncepció eseteket később.

## <a name="theme---lots-of-apps-one-identity"></a>Téma - alkalmazások rengeteg, egy identitás

| Forgatókönyv | Építőelemek| 
| --- | --- |  
| [Integrálhatja SaaS-alkalmazásokhoz - összevont egyszeri bejelentkezés](#integrate-saas-applications---federated-sso) | [SaaS összevont egyszeri bejelentkezés konfigurálása](active-directory-playbook-building-blocks.md#saas-federated-sso-configuration) <br/>[Csoportok - delegált tulajdonosa](active-directory-playbook-building-blocks.md#groups---delegated-ownership) |
| [Integrálhatja SaaS-alkalmazásokhoz - jelszó egyszeri bejelentkezés](#integrate-saas-applications---password-sso) | [SaaS-jelszó egyszeri bejelentkezés konfigurálása](active-directory-playbook-building-blocks.md#saas-password-sso-configuration) |
| [Egyszeri Bejelentkezéssel és identitás életciklus-események](#sso-and-identity-lifecycle-events) | [SaaS és identitás életciklusa](active-directory-playbook-building-blocks.md#saas-and-identity-lifecycle) |
| [Biztonságos hozzáférés tooShared fiókok](#secure-access-to-shared-accounts) | [Megosztott SaaS konfigurációja](active-directory-playbook-building-blocks.md#saas-shared-accounts-configuration) |
| [Biztonságos távelérés tooOn helyszíni alkalmazások](#secure-remote-access-to-on-premises-applications) | [Alkalmazás proxykonfigurációt](active-directory-playbook-building-blocks.md#app-proxy-configuration) |
| [LDAP identitások tooAzure AD szinkronizálása](#synchronize-ldap-identities-to-azure-ad) |  [Általános LDAP-összekötő konfigurálása](active-directory-playbook-building-blocks.md#generic-ldap-connector-configuration) |

### <a name="integrate-saas-applications---federated-sso"></a>Integrálhatja SaaS-alkalmazásokhoz - összevont egyszeri bejelentkezés 

1. Bob hello Azure AD globális rendszergazda, és kérelmet kap hello Marketing részlege tooenable hozzáférés tootheir ServiceNow példány. Bob hello részletes oktatóanyag az Azure AD dokumentációjában talál, és azt követő, és delegáltak hello felhasználók toohello app tooKevin, Marketing csapat hello vezetője hozzárendelését. 
2. Kevin ServiceNow jogosultságok hello tulajdonosaként jelentkezik be, és hozzárendeli a Susie toohello alkalmazást. Kevin is észleli, hogy Susie tartozó profil automatikusan ServiceNow jött létre
3. Susie az információkkal dolgozó hello marketingosztály. Azt naplózza tooazure AD és az SaaS-alkalmazásokhoz ő hozzá van rendelve tooin myapps portálon talál. Ott így zökkenőmentesen megkapja a hozzáférési tooServiceNow.
4. hello Marketing részlege azt szeretné, hogy ki fért hozzá a ServiceNow tooaudit. Bob tölt le egy tevékenységgel kapcsolatos jelentés, és azt Kevin megosztja e-mailek keresztül.  

### <a name="sso-and-identity-lifecycle-events"></a>Egyszeri Bejelentkezéssel és identitás életciklus-események

1. Susie tart egy hagyja az hiánya, és a vállalati házirend hello a helyszíni AD-fiókját ideiglenesen le van tiltva. Susie most nem lehet-e jelentkezni tooAzure AD, és ezért nem tud hozzáférni a ServiceNow. 
2. Susie lehetővé teszi a Marketing tooSales oldalirányú áthelyezésre. Kevin saját hozzáférés ServiceNow eltávolítja. Susie hello azure ad myapps bejelentkezik, és azt nem látja a ServiceNow csempe hello. 10 perc elteltével Kevin megerősíti, hogy Susie fiók le lett tiltva, a ServiceNow felügyeleti konzolról.

### <a name="integrate-saas-applications---password-sso"></a>Integrálhatja SaaS-alkalmazásokhoz - jelszó egyszeri bejelentkezés

1. Bob hozzáférési tooAtlassian HipChat konfigurálja. HipChat jelszó SSO-integrációval és támogatás hozzáférés tooSusie rendelkezik
2. Susie toohello myapps portálon naplózza, és megtekinti a hivatkozás toodownload hello Azure AD Internet Explorer böngésző-kiterjesztés, ő letölti
3. Gomb megnyomásakor, ő lekérdezi az HipChat felhasználónévvel és jelszóval hitelesítő adatokat kér. Ez egy egyszeri művelet, és azt befejezése után részéhez, amelyhez hozzáférési tooHipChat
4. Később, néhány nap múlva Susie myapps portál megnyitja és HipChat újra kattint. Ez alkalommal körülbelül így megkapja zökkenőmentes hozzáférést
5. Kevin, hello HipChat alkalmazás tulajdonosa, ki fért hozzá hello alkalmazás tooaudit szeretne. Bob tölti le a jelentést, és azt Kevin megosztja e-mailek keresztül. 

### <a name="secure-access-tooshared-accounts"></a>Biztonságos hozzáférés tooShared fiókok 

1. Bob kiadta toosecure megosztott hello Twitter-leíró hello értékesítési csapat tagjai. Ezután Twitter hozzáadja az SSO alkalmazásként, és hozzárendeli az értékesítési csapat hello toohello biztonsági csoport. Hello hitelesítő adatok megosztott toohello fiókhoz megadott, és ezután megadja az azt hello rendszerben. 
2. Megosztás a Twitteren hitelesítő adatok már nem megbízható tudtán kívül toomultiple személyek miatt. Bob lehetővé teszi, hogy a hello Twitter jelszó automatikus kapcsolódó kulcsváltást.
3. Susie, hello értékesítési csoport tagja toohello myapps portálon naplózza, és a hivatkozás toodownload hello Azure AD Internet Explorer böngésző bővítmény látja. Telepítené.
4. Gomb megnyomásakor ő get való közvetlen hozzáféréshez tooTwitter. Ezzel nem tudja hello jelszót.
5. Arnold is hello értékesítők részét képezi. Azonos állást Susie, 3 – 4. lépéseket a hello rendelkezik
6. hello értékesítési részleg szeretné tooaudit, ki fért hozzá a Twitteren. Bob tölt le egy tevékenységgel kapcsolatos jelentés, és azt Kevin megosztja e-mailek keresztül. 

### <a name="secure-remote-access-tooon-premises-applications"></a>Biztonságos távelérés tooOn helyszíni alkalmazások

1. Bob, az Azure AD globális rendszergazda, hello ágyazott számos kérelmek tooenable alkalmazottak tooaccess számos hasznos a helyszíni erőforrások, például hello költségek alkalmazást, amikor távolról dolgoznak. Ezután a következő hello [alkalmazásproxy dokumentáció](active-directory-application-proxy-enable.md) tooinstall összekötő és alkalmazásproxy alkalmazásként is közzéteheti. 
2. Bob hello külső költségek alkalmazás URL-Címének megosztása Susie, egy hello alkalmazottak, akik távoli hozzáférésre van szüksége. Ő hello hivatkozás fér hozzá, és után AAD hitelesítést, ő képes tooaccess hello költségek alkalmazás, és továbbra is a termelési toobe távoli közben. 
3. Bob Folytatás toopublish további helyszíni alkalmazások hello azonos folyamat és hozzáférési toousers próbálkozik, igény szerint. Feltételes hozzáférés és a többtényezős hitelesítést a hello hozzáad több érzékeny alkalmazások, amelyek ezután tesz közzé, tooensure további biztonsági.

### <a name="synchronize-ldap-identities-tooazure-ad"></a>LDAP identitások tooAzure AD szinkronizálása

1. Bob vállalatnak összetett identitás-infrastruktúra. A legtöbb hello felhasználók karbantartása belül a Windows Server Active Directory tartományi szolgáltatások (ADDS). Egy részüket HR rendszer belül Active Directory Lightweight Directory Services (ADLDS) kezeli.
2. Bob hozzáférési tooSaaS alkalmazások az összes felhasználó számára (is ezek nem szerepelnek ADDS) engedélyezése az biztosítja.
3. Bob ADLDS toopull adatait általános LDAP-összekötő konfigurálása az Azure AD Connectben.
4. Bob szinkronizálási szabályokat hoz létre, így LDAP felhasználók Metaverse és tooAzure AD feltöltése
5. LDAP felhasználó hozzáfér az SaaS alkalmazás használatával alatt Susie szinkronizált identitás



> [!IMPORTANT] 
> Ez a beállítás egy speciális igénylő FIM vagy MIM bizonyos fokú ismeretét. Ha használja termelési, javasoljuk, hogy ez a konfiguráció kapcsolatos kérdéseket halad át [Premier támogatási](https://support.microsoft.com/premier).



## <a name="theme---increase-your-security"></a>Téma - a biztonság növelése 

| Forgatókönyv | Építőelemek| 
| --- | --- |  
| [Biztonságos rendszergazdai fiók hozzáféréssel](#secure-administrator-account-access) | [Az Azure MFA telefonos hívások](active-directory-playbook-building-blocks.md#azure-multi-factor-authentication-with-phone-calls) |
| [Az alkalmazások biztonságos hozzáférést](#secure-access-to-applications) | [Feltételes hozzáférés a Szolgáltatottszoftver-alkalmazáshoz](active-directory-playbook-building-blocks.md#mfa-conditional-access-for-saas-applications) |
| [Csak a felügyelet engedélyezése](#enable-just-in-time-jit-administration) | [Privileged Identity Management](active-directory-playbook-building-blocks.md#privileged-identity-management-pim) |
| [A kockázat alapján identitások védelme](#protect-identities-based-on-risk) | [Kockázati események felderítéséhez](active-directory-playbook-building-blocks.md#discovering-risk-events) <br/>[Bejelentkezés a kockázat házirendek](active-directory-playbook-building-blocks.md#deploying-sign-in-risk-policies) |
| [A jelszavak a Tanúsítványalapú hitelesítés használata nélkül végezzen hitelesítést](#authenticate-without-passwords-using-certificate-based-authentication) | [A Tanúsítványalapú hitelesítés konfigurálása](active-directory-playbook-building-blocks.md#configuring-certificate-based-authentication)

### <a name="secure-administrator-account-access"></a>Biztonságos rendszergazdai fiók hozzáféréssel

1. Bob hello Azure AD globális rendszergazda. Ezután észlelt Stuart társadminisztrátoraként hello szolgáltatást. 
2. Bob tooalways szükséges MFA tooimprove hello biztonságot Stuart tartozó fiók konfigurálása
3. Naplók Stuart toohello Azure-portálon, és értesítéseket, hogy ő kell tooregister a telefon száma toocontinue hello bejelentkezés
4. Stuart a későbbi bejelentkezések során ezzel védetté tette a multi-factor Authentication hitelesítéshez, és most kapjuk a telefonhívás tooverify személyazonosságát.

### <a name="secure-access-tooapplications"></a>Biztonságos hozzáférés tooapplications

1. Kevin ServiceNow hello üzleti tulajdonosa. hello vállalat most szeretne ezen felhasználók toologin többtényezős hitelesítéssel, külső hello vállalati hálózaton való hozzáféréskor.
2. Bob, az Azure AD globális rendszergazda, a feltételes hozzáférési házirend toohello ServiceNow alkalmazás tooenable többtényezős Hitelesítést a külső hozzáférés hozzáadása
3. Susie, az információkkal dolgozó szakember, a alkalmazások portál bejelentkezik, és hello ServiceNow csempére kattint. Ő most megkérdőjelezhető MFA-val.

### <a name="enable-just-in-time-jit-administration"></a>Csak a szerinti (JIT) felügyelet engedélyezése

1. Bob és Stuart az Azure AD globális rendszergazda. Kívánják-tooenable JIT hozzáférés toohello kezelési szerepköröket és is hello hello használata tookeep rekord kiemelt szerepköröket.
2. Bob lehetővé teszi a PIM hello Azure AD-bérlő és hello biztonsági rendszergazdája lesz. Állandó tooeligible saját maga és Stuart tartozó globális rendszergazdai szerepkör tagsági úgy módosítja.
3. Bob és Stuart, most kell hello Azure-portálon keresztül a szerepkör aktiválása előtt végre változik tooAzure AD konfigurációs. 

### <a name="protect-identities-based-on-risk"></a>A kockázat alapján identitások védelme 

1. Susie, az Infomunkás kísérel meg a tor böngészőből van bejelentkezve. 
2. Bob hello Azure AD identity protection-irányítópult ellenőrzi, és névtelen IP-címet a Susie tartozó bejelentkezési látja. hello biztonsági csapat azt akarja, hogy ilyen toochallenge fér hozzá a felhasználóknak többtényezős hitelesítéssel
3. Bob lehetővé teszi, hogy az Azure AD Identity Protection-házirend toochallenge MFA közepes vagy magasabb kockázati események
4. Idő kerül, és Susie jelentkezik be a Tor böngészőből újra. Megadott idő látja hello MFA-kérdést

### <a name="authenticate-without-passwords-using-certificate-based-authentication"></a>A jelszavak a Tanúsítványalapú hitelesítés használata nélkül végezzen hitelesítést

1. Bob globális rendszergazda a pénzügyi intézménynél, amely tiltja a jelszavak használatát egy hitelesítési tényezőként az alkalmazásuk kezelésére.
2. Bob lehetővé teszi, és érvényesíti a az AD FS és az Azure AD a tanúsítványhitelesítés
3. Alkalmazás elérésének pedig Susie kéri tooauthenticate tanúsítvánnyal

## <a name="theme---scale-with-self-service"></a>Téma - méretezéssel önkiszolgáló

| Forgatókönyv | Építőelemek| 
| --- | --- |  
| [Önkiszolgáló jelszóváltoztatás](#self-service-password-reset) | [Önkiszolgáló jelszóváltoztatás](active-directory-playbook-building-blocks.md#self-service-password-reset) |
| [Önkiszolgáló hozzáférés tooApplications](#self-service-access-to-applications) | [Önkiszolgáló hozzáférés tooApplications](active-directory-playbook-building-blocks.md#self-service-access-to-application-management) |

### <a name="self-service-password-reset"></a>Önkiszolgáló jelszóváltoztatás 

1. Bob hello Azure AD globális rendszergazda, és lehetővé teszi, hogy önkiszolgáló Service jelszókezelés tooa részhalmaza Susie is. 
2. Susie bejelentkezik toomyapps portálon, és ellenőrizze, hogy egy üzenet tooregister saját biztonsági adatokat a jövőbeli jelszó-átállítási események.
3. Előre néhány nap múlva, Susie elfelejti a jelszavát, majd visszaállítja az Azure AD portálon keresztül

### <a name="self-service-access-tooapplications"></a>Önkiszolgáló hozzáférés tooApplications 

1. Kevin ServiceNow hello üzleti tulajdonosa. Azt szeretné, hogy a felhasználók túl "regisztráljon" azt az igény szerinti, ahelyett, hogy hozzáadják őket egyszerre
2. Bob, az Azure AD globális rendszergazda, módosítja hello ServiceNow alkalmazás tooenable önkiszolgáló kérelmek
3. Susie, az információkkal dolgozó szakember naplózza a alkalmazások portálon és gombra kell kattintania hello "További alkalmazások felvétele" gombra, és tekintse meg a ServiceNow rendelkezésre álló alkalmazások ajánlott hello. Majd ő hátsó toomy alkalmazások portal nyit, és megtekintheti a hello ServiceNow alkalmazás.

[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]