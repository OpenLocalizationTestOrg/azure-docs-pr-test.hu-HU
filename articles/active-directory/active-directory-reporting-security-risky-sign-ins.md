---
title: "aaaRisky bejelentkezések jelentés hello Azure Active Directory portálon |} Microsoft Docs"
description: "További tudnivalók: hello kockázatos bejelentkezések jelentés hello Azure Active Directory portálon"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: 7728fcd7-3dd5-4b99-a0e4-949c69788c0f
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: d8df5cafea6b38f3e364c24a6aff599abe088e88
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="risky-sign-ins-report-in-hello-azure-active-directory-portal"></a>Kockázatos bejelentkezések jelentés hello Azure Active Directory portálon

Hello biztonsági jelentések segítségével az Azure Active Directory (Azure AD) akkor is kaphat a hello valószínűségértékének inverzét feltört felhasználói fiókok a környezetben. 

Az Azure AD kapcsolódó tooyour felhasználói fiókok gyanús műveleteket észleli. A rendszer minden egyes észlelt tevékenység esetében létrehoz egy *kockázati esemény* nevű rekordot. További részletek: [Az Azure Active Directory kockázati eseményei](active-directory-identity-protection-risk-events.md). 

hello észlelt kockázati események használt toocalculate:

- **Kockázatos bejelentkezések** -kockázatos bejelentkezés egy bejelentkezési kísérlet, amely előfordulhat, hogy nincs egy felhasználói fiókot hello jogos tulajdonosa, aki elvégezte mutatója. További részletek: [Kockázatos bejelentkezések](active-directory-identityprotection.md#risky-sign-ins). 

- **Kockázatosként megjelölt felhasználók** – A kockázatos felhasználó egy olyan felhasználói fiókot jelöl, amelynek elképzelhető, hogy sérült a biztonsága. További részletek: [Kockázatosként megjelölt felhasználók](active-directory-identityprotection.md#users-flagged-for-risk).  

A [hello Azure-portálon](https://portal.azure.com), hello biztonsági jelentések hello található **Azure Active Directory** hello paneljén **biztonsági** szakasz. 

![Kockázatos bejelentkezések](./media/active-directory-reporting-security-risky-sign-ins/10.png)


## <a name="what-azure-ad-license-do-you-need-tooaccess-a-security-report"></a>Milyen az Azure AD licencet kell tooaccess biztonsági jelentést?  

Az Azure Active Directory minden kiadása biztosítja a kockázatos bejelentkezések jelentéseit.  
Azonban a jelentések részletességét hello szintű platformjától függően eltérő hello verziója esetén: 

- A hello **Azure Active Directory szabad és alapszintű kiadásai**, kockázatos bejelentkezések listájának már beolvasása. 

- Hello **Azure Active Directory Premium 1** edition kiterjeszti a modell is így már tooexamine néhány hello az alapul szolgáló kockázati eseményekről, amelyek az egyes jelentések vonatkozóan nem észlelhetők. 

- Hello **Azure Active Directory Premium 2** kiadás hello a leginkább részletes információk minden alapul szolgáló kockázati eseményekről, és emellett lehetővé teszi a tooconfigure biztonsági házirendek, amelyek automatikusan reagálnak tooconfigured tartalmazza kockázati szintek.



## <a name="azure-active-directory-free-and-basic-edition"></a>Azure Active Directory – ingyenes és alapszintű kiadások

Azure Active Directory ingyenes hello és alapszintű kiadásai biztosít kockázatos bejelentkezések észlelt listáját a felhasználók számára. Ez a jelentés a következőket listázza:

- **Felhasználói** – hello hello bejelentkezési művelet során használt hello felhasználó neve
- **IP** -hello használt tooconnect tooAzure Active Directory hello eszköz IP-címe
- **Hely** -hello helyet használja az Active Directory tooconnect tooAzure
- **Bejelentkezés idejét** -hello idő, amikor hello bejelentkezés történt meg
- **Állapot** -hello hello bejelentkezés állapota


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-risky-sign-ins/01.png)

A vizsgálni hello kockázatos bejelentkezés alapján, megadhatja a visszajelzés tooAzure Active Directory-hello műveletek a következő formában:

- Feloldás
- Megjelölés téves riasztásként
- Kihagyás
- Újraaktiválás

![Kockázatos bejelentkezések](./media/active-directory-reporting-security-risky-sign-ins/21.png)

További részletek: [Kockázati események manuális lezárása](active-directory-identityprotection.md#closing-risk-events-manually).

Ez a jelentés a következő lehetőségeket kínálja:

- Erőforrások keresése
- Hello jelentés adatok letöltése


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-risky-sign-ins/93.png)


## <a name="azure-active-directory-premium-editions"></a>Azure Active Directory – prémium szintű kiadások

hello kockázatos bejelentkezések jelentés hello Azure Active Directory prémium kiadásaiban biztosítja:

- Hello kapcsolatos információk összesítése [eseménytípusok kockázati](active-directory-identity-protection-risk-events.md) észlelt

- Egy beállítás toodownload hello jelentés


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-risky-sign-ins/456.png)


Egy kockázati esemény kiválasztásakor megkapja az esemény részletes jelentési nézetét, amely a következőket teszi lehetővé:

- Egy beállítás tooconfigure egy [felhasználói kockázat szervizelési házirend](active-directory-identityprotection.md#user-risk-security-policy)  

- Tekintse át a hello észlelési ütemtervét hello kockázat esemény  

- Azon felhasználók listájának áttekintését, amelyeknél a rendszer kockázati esemény észlelt

- [A kockázati események manuális bezárását](active-directory-identityprotection.md#closing-risk-events-manually) és a manuálisan bezárt kockázati események újbóli aktiválását. 


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-risky-sign-ins/457.png)

Egy felhasználó kiválasztásakor megkapja a felhasználó részletes jelentési nézetét, amely a következőket teszi lehetővé:

- Nyissa meg a hello összes bejelentkezések megtekintése

- Hello felhasználói jelszó alaphelyzetbe állítása

- Az összes esemény elvetését

- Vizsgálja meg a hello felhasználóhoz jelentett kockázati eseményekről. 


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-risky-sign-ins/324.png)


a kockázat esemény tooinvestigate válasszon egyet a hello listából.  
Ekkor megnyílik a hello **részletek** panel kockázat eseményhez. A hello **részletek** panelen hello beállítás tooeither rendelkezik [kézzel zárja be a kockázati események](active-directory-identityprotection.md#closing-risk-events-manually) vagy egy manuális lezárt kockázat esemény újraaktiválásához. 


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-risky-sign-ins/325.png)





## <a name="next-steps"></a>Következő lépések

- Az Azure Active Directory Identity Protectionnel kapcsolatos további részletekért lásd: [Azure Active Directory Identity Protection](active-directory-identityprotection.md).

