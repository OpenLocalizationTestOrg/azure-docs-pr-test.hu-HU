---
title: "Az Azure AD az önkiszolgáló jelszó-átállítási áttekintése |} Microsoft Docs"
description: "Milyen műveleteket végezhetnek el az önkiszolgáló jelszó-változtatási az Azure AD a szervezet számára?"
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 3903c53b78e61b380bb812a9d3ade694655b5afb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-self-service-password-reset-for-the-it-professional"></a>Az Azure AD az önkiszolgáló jelszó-változtatási az informatikai szakemberek számára

"Önkiszolgáló" egy buzzword történt körül számos számítástechnikai osztály számára a világ különböző jelentését és között. A piacon árasztott termékekkel, amelyek lehetővé teszik a helyi csoportok, a jelszavakat vagy a felhasználói profilok felügyeletéhez a felhőbeli vagy helyszíni.

Az Azure Active Directory (Azure AD) önkiszolgáló jelszó-változtatási (SSPR) beállítása maga egymástól a könnyű használata és telepítése. Az Azure AD az önkiszolgáló jelszó alaphelyzetbe állítása egyesíti a képességek egy készletét, amely:

* Lehetővé teszi a felhasználók a saját jelszavát kezelése
  * Bármilyen eszközről
  * Bármely helyen
  * Bármikor
* Rendszergazdaként megadhatja szabályzatoknak való megfelelőségét karbantartása

Ha készen áll, Ismerkedés az Azure AD SSPR használatával a [gyors üzembe helyezési útmutató](active-directory-passwords-getting-started.md) , és gyorsan a felhasználók a saját jelszavak alaphelyzetbe állítását.

## <a name="what-is-possible"></a>Mi az lehetséges

