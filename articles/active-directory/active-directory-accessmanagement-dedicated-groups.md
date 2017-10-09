---
title: az Azure Active Directoryban aaaDedicated csoportok |} Microsoft Docs
description: "Hogyan dedikált csoportok áttekintése az Azure Active Directory és a létrehozott hogyan működnek."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 86158909-083a-41fe-8090-955e96ad1865
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: curtand
ms.openlocfilehash: 8feec6e1a4e6b384470392d3043caeeec2b03dd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="dedicated-groups-in-azure-active-directory"></a>Dedikált csoportok az Azure Active Directoryban
Az Azure Active Directory (Azure AD) a hello dedikált csoportok funkció automatikusan létrehozza, és tölti fel az előre definiált az Azure AD-csoportok tagságát. Dedikált csoportok tagjai nem adható hozzá, vagy eltávolított használatával hello Azure klasszikus portál, a Windows PowerShell-parancsmagok, illetve programozott módon.

> [!NOTE]
> Dedikált csoportok szükséges, hogy a van rendelve egy Azure AD Premium-licenc
>
> * hello felügyelő rendszergazdához; csoporton hello szabály
> * minden felhasználó, aki hello szerint be vannak jelölve szabály toobe hello csoport tagjai
>
>

**tooenable dedikált csoportok**

1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), jelölje be **Active Directory**, majd nyissa meg a munkahely címtárában.
2. Jelölje be hello **csoportok** fülre, és azt szeretné, hogy tooedit majd nyitott hello csoport.
3. Jelölje be hello **konfigurálása** lapot, és utána állítsa be **dedikált csoportok engedélyezése** túl**Igen**.

Dedikált csoportok engedélyezéséhez váltson hello túl beállítása után**Igen**, további engedélyezheti hello directory tooautomatically hello minden felhasználó dedikált csoport létrehozásához hello beállítása **az "Összes felhasználó" csoport** Váltás túl**Igen**. Is szerkesztheti a kijelölt csoport hello nevét írja be a hello **"Minden felhasználó" megjelenített név csoport** mező.

hello minden felhasználói csoport használható tooassign hello azonos tooall hello felhasználók engedélyeinek a címtárban. Például a könyvtár hozzáférési tooa SaaS-alkalmazás minden felhasználó is megadta, ha a hozzáférés az összes felhasználó dedikált csoport toothis alkalmazás hello hozzárendelése.

dedikált hello minden felhasználó csoportban hello könyvtárban, beleértve a vendégek és a külső felhasználók felhasználókat tartalmazza. Ha egy csoportot kell, amely nem tartalmazza a külső felhasználók számára, akkor ez által csoportot hoz létre egy attribútum-alapú dinamikus szabály például hello következő érhető el:

                (user.userPrincipalName -notContains "#EXT#@")

Egy csoport, amely nem tartalmazza az összes vendégek például hello következő szabályt használni:

                (user.userType -ne "Guest")

arról, hogyan toolearn toocreate *speciális* (szabályok többszörös összehasonlítást is tartalmazó) szabályok dinamikus csoporttagság, lásd: [toocreate attribútumok használata speciális szabályok](active-directory-accessmanagement-groups-with-advanced-rules.md).

### <a name="next-steps"></a>Következő lépések
E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.

* [Hozzáférés tooresources kezelése az Azure Active Directoryval](active-directory-manage-groups.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Mi az az Azure Active Directory?](active-directory-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
