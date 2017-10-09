---
title: "csoportok használata kezelési lépések aaaNext |} Microsoft Docs"
description: "Hogyan speciális-a következőre vonatkozó biztonsági csoportok kezelése és hogyan toouse ezen csoportok toomanage hozzáférés tooa erőforrás."
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
ms.openlocfilehash: 4fd55f893290fac3551a130f29bd12709cf551e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-owners-for-a-group"></a><span data-ttu-id="48ac3-103">Egy csoport tulajdonosainak kezelése</span><span class="sxs-lookup"><span data-stu-id="48ac3-103">Managing owners for a group</span></span>
<span data-ttu-id="48ac3-104">Amennyiben az erőforrás tulajdonosa hozzá van rendelve az access tooa erőforrás tooan az Azure AD-csoport, hello csoport tagságának hello hello csoport tulajdonosa végzi.</span><span class="sxs-lookup"><span data-stu-id="48ac3-104">Once a resource owner has assigned access tooa resource tooan Azure AD group, hello membership of hello group is managed by hello group owner.</span></span> <span data-ttu-id="48ac3-105">hello erőforrás tulajdonosa hatékonyan ad hello engedély tooassign felhasználók toohello erőforrás toohello hello csoport tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="48ac3-105">hello resource owner effectively delegates hello permission tooassign users toohello resource toohello owner of hello group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="48ac3-106">A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="48ac3-106">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span> 

## <a name="assigning-group-ownership"></a><span data-ttu-id="48ac3-107">Csoport tulajdonjogának hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="48ac3-107">Assigning group ownership</span></span>
<span data-ttu-id="48ac3-108">**tooadd egy tulajdonos tooa csoport**</span><span class="sxs-lookup"><span data-stu-id="48ac3-108">**tooadd an owner tooa group**</span></span>

1. <span data-ttu-id="48ac3-109">A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), jelölje be **Active Directory**, majd nyissa meg a munkahely címtárában.</span><span class="sxs-lookup"><span data-stu-id="48ac3-109">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then open your organization’s directory.</span></span>
2. <span data-ttu-id="48ac3-110">Jelölje be hello **csoportok** fülre, és ezután nyissa meg hello csoport tooadd tulajdonosai számára.</span><span class="sxs-lookup"><span data-stu-id="48ac3-110">Select hello **Groups** tab, and then open hello group that you want tooadd owners to.</span></span>
3. <span data-ttu-id="48ac3-111">Válassza ki **adja hozzá a tulajdonosok**.</span><span class="sxs-lookup"><span data-stu-id="48ac3-111">Select **Add Owners**.</span></span>
4. <span data-ttu-id="48ac3-112">A hello **adja hozzá a tulajdonosok** lapra, válassza hello felhasználói tooadd szeretné, hogy ez a csoport hello tulajdonosaként, és győződjön meg arról, hogy ez a név jelenik meg toohello **kijelölt** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="48ac3-112">On hello **Add owners** page, select hello user that you want tooadd as hello owner of this group, and make sure this name is added toohello **Selected** pane.</span></span>

<span data-ttu-id="48ac3-113">**a csoport tulajdonosa tooremove**</span><span class="sxs-lookup"><span data-stu-id="48ac3-113">**tooremove an owner from a group**</span></span>

1. <span data-ttu-id="48ac3-114">A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), jelölje be **Active Directory**, majd nyissa meg a munkahely címtárában.</span><span class="sxs-lookup"><span data-stu-id="48ac3-114">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then open your organization’s directory.</span></span>
2. <span data-ttu-id="48ac3-115">Jelölje be hello **csoportok** fülre, és ezután nyissa meg hello csoport, amelyet tooremove tulajdonos.</span><span class="sxs-lookup"><span data-stu-id="48ac3-115">Select hello **Groups** tab, and then open hello group that you want tooremove an owner from.</span></span>
3. <span data-ttu-id="48ac3-116">Jelölje be hello **tulajdonosok** fülre.</span><span class="sxs-lookup"><span data-stu-id="48ac3-116">Select hello **Owners** tab.</span></span>
4. <span data-ttu-id="48ac3-117">Jelölje be hello tulajdonos, hogy szeretné, hogy tooremove ebből a csoportból, és válassza **eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="48ac3-117">Select hello owner that you want tooremove from this group, and then select **Remove**.</span></span>

## <a name="additional-information"></a><span data-ttu-id="48ac3-118">További információ</span><span class="sxs-lookup"><span data-stu-id="48ac3-118">Additional information</span></span>
<span data-ttu-id="48ac3-119">E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="48ac3-119">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="48ac3-120">Hozzáférés tooresources kezelése az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="48ac3-120">Managing access tooresources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="48ac3-121">Azure Active Directory-parancsmagok csoportbeállítások konfigurálásához</span><span class="sxs-lookup"><span data-stu-id="48ac3-121">Azure Active Directory cmdlets for configuring group settings</span></span>](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [<span data-ttu-id="48ac3-122">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="48ac3-122">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="48ac3-123">Mi az az Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="48ac3-123">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="48ac3-124">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="48ac3-124">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
