---
title: "Azure Active Directory eszközalapú feltételes hozzáférési szabályzatok konfigurálására |} Microsoft Docs"
description: "Útmutató: Azure Active Directory eszközalapú feltételes hozzáférési házirendek konfigurálása."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a27862a6-d513-43ba-97c1-1c0d400bf243
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: a26c40351c6b982fd90acb4bf06220ef3f79f399
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="configure-azure-active-directory-device-based-conditional-access-policies"></a><span data-ttu-id="25560-103">Azure Active Directory eszközalapú feltételes hozzáférési házirendek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="25560-103">Configure Azure Active Directory device-based conditional access policies</span></span>

<span data-ttu-id="25560-104">A [Azure Active Directory (Azure AD) feltételes hozzáférés](active-directory-conditional-access-azure-portal.md), úgy finomhangolhatja, hogyan engedéllyel rendelkező felhasználók férhetnek hozzá az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="25560-104">With [Azure Active Directory (Azure AD) conditional access](active-directory-conditional-access-azure-portal.md), you can fine-tune how authorized users can access your resources.</span></span> <span data-ttu-id="25560-105">Például korlátozhatja a hozzáférést bizonyos erőforrások megbízható eszközökre.</span><span class="sxs-lookup"><span data-stu-id="25560-105">For example, you limit the access to certain resources to trusted devices.</span></span> <span data-ttu-id="25560-106">Egy feltételes hozzáférési szabályzatot, amely egy megbízható eszközt eszközalapú feltételes hozzáférési szabályzat is nevezik.</span><span class="sxs-lookup"><span data-stu-id="25560-106">A conditional access policy that requires a trusted device is also known as device-based conditional access policy.</span></span>

<span data-ttu-id="25560-107">Ez a témakör az Azure AD-kompatibilis alkalmazásokat az eszközalapú feltételes hozzáférési házirendek konfigurálásával kapcsolatos információkat nyújt.</span><span class="sxs-lookup"><span data-stu-id="25560-107">This topic provides you with information on how to configure device-based conditional access policies for Azure AD-connected applications.</span></span> 


## <a name="before-you-begin"></a><span data-ttu-id="25560-108">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="25560-108">Before you begin</span></span>

<span data-ttu-id="25560-109">Eszközalapú feltételes hozzáférési ties **Azure AD feltételes hozzáférésével** és **együtt az Azure AD Eszközkezelés**.</span><span class="sxs-lookup"><span data-stu-id="25560-109">Device-based conditional access ties **Azure AD conditional access** and **Azure AD device management together**.</span></span> <span data-ttu-id="25560-110">Ha még nem ismeri a következő területeken, olvassa el a következő témakörök először:</span><span class="sxs-lookup"><span data-stu-id="25560-110">If you are not familiar with one of these areas yet, you should read the following topics, first:</span></span>

- <span data-ttu-id="25560-111">**[Feltételes hozzáférés az Azure Active Directoryban](active-directory-conditional-access-azure-portal.md)**  – Ez a témakör a feltételes hozzáférés és a kapcsolódó terminológia elméleti áttekintését.</span><span class="sxs-lookup"><span data-stu-id="25560-111">**[Conditional access in Azure Active Directory](active-directory-conditional-access-azure-portal.md)** - This topic provides you with a conceptual overview of conditional access and the related terminology.</span></span>

- <span data-ttu-id="25560-112">**[Bevezetés az Azure Active Directoryban eszközfelügyeletre](device-management-introduction.md)**  – Ez a témakör áttekintést nyújt az eszközök csatlakoztatása az Azure ad-vel, hogy különböző lehetőségek közül.</span><span class="sxs-lookup"><span data-stu-id="25560-112">**[Introduction to device management in Azure Active Directory](device-management-introduction.md)** - This topic gives you an overview of the various options you have to connect devices with Azure AD.</span></span> 


## <a name="trusted-devices"></a><span data-ttu-id="25560-113">Megbízható eszközök</span><span class="sxs-lookup"><span data-stu-id="25560-113">Trusted devices</span></span>

<span data-ttu-id="25560-114">Mobileszköz-first, a felhő-első világában Azure Active Directory lehetővé teszi, hogy az egyszeri bejelentkezés eszközök, alkalmazások és szolgáltatások bárhonnan.</span><span class="sxs-lookup"><span data-stu-id="25560-114">In a mobile-first, cloud-first world, Azure Active Directory enables single sign-on to devices, apps, and services from anywhere.</span></span> <span data-ttu-id="25560-115">Az egyes erőforrásoknak a környezetben, és hozzáférést biztosít az arra jogosult felhasználók előfordulhat elég helyes.</span><span class="sxs-lookup"><span data-stu-id="25560-115">For certain resources in your environment, granting access to the right users might not be good enough.</span></span> <span data-ttu-id="25560-116">Az arra jogosult felhasználók mellett is szüksége lehet a megbízható eszközök erőforrások eléréséhez használt.</span><span class="sxs-lookup"><span data-stu-id="25560-116">In addition to the right users, you might also require a trusted device to be used to access a resource.</span></span> <span data-ttu-id="25560-117">A környezetben, megadhatja, mi megbízható eszköz alapján a következő összetevőket:</span><span class="sxs-lookup"><span data-stu-id="25560-117">In your environment, you can define what a trusted device is based on the following components:</span></span>

