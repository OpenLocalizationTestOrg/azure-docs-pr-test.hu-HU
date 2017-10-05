---
title: "Nincs előfizetés hibát talált, amikor megpróbál bejelentkezni az Azure portál vagy az Azure-fiók center |} Microsoft Docs"
description: "A megoldást kínál a probléma, amelyben nem találhatók előfizetések hiba akkor fordul elő, amikor jelentkezzen be Azure-portálon vagy az Azure-fiók center."
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.assetid: d1545298-99db-4941-8e97-f24a06bb7cb6
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: genli
ms.openlocfilehash: a4ce9b219c05f8469379c2aac5241fcfffd16033
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="no-subscriptions-found-error-in-azure-portal-or-azure-account-center"></a><span data-ttu-id="e4578-103">Nem található a hiba az Azure portálon vagy az Azure-fiók center előfizetés.</span><span class="sxs-lookup"><span data-stu-id="e4578-103">No subscriptions found error in Azure portal or Azure account center</span></span>
<span data-ttu-id="e4578-104">A "Nem található előfizetés" hibaüzenetet kaphat, ha megpróbál bejelentkezni a [Azure-portálon](https://portal.azure.com/) vagy a [Azure-fiók center](https://account.windowsazure.com/Subscriptions).</span><span class="sxs-lookup"><span data-stu-id="e4578-104">You might receive a "No subscriptions found" error message when you try to sign in to the [Azure portal](https://portal.azure.com/) or the [Azure account center](https://account.windowsazure.com/Subscriptions).</span></span> <span data-ttu-id="e4578-105">Ez a cikk megoldást kínál a probléma.</span><span class="sxs-lookup"><span data-stu-id="e4578-105">This article provides a solution for this problem.</span></span>

## <a name="symptom"></a><span data-ttu-id="e4578-106">Jelenség</span><span class="sxs-lookup"><span data-stu-id="e4578-106">Symptom</span></span>

<span data-ttu-id="e4578-107">Amikor megpróbál bejelentkezni a [Azure-portálon](https://portal.azure.com/) vagy a [Azure-fiók center](https://account.windowsazure.com/Subscriptions), a következő hibaüzenetet kapja: "Nem található előfizetés".</span><span class="sxs-lookup"><span data-stu-id="e4578-107">When you try to sign in to the [Azure portal](https://portal.azure.com/) or the [Azure account center](https://account.windowsazure.com/Subscriptions), you receive the following error message: "No subscriptions found".</span></span>

## <a name="cause"></a><span data-ttu-id="e4578-108">Ok</span><span class="sxs-lookup"><span data-stu-id="e4578-108">Cause</span></span>

<span data-ttu-id="e4578-109">Ez a probléma akkor fordul elő, ha a fiók nem rendelkezik megfelelő engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="e4578-109">This problem occurs if your account doesn’t have sufficient permissions.</span></span> 

## <a name="solution"></a><span data-ttu-id="e4578-110">Megoldás</span><span class="sxs-lookup"><span data-stu-id="e4578-110">Solution</span></span>

<span data-ttu-id="e4578-111">Győződjön meg arról, hogy jelentkezik be a megfelelő rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="e4578-111">Make sure that you log in as the correct administrator.</span></span> <span data-ttu-id="e4578-112">A fiók rendszergazda csak az Account Center férhetnek hozzá.</span><span class="sxs-lookup"><span data-stu-id="e4578-112">An Account Administrator can access only the Account Center.</span></span> <span data-ttu-id="e4578-113">Szolgáltatás-rendszergazdák (SA) és a Társadminisztrátorok (CA) rendelkezik az engedéllyel csak az Azure-portálon vagy a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="e4578-113">Service Administrators (SA) and Co-Administrators (CA) have access permission only to the Azure portal or the Azure classic portal.</span></span>

### <a name="scenario-1-error-message-is-received-in-the-azure-portalhttpsportalazurecom"></a><span data-ttu-id="e4578-114">1. forgatókönyv: Hibaüzenet érkezett a [Azure-portálon](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="e4578-114">Scenario 1: Error message is received in the [Azure portal](https://portal.azure.com)</span></span>

<span data-ttu-id="e4578-115">A probléma kijavítása:</span><span class="sxs-lookup"><span data-stu-id="e4578-115">To fix this issue:</span></span>

* <span data-ttu-id="e4578-116">Győződjön meg arról, hogy a megfelelő Azure-címtár kattintson a jobb felső sarokban a fiók van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="e4578-116">Make sure that the correct Azure directory is selected by clicking your account at the top right.</span></span>

  ![Válassza ki azt a címtárat a bal felső sarkában az Azure-portálon](./media/billing-no-subscriptions-found/directory-switch.png)

* <span data-ttu-id="e4578-118">Ha a jobb oldali Azure-címtár van kiválasztva, de továbbra is megjelenik a hibaüzenet [fiókját tulajdonos rendelkezik](billing-add-change-azure-subscription-administrator.md).</span><span class="sxs-lookup"><span data-stu-id="e4578-118">If the right Azure directory is selected but you still receive the error message, [have your account added as an Owner](billing-add-change-azure-subscription-administrator.md).</span></span>

### <a name="scenario-2-error-message-is-received-in-the-azure-account-centerhttpsaccountwindowsazurecomsubscriptions"></a><span data-ttu-id="e4578-119">2. forgatókönyv: Hibaüzenet érkezett a [Azure-fiók center](https://account.windowsazure.com/Subscriptions)</span><span class="sxs-lookup"><span data-stu-id="e4578-119">Scenario 2: Error message is received in the [Azure account center](https://account.windowsazure.com/Subscriptions)</span></span>

<span data-ttu-id="e4578-120">Ellenőrizze, hogy a használt fiók a fiók rendszergazdájához.</span><span class="sxs-lookup"><span data-stu-id="e4578-120">Check whether the account that you used is the Account Administrator.</span></span> <span data-ttu-id="e4578-121">A fiók rendszergazdájához, aki ellenőrzéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="e4578-121">To verify who the Account Administrator is, follow these steps:</span></span>

1. <span data-ttu-id="e4578-122">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e4578-122">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="e4578-123">A központ menüben válassza ki a **előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="e4578-123">On the Hub menu, select **Subscription**.</span></span>
3. <span data-ttu-id="e4578-124">Válassza ki az előfizetést, és válassza ki a kívánt **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="e4578-124">Select the subscription that you want to check, and then select **Settings**.</span></span>
4. <span data-ttu-id="e4578-125">Válassza ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="e4578-125">Select **Properties**.</span></span> <span data-ttu-id="e4578-126">A fiókadminisztrátor az előfizetés jelenik meg a **Fiókadminisztrátor** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="e4578-126">The account administrator of the subscription is displayed in the **Account Admin** box.</span></span>

## <a name="need-help-contact-support"></a><span data-ttu-id="e4578-127">Segítség</span><span class="sxs-lookup"><span data-stu-id="e4578-127">Need help?</span></span> <span data-ttu-id="e4578-128">Forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="e4578-128">Contact support.</span></span>
<span data-ttu-id="e4578-129">Ha további segítségre van, [forduljon a támogatási szolgálathoz](http://go.microsoft.com/fwlink/?linkid=544831&clcid=0x409) a probléma elhárítva gyors eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="e4578-129">If you still need help, [contact support](http://go.microsoft.com/fwlink/?linkid=544831&clcid=0x409) to get your issue resolved quickly.</span></span> 
