---
title: "aaaAzure biztonsági funkciói segítenek az identity management alkalmazással |} Microsoft Docs"
description: " Ez a cikk áttekintést hello alapvető Azure biztonsági funkcióit, amelyek segítenek az identity management alkalmazással. Microsoft identitások és hozzáférések felügyeleti megoldások Súgó informatikai hozzáférés tooapplications és erőforrások védelmében hello vállalati adatközpontban és hello felhőre, például a többtényezős hitelesítés és a feltételes érvényesítési további szinteket engedélyezése hozzáférési házirendek. "
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 5aa0a7ac-8f18-4ede-92a1-ae0dfe585e28
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/09/2017
ms.author: terrylan
ms.openlocfilehash: f08e4f6cf2e48e455a16858b7fee08b53d5aa585
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-identity-management-security-overview"></a>Azure-identitás biztonsági – áttekintés
Microsoft identitások és hozzáférések felügyeleti megoldások Súgó informatikai hozzáférés tooapplications és erőforrások védelmében hello vállalati adatközpontban és hello felhőre, például a többtényezős hitelesítés és a feltételes érvényesítési további szinteket engedélyezése hozzáférési házirendek. Gyanús tevékenységek figyelése keresztül speciális biztonsági jelentések, a naplózás és a riasztási segít mérsékelni a potenciális biztonsági problémákat. [Az Azure Active Directory Premium](../active-directory/active-directory-editions.md) biztosítja az egyszeri bejelentkezés toothousands felhőalapú (SaaS) alkalmazások és az access tooweb alkalmazások helyi futtatása.

Biztonsági szempontból előnyökkel járhat az Azure Active Directory (AD) megadhatják hello:

* Létrehozásához és kezeléséhez minden felhasználó egy egyetlen identitást a hibrid vállalat, felhasználók, csoportok és az eszközök szinkronban tartása
* Egyszeri bejelentkezéses hozzáférést több ezer előre integrált Szolgáltatottszoftver-alkalmazásoknál például tooyour alkalmazások biztosítása
* Engedélyezze a hozzáférést alkalmazásbiztonsági mind a helyszíni szabályalapú többtényezős hitelesítés kényszerítése, és a felhőalapú alkalmazásokhoz
* Rendelkezés biztonságos távoli hozzáférés tooon helyszíni webalkalmazást az Azure AD-alkalmazásproxy használatával

hello Ez a cikk célja tooprovide hello alapvető Azure biztonsági funkcióit, amelyek segítenek az identity management alkalmazással áttekintését. Azt is biztosítanak, amelyek egyes szolgáltatások részleteit biztosítanak, így további hivatkozások tooarticles.  

hello a cikk a következő alapvető Azure identitáskezelési funkciói hello koncentrál:

* Egyszeri bejelentkezés
* Fordított proxy
* Multi-Factor Authentication
* Biztonsági figyelést, a riasztások és a machine learning-alapú jelentések
* A felhasználói identitások és hozzáférés kezelése
* Eszközregisztráció
* Privileged identity management
* Identity Protection
* Hibrid Identitáskezelés

## <a name="single-sign-on"></a>Egyszeri bejelentkezés
Egyszeri bejelentkezés (SSO) azt jelenti, hogy képes tooaccess alatt álló összes hello alkalmazásokat és erőforrásokat, hogy kell-e toodo üzleti történő bejelentkezéssel csak egyszer egyetlen felhasználói fiókkal. Miután bejelentkezett, van-e hozzáférési összes hello alkalmazás anélkül, hogy a szükséges tooauthenticate van szüksége (például adjon meg egy jelszót) még egyszer.

Számos szervezet számítson arra, hogy szoftver alkalmazásokként egy szolgáltatott szoftverként (SaaS) például Office 365, a mezőben és a Salesforce a végfelhasználó hatékonyságát. Hagyományosan informatikai munkatársak szükséges tooindividually létrehozása és frissítése minden SaaS-alkalmazás felhasználói fiókokat, és felhasználók tooremember minden SaaS-alkalmazáshoz tartozó jelszót.

