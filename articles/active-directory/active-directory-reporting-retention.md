---
title: "Az Azure Active Directory-jelentés adatmegőrzési |} Microsoft Docs"
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
ms.openlocfilehash: ffeba8a6c32a21c75af21f948bbd6ea88dd9278c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-report-retention-policies"></a><span data-ttu-id="50835-103">Az Azure Active Directory-jelentés adatmegőrzési szabályai</span><span class="sxs-lookup"><span data-stu-id="50835-103">Azure Active Directory report retention policies</span></span>


<span data-ttu-id="50835-104">Ez a témakör a kérdésekre adott válaszok a leggyakrabban használt a különböző tevékenység jelentések az Azure Active Directoryban az adatok megőrzése mellett.</span><span class="sxs-lookup"><span data-stu-id="50835-104">This topic provides you with answers to the most common questions in conjunction with the data retention for the different activity reports in Azure Active Directory.</span></span> 

<span data-ttu-id="50835-105">**K: hogyan férhetnek indítása tevékenység adatok gyűjtését?**</span><span class="sxs-lookup"><span data-stu-id="50835-105">**Q: How can you get the collection of activity data started?**</span></span>

<span data-ttu-id="50835-106">**V:**</span><span class="sxs-lookup"><span data-stu-id="50835-106">**A:**</span></span>

