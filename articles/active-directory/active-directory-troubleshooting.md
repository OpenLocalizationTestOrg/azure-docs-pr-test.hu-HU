---
title: "Hibaelhárítás: \"Active Directory\" elem nem található vagy nem érhető el |} Microsoft Docs"
description: "Milyen toodo, amikor az Active Directory menüpont hello Azure felügyeleti portál nem jelenik meg."
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
ms.openlocfilehash: d7355a4e39141f0b09272dc5615c309b23c8c70f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-active-directory-item-is-missing-or-not-available"></a><span data-ttu-id="84b90-103">Hibaelhárítás: "Active Directory" elem nem található vagy nem érhető el</span><span class="sxs-lookup"><span data-stu-id="84b90-103">Troubleshooting: 'Active Directory' item is missing or not available</span></span>
<span data-ttu-id="84b90-104">Hello utasítások az Azure Active Directory-szolgáltatások és szolgáltatások használatával számos kezdődhet "nyissa meg az Azure felügyeleti portálon toohello és **Active Directory**."</span><span class="sxs-lookup"><span data-stu-id="84b90-104">Many of hello instructions for using Azure Active Directory features and services begin with "Go toohello Azure Management Portal and click **Active Directory**."</span></span> <span data-ttu-id="84b90-105">Mi a teendő ha hello Active Directory kiterjesztés vagy a menü elem nem jelenik meg, vagy ha meg van jelölve, de **nem érhető el**?</span><span class="sxs-lookup"><span data-stu-id="84b90-105">But what do you do if hello Active Directory extension or menu item does not appear or if it is marked **Not Available**?</span></span> <span data-ttu-id="84b90-106">Ez a témakör a tervezett toohelp.</span><span class="sxs-lookup"><span data-stu-id="84b90-106">This topic is designed toohelp.</span></span> <span data-ttu-id="84b90-107">Ismerteti, hogyan hello feltételeket, amelyek alapján **Active Directory** nem jelenik meg vagy nem érhető el, és elmagyarázza, hogyan tooproceed.</span><span class="sxs-lookup"><span data-stu-id="84b90-107">It describes hello conditions under which **Active Directory** does not appear or is unavailable and explains how tooproceed.</span></span>

## <a name="active-directory-is-missing"></a><span data-ttu-id="84b90-108">Az Active Directory hiányzik.</span><span class="sxs-lookup"><span data-stu-id="84b90-108">Active Directory is missing</span></span>
<span data-ttu-id="84b90-109">Általában egy **Active Directory** hello bal oldali navigációs menü elem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="84b90-109">Typically, an **Active Directory** item appears in hello left navigation menu.</span></span> <span data-ttu-id="84b90-110">Azure Active Directory eljárások hello utasítások feltételezik, hogy ezt az elemet a nézetben.</span><span class="sxs-lookup"><span data-stu-id="84b90-110">hello instructions in Azure Active Directory procedures assume that this item is in your view.</span></span>

![Képernyőfelvétel: az Azure Active Directory](./media/active-directory-troubleshooting/typical-view.png)

<span data-ttu-id="84b90-112">hello bal oldali navigációs menü hello Active Directory elem jelenik meg, ha hello a következő feltételek bármelyike teljesül.</span><span class="sxs-lookup"><span data-stu-id="84b90-112">hello Active Directory item appears in hello left navigation menu when any of hello following conditions is true.</span></span> <span data-ttu-id="84b90-113">Ellenkező esetben hello elem nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="84b90-113">Otherwise, hello item does not appear.</span></span>

* <span data-ttu-id="84b90-114">(korábbi nevén Windows Live ID) Microsoft-fiókkal bejelentkezve hello aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="84b90-114">hello current user signed on with a Microsoft account (formerly known as a Windows Live ID).</span></span>
  
    <span data-ttu-id="84b90-115">VAGY</span><span class="sxs-lookup"><span data-stu-id="84b90-115">OR</span></span>
* <span data-ttu-id="84b90-116">hello Azure-bérlőhöz tartozik egy könyvtárat, valamint hello jelenlegi fiókot directory rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="84b90-116">hello Azure tenant has a directory and hello current account is a directory administrator.</span></span>
  
    <span data-ttu-id="84b90-117">VAGY</span><span class="sxs-lookup"><span data-stu-id="84b90-117">OR</span></span>
