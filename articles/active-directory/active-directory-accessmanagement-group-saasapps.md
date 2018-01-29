---
title: "SaaS-alkalmazásokhoz való hozzáférés kezelése csoport segítségével |} Microsoft Docs"
description: "Hogyan használható az Azure Active Directory prémium vagy alapszintű kiadásra csoportok hozzáférés hozzárendelése az Azure Active Directoryval integrált SaaS-alkalmazásokhoz."
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: ab8dee63-bedc-46ca-8852-234f5c16ae98
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: it-pro
ms.openlocfilehash: 49a2c86516f0882f341597876d2a44ea3a312ea8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/11/2017
---
# <a name="using-a-group-to-manage-access-to-saas-applications"></a>Hozzáférés kezelése SaaS alkalmazásokhoz egy csoport használatával
Az Azure Active Directoryval (Azure AD) az Azure AD prémium vagy alapszintű Azure AD-licenccel rendelkező, segítségével csoportok hozzáférés hozzárendelése integrálva van az Azure AD SaaS-alkalmazás. Például szeretne hozzárendelni a marketingrészleg használatára öt különböző SaaS-alkalmazásokhoz hozzáférést, ha akkor is hozzon létre egy csoportot, amely tartalmazza a felhasználók a marketingosztályon belül, és hozzárendelheti csoport ezek öt SaaS-alkalmazások által használt a marketingrészleg. Így időt takaríthat meg egy helyen a marketingrészleg tagjainak kezelésével. Felhasználók majd vannak hozzárendelve az alkalmazás hozzáadásuk után a marketing csoport tagjaként, és a hozzárendeléseik eltávolította az alkalmazásból a marketing csoport eltávolításakor. Ez a funkció több száz alkalmazásokat, amelyek adhat hozzá az Azure AD Application Gallery használható.

> [!IMPORTANT]
> Ez a szolgáltatás használható, csak azt követően, az Azure AD Premium kipróbálása vagy Azure AD prémium vagy alapszintű Azure AD licencet vásárolt.
> A beágyazott csoporttagság az alkalmazásokhoz történő csoportalapú hozzárendeléseknél egyelőre nem támogatott.

**Hozzáférés egy felhasználó vagy csoport hozzárendelése egy SaaS-alkalmazáshoz**

1. Az a [az Azure AD felügyeleti központban](https://aad.portal.azure.com), jelölje be **vállalati alkalmazások**.
2. Jelöljön ki egy alkalmazáskészletet, hogy hozzáadta-e az alkalmazás-galériából való megnyitásához.
3. Válassza ki **felhasználók és csoportok**, majd válassza ki **felhasználó hozzáadása**.
4. A **hozzáadása hozzárendelés**, jelölje be **felhasználók és csoportok** megnyitásához a **felhasználók és csoportok** kiválasztási lista.
6. Válassza ki a lehető legtöbb csoportok vagy felhasználók ahányat csak szeretne, majd kattintson vagy koppintson **válasszon** kattintva vegye fel a **hozzáadása hozzárendelés** listája. Ebben a szakaszban is hozzárendelése egy szerepkörhöz felhasználóhoz.
7. Válassza ki **hozzárendelése** a felhasználók és csoportok hozzárendelése a kijelölt vállalati alkalmazás.

### <a name="next-steps"></a>Következő lépések
E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.

* [Erőforráshozzáférés-kezelés Azure Active Directory-csoportokkal](active-directory-manage-groups.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Azure Active Directory-parancsmagok csoportbeállítások konfigurálásához](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Mi az az Azure Active Directory?](active-directory-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
