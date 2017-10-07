---
title: "aaaAzure Active Directory-Identity Protection-értesítések |} Microsoft Docs"
description: "Ismerje meg, hogyan támogatják a különböző értesítések a vizsgálati tevékenységet."
services: active-directory
keywords: "az Azure active directory azonosító adatok védelmét, a cloud app discovery, alkalmazások, biztonság, kockázat, kockázati szint, biztonsági rés, biztonsági házirend kezelése"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 65ca79b9-4da1-4d5b-bebd-eda776cc32c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 19e62374873f034591c658a779f97d935766c612
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection-notifications"></a><span data-ttu-id="dcb18-104">Az Azure Active Directory Identity Protection-értesítések</span><span class="sxs-lookup"><span data-stu-id="dcb18-104">Azure Active Directory Identity Protection notifications</span></span>
<span data-ttu-id="dcb18-105">Az Azure AD Identity Protection küld a kétféle típusú automatizált értesítési e-mailt küld toohelp kezelheti a felhasználói kockázat és kockázati események:</span><span class="sxs-lookup"><span data-stu-id="dcb18-105">Azure AD Identity Protection sends two types of automated notification emails toohelp you manage user risk and risk events:</span></span>

* <span data-ttu-id="dcb18-106">Felhasználói értesítés e-mailek biztonsága sérült</span><span class="sxs-lookup"><span data-stu-id="dcb18-106">User compromised alert email</span></span>
* <span data-ttu-id="dcb18-107">Heti kivonatoló e-mail</span><span class="sxs-lookup"><span data-stu-id="dcb18-107">Weekly digest email</span></span>

## <a name="user-compromised-alert-email"></a><span data-ttu-id="dcb18-108">Felhasználói értesítés e-mailek biztonsága sérült</span><span class="sxs-lookup"><span data-stu-id="dcb18-108">User compromised alert email</span></span>
<span data-ttu-id="dcb18-109">Egy felhasználó sérült biztonságú e-mail riasztást generál, ha az Azure AD Identity Protection egy fiókot, biztonsági sérülés.</span><span class="sxs-lookup"><span data-stu-id="dcb18-109">A user compromised email alert is generated when Azure AD Identity Protection identifies an account as compromised.</span></span> <span data-ttu-id="dcb18-110">hello e-mail tartalmaz egy hivatkozást toohello felhasználók meg van jelölve, a kockázat jelentés hello Identity Protection-irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="dcb18-110">hello email includes a link toohello Users flagged for risk report in hello Identity Protection dashboard.</span></span> <span data-ttu-id="dcb18-111">Azt javasoljuk, hogy a sérült biztonságú fiókok értesítések azonnal vizsgálata.</span><span class="sxs-lookup"><span data-stu-id="dcb18-111">We recommend that you immediately investigate notifications of compromised accounts.</span></span>

## <a name="weekly-digest-email"></a><span data-ttu-id="dcb18-112">Heti kivonatoló e-mail</span><span class="sxs-lookup"><span data-stu-id="dcb18-112">Weekly digest email</span></span>
<span data-ttu-id="dcb18-113">hello heti kivonatoló e-mail új kockázati események összegzését tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="dcb18-113">hello weekly digest email contains a summary of new risk events.</span></span><br>
<span data-ttu-id="dcb18-114">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="dcb18-114">It includes:</span></span>

* <span data-ttu-id="dcb18-115">Érintett felhasználók</span><span class="sxs-lookup"><span data-stu-id="dcb18-115">Users at risk</span></span>
* <span data-ttu-id="dcb18-116">A gyanús tevékenységek</span><span class="sxs-lookup"><span data-stu-id="dcb18-116">Suspicious activities</span></span>
* <span data-ttu-id="dcb18-117">Észlelt biztonsági rések</span><span class="sxs-lookup"><span data-stu-id="dcb18-117">Detected vulnerabilities</span></span>
* <span data-ttu-id="dcb18-118">Hivatkozások toohello kapcsolódó Identity Protection a jelentések</span><span class="sxs-lookup"><span data-stu-id="dcb18-118">Links toohello related reports in Identity Protection</span></span>

<br><span data-ttu-id="dcb18-119">
![Szervizelés](./media/active-directory-identityprotection-notifications/400.png "szervizelés")
</span><span class="sxs-lookup"><span data-stu-id="dcb18-119">
![Remediation](./media/active-directory-identityprotection-notifications/400.png "Remediation")
</span></span><br>

<span data-ttu-id="dcb18-120">Válthat heti kivonatoló e-mailek küldésekor.</span><span class="sxs-lookup"><span data-stu-id="dcb18-120">You can switch sending a weekly digest email off.</span></span>
<br><br><span data-ttu-id="dcb18-121">
![Felhasználói kockázatok](./media/active-directory-identityprotection-notifications/62.png "felhasználói kockázatok")
</span><span class="sxs-lookup"><span data-stu-id="dcb18-121">
![User risks](./media/active-directory-identityprotection-notifications/62.png "User risks")
</span></span><br>

<span data-ttu-id="dcb18-122">**tooopen hello kapcsolódó konfigurációs párbeszédpanel**:</span><span class="sxs-lookup"><span data-stu-id="dcb18-122">**tooopen hello related configuration dialog**:</span></span>

1. <span data-ttu-id="dcb18-123">A hello **Azure AD Identity Protection** panelen kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="dcb18-123">On hello **Azure AD Identity Protection** blade, click **Settings**.</span></span>
   <br><br><span data-ttu-id="dcb18-124">
   ![Felhasználói kockázat házirendnek](./media/active-directory-identityprotection-notifications/401.png "felhasználói kockázat házirendnek")
   </span><span class="sxs-lookup"><span data-stu-id="dcb18-124">
![User risk policy](./media/active-directory-identityprotection-notifications/401.png "User risk policy")
</span></span><br>
2. <span data-ttu-id="dcb18-125">A hello **általános** kattintson **értesítések**.</span><span class="sxs-lookup"><span data-stu-id="dcb18-125">In hello **General** section, click **Notifications**.</span></span>
   <br><br><span data-ttu-id="dcb18-126">
   ![Felhasználói kockázat házirendnek](./media/active-directory-identityprotection-notifications/405.png "felhasználói kockázat házirendnek")
   </span><span class="sxs-lookup"><span data-stu-id="dcb18-126">
![User risk policy](./media/active-directory-identityprotection-notifications/405.png "User risk policy")
</span></span><br>

## <a name="see-also"></a><span data-ttu-id="dcb18-127">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="dcb18-127">See also</span></span>
* [<span data-ttu-id="dcb18-128">Az Azure Active Directory azonosító adatok védelmét</span><span class="sxs-lookup"><span data-stu-id="dcb18-128">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)
