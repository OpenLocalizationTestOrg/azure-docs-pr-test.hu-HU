---
title: "a klasszikus Azure portálon hello aaaConditional hozzáférés |} Microsoft Docs"
description: "Feltételes hozzáférés-vezérlés az Azure klasszikus portál toocheck hello a megadott feltételek hitelesítéséhez használt a hozzáférési tooapplications."
services: active-directory
keywords: "feltételes hozzáférés tooapps, feltételes hozzáférés az Azure AD-vel biztonságos hozzáférés toocompany erőforrásokat, a feltételes hozzáférési házirendek"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: da3f0a44-1399-4e0b-aefb-03a826ae4ead
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/07/2017
ms.author: markvi
ms.reviewer: calebb
ms.custom: oldportal
ms.openlocfilehash: 049bd3f6ec9e35479319ee7608b007b9a1e16647
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="conditional-access-in-hello-azure-classic-portal"></a>A klasszikus Azure portálon hello feltételes hozzáférés

Ez a témakör tárgya feltételes hozzáférés a klasszikus Azure portálon hello. Feltételes hozzáférés az Azure Active Directory hello információ legutóbbi hello,: [feltételes hozzáférés az Azure Active Directoryban](active-directory-conditional-access-azure-portal.md).


hello vezérlés képességeinek az Azure Active Directory (Azure AD) feltételes hozzáférés ajánlatot egyszerű módon toohelp biztonságos hello felhőalapú és helyszíni erőforrások. Feltételes hozzáférési szabályzatok, például többtényezős hitelesítés szemben biztosítanak védelmet hello kockázatát ellopják és phished hitelesítő adatait. Más feltételes hozzáférési házirendek segítségével a szervezet adatai biztonságát. Például továbbá toorequiring hitelesítő adatokat, lehetséges, hogy egy házirendet, hogy csak regisztrált eszközök esetében a mobileszköz-kezelési rendszerbe például a Microsoft Intune férhetnek hozzá a szervezet bizalmas szolgáltatásokhoz.

