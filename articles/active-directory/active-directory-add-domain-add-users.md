---
title: "Felhasználók hozzárendelése egyéni tartományhoz az Azure Active Directoryban |} Microsoft Docs"
description: "Hogyan tölthető fel a felhasználói fiókok Azure Active Directoryban egyéni tartományt."
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
ms.openlocfilehash: 39cb54a6637088c35c6aef864a804c24803f48ba
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="assign-users-to-a-custom-domain"></a>Felhasználók hozzárendelése egyéni tartományhoz
Miután hozzáadta az egyéni tartomány az Azure Active Directoryhoz, hozzá kell adnia a felhasználói fiókokat a tartományhoz, hogy elkezdheti hitelesítése őket.

> [!IMPORTANT]
> A Microsoft javasolja, hogy az Azure Portalon található [Azure AD felügyeleti központból](https://aad.portal.azure.com) kezelje az Azure AD-t az ebben a cikkben javasolt klasszikus Azure portál helyett. Az Azure AD felügyeleti központban a tartománynevek kezelése című történő [az Azure Active Directoryban egyéni tartománynevek kezelése](active-directory-domains-manage-azure-portal.md).

## <a name="users-synced-from-a-on-premises-directory"></a>Az egy helyszíni Directoryból szinkronizált felhasználók
Ha már meg van kapcsolat között a helyszíni Active Directory és az Azure Active Directory, a szinkronizálás feltöltheti a fiókokat. Az Azure Active Directory szinkronizálása a helyszíni Active Directoryban további információkért lásd: [a helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).

## <a name="users-added-and-managed-in-the-cloud"></a>Felhasználók hozzáadása és kezelése a felhőben
A tartomány egy olyan meglévő felhasználói fiók módosítása:

1. Nyissa meg a klasszikus Azure portálra egy olyan fiókkal, amely globális rendszergazda vagy a felhasználó rendszergazda segítségét.
2. Nyissa meg a címtárat.
3. Válassza a **Felhasználók** lapot.
4. Válassza ki a felhasználót a listáról.
5. A felhasználó a tartományt, majd válassza ki **mentése**.

Ez is végezhető használatával [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) vagy a [Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).

## <a name="select-a-custom-domain-when-creating-a-new-user"></a>Jelöljön ki egy egyéni tartományt, amikor új felhasználó létrehozása
1. Nyissa meg a klasszikus Azure portálra egy olyan fiókkal, amely globális rendszergazda vagy a felhasználó rendszergazda segítségét.
2. Nyissa meg a címtárat.
3. Válassza a **Felhasználók** lapot.
4. A parancssávon válassza **Hozzáadás**.
5. Amikor a felhasználó nevét, a tartomány listából válassza ki az egyéni tartomány.
6. Kattintson a **Mentés** gombra.

## <a name="next-steps"></a>Következő lépések
* [Egyéni tartománynevek használatával egyszerűbbé teheti a felhasználói bejelentkezési élmény](active-directory-add-domain.md)
* [Egyéni tartománynevek kezelése](active-directory-add-manage-domain-names.md)
* [Ismerkedés az Azure AD tartománykezelési fogalmaival](active-directory-add-domain-concepts.md)

