---
title: "az Azure Analysis Services aaaManage kiszolgáló-rendszergazdák |} Microsoft Docs"
description: "Megtudhatja, hogyan toomanage kiszolgáló-rendszergazdák az Azure Analysis Services-kiszolgáló."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: e04387e48e9b9483c382ee5cc9fd65f8331fb2a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-server-administrators"></a>Kiszolgáló-rendszergazdák kezelése
Kiszolgáló-rendszergazdák, mely hello server található hello bérlőjének kell lennie egy érvényes felhasználót vagy csoportot az hello Azure Active Directory (Azure AD). Használhat **Analysis Services rendszergazdák** hello vezérlő panelen a kiszolgáló Azure-portálon, vagy az SSMS toomanage rendszergazdái kiszolgáló tulajdonságai. 

## <a name="tooadd-server-administrators-by-using-azure-portal"></a>tooadd kiszolgáló-rendszergazdák az Azure portál használatával
1. A kiszolgáló hello vezérlő paneljén kattintson **Analysis Services rendszergazdák**.
2. A hello  **\<kiszolgálónév >-Analysis Services rendszergazdák** panelen kattintson a **Hozzáadás**.
3. A hello **kiszolgáló-rendszergazdák hozzáadása** panelen válassza ki a felhasználói fiókokat az Azure ad- vagy meghívhatja a külső felhasználók e-mail címét.

    ![Kiszolgáló-rendszergazdák az Azure portálon](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="tooadd-server-administrators-by-using-ssms"></a>kiszolgáló-rendszergazdák tooadd szolgáltatáshoz az SSMS használatával
1. Kattintson a jobb gombbal hello server > **tulajdonságok**.
2. A **elemzési kiszolgáló tulajdonságai**, kattintson a **biztonsági**.
3. Kattintson a **Hozzáadás**, és adja meg hello e-mail cím egy felhasználót vagy csoportot az Azure AD-ben.
   
    ![Kiszolgáló-rendszergazdák hozzáadása a szolgáltatáshoz az ssms](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="next-steps"></a>Következő lépések 
[Hitelesítés és felhasználói engedélyek](analysis-services-manage-users.md)  
[Adatbázis-szerepkörök és a felhasználók kezelése](analysis-services-database-users.md)  
[Szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md)  

