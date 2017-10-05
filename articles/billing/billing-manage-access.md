---
title: "Azure számlázás szerepkörök használatával való hozzáférés kezelése |} Microsoft Docs"
description: 
services: 
documentationcenter: 
author: vikramdesai01
manager: vikdesai
editor: 
tags: billing
ms.assetid: e4c4d136-2826-4938-868f-a7e67ff6b025
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: vikdesai
ms.openlocfilehash: c70904097f139bc2178feed83f1cf1274f3c738d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="manage-access-to-billing-information-for-azure-using-role-based-access-control"></a><span data-ttu-id="d4579-102">Számlázási adatokat az Azure szerepköralapú hozzáférés-vezérlés használatával való hozzáférés kezelése</span><span class="sxs-lookup"><span data-stu-id="d4579-102">Manage access to billing information for Azure using role-based access control</span></span>

<span data-ttu-id="d4579-103">Ön hozzáférést biztosíthat az Azure számlázási adatokat a csoport tagjai számára úgy egyet az alábbi felhasználói szerepkörök hozzárendelése az előfizetéshez: Fiókadminisztrátor, szolgáltatás-rendszergazdát, társadminisztrátoraként, tulajdonos, közreműködő, olvasó és számlázási olvasó.</span><span class="sxs-lookup"><span data-stu-id="d4579-103">You can grant access for Azure billing information to members of your team by assigning one of the following user roles to your subscription: Account Administrator, Service Administrator, Co-administrator, Owner, Contributor, Reader, and Billing Reader.</span></span> <span data-ttu-id="d4579-104">Férhet számlázási adatokhoz a [Azure-portálon](https://portal.azure.com/), és használhatják a [számlázási API-k](billing-usage-rate-card-overview.md) programozott módon le a számlák (egyszer engedélyezte, bejövő) és a használat részleteiről.</span><span class="sxs-lookup"><span data-stu-id="d4579-104">They would have access to billing information in the [Azure portal](https://portal.azure.com/), and they can use the [Billing APIs](billing-usage-rate-card-overview.md) to programmatically get invoices (once opted-in) and usage details.</span></span> <span data-ttu-id="d4579-105">További információ szerepkörök adhatnak ki, és mely szerepköröket is mi, olvassa el [szerepkörök az Azure RBAC](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="d4579-105">For more information about who can grant roles, and which roles can do what, see [Roles in Azure RBAC](../active-directory/role-based-access-built-in-roles.md).</span></span>

## <span data-ttu-id="d4579-106"><a name="opt-in"></a>Felhasználó számlák eléréséhez</span><span class="sxs-lookup"><span data-stu-id="d4579-106"><a name="opt-in"></a> Allowing additional users to access invoices</span></span>

<span data-ttu-id="d4579-107">A Fiókadminisztrátor használatával bírálhatja felül a [Azure-portálon](https://portal.azure.com/) számlák a hozzáférést, a más felhasználók számára, és API-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="d4579-107">The Account Administrator must opt in using the [Azure portal](https://portal.azure.com/) allow access to invoices for other users and via API.</span></span>

1. <span data-ttu-id="d4579-108">A fiók rendszergazdaként, válassza ki az előfizetést a [előfizetések panel](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="d4579-108">As the Account Administrator, select your subscription from the [Subscriptions blade](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in Azure portal.</span></span>

1. <span data-ttu-id="d4579-109">Válassza ki **számlák** , majd **számlák elérésére**.</span><span class="sxs-lookup"><span data-stu-id="d4579-109">Select **Invoices** and then **Access to invoices**.</span></span>

    ![Képernyőkép bemutatja, hogyan adhat hozzáférést számlák](./media/billing-manage-access/AA-optin.png)

1. <span data-ttu-id="d4579-111">Kapcsolja be **a** a hozzáférés a felhasználó az előfizetéshez, a módosítások mentése után töltse le a számla szerepkörök hatóköre.</span><span class="sxs-lookup"><span data-stu-id="d4579-111">Turn **On** the access followed by saving the changes, to allow users in subscription scoped roles to download invoice.</span></span>

    ![Képernyőfelvétel a ki-és delegálja a számla elérésére](./media/billing-manage-access/AA-optinAllow.png)

<span data-ttu-id="d4579-113">Engedélyezés lehetővé teszi, hogy szolgáltatás-rendszergazdát, társadminisztrátoraként, tulajdonos, közreműködő, olvasó és számlázási olvasó letölteni az Azure portálon PDF számlák előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="d4579-113">Opting in allows Service Administrator, Co-administrator, Owner, Contributor, Reader, and Billing Reader on the subscription to download PDF invoices in the Azure portal.</span></span> <span data-ttu-id="d4579-114">Azonban régebbi, mint a 2016. December számlák érhetők el csak az a fiók rendszergazdája most.</span><span class="sxs-lookup"><span data-stu-id="d4579-114">However, invoices older than December 2016 are available only to the Account Administrator for now.</span></span>

<span data-ttu-id="d4579-115">A Fiókadminisztrátor is konfigurálhatja az e-mailben küldött számlákat.</span><span class="sxs-lookup"><span data-stu-id="d4579-115">The Account Administrator can also configure to have invoices sent via email.</span></span> <span data-ttu-id="d4579-116">További tudnivalókért lásd: [e-mailben a számla beolvasása](billing-download-azure-invoice-daily-usage-date.md).</span><span class="sxs-lookup"><span data-stu-id="d4579-116">To learn more, see [Get your invoice in email](billing-download-azure-invoice-daily-usage-date.md).</span></span>

## <a name="adding-users-to-the-billing-reader-role"></a><span data-ttu-id="d4579-117">Felhasználók hozzáadása a számlázási olvasó szerepkört</span><span class="sxs-lookup"><span data-stu-id="d4579-117">Adding users to the Billing Reader role</span></span>

<span data-ttu-id="d4579-118">A számlázási olvasó szerepkört az előfizetés számlázási adatokat az Azure portál csak olvasási hozzáféréssel, és nem érhető el a szolgáltatásokat, például virtuális gépek és tárfiókok rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d4579-118">The Billing Reader role has read-only access to subscription billing information in Azure portal, and no access to services such as VMs and storage accounts.</span></span> <span data-ttu-id="d4579-119">A számlázási olvasó szerepkör hozzárendelése valaki hozzá kell férnie az előfizetés számlázási adatokat, de nem képes kezelni az Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="d4579-119">Assign the Billing Reader role to someone that needs access to the subscription billing information but not the ability to manage Azure services.</span></span> <span data-ttu-id="d4579-120">Ez a szerepkör azon egy szervezet számára, akik csak Azure-előfizetések végrehajtani a pénzügyi és költség-kezelés megfelelő.</span><span class="sxs-lookup"><span data-stu-id="d4579-120">This role is appropriate for users in an organization who only perform financial and cost management for Azure subscriptions.</span></span>

1. <span data-ttu-id="d4579-121">Válassza ki az előfizetést a [előfizetések panel](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="d4579-121">Select your subscription from the [Subscriptions blade](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in Azure portal.</span></span>

1. <span data-ttu-id="d4579-122">Válassza ki **hozzáférés-vezérlés (IAM)** majd **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="d4579-122">Select **Access control (IAM)** and then click **Add**.</span></span>

    ![Képernyőfelvétel az előfizetés panelen IAM](./media/billing-manage-access/select-iam.PNG)

1. <span data-ttu-id="d4579-124">Válasszon **számlázási olvasó** a a **Szerepkörválasztás** lap.</span><span class="sxs-lookup"><span data-stu-id="d4579-124">Choose **Billing Reader** in the **Select a role** page.</span></span>

    ![Képernyőfelvétel a helyi menü Nézet számlázási olvasó](./media/billing-manage-access/select-roles.PNG)

1. <span data-ttu-id="d4579-126">Írja be az e-mailt a meghívót, majd kattintson a kívánt felhasználó **OK** a meghívó elküldésére.</span><span class="sxs-lookup"><span data-stu-id="d4579-126">Type the email for the user you want to invite, then click **OK** to send the invitation.</span></span>

    ![Adja meg e-mail címét meghívni a képernyőkép](./media/billing-manage-access/add-user.PNG)

1. <span data-ttu-id="d4579-128">Kövesse a meghívott felhasználó e-mailben szereplő utasításokat Jelentkezzen be egy számlázási olvasó.</span><span class="sxs-lookup"><span data-stu-id="d4579-128">Follow instructions in the invite email to log in as a Billing Reader.</span></span>

    ![Képernyőkép a számlázási olvasó csoportszűrést Azure-portálon](./media/billing-manage-access/billing-reader-view.png)

> [!NOTE]
> <span data-ttu-id="d4579-130">A számlázási olvasó jelenleg előzetes verzióban érhető vagy szolgáltatás egyelőre nem támogatják a vállalati (EA) előfizetések vagy nem globális felhők.</span><span class="sxs-lookup"><span data-stu-id="d4579-130">The Billing Reader feature is in preview, and does not yet support enterprise (EA) subscriptions or non-global clouds.</span></span>

## <a name="adding-users-to-other-roles"></a><span data-ttu-id="d4579-131">Felhasználók más szerepkörök hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d4579-131">Adding users to other roles</span></span>

<span data-ttu-id="d4579-132">Más szerepkörök, például a tulajdonos vagy közreműködő szerepkörrel, a felhasználók férhetnek hozzá csak számlázási adatokat, de az Azure-szolgáltatásokat is.</span><span class="sxs-lookup"><span data-stu-id="d4579-132">Users in other roles, such as Owner or Contributor, can access not just billing information, but Azure services as well.</span></span> <span data-ttu-id="d4579-133">Ezek a szerepkörök kezeléséhez, tekintse meg a [hozzáadása vagy módosítása, hogy az előfizetés vagy a szolgáltatások kezelése az Azure rendszergazdai szerepkörök](billing-add-change-azure-subscription-administrator.md).</span><span class="sxs-lookup"><span data-stu-id="d4579-133">To manage these roles, see [Add or change Azure administrator roles that manage the subscription or services](billing-add-change-azure-subscription-administrator.md).</span></span>

## <a name="who-can-access-the-account-centerhttpsaccountwindowsazurecom"></a><span data-ttu-id="d4579-134">Ki férhet hozzá a [Account Center](https://account.windowsazure.com)?</span><span class="sxs-lookup"><span data-stu-id="d4579-134">Who can access the [Account Center](https://account.windowsazure.com)?</span></span>

<span data-ttu-id="d4579-135">Csak a Fiókadminisztrátor az Account center bejelentkezhet.</span><span class="sxs-lookup"><span data-stu-id="d4579-135">Only the Account Administrator can log in to the Account center.</span></span> <span data-ttu-id="d4579-136">A Fiókadminisztrátor az előfizetés jogi tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="d4579-136">The Account Administrator is the legal owner of the subscription.</span></span> <span data-ttu-id="d4579-137">Alapértelmezés szerint a a személy, aki regisztrált a vagy vásárolt Azure-előfizetést a Fiókadminisztrátor, kivéve, ha a [előfizetés tulajdonjogának áthelyezése történt](billing-subscription-transfer.md) valaki másnak.</span><span class="sxs-lookup"><span data-stu-id="d4579-137">By default, the person who signed up for or bought the Azure subscription is the Account Administrator, unless the [subscription ownership was transferred](billing-subscription-transfer.md) to somebody else.</span></span> <span data-ttu-id="d4579-138">A Fiókadminisztrátor előfizetések létrehozása, előfizetések megszakíthatja, módosítsa az előfizetés számlázási címét, és hozzáférési házirendek az előfizetés kezelése.</span><span class="sxs-lookup"><span data-stu-id="d4579-138">The Account Administrator can create subscriptions, cancel subscriptions, change the billing address for a subscription, and manage access policies for the subscription.</span></span>

## <a name="need-help-contact-support"></a><span data-ttu-id="d4579-139">Segítség</span><span class="sxs-lookup"><span data-stu-id="d4579-139">Need help?</span></span> <span data-ttu-id="d4579-140">Forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="d4579-140">Contact support.</span></span>

<span data-ttu-id="d4579-141">Ha további kérdései további, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma elhárítva gyors eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="d4579-141">If you still have further questions, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your issue resolved quickly.</span></span>
