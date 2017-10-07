---
title: "az Active Directory hibrid identitás aaaAzure kialakítási szempontok - adja meg, hibrid identitás felügyeleti feladatai |} Microsoft Docs"
description: "Feltételes hozzáférés-vezérlést Azure Active Directory ellenőrzi hello megadott feltételek hello felhasználói hitelesítés során, és mielőtt engedélyezi a hozzáférést toohello alkalmazás kiválasztása. Ha ezek a feltételek teljesülnek, hello felhasználó hitelesítése és hozzáférési toohello alkalmazás engedélyezve."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 65f80aea-0426-4072-83e1-faf5b76df034
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: d3c0e9b23f43127b3d8e0b3a4e8f03d4bc148c27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="plan-for-hybrid-identity-lifecycle"></a>Hibrid identitás életciklusának tervezése
Identitás az egyik a vállalati mobilitás és alkalmazás-hozzáférési stratégia hello alapjainak. Tooyour mobileszköz vagy SaaS-alkalmazás jelentkezik, hogy a személyazonosságát hello kulcs toogaining hozzáférés tooeverything. A legmagasabb szintjén az identitáskezelési megoldás magában foglalja a egységesíti és az identitás tárházak találhatók, ide tartozik az automatizálás és központosítása hello folyamata erőforrások kiépítése közötti szinkronizálása. hello identitáskezelési megoldás legyen egy központosított identitás a helyszíni és felhőalapú és valamilyen identitás-összevonási toomaintain központosított hitelesítés is használható és biztonságosan megosztani, és a külső felhasználók és a vállalatok közötti együttműködés. Erőforrások operációs rendszerek és alkalmazások toopeople a között, vagy (DEP) részt, a szervezetek. Szervezeti struktúra lehet módosítani tooaccommodate hello szabályzatok és eljárásokat.

Szintén fontos toohave egy identitás-megoldás célja tooempower a felhasználók számukra az önkiszolgáló észlel tookeep őket hatékonyan dolgozhatnak. Az identitás-megoldás robusztusabb, ha egyszeri bejelentkezéshez a felhasználók számára lehetővé teszi egyes rendszergazdák minden eléréséhez szükséges összes hello erőforrások szintek felhasználói hitelesítő adatok kezelésére szolgáló szabványosított eljárásokat használhatja. Bizonyos szintű felügyeleti csökkenthetők vagy szüntetni, attól függően, hogy a felügyeleti megoldás kiépítés hello hello szélessége. Továbbá biztonságos terjesztheti felügyeleti képességek, manuálisan vagy automatikusan, különböző szervezetek között. Például a tartományi rendszergazda ki tud szolgálni csak hello személyek és erőforrásaihoz. Ez a felhasználó rendszergazdai és üzembe helyezési feladatokat is végrehajthat rá azonban nem jogosult toodo konfigurációs feladatok, például a munkafolyamatok létrehozása.

## <a name="determine-hybrid-identity-management-tasks"></a>Hibrid identitás felügyeleti feladatok meghatározása
A szervezet felügyeleti feladatok fokozza a hello pontosság és a felügyelet hatékonysága és javítja a szervezet hello munkaterhelés hello egyenleg. Az alábbiakban egy robusztus identity management rendszer meghatározó hello pivots.

 ![](./media/hybrid-id-design-considerations/Identity_management_considerations.png)

toodefine hibrid identitás feladatokat, ismernie kell a hello olyan szervezet, amely hibrid identitás bevezetése fog néhány alapvető jellemzőit. Fontos toounderstand hello aktuális adattárak identitás adatforrások is használatban. És ismerje meg, azok központi elemei, hello eligazodást követelményekkel rendelkezik, és annak alapján, hogy szüksége lesz a tooask részletesebb kérdésekre, amely vezet, tooa jobb tervezési döntés a megoldást.  

Ezek a követelmények meghatározásánál tart, miközben biztosítása, amely legalább hello a következő kérdések és válaszok

* Üzembe helyezési lehetőségek: 
  
  * Támogatja-e hello hibrid identitáskezelési megoldás egy robusztus fiók kezelési és üzembe helyezési rendszert?
  * Hogyan történik a felhasználók, csoportok és jelszavak felügyelt toobe is?
  * Az rugalmas hello identitás életciklusának kezelésére? 
    * Mennyi időt jelszó frissítések fiók felfüggesztését igénybe?
* Licenckezelés: 
  
  * Hibrid identitáskezelési megoldás leírók licenckezelés nem hello?
    * Ha igen, milyen lehetőségek érhetők el?
* Nem hello megoldás leíró Csoportalapú Licenckezelése? 
  
      - Ha igen, az azt lehetséges tooassign egy biztonsági csoport tooit? 
       - Ha igen, hello címtárak automatikusan hozzárendeli licencek hello csoport tagjai tooall hello? 
        - Mi történik, ha a felhasználók ezt követően hozzá, vagy hello csoportból eltávolított, lesz egy licencet kell automatikusan kiosztani vagy megvonni szükség szerint? 
* Integráció más külső Identitásszolgáltatók:
* A hibrid megoldás integrálható külső identity providers tooimplement egyszeri bejelentkezéshez?
* Ennyi lehetséges toounify az összes különböző identitás-szolgáltatóktól hello javul identitás rendszerben?
* Ha igen, hogyan és azokat, és milyen lehetőségek érhetők el?

## <a name="synchronization-management"></a>Szinkronizálás kezelése
Egyik hello célja, identitáskezelő toobe képes toobring összes hello identitás-szolgáltatóktól, és láthatóan tartja őket szinkronizálva. Hello adatok szinkronizálásához mindig a fő identitásszolgáltató mérvadó alapján. Hibrid identitáskezelési forgatókönyvben a szinkronizált felügyeleti modellt identitáskezelést minden felhasználó és eszköz egy helyszíni kiszolgálón, és hello fiókokat, és ha szükséges, jelszavak toohello felhő szinkronizálásához. hello felhasználói azonos jelszót a helyi ő hello felhő, és a bejelentkezés, hello jelszó nem ellenőrizte-e hello identitáskezelési megoldás hello kerül. Ez a modell a címtár-Szinkronizáló eszköz használja.

![](./media/hybrid-id-design-considerations/Directory_synchronization.png)tooproper tervezési hello szinkronizálása a hibrid identitáskezelési megoldás győződjön meg arról, hogy a következő kérdések hello válaszok: • Mik azok a hello szinkronizálási megoldások hello hibrid identitáskezelési megoldás érhető el?
Mik azok a hello egyszeri bejelentkezési képességek elérhető •?
• Mik hello lehetőségei identitás-összevonási B2B és B2C között?

## <a name="next-steps"></a>Következő lépések
[Hibrid identitás kezelés bevezetési stratégiájának meghatározása](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)

## <a name="see-also"></a>Lásd még:
[Kialakítási szempontok áttekintése](active-directory-hybrid-identity-design-considerations-overview.md)

