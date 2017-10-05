---
title: "SaaS-alkalmazásokhoz való hozzáférés kezelése csoport segítségével |} Microsoft Docs"
description: "Hogyan használható az Azure Active Directory prémium vagy alapszintű kiadásra csoportok hozzáférés hozzárendelése az Azure Active Directoryval integrált SaaS-alkalmazásokhoz."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: ab8dee63-bedc-46ca-8852-234f5c16ae98
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: it-pro;oldportal
ms.openlocfilehash: d350011ee9fc5ced9ddb16993f68d3c840a645a5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="using-a-group-to-manage-access-to-saas-applications"></a>Hozzáférés kezelése SaaS alkalmazásokhoz egy csoport használatával
Az Azure Active Directoryval (Azure AD) az Azure AD prémium vagy alapszintű Azure AD-licenccel rendelkező, segítségével csoportok hozzáférés hozzárendelése integrálva van az Azure AD SaaS-alkalmazás. Például szeretne hozzárendelni a marketingrészleg használatára öt különböző SaaS-alkalmazásokhoz hozzáférést, ha akkor is hozzon létre egy csoportot, amely tartalmazza a felhasználók a marketingosztályon belül, és hozzárendelheti csoport ezek öt SaaS-alkalmazások által használt a marketingrészleg. Így időt takaríthat meg egy helyen a marketingrészleg tagjainak kezelésével. Felhasználók majd vannak hozzárendelve az alkalmazás hozzáadásuk után a marketing csoport tagjaként, és a hozzárendeléseik eltávolította az alkalmazásból a marketing csoport eltávolításakor.

> [!IMPORTANT]
> A Microsoft javasolja, hogy az Azure Portalon található [Azure AD felügyeleti központból](https://aad.portal.azure.com) kezelje az Azure AD-t az ebben a cikkben javasolt klasszikus Azure portál helyett. 

Ez a funkció több száz alkalmazásokat, amelyek adhat hozzá az Azure AD Application Gallery használható.

**Egy csoport hozzáférés hozzárendelése egy SaaS-alkalmazáshoz**

1. Az a [a klasszikus Azure portálon](https://manage.windowsazure.com), jelölje be **Active Directory** a bal oldali navigációs sávján látható.
2. Válassza ki a **Directory** lapra, és nyissa meg a könyvtárat, amelyben hozzáférési csoport egy SaaS-alkalmazáshoz hozzárendelni kívánt.
3. Válassza ki a **alkalmazások** fülre. Válasszon ki egy alkalmazást, az alkalmazás-galériából hozzáadott, és kattintson a **felhasználók és csoportok** fülre.
4. Az a **felhasználók és csoportok** lap a **kezdve** mezőben adja meg a kívánt hozzáférés hozzárendelése a csoport nevét, majd válassza ki a pipa jelre a jobb felső részén. Csak Önnek kell beírnia a csoportnév első része.
5. Válassza ki azt a csoportot, majd válassza ki a **hozzáférés hozzárendelése** gombra. Válassza ki **Igen** , amikor megjelenik a jóváhagyást kérő üzenet. A beágyazott csoporttagság az alkalmazásokhoz történő csoportalapú hozzárendeléseknél egyelőre nem támogatott.
6. Mely felhasználók legyenek társítva az alkalmazáshoz, vagy közvetlenül a csoport tagsága is megtekinthető. Ehhez az szükséges, módosítsa a **legördülő lista megjelenítése "csoportból** való **"Minden felhasználó"**. A listán látható a felhasználók a könyvtárban, és-e minden felhasználóhoz hozzá van rendelve az alkalmazáshoz. A lista mutatja azokat is, hogy a hozzárendelt felhasználó kapcsolódik, az alkalmazás közvetlenül (az hozzárendelés-típus "Közvetlen" jelenik meg), vagy csoporttagság (hozzárendelés-típus megjelennek az helyeként "Örökölt.") alapján

> [!NOTE]
> Csak a prémium szintű Azure AD vagy az Azure AD alapvető engedélyezése után a felhasználók és csoportok lapon tekintheti meg.
>
>

### <a name="next-steps"></a>Következő lépések
E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.

* [Erőforráshozzáférés-kezelés Azure Active Directory-csoportokkal](active-directory-manage-groups.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Azure Active Directory-parancsmagok csoportbeállítások konfigurálásához](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Mi az az Azure Active Directory?](active-directory-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
