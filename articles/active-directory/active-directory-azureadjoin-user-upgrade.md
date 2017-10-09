---
title: "Windows 10-eszköz az Azure AD a beállítások mentése aaaSet |} Microsoft Docs"
description: "Ismerteti, hogyan felhasználók csatlakozhatnak tooAzure AD keresztül hello-beállítások menüjében."
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
ms.openlocfilehash: d6485228338db571cc01f913c99fbba49e0bb74a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a>A beállítások Windows 10-es eszköz az Azure AD beállítása
Ha már használja a Windows 7 vagy Windows 8 és a számítógép vagy eszköz frissített tooWindows 10, Active Directory (Azure AD) tooAzure hello beállítások menü keresztül is csatlakozhat.

## <a name="toojoin-tooazure-ad-from-hello-settings-menu"></a>toojoin tooAzure AD hello-beállítások menüjében
1. A hello **Start** menü hello kattintson **beállítások** gombra.
2. A **beállítások**, jelölje be **rendszer**->**kapcsolatos**->**az Azure AD Join**.
   
   <center>
   ![Csatlakozás az Azure AD hello-beállítások menüjében](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png)</center>
3. Kattintson a **Folytatás** hello Azure AD Join üzenetablakban.
   
   <center>
   ![Az Azure AD JOIN üzenetablakban](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png)</center>
4. Adja meg a bejelentkezési hitelesítő adatokat. A bejelentkezés során tapasztal élmény tartalmazza, amelyek a szükséges toocomplete hitelesítési hello lépéseket. Ha egy összevont bérlői részét képezik, a rendszergazda biztosít a szervezet által futtatott hello összevonási élmény.
   <center>
   ![Adja meg a bejelentkezési hitelesítő adatok](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</center>
5. Ha a szervezet tooAzure AD csatlakoztatása Azure multi-factor Authentication hitelesítés van beállítva, adja meg a hello második tényezőként a folytatás előtt.
6. Kattintson a **elfogadás** a hello **engedélyezése a felügyelt eszközök toobe** képernyő.
7. "Az eszköz már csatlakoztatott tooyour szervezet az Azure AD" üdvözlőüzenetére kell megjelennie.

## <a name="additional-information"></a>További információ
* [További információk az Azure AD Join használati forgatókönyveiről](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Csatlakozás a tartományhoz csatlakozó eszközök tooAzure AD-hez Windows 10-es környezetben](active-directory-azureadjoin-devices-group-policy.md)
* [Az Azure AD Join beállítása](active-directory-azureadjoin-setup.md)
* [Microsoft Passporton keresztül jelszó nélkül identitások hitelesítése](active-directory-azureadjoin-passport.md)

