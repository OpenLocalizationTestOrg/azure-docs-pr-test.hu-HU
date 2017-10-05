---
title: "További lépések a csoportok használatával access Management szolgáltatáshoz |} Microsoft Docs"
description: "Hogyan speciális-a következőre vonatkozó biztonsági csoportok és erőforrásokhoz való hozzáférés kezelése ezek a csoportok használatával történő kezelése."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 44350a3c-8ea1-4da1-aaac-7fc53933dd21
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 82fbeb379e90add09f7c569111053f6e9b1bc9c5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="managing-owners-for-a-group"></a><span data-ttu-id="355c6-103">Egy csoport tulajdonosainak kezelése</span><span class="sxs-lookup"><span data-stu-id="355c6-103">Managing owners for a group</span></span>
<span data-ttu-id="355c6-104">Amennyiben az erőforrás tulajdonosa hozzá van rendelve hozzáférést egy erőforráshoz egy Azure Active Directory-csoportba, a csoport tagsága kezeli a csoport tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="355c6-104">Once a resource owner has assigned access to a resource to an Azure AD group, the membership of the group is managed by the group owner.</span></span> <span data-ttu-id="355c6-105">Az erőforrás tulajdonosa hatékonyan felhasználók hozzárendelése az erőforrást a csoport tulajdonosa engedélyt ad.</span><span class="sxs-lookup"><span data-stu-id="355c6-105">The resource owner effectively delegates the permission to assign users to the resource to the owner of the group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="355c6-106">A Microsoft javasolja, hogy az Azure Portalon található [Azure AD felügyeleti központból](https://aad.portal.azure.com) kezelje az Azure AD-t az ebben a cikkben javasolt klasszikus Azure portál helyett.</span><span class="sxs-lookup"><span data-stu-id="355c6-106">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span> 

## <a name="assigning-group-ownership"></a><span data-ttu-id="355c6-107">Csoport tulajdonjogának hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="355c6-107">Assigning group ownership</span></span>
<span data-ttu-id="355c6-108">**Egy olyan tulajdonost hozzáadása egy csoporthoz**</span><span class="sxs-lookup"><span data-stu-id="355c6-108">**To add an owner to a group**</span></span>

1. <span data-ttu-id="355c6-109">Az a [a klasszikus Azure portálon](https://manage.windowsazure.com), jelölje be **Active Directory**, majd nyissa meg a munkahely címtárában.</span><span class="sxs-lookup"><span data-stu-id="355c6-109">In the [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then open your organization’s directory.</span></span>
2. <span data-ttu-id="355c6-110">Válassza ki a **csoportok** lapra, és nyissa meg a csoportot, amelyhez hozzá szeretné adni a tulajdonosai számára.</span><span class="sxs-lookup"><span data-stu-id="355c6-110">Select the **Groups** tab, and then open the group that you want to add owners to.</span></span>
3. <span data-ttu-id="355c6-111">Válassza ki **adja hozzá a tulajdonosok**.</span><span class="sxs-lookup"><span data-stu-id="355c6-111">Select **Add Owners**.</span></span>
4. <span data-ttu-id="355c6-112">Az a **adja hozzá a tulajdonosok** lapon, válassza ki a felhasználót, hogy ez a csoport tulajdonosa hozzá, és győződjön meg arról, hogy ez a név a **kijelölt** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="355c6-112">On the **Add owners** page, select the user that you want to add as the owner of this group, and make sure this name is added to the **Selected** pane.</span></span>

<span data-ttu-id="355c6-113">**Egy olyan tulajdonost eltávolítása egy csoportból**</span><span class="sxs-lookup"><span data-stu-id="355c6-113">**To remove an owner from a group**</span></span>

1. <span data-ttu-id="355c6-114">Az a [a klasszikus Azure portálon](https://manage.windowsazure.com), jelölje be **Active Directory**, majd nyissa meg a munkahely címtárában.</span><span class="sxs-lookup"><span data-stu-id="355c6-114">In the [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then open your organization’s directory.</span></span>
2. <span data-ttu-id="355c6-115">Válassza ki a **csoportok** lapot, és nyissa meg a csoportot, amely a el kívánja távolítani a tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="355c6-115">Select the **Groups** tab, and then open the group that you want to remove an owner from.</span></span>
3. <span data-ttu-id="355c6-116">Válassza ki a **tulajdonosok** fülre.</span><span class="sxs-lookup"><span data-stu-id="355c6-116">Select the **Owners** tab.</span></span>
4. <span data-ttu-id="355c6-117">Válassza ki a csoportból eltávolítani, majd válassza ki a kívánt tulajdonosát **eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="355c6-117">Select the owner that you want to remove from this group, and then select **Remove**.</span></span>

## <a name="additional-information"></a><span data-ttu-id="355c6-118">További információ</span><span class="sxs-lookup"><span data-stu-id="355c6-118">Additional information</span></span>
<span data-ttu-id="355c6-119">E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="355c6-119">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="355c6-120">Erőforráshozzáférés-kezelés Azure Active Directory-csoportokkal</span><span class="sxs-lookup"><span data-stu-id="355c6-120">Managing access to resources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="355c6-121">Azure Active Directory-parancsmagok csoportbeállítások konfigurálásához</span><span class="sxs-lookup"><span data-stu-id="355c6-121">Azure Active Directory cmdlets for configuring group settings</span></span>](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [<span data-ttu-id="355c6-122">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="355c6-122">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="355c6-123">Mi az az Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="355c6-123">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="355c6-124">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="355c6-124">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
