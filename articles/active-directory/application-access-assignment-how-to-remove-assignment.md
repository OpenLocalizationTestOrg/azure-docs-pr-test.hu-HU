---
title: "aaaHow tooremove a felhasználó által tooan alkalmazás hozzáférésének |} Microsoft Docs"
description: "Megértéséhez hogyan tooremove egy felhasználó tooan alkalmazás elérése"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 17017bddb73aad5a0ef3a411ac91bf0423f0b600
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooremove-a-users-access-tooan-application"></a>Hogyan tooremove egy felhasználó tooan alkalmazás elérése

Ez a cikk segítséget toounderstand hogyan tooremove egy felhasználó tooan alkalmazás eléréséhez.

## <a name="i-want-tooremove-a-specific-users-or-groups-assignment-tooan-application"></a>Egy adott felhasználó vagy csoport hozzárendelése tooan alkalmazás kívánt tooremove

egy felhasználó vagy csoport hozzárendelése tooan alkalmazást, tooremove lépésekkel hello hello felsorolt [egy felhasználó vagy csoport-hozzárendelés eltávolítása egy vállalati alkalmazás Azure Active Directoryban](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) cikk.

. ## Kívánt toodisable összes access tooan alkalmazás minden felhasználó

toodisable minden felhasználói bejelentkezések tooan alkalmazást, kövesse hello lépéseket hello [tiltsa le a felhasználói bejelentkezések Azure Active Directory vállalati alkalmazás](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) cikk.

## <a name="i-want-toodelete-an-application-entirely"></a>Egy alkalmazás toodelete teljesen kívánt

túl**alkalmazás törlése**, kövesse az alábbi hello utasításokat:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

   * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt toodelete hello alkalmazást.

7.  Amikor hello alkalmazás betölt, kattintson a **törlése** hello felső alkalmazás ikonja **áttekintése** panelen.

## <a name="i-want-toodisable-all-future-user-consent-operations-tooany-application"></a>Az összes jövőbeni felhasználói hozzájárulás műveletek tooany alkalmazás kívánt toodisable

Felhasználói hozzájárulás letiltása, az a teljes címtár megakadályozhatja a végfelhasználók számára a küldőnek tooany alkalmazásból. A rendszergazdák továbbra is a felhasználó behalves is hozzájárulás. További információ az alkalmazás toolearn hozzájárulás, és miért lehet, vagy előfordulhat, hogy nem kívánja toodo, olvassa el a következőt [ismertetése felhasználói és rendszergazdai hozzájárulási](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).

túl**tiltsa le a teljes címtár minden jövőbeni felhasználói hozzájárulás műveletei**, kövesse az alábbi hello utasításokat:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **felhasználói beállítások**.

6.  Tiltsa le az összes jövőbeni felhasználói hozzájárulás műveletek hello beállítása az **felhasználók engedélyezhetik alkalmazások tooaccess adataikat** túl váltása**nem** hello kattintson **mentése** gombra.


# <a name="next-steps"></a>Következő lépések
[Hozzáférés tooapps kezelése](active-directory-managing-access-to-apps.md)
