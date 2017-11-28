---
title: "Dedikált csoportok az Azure Active Directoryban |} Microsoft Docs"
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
ms.openlocfilehash: d9decd5de6a5bafc525edc5b04c82701185088ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="dedicated-groups-in-azure-active-directory"></a><span data-ttu-id="60911-103">Dedikált csoportok az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="60911-103">Dedicated groups in Azure Active Directory</span></span>
<span data-ttu-id="60911-104">Az Azure Active Directory (Azure AD) a dedikált csoportok funkció automatikusan létrehozza és tölti fel az előre definiált az Azure AD-csoportok tagságát.</span><span class="sxs-lookup"><span data-stu-id="60911-104">In Azure Active Directory (Azure AD), the dedicated groups feature automatically creates and populates membership for Azure AD predefined groups.</span></span> <span data-ttu-id="60911-105">Dedikált csoportok tagjai nem adható hozzá, vagy a klasszikus Azure portálon, a Windows PowerShell-parancsmagok segítségével távolítja el vagy programon keresztül.</span><span class="sxs-lookup"><span data-stu-id="60911-105">Members of dedicated groups cannot be added or removed using the Azure classic portal, Windows PowerShell cmdlets, or programmatically.</span></span>

> [!NOTE]
> <span data-ttu-id="60911-106">Dedikált csoportok szükséges, hogy a van rendelve egy Azure AD Premium-licenc</span><span class="sxs-lookup"><span data-stu-id="60911-106">Dedicated groups require that an Azure AD Premium license is assigned to</span></span>
>
> * <span data-ttu-id="60911-107">A szabály a csoporton felügyelő rendszergazda</span><span class="sxs-lookup"><span data-stu-id="60911-107">the administrator who manages the rule on a group</span></span>
> * <span data-ttu-id="60911-108">a felhasználók, akik ki van jelölve a szabály a csoport tagjai</span><span class="sxs-lookup"><span data-stu-id="60911-108">all users who are selected by the rule to be a member of the group</span></span>
>
>

<span data-ttu-id="60911-109">**Dedikált csoportok engedélyezése**</span><span class="sxs-lookup"><span data-stu-id="60911-109">**To enable dedicated groups**</span></span>

1. <span data-ttu-id="60911-110">Az a [a klasszikus Azure portálon](https://manage.windowsazure.com), jelölje be **Active Directory**, majd nyissa meg a munkahely címtárában.</span><span class="sxs-lookup"><span data-stu-id="60911-110">In the [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then open your organization’s directory.</span></span>
2. <span data-ttu-id="60911-111">Válassza ki a **csoportok** lapra, és nyissa meg a szerkeszteni kívánt csoportot.</span><span class="sxs-lookup"><span data-stu-id="60911-111">Select the **Groups** tab, and then open the group you want to edit.</span></span>
3. <span data-ttu-id="60911-112">Válassza ki a **konfigurálása** lapot, és utána állítsa be **dedikált csoportok engedélyezése** való **Igen**.</span><span class="sxs-lookup"><span data-stu-id="60911-112">Select the **Configure** tab, and then set **Enable Dedicated Groups** to **Yes**.</span></span>

<span data-ttu-id="60911-113">Amennyiben a dedikált csoportok engedélyezése kapcsoló értéke **Igen**, további engedélyezheti a könyvtárat úgy, hogy az összes felhasználó dedikált csoport automatikus létrehozásához a **az "Összes felhasználó" csoport** váltani **Igen**.</span><span class="sxs-lookup"><span data-stu-id="60911-113">Once the Enable Dedicated Groups switch is set to **Yes**, you can further enable the directory to automatically create the All Users dedicated group by setting the **Enable “All Users” Group** switch to **Yes**.</span></span> <span data-ttu-id="60911-114">Is szerkesztheti a kijelölt csoport nevét írja be azt a **"Minden felhasználó" megjelenített név csoport** mező.</span><span class="sxs-lookup"><span data-stu-id="60911-114">You can then also edit the name of this dedicated group by typing it in the **Display Name for “All Users” Group** field.</span></span>

<span data-ttu-id="60911-115">A minden felhasználó csoportban a azonos engedélyek hozzárendelése a címtárban szereplő összes felhasználó használható.</span><span class="sxs-lookup"><span data-stu-id="60911-115">The All Users group can be used to assign the same permissions to all the users in your directory.</span></span> <span data-ttu-id="60911-116">Például biztosíthat minden felhasználó a könyvtár hozzáférés SaaS-alkalmazás az összes felhasználó dedikált csoport hozzáférést rendel az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="60911-116">For example, you can grant all users in your directory access to a SaaS application by assigning access for the All Users dedicated group to this application.</span></span>

<span data-ttu-id="60911-117">A dedikált minden felhasználó csoportban a címtárban, beleértve a vendégek és a külső felhasználók felhasználókat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="60911-117">The dedicated All Users group includes all users in the directory, including guests and external users.</span></span> <span data-ttu-id="60911-118">Ha egy csoportot kell, amely nem tartalmazza a külső felhasználók számára, akkor ez által csoportot hoz létre egy attribútum-alapú dinamikus szabály például a következő érhető el:</span><span class="sxs-lookup"><span data-stu-id="60911-118">If you need a group that excludes external users, then you can accomplish this by creating a group with an attribute-based dynamic rule such as the following:</span></span>

                (user.userPrincipalName -notContains "#EXT#@")

<span data-ttu-id="60911-119">Egy csoport, amely nem tartalmazza az összes vendégek például a következő szabályt használni:</span><span class="sxs-lookup"><span data-stu-id="60911-119">For a group that excludes all Guests, use a rule such as the following:</span></span>

                (user.userType -ne "Guest")

<span data-ttu-id="60911-120">Információk a dinamikus csoporttagsághoz kapcsolódó *speciális* (akár többszörös összehasonlítást is tartalmazó) szabályok létrehozásáról: [Using attributes to create advanced rules](active-directory-accessmanagement-groups-with-advanced-rules.md) (Attribútumok használata speciális szabályok létrehozásához).</span><span class="sxs-lookup"><span data-stu-id="60911-120">To learn about how to create *advanced* rules (rules that can contain multiple comparisons) for dynamic group membership, see [Using attributes to create advanced rules](active-directory-accessmanagement-groups-with-advanced-rules.md).</span></span>

### <a name="next-steps"></a><span data-ttu-id="60911-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="60911-121">Next steps</span></span>
<span data-ttu-id="60911-122">E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="60911-122">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="60911-123">Erőforráshozzáférés-kezelés Azure Active Directory-csoportokkal</span><span class="sxs-lookup"><span data-stu-id="60911-123">Managing access to resources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="60911-124">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="60911-124">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="60911-125">Mi az az Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="60911-125">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="60911-126">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="60911-126">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
