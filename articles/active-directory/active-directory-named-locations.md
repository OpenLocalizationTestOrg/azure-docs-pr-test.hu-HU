---
title: az Azure Active Directoryban aaaNamed helyek |} Microsoft Docs
description: "Úgy konfigurálja a helyek nevű, elkerülheti a rendelkező IP hello lehetetlen odautazás tooatypical helyek esemény kockázattípus létrehozni a szervezete tulajdonában lévő címek vakriasztások tömkelegére."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 591e4b94b2ec9d45e20c01711e922f9972e047e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="named-locations-in-azure-active-directory"></a><span data-ttu-id="20512-103">Az Azure Active Directoryban elnevezett helyek</span><span class="sxs-lookup"><span data-stu-id="20512-103">Named locations in Azure Active Directory</span></span>

<span data-ttu-id="20512-104">A hello nevű helyek szolgáltatás az Azure Active Directory a címkézés megbízható IP-címtartományok a szervezetben.</span><span class="sxs-lookup"><span data-stu-id="20512-104">With hello named locations feature of Azure Active Directory, you can label trusted IP address ranges in your organizations.</span></span> <span data-ttu-id="20512-105">A környezetben, használhatja a hello észlelése hello környezetben elnevezett helyét [kockázati események](active-directory-reporting-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="20512-105">In your environment, you can use named locations in hello context of hello detection of [risk events](active-directory-reporting-risk-events.md).</span></span> <span data-ttu-id="20512-106">hello funkció csökkenti a hello jelentett téves hello számát *lehetetlen odautazás tooatypical helyek* eseménytípus kockázatát.</span><span class="sxs-lookup"><span data-stu-id="20512-106">hello feature helps reduce hello number of reported false positives for hello *Impossible travel tooatypical locations* risk event type.</span></span> 

## <a name="configuration"></a><span data-ttu-id="20512-107">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="20512-107">Configuration</span></span>

<span data-ttu-id="20512-108">tooconfigure elnevezett helye:</span><span class="sxs-lookup"><span data-stu-id="20512-108">tooconfigure a named location:</span></span>

1. <span data-ttu-id="20512-109">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) globális rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="20512-109">Sign in toohello [Azure portal](https://portal.azure.com) as global administrator.</span></span>

2. <span data-ttu-id="20512-110">Hello bal oldali ablaktáblában kattintson **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="20512-110">In hello left pane, click **Azure Active Directory**.</span></span>

    ![hello bal oldali ablaktáblán hello Azure Active Directory-hivatkozás](./media/active-directory-named-locations/01.png)

3. <span data-ttu-id="20512-112">A hello **Azure Active Directory** paneljén, hello **biztonsági** kattintson **feltételes hozzáférés**.</span><span class="sxs-lookup"><span data-stu-id="20512-112">On hello **Azure Active Directory** blade, in hello **Security** section, click **Conditional access**.</span></span>

    ![Feltételes hozzáférés parancs hello](./media/active-directory-named-locations/05.png)


4. <span data-ttu-id="20512-114">A hello **feltételes hozzáférés** paneljén, hello **kezelése** területén kattintson **helyek nevű**.</span><span class="sxs-lookup"><span data-stu-id="20512-114">On hello **Conditional Access** blade, in hello **Manage** section, click **Named locations**.</span></span>

    ![hello nevesített helyek parancs](./media/active-directory-named-locations/06.png)


5. <span data-ttu-id="20512-116">A hello **helyek nevű** panelen kattintson a **új helyre**.</span><span class="sxs-lookup"><span data-stu-id="20512-116">On hello **Named locations** blade, click **New location**.</span></span>

    ![Új hely parancs hello](./media/active-directory-named-locations/07.png)


6. <span data-ttu-id="20512-118">A hello **új** panelen a következő hello:</span><span class="sxs-lookup"><span data-stu-id="20512-118">On hello **New** blade, do hello following:</span></span>

    ![hello új panel](./media/active-directory-named-locations/08.png)

    <span data-ttu-id="20512-120">a.</span><span class="sxs-lookup"><span data-stu-id="20512-120">a.</span></span> <span data-ttu-id="20512-121">A hello **neve** mezőbe írja be a nevesített hely nevét.</span><span class="sxs-lookup"><span data-stu-id="20512-121">In hello **Name** box, type a name for your named location.</span></span>

    <span data-ttu-id="20512-122">b.</span><span class="sxs-lookup"><span data-stu-id="20512-122">b.</span></span> <span data-ttu-id="20512-123">A hello **IP-címtartományok** mezőbe írjon be egy IP-címtartományt.</span><span class="sxs-lookup"><span data-stu-id="20512-123">In hello **IP ranges** box, type an IP range.</span></span> <span data-ttu-id="20512-124">hello IP-címtartomány van szüksége a hello toobe *Classless Inter-Domain Routing* (CIDR) formátumban.</span><span class="sxs-lookup"><span data-stu-id="20512-124">hello IP range needs toobe in hello *Classless Inter-Domain Routing* (CIDR) format.</span></span>  

    <span data-ttu-id="20512-125">c.</span><span class="sxs-lookup"><span data-stu-id="20512-125">c.</span></span> <span data-ttu-id="20512-126">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="20512-126">Click **Create**.</span></span>



## <a name="what-you-should-know"></a><span data-ttu-id="20512-127">Tudnivalók</span><span class="sxs-lookup"><span data-stu-id="20512-127">What you should know</span></span>

<span data-ttu-id="20512-128">**Tömeges frissítés**: elnevezett helyek tömeges frissíti, frissítése vagy létrehozásakor feltöltheti, vagy az IP-címtartományok hello CSV-fájl letöltése.</span><span class="sxs-lookup"><span data-stu-id="20512-128">**Bulk updates**: When you create or update named locations, for bulk updates, you can upload or download a CSV file with hello IP ranges.</span></span> <span data-ttu-id="20512-129">Feltöltés hello IP-címtartományok helyez toohello fájllista hello helyett hello lista felülírja.</span><span class="sxs-lookup"><span data-stu-id="20512-129">An upload adds hello IP ranges in hello file toohello list instead of overwriting hello list.</span></span>

![hello hivatkozások le- és feltöltése](./media/active-directory-named-locations/09.png)


<span data-ttu-id="20512-131">**Korlátozások**: 60 elnevezett helyek, legfeljebb egy IP tartomány hozzárendelt tooeach azok adhat meg.</span><span class="sxs-lookup"><span data-stu-id="20512-131">**Limitations**: You can define a maximum of 60 named locations, with one IP range assigned tooeach of them.</span></span> <span data-ttu-id="20512-132">Ha csak egy elnevezett helyen konfigurálva van, adhat meg too500 IP-címtartományok fel azt.</span><span class="sxs-lookup"><span data-stu-id="20512-132">If you have just one named location configured, you can define up too500 IP ranges for it.</span></span>


## <a name="next-steps"></a><span data-ttu-id="20512-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="20512-133">Next steps</span></span>

<span data-ttu-id="20512-134">További információ a kockázati eseményekről, toolearn lásd [Azure Active Directory kockázati események](active-directory-reporting-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="20512-134">toolearn more about risk events, see [Azure Active Directory risk events](active-directory-reporting-risk-events.md).</span></span>

