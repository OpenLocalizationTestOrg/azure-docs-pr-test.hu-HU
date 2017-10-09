---
title: Azure Active Directory Identity Protection aaaEnabling |} Microsoft Docs
description: "Megtudhatja, hogyan tooenable Azure Active Directory azonosító adatok védelmét."
services: active-directory
keywords: "az Azure active directory azonosító adatok védelmét, a cloud app discovery, alkalmazások, biztonság, kockázat, kockázati szint, biztonsági rés, biztonsági házirend kezelése"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: f7a7ffaf-76bf-4cc7-96a1-86c944275c82
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: d26f466f5c7d6a425528a277d98c2c4b341ff8de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enabling-azure-active-directory-identity-protection"></a><span data-ttu-id="8ba75-104">Az Azure Active Directory-identitás védelem engedélyezése</span><span class="sxs-lookup"><span data-stu-id="8ba75-104">Enabling Azure Active Directory Identity Protection</span></span>
<span data-ttu-id="8ba75-105">Az Azure Active Directory azonosító adatok védelmét, mely tartalmazza a gyanús bejelentkezési tevékenységek és a potenciális biztonsági réseket egyesített nézetét, és értesítéseket, a javítási javaslatokat és a kockázati-alapú házirendek segítségével megóvhatja új képessége az üzleti.</span><span class="sxs-lookup"><span data-stu-id="8ba75-105">Azure Active Directory Identity Protection is a new capability that provides a consolidated view into suspicious sign-in activities and potential vulnerabilities and with notifications, remediation recommendations and risk-based policies helps you protect your business.</span></span> 

<span data-ttu-id="8ba75-106">hello szolgáltatás észleli a gyanús tevékenységek a végfelhasználók és a kiemelt (rendszergazda) identitások jelek, például a találgatásos támadáson alapuló támadások, kényszerítése szivárgását hitelesítő adatokat, ismeretlen helyekről indított bejelentkezések fertőzött eszközök, tooprotect ezeket a tevékenységeket, valós idejű ellen.</span><span class="sxs-lookup"><span data-stu-id="8ba75-106">hello service detects suspicious activities for end user and privileged (admin) identities based on signals like brute force attacks, leaked credentials, sign ins from unfamiliar locations, infected devices, tooprotect against these activities in real-time.</span></span> <span data-ttu-id="8ba75-107">Ennél is fontosabb ezek a gyanús tevékenységek alapján, a felhasználó kockázati besorolású számított és kockázati-alapú házirendek konfigurálhatók, és automatikusan védő hello a szervezet.</span><span class="sxs-lookup"><span data-stu-id="8ba75-107">More importantly, based on these suspicious activities, a user risk severity is computed and risk-based policies can be configured and automatically protect hello identities of your organization.</span></span> <span data-ttu-id="8ba75-108">További részletekért lásd: [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span><span class="sxs-lookup"><span data-stu-id="8ba75-108">For more details, see [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span></span>

<span data-ttu-id="8ba75-109">A témakörök bemutatja, hogyan tooenable Azure Active Directory azonosító adatok védelmét.</span><span class="sxs-lookup"><span data-stu-id="8ba75-109">This topics shows how tooenable Azure Active Directory Identity Protection.</span></span>

## <a name="steps-tooenable-azure-active-directory-identity-protection"></a><span data-ttu-id="8ba75-110">Azure Active Directory Identity Protection tooenable lépések</span><span class="sxs-lookup"><span data-stu-id="8ba75-110">Steps tooenable Azure Active Directory Identity Protection</span></span>
1. <span data-ttu-id="8ba75-111">[Bejelentkezés](https://ms.portal.azure.com/) tooyour globális rendszergazdaként az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="8ba75-111">[Sign-on](https://ms.portal.azure.com/) tooyour Azure portal as global administrator.</span></span> 
2. <span data-ttu-id="8ba75-112">Hello Azure-portálon, kattintson **piactér**.</span><span class="sxs-lookup"><span data-stu-id="8ba75-112">In hello Azure portal, click **Marketplace**.</span></span>
   
    <span data-ttu-id="8ba75-113">![Hozzon létre](./media/active-directory-identityprotection-enable/01.png "létrehozása")</span><span class="sxs-lookup"><span data-stu-id="8ba75-113">![Create](./media/active-directory-identityprotection-enable/01.png "Create")</span></span>
3. <span data-ttu-id="8ba75-114">Hello alkalmazások listájában kattintson **biztonság + identitás szakaszában**.</span><span class="sxs-lookup"><span data-stu-id="8ba75-114">In hello applications list, click **Security + Identity**.</span></span>
   
    <span data-ttu-id="8ba75-115">![Hozzon létre](./media/active-directory-identityprotection-enable/02.png "létrehozása")</span><span class="sxs-lookup"><span data-stu-id="8ba75-115">![Create](./media/active-directory-identityprotection-enable/02.png "Create")</span></span>
4. <span data-ttu-id="8ba75-116">Kattintson a **Azure AD Identity Protection**.</span><span class="sxs-lookup"><span data-stu-id="8ba75-116">Click **Azure AD Identity Protection**.</span></span>
   
    <span data-ttu-id="8ba75-117">![Hozzon létre](./media/active-directory-identityprotection-enable/03.png "létrehozása")</span><span class="sxs-lookup"><span data-stu-id="8ba75-117">![Create](./media/active-directory-identityprotection-enable/03.png "Create")</span></span>
5. <span data-ttu-id="8ba75-118">A hello **Azure AD Identity Protection** panelen kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="8ba75-118">On hello **Azure AD Identity Protection** blade, click **Create**.</span></span>
   
    <span data-ttu-id="8ba75-119">![Hozzon létre](./media/active-directory-identityprotection-enable/04.png "létrehozása")</span><span class="sxs-lookup"><span data-stu-id="8ba75-119">![Create](./media/active-directory-identityprotection-enable/04.png "Create")</span></span>

## <a name="next-steps"></a><span data-ttu-id="8ba75-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8ba75-120">Next Steps</span></span>
* [<span data-ttu-id="8ba75-121">Az Azure Active Directory azonosító adatok védelmét</span><span class="sxs-lookup"><span data-stu-id="8ba75-121">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)

