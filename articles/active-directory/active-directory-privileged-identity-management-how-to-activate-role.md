---
title: "aaaHow tooactivate vagy deaktiválása |} Microsoft Docs"
description: "Ismerje meg, hogyan tooactivate szerepkörök az emelt szintű identitások hello Azure Privileged Identity Management alkalmazással."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 1ce9e2e7-452b-4f66-9588-0d9cd2539e45
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/14/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 8c68ad69e4b006263bbb8a1cfc7ed3dba974e1db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooactivate-or-deactivate-roles-in-azure-ad-privileged-identity-management"></a>Hogyan tooactivate és szerepkörök az Azure AD Privileged Identity Management inaktiválása
Az Azure Active Directory (AD) Privileged Identity Management egyszerűbbé teszi a hogyan kezelhetik a vállalatok számára az Azure AD privileged access tooresources és más Microsoft online szolgáltatások, például az Office 365-öt vagy a Microsoft Intune.  

Ha Ön történtek egy rendszergazdai szerepkör esetében, amely azt jelenti, hogy ha tooperform privilegizált műveleteket kell, hogy a szerepkör aktiválhatja jogosult. Például ha alkalmanként kezelheti az Office 365-szolgáltatások, a szervezet kiemelt szerepkörű rendszergazda nem tehetik meg állandó globális rendszergazda, mivel ez a szerepkör túl befolyásolja az egyéb szolgáltatások. Helyette akkor Ön jogosult az Azure AD-szerepkörök, például az Exchange Online rendszergazdai. Kérhet tooactivate szerepkörhöz a jogosultságokat igényel, és ezzel meglesz felügyeleti vezérlő egy előre meghatározott időszak.

Ez a cikk az Azure AD Privileged Identity Management (PIM) szerepkörük a rendszergazdák, akik tooactivate számára. Az bemutatja, hogyan hello lépéseket tooactivate szerepkör hello engedélyekre van szükség, és inaktiválása hello szerepkör, amikor elkészült. Emellett a kiemelt szerepkörű rendszergazda megkövetelheti jóváhagyási tooactivate szerepkör (előzetes verzió). További információ [PIM jóváhagyási munkafolyamatok](./privileged-identity-management/azure-ad-pim-approval-workflow.md) itt.

## <a name="add-hello-privileged-identity-management-application"></a>Hello Privileged Identity Management alkalmazás felvétele
Hello Azure AD Privileged Identity Management alkalmazásával hello [Azure-portálon](https://portal.azure.com/) toorequest szerepkör aktiválásának, még akkor is, ha egy másik portál vagy PowerShell toooperate fog. Ha nem hello Azure AD Privileged Identity Management alkalmazás az Azure-portál, hajtsa végre a ezek lépések tooget elindult.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Válassza ki a felhasználónév hello jobb felső sarokban a hello Azure-portálon, és ahol lesz, jelölje be hello directory működik.
3. Válassza ki **további szolgáltatások** és hello szűrő szövegmező toosearch a **Azure AD Privileged Identity Management**.
4. Ellenőrizze **PIN-kód toodashboard** majd **létrehozása**. Megnyílik a Privileged Identity Management alkalmazás hello.

## <a name="activate-a-role"></a>A szerepkör aktiválása
Ha tootake szerepkör van szüksége, aktiválás kérhet hello kiválasztásával **saját szerepkörök** hello Azure AD Privileged Identity Management alkalmazás navigációs beállítás bal oldali navigációs oszlop.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/) és select hello Azure AD Privileged Identity Management képet.
2. Válassza ki **a szerepkörök**. A hozzárendelt jogosult szerepkörök listáját hello oldal hello tetején csoportosítási hello jelennek meg.
3. Válassza ki a szerepkör tooactivate.
4. Válassza ki **aktiválása**. Hello **szerepkör aktiválás kéréséhez** panel jelenik meg.
5. Egyes szerepkörökhöz szükség a multi-factor Authentication (MFA) hello szerepkör aktiválása előtt. Csak akkor kell tooauthenticate munkamenetenként egyszer.
   
    ![Ellenőrizze az MFA Használatát, mielőtt szerepkör aktiválása – képernyőkép][2]
6. Hello aktiválási kérelmet küldjön hello okát hello szövegmezőbe írja be.  Egyes szerepkörökhöz szükség toosupply egy hiba történt a hibajegy száma.
7. Kattintson az **OK** gombra.  Hello szerepkör nem jóváhagyás megkövetelése, ha már aktiválva van, és hello szerepkör aktív szerepkörök (közvetlenül alatt hello lista jogosult szerepkör-hozzárendelések) hello listája jelenik meg. Ha hello [szerepkör jóváhagyást igényel](./privileged-identity-management/azure-ad-pim-approval-workflow.md) tooactivate, egy bejelentési értesítést röviden megjelennek hello jobb felső sarkában a böngésző tájékoztat hello kérelem jóváhagyásra van.

    ![Értesítés - képernyőkép váró kérelem][3]

## <a name="deactivate-a-role"></a>Deaktiválása
Ha a szerepkör aktiválása után azt automatikusan inaktiválja az időkorlátot (jogosult időtartama) elérésekor.

Korai fejezze be a rendszergazdai feladatok elvégzéséhez, ha manuálisan hello Azure AD Privileged Identity Management alkalmazás szerepet is inaktiválhatók.  Válassza ki **saját szerepkörök**, végzett hello szerepkör kiválasztása származó hello **aktív szerepkör-hozzárendelések** csoportosítási, majd válassza **Deactivate**.  

## <a name="cancel-a-pending-request"></a>Megszakítja a függőben lévő kérelem
A hello esemény nem igényel, amelyhez jóváhagyás szükséges szerepkör aktiválásának, bármikor előfordulhat, hogy megszakítja a függőben lévő kérelem. Egyszerűen válassza hello **saját szerepkörök** hello Azure AD Privileged Identity Management alkalmazás navigációs beállítás bal oldali navigációs oszlop.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/) és select hello Azure AD Privileged Identity Management képet.
2. Válassza ki **a szerepkörök**. A hozzárendelt jogosult szerepkörök listáját hello oldal hello tetején csoportosítási hello jelennek meg.
3. Szerepkör kiválasztása.
4. Jelölje be hello **jóváhagyásra váró rendszer aktiválási** fejléc hello szerepkör aktiválása részletei panelen.
5. Válassza ki **Mégse** hello hello tetején **jóváhagyásra váró** panelen.

   ![Megszakítja a függőben lévő kérelem képernyőképe][4]

## <a name="next-steps"></a>Következő lépések
Ha szeretné használni az Azure AD Privileged Identity Management többet, hello következő hivatkozásokat kell további információt.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_activation_MFA.png
[3]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_Request_Pending_Toast2.png
[4]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_Request_Pending_Banner_Cancel.png
