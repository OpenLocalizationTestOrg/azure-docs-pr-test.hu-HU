---
title: "Hogyan kezelheti a felhasználói fiókokat az Azure API Management |} Microsoft Docs"
description: "Megtudhatja, hogyan hozható létre, vagy a meghívott felhasználóknak az Azure API Management"
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 078abfa5-1e4f-4c9d-b9c7-a172bd19c1a2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: d3a50f6d22cbf1797f580078bc0d2cc9cefe5064
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-user-accounts-in-azure-api-management"></a><span data-ttu-id="e1391-103">Az Azure API Management felhasználói fiókok kezelése</span><span class="sxs-lookup"><span data-stu-id="e1391-103">How to manage user accounts in Azure API Management</span></span>
<span data-ttu-id="e1391-104">Az API Management fejlesztők akkor teszi közzé az API Management használata API-k felhasználók.</span><span class="sxs-lookup"><span data-stu-id="e1391-104">In API Management, developers are the users of the APIs that you expose using API Management.</span></span> <span data-ttu-id="e1391-105">Ez az útmutató bemutatja, hogyan hozhat létre, és a fejlesztők meghívása a az API-k és termékek használatára, hogy elérhetővé számukra az API Management-példánnyal.</span><span class="sxs-lookup"><span data-stu-id="e1391-105">This guide shows to how to create and invite developers to use the APIs and products that you make available to them with your API Management instance.</span></span> <span data-ttu-id="e1391-106">A felhasználói fiókok kezelése programozott módon információkért lásd: a [felhasználói entitás](https://msdn.microsoft.com/library/azure/dn776330.aspx) dokumentációjában találhatók a [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="e1391-106">For information on managing user accounts programmatically, see the [User entity](https://msdn.microsoft.com/library/azure/dn776330.aspx) documentation in the [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) reference.</span></span>

## <span data-ttu-id="e1391-107"><a name="create-developer"></a>Hozzon létre egy új fejlesztő</span><span class="sxs-lookup"><span data-stu-id="e1391-107"><a name="create-developer"> </a>Create a new developer</span></span>
<span data-ttu-id="e1391-108">Hozzon létre egy új fejlesztő, kattintson a **Publisher portal** az Azure portálon az API Management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e1391-108">To create a new developer, click **Publisher portal** in the Azure Portal for your API Management service.</span></span> <span data-ttu-id="e1391-109">Ezzel továbblép az API Management közzétevő portáljára.</span><span class="sxs-lookup"><span data-stu-id="e1391-109">This takes you to the API Management publisher portal.</span></span> <span data-ttu-id="e1391-110">Ha még nem hozott létre API Management szolgáltatáspéldányt, tekintse meg az [Ismerkedés az Azure API Management szolgáltatással][Get started with Azure API Management] oktatóanyag [API Management szolgáltatáspéldány létrehozása][Create an API Management service instance] című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="e1391-110">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>

![Közzétevő portál][api-management-management-console]

<span data-ttu-id="e1391-112">Kattintson a **felhasználók** a a **API Management** a bal oldali menüben, majd **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="e1391-112">Click **Users** from the **API Management** menu on the left, and then click **add user**.</span></span>

![Fejlesztői létrehozása][api-management-create-developer]

<span data-ttu-id="e1391-114">Adja meg a **E-mail**, **jelszó**, és **neve** a új fejlesztői, majd kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="e1391-114">Enter the **Email**, **Password**, and **Name** for the new developer and click **Save**.</span></span>

![Fejlesztői létrehozása][api-management-add-new-user]

<span data-ttu-id="e1391-116">Alapértelmezés szerint az újonnan létrehozott fejlesztői fiókok vannak **aktív**, és a társított a **fejlesztők** csoport.</span><span class="sxs-lookup"><span data-stu-id="e1391-116">By default, newly created developer accounts are **Active**, and associated with the **Developers** group.</span></span>

![Új fejlesztői][api-management-new-developer]

<span data-ttu-id="e1391-118">A fejlesztői fiókok egy **aktív** állapota az API-kat, amelyeknél az előfizetések összes eléréséhez használható.</span><span class="sxs-lookup"><span data-stu-id="e1391-118">Developer accounts that are in an **active** state can be used to access all of the APIs for which they have subscriptions.</span></span> <span data-ttu-id="e1391-119">Az újonnan létrehozott fejlesztői társítandó további csoportokat, lásd: [csoportok társítása fejlesztők][How to associate groups with developers].</span><span class="sxs-lookup"><span data-stu-id="e1391-119">To associate the newly created developer with additional groups, see [How to associate groups with developers][How to associate groups with developers].</span></span>

## <span data-ttu-id="e1391-120"><a name="invite-developer"></a>Egy fejlesztő meghívása</span><span class="sxs-lookup"><span data-stu-id="e1391-120"><a name="invite-developer"> </a>Invite a developer</span></span>
<span data-ttu-id="e1391-121">Egy fejlesztő, kattintson a **felhasználók** a a **API Management** a bal oldali menüben, majd **hívhat meg felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="e1391-121">To invite a developer, click **Users** from the **API Management** menu on the left, and then click **Invite User**.</span></span>

![Fejlesztői meghívása][api-management-invite-developer]

<span data-ttu-id="e1391-123">Adja meg a fejlesztői név és e-mail címét, és kattintson a **meghívása**.</span><span class="sxs-lookup"><span data-stu-id="e1391-123">Enter the name and email address of the developer, and click **Invite**.</span></span>

![Fejlesztői meghívása][api-management-invite-developer-window]

<span data-ttu-id="e1391-125">Egy megerősítő üzenet jelenik meg, de az újonnan meghívott fejlesztői nem szerepelnek a listán csak azok elfogadja a meghívót.</span><span class="sxs-lookup"><span data-stu-id="e1391-125">A confirmation message is displayed, but the newly invited developer does not appear in the list until after they accept the invitation.</span></span> 

![Jóváhagyás meghívása][api-management-invite-developer-confirmation]

<span data-ttu-id="e1391-127">Amikor egy fejlesztő felkérik, egy e-mailt küld a fejlesztőnek.</span><span class="sxs-lookup"><span data-stu-id="e1391-127">When a developer is invited, an email is sent to the developer.</span></span> <span data-ttu-id="e1391-128">Ez az e-mail sablon használatával jön létre, és testre szabható.</span><span class="sxs-lookup"><span data-stu-id="e1391-128">This email is generated using a template and is customizable.</span></span> <span data-ttu-id="e1391-129">További információkért lásd: [konfigurálás e-mail sablonok][Configure email templates].</span><span class="sxs-lookup"><span data-stu-id="e1391-129">For more information, see [Configure email templates][Configure email templates].</span></span>

<span data-ttu-id="e1391-130">Ha elfogadja a meghívót, a fiók aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="e1391-130">Once the invitation is accepted, the account becomes active.</span></span>

## <span data-ttu-id="e1391-131"><a name="block-developer"></a> Inaktiválja vagy újraaktiválásához fejlesztői fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="e1391-131"><a name="block-developer"> </a> Deactivate or reactivate a developer account</span></span>
<span data-ttu-id="e1391-132">Alapértelmezés szerint az újonnan létrehozott vagy a meghívott fejlesztői fiókok vannak **aktív**.</span><span class="sxs-lookup"><span data-stu-id="e1391-132">By default, newly created or invited developer accounts are **Active**.</span></span> <span data-ttu-id="e1391-133">Fejlesztői fiók létrehozása inaktiválásához kattintson **blokk**.</span><span class="sxs-lookup"><span data-stu-id="e1391-133">To deactivate a developer account, click **Block**.</span></span> <span data-ttu-id="e1391-134">Letiltott fejlesztői fiók létrehozása újraaktiválásához kattintson **aktiválás**.</span><span class="sxs-lookup"><span data-stu-id="e1391-134">To reactivate a blocked developer account, click **Activate**.</span></span> <span data-ttu-id="e1391-135">Letiltott fejlesztői fiók létrehozása nem a fejlesztői portálján érheti el és bármely API-k hívása.</span><span class="sxs-lookup"><span data-stu-id="e1391-135">A blocked developer account can not access the developer portal or call any APIs.</span></span> <span data-ttu-id="e1391-136">A felhasználói fiók törléséhez kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="e1391-136">To delete a user account, click **Delete**.</span></span>

![Blokk fejlesztői][api-management-new-developer]

## <a name="reset-a-user-password"></a><span data-ttu-id="e1391-138">Felhasználói jelszó alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="e1391-138">Reset a user password</span></span>
<span data-ttu-id="e1391-139">A felhasználói fiók jelszavának alaphelyzetbe állításához kattintson a fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="e1391-139">To reset the password for a user account, click the name of the account.</span></span>

![Új jelszó létrehozása][api-management-view-developer]

<span data-ttu-id="e1391-141">Kattintson a **jelszó-átállítási** elküld egy hivatkozást a felhasználót, hogy a jelszó alaphelyzetbe állítása.</span><span class="sxs-lookup"><span data-stu-id="e1391-141">Click **Reset password** to send a link to the user to reset their password.</span></span>

![Új jelszó létrehozása][api-management-reset-password]

<span data-ttu-id="e1391-143">Felhasználói fiókok programozott módon dolgozni, tekintse meg a [felhasználói entitás](https://msdn.microsoft.com/library/azure/dn776330.aspx) dokumentációjában találhatók a [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="e1391-143">To programmatically work with user accounts, see the [User entity](https://msdn.microsoft.com/library/azure/dn776330.aspx) documentation in the [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) reference.</span></span> <span data-ttu-id="e1391-144">A felhasználói fiók jelszavának visszaállítása egy adott értékre, használhatja a [egy felhasználó frissítéséhez](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) műveletet, és adja meg a kívánt jelszót.</span><span class="sxs-lookup"><span data-stu-id="e1391-144">To reset a user account password to a specific value, you can use the [Update a user](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) operation and specify the desired password.</span></span>

## <a name="pending-verification"></a><span data-ttu-id="e1391-145">Függőben lévő ellenőrzési</span><span class="sxs-lookup"><span data-stu-id="e1391-145">Pending verification</span></span>
![Függőben lévő ellenőrzési][api-management-pending-verification]

## <span data-ttu-id="e1391-147"><a name="next-steps"> </a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e1391-147"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="e1391-148">A fejlesztői fiók létrehozása után szerepkörökhöz társítandó, és előfizetni termékek és API-kat.</span><span class="sxs-lookup"><span data-stu-id="e1391-148">Once a developer account is created, you can associate it with roles and subscribe it to products and APIs.</span></span> <span data-ttu-id="e1391-149">További információkért lásd: [létrehozásáról és-csoportok használata][How to create and use groups].</span><span class="sxs-lookup"><span data-stu-id="e1391-149">For more information, see [How to create and use groups][How to create and use groups].</span></span>

[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png


[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Configure email templates]: api-management-howto-configure-notifications.md#email-templates