- <span data-ttu-id="25560-118">A [eszközplatformok](active-directory-conditional-access-azure-portal.md#device-platforms) az eszközön</span><span class="sxs-lookup"><span data-stu-id="25560-118">The [device platforms](active-directory-conditional-access-azure-portal.md#device-platforms) on a device</span></span>
- <span data-ttu-id="25560-119">Egy eszköz-e megfelelő</span><span class="sxs-lookup"><span data-stu-id="25560-119">Whether a device is compliant</span></span>
- <span data-ttu-id="25560-120">Egy eszköz-e a tartományhoz</span><span class="sxs-lookup"><span data-stu-id="25560-120">Whether a device is domain-joined</span></span> 

<span data-ttu-id="25560-121">A [eszközplatformok](active-directory-conditional-access-azure-portal.md#device-platforms) jellemzőek, az eszközön futó operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="25560-121">The [device platforms](active-directory-conditional-access-azure-portal.md#device-platforms) is characterized by the operating system that is running on your device.</span></span> <span data-ttu-id="25560-122">Az eszközalapú feltételes hozzáférési házirendben korlátozhatja az egyes erőforrásokhoz adott eszközplatformhoz való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="25560-122">In your device-based conditional access policy, you can limit access to certain resources to specific device platforms.</span></span>



<span data-ttu-id="25560-123">Eszközalapú feltételes hozzáférési szabályzatot, a megfelelőnek kell megjelölni megbízható eszközökkel lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="25560-123">In a device-based conditional access policy, you can require trusted devices to be marked as compliant.</span></span>

![Felhőalkalmazások](./media/active-directory-conditional-access-policy-connected-applications/24.png)

<span data-ttu-id="25560-125">A címtár által megfelelőnek eszközök jelölhető ki:</span><span class="sxs-lookup"><span data-stu-id="25560-125">Devices can be marked as compliant in the directory by:</span></span>

- <span data-ttu-id="25560-126">Intune-ban</span><span class="sxs-lookup"><span data-stu-id="25560-126">Intune</span></span> 
- <span data-ttu-id="25560-127">Egy harmadik fél mobileszköz felügyeleti rendszer, amely az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="25560-127">A third-party mobile device management system that integrates with Azure AD</span></span>  

<span data-ttu-id="25560-128">Csak az Azure AD szolgáltatáshoz kapcsolódó eszközök megfelelőnek jelölhető.</span><span class="sxs-lookup"><span data-stu-id="25560-128">Only devices that are connected to Azure AD can be marked as compliant.</span></span> <span data-ttu-id="25560-129">Egy eszköz csatlakozni az Azure Active Directory, az alábbi lehetőségei vannak:</span><span class="sxs-lookup"><span data-stu-id="25560-129">To connect a device to Azure Active Directory, you have the following options:</span></span> 

- <span data-ttu-id="25560-130">Regisztrálva az Azure AD</span><span class="sxs-lookup"><span data-stu-id="25560-130">Azure AD registered</span></span>
- <span data-ttu-id="25560-131">Az Azure AD-tartományhoz</span><span class="sxs-lookup"><span data-stu-id="25560-131">Azure AD joined</span></span>
- <span data-ttu-id="25560-132">Az Azure AD hibrid csatlakoztatva</span><span class="sxs-lookup"><span data-stu-id="25560-132">Hybrid Azure AD joined</span></span>

    ![Felhőalkalmazások](./media/active-directory-conditional-access-policy-connected-applications/26.png)

<span data-ttu-id="25560-134">Ha egy a helyszíni Active Directory (AD) kezdjen, érdemes lehet eszközökön nem kapcsolódik az Azure AD, de a megbízható AD csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="25560-134">If you have an on-premises Active Directory (AD) footprint, you might consider devices that are not connected to Azure AD but joined to your AD to be trusted.</span></span>

![Felhőalkalmazások](./media/active-directory-conditional-access-policy-connected-applications/25.png)


## <a name="next-steps"></a><span data-ttu-id="25560-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="25560-136">Next steps</span></span>

<span data-ttu-id="25560-137">Eszközalapú feltételes hozzáférési házirend konfigurálása a környezetben, előtt meg kell vessen egy pillantást a [ajánlott eljárások a feltételes hozzáférés az Azure Active Directoryban](active-directory-conditional-access-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="25560-137">Before configuring a device-based conditional access policy in your environment, you should take a look at the [best practices for conditional access in Azure Active Directory](active-directory-conditional-access-best-practices.md).</span></span>

