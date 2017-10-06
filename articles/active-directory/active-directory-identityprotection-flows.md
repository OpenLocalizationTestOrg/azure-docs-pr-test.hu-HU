---
title: "aaaSign az Azure AD Identity Protection lép |} Microsoft Docs"
description: "Hello felhasználói élmény áttekintést nyújt, ha Identity Protection problémák elhárításáról vagy javítja a felhasználó, vagy ha egy házirend többtényezős hitelesítés szükséges."
services: active-directory
keywords: "az Azure active directory azonosító adatok védelmét, a cloud app discovery, alkalmazások, biztonság, kockázat, kockázati szint, biztonsági rés, biztonsági házirend kezelése"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: de5bf637-75a7-4104-b6d8-03686372a319
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: fbdca5b86ed93d0a2f2b6df1dd0150da9c0c85c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a>Az Azure AD Identity Protection bejelentkezési élmény
Az Azure Active Directory azonosító adatok védelmét a következőket teheti:

* felhasználók tooregister a többtényezős hitelesítés megkövetelése
* kockázatos bejelentkezéseket és a sérült biztonságú felhasználók kezelése

hello válasz hello rendszer toothese problémák hatással van a felhasználói bejelentkezés során tapasztal élmény mivel most közvetlen aláírás-a felhasználónév és jelszó megadásával nem lesznek lehetséges többé. További lépések végrehajtására szükség a felhasználók biztonságosan vissza az üzleti tooget.

Ez a témakör áttekintést nyújt a felhasználói bejelentkezési felhasználói felület minden olyan esetben, amik akkor léphetnek fel.

**Többtényezős hitelesítés**

* A multi-factor authentication regisztráció

**Bejelentkezés veszélyben**

* Kockázatos bejelentkezési helyreállítási
* Kockázatos bejelentkezés letiltva
* A multi-factor authentication regisztráció egy kockázatos bejelentkezés során

**Felhasználói veszélyben**

* Sérült biztonságú fiók helyreállítási
* Sérült biztonságú fiók blokkolva van

## <a name="multi-factor-authentication-registration"></a>A multi-factor authentication regisztráció
hello mind a lehető legjobb felhasználói élmény, sérült biztonságú fiók helyreállítási folyamata hello hello kockázatos bejelentkezési folyamata, amikor hello felhasználói önálló helyre tudja állítani. Ha a felhasználók a multi-factor authentication van regisztrálva, már rendelkeznek, amelyek lehetnek használt toopass jelentette biztonsági kihívásokkal a fiókjához társított telefonszám. Nincs súgó ügyfélszolgálat vagy a rendszergazdai beavatkozás szükséges toorecover a fiók biztonsága sérülésétől. Ebből kifolyólag magas javasoljuk tooget a felhasználók a többtényezős hitelesítés regisztrált. 

A rendszergazdák a következőket teheti:

* azok a fiókok létrehozása a felhasználók tooset további biztonsági ellenőrzést igénylő házirend beállítása. 
* engedélyezi a mentést too30 nap, a multi-factor authentication regisztrálása kihagyása abban az esetben, ha szeretnék toogive a türelmi időszak, mielőtt regisztrálná.

**a multi-factor authentication regisztráció hello rendelkezik három lépést:**

1. Hello első lépésként hello felhasználói hello követelmény tooset hello fiók az a multi-factor authentication szolgáltatáshoz értesítést kap. 
   
    ![Szervizelés](./media/active-directory-identityprotection-flows/140.png "szervizelés")
2. toolet hello rendszer tooset multi-factor authentication fel kell tudni kapcsolódni toobe módját.
   
    ![Szervizelés](./media/active-directory-identityprotection-flows/141.png "szervizelés")
3. hello rendszer elküld egy ellenőrző tooyou és toorespond van szüksége.
   
    ![Szervizelés](./media/active-directory-identityprotection-flows/142.png "szervizelés")

## <a name="risky-sign-in-recovery"></a>Kockázatos bejelentkezési helyreállítási
Amikor egy rendszergazda úgy konfigurálta a kockázatok bejelentkezési házirend, ha mégis megpróbálják toosign a hello érintett felhasználók értesítést kap. 

