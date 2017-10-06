---
title: "aaaAssign felhasználók tooa egyéni tartományt az Azure Active Directoryban |} Microsoft Docs"
description: "Hogyan toopopulate a felhasználói fiókok Azure Active Directoryban egyéni tartományt."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 717b5a7c-7bc3-4ab1-98b5-4740b53338fe
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 23c338a361a90fddf42d4df90db94c9774305886
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="assign-users-tooa-custom-domain"></a>Rendelje hozzá a felhasználók tooa egyéni tartományt
Miután hozzáadta az egyéni tartomány tooAzure Active Directory, hozzá kell adnia a hello felhasználói fiókokat a tartományhoz, így el lehet kezdeni az hitelesítése őket.

> [!IMPORTANT]
> A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon. Hogyan toomanage a tartománynevek hello Azure AD felügyeleti központban, lásd: [az Azure Active Directoryban egyéni tartománynevek kezelése](active-directory-domains-manage-azure-portal.md).

## <a name="users-synced-from-a-on-premises-directory"></a>Az egy helyszíni Directoryból szinkronizált felhasználók
Ha már meg van kapcsolat között a helyszíni Active Directory és az Azure Active Directory, a szinkronizálás feltöltheti hello fiókok. További információk hogyan toosynchronize az Azure Active Directory a helyszíni Active Directoryban: [a helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).

## <a name="users-added-and-managed-in-hello-cloud"></a>Felhasználók hozzáadása és kezelése hello felhőben
meglévő felhasználói fiók toochange hello tartományt:

1. Nyissa meg hello klasszikus Azure portál egy olyan fiókkal, amely globális rendszergazda vagy a felhasználó rendszergazda segítségét.
2. Nyissa meg a címtárat.
3. Jelölje be hello **felhasználók** fülre.
4. Válassza ki a hello felhasználói hello listából.
5. Hello felhasználó hello tartományt, majd válassza ki **mentése**.

Ez is végezhető használatával [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) vagy hello [Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).

## <a name="select-a-custom-domain-when-creating-a-new-user"></a>Jelöljön ki egy egyéni tartományt, amikor új felhasználó létrehozása
1. Nyissa meg hello klasszikus Azure portál egy olyan fiókkal, amely globális rendszergazda vagy a felhasználó rendszergazda segítségét.
2. Nyissa meg a címtárat.
3. Jelölje be hello **felhasználók** fülre.
4. Hello parancssávon válassza **Hozzáadás**.
5. Hello felhasználónév hozzáadásakor hello tartomány listából válassza ki az egyéni tartomány hello.
6. Kattintson a **Mentés** gombra.

## <a name="next-steps"></a>Következő lépések
* [Az egyéni tartomány nevét toosimplify hello bejelentkezés során tapasztal élmény segítségével a felhasználók számára](active-directory-add-domain.md)
* [Egyéni tartománynevek kezelése](active-directory-add-manage-domain-names.md)
* [Ismerkedés az Azure AD tartománykezelési fogalmaival](active-directory-add-domain-concepts.md)