| <span data-ttu-id="50835-107">Az Azure AD Edition</span><span class="sxs-lookup"><span data-stu-id="50835-107">Azure AD Edition</span></span> | <span data-ttu-id="50835-108">Gyűjtemény indítása</span><span class="sxs-lookup"><span data-stu-id="50835-108">Collection Start</span></span> |
| :--              | :--   |
| <span data-ttu-id="50835-109">Prémium szintű Azure AD P1</span><span class="sxs-lookup"><span data-stu-id="50835-109">Azure AD Premium P1</span></span> <br /> <span data-ttu-id="50835-110">Prémium szintű Azure AD P2</span><span class="sxs-lookup"><span data-stu-id="50835-110">Azure AD Premium P2</span></span> | <span data-ttu-id="50835-111">Ha a regisztráláskor az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="50835-111">When you sign-up for a subscription</span></span> |
| <span data-ttu-id="50835-112">Azure AD Free</span><span class="sxs-lookup"><span data-stu-id="50835-112">Azure AD Free</span></span> | <span data-ttu-id="50835-113">Az első megnyitásakor a [Azure Active Directory panel](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) , vagy használja a [reporting API-k](https://aka.ms/aadreports)</span><span class="sxs-lookup"><span data-stu-id="50835-113">The first time you open the [Azure Active Directory blade](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) or use the [reporting APIs](https://aka.ms/aadreports)</span></span>  |

---
<span data-ttu-id="50835-114">**K: mikor érhető el a tevékenységek adatai az Azure-portálon?**</span><span class="sxs-lookup"><span data-stu-id="50835-114">**Q: When is your activity data available in the Azure portal?**</span></span>

<span data-ttu-id="50835-115">**V:**</span><span class="sxs-lookup"><span data-stu-id="50835-115">**A:**</span></span>

- <span data-ttu-id="50835-116">**Azonnal** – Ha már dolgozott jelentéseket a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="50835-116">**Immediately** - If you have already been working with reports in the Azure classic portal</span></span>
- <span data-ttu-id="50835-117">**2 órán belül** – Ha nem kapcsolta reporting klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="50835-117">**Within 2 hours** - If you haven’t turned reporting on  in the Azure classic portal</span></span>

---
<span data-ttu-id="50835-118">**K: hogyan férhetnek a gyűjtemény biztonsági jelek lépések?**</span><span class="sxs-lookup"><span data-stu-id="50835-118">**Q: How can you get the collection of security signals started?**</span></span>  

<span data-ttu-id="50835-119">**V:** a biztonsági jelek, az adatgyűjtési folyamat kezdődik, amikor Ön részt a Identity Protection-központtól használja.</span><span class="sxs-lookup"><span data-stu-id="50835-119">**A:** For security signals, the collection process starts when you opt-in to use the Identity Protection Center.</span></span> 


---
<span data-ttu-id="50835-120">**K: mennyi ideig az összegyűjtött adatok tárolják?**</span><span class="sxs-lookup"><span data-stu-id="50835-120">**Q: For how long is the collected data stored?**</span></span>

<span data-ttu-id="50835-121">**V:**</span><span class="sxs-lookup"><span data-stu-id="50835-121">**A:**</span></span>

<span data-ttu-id="50835-122">**Tevékenységjelentések**</span><span class="sxs-lookup"><span data-stu-id="50835-122">**Activity reports**</span></span>    

| <span data-ttu-id="50835-123">Jelentés</span><span class="sxs-lookup"><span data-stu-id="50835-123">Report</span></span>                 | <span data-ttu-id="50835-124">Azure AD Free</span><span class="sxs-lookup"><span data-stu-id="50835-124">Azure AD Free</span></span> | <span data-ttu-id="50835-125">Prémium szintű Azure AD P1</span><span class="sxs-lookup"><span data-stu-id="50835-125">Azure AD Premium P1</span></span> | <span data-ttu-id="50835-126">Prémium szintű Azure AD P2</span><span class="sxs-lookup"><span data-stu-id="50835-126">Azure AD Premium P2</span></span> |
| :--                    | :--           | :--                 | :--                 |
| <span data-ttu-id="50835-127">Címtárnaplózás</span><span class="sxs-lookup"><span data-stu-id="50835-127">Directory Audit</span></span>        | <span data-ttu-id="50835-128">7 nap</span><span class="sxs-lookup"><span data-stu-id="50835-128">7 days</span></span>        | <span data-ttu-id="50835-129">30 nap</span><span class="sxs-lookup"><span data-stu-id="50835-129">30 days</span></span>             | <span data-ttu-id="50835-130">30 nap</span><span class="sxs-lookup"><span data-stu-id="50835-130">30 days</span></span>             |
| <span data-ttu-id="50835-131">Bejelentkezési tevékenységek</span><span class="sxs-lookup"><span data-stu-id="50835-131">Sign-in Activity</span></span>       | <span data-ttu-id="50835-132">7 nap</span><span class="sxs-lookup"><span data-stu-id="50835-132">7 days</span></span>        | <span data-ttu-id="50835-133">30 nap</span><span class="sxs-lookup"><span data-stu-id="50835-133">30 days</span></span>             | <span data-ttu-id="50835-134">30 nap</span><span class="sxs-lookup"><span data-stu-id="50835-134">30 days</span></span>             |

<span data-ttu-id="50835-135">**Biztonsági jelek**</span><span class="sxs-lookup"><span data-stu-id="50835-135">**Security Signals**</span></span>

| <span data-ttu-id="50835-136">Jelentés</span><span class="sxs-lookup"><span data-stu-id="50835-136">Report</span></span>         | <span data-ttu-id="50835-137">Azure AD Free</span><span class="sxs-lookup"><span data-stu-id="50835-137">Azure AD Free</span></span> | <span data-ttu-id="50835-138">Prémium szintű Azure AD P1</span><span class="sxs-lookup"><span data-stu-id="50835-138">Azure AD Premium P1</span></span> | <span data-ttu-id="50835-139">Prémium szintű Azure AD P2</span><span class="sxs-lookup"><span data-stu-id="50835-139">Azure AD Premium P2</span></span> |
| :--            | :--           | :--                 | :--                 |
| <span data-ttu-id="50835-140">Érintett felhasználók</span><span class="sxs-lookup"><span data-stu-id="50835-140">Users at risk</span></span>  | <span data-ttu-id="50835-141">7 nap</span><span class="sxs-lookup"><span data-stu-id="50835-141">7 days</span></span>        | <span data-ttu-id="50835-142">30 nap</span><span class="sxs-lookup"><span data-stu-id="50835-142">30 days</span></span>             | <span data-ttu-id="50835-143">90 nap</span><span class="sxs-lookup"><span data-stu-id="50835-143">90 days</span></span>             |
| <span data-ttu-id="50835-144">Kockázatos bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="50835-144">Risky sign-ins</span></span> | <span data-ttu-id="50835-145">7 nap</span><span class="sxs-lookup"><span data-stu-id="50835-145">7 days</span></span>        | <span data-ttu-id="50835-146">30 nap</span><span class="sxs-lookup"><span data-stu-id="50835-146">30 days</span></span>             | <span data-ttu-id="50835-147">90 nap</span><span class="sxs-lookup"><span data-stu-id="50835-147">90 days</span></span>             |

---