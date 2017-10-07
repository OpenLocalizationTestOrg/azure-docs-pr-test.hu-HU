---
title: "aaaAzure AD az önkiszolgáló jelszó-átállítási áttekintése |} Microsoft Docs"
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
ms.openlocfilehash: 0efc291b1eeec0b7ae33ff5a7d9ed38e70c8be6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-self-service-password-reset-for-hello-it-professional"></a>Az Azure AD az önkiszolgáló jelszó-változtatási hello informatikai szakemberek számára

Egy buzzword történt körül számos számítástechnikai osztály számára között a különböző jelentését hello world "Önkiszolgáló". hello piaci árasztott termékekkel, amelyek lehetővé teszik a toomanage helyi csoportok, jelszavakat vagy a felhasználói profilok hello felhőalapú vagy helyszíni.

Az Azure Active Directory (Azure AD) önkiszolgáló jelszó-változtatási (SSPR) beállítása maga egymástól a könnyű használata és telepítése. Az Azure AD az önkiszolgáló jelszó alaphelyzetbe állítása egyesíti a képességek egy készletét, amely:

* Lehetővé teszi a felhasználók toomanage a saját jelszavát
  * Bármilyen eszközről
  * Bármely helyen
  * Bármikor
* Rendszergazdaként megadhatja szabályzatoknak való megfelelőségét karbantartása

Ha készen áll, Ismerkedés az Azure AD SSPR használatával a [gyors üzembe helyezési útmutató](active-directory-passwords-getting-started.md) , és gyorsan a felhasználók a saját jelszavak alaphelyzetbe állítását.

## <a name="what-is-possible"></a>Mi az lehetséges

