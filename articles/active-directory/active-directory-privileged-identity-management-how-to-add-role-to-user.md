---
title: "aaaHow tooadd vagy egy felhasználói szerepkör eltávolítása |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd szerepkörök tooprivileged identitásokat hello Azure Active Directory Privileged Identity Management alkalmazás."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 6a47ced8-cf34-4ce8-bea2-e4fc548cfe22
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim;oldportal;it-pro;
ms.openlocfilehash: f84639757dd76061ea12ed6ea7ec9e62ad942109
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-privileged-identity-management-how-tooadd-or-remove-a-user-role"></a>Az Azure AD Privileged Identity Management: Hogyan tooadd vagy egy felhasználói szerepkör eltávolítása
Az Azure Active Directory (AD), egy globális rendszergazda (vagy vállalati rendszergazda) frissítheti felhasználók amelyek **véglegesen** tooroles hozzárendelése az Azure ad-ben. Ez történik, például a PowerShell-parancsmagokkal `Add-MsolRoleMember` és `Remove-MsolRoleMember`. Vagy a klasszikus Azure portálon hello az használhatják a [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory](active-directory-assign-admin-roles.md).

hello Azure AD Privileged Identity Management alkalmazás lehetővé teszi a kiemelt szerepkörű toomake állandó szerepkör-hozzárendelések, is. Emellett a kiemelt szerepkörű rendszergazda felhasználók tehet **jogosult** szabása rendszergazdai szerepkörökhöz. Jogosult rendszergazda is hello szerepkör aktiválásához, akkor szükséges, és majd rájuk vonatkozó engedélyek lejárnak, ha kész van.

## <a name="manage-roles-with-pim-in-hello-azure-portal"></a>A PIM hello Azure-portálon a szerepkörök kezelése
A szervezet felhasználók toodifferent rendszergazdai szerepköröket rendelhet az Azure AD, az Office 365 és más Microsoft-szolgáltatásokat és alkalmazásokat.  További részleteket a hello elérhető szerepkörök található [szerepkörök az Azure AD PIM](active-directory-privileged-identity-management-roles.md).

tooadd vagy távolítsa el a felhasználó a Privileged Identity Management használatával szerepkör hello PIM irányítópult elindítani. Vagy hello majd **rendszergazdai szerepkörök a felhasználók** gombra, vagy válasszon egy adott szerepkör (például a globális rendszergazda) hello szerepkörök táblából.

> [!NOTE]
> Ha a PIM még nem engedélyezte a hello Azure-portálon, nyissa meg túl[Ismerkedés az Azure AD PIM](active-directory-privileged-identity-management-getting-started.md) részleteiről.

Ha azt szeretné, toogive magát, amelyhez PIM hello felhasználói toohave részelemcímkék ismertetését további hello szerepköröket egy másik felhasználó hozzáférési tooPIM [hogyan férhetnek hozzá a toogive tooPIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="add-a-user-tooa-role"></a>Egy felhasználó tooa szerepkör hozzáadása
1. A hello [Azure-portálon](https://portal.azure.com/), jelölje be hello **Azure AD Privileged Identity Management** hello irányítópult csempét.
2. Válassza ki **kiemelt szerepköröket kezelése**.
3. A hello **szerepkör összegzés** tábla, toomanage kívánt válassza hello szerepkör.
4. Hello szerepkör panelen válassza ki a **Hozzáadás**.
5. Kattintson a **válasszon ki egy felhasználót** és keresse meg a hello hello felhasználót **válasszon ki egy felhasználót** panelen.  
6. Válassza ki a hello felhasználói hello keresési eredmények listájáról, és kattintson a **végzett**.
7. Kattintson a **OK** toosave a kijelölés. kijelölt hello felhasználói hello szerepkör jogosultnak hello lista megjelenik.

> [!NOTE]
> Új felhasználók szerepkör hello szerepkör alapértelmezés szerint csak jogosultak. Toomake hello szerepkör állandó, kattintson a hello felhasználói hello listában. hello felhasználói adatok egy új panelen megjelennek. Válassza ki **ellenőrizze perm** hello felhasználói információ menüben.  
> Ha a felhasználó nem tudja regisztrálni Azure multi-factor Authentication (MFA), vagy egy Microsoft-fiókot használ (általában @outlook.com), toomake kell azokat állandó a szerepkört. Jogosult rendszergazdák felkérést tooregister az MFA szolgáltatásra az aktiválás során.

Most, hogy hello felhasználó nem jogosult a szerepkörhöz, tájékoztathatja a felhasználókat, hogy akkor is aktiválhatja toohello utasításait szerint [hogyan tooactivate vagy deaktiválása](active-directory-privileged-identity-management-how-to-activate-role.md).

## <a name="remove-a-user-from-a-role"></a>Felhasználó eltávolítása egy szerepkör
Távolítsa el a felhasználók a megfelelő szerepkör-hozzárendelések, de győződjön meg arról, hogy mindig van legalább egy felhasználót, aki egy állandó globális rendszergazdai.

Kövesse ezeket a lépéseket tooremove egy adott felhasználói szerepkör:

1. Keresse meg a hello szerepkör listában toohello szerepkör egy szerepkör kiválasztásával hello Azure AD PIM irányítópulton vagy hello kattintva **rendszergazdai szerepkörök a felhasználók** gombra.
2. Kattintson a Felhasználólista hello hello felhasználói.
3. Kattintson a **eltávolítása**. Egy üzenet ekkor megkérdezi, hogy tooconfirm.
4. Kattintson a **Igen** tooremove hello hello felhasználói szerepkörnek.

Ha nem biztos abban, hogy mely felhasználók továbbra is szükséges a szerepkör-hozzárendeléseket, akkor is [hello szerepkör egy hozzáférés-ellenőrzés indítása](active-directory-privileged-identity-management-how-to-start-security-review.md).

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

