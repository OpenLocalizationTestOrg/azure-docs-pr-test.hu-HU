---
title: "adott csoporthoz tartozó felhasználók az Azure Active Directoryban aaaCreate |} Microsoft Docs"
description: "Hogyan toocreate egy csoportot az Azure Active Directoryban és tagok toohello csoport hozzáadása"
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
ms.openlocfilehash: fc583a7f02ce50e7f3b2c8f97a9c032a3e2dc33a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-group-and-add-members-in-azure-active-directory"></a><span data-ttu-id="741ba-103">Hozzon létre egy csoportot, és tagokat vehet az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="741ba-103">Create a group and add members in Azure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="741ba-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="741ba-104">Azure portal</span></span>](active-directory-groups-create-azure-portal.md)
> * [<span data-ttu-id="741ba-105">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="741ba-105">Azure classic portal</span></span>](active-directory-accessmanagement-manage-groups.md)
> * [<span data-ttu-id="741ba-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="741ba-106">PowerShell</span></span>](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

<span data-ttu-id="741ba-107">Ez a cikk azt ismerteti, hogyan toocreate és feltöltése az Azure Active Directoryban egy új csoportot.</span><span class="sxs-lookup"><span data-stu-id="741ba-107">This article explains how toocreate and populate a new group in Azure Active Directory.</span></span> <span data-ttu-id="741ba-108">Egy csoport tooperform felügyeleti feladatokhoz, mint az hozzárendelése licencek vagy hozzáférési engedélyekkel a felhasználók vagy eszközök tooa száma egyszerre használja.</span><span class="sxs-lookup"><span data-stu-id="741ba-108">Use a group tooperform management tasks such as assigning licenses or permissions tooa number of users or devices at once.</span></span>

## <a name="how-do-i-create-a-group"></a><span data-ttu-id="741ba-109">Hogyan hozható létre csoport?</span><span class="sxs-lookup"><span data-stu-id="741ba-109">How do I create a group?</span></span>
1. <span data-ttu-id="741ba-110">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="741ba-110">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="741ba-111">Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** hello szövegmezőbe, és válassza ki **Enter**.</span><span class="sxs-lookup"><span data-stu-id="741ba-111">Select **More services**, enter **User and groups** in hello text box, and then select **Enter**.</span></span>

   ![Nyitó felhasználók kezelése](./media/active-directory-groups-create-azure-portal/search-user-management.png)
3. <span data-ttu-id="741ba-113">A hello **felhasználók és csoportok** panelen válassza **összes csoport**.</span><span class="sxs-lookup"><span data-stu-id="741ba-113">On hello **Users and groups** blade, select **All groups**.</span></span>

   ![Nyitó hello csoportok panelen](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)
4. <span data-ttu-id="741ba-115">A hello **felhasználók és csoportok – minden csoport** panelen, jelölje be hello **Hozzáadás** parancsot.</span><span class="sxs-lookup"><span data-stu-id="741ba-115">On hello **Users and groups - All groups** blade, select hello **Add** command.</span></span>

   ![Hello hozzáadása paranccsal](./media/active-directory-groups-create-azure-portal/add-group-command.png)
5. <span data-ttu-id="741ba-117">A hello **csoport** panelen név és leírás hello csoport hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="741ba-117">On hello **Group** blade, add a name and description for hello group.</span></span>
6. <span data-ttu-id="741ba-118">tooselect tagok tooadd toohello csoportban válassza **hozzárendelt** a hello **tagságtípusának** mezőbe, majd válassza ki **tagok**.</span><span class="sxs-lookup"><span data-stu-id="741ba-118">tooselect members tooadd toohello group, select **Assigned** in hello **Membership type** box, and then select **Members**.</span></span> <span data-ttu-id="741ba-119">Hogyan a toomanage hello csoport tagságának dinamikusan kapcsolatos további információkért lásd: [attribútumok toocreate használata speciális szabályok csoporttagság](active-directory-groups-dynamic-membership-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="741ba-119">For more information about how toomanage hello membership of a group dynamically, see [Using attributes toocreate advanced rules for group membership](active-directory-groups-dynamic-membership-azure-portal.md).</span></span>

   ![Tagok tooadd kiválasztása](./media/active-directory-groups-create-azure-portal/select-members.png)
7. <span data-ttu-id="741ba-121">A hello **tagok** panelen, válasszon ki egy vagy több felhasználók vagy eszközök tooadd toohello csoport és select hello **válasszon** gomb hello panel tooadd hello alján őket toohello csoport.</span><span class="sxs-lookup"><span data-stu-id="741ba-121">On hello **Members** blade, select one or more users or devices tooadd toohello group and select hello **Select** button at hello bottom of hello blade tooadd them toohello group.</span></span> <span data-ttu-id="741ba-122">Hello **felhasználói** szűri hello megjelenítési megfelelő a bejegyzés tooany része egy felhasználó vagy eszköz nevét.</span><span class="sxs-lookup"><span data-stu-id="741ba-122">hello **User** box filters hello display based on matching your entry tooany part of a user or device name.</span></span> <span data-ttu-id="741ba-123">Nem helyettesítő karakterek használata engedélyezett, hogy a mezőben.</span><span class="sxs-lookup"><span data-stu-id="741ba-123">No wildcard characters are accepted in that box.</span></span>
8. <span data-ttu-id="741ba-124">Amikor befejezte a tagok toohello csoport hozzáadása, válassza ki a **létrehozása** a hello **csoport** panelen.</span><span class="sxs-lookup"><span data-stu-id="741ba-124">When you finish adding members toohello group, select **Create** on hello **Group** blade.</span></span>    

   ![Hozzon létre a csoport megerősítése](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)


## <a name="next-steps"></a><span data-ttu-id="741ba-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="741ba-126">Next steps</span></span>
<span data-ttu-id="741ba-127">E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="741ba-127">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="741ba-128">Tekintse meg a meglévő csoportok</span><span class="sxs-lookup"><span data-stu-id="741ba-128">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="741ba-129">Egy csoport beállításainak kezelése</span><span class="sxs-lookup"><span data-stu-id="741ba-129">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="741ba-130">A csoport tagjai kezelése</span><span class="sxs-lookup"><span data-stu-id="741ba-130">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="741ba-131">A csoport tagságát kezelése</span><span class="sxs-lookup"><span data-stu-id="741ba-131">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="741ba-132">A csoport dinamikus szabályok kezelése</span><span class="sxs-lookup"><span data-stu-id="741ba-132">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