## <a name="prerequisites"></a>Előfeltételek
Azure AD feltételes hozzáférés csak a [Azure Active Directory Premium](http://www.microsoft.com/identity). Minden felhasználó, aki hozzáfér a feltételes hozzáférési házirendeket alkalmaztak alkalmazások az Azure AD Premium licenccel kell rendelkeznie. További tudnivalók a licenckövetelmények vonatkoznak [licenccel nem rendelkező felhasználó jelentés](https://aka.ms/utc5ix).

## <a name="how-is-conditional-access-control-enforced"></a>Hogyan kényszeríti ki a feltételes hozzáférés-vezérlést?
Feltételes hozzáférés-vezérléssel helyen, az Azure AD hello adott feltételeket ellenőrzi állítja be a felhasználó tooaccess kérelmet. Miután a hozzáférési követelmények teljesülnek, a hello felhasználó van hitelesítve, és hello alkalmazást érheti el.  

![Feltételes hozzáférés – áttekintés](./media/active-directory-conditional-access/conditionalaccess-overview.png)

## <a name="conditions"></a>Feltételek
Ezek azok a feltételeket, amelyek is megadhat a feltételes hozzáférési szabályzatot:

* **Csoporttagságát**. A csoport tagsága alapján a felhasználói hozzáférés szabályozása.
* **Hely**. Hello hely hello felhasználói tootrigger multi-factor Authentication használata, és a blokk-vezérlők használata, ha egy felhasználó nem megbízható hálózatokon.
* **Eszközplatform**. Használja a hello eszközplatform, például az iOS, Android, Windows Mobile vagy Windows-házirend alkalmazására vonatkozó feltétele.
* **Eszköz-kompatibilis**. Az eszköz állapotát, hogy engedélyezve van vagy le van tiltva, érvényesítése eszköz házirend kiértékelése közben. Ha letilt egy elveszett vagy ellopott eszközt hello könyvtárban, akkor már nem felel meg házirend követelményeinek.
* **Be- és felhasználói kockázati**. Használhat [Azure AD Identity Protection](active-directory-identityprotection.md) a feltételes hozzáférési kockázat házirendekhez. Feltételes hozzáférés kockázat házirendek segítenek, hogy a szervezet előzetes védelme a kockázati eseményekről és a szokatlan bejelentkezési tevékenység alapján.

## <a name="controls"></a>Vezérlők
Használható tooenforce feltételes hozzáférési házirend vezérlőelemek az alábbiak:

* **A multi-factor authentication**. Erős hitelesítés, a többtényezős hitelesítés megkövetelése A multi-factor authentication használható Azure multi-factor Authentication szolgáltatással vagy a helyszíni többtényezős hitelesítési szolgáltató, kombinálja az Active Directory összevonási szolgáltatások (AD FS) használatával. A multi-factor authentication segítségével erőforrások védelme a fent konfigurált egy jogosulatlan felhasználó, aki előfordulhat, hogy rendelkezik kiszolgálószoftvertől toohello érvényes felhasználó hitelesítő adatait.
* **Blokk**. Feltételek, például a felhasználó helye tooblock felhasználói hozzáférés is alkalmazhatja. Letilthatja például hozzáférést, ha egy felhasználó nem megbízható hálózatokon.
* **Az előírásoknak megfelelő eszközök**. Feltételes hozzáférési házirendek hello eszköz szinten állíthatja be. Előfordulhat, hogy beállította egy házirendet, hogy csak a tartományhoz csatlakoztatott számítógépek vagy mobileszközök vannak léptetve egy mobileszköz-kezelési alkalmazás elérhessék a szervezeti erőforrásokhoz. Például használja az Intune toocheck az eszköz megfelelőségét, és jelentést készít az tooAzure kényszerített AD amikor hello felhasználó próbál tooaccess egy alkalmazás. Hogyan toouse Intune tooprotect alkalmazásokat és adatokat, lásd: tartalmaznak részletes útmutatást [alkalmazások és a Microsoft Intune-nal adatainak védelme](https://docs.microsoft.com/intune/deploy-use/protect-apps-and-data-with-microsoft-intune). Intune tooenforce adatvédelem elveszett vagy ellopott eszközök is használja. További információkért lásd: [az adatok védelme teljes vagy szelektív törléssel a Microsoft Intune használatával](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune).

## <a name="applications"></a>Alkalmazások
Kényszerítheti a feltételes hozzáférési szabályzat hello alkalmazás szintjén. Állítsa be a hozzáférési szintek az alkalmazások és szolgáltatások hello felhőalapú vagy helyszíni. hello házirend alkalmazott közvetlenül toohello webhelyre vagy szolgáltatásba. az access toohello böngésző és hello szolgáltatás elérő tooapplications hello szabályzat érvényes.

## <a name="device-based-conditional-access"></a>Eszközalapú feltételes hozzáférés
Korlátozhatja a hozzáférést tooapplications eszközökről, amely regisztrálva van az Azure ad-val, és amelyek megfelelnek a megadott feltételeknek. Eszközalapú feltételes hozzáférési egy szervezet erőforrásaihoz kísérlet tooaccess hello erőforrások a felhasználók nem használhatják:

* Ismeretlen vagy nem felügyelt eszközökön.
* Azok az eszközök, amelyek nem felelnek meg hello biztonsági házirendek beállítása a szervezet.

Ezen követelmények alapján házirendek állíthatja be:

* **Tartományhoz csatlakozó eszközök**. Állítsa be a házirend toorestrict hozzáférés toodevices, a helyszíni Active Directory-tartományhoz csatlakoztatott tooan, és, amely is van regisztrálva az Azure ad-val. Ez a házirend tooWindows asztali gépek, laptopok és vállalati táblagépeken vonatkozik.
  További információ az Azure ad-vel, a tartományhoz csatlakoztatott eszközök automatikus regisztrálása mentése tooset lásd: [beállítása a Windows Azure Active Directory tartományhoz csatlakoztatott eszközök automatikus regisztrálása](active-directory-conditional-access-automatic-device-registration-setup.md).
* **Az előírásoknak megfelelő eszközök**. Állítsa be a házirend toorestrict hozzáférés toodevices megjelölt **megfelelő** hello felügyeleti rendszer könyvtárban. Ez a házirend biztosítja, hogy csak a biztonsági házirendek, például a kényszerítése az eszközön a fájltitkosítást teljesítő eszközöket hozzáférhetnek. A házirend toorestrict hozzáférés a következő eszközök hello használhatja:
  
  * **Windows-tartományhoz csatlakoztatott eszközök**. A System Center Configuration Manager által felügyelt (a hello aktuális ág) hibrid konfigurációban.
  * **Windows 10 Mobile munkahelyi vagy személyes eszközök**. Intune vagy egy támogatott harmadik fél mobileszköz felügyeleti rendszer kezeli.
  * **iOS és Android-eszközök**. Intune által felügyelt.

Azok a felhasználók számára, akik egy eszköz-alapú által védett alkalmazások, certification authority házirend hello alkalmazás férni egy eszközről, amely megfelel a házirend követelményeinek. Hozzáférés megtagadva, ha a kísérlet történt egy eszközön, amely nem felel meg a házirend követelményeinek.

További információ a hogyan tooconfigure eszköz alapú, az Azure AD-hitelesítésszolgáltató irányelvét: [eszközalapú feltételes hozzáférési házirend, az Azure Active Directory-kompatibilis alkalmazásokat](active-directory-conditional-access-policy-connected-applications.md).

## <a name="resources"></a>Erőforrások
Tekintse meg a következő erőforrás kategóriák és a cikkek toolearn a szervezet feltételes hozzáférés beállításáról további hello.

### <a name="multi-factor-authentication-and-location-policies"></a>Többtényezős hitelesítés és a hely házirendek
* [Ismerkedés a feltételes hozzáférés tooAzure AD-kompatibilis alkalmazások csoport, a hely és a többtényezős hitelesítési házirendek alapján](active-directory-conditional-access-azuread-connected-apps.md)
* [Alkalmazások és a támogatott böngészők](active-directory-conditional-access-supported-apps.md)

### <a name="device-based-conditional-access"></a>Eszközalapú feltételes hozzáférés
* [Állítsa be a hozzáférési eszközalapú feltételes hozzáférési házirend vezérlő tooAzure Active Directory-kompatibilis alkalmazásokat](active-directory-conditional-access-policy-connected-applications.md)
* [Állítsa be az automatikus regisztráció, a Windows Azure Active Directory tartományhoz csatlakozó eszközök](active-directory-conditional-access-automatic-device-registration-setup.md)
* [Azure Active Directory-hozzáférési problémák elhárítása](active-directory-conditional-access-device-remediation.md)
* [Elveszett vagy ellopott eszközökön lévő adatok védelme a Microsoft Intune segítségével](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune)

### <a name="protect-resources-based-on-sign-in-risk"></a>A bejelentkezési kockázat alapján erőforrások védelme
* [Az Azure AD identity protection](active-directory-identityprotection.md)

### <a name="next-steps"></a>Következő lépések
* [Feltételes hozzáférés – gyakori kérdések](active-directory-conditional-faqs.md)
* [Technikai útmutató](active-directory-conditional-access-technical-reference.md)

