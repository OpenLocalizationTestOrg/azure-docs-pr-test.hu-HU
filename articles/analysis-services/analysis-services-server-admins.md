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
# <a name="manage-server-administrators"></a><span data-ttu-id="9b066-103">Kiszolgáló-rendszergazdák kezelése</span><span class="sxs-lookup"><span data-stu-id="9b066-103">Manage server administrators</span></span>
<span data-ttu-id="9b066-104">Kiszolgáló-rendszergazdák, mely hello server található hello bérlőjének kell lennie egy érvényes felhasználót vagy csoportot az hello Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9b066-104">Server administrators must be a valid user or group in hello Azure Active Directory (Azure AD) for hello tenant in which hello server resides.</span></span> <span data-ttu-id="9b066-105">Használhat **Analysis Services rendszergazdák** hello vezérlő panelen a kiszolgáló Azure-portálon, vagy az SSMS toomanage rendszergazdái kiszolgáló tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="9b066-105">You can use **Analysis Services Admins** in hello control blade for your server in Azure portal, or Server Properties in SSMS toomanage server administrators.</span></span> 

## <a name="tooadd-server-administrators-by-using-azure-portal"></a><span data-ttu-id="9b066-106">tooadd kiszolgáló-rendszergazdák az Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="9b066-106">tooadd server administrators by using Azure portal</span></span>
1. <span data-ttu-id="9b066-107">A kiszolgáló hello vezérlő paneljén kattintson **Analysis Services rendszergazdák**.</span><span class="sxs-lookup"><span data-stu-id="9b066-107">In hello control blade for your server, click **Analysis Services Admins**.</span></span>
2. <span data-ttu-id="9b066-108">A hello  **\<kiszolgálónév >-Analysis Services rendszergazdák** panelen kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="9b066-108">In hello **\<servername> - Analysis Services Admins** blade, click **Add**.</span></span>
3. <span data-ttu-id="9b066-109">A hello **kiszolgáló-rendszergazdák hozzáadása** panelen válassza ki a felhasználói fiókokat az Azure ad- vagy meghívhatja a külső felhasználók e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="9b066-109">In hello **Add Server Administrators** blade, select user accounts from your Azure AD or invite external users by email address.</span></span>

    ![Kiszolgáló-rendszergazdák az Azure portálon](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="tooadd-server-administrators-by-using-ssms"></a><span data-ttu-id="9b066-111">kiszolgáló-rendszergazdák tooadd szolgáltatáshoz az SSMS használatával</span><span class="sxs-lookup"><span data-stu-id="9b066-111">tooadd server administrators by using SSMS</span></span>
1. <span data-ttu-id="9b066-112">Kattintson a jobb gombbal hello server > **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="9b066-112">Right-click hello server > **Properties**.</span></span>
2. <span data-ttu-id="9b066-113">A **elemzési kiszolgáló tulajdonságai**, kattintson a **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="9b066-113">In **Analysis Server Properties**, click **Security**.</span></span>
3. <span data-ttu-id="9b066-114">Kattintson a **Hozzáadás**, és adja meg hello e-mail cím egy felhasználót vagy csoportot az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="9b066-114">Click **Add**, and then enter hello email address for a user or group in your Azure AD.</span></span>
   
    ![Kiszolgáló-rendszergazdák hozzáadása a szolgáltatáshoz az ssms](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="next-steps"></a><span data-ttu-id="9b066-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9b066-116">Next steps</span></span> 
[<span data-ttu-id="9b066-117">Hitelesítés és felhasználói engedélyek</span><span class="sxs-lookup"><span data-stu-id="9b066-117">Authentication and user permissions</span></span>](analysis-services-manage-users.md)  
[<span data-ttu-id="9b066-118">Adatbázis-szerepkörök és a felhasználók kezelése</span><span class="sxs-lookup"><span data-stu-id="9b066-118">Manage database roles and users</span></span>](analysis-services-database-users.md)  
[<span data-ttu-id="9b066-119">Szerepköralapú hozzáférés-vezérlés</span><span class="sxs-lookup"><span data-stu-id="9b066-119">Role-Based Access Control</span></span>](../active-directory/role-based-access-control-what-is.md)  

