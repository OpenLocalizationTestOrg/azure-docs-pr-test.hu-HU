---
title: "az Active Directory-jelentés adatmegőrzési aaaAzure |} Microsoft Docs"
description: "A jelentés adatokat az Azure Active Directoryban adatmegőrzési"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/05/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: c46a68198cb34e9c92662b2f8461010745392c04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-report-retention-policies"></a><span data-ttu-id="ed699-103">Az Azure Active Directory-jelentés adatmegőrzési szabályai</span><span class="sxs-lookup"><span data-stu-id="ed699-103">Azure Active Directory report retention policies</span></span>


<span data-ttu-id="ed699-104">Ez a témakör a válaszok toohello leggyakoribb kérdések hello különböző tevékenység jelentések az Azure Active Directoryban hello adatok megőrzése mellett.</span><span class="sxs-lookup"><span data-stu-id="ed699-104">This topic provides you with answers toohello most common questions in conjunction with hello data retention for hello different activity reports in Azure Active Directory.</span></span> 

<span data-ttu-id="ed699-105">**K: hogyan férhetnek hello adatgyűjtés tevékenység elindult?**</span><span class="sxs-lookup"><span data-stu-id="ed699-105">**Q: How can you get hello collection of activity data started?**</span></span>

<span data-ttu-id="ed699-106">**V:**</span><span class="sxs-lookup"><span data-stu-id="ed699-106">**A:**</span></span>

