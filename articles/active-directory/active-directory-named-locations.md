---
title: "Az Azure Active Directory helyek nevű |} Microsoft Docs"
description: "Úgy konfigurálja a helyek nevű, elkerülheti a rendelkezik a szervezete tulajdonában lévő IP-címeket létrehozni a Impossible a vakriasztások utazik és bejelentkezés szokatlan helyekről kockázat eseménytípus."
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
ms.openlocfilehash: ff31ded1d9d60e47e0ae5f01119de78cd7f2df38
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="named-locations-in-azure-active-directory"></a><span data-ttu-id="71645-103">Az Azure Active Directoryban elnevezett helyek</span><span class="sxs-lookup"><span data-stu-id="71645-103">Named locations in Azure Active Directory</span></span>

<span data-ttu-id="71645-104">Az elnevezett helyek szolgáltatás az Azure Active Directory a címkézés megbízható IP-címtartományok a szervezetben.</span><span class="sxs-lookup"><span data-stu-id="71645-104">With the named locations feature of Azure Active Directory, you can label trusted IP address ranges in your organizations.</span></span> <span data-ttu-id="71645-105">A környezetben, használhatja a észlelését, az adott környezetben elnevezett helyét [kockázati események](active-directory-reporting-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="71645-105">In your environment, you can use named locations in the context of the detection of [risk events](active-directory-reporting-risk-events.md).</span></span> <span data-ttu-id="71645-106">A szolgáltatás segítségével a jelentett vakriasztások számának csökkentése a *Impossible utazik bejelentkezés szokatlan helyekről* eseménytípus kockázatát.</span><span class="sxs-lookup"><span data-stu-id="71645-106">The feature helps reduce the number of reported false positives for the *Impossible travel to atypical locations* risk event type.</span></span> 

## <a name="configuration"></a><span data-ttu-id="71645-107">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="71645-107">Configuration</span></span>

<span data-ttu-id="71645-108">Egy elnevezett hely konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="71645-108">To configure a named location:</span></span>

1. <span data-ttu-id="71645-109">Jelentkezzen be a [Azure-portálon](https://portal.azure.com) globális rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="71645-109">Sign in to the [Azure portal](https://portal.azure.com) as global administrator.</span></span>

2. <span data-ttu-id="71645-110">Kattintson a bal oldali ablaktáblában **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="71645-110">In the left pane, click **Azure Active Directory**.</span></span>

    ![A bal oldali panelen az Azure Active Directory hivatkozás](./media/active-directory-named-locations/01.png)

3. <span data-ttu-id="71645-112">Az a **Azure Active Directory** panelen, a a **biztonsági** kattintson **feltételes hozzáférési**.</span><span class="sxs-lookup"><span data-stu-id="71645-112">On the **Azure Active Directory** blade, in the **Security** section, click **Conditional access**.</span></span>

    ![A feltételes hozzáférés parancs](./media/active-directory-named-locations/05.png)


4. <span data-ttu-id="71645-114">A a **feltételes hozzáférés** panelen, a a **kezelése** kattintson **helyek nevű**.</span><span class="sxs-lookup"><span data-stu-id="71645-114">On the **Conditional Access** blade, in the **Manage** section, click **Named locations**.</span></span>

    ![A nevesített helyek parancs](./media/active-directory-named-locations/06.png)


5. <span data-ttu-id="71645-116">Az a **helyek nevű** panelen kattintson a **új helyre**.</span><span class="sxs-lookup"><span data-stu-id="71645-116">On the **Named locations** blade, click **New location**.</span></span>

    ![Az új hely parancs](./media/active-directory-named-locations/07.png)


6. <span data-ttu-id="71645-118">Az a **új** panelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="71645-118">On the **New** blade, do the following:</span></span>

    ![Az új panel](./media/active-directory-named-locations/08.png)

    <span data-ttu-id="71645-120">a.</span><span class="sxs-lookup"><span data-stu-id="71645-120">a.</span></span> <span data-ttu-id="71645-121">Az a **neve** mezőbe írja be a nevesített hely nevét.</span><span class="sxs-lookup"><span data-stu-id="71645-121">In the **Name** box, type a name for your named location.</span></span>

    <span data-ttu-id="71645-122">b.</span><span class="sxs-lookup"><span data-stu-id="71645-122">b.</span></span> <span data-ttu-id="71645-123">Az a **IP-címtartományok** mezőbe írjon be egy IP-címtartományt.</span><span class="sxs-lookup"><span data-stu-id="71645-123">In the **IP ranges** box, type an IP range.</span></span> <span data-ttu-id="71645-124">Az IP-címtartomány kell lennie a *Classless Inter-Domain Routing* (CIDR) formátumban.</span><span class="sxs-lookup"><span data-stu-id="71645-124">The IP range needs to be in the *Classless Inter-Domain Routing* (CIDR) format.</span></span>  

    <span data-ttu-id="71645-125">c.</span><span class="sxs-lookup"><span data-stu-id="71645-125">c.</span></span> <span data-ttu-id="71645-126">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="71645-126">Click **Create**.</span></span>



## <a name="what-you-should-know"></a><span data-ttu-id="71645-127">Tudnivalók</span><span class="sxs-lookup"><span data-stu-id="71645-127">What you should know</span></span>

<span data-ttu-id="71645-128">**Tömeges frissítés**: elnevezett helyek tömeges frissíti, frissítése vagy létrehozásakor feltölteni, vagy az IP-címtartományai a CSV-fájl letöltése.</span><span class="sxs-lookup"><span data-stu-id="71645-128">**Bulk updates**: When you create or update named locations, for bulk updates, you can upload or download a CSV file with the IP ranges.</span></span> <span data-ttu-id="71645-129">Feltöltés a fájlban az IP-címtartományok hozzáadása a listához, a lista felülírja helyett.</span><span class="sxs-lookup"><span data-stu-id="71645-129">An upload adds the IP ranges in the file to the list instead of overwriting the list.</span></span>

![A feltöltés és letöltés hivatkozások](./media/active-directory-named-locations/09.png)


<span data-ttu-id="71645-131">**Korlátozások**: legfeljebb 60 elnevezett helyek, a hozzájuk rendelt egy IP-címtartomány adhat meg.</span><span class="sxs-lookup"><span data-stu-id="71645-131">**Limitations**: You can define a maximum of 60 named locations, with one IP range assigned to each of them.</span></span> <span data-ttu-id="71645-132">Ha csak egy elnevezett helyen konfigurálva van, akkor azt legfeljebb 500 IP-címtartományok adhat meg.</span><span class="sxs-lookup"><span data-stu-id="71645-132">If you have just one named location configured, you can define up to 500 IP ranges for it.</span></span>


## <a name="next-steps"></a><span data-ttu-id="71645-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="71645-133">Next steps</span></span>

<span data-ttu-id="71645-134">Kockázati események kapcsolatos további információkért lásd: [Azure Active Directory kockázati események](active-directory-reporting-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="71645-134">To learn more about risk events, see [Azure Active Directory risk events](active-directory-reporting-risk-events.md).</span></span>

