---
title: "Azure Active Directory-eszközalapú feltételes hozzáférési házirendek aaaConfigure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure Azure Active Directory eszközalapú feltételes hozzáférési házirendek."
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
ms.openlocfilehash: cc5923b8ccee4335442c17ef63b2ee6f098e104e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-active-directory-device-based-conditional-access-policies"></a><span data-ttu-id="4dd3c-103">Azure Active Directory eszközalapú feltételes hozzáférési házirendek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4dd3c-103">Configure Azure Active Directory device-based conditional access policies</span></span>

<span data-ttu-id="4dd3c-104">A [Azure Active Directory (Azure AD) feltételes hozzáférés](active-directory-conditional-access-azure-portal.md), úgy finomhangolhatja, hogyan engedéllyel rendelkező felhasználók férhetnek hozzá az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="4dd3c-104">With [Azure Active Directory (Azure AD) conditional access](active-directory-conditional-access-azure-portal.md), you can fine-tune how authorized users can access your resources.</span></span> <span data-ttu-id="4dd3c-105">Például hogy korlátozza hello hozzáférés toocertain erőforrások tootrusted eszközök.</span><span class="sxs-lookup"><span data-stu-id="4dd3c-105">For example, you limit hello access toocertain resources tootrusted devices.</span></span> <span data-ttu-id="4dd3c-106">Egy feltételes hozzáférési szabályzatot, amely egy megbízható eszközt eszközalapú feltételes hozzáférési szabályzat is nevezik.</span><span class="sxs-lookup"><span data-stu-id="4dd3c-106">A conditional access policy that requires a trusted device is also known as device-based conditional access policy.</span></span>

<span data-ttu-id="4dd3c-107">Ez a témakör nyújt tájékoztatást, hogyan tooconfigure eszközalapú feltételes hozzáférési házirendek az Azure AD-kompatibilis alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="4dd3c-107">This topic provides you with information on how tooconfigure device-based conditional access policies for Azure AD-connected applications.</span></span> 


## <a name="before-you-begin"></a><span data-ttu-id="4dd3c-108">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="4dd3c-108">Before you begin</span></span>

<span data-ttu-id="4dd3c-109">Eszközalapú feltételes hozzáférési ties **Azure AD feltételes hozzáférésével** és **együtt az Azure AD Eszközkezelés**.</span><span class="sxs-lookup"><span data-stu-id="4dd3c-109">Device-based conditional access ties **Azure AD conditional access** and **Azure AD device management together**.</span></span> <span data-ttu-id="4dd3c-110">Ha még nem ismeri a következő területeken, olvassa el a következő témakörök, először hello:</span><span class="sxs-lookup"><span data-stu-id="4dd3c-110">If you are not familiar with one of these areas yet, you should read hello following topics, first:</span></span>

- <span data-ttu-id="4dd3c-111">**[Feltételes hozzáférés az Azure Active Directoryban](active-directory-conditional-access-azure-portal.md)**  – Ez a témakör ismerteti a feltételes elméleti áttekintését való eléréséhez és kapcsolódó terminológia hello.</span><span class="sxs-lookup"><span data-stu-id="4dd3c-111">**[Conditional access in Azure Active Directory](active-directory-conditional-access-azure-portal.md)** - This topic provides you with a conceptual overview of conditional access and hello related terminology.</span></span>

- <span data-ttu-id="4dd3c-112">**[Bevezetés toodevice kezelése az Azure Active Directoryban](device-management-introduction.md)**  – Ez a témakör áttekintést nyújt a hello különböző beállítások tooconnect eszközzel rendelkezik az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="4dd3c-112">**[Introduction toodevice management in Azure Active Directory](device-management-introduction.md)** - This topic gives you an overview of hello various options you have tooconnect devices with Azure AD.</span></span> 


## <a name="trusted-devices"></a><span data-ttu-id="4dd3c-113">Megbízható eszközök</span><span class="sxs-lookup"><span data-stu-id="4dd3c-113">Trusted devices</span></span>

<span data-ttu-id="4dd3c-114">Mobileszköz-first, a felhő-első világában Azure Active Directory lehetővé teszi, hogy egyszeri bejelentkezés toodevices, alkalmazásokhoz és szolgáltatásokhoz bárhonnan.</span><span class="sxs-lookup"><span data-stu-id="4dd3c-114">In a mobile-first, cloud-first world, Azure Active Directory enables single sign-on toodevices, apps, and services from anywhere.</span></span> <span data-ttu-id="4dd3c-115">Az egyes erőforrásoknak a környezetben, az arra jogosult felhasználók toohello hozzáférés biztosítása nem feltétlenül meg elég helyes.</span><span class="sxs-lookup"><span data-stu-id="4dd3c-115">For certain resources in your environment, granting access toohello right users might not be good enough.</span></span> <span data-ttu-id="4dd3c-116">Továbbá toohello arra jogosult felhasználók, is szüksége lehet a megbízható eszköz toobe használt tooaccess erőforrás.</span><span class="sxs-lookup"><span data-stu-id="4dd3c-116">In addition toohello right users, you might also require a trusted device toobe used tooaccess a resource.</span></span> <span data-ttu-id="4dd3c-117">A környezetben, megadhatja, mi megbízható eszköz alapuló összetevők következő hello:</span><span class="sxs-lookup"><span data-stu-id="4dd3c-117">In your environment, you can define what a trusted device is based on hello following components:</span></span>

