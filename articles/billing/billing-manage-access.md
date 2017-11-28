---
title: "aaaManage hozzáférés tooAzure számlázási szerepkörök használatával |} Microsoft Docs"
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
ms.openlocfilehash: 5937fac5ffa5ca204eb03a1dcbc5e800b3d5eb74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-access-toobilling-information-for-azure-using-role-based-access-control"></a><span data-ttu-id="773b4-102">Az Azure szerepköralapú hozzáférés-vezérlés használatával hozzáférést toobilling információk kezelése</span><span class="sxs-lookup"><span data-stu-id="773b4-102">Manage access toobilling information for Azure using role-based access control</span></span>

<span data-ttu-id="773b4-103">Az Azure számlázási információkat toomembers a csoport hozzáférést biztosíthat a következő felhasználói szerepkörök tooyour előfizetés hello hozzárendelésével: Fiókadminisztrátor, szolgáltatás-rendszergazdát, társadminisztrátoraként, tulajdonos, közreműködő, olvasó és számlázási olvasó.</span><span class="sxs-lookup"><span data-stu-id="773b4-103">You can grant access for Azure billing information toomembers of your team by assigning one of hello following user roles tooyour subscription: Account Administrator, Service Administrator, Co-administrator, Owner, Contributor, Reader, and Billing Reader.</span></span> <span data-ttu-id="773b4-104">Akkor toobilling adatok eléréséhez a hello [Azure-portálon](https://portal.azure.com/), és használhatják a hello [számlázási API-k](billing-usage-rate-card-overview.md) tooprogrammatically le a számlák (egyszer engedélyezte, bejövő) és a használat részleteiről.</span><span class="sxs-lookup"><span data-stu-id="773b4-104">They would have access toobilling information in hello [Azure portal](https://portal.azure.com/), and they can use hello [Billing APIs](billing-usage-rate-card-overview.md) tooprogrammatically get invoices (once opted-in) and usage details.</span></span> <span data-ttu-id="773b4-105">További információ szerepkörök adhatnak ki, és mely szerepköröket is mi, olvassa el [szerepkörök az Azure RBAC](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="773b4-105">For more information about who can grant roles, and which roles can do what, see [Roles in Azure RBAC](../active-directory/role-based-access-built-in-roles.md).</span></span>

## <span data-ttu-id="773b4-106"><a name="opt-in"></a>További felhasználók tooaccess számlák engedélyezése</span><span class="sxs-lookup"><span data-stu-id="773b4-106"><a name="opt-in"></a> Allowing additional users tooaccess invoices</span></span>

<span data-ttu-id="773b4-107">hello Fiókadminisztrátort bírálhatja felül a hello használatával [Azure-portálon](https://portal.azure.com/) lehetővé teszik a hozzáférést tooinvoices más felhasználók számára, és API-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="773b4-107">hello Account Administrator must opt in using hello [Azure portal](https://portal.azure.com/) allow access tooinvoices for other users and via API.</span></span>

1. <span data-ttu-id="773b4-108">Hello fiók rendszergazdájához, akkor jelölje ki az előfizetés a hello [előfizetések panel](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="773b4-108">As hello Account Administrator, select your subscription from hello [Subscriptions blade](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in Azure portal.</span></span>

1. <span data-ttu-id="773b4-109">Válassza ki **számlák** , majd **tooinvoices hozzáférési**.</span><span class="sxs-lookup"><span data-stu-id="773b4-109">Select **Invoices** and then **Access tooinvoices**.</span></span>

    ![Képernyőfelvételen látható, hogyan férhetnek hozzá a toodelegate tooinvoices](./media/billing-manage-access/AA-optin.png)

1. <span data-ttu-id="773b4-111">Kapcsolja be **a** hello hozzáférés követ hello változások, az előfizetés hatókörbe tartozó szerepkörök toodownload számlán tooallow felhasználók mentése.</span><span class="sxs-lookup"><span data-stu-id="773b4-111">Turn **On** hello access followed by saving hello changes, tooallow users in subscription scoped roles toodownload invoice.</span></span>

    ![Képernyőfelvétel a ki-toodelegate hozzáférés tooinvoice](./media/billing-manage-access/AA-optinAllow.png)

<span data-ttu-id="773b4-113">Engedélyezés lehetővé teszi az szolgáltatás-rendszergazdát, társadminisztrátoraként, tulajdonos, közreműködő, olvasó és számlázási olvasó hello előfizetés toodownload PDF számlák hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="773b4-113">Opting in allows Service Administrator, Co-administrator, Owner, Contributor, Reader, and Billing Reader on hello subscription toodownload PDF invoices in hello Azure portal.</span></span> <span data-ttu-id="773b4-114">Azonban régebbi, mint a 2016. December számlák egyelőre nem érhető el egyetlen toohello fiók rendszergazdájához.</span><span class="sxs-lookup"><span data-stu-id="773b4-114">However, invoices older than December 2016 are available only toohello Account Administrator for now.</span></span>

<span data-ttu-id="773b4-115">hello Fiókadminisztrátort is konfigurálhatja, e-mailben elküldött toohave számlákat.</span><span class="sxs-lookup"><span data-stu-id="773b4-115">hello Account Administrator can also configure toohave invoices sent via email.</span></span> <span data-ttu-id="773b4-116">toolearn több, lásd: [e-mailben a számla beolvasása](billing-download-azure-invoice-daily-usage-date.md).</span><span class="sxs-lookup"><span data-stu-id="773b4-116">toolearn more, see [Get your invoice in email](billing-download-azure-invoice-daily-usage-date.md).</span></span>

## <a name="adding-users-toohello-billing-reader-role"></a><span data-ttu-id="773b4-117">Felhasználók toohello számlázási olvasó szerepkör hozzáadása</span><span class="sxs-lookup"><span data-stu-id="773b4-117">Adding users toohello Billing Reader role</span></span>

<span data-ttu-id="773b4-118">hello számlázási olvasó szerepkört a csak olvasási hozzáféréssel toosubscription számlázási adatokat az Azure portálon, és nincs hozzáférési tooservices, például a virtuális gépek és tárfiókok rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="773b4-118">hello Billing Reader role has read-only access toosubscription billing information in Azure portal, and no access tooservices such as VMs and storage accounts.</span></span> <span data-ttu-id="773b4-119">Rendelje hozzá a hello számlázási olvasó szerepkört toosomeone kell toohello előfizetés számlázási adatok eléréséhez, de nem hello képességét toomanage Azure szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="773b4-119">Assign hello Billing Reader role toosomeone that needs access toohello subscription billing information but not hello ability toomanage Azure services.</span></span> <span data-ttu-id="773b4-120">Ez a szerepkör azon egy szervezet számára, akik csak Azure-előfizetések végrehajtani a pénzügyi és költség-kezelés megfelelő.</span><span class="sxs-lookup"><span data-stu-id="773b4-120">This role is appropriate for users in an organization who only perform financial and cost management for Azure subscriptions.</span></span>

1. <span data-ttu-id="773b4-121">Jelölje ki az előfizetését a hello [előfizetések panel](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="773b4-121">Select your subscription from hello [Subscriptions blade](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) in Azure portal.</span></span>

1. <span data-ttu-id="773b4-122">Válassza ki **hozzáférés-vezérlés (IAM)** majd **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="773b4-122">Select **Access control (IAM)** and then click **Add**.</span></span>

    ![Képernyőfelvétel IAM hello előfizetés panelen](./media/billing-manage-access/select-iam.PNG)

1. <span data-ttu-id="773b4-124">Válasszon **számlázási olvasó** a hello **Szerepkörválasztás** lap.</span><span class="sxs-lookup"><span data-stu-id="773b4-124">Choose **Billing Reader** in hello **Select a role** page.</span></span>

    ![Képernyőfelvétel a hello helyi menü Nézet számlázási olvasó](./media/billing-manage-access/select-roles.PNG)

1. <span data-ttu-id="773b4-126">Írja be az üdvözlő e-mail hello felhasználói tooinvite szeretné, majd kattintson a **OK** toosend hello meghívó.</span><span class="sxs-lookup"><span data-stu-id="773b4-126">Type hello email for hello user you want tooinvite, then click **OK** toosend hello invitation.</span></span>

    ![Képernyőkép a tooenter e-mail tooinvite valaki](./media/billing-manage-access/add-user.PNG)

1. <span data-ttu-id="773b4-128">Kövesse a témakör utasításait: hello a meghívás e-mail toolog a, a számlázási olvasó.</span><span class="sxs-lookup"><span data-stu-id="773b4-128">Follow instructions in hello invite email toolog in as a Billing Reader.</span></span>

    ![Képernyőkép a számlázási olvasó hello mit láthatnak a Azure-portálon](./media/billing-manage-access/billing-reader-view.png)

> [!NOTE]
> <span data-ttu-id="773b4-130">hello számlázási olvasó a szolgáltatás jelenleg előzetes verzióban érhető és egyelőre nem támogatják a vállalati (EA) előfizetések vagy nem globális felhők.</span><span class="sxs-lookup"><span data-stu-id="773b4-130">hello Billing Reader feature is in preview, and does not yet support enterprise (EA) subscriptions or non-global clouds.</span></span>

## <a name="adding-users-tooother-roles"></a><span data-ttu-id="773b4-131">Felhasználók tooother szerepkörök hozzáadása</span><span class="sxs-lookup"><span data-stu-id="773b4-131">Adding users tooother roles</span></span>

<span data-ttu-id="773b4-132">Más szerepkörök, például a tulajdonos vagy közreműködő szerepkörrel, a felhasználók férhetnek hozzá csak számlázási adatokat, de az Azure-szolgáltatásokat is.</span><span class="sxs-lookup"><span data-stu-id="773b4-132">Users in other roles, such as Owner or Contributor, can access not just billing information, but Azure services as well.</span></span> <span data-ttu-id="773b4-133">toomanage ezeket a szerepköröket, lásd: [kezelése hello előfizetés vagy szolgáltatások hozzáadása vagy módosítása Azure-rendszergazdai szerepkörök](billing-add-change-azure-subscription-administrator.md).</span><span class="sxs-lookup"><span data-stu-id="773b4-133">toomanage these roles, see [Add or change Azure administrator roles that manage hello subscription or services](billing-add-change-azure-subscription-administrator.md).</span></span>

## <a name="who-can-access-hello-account-centerhttpsaccountwindowsazurecom"></a><span data-ttu-id="773b4-134">Hello hozzáférő felhasználók [Account Center](https://account.windowsazure.com)?</span><span class="sxs-lookup"><span data-stu-id="773b4-134">Who can access hello [Account Center](https://account.windowsazure.com)?</span></span>

<span data-ttu-id="773b4-135">Csak a Fiókadminisztrátor hello toohello Account center bejelentkezhet.</span><span class="sxs-lookup"><span data-stu-id="773b4-135">Only hello Account Administrator can log in toohello Account center.</span></span> <span data-ttu-id="773b4-136">hello Fiókadminisztrátort hello előfizetés hello jogi tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="773b4-136">hello Account Administrator is hello legal owner of hello subscription.</span></span> <span data-ttu-id="773b4-137">Alapértelmezés szerint a hello személy, aki regisztrált a vagy vásárolt Azure-előfizetés hello hello fiók rendszergazdájához,, kivéve, ha hello [előfizetés tulajdonjogának áthelyezése történt](billing-subscription-transfer.md) toosomebody más.</span><span class="sxs-lookup"><span data-stu-id="773b4-137">By default, hello person who signed up for or bought hello Azure subscription is hello Account Administrator, unless hello [subscription ownership was transferred](billing-subscription-transfer.md) toosomebody else.</span></span> <span data-ttu-id="773b4-138">hello Fiókadminisztrátort előfizetések létrehozása, előfizetések megszakíthatja, módosítsa hello számlázási címét az előfizetéshez, és hello előfizetés hozzáférési házirendek kezelése.</span><span class="sxs-lookup"><span data-stu-id="773b4-138">hello Account Administrator can create subscriptions, cancel subscriptions, change hello billing address for a subscription, and manage access policies for hello subscription.</span></span>

## <a name="need-help-contact-support"></a><span data-ttu-id="773b4-139">Segítség</span><span class="sxs-lookup"><span data-stu-id="773b4-139">Need help?</span></span> <span data-ttu-id="773b4-140">Forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="773b4-140">Contact support.</span></span>

<span data-ttu-id="773b4-141">Ha további kérdései további, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma megoldódik gyorsan tooget.</span><span class="sxs-lookup"><span data-stu-id="773b4-141">If you still have further questions, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget your issue resolved quickly.</span></span>
