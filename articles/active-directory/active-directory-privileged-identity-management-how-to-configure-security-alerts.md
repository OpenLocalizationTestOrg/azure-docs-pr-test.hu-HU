---
title: "aaaHow tooconfigure biztonsági riasztások |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure biztonsági riasztások Azure Privileged Identity Management bővítmény."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 4e0c911a-36c6-42a0-8f79-a01c03d2d04f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 1b3c4a7d36fa3f81bb3fe2574d675fdf0ab34909
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-security-alerts-in-azure-ad-privileged-identity-management"></a>Hogyan Azure AD Privileged Identity Management tooconfigure biztonsági riasztások
## <a name="security-alerts"></a>Biztonsági riasztások
Azure Privileged Identity Management (PIM) riasztásokat állít elő, ha nem biztonságos vagy gyanús tevékenységek a környezetben. Riasztást vált ki, mutat hello PIM irányítópulton. Válassza ki a hello riasztási toosee, hogy listák hello felhasználókkal vagy szerepkörökkel hello riasztást kiváltó jelentést.

![A PIM irányítópult biztonsági riasztások – képernyőkép][1]

| Riasztás | Eseményindító | Ajánlás |
| --- | --- | --- |
| **Szerepkörök hozzárendeli PIM-en kívül** |A rendszergazda véglegesen van hozzárendelve tooa szerepkör kívül hello PIM felületet. |Tekintse át a hello új szerepkör-hozzárendelés. Egyéb szolgáltatások állandó rendszergazdák csak rendelhet hozzá, mivel azt tooan jogosult hozzárendelés szükség esetén módosítsa. |
| **Szerepkörök aktiválása túl gyakran van folyamatban** |Az ugyanarra a szerepkörre hello időn belül engedélyezett hello beállítások hello túl sok újraaktiválás történt. |Lépjen kapcsolatba a hello felhasználói toosee miért ezek aktiválta hello szerepkör annyi alkalommal. Lehet, hogy hello idő érték túl rövid azokat a feladataikat, vagy lehet, hogy ezek használ toocomplete parancsfájlok tooautomatically egy szerepkör aktiválásához. |
| **Szerepkörök az aktiváláshoz nincs szükség a multi-factor authentication** |Hello-beállítások engedélyezve van az MFA nélkül szerepkör is létezik. |Rendszer legtöbb magas szintű jogosultságokkal hello szerepkörök megkövetelő, de erősen ösztönözzék a többtényezős hitelesítés engedélyezése a szerepkörök aktiválásához. |
| **A rendszergazdák a kiemelt szerepköröket a nem használt.** |Nincsenek jogosult rendszergazdák, amely még nem aktiválta a közelmúltban szerepük. |Indítsa el az access felülvizsgálati toodetermine hello felhasználók, amelyek többé nem kell hozzáférést. |
| **Nincsenek a túl sok globális rendszergazda** |Nincsenek további globális rendszergazdák mint ajánlott. |Ha nagyszámú globális rendszergazdák, valószínű, hogy a felhasználók kihozhatják több engedélyt van szükségük. Áthelyezési felhasználók tooless kiemelt szerepköröket, vagy ellenőrizze némelyikük abban az esetben jogosult hello szerepköre tartósan hozzárendelt. |

## <a name="configure-security-alert-settings"></a>Biztonsági riasztási beállításainak konfigurálása
Néhány hello biztonsági riasztások a PIM toowork a környezet és a biztonsági célok szabhatja testre. Kövesse a lépéseket tooreach hello beállítások panel:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/) és select hello **Azure AD Privileged Identity Management** hello irányítópultról csempére.
2. Válassza ki **kiemelt szerepköröket felügyelt** > **beállítások** > **beállításokat riasztások**.
   
    ![Keresse meg a toosecurity riasztások beállításai][2]

### <a name="roles-are-being-activated-too-frequently-alert"></a>"Szerepkörök vannak aktiválásának túl gyakran" riasztás
A riasztás, ha egy felhasználó aktiválja eseményindítók hello ugyanarra a kiemelt szerepkörre meghatározott időn belül többször. Beállíthatja, hogy mindkét hello idő időszak és hello számú aktiválást tegyenek lehetővé.

* **Az aktiválás megújítása időkeretre**: Adja meg a nap, óra, perc, és másodszor hello időszak kívánja toouse tootrack gyanús megújításokat.
* **Aktiválási megújításának száma**: aktiválás, a 2 too100, figyelembe venni a riasztásból úgy döntött, hogy hello időkereten belül worthy hello számát adja meg. Ez a beállítás által áthelyezése hello csúszkát vagy hello szövegmezőbe írjon be egy értéket módosíthatja.

### <a name="there-are-too-many-global-administrators-alert"></a>"There are túl sok globális rendszergazdák" riasztás
PIM váltja ki ezt a figyelmeztetést, ha két különböző feltételek teljesülnek, és mindkettő konfigurálhatja. Először egy bizonyos küszöböt, a globális rendszergazdai tooreach. Az összes szerepkör-hozzárendelések bizonyos százalékát, globális rendszergazdák kell lennie. Ha csak megfelel egy, a mérési adatok, hello riasztás nem jelenik meg.  

* **Globális rendszergazdák minimális száma**: Adja meg a globális rendszergazdákat, 2 too100, akkor fontolja meg, hogy nem biztonságos összeget hello száma.
* **Globális rendszergazdák aránya**: Adja meg a rendszergazdák és globális rendszergazdák, a 0 % hello százaléka too100 %, a környezetben nem biztonságos.

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>"A rendszergazdák nem használják a kiemelt szerepköröket" riasztás
Ez a riasztás akkor váltja ki, ha egy felhasználó egy bizonyos időn kerül a szerepkör aktiválása nélkül.

* **A napok számát**: Adja meg a napok, 0 too100, amely a felhasználói szerepkör aktiválása nélkül folytathatja hello száma.

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_dash.png
[2]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_settings.png
