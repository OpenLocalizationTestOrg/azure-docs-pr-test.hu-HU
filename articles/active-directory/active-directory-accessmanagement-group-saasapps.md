---
title: "egy csoport toomanage hozzáférés tooSaaS alkalmazások aaaUsing |} Microsoft Docs"
description: "Az Azure Active Directory prémium vagy alapszintű tooassign toouse csoportok miként férhetnek hozzá a tooSaaS alkalmazások az Azure Active Directoryval integrált."
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
ms.openlocfilehash: f45ea4472b3d88e8ea514af3db31b4cc9ea68d58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-a-group-toomanage-access-toosaas-applications"></a>Egy csoport toomanage tooSaaS alkalmazásokat használatával
Azure Active Directory (Azure AD) a prémium szintű Azure AD vagy az Azure AD alapvető licenccel rendelkező, csoportok tooassign hozzáférés tooa SaaS-alkalmazás, amely integrálva van az Azure AD használatával. Például ha tooassign hozzáférés az hello marketing részlege toouse öt különböző SaaS-alkalmazásokhoz, akkor is hello marketingrészleg hello felhasználókat tartalmazó csoport létrehozása, és majd rendelje hozzá az adott csoport toothese öt SaaS-alkalmazásokhoz, amelyek hello marketingrészleg igényli. Így időt takaríthat marketing osztályon egy helyen hello hello tagsága kezelésével. Felhasználók majd rendelt toohello alkalmazás hozzáadásuk után hello marketing csoport tagjaként, és a hozzárendeléseik eltávolított hello alkalmazásból marketing csoportnak hello való eltávolításakor.

> [!IMPORTANT]
> A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon. 

Ez a funkció több száz alkalmazásokat, amelyek adhat hozzá az Azure AD Application Gallery hello használható.

**egy csoport tooa SaaS-alkalmazás hozzáférését tooassign**

1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), jelölje be **Active Directory** hello bal oldali navigációs sávján hello.
2. Jelölje be hello **Directory** fülre, és ezután nyissa meg hello directory tooassign hozzáférést egy csoport tooa SaaS-alkalmazáshoz használni kívánt.
3. Jelölje be hello **alkalmazások** fülre. Válasszon ki egy alkalmazást a hello Alkalmazáskatalógusában hozzáadott, és kattintson a hello **felhasználók és csoportok** fülre.
4. A hello **felhasználók és csoportok** lap hello **kezdve** mezőbe írja be a hello neve hello csoport toowhich kívánt tooassign hozzáférést, és ezután válasszon hello hello jobb felső van jelölve. Csak akkor kell tootype hello első részére a csoport nevét.
5. Hello válasszon ki, majd válassza a hello **hozzáférés hozzárendelése** gombra. Válassza ki **Igen** amikor hello megerősítő üzenet megjelenik. A beágyazott csoporttagság nem támogatottak a csoport-alapú hozzárendelés tooapplications most.
6. Mely felhasználók legyenek társítva toohello alkalmazás, vagy közvetlenül a csoport tagsága is megtekinthető. toodo, a módosítás hello **legördülő lista megjelenítése "csoportból** túl**"Minden felhasználó"**. hello listán látható a felhasználók hello könyvtárban és-e minden felhasználóhoz hozzá van rendelve toohello alkalmazás. hello lista is mutatja, hogy hello hozzárendelt felhasználók legyenek társítva közvetlenül toohello alkalmazás (hozzárendelés-típus "Közvetlen" jelenik meg), vagy csoporttagság (hozzárendelés-típus megjelennek az helyeként "Örökölt.") alapján

> [!NOTE]
> Hello felhasználók és csoportok lapon csak a prémium szintű Azure AD vagy az Azure AD alapvető engedélyezése után tekintheti meg.
>
>

### <a name="next-steps"></a>Következő lépések
E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.

* [Hozzáférés tooresources kezelése az Azure Active Directoryval](active-directory-manage-groups.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Azure Active Directory-parancsmagok csoportbeállítások konfigurálásához](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Mi az az Azure Active Directory?](active-directory-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
