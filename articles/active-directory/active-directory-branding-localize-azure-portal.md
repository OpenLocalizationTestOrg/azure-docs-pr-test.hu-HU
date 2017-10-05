---
title: "Nyelvspecifikus vállalati arculat megjelenítése a bejelentkezési oldal az Azure Active Directory |} Microsoft Docs"
description: "Útmutató: a nyelvi adott vállalati arculat képek és szövegek egy Azure bejelentkezési oldalra"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a0310d6a-aaa7-4ea0-991d-6d3135b4382a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: e1fe8d855386ceec39edbc985538cdf32d78a13b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-language-specific-company-branding-to-your-sign-in-page-in-the-azure-active-directory"></a>Nyelvspecifikus vállalati arculat megjelenítése a bejelentkezési oldal az Azure Active Directoryban
A félreértések elkerülése végett számos vállalat igyekszik egységes megjelenést adni az általa kezelt összes webhelynek és szolgáltatásnak. Az Azure Active Directory ezt a képességet biztosít azáltal, hogy a vállalat emblémájának elhelyezésével és egyéni színsémák alkalmazásával a bejelentkezési oldal testreszabása. A bejelentkezési oldal az Office 365 vagy más webes alkalmazásokat az identitás-szolgáltatóként az Azure AD-t használó bejelentkezéskor megjelenő lap. Ön a szolgáltatóosztályokkal ezen a lapon adja meg a hitelesítő adatokat.

## <a name="customizing-the-sign-in-page-for-another-language"></a>A bejelentkezési oldal más nyelven történő testreszabása
Hozzáadhat nyelvspecifikus elemek az egyéni bejelentkezési oldal csak akkor, ha már létrehozott egy egyéni bejelentkezési oldala a [vállalati arculat megjelenítése a bejelentkezési oldal](active-directory-branding-custom-signon-azure-portal.md). Egy bejelentkezési oldal Címtáranként egy testreszabható elemek egy alapértelmezett készletét konfigurálható. Miután konfigurálta a elemei alapértelmezett készletét, konfigurálhatja a különböző területi beállításokhoz további verziókat. A különböző elemek szabadon kombinálhatók. Például hogy a következőket teheti:

* Hozzon létre egy alapértelmezett **bejelentkezési lap képe** , minden országban használható, majd hozzon létre különböző verziókat angol és francia. Ha úgy állítja be a böngészőbe egy két nyelv valamelyikére, a nyelvspecifikus kép jelenik meg, míg minden más nyelv az alapértelmezett ábra jelenik meg.
* A vállalat eltérő verziójú (például japán vagy héber) emblémáit is beállíthatja.

Azt javasoljuk, hogy ne túl sok nyelvi változat alacsony karbantartási és teljesítmény okokból.

**Vállalti arculatot szeretne adni a könyvtárhoz:**

1. Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.
2. Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** a szövegmezőbe, majd válassza ki azt a **Enter**.

   ![Nyitó felhasználók kezelése](./media/active-directory-branding-localize-azure-portal/user-management.png)
3. Az a **felhasználók és csoportok** panelen válassza **vállalati arculat**.
4. Az a **felhasználók és csoportok – a vállalati arculat** panelen válassza a **nyelv hozzáadása** parancsot.

    ![Nyelvspecifikus márkajelzési elemek hozzáadása](./media/active-directory-branding-localize-azure-portal/add-language.png)
5. Módosítsa az elemeket, amelyeket testre szeretne szabni. Ezen elemek egyike sem kötelező.
6. Kattintson a **Save** (Mentés) gombra.

A bejelentkezési oldal megjelenik márkajelzési végrehajtott módosítások egy órát is igénybe vehet.

## <a name="next-steps"></a>Következő lépések
[Vállalati arculat megjelenítése a bejelentkezési oldal](active-directory-branding-custom-signon-azure-portal.md)