**hello kockázatos bejelentkezési folyamata két lépésből áll:** 

1. hello felhasználói tájékoztatják, hogy valami szokatlan rendszer észlelte a bejelentkezéssel kapcsolatban, például egy új helyről, eszközről vagy alkalmazásból jelentkezik be. 
   
    ![Szervizelés](./media/active-directory-identityprotection-flows/120.png "szervizelés")
2. hello felhasználó van szükség tooprove az identitásukat egy biztonsági kérdéssel megoldása. Ha hello a felhasználó a multi-factor authentication regisztrálva van szükségük tooround-út egy biztonsági kódot tootheir telefonszámot. Mivel ez csak egy kockázatos bejelentkezési és a sérült biztonságú fiók, hello felhasználó nem rendelkezik toochange hello jelszó a folyamatot. 
   
    ![Szervizelés](./media/active-directory-identityprotection-flows/121.png "szervizelés")

## <a name="risky-sign-in-blocked"></a>Kockázatos bejelentkezés letiltva
A rendszergazdák is kiválaszthatják tooset egy bejelentkezési kockázat házirend tooblock felhasználók után bejelentkezhet attól függően, hogy hello kockázati szintjét. tooget feloldva, a végfelhasználók kapcsolatba kell lépnie egy rendszergazda vagy a segélyszolgálat segítségét, vagy próbálnak jelentkezik be egy ismert helyre vagy egy eszközt. A multi-factor authentication megoldása által önálló helyreállítása lehetőség nem érhető el ebben az esetben.

![Szervizelés](./media/active-directory-identityprotection-flows/200.png "szervizelés")

## <a name="compromised-account-recovery"></a>Sérült biztonságú fiók helyreállítási
Amikor egy felhasználó kockázat biztonsági házirend konfigurálva van, a felhasználók, akik megfelelnek a hello felhasználói kockázati hello házirendben megadott szint (és ezért feltételezik sérült) hello felhasználói sérült biztonság esetén a helyreállítási folyamat keresztül kell haladnia, mielőtt azok is bejelentkezhet. 

**hello felhasználói sérült biztonság esetén a helyreállítási folyamat három lépést rendelkezik:**

1. hello felhasználói tájékoztatják, hogy a fiók biztonsági kockázatnak miatt gyanús tevékenységet vagy hitelesítő adatok szivárgását.
   
    ![Szervizelés](./media/active-directory-identityprotection-flows/101.png "szervizelés")
2. hello felhasználó van szükség tooprove az identitásukat egy biztonsági kérdéssel megoldása. Ha hello felhasználó regisztrálva van a multi-factor authentication azok önállóan helyreállítani feltörésének. Egy biztonsági kódot tootheir telefonszámot tooround-út kell rendelkeznie. 
   
   ![Szervizelés](./media/active-directory-identityprotection-flows/110.png "szervizelés")
3. Végezetül, a felhasználó hello is a jelszavukat kényszerített toochange mivel valaki más korábban hozzáférési tootheir fiók. 
   A felhasználói felület képernyőfelvételek alatt van.
   
   ![Szervizelés](./media/active-directory-identityprotection-flows/111.png "szervizelés")

## <a name="compromised-account-blocked"></a>Sérült biztonságú fiók blokkolva van
tooget a felhasználó által feloldva felhasználói kockázat biztonsági házirend letiltott, hello felhasználónak kapcsolatba kell lépnie egy rendszergazda vagy a help ügyfélszolgálati. A multi-factor authentication megoldása által önálló helyreállítása lehetőség nem érhető el ebben az esetben.

![Szervizelés](./media/active-directory-identityprotection-flows/104.png "szervizelés")

## <a name="reset-password"></a>Új jelszó létrehozása
Sérült biztonságú felhasználók nincs hozzáférése a bejelentkezés, ha a rendszergazda egy ideiglenes jelszót hozhat létre a számukra. hello felhasználónak lesz toochange a jelszavát a következő bejelentkezés során.

![Szervizelés](./media/active-directory-identityprotection-flows/160.png "szervizelés")

## <a name="see-also"></a>Lásd még:
* [Az Azure Active Directory azonosító adatok védelmét](active-directory-identityprotection.md) 

