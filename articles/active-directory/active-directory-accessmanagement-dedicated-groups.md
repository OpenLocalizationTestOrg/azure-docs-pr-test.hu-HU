---
title: az Azure Active Directoryban aaaDedicated csoportok |} Microsoft Docs
description: "Hogyan dedikált csoportok áttekintése az Azure Active Directory és a létrehozott hogyan működnek."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 86158909-083a-41fe-8090-955e96ad1865
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: curtand
ms.openlocfilehash: 8feec6e1a4e6b384470392d3043caeeec2b03dd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="dedicated-groups-in-azure-active-directory"></a><span data-ttu-id="a79c7-103">Dedikált csoportok az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="a79c7-103">Dedicated groups in Azure Active Directory</span></span>
<span data-ttu-id="a79c7-104">Az Azure Active Directory (Azure AD) a hello dedikált csoportok funkció automatikusan létrehozza, és tölti fel az előre definiált az Azure AD-csoportok tagságát.</span><span class="sxs-lookup"><span data-stu-id="a79c7-104">In Azure Active Directory (Azure AD), hello dedicated groups feature automatically creates and populates membership for Azure AD predefined groups.</span></span> <span data-ttu-id="a79c7-105">Dedikált csoportok tagjai nem adható hozzá, vagy eltávolított használatával hello Azure klasszikus portál, a Windows PowerShell-parancsmagok, illetve programozott módon.</span><span class="sxs-lookup"><span data-stu-id="a79c7-105">Members of dedicated groups cannot be added or removed using hello Azure classic portal, Windows PowerShell cmdlets, or programmatically.</span></span>

> [!NOTE]
> <span data-ttu-id="a79c7-106">Dedikált csoportok szükséges, hogy a van rendelve egy Azure AD Premium-licenc</span><span class="sxs-lookup"><span data-stu-id="a79c7-106">Dedicated groups require that an Azure AD Premium license is assigned to</span></span>
>
> * <span data-ttu-id="a79c7-107">hello felügyelő rendszergazdához; csoporton hello szabály</span><span class="sxs-lookup"><span data-stu-id="a79c7-107">hello administrator who manages hello rule on a group</span></span>
> * <span data-ttu-id="a79c7-108">minden felhasználó, aki hello szerint be vannak jelölve szabály toobe hello csoport tagjai</span><span class="sxs-lookup"><span data-stu-id="a79c7-108">all users who are selected by hello rule toobe a member of hello group</span></span>
>
>

<span data-ttu-id="a79c7-109">**tooenable dedikált csoportok**</span><span class="sxs-lookup"><span data-stu-id="a79c7-109">**tooenable dedicated groups**</span></span>

1. <span data-ttu-id="a79c7-110">A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), jelölje be **Active Directory**, majd nyissa meg a munkahely címtárában.</span><span class="sxs-lookup"><span data-stu-id="a79c7-110">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then open your organization’s directory.</span></span>
2. <span data-ttu-id="a79c7-111">Jelölje be hello **csoportok** fülre, és azt szeretné, hogy tooedit majd nyitott hello csoport.</span><span class="sxs-lookup"><span data-stu-id="a79c7-111">Select hello **Groups** tab, and then open hello group you want tooedit.</span></span>
3. <span data-ttu-id="a79c7-112">Jelölje be hello **konfigurálása** lapot, és utána állítsa be **dedikált csoportok engedélyezése** túl**Igen**.</span><span class="sxs-lookup"><span data-stu-id="a79c7-112">Select hello **Configure** tab, and then set **Enable Dedicated Groups** too**Yes**.</span></span>

<span data-ttu-id="a79c7-113">Dedikált csoportok engedélyezéséhez váltson hello túl beállítása után**Igen**, további engedélyezheti hello directory tooautomatically hello minden felhasználó dedikált csoport létrehozásához hello beállítása **az "Összes felhasználó" csoport** Váltás túl**Igen**.</span><span class="sxs-lookup"><span data-stu-id="a79c7-113">Once hello Enable Dedicated Groups switch is set too**Yes**, you can further enable hello directory tooautomatically create hello All Users dedicated group by setting hello **Enable “All Users” Group** switch too**Yes**.</span></span> <span data-ttu-id="a79c7-114">Is szerkesztheti a kijelölt csoport hello nevét írja be a hello **"Minden felhasználó" megjelenített név csoport** mező.</span><span class="sxs-lookup"><span data-stu-id="a79c7-114">You can then also edit hello name of this dedicated group by typing it in hello **Display Name for “All Users” Group** field.</span></span>

<span data-ttu-id="a79c7-115">hello minden felhasználói csoport használható tooassign hello azonos tooall hello felhasználók engedélyeinek a címtárban.</span><span class="sxs-lookup"><span data-stu-id="a79c7-115">hello All Users group can be used tooassign hello same permissions tooall hello users in your directory.</span></span> <span data-ttu-id="a79c7-116">Például a könyvtár hozzáférési tooa SaaS-alkalmazás minden felhasználó is megadta, ha a hozzáférés az összes felhasználó dedikált csoport toothis alkalmazás hello hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="a79c7-116">For example, you can grant all users in your directory access tooa SaaS application by assigning access for hello All Users dedicated group toothis application.</span></span>

<span data-ttu-id="a79c7-117">dedikált hello minden felhasználó csoportban hello könyvtárban, beleértve a vendégek és a külső felhasználók felhasználókat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a79c7-117">hello dedicated All Users group includes all users in hello directory, including guests and external users.</span></span> <span data-ttu-id="a79c7-118">Ha egy csoportot kell, amely nem tartalmazza a külső felhasználók számára, akkor ez által csoportot hoz létre egy attribútum-alapú dinamikus szabály például hello következő érhető el:</span><span class="sxs-lookup"><span data-stu-id="a79c7-118">If you need a group that excludes external users, then you can accomplish this by creating a group with an attribute-based dynamic rule such as hello following:</span></span>

                (user.userPrincipalName -notContains "#EXT#@")

<span data-ttu-id="a79c7-119">Egy csoport, amely nem tartalmazza az összes vendégek például hello következő szabályt használni:</span><span class="sxs-lookup"><span data-stu-id="a79c7-119">For a group that excludes all Guests, use a rule such as hello following:</span></span>

                (user.userType -ne "Guest")

<span data-ttu-id="a79c7-120">arról, hogyan toolearn toocreate *speciális* (szabályok többszörös összehasonlítást is tartalmazó) szabályok dinamikus csoporttagság, lásd: [toocreate attribútumok használata speciális szabályok](active-directory-accessmanagement-groups-with-advanced-rules.md).</span><span class="sxs-lookup"><span data-stu-id="a79c7-120">toolearn about how toocreate *advanced* rules (rules that can contain multiple comparisons) for dynamic group membership, see [Using attributes toocreate advanced rules](active-directory-accessmanagement-groups-with-advanced-rules.md).</span></span>

### <a name="next-steps"></a><span data-ttu-id="a79c7-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a79c7-121">Next steps</span></span>
<span data-ttu-id="a79c7-122">E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="a79c7-122">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="a79c7-123">Hozzáférés tooresources kezelése az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="a79c7-123">Managing access tooresources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="a79c7-124">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="a79c7-124">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="a79c7-125">Mi az az Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a79c7-125">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="a79c7-126">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="a79c7-126">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
