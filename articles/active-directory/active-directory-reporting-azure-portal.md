---
title: aaaAzure Active Directory reporting |} Microsoft Docs
description: "Az Azure Active Directory jelentéskészítés általános áttekintését nyújtja."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 6141a333-38db-478a-927e-526f1e7614f4
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: c91813acbdc4b0bfcd164169b0b575accac227d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting"></a>Jelentéskészítés az Azure Active Directoryban

Az Azure Active Directory jelentéskészítéssel betekintést nyerhet a környezet működésébe.  
a megadott hello adatok teszi lehetővé:

- Meghatározhatja, hogy a felhasználók hogyan használják az alkalmazásokat és szolgáltatásokat
- A környezet állapotának hello érintő kockázatok észlelése
- Elháríthatja azokat a hibákat, amelyek megakadályozzák a felhasználók munkavégzését  

hello jelentéskészítési architektúra két fő oszlopok támaszkodik:

- Biztonsági jelentések
- Tevékenységjelentések

![Jelentéskészítés](./media/active-directory-reporting-azure-portal/01.png)



## <a name="security-reports"></a>Biztonsági jelentések

az Azure Active Directoryban hello biztonsági jelentések segítségével meg tooprotect a szervezet identitásait.  
Az Azure Active Directoryban két típusú biztonsági jelentés létezik:

- **Felhasználók meg van jelölve, a kockázat** – hello a [felhasználók meg van jelölve, a kockázat biztonsági jelentés](active-directory-reporting-security-user-at-risk.md), felhasználói fiókokat, előfordulhat, hogy sérült áttekintést kaphat.

- **Kockázatos bejelentkezések** – hello a [kockázatos bejelentkezési biztonsági jelentés](active-directory-reporting-security-risky-sign-ins.md), a bejelentkezési kísérletek előfordulhat, hogy valaki, aki elvégezte nem hello egy felhasználói fiókot jogos tulajdonosa kap mutató. 

**Milyen az Azure AD licencet kell tooaccess biztonsági jelentést?**  
Az Azure Active Directory minden kiadása biztosítja a kockázatosként megjelölt felhasználók és a kockázatos bejelentkezések jelentéseit.  
Azonban a jelentések részletességét hello szintű platformjától függően eltérő hello verziója esetén: 

- A hello **Azure Active Directory szabad és alapszintű kiadásai**, kockázat és kockázatos bejelentkezések megjelölt felhasználók listájának már beolvasása. 

- Hello **Azure Active Directory Premium 1** edition kiterjeszti a modell is így már tooexamine néhány hello az alapul szolgáló kockázati eseményekről, amelyek az egyes jelentések vonatkozóan nem észlelhetők. 

- Hello **Azure Active Directory Premium 2** kiadás való hello legtöbb részletes információkat az alapul szolgáló kockázati események hello és emellett lehetővé teszi a tooconfigure biztonsági házirendek, amelyek automatikusan reagálnak tooconfigured tartalmazza kockázati szintek.


## <a name="activity-reports"></a>Tevékenységjelentések

Az Azure Active Directoryban két típusú tevékenységjelentés létezik:

- **A naplók** – hello [auditnaplókat tevékenységgel kapcsolatos jelentés](active-directory-reporting-activity-audit-logs.md) biztosít hozzáférést toohello előzményeit minden feladat az Ön bérelt szolgáltatásának.

- **Bejelentkezések** – hello a [bejelentkezések tevékenységgel kapcsolatos jelentés](active-directory-reporting-activity-sign-ins.md), meghatározhatja, akik hello naplózási naplók jelentés által jelentett hello feladatok hajtott végre.



Hello **auditnaplókat jelentés** biztosít megfelelőségi rendszer tevékenységeket rögzíti.
Többek között a hello megadott adatok lehetővé teszi tooaddress gyakori forgatókönyvek például:

- Valaki hozzáfér a bérlők a kapott hozzáférési tooan felügyeleti csoportot. Ki biztosított neki hozzáférést? 

- Szeretném tooknow hello azoknak a felhasználóknak jelentkezik be egy adott alkalmazást szeretnék óta nemrég előkészítve hello app és tooknow szeretné, ha a helyes műveletet

- A kívánt tooknow hány jelszó alaphelyzetbe állítása folyamatban van a bérlő


**Milyen az Azure AD licencet igényelnek tooaccess hello vizsgálati naplók jelentést?**  
hello naplózási naplók jelentés érhető el szolgáltatásokat, amelyhez licenccel rendelkezik. Ha egy adott funkcióhoz licenccel rendelkezik, akkor is napló naplóadatok az access toohello.

További részletekért lásd: **hello ingyenes, a Basic és a Premium kiadásainak általánosan elérhető szolgáltatások összehasonlítása** a [Azure Active Directory szolgáltatásokat és képességeket](https://www.microsoft.com/cloud-platform/azure-active-directory-features).   



Hello **bejelentkezések tevékenységgel kapcsolatos jelentés** lehetővé teszi, hogy tootoofind megválaszolja tooquestions, mint:

- Mi az a hello bejelentkezési minta egy olyan felhasználó?
- Hány felhasználó jelentkezett be egy adott héten?
- Mi az a bejelentkezéseket hello állapotának?


**Milyen az Azure AD licencet elvégzi kell tooaccess hello bejelentkezések tevékenységgel kapcsolatos jelentés?**  
tooaccess hello bejelentkezések tevékenységgel kapcsolatos jelentés, a bérlő rendelkeznie kell egy hozzá társított Azure AD Premium licenchez.


## <a name="programmatic-access"></a>Szoftveres hozzáférés

Továbbá toohello felhasználói felületén, Azure Active Directory reporting is biztosít [programozott hozzáférés](active-directory-reporting-api-getting-started-azure-portal.md) toohello jelentésadatait. Ezek a jelentések adatainak hello lehet nagyon hasznos tooyour alkalmazások, például a SIEM rendszerek, naplózási és üzleti intelligencia eszközök. hello Azure AD jelentéskészítési API-k olyan programozott hozzáférés toohello adatokat REST-alapú API-készlet. Különböző programnyelvekkel és eszközökkel hívhatja ezeket az API-kat. 


## <a name="next-steps"></a>Következő lépések

Ha többet hello tooknow különböző jelentéstípusok az Azure Active Directoryban, lásd:

- [Kockázatosként megjelölt felhasználók jelentés](active-directory-reporting-security-user-at-risk.md)
- [Kockázatos bejelentkezések jelentés](active-directory-reporting-security-risky-sign-ins.md)
- [Naplók jelentés](active-directory-reporting-activity-audit-logs.md)
- [Bejelentkezési naplók jelentés](active-directory-reporting-activity-sign-ins.md)

Ha azt szeretné, hogy a jelentés adatainak hello reporting API használatával hello hello elérésével kapcsolatos további tooknow, lásd: 

- [Ismerkedés az Azure Active Directory reporting API-val hello](active-directory-reporting-api-getting-started-azure-portal.md)


<!--Image references-->
[1]: ./media/active-directory-reporting-azure-portal/ic195031.png