* **Az önkiszolgáló jelszóváltoztatáshoz** lehetővé teszi, hogy a végfelhasználók vagy a rendszergazdák jelszavukat rendszergazdai segítség nélkül
* **Önkiszolgáló fiókfeloldási** lehetővé teszi, hogy a végfelhasználók számára, hogy a saját rendszergazdai segítség nélkül fiók zárolásának feloldása
* **Az önkiszolgáló jelszó-átállítási** lehetővé teszi, hogy a végfelhasználók vagy a rendszergazdák visszaállíthassák a jelszavukat automatikusan rendszergazdai segítség nélkül. Önkiszolgáló jelszóváltoztatás igényel az Azure AD prémium vagy alapszintű kiadásra - [Azure Active Directory-kiadások](active-directory-editions.md).
* **Rendszergazda által kezdeményezett jelszó alaphelyzetbe állítása** belül a végfelhasználó vagy egy másik rendszergazda jelszó alaphelyzetbe állításához a rendszergazda a [Azure-portálon](https://docs.microsoft.com/azure/azure-portal-overview)
* **Jelszó felügyeleti Tevékenységjelentések** jelszó alaphelyzetbe állítása és nyilvántartási tevékenység rendszergazdák betekintést biztosít a szervezetek - előforduló [felügyeleti jelentések](active-directory-passwords-reporting.md)
* **A Jelszóvisszaírás** lehetővé teszi a helyszíni jelszavak a felhőből kezelését, így összevont összes a portáladatbázis előző forgatókönyvek hajtható végre, vagy az nevében, vagy a jelszó szinkronizált felhasználók. A Jelszóvisszaírás szükséges [Azure AD Premium](active-directory-get-started-premium.md).

## <a name="why-choose-azure-ad-self-service-password-reset"></a>Miért válassza az Azure AD az önkiszolgáló jelszóváltoztatás

* **Csökkentheti a költségeket** segélyszolgálat és támogatja támogatott jelszó alaphelyzetbe állítása általában 20 %-át egy informatikai szervezet töltött
* **Végfelhasználói élmény javítása** és **segélyszolgálat csökkenthető** azzal, hogy a végfelhasználók a saját jelszó problémák megoldásához egyszerre egy támogatási kérést megnyitásakor vagy a segélyszolgálat hívása nélkül power.
* **A meghajtó mobilitási** , a felhasználók alaphelyzetbe állíthatja a jelszavukat a bárhol vannak.

## <a name="azure-ad-self-service-password-reset-availability"></a>Az Azure AD az önkiszolgáló jelszó elérhetőségének alapállapotba állítása

Az Azure AD az önkiszolgáló jelszó alaphelyzetbe állítása a három réteg attól függően, hogy az előfizetés érhető el.

* **Az Azure AD ingyenes** – csak felhőalapú rendszergazdák is új jelszót készíthessenek maguknak
* **Az Azure AD alapszintű** vagy bármely **fizetős Office 365-előfizetéssel** – csak felhőalapú felhasználók és a csak felhőalapú rendszergazdák is az új jelszó
* **Prémium szintű Azure AD** – összevont bármely felhasználó vagy a rendszergazda, csak felhőalapú, beleértve, vagy a szinkronizált felhasználók és az új jelszó is. A helyszíni jelszavak a jelszóvisszaírás engedélyezése szükséges.

## <a name="azure-ad-self-service-password-reset-a-sum-of-the-parts"></a>Az Azure AD az önkiszolgáló jelszó alaphelyzetbe állításához a kijelzők összege

Az önkiszolgáló jelszó-változtatási az Azure AD a következő összetevőkből áll:

* **Felügyeleti konfigurációs jelszóportál** ahol megadhatja a beállítások a jelszavak hogyan kezeli az Ön bérlőjében, az Azure-portálon
* **Jelszó-változtatási regisztrációs portálra** ahol segítségével a felhasználók saját magukat jelszó alaphelyzetbe állítása
* **Jelszó-visszaállítási portál** ahol felhasználók is jelszavuk használatával a rendszergazda által meghatározott kihívás, és a válaszok felhasználók megadott
* **Felhasználói jelszó-módosítási portál** ahol felhasználók módosíthatják a saját jelszavukat a régi jelszavát be- és új jelszó megadása
* **Jelszó jelentések** ahol rendszergazdák megtekintése és elemzése jelszó tevékenység jelent a bérlők számára az Azure-portálon
* **Jelszavak visszaírását a helyszíni Azure AD Connect használatával** lehetővé teszi az eszközök kezelését a helyszíni, összevont, vagy a jelszó szinkronizálása a felhőalapú felhasználók

## <a name="azure-ad-pricing-sla-updates-and-roadmap"></a>Az Azure AD díjszabás, szolgáltatásiszint-szerződés, frissítések és terv

Az alábbi témakörök további információt a következő oldalakon található

* [**Az Azure AD díjszabása**](https://azure.microsoft.com/pricing/details/active-directory/)
* [**Az Office 365 díjszabása**](https://products.office.com/compare-all-microsoft-office-products?tab=2)
* [**Az Azure szolgáltatásiszint-szerződések**](https://azure.microsoft.com/support/legal/sla/)
* [**A Microsoft Online Services szolgáltatásiszint-szerződés**](http://go.microsoft.com/fwlink/?LinkID=272026&clcid=0x409)
* [**Azure-hírt**](https://azure.microsoft.com/updates/)
* [**Azure menetrend**](https://www.microsoft.com/cloud-platform/roadmap-recently-available)

## <a name="next-steps"></a>Következő lépések

Az alábbi hivatkozásokat követve az Azure AD jelszóátállításáról olvashat további információkat.

* [**Gyors üzembe helyezés**](active-directory-passwords-getting-started.md) – Percek alatt üzembe helyezheti az Azure AD önkiszolgáló jelszókezelőjét. 
* [**Licencelés**](active-directory-passwords-licensing.md) – Az Azure AD licencelésének konfigurálása.
* [**Adatok**](active-directory-passwords-data.md) – A szükséges adatok megismerése, és az adatok használata a rendszer jelszókezelésre.
* [**Bevezetés**](active-directory-passwords-best-practices.md) – Az SSPR funkció tervezése és üzembe helyezése a felhasználók számára az itt található útmutató segítségével.
* [**Testreszabás**](active-directory-passwords-customize.md) – Az SSPR-felület megjelenésének és működésének testre szabása a cége számára.
* [**Jelentéskészítés**](active-directory-passwords-reporting.md) – Megtudhatja, mikor és hol érik el a felhasználói az SSPR funkcióit.
* [**Részletes műszaki bemutatás**](active-directory-passwords-how-it-works.md) – Egy pillantás a függöny mögé, hogy megértse, hogyan is működik.
* [**Gyakori kérdések**](active-directory-passwords-faq.md) – Hogyan? Hogy miért? Mi? Hová? Ki? Mikor? – Válaszok olyan kérdésekre, amiket mindig is fel akart tenni
* [**Hibaelhárítás**](active-directory-passwords-troubleshoot.md) – Ismerje meg, hogyan oldhat meg általános, az SSPR működése során jelentkező hibákat.

