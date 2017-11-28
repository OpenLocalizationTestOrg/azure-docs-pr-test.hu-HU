---
title: "SaaS-alkalmazásokhoz való hozzáférés kezelése csoport segítségével |} Microsoft Docs"
description: "Hogyan használható az Azure Active Directory prémium vagy alapszintű kiadásra csoportok hozzáférés hozzárendelése az Azure Active Directoryval integrált SaaS-alkalmazásokhoz."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: ab8dee63-bedc-46ca-8852-234f5c16ae98
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: it-pro;oldportal
ms.openlocfilehash: d350011ee9fc5ced9ddb16993f68d3c840a645a5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="using-a-group-to-manage-access-to-saas-applications"></a><span data-ttu-id="7074c-103">Hozzáférés kezelése SaaS alkalmazásokhoz egy csoport használatával</span><span class="sxs-lookup"><span data-stu-id="7074c-103">Using a group to manage access to SaaS applications</span></span>
<span data-ttu-id="7074c-104">Az Azure Active Directoryval (Azure AD) az Azure AD prémium vagy alapszintű Azure AD-licenccel rendelkező, segítségével csoportok hozzáférés hozzárendelése integrálva van az Azure AD SaaS-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="7074c-104">Using Azure Active Directory (Azure AD) with an Azure AD Premium or Azure AD Basic license, you can use groups to assign access to a SaaS application that's integrated with Azure AD.</span></span> <span data-ttu-id="7074c-105">Például szeretne hozzárendelni a marketingrészleg használatára öt különböző SaaS-alkalmazásokhoz hozzáférést, ha akkor is hozzon létre egy csoportot, amely tartalmazza a felhasználók a marketingosztályon belül, és hozzárendelheti csoport ezek öt SaaS-alkalmazások által használt a marketingrészleg.</span><span class="sxs-lookup"><span data-stu-id="7074c-105">For example, if you want to assign access for the marketing department to use five different SaaS applications, you can create a group that contains the users in the marketing department, and then assign that group to these five SaaS applications that are needed by the marketing department.</span></span> <span data-ttu-id="7074c-106">Így időt takaríthat meg egy helyen a marketingrészleg tagjainak kezelésével.</span><span class="sxs-lookup"><span data-stu-id="7074c-106">This way you can save time by managing the membership of the marketing department in one place.</span></span> <span data-ttu-id="7074c-107">Felhasználók majd vannak hozzárendelve az alkalmazás hozzáadásuk után a marketing csoport tagjaként, és a hozzárendeléseik eltávolította az alkalmazásból a marketing csoport eltávolításakor.</span><span class="sxs-lookup"><span data-stu-id="7074c-107">Users then are assigned to the application when they are added as members of the marketing group, and have their assignments removed from the application when they are removed from the marketing group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7074c-108">A Microsoft javasolja, hogy az Azure Portalon található [Azure AD felügyeleti központból](https://aad.portal.azure.com) kezelje az Azure AD-t az ebben a cikkben javasolt klasszikus Azure portál helyett.</span><span class="sxs-lookup"><span data-stu-id="7074c-108">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span> 

<span data-ttu-id="7074c-109">Ez a funkció több száz alkalmazásokat, amelyek adhat hozzá az Azure AD Application Gallery használható.</span><span class="sxs-lookup"><span data-stu-id="7074c-109">This capability can be used with hundreds of applications that you can add from within the Azure AD Application Gallery.</span></span>

<span data-ttu-id="7074c-110">**Egy csoport hozzáférés hozzárendelése egy SaaS-alkalmazáshoz**</span><span class="sxs-lookup"><span data-stu-id="7074c-110">**To assign access for a group to a SaaS application**</span></span>

