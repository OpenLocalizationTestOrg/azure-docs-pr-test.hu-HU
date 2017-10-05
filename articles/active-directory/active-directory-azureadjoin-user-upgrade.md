---
title: "A beállítások Windows 10-es eszköz az Azure AD beállítása |} Microsoft Docs"
description: "Ismerteti, hogy a felhasználók hogyan csatlakozhatnak az Azure AD használatával a beállítások menü."
services: active-directory
documentationcenter: 
author: femila
manager: swadhwa
editor: 
tags: azure-classic-portal
ms.assetid: b844e1d9-ad43-4e4a-a398-5c4a43bf2f5c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 5a3963f16b471ad1ca8681b22a1a027935400465
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a>A beállítások Windows 10-es eszköz az Azure AD beállítása
Ha már használja a Windows 7 vagy Windows 8, és a számítógép vagy eszköz frissítve lett a Windows 10-re, csatlakozhat az Azure Active Directory (Azure AD) révén a beállítások menüben.

## <a name="to-join-to-azure-ad-from-the-settings-menu"></a>Csatlakozás az Azure AD a beállítások menüből
1. Az a **Start** menüben kattintson a **beállítások** gombra.
2. A **beállítások**, jelölje be **rendszer**->**kapcsolatos**->**az Azure AD Join**.
   
   <center>
   ![A beállítások menüből az Azure AD JOIN](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png)</center>
3. Kattintson a **Folytatás** az Azure AD Join üzenetablakban.
   
   <center>
   ![Az Azure AD JOIN üzenetablakban](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png)</center>
4. Adja meg a bejelentkezési hitelesítő adatokat. A bejelentkezés során tapasztal élmény tartalmazza azokat a lépéseket, amelyek szükségesek a teljes hitelesítés. Ha egy összevont bérlői részét képezik, a rendszergazda biztosít az Ön szervezete által működtetett összevonási élményt nyújt.
   <center>
   ![Adja meg a bejelentkezési hitelesítő adatok](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</center>
5. Ha a szervezet az Azure AD-csatlakozás Azure multi-factor Authentication hitelesítés van beállítva, adja meg a második tényező a folytatás előtt.
6. Kattintson a **elfogadás** a a **felügyelhető eszköz** képernyő.
7. Megjelenik az üzenet "az eszköz most már csatlakozott a szervezet Azure AD-ben".

## <a name="additional-information"></a>További információ
* [További információk az Azure AD Join használati forgatókönyveiről](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Tartományhoz csatlakoztatott eszközök csatlakoztatása az Azure AD-hez Windows 10-es környezetben](active-directory-azureadjoin-devices-group-policy.md)
* [Az Azure AD Join beállítása](active-directory-azureadjoin-setup.md)
* [Microsoft Passporton keresztül jelszó nélkül identitások hitelesítése](active-directory-azureadjoin-passport.md)

