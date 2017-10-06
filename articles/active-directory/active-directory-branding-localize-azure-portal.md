---
title: "aaaAdd nyelvspecifikus Védjegyadatok tooyour bejelentkezési oldal az Azure Active Directory hello |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd egy nyelvspecifikus vállalati arculat képek és szöveget az Azure-bejelentkezés tooan lap"
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
ms.openlocfilehash: 1e33c31abc242e8455290beb1f03760be7b9ac42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-language-specific-company-branding-tooyour-sign-in-page-in-hello-azure-active-directory"></a>Nyelvspecifikus vállalati arculat megjelenítése a tooyour hello Azure Active Directory bejelentkezési lap
tooavoid zavart, számos vállalat szeretné, hogy egy egységes megjelenést és működést tooapply összes hello webhelyek és szolgáltatások kezelnek. Az Azure Active Directory toocustomize hello megjelenését a vállalat emblémájának elhelyezésével és egyéni színsémák alkalmazásával a hello bejelentkezési oldal lehetővé ezt a képességet nyújt. hello bejelentkezési oldal hello oldal jelenik meg a bejelentkezéskor tooOffice 365 vagy más webes alkalmazásokat, az identitás-szolgáltatóként az Azure AD-t használó. Ön a szolgáltatóosztályokkal a lap tooenter a hitelesítő adatait.

## <a name="customizing-hello-sign-in-page-for-another-language"></a>Más nyelven történő hello bejelentkezési oldal testreszabása
Hozzáadhat nyelvspecifikus elemek tooyour egyéni bejelentkezési oldal csak akkor, ha már létrehozott egy egyéni bejelentkezési oldal leírtak [vállalati arculat megjelenítése a bejelentkezési oldal tooyour](active-directory-branding-custom-signon-azure-portal.md). Egy bejelentkezési oldal Címtáranként egy testreszabható elemek egy alapértelmezett készletét konfigurálható. Miután konfigurálta a elemei hello alapértelmezett készletét, konfigurálhatja a különböző területi beállításokhoz további verziókat. A különböző elemek szabadon kombinálhatók. Például hogy a következőket teheti:

* Hozzon létre egy alapértelmezett **bejelentkezési lap képe** , minden országban használható, majd hozzon létre különböző verziókat angol és francia. Ha úgy állítja be a böngészők tooone két nyelv valamelyikére, hello nyelvspecifikus kép jelenik meg, míg minden más nyelv hello alapértelmezett ábra jelenik meg.
* A vállalat eltérő verziójú (például japán vagy héber) emblémáit is beállíthatja.

Azt javasoljuk, hogy ne hello túl sok nyelvi változat alacsony karbantartási és teljesítmény okokból.

**tooadd vállalati arculat tooyour könyvtár:**

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.
2. Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** hello szövegmezőbe, és válassza ki **Enter**.

   ![Nyitó felhasználók kezelése](./media/active-directory-branding-localize-azure-portal/user-management.png)
3. A hello **felhasználók és csoportok** panelen válassza **vállalati arculat**.
4. A hello **felhasználók és csoportok – a vállalati arculat** panelen, jelölje be hello **nyelv hozzáadása** parancsot.

    ![Nyelvspecifikus márkajelzési elemek hozzáadása](./media/active-directory-branding-localize-azure-portal/add-language.png)
5. Módosítsa a kívánt toocustomize hello elemek. Ezen elemek egyike sem kötelező.
6. Kattintson a **Save** (Mentés) gombra.

A módosítások toohello bejelentkezési lapon márkajelzési tooappear is eltarthat tooan óra.

## <a name="next-steps"></a>Következő lépések
[Vállalati arculat megjelenítése a tooyour bejelentkezési oldal](active-directory-branding-custom-signon-azure-portal.md)
