---
title: "Az Azure Active Directory csoport tulajdonságainak kezelése |} Microsoft Docs"
description: "A tulajdonságok és egyéb Azure Active Directoryban egy csoport konfigurációs beállításainak szerkesztése"
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
ms.openlocfilehash: b4baccafc0a9178223dbf64c664fc34ab9f7f916
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="manage-the-settings-for-a-group-in-azure-active-directory"></a><span data-ttu-id="0acbc-103">Az Azure Active Directory csoport beállításainak kezelése</span><span class="sxs-lookup"><span data-stu-id="0acbc-103">Manage the settings for a group in Azure Active Directory</span></span>
<span data-ttu-id="0acbc-104">Ez a cikk ismerteti az Azure Active Directory (Azure AD) csoport beállításainak módosításához.</span><span class="sxs-lookup"><span data-stu-id="0acbc-104">This article explains how to change the settings for a group in Azure Active Directory (Azure AD).</span></span>

## <a name="how-do-i-find-and-change-the-settings"></a><span data-ttu-id="0acbc-105">Hogyan található és módosíthatja a beállításokat?</span><span class="sxs-lookup"><span data-stu-id="0acbc-105">How do I find and change the settings?</span></span>
1. <span data-ttu-id="0acbc-106">Jelentkezzen be az [Azure AD felügyeleti központba](https://aad.portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="0acbc-106">Sign in to the [Azure AD admin center](https://aad.portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="0acbc-107">Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** a szövegmezőbe, majd válassza ki azt a **Enter**.</span><span class="sxs-lookup"><span data-stu-id="0acbc-107">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

   ![Nyitó felhasználók és csoportok panelen](./media/active-directory-groups-settings-azure-portal/search-user-management.png)
3. <span data-ttu-id="0acbc-109">Az a **felhasználók és csoportok** panelen válassza **összes csoport**.</span><span class="sxs-lookup"><span data-stu-id="0acbc-109">On the **Users and groups** blade, select **All groups**.</span></span>

   ![Az összes csoport panel megnyitása](./media/active-directory-groups-settings-azure-portal/view-groups-blade.png)
4. <span data-ttu-id="0acbc-111">Az a **felhasználók és csoportok – minden csoport** panelen válasszon ki egy csoportot.</span><span class="sxs-lookup"><span data-stu-id="0acbc-111">On the **Users and groups - All groups** blade, select a group.</span></span>
5. <span data-ttu-id="0acbc-112">Az a **csoport - *groupname***  panelen válassza **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="0acbc-112">On the **Group - *groupname*** blade, select **Properties**.</span></span>

   ![A Tulajdonságok panelére léphet megnyitása](./media/active-directory-groups-settings-azure-portal/select-group-properties.png)
6. <span data-ttu-id="0acbc-114">Ha befejezte a csoport tulajdonságai, válassza ki a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="0acbc-114">When you finish changing properties for the group, select **Save**.</span></span>    

   ![Tulajdonságok módosításainak mentése](./media/active-directory-groups-settings-azure-portal/save-group-properties.png)

## <a name="next-steps"></a><span data-ttu-id="0acbc-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0acbc-116">Next steps</span></span>
<span data-ttu-id="0acbc-117">E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="0acbc-117">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="0acbc-118">Tekintse meg a meglévő csoportok</span><span class="sxs-lookup"><span data-stu-id="0acbc-118">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="0acbc-119">Hozzon létre egy új csoportot és tagok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0acbc-119">Create a new group and adding members</span></span>](active-directory-groups-create-azure-portal.md)
* [<span data-ttu-id="0acbc-120">A csoport tagjai kezelése</span><span class="sxs-lookup"><span data-stu-id="0acbc-120">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="0acbc-121">A csoport tagságát kezelése</span><span class="sxs-lookup"><span data-stu-id="0acbc-121">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="0acbc-122">A csoport dinamikus szabályok kezelése</span><span class="sxs-lookup"><span data-stu-id="0acbc-122">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