Az Azure AD kiterjeszti a helyszíni Active Directory-környezeteket hello felhőbe engedélyezése a felhasználók toouse a szervezeti fiók elsődleges toonot csak bejelentkezési tootheir tartományhoz csatlakoztatott eszközökre és a vállalati erőforrásokhoz, de is összes hello web- és SaaS-alkalmazásokhoz a feladat szükséges.

Nem csak felhasználók nem rendelkeznek toomanage több példányban felhasználónevei és jelszavai, alkalmazás-hozzáférés csak alapján automatikusan kiosztott vagy vonja kiosztott szervezeti csoportok és az állapotuk egy alkalmazott. Az Azure AD biztonsági vezet be, és hozzáférési irányítás vezérlők, amelyek lehetővé teszik, hogy toocentrally kezelését a felhasználói hozzáférés SaaS-alkalmazásokhoz.

További információ:

* [Az egyszeri bejelentkezés – áttekintés](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](../active-directory/active-directory-appssoaccess-whatis.md)
* [Azure Active Directoryval az egyszeri bejelentkezés integrálása SaaS-alkalmazásokhoz](../active-directory/active-directory-sso-integrate-saas-apps.md)

## <a name="reverse-proxy"></a>Fordított proxy
Az Azure AD-alkalmazásproxy lehetővé teszi, hogy a helyszíni alkalmazások, például közzététele [SharePoint](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US) helyek, [Outlook Web App](https://technet.microsoft.com/library/jj657718.aspx), és [IIS](http://www.iis.net/)-alapú alkalmazások a magánhálózaton belül és a hálózaton kívüli biztonságos hozzáférést toousers biztosít. Távoli hozzáférést biztosít az alkalmazás Proxy és egyszeri bejelentkezés (SSO) számos különböző helyszíni hello több ezer alkalmazások az Azure AD támogatja a Szolgáltatottszoftver-alkalmazáshoz. Az alkalmazottak tooyour alkalmazásokat is bejelentkezhetnek a saját eszközeik otthoni és a felhő alapú proxyn keresztül történő hitelesítéséhez.

További információ:

* [Az Azure AD-alkalmazásproxy engedélyezése](../active-directory/active-directory-application-proxy-enable.md)
* [Az Azure AD-alkalmazásproxy használó alkalmazások közzététele](../active-directory/active-directory-application-proxy-publish.md)
* [Single-sign-on a Proxy](../active-directory/active-directory-application-proxy-sso-using-kcd.md)
* [Feltételes hozzáférés használata](../active-directory/active-directory-application-proxy-conditional-access.md)

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication
Az Azure többtényezős hitelesítés (MFA), amely hello egynél több ellenőrzési módszer használatát igényli, és a kritikus fontosságú második réteget biztonsági toouser bejelentkezéseket és tranzakciókat ad hitelesítési mód. MFA segít a biztonságos működés érdekében hozzáférés toodata és alkalmazások mellett egyszerű bejelentkezési folyamatot a felhasználó igény szerint. Erős hitelesítés, ellenőrzési lehetőségek széles keresztül biztosítja – a telefonhívás, szöveges üzenet vagy mobilalkalmazás értesítés vagy ellenőrző kód és a külső OAuth jogkivonatokat.

További információ:

* [Többtényezős hitelesítés](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Mi az az Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)
* [Azure multi-factor Authentication működése](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Biztonsági figyelést, a riasztások és a machine learning-alapú jelentések
A biztonság ellenőrzése és a riasztások és a machine learning-alapú jelentések, amelyek azonosítják az inkonzisztens hozzáférési mintázatokat segítségével védelmet az üzleti. Használhatja az Azure Active Directory hozzáférési és használati jelentések toogain láthatósága hello adatintegritási és biztonsági a szervezete címtárát. Ezt az információt a directory-rendszergazda is jobban meghatározhatja, ahol lehetséges biztonsági kockázatokat, hogy azok megfelelően megtervezheti toomitigate kockázatok vizsgálandó.

Hello a klasszikus Azure portálon, a jelentések szerint vannak kategóriába sorolva hello a következő módon:

* Anomáliadetektálási jelentések – jelentkezzen be, hogy észleltünk toobe rendellenes eseményeket tartalmazza. Célunk toomake tud-e a tevékenység, és lehetővé teszik a toobe képes toomake arról, hogy az esemény gyanúsnak meghatározása.
* Integrált alkalmazás jelentések – betekintést, hogyan használja a szervezet a felhőalapú alkalmazásokhoz. Az Azure Active Directory integrálható a felhőalapú alkalmazások ezer.
* Hibajelentések – fiókok tooexternal alkalmazások létesítésekor előforduló hibákat jelzik.
* Felhasználó-specifikus jelentései – eszköz/sign tevékenységek adatai egy adott felhasználó jelenítenek meg.
* Tevékenységi naplóit – tartalmazhat Feljegyzés hello belül minden naplózott események az elmúlt 24 órában, az utolsó 7 napig, vagy utolsó 30 nap során, és a csoport tevékenység módosításainak és jelszó alaphelyzetbe állítása és nyilvántartási tevékenység.

További információ:

* [A hozzáférési és használati jelentések megtekintése](../active-directory/active-directory-view-access-usage-reports.md)
* [Ismerkedés az Azure Active Directory-jelentéskészítés](../active-directory/active-directory-reporting-getting-started.md)
* [Az Azure Active Directory-jelentéskészítés – útmutató](../active-directory/active-directory-reporting-guide.md)

## <a name="consumer-identity-and-access-management"></a>A felhasználói identitások és hozzáférés kezelése
Az Azure Active Directory B2C egy magas rendelkezésre állású, globális, identitás szolgáltatást a felhasználók felé néző alkalmazások identitások millióinak toohundreds méretezi. Mobil- és webes platformokba is integrálható. A felhasználók bejelentkezhetnek tooall, testre szabható felhasználói élmény mellett az alkalmazások új vagy meglévő közösségi fiókjaik használatával.

Az elmúlt hello alkalmazásfejlesztők számára toosign fel, és jelentkezzen be a fogyasztói alkalmazásokba integrálhassák lenne írt a saját kód. És ezeket használja, a helyszíni adatbázisokat vagy rendszereket toostore felhasználóneveket és jelszavakat. Az Azure Active Directory B2C kínál a szervezet egy jobb módon toointegrate felhasználói Identitáskezelés alkalmazásokba hello segítségével biztonságos, szabványokon alapuló platformja és bővíthető szabályzatainak számos.

Azure Active Directory B2C használata esetén a felhasználók regisztrálhatnak az alkalmazások új hitelesítő adatok (e-mail címet és jelszót, vagy felhasználónév és jelszó) létrehozásával vagy meglévő közösségi fiókjaik használatával (Facebook, Google, Amazon, LinkedIn).

További információ:

* [Mi az Azure Active Directory B2C?](https://azure.microsoft.com/services/active-directory-b2c/)
* [Azure Active Directory B2C előzetes verzió: bejelentkezés regisztrálása és beléptetése az alkalmazások a fogyasztói](../active-directory-b2c/active-directory-b2c-overview.md)
* [Az Azure Active Directory B2C előzetes verziója: Típusú alkalmazások](../active-directory-b2c/active-directory-b2c-apps.md)

## <a name="device-registration"></a>Eszközregisztráció
Az Azure AD Eszközregisztrációval szolgáltatás hello alapja eszközalapú [feltételes hozzáférés](../active-directory/active-directory-conditional-access-device-registration-overview.md) forgatókönyvek. Amikor regisztrál egy eszközt, az Azure Active Directory Eszközregisztrációs biztosít hello eszköz, amely használt tooauthenticate hello eszköz hello felhasználó bejelentkezésekor identitással. hello hitelesített eszköz és hello eszköz attribútumai – hello, majd lehet használt tooenforce feltételes hozzáférési házirendek hello felhő és a helyszínen tárolt alkalmazások esetében.

Például az Intune mobileszköz-kezelési (MDM) megoldás kombinálva hello eszközattribútumokon az Azure Active Directoryban frissítődik hello eszközzel kapcsolatos további információk. Ez lehetővé teszi toocreate feltételes hozzáférési szabályok, amelyeket eszközök toomeet való hozzáférést a biztonsági és megfelelőségi szabványoknak.

További információ:

* [Ismerkedés az Azure Active Directory Eszközregisztrációjával](../active-directory/active-directory-conditional-access-device-registration-overview.md)
* [Automatikus eszközregisztráció az Azure Active Directory for Windows-tartományhoz csatlakozó eszközök](../active-directory/active-directory-conditional-access-automatic-device-registration.md)
* [Állítsa be az automatikus regisztráció, a Windows Azure Active Directory tartományhoz csatlakozó eszközök](../active-directory/active-directory-conditional-access-automatic-device-registration-setup.md)

## <a name="privileged-identity-management"></a>Privileged identity management
Az Azure Active Directory (AD) Privileged Identity Management lehetővé teszi kezelése, szabályozása és figyelése a kiemelt jogosultságú identitások és az Azure AD hozzáférési tooresources, valamint más Microsoft online szolgáltatások, például az Office 365-öt vagy a Microsoft Intune.

Néha a felhasználóknak kell toocarry ki az Azure vagy az Office 365 erőforrásokhoz, vagy más Szolgáltatottszoftver-alkalmazásoknál privilegizált műveleteket. Ez gyakran azt jelenti, a szervezetek toogive őket állandó jogosultsági szintű hozzáférés az Azure ad-ben. Ez a felhőben üzemeltetett erőforrásokhoz az egyre növekvő biztonsági kockázatot jelent, mert a szervezeteknek elég nem tud figyelni, ezek a felhasználók tevékenységeit a rendszergazda jogosultságokkal. Továbbá ha jogosultsági szintű hozzáféréssel rendelkező felhasználói fiók biztonsága sérül, egy megsértésének jelentős hatással lehet a felhő átfogó biztonsági. Az Azure AD Privileged Identity Management segít tooresolve a kockázat.

Az Azure AD Privileged Identity Management lehetővé teszi:

* Mely felhasználók vannak-e az Azure AD-rendszergazdák
* Engedélyezze az igény, "csak az időben" rendszergazdai hozzáférés tooMicrosoft Online szolgáltatások, például Office 365 és az Intune-ban
* Rendszergazda-hozzárendelések beolvasása a rendszergazdai hozzáférési műveleteiről és a változások
* Értesítéskérés a hozzáférés tooa kiemelt szerepkörű

További információ:

* [Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md)
* [Szerepkörök az Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-roles.md)
* [Az Azure AD Privileged Identity Management: Hogyan tooadd vagy egy felhasználói szerepkör eltávolítása](../active-directory/active-directory-privileged-identity-management-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Identity Protection
Az Azure AD Identity Protection olyan biztonsági szolgáltatás, amely a kockázati eseményekről és a szervezet identitásait érintő lehetséges biztonsági rések egyesített nézetét biztosítja. Azonosító adatok védelmét kihasználja a meglévő Azure Active Directory anomáliadetektálási az észlelési képességek (az Azure AD rendellenes Tevékenységjelentések keresztül elérhető), és vezet be új kockázat típusait, amely valós idejű rendellenességek észlelését.

További információ:

* [Az Azure Active Directory azonosító adatok védelmét](../active-directory/active-directory-identityprotection.md)
* [9. csatornán: Az Azure AD és az Identity: Identity Protection előzetes kiadásának](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="hybrid-identity-management"></a>Hibrid Identitáskezelés
A Microsoft megközelítés tooidentity is lefedik a helyszíni és hello felhő létrehozása egy felhasználói azonosítót a hitelesítéshez és engedélyezéshez tooall erőforrások helyétől függetlenül.

További információ:

* [Hibrid identitáskezelési dokumentáció](http://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
* [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
* [Active Directory csapat blogja](https://blogs.technet.microsoft.com/ad/)