* <span data-ttu-id="84b90-118">Azure-bérlőhöz hello legalább egy Azure AD-hozzáférés-vezérlés (ACS) névtérrel.</span><span class="sxs-lookup"><span data-stu-id="84b90-118">hello Azure tenant has at least one Azure AD Access Control (ACS) namespace.</span></span> <span data-ttu-id="84b90-119">További információkért lásd: [hozzáférés-vezérlési Namespace](https://msdn.microsoft.com/library/azure/gg185908.aspx).</span><span class="sxs-lookup"><span data-stu-id="84b90-119">For more information, see [Access Control Namespace](https://msdn.microsoft.com/library/azure/gg185908.aspx).</span></span>
  
    <span data-ttu-id="84b90-120">VAGY</span><span class="sxs-lookup"><span data-stu-id="84b90-120">OR</span></span>
* <span data-ttu-id="84b90-121">Azure-bérlőhöz hello van legalább egy Azure multi-factor Authentication-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="84b90-121">hello Azure tenant has at least one Azure Multi-Factor Authentication provider.</span></span> <span data-ttu-id="84b90-122">További információkért lásd: [Administering Azure többtényezős hitelesítési szolgáltatók](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="84b90-122">For more information, see [Administering Azure Multi-Factor Authentication Providers](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).</span></span>

<span data-ttu-id="84b90-123">toocreate egy hozzáférés-vezérlés névtér vagy egy többtényezős hitelesítési szolgáltató kattintson **+ új** > **alkalmazásszolgáltatások** > **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="84b90-123">toocreate an Access Control namespace or a Multi-Factor Authentication provider, click **+New** > **App Services** > **Active Directory**.</span></span>

<span data-ttu-id="84b90-124">tooget rendszergazdai jogosultságokkal tooa directory rendelkezik egy rendszergazda rendelje hozzá a rendszergazda szerepkör tooyour fiók.</span><span class="sxs-lookup"><span data-stu-id="84b90-124">tooget administrative rights tooa directory, have an administrator assign an administrator role tooyour account.</span></span> <span data-ttu-id="84b90-125">További információkért lásd: [rendszergazdai szerepkörök hozzárendelése](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="84b90-125">For details, see [Assigning administrator roles](active-directory-assign-admin-roles.md).</span></span>

## <a name="active-directory-is-not-available"></a><span data-ttu-id="84b90-126">Active Directory nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="84b90-126">Active Directory is not available</span></span>
<span data-ttu-id="84b90-127">Amikor rákattint **+ új** > **alkalmazásszolgáltatások**, egy **Active Directory** elem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="84b90-127">When you click **+New** > **App Services**, an **Active Directory** item appears.</span></span> <span data-ttu-id="84b90-128">Pontosabban a hello Active Directory elem jelenik meg, amikor hello Active Directory-szolgáltatások Directory, hozzáférés-vezérlés és többtényezős hitelesítésszolgáltató, például bármelyike elérhető toohello aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="84b90-128">Specifically, hello Active Directory item appears when any of hello Active Directory features, such as Directory, Access Control, or Multi-Factor Auth Provider, are available toohello current user.</span></span>

<span data-ttu-id="84b90-129">Azonban hello oldal betöltése, amíg hello elem nem aktív, és van megjelölve **nem érhető el**.</span><span class="sxs-lookup"><span data-stu-id="84b90-129">However, while hello page is loading, hello item is dimmed and is marked **Not Available**.</span></span> <span data-ttu-id="84b90-130">Ez egy ideiglenes állapot.</span><span class="sxs-lookup"><span data-stu-id="84b90-130">This is a temporary state.</span></span> <span data-ttu-id="84b90-131">Várjon néhány másodpercet, ha hello elem elérhetővé válik.</span><span class="sxs-lookup"><span data-stu-id="84b90-131">If you wait a few seconds, hello item becomes available.</span></span> <span data-ttu-id="84b90-132">Hello késleltetés meghosszabbítják, ha frissíti hello weblap gyakran megszünteti hello probléma.</span><span class="sxs-lookup"><span data-stu-id="84b90-132">If hello delay is prolonged, refreshing hello web page often resolves hello problem.</span></span>

![Képernyőfelvétel: az Active Directory nem érhető el.](./media/active-directory-troubleshooting/not-available.png)