* **Az önkiszolgáló jelszóváltoztatáshoz** lehetővé teszi a végfelhasználók vagy a rendszergazdák toochange a jelszavuk rendszergazdai segítség nélkül
* **Önkiszolgáló fiókfeloldási** lehetővé teszi, hogy a befejező felhasználóknak toounlock saját fiók rendszergazdai segítség nélkül
* **Az önkiszolgáló jelszó-átállítási** lehetővé teszi a végfelhasználók vagy a rendszergazdák tooreset automatikusan rendszergazdai segítség nélkül jelszavukat. Önkiszolgáló jelszóváltoztatás igényel az Azure AD prémium vagy alapszintű kiadásra - [Azure Active Directory-kiadások](active-directory-editions.md).
* **Rendszergazda által kezdeményezett jelszó alaphelyzetbe állítása** lehetővé teszi egy rendszergazda tooreset a végfelhasználó vagy egy másik rendszergazda jelszó programból hello [Azure-portálon](https://docs.microsoft.com/azure/azure-portal-overview)
* **Jelszó felügyeleti Tevékenységjelentések** jelszó alaphelyzetbe állítása és nyilvántartási tevékenység rendszergazdák betekintést biztosít a szervezetek - előforduló [felügyeleti jelentések](active-directory-passwords-reporting.md)
* **A Jelszóvisszaírás** lehetővé teszi a helyszíni jelszavak hello felhőből kezelését, így forgatókönyvek szerint, vagy hello nevében, hajtható végre hello portáladatbázis előző összevont vagy jelszó szinkronizált felhasználók. A Jelszóvisszaírás szükséges [Azure AD Premium](active-directory-get-started-premium.md).

## <a name="why-choose-azure-ad-self-service-password-reset"></a>Miért válassza az Azure AD az önkiszolgáló jelszóváltoztatás

* **Csökkentheti a költségeket** segélyszolgálat és támogatja támogatott jelszó alaphelyzetbe állítása általában 20 %-át egy informatikai szervezet töltött
* **Végfelhasználói élmény javítása** és **segélyszolgálat csökkenthető** adjon befejező felhasználóknak hello power tooresolve saját jelszóval kapcsolatos problémák egyszerre egy támogatási kérést megnyitásakor vagy a segélyszolgálat hívása nélkül.
* **A meghajtó mobilitási** , a felhasználók alaphelyzetbe állíthatja a jelszavukat a bárhol vannak.

## <a name="azure-ad-self-service-password-reset-availability"></a>Az Azure AD az önkiszolgáló jelszó elérhetőségének alapállapotba állítása

Az Azure AD az önkiszolgáló jelszó alaphelyzetbe állítása a három réteg attól függően, hogy az előfizetés érhető el.

* **Az Azure AD ingyenes** – csak felhőalapú rendszergazdák is új jelszót készíthessenek maguknak
* **Az Azure AD alapszintű** vagy bármely **fizetős Office 365-előfizetéssel** – csak felhőalapú felhasználók és a csak felhőalapú rendszergazdák is az új jelszó
* **Prémium szintű Azure AD** – összevont bármely felhasználó vagy a rendszergazda, csak felhőalapú, beleértve, vagy a szinkronizált felhasználók és az új jelszó is. A helyszíni jelszavak jelszó visszaírási toobe engedélyezve van szükség.

## <a name="azure-ad-self-service-password-reset-a-sum-of-hello-parts"></a>Az Azure AD az önkiszolgáló jelszó alaphelyzetbe állításához a hello részek összege

Az önkiszolgáló jelszó-változtatási Azure AD-ben a következő összetevők hello tevődik össze:

* **Felügyeleti konfigurációs jelszóportál** ahol megadhatja a jelszavak hello Azure-portálon keresztül a bérlői kezelésének beállításai
* **Jelszó-változtatási regisztrációs portálra** ahol segítségével a felhasználók saját magukat jelszó alaphelyzetbe állítása
* **Jelszó-visszaállítási portál** ahol felhasználók is jelszavuk használatával hello kihívást hello rendszergazda által meghatározott, és hello válaszok felhasználók megadott
* **Felhasználói jelszó-módosítási portál** ahol felhasználók módosíthatják a saját jelszavukat a régi jelszavát be- és új jelszó megadása
* **Jelszó jelentések** ahol rendszergazdák megtekintése és elemzése jelszó tevékenység jelentései a bérlők hello Azure-portálon
* **Jelszó visszaírási tooon helyszíni Azure AD Connect használatával** lehetővé teszi az Ön tooenable kezelését a helyszíni összevont, vagy a jelszó szinkronizálva hello felhőből felhasználók

## <a name="azure-ad-pricing-sla-updates-and-roadmap"></a>Az Azure AD díjszabás, szolgáltatásiszint-szerződés, frissítések és terv

Ezek a témakörök további információkra található meg a következő lapok hello

* [**Az Azure AD díjszabása**](https://azure.microsoft.com/pricing/details/active-directory/)
* [**Az Office 365 díjszabása**](https://products.office.com/compare-all-microsoft-office-products?tab=2)
* [**Az Azure szolgáltatásiszint-szerződések**](https://azure.microsoft.com/support/legal/sla/)
* [**A Microsoft Online Services szolgáltatásiszint-szerződés**](http://go.microsoft.com/fwlink/?LinkID=272026&clcid=0x409)
* [**Azure-hírt**](https://azure.microsoft.com/updates/)
* [**Azure menetrend**](https://www.microsoft.com/cloud-platform/roadmap-recently-available)

## <a name="next-steps"></a>Következő lépések

a következő hivatkozások hello adja meg a jelszó alaphelyzetbe állítása, az Azure AD használatával kapcsolatos további információk

* [**Gyors üzembe helyezés**](active-directory-passwords-getting-started.md) – Percek alatt üzembe helyezheti az Azure AD önkiszolgáló jelszókezelőjét. 
* [**Licencelés**](active-directory-passwords-licensing.md) – Az Azure AD licencelésének konfigurálása.
* [**Adatok** ](active-directory-passwords-data.md) - szükséges hello adatok megismeréséhez, és hogyan használja fel azokat a jelszókezelés
* [**Bevezetés** ](active-directory-passwords-best-practices.md) -megtervezése és telepítése az önkiszolgáló jelszó-Változtatási tooyour felhasználók hello útmutatást itt talál
* [**Testre szabhatja** ](active-directory-passwords-customize.md) -testreszabása, önkiszolgáló jelszó-Változtatási élményt a vállalata hello hello megjelenését és működését.
* [**Jelentéskészítés**](active-directory-passwords-reporting.md) – Megtudhatja, mikor és hol érik el a felhasználói az SSPR funkcióit.
* [**Műszaki mélyreható** ](active-directory-passwords-how-it-works.md) -mögött hello függöny toounderstand nyissa meg annak működéséről
* [**Gyakori kérdések**](active-directory-passwords-faq.md) – Hogyan? Hogy miért? Mi? Hová? Ki? Mikor? -Válaszok mindig kívánta tooask tooquestions
* [**Hibaelhárítás** ](active-directory-passwords-troubleshoot.md) -megtudhatja, hogyan tooresolve közös állít ki, hogy az önkiszolgáló jelszó-Változtatási látható

