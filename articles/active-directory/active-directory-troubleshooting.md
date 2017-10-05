---
title: "Hibaelhárítás: \"Active Directory\" elem nem található vagy nem érhető el |} Microsoft Docs"
description: "Mi a teendő, ha az Active Directory menüpont nem jelenik meg az Azure felügyeleti portálján."
services: active-directory
documentationcenter: na
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 3383020d-6397-43ea-b7aa-c6a9d6a1e3df
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: bryanla
ms.openlocfilehash: be3a797c4a405fd2f6636e67f4c961dd6d143486
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-active-directory-item-is-missing-or-not-available"></a><span data-ttu-id="679b1-103">Hibaelhárítás: "Active Directory" elem nem található vagy nem érhető el</span><span class="sxs-lookup"><span data-stu-id="679b1-103">Troubleshooting: 'Active Directory' item is missing or not available</span></span>
<span data-ttu-id="679b1-104">Az Azure Active Directory-szolgáltatások és szolgáltatások használatára vonatkozó utasításokat számos kezdődhet "nyissa meg az Azure felügyeleti portálra, és kattintson a **Active Directory**."</span><span class="sxs-lookup"><span data-stu-id="679b1-104">Many of the instructions for using Azure Active Directory features and services begin with "Go to the Azure Management Portal and click **Active Directory**."</span></span> <span data-ttu-id="679b1-105">Mi a teendő, ha az Active Directory kiterjesztés vagy a menü elem nem jelenik meg, vagy ha meg van jelölve, de **nem érhető el**?</span><span class="sxs-lookup"><span data-stu-id="679b1-105">But what do you do if the Active Directory extension or menu item does not appear or if it is marked **Not Available**?</span></span> <span data-ttu-id="679b1-106">Ez a témakör célja.</span><span class="sxs-lookup"><span data-stu-id="679b1-106">This topic is designed to help.</span></span> <span data-ttu-id="679b1-107">Azt ismerteti, hogy a feltételeket, amelyek alapján **Active Directory** nem jelenik meg vagy nem érhető el, és elmagyarázza, hogyan folytatható.</span><span class="sxs-lookup"><span data-stu-id="679b1-107">It describes the conditions under which **Active Directory** does not appear or is unavailable and explains how to proceed.</span></span>

## <a name="active-directory-is-missing"></a><span data-ttu-id="679b1-108">Az Active Directory hiányzik.</span><span class="sxs-lookup"><span data-stu-id="679b1-108">Active Directory is missing</span></span>
<span data-ttu-id="679b1-109">Általában egy **Active Directory** elem jelenik meg a bal oldali navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="679b1-109">Typically, an **Active Directory** item appears in the left navigation menu.</span></span> <span data-ttu-id="679b1-110">Azure Active Directory eljárások utasítások feltételezik, hogy ezt az elemet a nézetben.</span><span class="sxs-lookup"><span data-stu-id="679b1-110">The instructions in Azure Active Directory procedures assume that this item is in your view.</span></span>

![Képernyőfelvétel: az Azure Active Directory](./media/active-directory-troubleshooting/typical-view.png)

<span data-ttu-id="679b1-112">Az Active Directory elem megjelenik a bal oldali navigációs menü, a következő feltételek teljesülése esetén.</span><span class="sxs-lookup"><span data-stu-id="679b1-112">The Active Directory item appears in the left navigation menu when any of the following conditions is true.</span></span> <span data-ttu-id="679b1-113">Ellenkező esetben az elem nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="679b1-113">Otherwise, the item does not appear.</span></span>

* <span data-ttu-id="679b1-114">Az aktuális felhasználó feliratkozva a Microsoft-fiók (korábbi nevén Windows Live ID).</span><span class="sxs-lookup"><span data-stu-id="679b1-114">The current user signed on with a Microsoft account (formerly known as a Windows Live ID).</span></span>
  
    <span data-ttu-id="679b1-115">VAGY</span><span class="sxs-lookup"><span data-stu-id="679b1-115">OR</span></span>
* <span data-ttu-id="679b1-116">Az Azure-bérlőhöz tartozik egy könyvtárat, és a jelenlegi fiókot a címtár rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="679b1-116">The Azure tenant has a directory and the current account is a directory administrator.</span></span>
  
    <span data-ttu-id="679b1-117">VAGY</span><span class="sxs-lookup"><span data-stu-id="679b1-117">OR</span></span>
