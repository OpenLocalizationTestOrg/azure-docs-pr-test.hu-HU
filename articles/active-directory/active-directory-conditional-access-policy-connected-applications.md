---
title: "Azure Active Directory-eszközalapú feltételes hozzáférési házirendek aaaConfigure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure Azure Active Directory eszközalapú feltételes hozzáférési házirendek."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a27862a6-d513-43ba-97c1-1c0d400bf243
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: cc5923b8ccee4335442c17ef63b2ee6f098e104e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-active-directory-device-based-conditional-access-policies"></a>Azure Active Directory eszközalapú feltételes hozzáférési házirendek konfigurálása

A [Azure Active Directory (Azure AD) feltételes hozzáférés](active-directory-conditional-access-azure-portal.md), úgy finomhangolhatja, hogyan engedéllyel rendelkező felhasználók férhetnek hozzá az erőforrásokat. Például hogy korlátozza hello hozzáférés toocertain erőforrások tootrusted eszközök. Egy feltételes hozzáférési szabályzatot, amely egy megbízható eszközt eszközalapú feltételes hozzáférési szabályzat is nevezik.

Ez a témakör nyújt tájékoztatást, hogyan tooconfigure eszközalapú feltételes hozzáférési házirendek az Azure AD-kompatibilis alkalmazásokat. 


## <a name="before-you-begin"></a>Előkészületek

Eszközalapú feltételes hozzáférési ties **Azure AD feltételes hozzáférésével** és **együtt az Azure AD Eszközkezelés**. Ha még nem ismeri a következő területeken, olvassa el a következő témakörök, először hello:

- **[Feltételes hozzáférés az Azure Active Directoryban](active-directory-conditional-access-azure-portal.md)**  – Ez a témakör ismerteti a feltételes elméleti áttekintését való eléréséhez és kapcsolódó terminológia hello.

- **[Bevezetés toodevice kezelése az Azure Active Directoryban](device-management-introduction.md)**  – Ez a témakör áttekintést nyújt a hello különböző beállítások tooconnect eszközzel rendelkezik az Azure ad-val. 


## <a name="trusted-devices"></a>Megbízható eszközök

Mobileszköz-first, a felhő-első világában Azure Active Directory lehetővé teszi, hogy egyszeri bejelentkezés toodevices, alkalmazásokhoz és szolgáltatásokhoz bárhonnan. Az egyes erőforrásoknak a környezetben, az arra jogosult felhasználók toohello hozzáférés biztosítása nem feltétlenül meg elég helyes. Továbbá toohello arra jogosult felhasználók, is szüksége lehet a megbízható eszköz toobe használt tooaccess erőforrás. A környezetben, megadhatja, mi megbízható eszköz alapuló összetevők következő hello:

- Hello [eszközplatformok](active-directory-conditional-access-azure-portal.md#device-platforms) az eszközön
- Egy eszköz-e megfelelő
- Egy eszköz-e a tartományhoz 

Hello [eszközplatformok](active-directory-conditional-access-azure-portal.md#device-platforms) jellemzőek, az eszközön futó operációs rendszer hello. Az eszközalapú feltételes hozzáférési házirend korlátozhatja a hozzáférést toocertain erőforrások toospecific eszközplatformokat.



Eszközalapú feltételes hozzáférési szabályzatot, a megbízható eszközök toobe megjelölve megfelelőnek lehet szükség.

![Felhőalkalmazások](./media/active-directory-conditional-access-policy-connected-applications/24.png)

Eszközök hello könyvtárban által megfelelőnek jelölhető ki:

- Intune-ban 
- Egy harmadik fél mobileszköz felügyeleti rendszer, amely az Azure ad szolgáltatással  

Csak az eszközök, amelyek csatlakoztatott tooAzure AD megfelelőnek jelölhető. tooconnect egy eszköz tooAzure Active Directory, a következő beállítások hello rendelkezik: 

- Regisztrálva az Azure AD
- Az Azure AD-tartományhoz
- Az Azure AD hibrid csatlakoztatva

    ![Felhőalkalmazások](./media/active-directory-conditional-access-policy-connected-applications/26.png)

Ha egy a helyszíni Active Directory (AD) kezdjen, érdemes lehet eszközök, amelyek nem csatlakoztatott tooAzure AD, de a megbízható AD illesztett tooyour toobe.

![Felhőalkalmazások](./media/active-directory-conditional-access-policy-connected-applications/25.png)


## <a name="next-steps"></a>Következő lépések

Eszközalapú feltételes hozzáférési házirend konfigurálása a környezetben, előtt meg kell vessen egy pillantást hello [ajánlott eljárások a feltételes hozzáférés az Azure Active Directoryban](active-directory-conditional-access-best-practices.md).

