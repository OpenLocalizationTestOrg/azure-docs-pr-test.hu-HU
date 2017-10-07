---
title: "aaaUsers meg van jelölve, a kockázat biztonsági jelentés hello Azure Active Directory portálon |} Microsoft Docs"
description: "További tudnivalók: hello felhasználók megjelölt kockázat biztonsági jelentés hello Azure Active Directory portálon"
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
ms.openlocfilehash: 5077cd61d6119745a85ed712623904633a151331
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="users-flagged-for-risk-security-report-in-hello-azure-active-directory-portal"></a>Felhasználók meg van jelölve, a kockázat biztonsági jelentés hello Azure Active Directory portálon

Hello biztonsági jelentésekkel hello Azure Active Directory (Azure AD) akkor is kaphat a hello valószínűségértékének inverzét feltört felhasználói fiókok a környezetben. 

Az Azure Active Directory kapcsolódó tooyour felhasználói fiókok gyanús műveleteket észleli. A rendszer minden egyes észlelt tevékenység esetében létrehoz egy *kockázati esemény* nevű rekordot. További információkért tekintse át [Az Azure Active Directory kockázati eseményeivel](active-directory-identity-protection-risk-events.md) foglalkozó cikket. 

hello észlelt kockázati események használt toocalculate:

- **Kockázatos bejelentkezések** -kockázatos bejelentkezés egy bejelentkezési kísérlet, amely előfordulhat, hogy nincs egy felhasználói fiókot hello jogos tulajdonosa, aki elvégezte mutatója. További információkért lásd a [kockázatos bejelentkezésekkel](active-directory-identityprotection.md#risky-sign-ins) foglalkozó részt. 

- **Kockázatosként megjelölt felhasználók** – A kockázatos felhasználó egy olyan felhasználói fiókot jelöl, amelynek elképzelhető, hogy sérült a biztonsága. További információkért lásd a [kockázatosként megjelölt felhasználókkal](active-directory-identityprotection.md#users-flagged-for-risk) foglalkozó részt.  

Hello Azure-portálon, az hello biztonsági jelentések hello megtalálhatja **Azure Active Directory** hello paneljén **biztonsági** szakasz.  

![Kockázatos bejelentkezések](./media/active-directory-reporting-security-user-at-risk/10.png)



## <a name="what-azure-ad-license-do-you-need-tooaccess-a-security-report"></a>Milyen az Azure AD licencet kell tooaccess biztonsági jelentést?  

Az Azure Active Directory minden kiadása biztosítja a kockázatosként megjelölt felhasználók jelentéseit.  
Azonban a jelentések részletességét hello szintű platformjától függően eltérő hello verziója esetén: 

- A hello **Azure Active Directory szabad és alapszintű kiadásai**, elérhetővé már meg van jelölve a kockázat felhasználók listáját. 

- Hello **Azure Active Directory Premium 1** edition kiterjeszti a modell is így már tooexamine néhány hello az alapul szolgáló kockázati eseményekről, amelyek az egyes jelentések vonatkozóan nem észlelhetők. 

- Hello **Azure Active Directory Premium 2** kiadás tartalmazza a hello minden alapul szolgáló kockázati események részletesebb információt, és lehetővé teszi a tooconfigure biztonsági házirendek, amelyek automatikusan reagálnak tooconfigured kockázata szintek.



## <a name="azure-active-directory-free-and-basic-edition"></a>Azure Active Directory – ingyenes és alapszintű kiadások

meg van jelölve a kockázat jelentés hello Azure Active Directory ingyenes és alapszintű kiadásai hello felhasználók tesz lehetővé a felhasználói fiókokat, előfordulhat, hogy sérült listáját. 


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-user-at-risk/03.png)

Felhasználó megadásával hello kapcsolódó felhasználói adatok paneljének megnyitása.
A felhasználók számára, az érintettek áttekintheti a hello felhasználó bejelentkezési előzményei és hello jelszó alaphelyzetbe állítása, ha szükséges.

![Kockázatos bejelentkezések](./media/active-directory-reporting-security-user-at-risk/46.png)


Ez a párbeszédablak a következő lehetőségeket kínálja:

- Töltse le a hello jelentés

- Felhasználók keresése

![Kockázatos bejelentkezések](./media/active-directory-reporting-security-user-at-risk/16.png)


## <a name="azure-active-directory-premium-editions"></a>Azure Active Directory – prémium szintű kiadások

meg van jelölve a kockázat jelentés hello Azure Active Directory prémium kiadásaiban hello felhasználók biztosítja:

- Egy [lista azokról a felhasználói fiókokról](active-directory-identityprotection.md#users-flagged-for-risk), amelyeknek elképzelhető, hogy sérült a biztonsága. 

- Hello kapcsolatos információk összesítése [eseménytípusok kockázati](active-directory-identity-protection-risk-events.md) észlelt

- Egy beállítás toodownload hello jelentés

- Egy beállítás tooconfigure egy [felhasználói kockázat szervizelési házirend](active-directory-identityprotection.md#user-risk-security-policy)  


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-user-at-risk/71.png)

Egy felhasználó kiválasztásakor megkapja a felhasználó részletes jelentési nézetét, amely a következőket teszi lehetővé:

- Nyissa meg a hello összes bejelentkezések megtekintése

- Hello felhasználói jelszó alaphelyzetbe állítása

- Az összes esemény elvetését

- Vizsgálja meg a hello felhasználóhoz jelentett kockázati eseményekről. 


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-user-at-risk/324.png)


a kockázat esemény tooinvestigate válasszon egyet a hello lista tooopen hello **részletek** panel kockázat eseményhez. A hello **részletek** panelen hello beállítás tooeither rendelkezik [kézzel zárja be a kockázati események](active-directory-identityprotection.md#closing-risk-events-manually) vagy egy manuális lezárt kockázat esemény újraaktiválásához. 


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-user-at-risk/325.png)



## <a name="next-steps"></a>Következő lépések

- Az Azure Active Directory Identity Protectionnel kapcsolatos további részletekért lásd: [Azure Active Directory Identity Protection](active-directory-identityprotection.md).

