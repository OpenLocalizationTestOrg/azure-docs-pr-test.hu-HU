---
title: "a Bejelentkezés lapon található hello Azure Active Directory aaaCustomize |} Microsoft Docs"
description: "Megtudhatja, hogyan tooadd egy vállalati arculat Azure bejelentkezés toohello lap"
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
ms.openlocfilehash: 151521e3b9cbc6a438a589735058fbff78443cf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-company-branding-tooyour-sign-in-page-in-hello-azure-active-directory"></a>Vállalati arculat megjelenítése a tooyour hello Azure Active Directory bejelentkezési lap
tooavoid zavart, számos vállalat szeretné, hogy egy egységes megjelenést és működést tooapply összes hello webhelyek és szolgáltatások kezelnek. Az Azure Active Directory toocustomize hello megjelenését a vállalat emblémájának elhelyezésével és egyéni színsémák alkalmazásával a hello bejelentkezési oldal lehetővé ezt a képességet nyújt. hello bejelentkezési oldal hello oldal jelenik meg a bejelentkezéskor tooOffice 365 vagy más webes alkalmazásokat, az identitás-szolgáltatóként az Azure AD-t használó. Ön a szolgáltatóosztályokkal a lap tooenter a hitelesítő adatait.

Ha azt szeretné, hogy tooshow vállalatának arculatát, színeit és egyéb testreszabható elemeit ezen a lapon lásd a következő lemezképek toounderstand hello különbségének hello két megközelítés hello.

következő képernyőfelvételen látható és példán hello Office 365 bejelentkezési oldala egy asztali számítógépen hello **előtt** testreszabás:

![Az Office 365 bejelentkezési oldala a testreszabást megelőzően](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-before-customization.png)

következő képernyőfelvételen látható és példán hello Office 365 bejelentkezési oldala egy asztali számítógépen hello **után** testreszabás:

![Az Office 365 bejelentkezési oldala a testreszabást követően](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-after-customization.png)

## <a name="customizing-hello-sign-in-page"></a>Hello bejelentkezési oldal testreszabása
Általában ha böngészőalapú hozzáférésre tooyour felhőalapú alkalmazások és szolgáltatások, amelyek a szervezet által előfizetett, van szüksége, használja hello bejelentkezési oldalára.

Módosítások tooyour bejelentkezési oldal alkalmazta, hello módosítások tooappear tooan órát is eltarthat.

A vállalati arculattal ellátott bejelentkezési oldal csak akkor jelenik meg, ha bérlőspecifikus URL-címmel (például https://outlook.com/**contoso**.com vagy https://mail.**contoso**.com) rendelkező szolgáltatást keres fel.

Nem bérlőspecifikus URL-címmel (például https://mail.office365.com) ellátott szolgáltatás felkeresésekor nem vállalati arculattal rendelkező bejelentkezési oldal jelenik meg. Ebben az esetben a vállalati arculat a felhasználói azonosító megadása és a felhasználói csempe kiválasztása után jelenik meg.

> [!NOTE]
> * A tartománynév szerepelnie kell "Aktív" hello **tartományok** része hello Azure portál, amely a vállalati arculatot konfigurálta. További információkért lásd: [egyéni tartományneveket adhat hozzá](active-directory-domains-add-azure-portal.md).
> * Bejelentkezési oldal vállalati arculata toohello ügyfél-bejelentkezési Microsoft oldalán nem jelenik. Ha Microsoft-fiókkal jelentkezik be, láthatja az Azure AD által nyújtott felhasználói csempék vállalati arculattal ellátott listáját, de hello branding a szervezet nem érvényes toohello Microsoft-fiókja bejelentkezési oldalára.
>
>

A bejelentkezési oldalon hello **bejelentkezve szeretnék maradni** jelölőnégyzet lehetővé teszi, hogy egy felhasználó tooremain bejelentkezett zárja be, és nyissa meg újra a böngészőt.

   ![Bejelentkezve szeretnék maradni](./media/active-directory-branding-custom-signon-azure-portal/01.png)

Ez nincs hatással a munkamenet élettartamára. Hello Azure Active Directory bejelentkezési oldal hello jelölőnégyzet elrejthetők.
Hello beállítása hello jelölőnégyzet megjelenítését függ **bejelentkezve szeretnék maradni tiltva**.

   ![Bejelentkezve szeretnék maradni](./media/active-directory-branding-custom-signon-azure-portal/02.png)

toohide hello jelölőnégyzet, konfigurálja ezt a beállítást túl**Igen**.

> [!NOTE]
> Néhány funkciójának Office 2010 és SharePoint Online attól függnek, hogy a felhasználók képesek toocheck ebben a mezőben. Ha ez a beállítás toohidden konfigurál, előfordulhat, hogy jelennek meg a felhasználók további és váratlan toosign a megjelenő utasításokat.
>
>

**tooadd vállalati arculat tooyour könyvtár:**

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.
2. Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** hello szövegmezőbe, és válassza ki **Enter**.

   ![Nyitó felhasználók kezelése](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)
3. A hello **felhasználók és csoportok** panelen válassza **vállalati arculat**.
4. A hello **felhasználók és csoportok – a vállalati arculat** panelen, jelölje be hello **szerkesztése** parancsot.

    ![Egyéni branding szerkesztése](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)
5. Módosítsa a kívánt toocustomize hello elemek. Ezen elemek egyike sem kötelező.
6. Kattintson a **Save** (Mentés) gombra.

A módosítások toohello bejelentkezési lapon márkajelzési tooappear is eltarthat tooan óra.

## <a name="next-steps"></a>Következő lépések
[Nyelvspecifikus vállalati arculat megjelenítése](active-directory-branding-localize-azure-portal.md)
