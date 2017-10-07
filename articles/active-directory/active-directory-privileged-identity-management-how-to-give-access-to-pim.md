---
title: "aaaHow toogive hozzáférés tooPrivileged Identity Management - Azure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd szerepkörök toousers a hello-e Azure Active Directory Privileged Identity Management bővítmény, PIM kezelésére."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: d4c53b53-2b37-41e6-813c-96ec08a1c897
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 5d99589af4af766e430d7cecd743ace752f63768
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="giving-access-toomanage-azure-ad-privileged-identity-management"></a>Jogosultságot ad az Azure AD Privileged Identity Management hozzáférés toomanage
hello globális rendszergazda Azure AD Privileged Identity Management (PIM) egy olyan szervezet automatikusan lehetővé szerepkör-hozzárendelések hozzáférni és tooPIM. Más írási hozzáféréssel alapértelmezés szerint kap, azonban más globális rendszergazdákat is. Más globális rendszergazdák, biztonsági rendszergazdák és biztonsági olvasók rendelkezik olvasási hozzáféréssel tooAzure AD PIM. toogive hozzáférés tooPIM, hello első felhasználó rendelhet mások toohello **kiemelt szerepkörű rendszergazda** szerepkör. Ehhez a hozzárendeléshez maga PIM belül kell végrehajtani, és a PowerShell vagy más portálokon keresztül nem módosítható.

> [!NOTE]
> Az Azure MFA kezelése az Azure AD PIM van szükség. Microsoft-fiókok az Azure MFA nem regisztrálható, mert a Microsoft-fiókkal jelentkezik be egy felhasználó nem fér hozzá a Azure AD PIM.
> 
> 

Győződjön meg arról, hogy mindig legalább két felhasználók egy kiemelt szerepkörű rendszergazda, abban az esetben, ha egy felhasználó ki van zárva, vagy a fiókot törölték.

## <a name="give-another-user-access-toomanage-pim"></a>Adjon meg egy másik felhasználó hozzáférési toomanage PIM
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/) és select hello **Azure AD Privileged Identity Management** app hello irányítópulton.
2. Válassza ki **kiemelt szerepköröket kezelése** > **kiemelt szerepkörű rendszergazda** > **Hozzáadás**.
   
    ![Adja hozzá a kiemelt szerepkörű rendszergazda – képernyőkép][1]
3. Hello felügyelt felhasználók hozzáadása paneljét, az 1. lépés már befejeződött. Válassza ki a 2 **válasszon ki egy felhasználót** és a keresési hello felhasználói tooadd keresi.
   
    ![Válassza ki a felhasználók – képernyőkép][2]
4. Jelöljön ki hello felhasználói hello keresési eredményeket, majd **végzett**.
5. Kattintson a **OK** toosave a kijelölés. kijelölt hello felhasználói kiemelt szerepkörű rendszergazda hello listája megjelenik.
   
   * Amikor egy új szerepkör toosomeone rendelje hozzá, azok automatikusan be vannak állítva jogosult tooactivate hello szerepkörként. Ha azt szeretné, hogy toomake hello felhasználói hello listában kattintson azokat állandó hello szerepkörben. Válassza ki **perm ellenőrizze** hello felhasználói információ menüben.
6. Hello felhasználói hivatkozás küldése túl[Ismerkedés az Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md).

## <a name="remove-another-users-access-rights-for-managing-pim"></a>Egy másik felhasználói hozzáférési jogosultságokat PIM kezelésére szolgáló eltávolítása
Mielőtt törli valakit hello kiemelt szerepkörű rendszergazda, ügyeljen arra, hogy még mindig lesz két felhasználóira tooit.

1. Hello PIM irányítópultot, kattintson a hello szerepkör **kiemelt szerepkörű rendszergazda**.  jelenleg a szerepet betöltő felhasználók hello listája jelenik meg.
2. Kattintson a Felhasználólista hello hello felhasználói.
3. Kattintson a **eltávolítása**.  Egy megerősítő üzenet jelenik meg.
4. Kattintson a **Igen** tooremove hello felhasználói hello szerepkörből.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Következő lépések
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png
