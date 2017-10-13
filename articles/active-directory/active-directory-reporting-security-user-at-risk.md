---
title: "Kockázatosként megjelölt felhasználókról szóló biztonsági jelentés az Azure Active Directory portálon | Microsoft Docs"
description: "Ismerje meg az Azure Active Directory portál kockázatosként megjelölt felhasználókról szóló biztonsági jelentését"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 04f15384a7cd0fa03300acdf159d371569ecf9fc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="users-flagged-for-risk-security-report-in-the-azure-active-directory-portal"></a>Kockázatosként megjelölt felhasználókról szóló biztonsági jelentés az Azure Active Directory portálon

Az Azure Active Directory (Azure AD) biztonsági jelentéseivel megtudhatja, hogy a környezetben mekkora valószínűséggel sérült egyes felhasználói fiókok biztonsága. 

Az Azure Active Directory észleli a felhasználói fiókokhoz kapcsolódó gyanús tevékenységeket. A rendszer minden egyes észlelt tevékenység esetében létrehoz egy *kockázati esemény* nevű rekordot. További információkért tekintse át [Az Azure Active Directory kockázati eseményeivel](active-directory-identity-protection-risk-events.md) foglalkozó cikket. 

A rendszer az észlelt kockázati eseményeket a következők kiszámítására használja:

- **Kockázatos bejelentkezések** – A kockázatos bejelentkezés egy olyan bejelentkezési kísérletet jelöl, amelyet elképzelhető, hogy olyan személy hajtott végre, aki nem a felhasználói fiók jogos tulajdonosa. További információkért lásd a [kockázatos bejelentkezésekkel](active-directory-identityprotection.md#risky-sign-ins) foglalkozó részt. 

- **Kockázatosként megjelölt felhasználók** – A kockázatos felhasználó egy olyan felhasználói fiókot jelöl, amelynek elképzelhető, hogy sérült a biztonsága. További információkért lásd a [kockázatosként megjelölt felhasználókkal](active-directory-identityprotection.md#users-flagged-for-risk) foglalkozó részt.  

Az Azure Portalon a biztonsági jelentések az **Azure Active Directory** panel **Biztonság** szakaszában találhatók.  

![Kockázatos bejelentkezések](./media/active-directory-reporting-security-user-at-risk/10.png)



## <a name="what-azure-ad-license-do-you-need-to-access-a-security-report"></a>Milyen Azure AD-licencre van szükség a biztonsági jelentések eléréséhez?  

Az Azure Active Directory minden kiadása biztosítja a kockázatosként megjelölt felhasználók jelentéseit.  
A jelentések részletességi szintje azonban különbözik a kiadások között: 

- Már az **Azure Active Directory ingyenes és alapszintű kiadásaiban** is szerepel a kockázatosként megjelölt felhasználók listája. 

- Az **Azure Active Directory 1. prémium** kiadása kibővíti ezt a modellt, mert lehetővé teszi azt is, hogy megvizsgáljon néhány, az egyes jelentésekhez észlelt mögöttes kockázatos eseményt. 

- Az **Azure Active Directory 2. prémium** kiadása nyújtja a legrészletesebb információkat minden mögöttes kockázatos eseményről, és lehetővé teszi olyan biztonsági szabályzatok konfigurálását, amelyek automatikusan, a konfigurált kockázati szinteknek megfelelően válaszolnak.



## <a name="azure-active-directory-free-and-basic-edition"></a>Azure Active Directory – ingyenes és alapszintű kiadások

Az Azure Active Directory ingyenes és alapszintű kiadásának kockázatosként megjelölt felhasználókról szóló jelentése azokról a felhasználói fiókokról biztosít listát, amelyeknek elképzelhető, hogy sérült a biztonsága. 


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-user-at-risk/03.png)

Az egyes felhasználók kijelölésével megnyithatja a kapcsolódó felhasználói adatok paneljét.
A veszélyeztetett felhasználók esetében áttekintheti a felhasználó bejelentkezési előzményeit, és szükség esetén alaphelyzetbe állíthatja a jelszót.

![Kockázatos bejelentkezések](./media/active-directory-reporting-security-user-at-risk/46.png)


Ez a párbeszédablak a következő lehetőségeket kínálja:

- A jelentés letöltése

- Felhasználók keresése

![Kockázatos bejelentkezések](./media/active-directory-reporting-security-user-at-risk/16.png)


## <a name="azure-active-directory-premium-editions"></a>Azure Active Directory – prémium szintű kiadások

Az Azure Active Directory prémium kiadásaiban a kockázatosként megjelölt felhasználókról szóló jelentés a következőket tartalmazza:

- Egy [lista azokról a felhasználói fiókokról](active-directory-identityprotection.md#users-flagged-for-risk), amelyeknek elképzelhető, hogy sérült a biztonsága. 

- Összesített adatok az észlelt [kockázati események típusairól](active-directory-identity-protection-risk-events.md)

- Lehetőség a jelentés letöltésére

- Egy [felhasználóikockázat-csökkentési szabályzat](active-directory-identityprotection.md#user-risk-security-policy) konfigurálását  


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-user-at-risk/71.png)

Egy felhasználó kiválasztásakor megkapja a felhasználó részletes jelentési nézetét, amely a következőket teszi lehetővé:

- Az Összes bejelentkezés nézet megnyitását

- A felhasználói jelszó alaphelyzetbe állítását

- Az összes esemény elvetését

- A felhasználóhoz kapcsolódó, jelentett kockázati események vizsgálatát. 


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-user-at-risk/324.png)


Egy kockázati esemény vizsgálatához válassza ki azt a listából a kockázati eseményhez tartozó **Részletek** panel megnyitásához. A **Részletek** panelen [manuálisan bezárhatja a kockázati eseményeket](active-directory-identityprotection.md#closing-risk-events-manually) vagy újra aktiválhatja a manuálisan bezárt kockázati eseményeket. 


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-user-at-risk/325.png)



## <a name="next-steps"></a>Következő lépések

- Az Azure Active Directory Identity Protectionnel kapcsolatos további részletekért lásd: [Azure Active Directory Identity Protection](active-directory-identityprotection.md).