- <span data-ttu-id="4dd3c-118">Hello [eszközplatformok](active-directory-conditional-access-azure-portal.md#device-platforms) az eszközön</span><span class="sxs-lookup"><span data-stu-id="4dd3c-118">hello [device platforms](active-directory-conditional-access-azure-portal.md#device-platforms) on a device</span></span>
- <span data-ttu-id="4dd3c-119">Egy eszköz-e megfelelő</span><span class="sxs-lookup"><span data-stu-id="4dd3c-119">Whether a device is compliant</span></span>
- <span data-ttu-id="4dd3c-120">Egy eszköz-e a tartományhoz</span><span class="sxs-lookup"><span data-stu-id="4dd3c-120">Whether a device is domain-joined</span></span> 

<span data-ttu-id="4dd3c-121">Hello [eszközplatformok](active-directory-conditional-access-azure-portal.md#device-platforms) jellemzőek, az eszközön futó operációs rendszer hello.</span><span class="sxs-lookup"><span data-stu-id="4dd3c-121">hello [device platforms](active-directory-conditional-access-azure-portal.md#device-platforms) is characterized by hello operating system that is running on your device.</span></span> <span data-ttu-id="4dd3c-122">Az eszközalapú feltételes hozzáférési házirend korlátozhatja a hozzáférést toocertain erőforrások toospecific eszközplatformokat.</span><span class="sxs-lookup"><span data-stu-id="4dd3c-122">In your device-based conditional access policy, you can limit access toocertain resources toospecific device platforms.</span></span>



<span data-ttu-id="4dd3c-123">Eszközalapú feltételes hozzáférési szabályzatot, a megbízható eszközök toobe megjelölve megfelelőnek lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="4dd3c-123">In a device-based conditional access policy, you can require trusted devices toobe marked as compliant.</span></span>

![Felhőalkalmazások](./media/active-directory-conditional-access-policy-connected-applications/24.png)

<span data-ttu-id="4dd3c-125">Eszközök hello könyvtárban által megfelelőnek jelölhető ki:</span><span class="sxs-lookup"><span data-stu-id="4dd3c-125">Devices can be marked as compliant in hello directory by:</span></span>

- <span data-ttu-id="4dd3c-126">Intune-ban</span><span class="sxs-lookup"><span data-stu-id="4dd3c-126">Intune</span></span> 
- <span data-ttu-id="4dd3c-127">Egy harmadik fél mobileszköz felügyeleti rendszer, amely az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="4dd3c-127">A third-party mobile device management system that integrates with Azure AD</span></span>  

<span data-ttu-id="4dd3c-128">Csak az eszközök, amelyek csatlakoztatott tooAzure AD megfelelőnek jelölhető.</span><span class="sxs-lookup"><span data-stu-id="4dd3c-128">Only devices that are connected tooAzure AD can be marked as compliant.</span></span> <span data-ttu-id="4dd3c-129">tooconnect egy eszköz tooAzure Active Directory, a következő beállítások hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="4dd3c-129">tooconnect a device tooAzure Active Directory, you have hello following options:</span></span> 

- <span data-ttu-id="4dd3c-130">Regisztrálva az Azure AD</span><span class="sxs-lookup"><span data-stu-id="4dd3c-130">Azure AD registered</span></span>
- <span data-ttu-id="4dd3c-131">Az Azure AD-tartományhoz</span><span class="sxs-lookup"><span data-stu-id="4dd3c-131">Azure AD joined</span></span>
- <span data-ttu-id="4dd3c-132">Az Azure AD hibrid csatlakoztatva</span><span class="sxs-lookup"><span data-stu-id="4dd3c-132">Hybrid Azure AD joined</span></span>

    ![Felhőalkalmazások](./media/active-directory-conditional-access-policy-connected-applications/26.png)

<span data-ttu-id="4dd3c-134">Ha egy a helyszíni Active Directory (AD) kezdjen, érdemes lehet eszközök, amelyek nem csatlakoztatott tooAzure AD, de a megbízható AD illesztett tooyour toobe.</span><span class="sxs-lookup"><span data-stu-id="4dd3c-134">If you have an on-premises Active Directory (AD) footprint, you might consider devices that are not connected tooAzure AD but joined tooyour AD toobe trusted.</span></span>

![Felhőalkalmazások](./media/active-directory-conditional-access-policy-connected-applications/25.png)


## <a name="next-steps"></a><span data-ttu-id="4dd3c-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4dd3c-136">Next steps</span></span>

<span data-ttu-id="4dd3c-137">Eszközalapú feltételes hozzáférési házirend konfigurálása a környezetben, előtt meg kell vessen egy pillantást hello [ajánlott eljárások a feltételes hozzáférés az Azure Active Directoryban](active-directory-conditional-access-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="4dd3c-137">Before configuring a device-based conditional access policy in your environment, you should take a look at hello [best practices for conditional access in Azure Active Directory](active-directory-conditional-access-best-practices.md).</span></span>

