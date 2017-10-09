---
title: az Azure AD Privileged Access aaaSecuring |} Microsoft Docs
description: "Ez a témakör azt ismerteti, hello megközelíti Azure, az Azure Active Directory és a Microsoft Online Services rendszerjogosultságú hozzáférés biztosítása érdekében."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: mwahl
ms.assetid: 235a0ce9-1daf-4433-8f65-9c6afcd64d08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: kgremban
ms.custom: pim
ms.openlocfilehash: 694835dc5c41640673dbd996d44b0d1f217220de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="securing-privileged-access-in-azure-ad"></a>Az Azure Active Directory biztonságossá tétele a privilegizált hozzáférési jogosultsága.
Biztonságossá tétele a privileged access kritikus az első lépés toohelp modern szervezet üzleti eszközök védelme. Kiemelt jogosultságú fiókok azok a fiókok, felügyelheti és kezelheti az informatikai rendszerek. A támadók számítógépes ezen fiókok toogain hozzáférés tooan szervezeti adatok és rendszerek célként. toosecure emelt szintű hozzáférés, hello fiókokat kell elkülönítése, és rendszerek hello kockázata, hogy a feltárt tooa rosszindulatú felhasználó.

További felhasználók indító tooget emelt szintű hozzáférés felhőalapú szolgáltatások segítségével. Ilyen lehet például az Office365 globális rendszergazdák, Azure-előfizetés rendszergazdák és felhasználók rendszergazdai hozzáféréssel rendelkező virtuális gépek vagy SaaS-alkalmazásokhoz.

A Microsoft azt javasolja, hogy kövesse a terv [emelt szintű hozzáférés biztonságossá tétele](https://technet.microsoft.com/library/mt631194.aspx).

A felhasználó Azure Active Directory, Office 365, vagy más Microsoft-szolgáltatások és alkalmazások esetén az alapelvek alkalmazni, hogy a felhasználói fiókok által kezelt és hitelesítése az Active Directory vagy az Azure Active Directoryban. hello következő szakaszokban további információt az emelt szintű hozzáférés biztonságossá tétele az Azure AD-szolgáltatások toosupport.

## <a name="azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication
rendszergazda hitelesítés tooincrease hello biztonságát, érdemes beállítani kétlépéses ellenőrzés jogosultságok megadása előtt. Kétlépéses ellenőrzés egy metódust, akik áll, amelyek ellenőrzése csupán felhasználónévvel és jelszóval hello használatát igényli. Biztonsági toouser bejelentkezéseket és tranzakciókat második réteget biztosít.

Az Azure multi-factor Authentication (MFA) egy a Microsoft kétlépéses ellenőrzés megoldás, mely segítségével megakadályozhatja hozzáférés toodata és alkalmazások mellett egyszerű bejelentkezési folyamatot a felhasználó igény szerint. Erős hitelesítés, többek között könnyen ellenőrzési lehetőségek széles keresztül biztosítja:

- telefonhívásokat
- Szöveges üzenetek
- mobilalkalmazás-értesítések
- mobilalkalmazás ellenőrző kódok kezelésére
- Harmadik féltől származó OATH jogkivonatokkal

Azure multi-factor Authentication működése áttekintését lásd a következő videó hello:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]

További információkért lásd: [többtényezős hitelesítés az Office 365 és az Azure MFA](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).

## <a name="time-bound-privileges"></a>Időhöz kötött jogosultságokkal
Egyes szervezetek tapasztalhatja, hogy túl sok felhasználó rendelkeznek magas szintű jogosultsággal rendelkező szerepkört. A felhasználó esetleg hozzá vannak adva egy adott tevékenységet, például az egy service szolgáltatáshoz toosign toohello szerepkör, de nem a gyakran ezt követően ezeket az engedélyeket.

toolower hello kitettség idő jogosultságokat, és növelheti a használatukat korlát felhasználók tooonly véve a jogosultságait az "igény szerint" láthatósága (JIT), ha egy feladat tooperform van szükségük. Az Azure Active Directory és a Microsoft Online Services használhatja [az Azure AD Privileged Identity Management (PIM)](http://aka.ms/AzurePIM).

![A PIM irányítópult][2]

## <a name="attack-detection"></a>Észlelt támadás
[Az Azure Active Directory Identity Protection](../active-directory-identityprotection.md) a kockázati eseményekről és a szervezet identitásait érintő lehetséges biztonsági rések egyesített nézetét biztosítja. Kockázati események alapján Identity Protection számítja ki a felhasználó kockázati szint minden felhasználóhoz, így már tooconfigure kockázati-alapú házirendek tooautomatically védelme hello identitások a szervezet. Ezek a házirendek más feltételes hozzáférés-szabályozási EMS, és az Azure Active Directory által biztosított automatikusan hello felhasználó blokkolása vagy ad javaslatokat, amelyek tartalmazzák a jelszó alaphelyzetbe állítását és a többtényezős hitelesítés kényszerítése.

![Azure AD Identity Protection][3]

## <a name="conditional-access"></a>Feltételes hozzáférés
Feltételes hozzáférés-vezérlést Azure Active Directory hello megadott feltételek úgy dönt, hogy a felhasználó hitelesítéséhez access tooan alkalmazás engedélyezése előtt ellenőrzi. Ha ezek a feltételek teljesülnek, hello felhasználó hitelesítése és hozzáférési toohello alkalmazás engedélyezve.

![Feltételes hozzáférési szabályok a multi-factor Authentication szolgáltatás beállítása][4]

## <a name="related-articles"></a>Kapcsolódó cikkek
* Engedélyezése [Azure többtényezős hitelesítés](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)
* Engedélyezése [az Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-configure.md)
* Engedélyezése [Azure AD Identity Protection](../active-directory-identityprotection.md)
* Engedélyezése [feltételes hozzáférés-vezérlést](../active-directory-conditional-access.md)

További információk egy teljes biztonsági terv, című hello "ügyfél feladatkörei és terv" hello [vállalati fejlesztők a Microsoft Cloud biztonsági](http://aka.ms/securecustomer) dokumentum. A Microsoft-szolgáltatások tooassist egyetlen ezek a témakörök végezhet további tájékoztatásért forduljon a Microsoft képviselőjével, vagy keresse fel a [számítógépes megoldások lap](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
[4]: ../media/active-directory-conditional-access/conditionalaccess-saas-apps.png
