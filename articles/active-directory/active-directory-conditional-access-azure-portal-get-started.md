---
title: "a feltételes hozzáférés az Azure Active Directory használatába aaaGet |} Microsoft Docs"
description: "A helyre vonatkozó feltétellel feltételes hozzáférés tesztelése."
services: active-directory
keywords: "feltételes hozzáférés tooapps, feltételes hozzáférés az Azure AD-vel biztonságos hozzáférés toocompany erőforrásokat, a feltételes hozzáférési házirendek"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 4521f5a34f5882e026f5e58a7127d8c55cba2f0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-conditional-access-in-azure-active-directory"></a>Ismerkedés a feltételes hozzáférés az Azure Active Directoryban

Feltételes hozzáférés az Azure Active Directoryban, amely lehetővé teszi, hogy Ön toodefine feltételek alapján, amely engedéllyel rendelkező felhasználók férhetnek hozzá az alkalmazások képesek. 

Ez a témakör nyújt útmutatást a hely feltétel a környezetben a feltételes hozzáférés tesztelése.  


## <a name="scenario-description"></a>Forgatókönyv leírása

Számos szervezetben egy közös vonatkozó követelmény akkor tooonly többtényezős hitelesítés megkövetelése, hogy nem történik a vállalati intranethez hello hozzáférés tooapps. Az Azure Active Directoryval könnyen megvalósításához a cél helyalapú feltételes hozzáférési házirend konfigurálása. Ez a témakör részletes útmutatást kapcsolódó házirend beállítása. házirend használja hello [megbízható IP-címek](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips) hozzáférés a vállalati hello kísérletek közötti toodistinguish tartozó intranetes és minden egyéb helyek.


## <a name="prerequisites"></a>Előfeltételek

hello ebben a témakörben ismertetett forgatókönyv feltételezi, hogy Ön ismeri a leírt hello fogalmak [Azure Active Directory feltételes hozzáférés](active-directory-conditional-access-azure-portal.md).

tootest ebben az esetben szüksége:

- Tesztfelhasználó létrehozása 

- Rendelje hozzá az Azure AD Premium licenc toohello tesztfelhasználó számára

- A kezelt alkalmazások konfigurálása és hozzárendelése a teszt felhasználó tooit

- Megbízható IP-címek konfigurálása

Ha további információt a megbízható IP-cím van szüksége, tekintse meg [megbízható IP-címek](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).


## <a name="policy-configuration-steps"></a>Házirend konfigurációs lépések

**tooconfigure a feltételes hozzáférési házirend tegye:**

1. Hello hello bal oldali navigációs sávja, az Azure-portálon kattintson **Azure Active Directory**. 

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/01.png)

2. A hello **Azure Active Directory** paneljén, hello **kezelése** kattintson **feltételes hozzáférési**.

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/02.png)
 
3. A hello **feltételes hozzáférés** panelen, tooopen hello **új** panelen hello legfelül hello eszköztáron kattintson **Hozzáadás**.

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/03.png)

4. A hello **új** paneljén, hello **neve** szövegmező, írja be a házirend nevét.

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/04.png)

5. A hello **hozzárendelés** kattintson **felhasználók és csoportok**.

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/05.png)

6. A hello **felhasználók és csoportok** panelen, hajtsa végre az alábbi lépésekkel hello:

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/06.png)

    a. Kattintson a **felhasználók és csoportok kiválasztása**.

    b. Kattintson a **Kiválasztás** gombra.

    c. A hello **válasszon** panelen, jelölje be a tesztfelhasználó számára, és kattintson **válasszon**.

    d. A hello **felhasználók és csoportok** panelen kattintson a **végzett**.

7. A hello **új** panelen, tooopen hello **felhőalapú alkalmazásokba** paneljén, hello **hozzárendelés** területén kattintson **felhőalapú alkalmazásokba**.

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/07.png)

8. A hello **felhőalapú alkalmazásokba** panelen, hajtsa végre az alábbi lépésekkel hello:

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/08.png)

    a. Kattintson a **alkalmazásokról**.

    b. Kattintson a **Kiválasztás** gombra.

    c. A hello **kiválasztása** panelen válassza ki a cloud app, és kattintson a **kiválasztása**.

    d. A hello **felhőalapú alkalmazásokba** panelen kattintson a **végzett**.

9. A hello **új** panelen, tooopen hello **feltételek** paneljén, hello **hozzárendelés** területén kattintson **feltételek**.

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/09.png)

10. A hello **feltételek** panelen, tooopen hello **helyek** panelen kattintson a **helyek**.

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/10.png)

11. A hello **helyek** panelen, hajtsa végre az alábbi lépésekkel hello:

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/11.png)

    a. A **konfigurálása**, kattintson a **Igen**.

    b. A **Include**, kattintson a **minden hely**.

    c. Kattintson a **kizárása**, és kattintson a **a megbízható IP-címek**.

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/12.png)

    d. Kattintson a **Done** (Kész) gombra.

12. A hello **feltételek** panelen kattintson a **végzett**.

13. A hello **új** panelen, tooopen hello **Grant** paneljén, hello **vezérlők** területén kattintson **Grant**.

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/13.png)

14. A hello **Grant** panelen, hajtsa végre az alábbi lépésekkel hello:

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/14.png)

    a. Válassza ki **többtényezős hitelesítést**.

    b. Kattintson a **Kiválasztás** gombra.

15. A hello **új** panel alatt **házirend engedélyezése**, kattintson a **a**.

    ![Feltételes hozzáférés](./media/active-directory-conditional-access-azure-portal-get-started/15.png)

16. A hello **új** panelen kattintson a **létrehozása**.


## <a name="testing-hello-policy"></a>Hello házirend tesztelése

tootest a házirendet, hozzá kell férnie az alkalmazást az eszközről, amely: 

1. IP-címet tartalmazza, amely megfelel a konfigurált megbízható IP-címek 

1. IP-címet tartalmazza, amely kívül esik a konfigurált megbízható IP-címek

A multi-factor authentication csak kell, amely egy eszközről, amely kívül esik a konfigurált megbízható IP-címek történt kapcsolódási kísérlet során. 


## <a name="next-steps"></a>Következő lépések

Ha szeretne további információ a feltételes hozzáférési toolearn, [Azure Active Directory feltételes hozzáférés](active-directory-conditional-access-azure-portal.md).