1. <span data-ttu-id="7074c-111">Az a [a klasszikus Azure portálon](https://manage.windowsazure.com), jelölje be **Active Directory** a bal oldali navigációs sávján látható.</span><span class="sxs-lookup"><span data-stu-id="7074c-111">In the [Azure classic portal](https://manage.windowsazure.com), select **Active Directory** on the navigation bar on the left hand side.</span></span>
2. <span data-ttu-id="7074c-112">Válassza ki a **Directory** lapra, és nyissa meg a könyvtárat, amelyben hozzáférési csoport egy SaaS-alkalmazáshoz hozzárendelni kívánt.</span><span class="sxs-lookup"><span data-stu-id="7074c-112">Select the **Directory** tab, and then open the directory in which you want to assign access for a group to a SaaS application.</span></span>
3. <span data-ttu-id="7074c-113">Válassza ki a **alkalmazások** fülre. Válasszon ki egy alkalmazást, az alkalmazás-galériából hozzáadott, és kattintson a **felhasználók és csoportok** fülre.</span><span class="sxs-lookup"><span data-stu-id="7074c-113">Select the **Applications** tab. Select an application that you added from the Application Gallery, and then click  the **Users and Groups** tab.</span></span>
4. <span data-ttu-id="7074c-114">Az a **felhasználók és csoportok** lap a **kezdve** mezőben adja meg a kívánt hozzáférés hozzárendelése a csoport nevét, majd válassza ki a pipa jelre a jobb felső részén.</span><span class="sxs-lookup"><span data-stu-id="7074c-114">On the **Users and Groups** tab, in the **Starting with** field, enter the name of the group to which you want to assign access, and then select the check mark in the upper right.</span></span> <span data-ttu-id="7074c-115">Csak Önnek kell beírnia a csoportnév első része.</span><span class="sxs-lookup"><span data-stu-id="7074c-115">You only need to type the first part of a group's name.</span></span>
5. <span data-ttu-id="7074c-116">Válassza ki azt a csoportot, majd válassza ki a **hozzáférés hozzárendelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="7074c-116">Select the group, then then select the **Assign Access** button.</span></span> <span data-ttu-id="7074c-117">Válassza ki **Igen** , amikor megjelenik a jóváhagyást kérő üzenet.</span><span class="sxs-lookup"><span data-stu-id="7074c-117">Select **Yes** when you see the confirmation message.</span></span> <span data-ttu-id="7074c-118">A beágyazott csoporttagság az alkalmazásokhoz történő csoportalapú hozzárendeléseknél egyelőre nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="7074c-118">Nested group memberships are not supported for group-based assignment to applications at this time.</span></span>
6. <span data-ttu-id="7074c-119">Mely felhasználók legyenek társítva az alkalmazáshoz, vagy közvetlenül a csoport tagsága is megtekinthető.</span><span class="sxs-lookup"><span data-stu-id="7074c-119">You can also see which users are assigned to the application, either directly or by membership in a group.</span></span> <span data-ttu-id="7074c-120">Ehhez az szükséges, módosítsa a **legördülő lista megjelenítése "csoportból** való **"Minden felhasználó"**.</span><span class="sxs-lookup"><span data-stu-id="7074c-120">To do this, change the **Show dropdown from 'Groups'** to **'All Users'**.</span></span> <span data-ttu-id="7074c-121">A listán látható a felhasználók a könyvtárban, és-e minden felhasználóhoz hozzá van rendelve az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="7074c-121">The list shows users in the directory and whether or not each user is assigned to the application.</span></span> <span data-ttu-id="7074c-122">A lista mutatja azokat is, hogy a hozzárendelt felhasználó kapcsolódik, az alkalmazás közvetlenül (az hozzárendelés-típus "Közvetlen" jelenik meg), vagy csoporttagság (hozzárendelés-típus megjelennek az helyeként "Örökölt.") alapján</span><span class="sxs-lookup"><span data-stu-id="7074c-122">The list also shows whether the assigned users are assigned to the application directly (assignment type shown as 'Direct'), or by virtue of group membership (assignment type shown as 'Inherited.')</span></span>

> [!NOTE]
> <span data-ttu-id="7074c-123">Csak a prémium szintű Azure AD vagy az Azure AD alapvető engedélyezése után a felhasználók és csoportok lapon tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="7074c-123">You can see the Users and Groups tab only after you have enabled Azure AD Premium or Azure AD Basic.</span></span>
>
>

### <a name="next-steps"></a><span data-ttu-id="7074c-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7074c-124">Next steps</span></span>
<span data-ttu-id="7074c-125">E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="7074c-125">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="7074c-126">Erőforráshozzáférés-kezelés Azure Active Directory-csoportokkal</span><span class="sxs-lookup"><span data-stu-id="7074c-126">Managing access to resources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="7074c-127">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="7074c-127">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="7074c-128">Azure Active Directory-parancsmagok csoportbeállítások konfigurálásához</span><span class="sxs-lookup"><span data-stu-id="7074c-128">Azure Active Directory cmdlets for configuring group settings</span></span>](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [<span data-ttu-id="7074c-129">Mi az az Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7074c-129">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="7074c-130">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="7074c-130">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
