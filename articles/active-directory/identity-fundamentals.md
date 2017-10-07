---
title: "az Azure identitásfelügyelet aaaFundamentals |} Microsoft Docs"
description: 
keywords: 
author: jeffgilb
manager: femila
ms.reviewr: jsnow
ms.author: jeffgilb
ms.date: 7/5/2017
ms.topic: article
ms.prod: 
ms.service: azure
ms.technology: 
ms.assetid: 
ms.custom: it-pro
ms.openlocfilehash: a9710b8e543cdbb2f78ea9e3f83b183e1983b31d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="fundamentals-of-azure-identity-management"></a>Azure-identitás kezelése – alapok
Több vállalati digitális erőforrások élő hello vállalati hálózaton, hello felhőben, és az eszközökön kívül, felhőalapú identitás- és hozzáférés felügyeleti kiválóan áttelepítésére válik. Felhőalapú Identitások is hello legjobb módja toomaintain vezérlő felett, és lássák, hogyan és mikor történjen a felhasználók férhetnek hozzá a vállalati alkalmazásokhoz és adatokhoz.

Microsoft rendelkezik lett felhőalapú identitásainak keresztül egy évtizedben és védelme, a [Azure Active Directory (AD)](https://docs.microsoft.com/azure/active-directory/active-directory-editions), ezek azonos védelmi rendszerek nem érhető el tooyou. Az Azure ad-vel a vállalati rendszergazdák könnyen arról, hogy felhasználói és rendszergazdai elszámolási kötelezettségéről szóló nagyobb biztonsággal és irányítási eddiginél.

Prémium szintű Azure AD egy olyan felhőalapú identitás- és hozzáférés felügyeleti megoldás speciális védelem képességekkel bővült, amely lehetővé teszi az összes olyan alkalmazáshoz, identity protection egy biztonságos identitás (hello fejlettebb [Microsoft eszközintelligencia biztonsági diagramot](https://www.microsoft.com/en-us/security/intelligence)), és a Privileged Identity Management. Nem csupán egy másik figyelés vagy jelentéskészítési eszköz, prémium szintű Azure AD a felhasználói identitások valós idejű megvédheti, illetve akkor toocreate kockázat, adaptív hozzáférési házirendek tooprotect engedélyezése a szervezet adatait.

A rövid videó az Azure AD identity kezelésén és védelmén gyors áttekintést:
<iframe width="560" height="315" src="https://www.youtube.com/embed/9LGIJ2-FKIM" frameborder="0" allowfullscreen></iframe>

A Microsoft csak nem biztosít az identitás, amely everywhere, de is eszközök tooautomate biztonságossá, és kezelheti a szervezeten belüli informatikai. A felhőalapú megoldások van még toomanage igény, és szabályozhatja az informatikai feladatok, például a segélyszolgálati hívások tooreset felhasználói jelszavak hello megjelenésével után is a felhasználói csoport felügyeleti és alkalmazás-kérelmek. Dolgot további része, alkalmazottak most a személyes eszközök toowork gépidőt, és azonnal elérhetők legyenek SaaS-alkalmazások használatával. Így az alkalmazások karbantartása szabályozhatják vállalati adatközpontban és a nyilvános felhő platform között jelentős kihívást.

[!INCLUDE [identity](../../includes/azure-ad-licenses.md)]

## <a name="connect-on-premises-active-directory-with-azure-ad-and-office-365"></a>A helyszíni Active Directory és az Azure AD Connect és az Office 365
A szervezetek, amelyek nagy beruházások történtek a helyszíni Active Directoryban a ezeket beruházások toohello felhő kibővítheti a helyszíni címtárak integrálása az Azure AD- [hibrid Identitáskezelés](https://docs.microsoft.com/azure/active-directory/active-directory-hybrid-identity-design-considerations-overview). Így a felhasználók hatékonyabb, adja meg a közös identitás függetlenül helyétől erőforrások eléréséhez. Felhasználók és a szervezetek csak akkor használja az egyszeri bejelentkezés (SSO) tooaccess mind a helyszíni erőforrásokat, és a felhőalapú szolgáltatások, például az Office 365.

[Az Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) hello csak eszköz tooget hello integrációs kész van szüksége. Azure AD Connect megadja képességek toosupport az identitások szinkronizálására van szüksége, és olyan például a DirSync és Azure AD Sync identitásintegrációs eszközök régebbi verzióit váltja fel. Az Azure AD Connect az Identitáskezelés és a helyszíni és az Azure AD közötti szinkronizálás keresztül engedélyezve van:

- Szinkronizálás – ez az összetevő a felhasználók, csoportok és egyéb objektumok létrehozásáért felelős. Is felelős arról, hogy a helyi felhasználók és csoportok identitásinformációi van megfelelő hello felhő. Jelszóvisszaírás is engedélyezett tookeep a helyszíni címtárakat szinkronban, amikor a felhasználó frissíti a jelszavát az Azure ad-ben.
- Az AD FS - összevonási egy nem kötelező biztosított képesség lehetővé teszi az Azure AD Connect használható használt tooconfigure egy hibrid környezetben helyszíni AD FS infrastruktúra. Összevonási szervezetek tooaddress összetett telepítések, például az egyszeri bejelentkezés, AD bejelentkezési házirend, és az intelligens kártya és a harmadik féltől származó MFA használhatja.
- Állapotfigyelés – [az Azure AD Connect Health](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health) is hatékony megfigyelési képességgel rendelkezik, és adja meg a központi helyet hello Azure portál tooview ezt a tevékenységet.

## <a name="increase-productivity-and-reduce-helpdesk-costs-with-self-service-and-single-sign-on-experiences"></a>A termelékenységet, és csökkenti az ügyfélszolgálati kiadásokat az önkiszolgáló és az egyszeri bejelentkezést

Az alkalmazottak is hatékonyabb, ha egyetlen felhasználónév és jelszó tooremember és egy egységes felhasználói élményt, minden eszközön. Akkor is időmegtakarítás mikor hajthat végre olyan feladatokat, például [elfelejtett jelszó alaphelyzetbe állítása](https://docs.microsoft.com/azure/active-directory/active-directory-passwords) vagy access tooan alkalmazás kérő hello támogató személyzet segítségét várakozás nélkül.

Az Azure AD [kiterjeszti a helyszíni Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) hello felhőbe engedélyezése felhasználók toouse az elsődleges, mind a tartományhoz csatlakozó eszközök, a vállalati erőforrásokhoz, és minden hello web- és SaaS-alkalmazásokhoz szervezeti fiók azok toouse tooget munkájuk elvégzéséhez szükséges. Ezenkívül tooremember felhasználónevek és jelszavak, a felhasználók alkalmazás-hozzáférés több példányban is lehet automatikusan kiépítve (vagy rendszer leépíti) rendelkező toonot alapján szervezet csoporttagságot és állapotuk alkalmazott. És segítségével ellenőrizheti, hogy gyűjtemény alkalmazások vagy a saját helyszíni alkalmazások, amelyek már fejlesztve és közzétéve hello [az Azure AD-alkalmazásproxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started).

## <a name="manage-and-control-access-toocorporate-resources"></a>Kezelhetik és szabályozhatják toocorporate erőforrások eléréséhez.
Microsoft identitások és hozzáférések felügyeleti megoldások Súgó informatikai hozzáférés tooapplications és erőforrások védelmében hello vállalati adatközpontban és hello felhőre, például engedélyezése az érvényesítési további szintek [többtényezőshitelesítés](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) és [feltételes hozzáférési házirendek](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Gyanús tevékenységek figyelése keresztül speciális biztonsági jelentések, a naplózás és a riasztási segít mérsékelni a potenciális biztonsági problémákat.

Feltételes hozzáférési szabályzatokat az Azure AD Premium biztosítanak, vállalati rendszergazdai hello, hello képességét toocreate csoportházirend-alapú hozzáférési szabályok bármely Azure AD-kompatibilis alkalmazás (SaaS-alkalmazásokhoz, hello felhőalapú vagy helyszíni webalkalmazásokban futó egyéni alkalmazások). Az Azure AD ezek a szabályzatok valós időben értékeli ki, és érvénybe lépteti őket, amikor egy felhasználó megpróbál tooaccess kérelmet. Azure-identitás adatvédelmi szabályzatok lehetővé teszik tooautomatically hajtsa végre a megfelelő műveletet, ha gyanús tevékenységet észlel. Ezek a műveletek lehetnek, blokkolja a hozzáférést toousers magas veszélyben, többtényezős hitelesítés kényszerítése, és ha úgy tűnik hitelesítő adatok alaphelyzetbe állítása felhasználói jelszavak biztonsága sérült.


## <a name="azure-active-directory-privileged-identity-management"></a>Azure Active Directory emelt szintű identitáskezelés

[Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started), hello Azure Active Directory Premium P2 mellékelt ajánlat, lehetővé teszi a toodiscover, korlátozása és rendszergazdai fiókok és a hozzáférés tooresources az Azure Active Directory és az egyéb figyelése Microsoft online services. Emellett segítséget nyújt igény rendszergazdai hozzáférés felügyelete hello pontos időre van szüksége.

A privileged Identity Management kényszerítheti igény rendszergazdai jogosultsággal, hogy a rendszergazdák lekérdezhetik-e többtényezős hitelesített, ideiglenes jogok kiterjesztése az előre megadott időszakokra, mielőtt fiókjukat tooa normál vissza felhasználói állapot.

## <a name="benefits-of-azure-identity"></a>Azure-identitás előnyei

Az Azure-identitás-kezelés segítségével:

-   Létrehozásához és kezeléséhez minden felhasználó egy egyetlen identitást a teljes vállalat, felhasználók, csoportok és az eszközök szinkronban tartása [Azure Active Directory Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

-   Egyszeri bejelentkezéses hozzáférést tooyour alkalmazások, beleértve a több ezer előre integrált Szolgáltatottszoftver-alkalmazásoknál, vagy biztonságos távoli hozzáférés tooon helyszíni SaaS-alkalmazások használatával hello [az Azure AD-alkalmazásproxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started).

-   Alkalmazás-hozzáférési biztonsági engedélyezése szabályalapú kényszerítésével [multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) a helyszíni és felhőalapú alkalmazások.

-   A felhasználói termelékenység növelése [önkiszolgáló jelszó-átállítási](https://docs.microsoft.com/azure/active-directory/active-directory-passwords), és a csoporthoz, és a hozzáférési kérelmek hello történő alkalmazás [MyApps portal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-user-help).

-   Hello előnyeit [magas rendelkezésre állása és megbízhatósága](https://docs.microsoft.com/azure/architecture/resiliency/high-availability-azure-applications) egy világszerte, vállalati szintű, a felhőalapú identitás- és hozzáférés felügyeleti megoldás.

## <a name="next-steps"></a>Következő lépések
[További tudnivalók az Azure identitáskezelési megoldásairól](https://docs.microsoft.com/azure/active-directory/understand-azure-identity-solutions)