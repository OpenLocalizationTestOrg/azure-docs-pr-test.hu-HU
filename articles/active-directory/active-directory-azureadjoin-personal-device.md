---
title: "Személyes eszköz csatlakoztatása a szervezet |} Microsoft Docs"
description: "Azt ismerteti, hogy a felhasználók regisztrálhatják Windows 10 saját eszközét, hogy a vállalati hálózaton, és a BYOD-forgatókönyvhöz a központi telepítés lépéseit ismerteti."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 9f3d38f5-1cfd-43d4-97da-4fed1255a1ff
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 9418365ea18b065551448742b21c8b17a1749fc8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="join-a-personal-device-to-your-organization"></a>Személyes eszköz csatlakoztatása a szervezet
## <a name="to-join-a-windows-10-device-to-your-organization"></a>A szervezet egy Windows 10 rendszerű eszköz csatlakoztatása
1. Az a **Start** menüjében válassza **beállítások**.
2. Válassza ki **fiókok**, és kattintson a **fiókja**.
3. Kattintson a **adja hozzá a munkahelyi vagy iskolai fiókjával**, majd írja be a szervezeti fiókjával.
4. A szervezet a bejelentkezési lapon adja meg a felhasználónevét és jelszavát, és kattintson **OK**.
5. Rendszer bekéri a multi-factor authentication kihívást. (A a kérdés lehet egy informatikai rendszergazda konfigurálni.)
6. Azure Active Directory (Azure AD) ellenőrzi, hogy az eszköz igényli-e a mobileszköz-kezelési beléptetés.
7. A Windows regisztrálja az eszközt a szervezet címtárához az Azure AD-ben, és regisztrálja a mobileszköz-kezelés, ha szükséges.
8. Ha egy felügyelt felhasználók Windows végigvezeti Önt az asztalhoz az automatikus bejelentkezés.
9. Ha egy összevont felhasználó, akkor megnyílik a Windows bejelentkezési képernyő hitelesítő adatait.

## <a name="additional-information"></a>További információ
* [Vállalati használatú Windows 10: Az eszközök munkahelyi célú használata](active-directory-azureadjoin-windows10-devices-overview.md)
* [A felhőalapú képességek kiterjesztése a Windows 10-eszközökre az Azure Active Directory Joinon keresztül](active-directory-azureadjoin-user-upgrade.md)
* [Microsoft Passporton keresztül jelszó nélkül identitások hitelesítése](active-directory-azureadjoin-passport.md)
* [További információk az Azure AD Join használati forgatókönyveiről](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Tartományhoz csatlakoztatott eszközök csatlakoztatása az Azure AD-hez Windows 10-es környezetben](active-directory-azureadjoin-devices-group-policy.md)
* [Az Azure AD Join beállítása](active-directory-azureadjoin-setup.md)

