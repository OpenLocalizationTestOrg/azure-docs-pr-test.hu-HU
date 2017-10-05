---
title: "Létrehoz egy csoportot a felhasználók számára az Azure Active Directory |} Microsoft Docs"
description: "Hozzon létre egy csoportot az Azure Active Directoryban, és vegye fel tagokat a csoporthoz"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cc5f232a-1e77-45c2-b28b-1fcb4621725c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6d3d37761a9fdf9bd9801396d45f2fcd47efb0be
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-group-and-add-members-in-azure-active-directory"></a><span data-ttu-id="f9f4c-103">Hozzon létre egy csoportot, és tagokat vehet az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="f9f4c-103">Create a group and add members in Azure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f9f4c-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f9f4c-104">Azure portal</span></span>](active-directory-groups-create-azure-portal.md)
> * [<span data-ttu-id="f9f4c-105">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="f9f4c-105">Azure classic portal</span></span>](active-directory-accessmanagement-manage-groups.md)
> * [<span data-ttu-id="f9f4c-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f9f4c-106">PowerShell</span></span>](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

<span data-ttu-id="f9f4c-107">Ez a cikk azt ismerteti, hogyan hozhat létre és tölthet egy új csoportot az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="f9f4c-107">This article explains how to create and populate a new group in Azure Active Directory.</span></span> <span data-ttu-id="f9f4c-108">Egy csoport segítségével végrehajthat felügyeleti feladatokat, mint a hozzárendelése a licencek vagy hozzáférési engedélyekkel a felhasználók vagy eszközök számát egyszerre.</span><span class="sxs-lookup"><span data-stu-id="f9f4c-108">Use a group to perform management tasks such as assigning licenses or permissions to a number of users or devices at once.</span></span>

## <a name="how-do-i-create-a-group"></a><span data-ttu-id="f9f4c-109">Hogyan hozható létre csoport?</span><span class="sxs-lookup"><span data-stu-id="f9f4c-109">How do I create a group?</span></span>
1. <span data-ttu-id="f9f4c-110">Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="f9f4c-110">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="f9f4c-111">Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** a szövegmezőbe, majd válassza ki azt a **Enter**.</span><span class="sxs-lookup"><span data-stu-id="f9f4c-111">Select **More services**, enter **User and groups** in the text box, and then select **Enter**.</span></span>

   ![Nyitó felhasználók kezelése](./media/active-directory-groups-create-azure-portal/search-user-management.png)
3. <span data-ttu-id="f9f4c-113">Az a **felhasználók és csoportok** panelen válassza **összes csoport**.</span><span class="sxs-lookup"><span data-stu-id="f9f4c-113">On the **Users and groups** blade, select **All groups**.</span></span>

   ![A csoportok panel megnyitása](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)
4. <span data-ttu-id="f9f4c-115">Az a **felhasználók és csoportok – minden csoport** panelen válassza a **Hozzáadás** parancsot.</span><span class="sxs-lookup"><span data-stu-id="f9f4c-115">On the **Users and groups - All groups** blade, select the **Add** command.</span></span>

   ![Az Add parancs kiválasztása](./media/active-directory-groups-create-azure-portal/add-group-command.png)
5. <span data-ttu-id="f9f4c-117">Az a **csoport** panelen, egy nevet és leírást a csoporthoz hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="f9f4c-117">On the **Group** blade, add a name and description for the group.</span></span>
6. <span data-ttu-id="f9f4c-118">Válasszon ki a csoportba felvenni kívánt tagot, jelölje be **hozzárendelt** a a **tagságtípusának** mezőbe, majd válassza ki **tagok**.</span><span class="sxs-lookup"><span data-stu-id="f9f4c-118">To select members to add to the group, select **Assigned** in the **Membership type** box, and then select **Members**.</span></span> <span data-ttu-id="f9f4c-119">Való egy csoport tagságának dinamikusan felügyeletével kapcsolatos további információkért lásd: [attribútumok használata a csoporttagság speciális szabályok létrehozása](active-directory-groups-dynamic-membership-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f9f4c-119">For more information about how to manage the membership of a group dynamically, see [Using attributes to create advanced rules for group membership](active-directory-groups-dynamic-membership-azure-portal.md).</span></span>

   ![Tagok hozzáadása kiválasztása](./media/active-directory-groups-create-azure-portal/select-members.png)
7. <span data-ttu-id="f9f4c-121">Az a **tagok** panelen, jelölje be egy vagy több felhasználók vagy eszközök felvétele a csoportba, és válassza ki a **válasszon** gomb felvétele a csoportba a panel alján.</span><span class="sxs-lookup"><span data-stu-id="f9f4c-121">On the **Members** blade, select one or more users or devices to add to the group and select the **Select** button at the bottom of the blade to add them to the group.</span></span> <span data-ttu-id="f9f4c-122">A **felhasználói** mezőben szűrése a képernyőt a megfelelő felhasználó vagy eszköz nevek a bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="f9f4c-122">The **User** box filters the display based on matching your entry to any part of a user or device name.</span></span> <span data-ttu-id="f9f4c-123">Nem helyettesítő karakterek használata engedélyezett, hogy a mezőben.</span><span class="sxs-lookup"><span data-stu-id="f9f4c-123">No wildcard characters are accepted in that box.</span></span>
8. <span data-ttu-id="f9f4c-124">Amikor befejezte a tagok hozzáadását a csoporthoz, válassza ki a **létrehozása** a a **csoport** panelen.</span><span class="sxs-lookup"><span data-stu-id="f9f4c-124">When you finish adding members to the group, select **Create** on the **Group** blade.</span></span>    

   ![Hozzon létre a csoport megerősítése](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)


## <a name="next-steps"></a><span data-ttu-id="f9f4c-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f9f4c-126">Next steps</span></span>
<span data-ttu-id="f9f4c-127">E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="f9f4c-127">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="f9f4c-128">Tekintse meg a meglévő csoportok</span><span class="sxs-lookup"><span data-stu-id="f9f4c-128">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="f9f4c-129">Egy csoport beállításainak kezelése</span><span class="sxs-lookup"><span data-stu-id="f9f4c-129">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="f9f4c-130">A csoport tagjai kezelése</span><span class="sxs-lookup"><span data-stu-id="f9f4c-130">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="f9f4c-131">A csoport tagságát kezelése</span><span class="sxs-lookup"><span data-stu-id="f9f4c-131">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="f9f4c-132">A csoport dinamikus szabályok kezelése</span><span class="sxs-lookup"><span data-stu-id="f9f4c-132">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
