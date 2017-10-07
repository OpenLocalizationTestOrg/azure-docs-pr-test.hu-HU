---
title: "aaaAzure Active Directory-Identity Protection-értesítések |} Microsoft Docs"
description: "Ismerje meg, hogyan támogatják a különböző értesítések a vizsgálati tevékenységet."
services: active-directory
keywords: "az Azure active directory azonosító adatok védelmét, a cloud app discovery, alkalmazások, biztonság, kockázat, kockázati szint, biztonsági rés, biztonsági házirend kezelése"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 65ca79b9-4da1-4d5b-bebd-eda776cc32c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 19e62374873f034591c658a779f97d935766c612
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Az Azure Active Directory Identity Protection-értesítések
Az Azure AD Identity Protection küld a kétféle típusú automatizált értesítési e-mailt küld toohelp kezelheti a felhasználói kockázat és kockázati események:

* Felhasználói értesítés e-mailek biztonsága sérült
* Heti kivonatoló e-mail

## <a name="user-compromised-alert-email"></a>Felhasználói értesítés e-mailek biztonsága sérült
Egy felhasználó sérült biztonságú e-mail riasztást generál, ha az Azure AD Identity Protection egy fiókot, biztonsági sérülés. hello e-mail tartalmaz egy hivatkozást toohello felhasználók meg van jelölve, a kockázat jelentés hello Identity Protection-irányítópulton. Azt javasoljuk, hogy a sérült biztonságú fiókok értesítések azonnal vizsgálata.

## <a name="weekly-digest-email"></a>Heti kivonatoló e-mail
hello heti kivonatoló e-mail új kockázati események összegzését tartalmazza.<br>
Ezek a következők:

* Érintett felhasználók
* A gyanús tevékenységek
* Észlelt biztonsági rések
* Hivatkozások toohello kapcsolódó Identity Protection a jelentések

<br>
![Szervizelés](./media/active-directory-identityprotection-notifications/400.png "szervizelés")
<br>

Válthat heti kivonatoló e-mailek küldésekor.
<br><br>
![Felhasználói kockázatok](./media/active-directory-identityprotection-notifications/62.png "felhasználói kockázatok")
<br>

**tooopen hello kapcsolódó konfigurációs párbeszédpanel**:

1. A hello **Azure AD Identity Protection** panelen kattintson a **beállítások**.
   <br><br>
   ![Felhasználói kockázat házirendnek](./media/active-directory-identityprotection-notifications/401.png "felhasználói kockázat házirendnek")
   <br>
2. A hello **általános** kattintson **értesítések**.
   <br><br>
   ![Felhasználói kockázat házirendnek](./media/active-directory-identityprotection-notifications/405.png "felhasználói kockázat házirendnek")
   <br>

## <a name="see-also"></a>Lásd még:
* [Az Azure Active Directory azonosító adatok védelmét](active-directory-identityprotection.md)
