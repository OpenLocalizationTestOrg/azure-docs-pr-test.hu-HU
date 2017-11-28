---
title: "Az Azure Analysis Services kiszolgálói rendszergazdák kezelése |} Microsoft Docs"
description: "Megtudhatja, hogyan kezelheti a kiszolgáló-rendszergazdák az Azure Analysis Services-kiszolgáló."
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
ms.openlocfilehash: a1b58125dafdf73f245b6a8cd0f4917513b22ea9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="manage-server-administrators"></a><span data-ttu-id="99db1-103">Kiszolgáló-rendszergazdák kezelése</span><span class="sxs-lookup"><span data-stu-id="99db1-103">Manage server administrators</span></span>
<span data-ttu-id="99db1-104">Kiszolgáló-rendszergazdák, amelyek a kiszolgálón található bérlőjének kell lennie egy érvényes felhasználó vagy csoport az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="99db1-104">Server administrators must be a valid user or group in the Azure Active Directory (Azure AD) for the tenant in which the server resides.</span></span> <span data-ttu-id="99db1-105">Használhat **Analysis Services rendszergazdák** a vezérlő panelen a kiszolgáló Azure-portálon, vagy a kiszolgáló tulajdonságai szolgáltatáshoz az ssms kiszolgáló-rendszergazdák kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="99db1-105">You can use **Analysis Services Admins** in the control blade for your server in Azure portal, or Server Properties in SSMS to manage server administrators.</span></span> 

## <a name="to-add-server-administrators-by-using-azure-portal"></a><span data-ttu-id="99db1-106">Kiszolgáló-rendszergazdák hozzáadása az Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="99db1-106">To add server administrators by using Azure portal</span></span>
1. <span data-ttu-id="99db1-107">A kiszolgáló a vezérlő paneljén kattintson **Analysis Services rendszergazdák**.</span><span class="sxs-lookup"><span data-stu-id="99db1-107">In the control blade for your server, click **Analysis Services Admins**.</span></span>
2. <span data-ttu-id="99db1-108">Az a  **\<kiszolgálónév >-Analysis Services rendszergazdák** panelen kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="99db1-108">In the **\<servername> - Analysis Services Admins** blade, click **Add**.</span></span>
3. <span data-ttu-id="99db1-109">Az a **kiszolgáló-rendszergazdák hozzáadása** panelen válassza ki a felhasználói fiókokat az Azure ad- vagy meghívhatja a külső felhasználók e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="99db1-109">In the **Add Server Administrators** blade, select user accounts from your Azure AD or invite external users by email address.</span></span>

    ![Kiszolgáló-rendszergazdák az Azure portálon](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="to-add-server-administrators-by-using-ssms"></a><span data-ttu-id="99db1-111">Kiszolgáló-rendszergazdák hozzáadása az SSMS használatával</span><span class="sxs-lookup"><span data-stu-id="99db1-111">To add server administrators by using SSMS</span></span>
1. <span data-ttu-id="99db1-112">Kattintson a jobb gombbal a kiszolgáló > **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="99db1-112">Right-click the server > **Properties**.</span></span>
2. <span data-ttu-id="99db1-113">A **elemzési kiszolgáló tulajdonságai**, kattintson a **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="99db1-113">In **Analysis Server Properties**, click **Security**.</span></span>
3. <span data-ttu-id="99db1-114">Kattintson a **Hozzáadás**, és adja meg az e-mail cím egy felhasználót vagy csoportot az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="99db1-114">Click **Add**, and then enter the email address for a user or group in your Azure AD.</span></span>
   
    ![Kiszolgáló-rendszergazdák hozzáadása a szolgáltatáshoz az ssms](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="next-steps"></a><span data-ttu-id="99db1-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="99db1-116">Next steps</span></span> 
[<span data-ttu-id="99db1-117">Hitelesítés és felhasználói engedélyek</span><span class="sxs-lookup"><span data-stu-id="99db1-117">Authentication and user permissions</span></span>](analysis-services-manage-users.md)  
[<span data-ttu-id="99db1-118">Adatbázis-szerepkörök és a felhasználók kezelése</span><span class="sxs-lookup"><span data-stu-id="99db1-118">Manage database roles and users</span></span>](analysis-services-database-users.md)  
[<span data-ttu-id="99db1-119">Szerepköralapú hozzáférés-vezérlés</span><span class="sxs-lookup"><span data-stu-id="99db1-119">Role-Based Access Control</span></span>](../active-directory/role-based-access-control-what-is.md)  

