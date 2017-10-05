---
title: "További lépések a csoportok használatával access Management szolgáltatáshoz |} Microsoft Docs"
description: "Hogyan speciális-a következőre vonatkozó biztonsági csoportok és erőforrásokhoz való hozzáférés kezelése ezek a csoportok használatával történő kezelése."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 44350a3c-8ea1-4da1-aaac-7fc53933dd21
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 82fbeb379e90add09f7c569111053f6e9b1bc9c5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="managing-owners-for-a-group"></a>Egy csoport tulajdonosainak kezelése
Amennyiben az erőforrás tulajdonosa hozzá van rendelve hozzáférést egy erőforráshoz egy Azure Active Directory-csoportba, a csoport tagsága kezeli a csoport tulajdonosa. Az erőforrás tulajdonosa hatékonyan felhasználók hozzárendelése az erőforrást a csoport tulajdonosa engedélyt ad.

> [!IMPORTANT]
> A Microsoft javasolja, hogy az Azure Portalon található [Azure AD felügyeleti központból](https://aad.portal.azure.com) kezelje az Azure AD-t az ebben a cikkben javasolt klasszikus Azure portál helyett. 

## <a name="assigning-group-ownership"></a>Csoport tulajdonjogának hozzárendelése
**Egy olyan tulajdonost hozzáadása egy csoporthoz**

1. Az a [a klasszikus Azure portálon](https://manage.windowsazure.com), jelölje be **Active Directory**, majd nyissa meg a munkahely címtárában.
2. Válassza ki a **csoportok** lapra, és nyissa meg a csoportot, amelyhez hozzá szeretné adni a tulajdonosai számára.
3. Válassza ki **adja hozzá a tulajdonosok**.
4. Az a **adja hozzá a tulajdonosok** lapon, válassza ki a felhasználót, hogy ez a csoport tulajdonosa hozzá, és győződjön meg arról, hogy ez a név a **kijelölt** ablaktáblán.

**Egy olyan tulajdonost eltávolítása egy csoportból**

1. Az a [a klasszikus Azure portálon](https://manage.windowsazure.com), jelölje be **Active Directory**, majd nyissa meg a munkahely címtárában.
2. Válassza ki a **csoportok** lapot, és nyissa meg a csoportot, amely a el kívánja távolítani a tulajdonosa.
3. Válassza ki a **tulajdonosok** fülre.
4. Válassza ki a csoportból eltávolítani, majd válassza ki a kívánt tulajdonosát **eltávolítása**.

## <a name="additional-information"></a>További információ
E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.

* [Erőforráshozzáférés-kezelés Azure Active Directory-csoportokkal](active-directory-manage-groups.md)
* [Azure Active Directory-parancsmagok csoportbeállítások konfigurálásához](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Mi az az Azure Active Directory?](active-directory-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