* <span data-ttu-id="679b1-118">Az Azure-bérlőhöz legalább egy Azure AD-hozzáférés-vezérlés (ACS) névtérrel.</span><span class="sxs-lookup"><span data-stu-id="679b1-118">The Azure tenant has at least one Azure AD Access Control (ACS) namespace.</span></span> <span data-ttu-id="679b1-119">További információkért lásd: [hozzáférés-vezérlési Namespace](https://msdn.microsoft.com/library/azure/gg185908.aspx).</span><span class="sxs-lookup"><span data-stu-id="679b1-119">For more information, see [Access Control Namespace](https://msdn.microsoft.com/library/azure/gg185908.aspx).</span></span>
  
    <span data-ttu-id="679b1-120">VAGY</span><span class="sxs-lookup"><span data-stu-id="679b1-120">OR</span></span>
* <span data-ttu-id="679b1-121">Az Azure-bérlőhöz van legalább egy Azure multi-factor Authentication-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="679b1-121">The Azure tenant has at least one Azure Multi-Factor Authentication provider.</span></span> <span data-ttu-id="679b1-122">További információkért lásd: [Administering Azure többtényezős hitelesítési szolgáltatók](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="679b1-122">For more information, see [Administering Azure Multi-Factor Authentication Providers](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).</span></span>

<span data-ttu-id="679b1-123">Egy hozzáférés-vezérlés névtér vagy egy többtényezős hitelesítési szolgáltató létrehozásához kattintson a **+ új** > **alkalmazásszolgáltatások** > **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="679b1-123">To create an Access Control namespace or a Multi-Factor Authentication provider, click **+New** > **App Services** > **Active Directory**.</span></span>

<span data-ttu-id="679b1-124">Ahhoz, hogy a rendszergazdai jogosultságokat biztosít egy könyvtárat, kérjen meg egy rendszergazdát egy rendszergazdai szerepkört rendelni a fiókot.</span><span class="sxs-lookup"><span data-stu-id="679b1-124">To get administrative rights to a directory, have an administrator assign an administrator role to your account.</span></span> <span data-ttu-id="679b1-125">További információkért lásd: [rendszergazdai szerepkörök hozzárendelése](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="679b1-125">For details, see [Assigning administrator roles](active-directory-assign-admin-roles.md).</span></span>

## <a name="active-directory-is-not-available"></a><span data-ttu-id="679b1-126">Active Directory nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="679b1-126">Active Directory is not available</span></span>
<span data-ttu-id="679b1-127">Amikor rákattint **+ új** > **alkalmazásszolgáltatások**, egy **Active Directory** elem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="679b1-127">When you click **+New** > **App Services**, an **Active Directory** item appears.</span></span> <span data-ttu-id="679b1-128">Pontosabban az Active Directory elem jelenik meg, ha, Directory, hozzáférés-vezérlés és többtényezős hitelesítésszolgáltató, például az Active Directory-szolgáltatások bármelyike nem érhető el az aktuális felhasználónak.</span><span class="sxs-lookup"><span data-stu-id="679b1-128">Specifically, the Active Directory item appears when any of the Active Directory features, such as Directory, Access Control, or Multi-Factor Auth Provider, are available to the current user.</span></span>

<span data-ttu-id="679b1-129">Azonban az oldal betöltése, amíg az elem nem aktív, és van megjelölve **nem érhető el**.</span><span class="sxs-lookup"><span data-stu-id="679b1-129">However, while the page is loading, the item is dimmed and is marked **Not Available**.</span></span> <span data-ttu-id="679b1-130">Ez egy ideiglenes állapot.</span><span class="sxs-lookup"><span data-stu-id="679b1-130">This is a temporary state.</span></span> <span data-ttu-id="679b1-131">Várjon néhány másodpercet, ha a cikk elérhetővé válik.</span><span class="sxs-lookup"><span data-stu-id="679b1-131">If you wait a few seconds, the item becomes available.</span></span> <span data-ttu-id="679b1-132">Ha a késés meghosszabbítják, gyakran frissíteni a weblap megoldja a problémát.</span><span class="sxs-lookup"><span data-stu-id="679b1-132">If the delay is prolonged, refreshing the web page often resolves the problem.</span></span>

![Képernyőfelvétel: az Active Directory nem érhető el.](./media/active-directory-troubleshooting/not-available.png)