| <span data-ttu-id="ed699-107">Az Azure AD Edition</span><span class="sxs-lookup"><span data-stu-id="ed699-107">Azure AD Edition</span></span> | <span data-ttu-id="ed699-108">Gyűjtemény indítása</span><span class="sxs-lookup"><span data-stu-id="ed699-108">Collection Start</span></span> |
| :--              | :--   |
| <span data-ttu-id="ed699-109">Prémium szintű Azure AD P1</span><span class="sxs-lookup"><span data-stu-id="ed699-109">Azure AD Premium P1</span></span> <br /> <span data-ttu-id="ed699-110">Prémium szintű Azure AD P2</span><span class="sxs-lookup"><span data-stu-id="ed699-110">Azure AD Premium P2</span></span> | <span data-ttu-id="ed699-111">Ha a regisztráláskor az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="ed699-111">When you sign-up for a subscription</span></span> |
| <span data-ttu-id="ed699-112">Azure AD Free</span><span class="sxs-lookup"><span data-stu-id="ed699-112">Azure AD Free</span></span> | <span data-ttu-id="ed699-113">hello első megnyitásakor hello [Azure Active Directory panel](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) , vagy használjon hello [reporting API-k](https://aka.ms/aadreports)</span><span class="sxs-lookup"><span data-stu-id="ed699-113">hello first time you open hello [Azure Active Directory blade](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) or use hello [reporting APIs](https://aka.ms/aadreports)</span></span>  |

---
<span data-ttu-id="ed699-114">**K: mikor érhető el a tevékenységek adatai hello Azure-portálon?**</span><span class="sxs-lookup"><span data-stu-id="ed699-114">**Q: When is your activity data available in hello Azure portal?**</span></span>

<span data-ttu-id="ed699-115">**V:**</span><span class="sxs-lookup"><span data-stu-id="ed699-115">**A:**</span></span>

- <span data-ttu-id="ed699-116">**Azonnal** – Ha már dolgozott jelentésekkel hello a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="ed699-116">**Immediately** - If you have already been working with reports in hello Azure classic portal</span></span>
- <span data-ttu-id="ed699-117">**2 órán belül** – Ha még nem bekapcsolta a jelentés a klasszikus Azure portálon hello</span><span class="sxs-lookup"><span data-stu-id="ed699-117">**Within 2 hours** - If you haven’t turned reporting on  in hello Azure classic portal</span></span>

---
<span data-ttu-id="ed699-118">**K: hogyan férhetnek lépések biztonsági jelek hello gyűjtemény?**</span><span class="sxs-lookup"><span data-stu-id="ed699-118">**Q: How can you get hello collection of security signals started?**</span></span>  

<span data-ttu-id="ed699-119">**V:** biztonsági jelek, az adatgyűjtési folyamat hello kezdődik, amikor Ön részt toouse hello Identity Protection-központtól.</span><span class="sxs-lookup"><span data-stu-id="ed699-119">**A:** For security signals, hello collection process starts when you opt-in toouse hello Identity Protection Center.</span></span> 


---
<span data-ttu-id="ed699-120">**K: mennyi ideig hello összegyűjtött adatok tárolják?**</span><span class="sxs-lookup"><span data-stu-id="ed699-120">**Q: For how long is hello collected data stored?**</span></span>

<span data-ttu-id="ed699-121">**V:**</span><span class="sxs-lookup"><span data-stu-id="ed699-121">**A:**</span></span>

<span data-ttu-id="ed699-122">**Tevékenységjelentések**</span><span class="sxs-lookup"><span data-stu-id="ed699-122">**Activity reports**</span></span>    

| <span data-ttu-id="ed699-123">Jelentés</span><span class="sxs-lookup"><span data-stu-id="ed699-123">Report</span></span>                 | <span data-ttu-id="ed699-124">Azure AD Free</span><span class="sxs-lookup"><span data-stu-id="ed699-124">Azure AD Free</span></span> | <span data-ttu-id="ed699-125">Prémium szintű Azure AD P1</span><span class="sxs-lookup"><span data-stu-id="ed699-125">Azure AD Premium P1</span></span> | <span data-ttu-id="ed699-126">Prémium szintű Azure AD P2</span><span class="sxs-lookup"><span data-stu-id="ed699-126">Azure AD Premium P2</span></span> |
| :--                    | :--           | :--                 | :--                 |
| <span data-ttu-id="ed699-127">Címtárnaplózás</span><span class="sxs-lookup"><span data-stu-id="ed699-127">Directory Audit</span></span>        | <span data-ttu-id="ed699-128">7 nap</span><span class="sxs-lookup"><span data-stu-id="ed699-128">7 days</span></span>        | <span data-ttu-id="ed699-129">30 nap</span><span class="sxs-lookup"><span data-stu-id="ed699-129">30 days</span></span>             | <span data-ttu-id="ed699-130">30 nap</span><span class="sxs-lookup"><span data-stu-id="ed699-130">30 days</span></span>             |
| <span data-ttu-id="ed699-131">Bejelentkezési tevékenységek</span><span class="sxs-lookup"><span data-stu-id="ed699-131">Sign-in Activity</span></span>       | <span data-ttu-id="ed699-132">7 nap</span><span class="sxs-lookup"><span data-stu-id="ed699-132">7 days</span></span>        | <span data-ttu-id="ed699-133">30 nap</span><span class="sxs-lookup"><span data-stu-id="ed699-133">30 days</span></span>             | <span data-ttu-id="ed699-134">30 nap</span><span class="sxs-lookup"><span data-stu-id="ed699-134">30 days</span></span>             |

<span data-ttu-id="ed699-135">**Biztonsági jelek**</span><span class="sxs-lookup"><span data-stu-id="ed699-135">**Security Signals**</span></span>

| <span data-ttu-id="ed699-136">Jelentés</span><span class="sxs-lookup"><span data-stu-id="ed699-136">Report</span></span>         | <span data-ttu-id="ed699-137">Azure AD Free</span><span class="sxs-lookup"><span data-stu-id="ed699-137">Azure AD Free</span></span> | <span data-ttu-id="ed699-138">Prémium szintű Azure AD P1</span><span class="sxs-lookup"><span data-stu-id="ed699-138">Azure AD Premium P1</span></span> | <span data-ttu-id="ed699-139">Prémium szintű Azure AD P2</span><span class="sxs-lookup"><span data-stu-id="ed699-139">Azure AD Premium P2</span></span> |
| :--            | :--           | :--                 | :--                 |
| <span data-ttu-id="ed699-140">Érintett felhasználók</span><span class="sxs-lookup"><span data-stu-id="ed699-140">Users at risk</span></span>  | <span data-ttu-id="ed699-141">7 nap</span><span class="sxs-lookup"><span data-stu-id="ed699-141">7 days</span></span>        | <span data-ttu-id="ed699-142">30 nap</span><span class="sxs-lookup"><span data-stu-id="ed699-142">30 days</span></span>             | <span data-ttu-id="ed699-143">90 nap</span><span class="sxs-lookup"><span data-stu-id="ed699-143">90 days</span></span>             |
| <span data-ttu-id="ed699-144">Kockázatos bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="ed699-144">Risky sign-ins</span></span> | <span data-ttu-id="ed699-145">7 nap</span><span class="sxs-lookup"><span data-stu-id="ed699-145">7 days</span></span>        | <span data-ttu-id="ed699-146">30 nap</span><span class="sxs-lookup"><span data-stu-id="ed699-146">30 days</span></span>             | <span data-ttu-id="ed699-147">90 nap</span><span class="sxs-lookup"><span data-stu-id="ed699-147">90 days</span></span>             |

---