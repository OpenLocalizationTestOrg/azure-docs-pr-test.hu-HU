---
title: "Az Azure Active Directory Identity Protection-értesítések |} Microsoft Docs"
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
ms.openlocfilehash: 079d16bbf75cd2b3b94269d684e1ae1a0e6aa967
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-identity-protection-notifications"></a><span data-ttu-id="71e6b-104">Az Azure Active Directory Identity Protection-értesítések</span><span class="sxs-lookup"><span data-stu-id="71e6b-104">Azure Active Directory Identity Protection notifications</span></span>
<span data-ttu-id="71e6b-105">Az Azure AD Identity Protection küld a kétféle típusú automatizált értesítési e-mailek felhasználói kockázat és a kockázati eseményekről kezeléséhez:</span><span class="sxs-lookup"><span data-stu-id="71e6b-105">Azure AD Identity Protection sends two types of automated notification emails to help you manage user risk and risk events:</span></span>

* <span data-ttu-id="71e6b-106">Felhasználói értesítés e-mailek biztonsága sérült</span><span class="sxs-lookup"><span data-stu-id="71e6b-106">User compromised alert email</span></span>
* <span data-ttu-id="71e6b-107">Heti kivonatoló e-mail</span><span class="sxs-lookup"><span data-stu-id="71e6b-107">Weekly digest email</span></span>

## <a name="user-compromised-alert-email"></a><span data-ttu-id="71e6b-108">Felhasználói értesítés e-mailek biztonsága sérült</span><span class="sxs-lookup"><span data-stu-id="71e6b-108">User compromised alert email</span></span>
<span data-ttu-id="71e6b-109">Egy felhasználó sérült biztonságú e-mail riasztást generál, ha az Azure AD Identity Protection egy fiókot, biztonsági sérülés.</span><span class="sxs-lookup"><span data-stu-id="71e6b-109">A user compromised email alert is generated when Azure AD Identity Protection identifies an account as compromised.</span></span> <span data-ttu-id="71e6b-110">Az e-mailben a felhasználóknak meg van jelölve a kockázat a jelentést az Identity Protection-irányítópult a hivatkozást tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="71e6b-110">The email includes a link to the Users flagged for risk report in the Identity Protection dashboard.</span></span> <span data-ttu-id="71e6b-111">Azt javasoljuk, hogy a sérült biztonságú fiókok értesítések azonnal vizsgálata.</span><span class="sxs-lookup"><span data-stu-id="71e6b-111">We recommend that you immediately investigate notifications of compromised accounts.</span></span>

## <a name="weekly-digest-email"></a><span data-ttu-id="71e6b-112">Heti kivonatoló e-mail</span><span class="sxs-lookup"><span data-stu-id="71e6b-112">Weekly digest email</span></span>
<span data-ttu-id="71e6b-113">A heti kivonatoló e-mail új kockázati események összegzését tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="71e6b-113">The weekly digest email contains a summary of new risk events.</span></span><br>
<span data-ttu-id="71e6b-114">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="71e6b-114">It includes:</span></span>

* <span data-ttu-id="71e6b-115">Érintett felhasználók</span><span class="sxs-lookup"><span data-stu-id="71e6b-115">Users at risk</span></span>
* <span data-ttu-id="71e6b-116">A gyanús tevékenységek</span><span class="sxs-lookup"><span data-stu-id="71e6b-116">Suspicious activities</span></span>
* <span data-ttu-id="71e6b-117">Észlelt biztonsági rések</span><span class="sxs-lookup"><span data-stu-id="71e6b-117">Detected vulnerabilities</span></span>
* <span data-ttu-id="71e6b-118">A kapcsolódó jelentéseket a Identity Protection mutató hivatkozások</span><span class="sxs-lookup"><span data-stu-id="71e6b-118">Links to the related reports in Identity Protection</span></span>

<br><span data-ttu-id="71e6b-119">
![Szervizelés](./media/active-directory-identityprotection-notifications/400.png "szervizelés")
</span><span class="sxs-lookup"><span data-stu-id="71e6b-119">
![Remediation](./media/active-directory-identityprotection-notifications/400.png "Remediation")
</span></span><br>

<span data-ttu-id="71e6b-120">Válthat heti kivonatoló e-mailek küldésekor.</span><span class="sxs-lookup"><span data-stu-id="71e6b-120">You can switch sending a weekly digest email off.</span></span>
<br><br><span data-ttu-id="71e6b-121">
![Felhasználói kockázatok](./media/active-directory-identityprotection-notifications/62.png "felhasználói kockázatok")
</span><span class="sxs-lookup"><span data-stu-id="71e6b-121">
![User risks](./media/active-directory-identityprotection-notifications/62.png "User risks")
</span></span><br>

<span data-ttu-id="71e6b-122">**A kapcsolódó konfigurációs párbeszédpanel megnyitásához**:</span><span class="sxs-lookup"><span data-stu-id="71e6b-122">**To open the related configuration dialog**:</span></span>

1. <span data-ttu-id="71e6b-123">Az a **Azure AD Identity Protection** panelen kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="71e6b-123">On the **Azure AD Identity Protection** blade, click **Settings**.</span></span>
   <br><br><span data-ttu-id="71e6b-124">
   ![Felhasználói kockázat házirendnek](./media/active-directory-identityprotection-notifications/401.png "felhasználói kockázat házirendnek")
   </span><span class="sxs-lookup"><span data-stu-id="71e6b-124">
![User risk policy](./media/active-directory-identityprotection-notifications/401.png "User risk policy")
</span></span><br>
2. <span data-ttu-id="71e6b-125">Az a **általános** kattintson **értesítések**.</span><span class="sxs-lookup"><span data-stu-id="71e6b-125">In the **General** section, click **Notifications**.</span></span>
   <br><br><span data-ttu-id="71e6b-126">
   ![Felhasználói kockázat házirendnek](./media/active-directory-identityprotection-notifications/405.png "felhasználói kockázat házirendnek")
   </span><span class="sxs-lookup"><span data-stu-id="71e6b-126">
![User risk policy](./media/active-directory-identityprotection-notifications/405.png "User risk policy")
</span></span><br>

## <a name="see-also"></a><span data-ttu-id="71e6b-127">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="71e6b-127">See also</span></span>
* [<span data-ttu-id="71e6b-128">Az Azure Active Directory azonosító adatok védelmét</span><span class="sxs-lookup"><span data-stu-id="71e6b-128">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)
