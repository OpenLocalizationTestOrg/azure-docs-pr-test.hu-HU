---
title: "Az Azure Active Directoryban a bejelentkezési oldal testreszabása |} Microsoft Docs"
description: "Útmutató: a vállalati arculat megjelenítése a Azure bejelentkezési oldal"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 27590c018ea55e9793246c7a4cab10f934ea502b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-company-branding-to-your-sign-in-page-in-the-azure-active-directory"></a>Vállalati arculat megjelenítése a bejelentkezési oldal az Azure Active Directoryban
A félreértések elkerülése végett számos vállalat igyekszik egységes megjelenést adni az általa kezelt összes webhelynek és szolgáltatásnak. Az Azure Active Directory ezt a képességet biztosít azáltal, hogy a vállalat emblémájának elhelyezésével és egyéni színsémák alkalmazásával a bejelentkezési oldal testreszabása. A bejelentkezési oldal az Office 365 vagy más webes alkalmazásokat az identitás-szolgáltatóként az Azure AD-t használó bejelentkezéskor megjelenő lap. Ön a szolgáltatóosztályokkal ezen a lapon adja meg a hitelesítő adatokat.

Ha meg kívánja jeleníteni vállalatának arculatát, színeit és egyéb testreszabható elemeit ezen az oldalon, az alábbi ábrákon megfigyelheti a különbséget a két megközelítés között.

Az alábbi képernyőfelvételen szereplő példán az Office 365 bejelentkezési oldala látható egy asztali számítógépen, a testreszabást **megelőzően**:

![Az Office 365 bejelentkezési oldala a testreszabást megelőzően](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-before-customization.png)

Az alábbi képernyőfelvételen szereplő példán az Office 365 bejelentkezési oldala látható egy asztali számítógépen, a testreszabást **követően**:

![Az Office 365 bejelentkezési oldala a testreszabást követően](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-after-customization.png)

## <a name="customizing-the-sign-in-page"></a>A bejelentkezési oldal testreszabása
A felhasználók jellemzően akkor használják a bejelentkezési oldalt, ha böngészőalapú hozzáférésre van szükségük a szervezetük által előfizetett felhőalkalmazásaikhoz és szolgáltatásaikhoz.

A bejelentkezési oldalon alkalmazott módosítások megjelenése akár egy órát is igénybe vehet.

A vállalati arculattal ellátott bejelentkezési oldal csak akkor jelenik meg, ha bérlőspecifikus URL-címmel (például https://outlook.com/**contoso**.com vagy https://mail.**contoso**.com) rendelkező szolgáltatást keres fel.

Nem bérlőspecifikus URL-címmel (például https://mail.office365.com) ellátott szolgáltatás felkeresésekor nem vállalati arculattal rendelkező bejelentkezési oldal jelenik meg. Ebben az esetben a vállalati arculat a felhasználói azonosító megadása és a felhasználói csempe kiválasztása után jelenik meg.

> [!NOTE]
> * A tartománynév szerepelnie kell "Aktív" a **tartományok** része az Azure portál, amely a vállalati arculatot konfigurálta. További információkért lásd: [egyéni tartományneveket adhat hozzá](active-directory-domains-add-azure-portal.md).
> * A bejelentkezési oldal vállalati arculata a Microsoft ügyfél-bejelentkezési oldalán nem jelenik meg. Ha Microsoft-fiókkal jelentkezik be, láthatja az Azure AD által nyújtott felhasználói csempék vállalati arculattal ellátott listáját, de a szervezet branding nem vonatkozik a Microsoft-fiókja bejelentkezési oldalára.
>
>

A bejelentkezési oldalon a **Bejelentkezve szeretnék maradni** jelölőnégyzet lehetővé teszi a felhasználó számára, hogy a böngészője bezárása és újbóli megnyitása után is bejelentkezve maradjon.

   ![Bejelentkezve szeretnék maradni](./media/active-directory-branding-custom-signon-azure-portal/01.png)

Ez nincs hatással a munkamenet élettartamára. Az Azure Active Directory bejelentkezési oldalán elrejtheti a jelölőnégyzetet.
A jelölőnégyzet megjelenítését a beállításától függ **bejelentkezve szeretnék maradni tiltva**.

   ![Bejelentkezve szeretnék maradni](./media/active-directory-branding-custom-signon-azure-portal/02.png)

A jelölőnégyzet elrejtéséhez e beállítás konfigurálásával **Igen**.

> [!NOTE]
> A SharePoint Online és az Office 2010 egyes funkciói attól függenek, hogy a felhasználók be tudják-e jelölni ezt a jelölőnégyzetet. Ha ezt a beállítást rejtett értékre konfigurálja, a felhasználóinak további és váratlan bejelentkezési felszólítások jelenhetnek meg.
>
>

**Vállalti arculatot szeretne adni a könyvtárhoz:**

1. Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.
2. Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** a szövegmezőbe, majd válassza ki azt a **Enter**.

   ![Nyitó felhasználók kezelése](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)
3. Az a **felhasználók és csoportok** panelen válassza **vállalati arculat**.
4. Az a **felhasználók és csoportok – a vállalati arculat** panelen válassza a **szerkesztése** parancsot.

    ![Egyéni branding szerkesztése](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)
5. Módosítsa az elemeket, amelyeket testre szeretne szabni. Ezen elemek egyike sem kötelező.
6. Kattintson a **Save** (Mentés) gombra.

A bejelentkezési oldal megjelenik márkajelzési végrehajtott módosítások egy órát is igénybe vehet.

## <a name="next-steps"></a>Következő lépések
[Nyelvspecifikus vállalati arculat megjelenítése](active-directory-branding-localize-azure-portal.md)
