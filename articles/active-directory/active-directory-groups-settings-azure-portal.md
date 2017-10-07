---
title: "az Azure Active Directoryban aaaManage tulajdonságai |} Microsoft Docs"
description: "Hogyan tooedit hello tulajdonságok és a konfigurációs beállításait, a csoport az Azure Active Directoryban"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 2f058f9a-5a8f-4b4b-b3b7-885ff10cb1be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: H1Hack27Feb2017;it-pro
ms.openlocfilehash: aa17c62b4824e5c2de8adc1d34cd9618f3e722f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hello-settings-for-a-group-in-azure-active-directory"></a><span data-ttu-id="18a93-103">Az Azure Active Directory csoport hello beállításainak kezelése</span><span class="sxs-lookup"><span data-stu-id="18a93-103">Manage hello settings for a group in Azure Active Directory</span></span>
<span data-ttu-id="18a93-104">Ez a cikk azt ismerteti, hogyan toochange hello egy csoporthoz az Azure Active Directory (Azure AD) beállításait.</span><span class="sxs-lookup"><span data-stu-id="18a93-104">This article explains how toochange hello settings for a group in Azure Active Directory (Azure AD).</span></span>

## <a name="how-do-i-find-and-change-hello-settings"></a><span data-ttu-id="18a93-105">Hogyan található és hello beállításainak módosítása?</span><span class="sxs-lookup"><span data-stu-id="18a93-105">How do I find and change hello settings?</span></span>
1. <span data-ttu-id="18a93-106">Jelentkezzen be toohello [az Azure AD felügyeleti központban](https://aad.portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="18a93-106">Sign in toohello [Azure AD admin center](https://aad.portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="18a93-107">Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** hello szövegmezőbe, és válassza ki **Enter**.</span><span class="sxs-lookup"><span data-stu-id="18a93-107">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

   ![Nyitó felhasználók és csoportok panelen](./media/active-directory-groups-settings-azure-portal/search-user-management.png)
3. <span data-ttu-id="18a93-109">A hello **felhasználók és csoportok** panelen válassza **összes csoport**.</span><span class="sxs-lookup"><span data-stu-id="18a93-109">On hello **Users and groups** blade, select **All groups**.</span></span>

   ![Minden megnyitásakor hello csoportok panelen](./media/active-directory-groups-settings-azure-portal/view-groups-blade.png)
4. <span data-ttu-id="18a93-111">A hello **felhasználók és csoportok – minden csoport** panelen válasszon ki egy csoportot.</span><span class="sxs-lookup"><span data-stu-id="18a93-111">On hello **Users and groups - All groups** blade, select a group.</span></span>
5. <span data-ttu-id="18a93-112">A hello **csoport - *groupname***  panelen válassza **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="18a93-112">On hello **Group - *groupname*** blade, select **Properties**.</span></span>

   ![Nyitó hello tulajdonságok panelen](./media/active-directory-groups-settings-azure-portal/select-group-properties.png)
6. <span data-ttu-id="18a93-114">Ha befejezte a hello csoport tulajdonságainak módosítását, válassza ki a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="18a93-114">When you finish changing properties for hello group, select **Save**.</span></span>    

   ![Tulajdonságok módosításainak mentése](./media/active-directory-groups-settings-azure-portal/save-group-properties.png)

## <a name="next-steps"></a><span data-ttu-id="18a93-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="18a93-116">Next steps</span></span>
<span data-ttu-id="18a93-117">E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="18a93-117">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="18a93-118">Tekintse meg a meglévő csoportok</span><span class="sxs-lookup"><span data-stu-id="18a93-118">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="18a93-119">Hozzon létre egy új csoportot és tagok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="18a93-119">Create a new group and adding members</span></span>](active-directory-groups-create-azure-portal.md)
* [<span data-ttu-id="18a93-120">A csoport tagjai kezelése</span><span class="sxs-lookup"><span data-stu-id="18a93-120">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="18a93-121">A csoport tagságát kezelése</span><span class="sxs-lookup"><span data-stu-id="18a93-121">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="18a93-122">A csoport dinamikus szabályok kezelése</span><span class="sxs-lookup"><span data-stu-id="18a93-122">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
