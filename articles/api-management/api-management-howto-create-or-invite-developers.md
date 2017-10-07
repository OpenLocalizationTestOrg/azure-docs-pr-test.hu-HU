---
title: "aaaHow az Azure API Management felhasználói fiókok kezelése |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate vagy a meghívott felhasználó felhasználók az Azure API Management"
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
ms.openlocfilehash: 3966f4454e29621d7c615beefee352ec91b48b2e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-user-accounts-in-azure-api-management"></a><span data-ttu-id="e7b29-103">Hogyan toomanage felhasználói fiókot, az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="e7b29-103">How toomanage user accounts in Azure API Management</span></span>
<span data-ttu-id="e7b29-104">Az API Management fejlesztők hello teszi az API Management használata API-k hello felhasználók.</span><span class="sxs-lookup"><span data-stu-id="e7b29-104">In API Management, developers are hello users of hello APIs that you expose using API Management.</span></span> <span data-ttu-id="e7b29-105">Ez az útmutató azt mutatja be toohow toocreate és a meghívás fejlesztők toouse hello API-k és termékek, hogy elérhető toothem az API Management-példánnyal.</span><span class="sxs-lookup"><span data-stu-id="e7b29-105">This guide shows toohow toocreate and invite developers toouse hello APIs and products that you make available toothem with your API Management instance.</span></span> <span data-ttu-id="e7b29-106">A felhasználói fiókok kezelése programozott módon információkért lásd: hello [felhasználói entitás](https://msdn.microsoft.com/library/azure/dn776330.aspx) hello dokumentáció [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="e7b29-106">For information on managing user accounts programmatically, see hello [User entity](https://msdn.microsoft.com/library/azure/dn776330.aspx) documentation in hello [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) reference.</span></span>

## <span data-ttu-id="e7b29-107"><a name="create-developer"></a>Hozzon létre egy új fejlesztő</span><span class="sxs-lookup"><span data-stu-id="e7b29-107"><a name="create-developer"> </a>Create a new developer</span></span>
<span data-ttu-id="e7b29-108">Kattintson egy új fejlesztői toocreate **Publisher portal** hello Azure portál, az API-kezelés szolgáltatás a.</span><span class="sxs-lookup"><span data-stu-id="e7b29-108">toocreate a new developer, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span> <span data-ttu-id="e7b29-109">Ezzel megnyitná toohello API Management publisher portálon.</span><span class="sxs-lookup"><span data-stu-id="e7b29-109">This takes you toohello API Management publisher portal.</span></span> <span data-ttu-id="e7b29-110">Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="e7b29-110">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>

![Közzétevő portál][api-management-management-console]

<span data-ttu-id="e7b29-112">Kattintson a **felhasználók** a hello **API Management** hello maradt, és válassza a menü **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="e7b29-112">Click **Users** from hello **API Management** menu on hello left, and then click **add user**.</span></span>

![Fejlesztői létrehozása][api-management-create-developer]

<span data-ttu-id="e7b29-114">Adja meg a hello **E-mail**, **jelszó**, és **neve** hello új fejlesztői, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="e7b29-114">Enter hello **Email**, **Password**, and **Name** for hello new developer and click **Save**.</span></span>

![Fejlesztői létrehozása][api-management-add-new-user]

<span data-ttu-id="e7b29-116">Alapértelmezés szerint az újonnan létrehozott fejlesztői fiókok vannak **aktív**, és a társított hello **fejlesztők** csoport.</span><span class="sxs-lookup"><span data-stu-id="e7b29-116">By default, newly created developer accounts are **Active**, and associated with hello **Developers** group.</span></span>

![Új fejlesztői][api-management-new-developer]

<span data-ttu-id="e7b29-118">A fejlesztői fiókok egy **aktív** állapot lehet használt tooaccess összes hello API-k, amelyeknél az előfizetések.</span><span class="sxs-lookup"><span data-stu-id="e7b29-118">Developer accounts that are in an **active** state can be used tooaccess all of hello APIs for which they have subscriptions.</span></span> <span data-ttu-id="e7b29-119">tooassociate az újonnan létrehozott hello fejlesztői további csoportokkal, lásd: [hogyan tooassociate csoportok fejlesztőkkel][How tooassociate groups with developers].</span><span class="sxs-lookup"><span data-stu-id="e7b29-119">tooassociate hello newly created developer with additional groups, see [How tooassociate groups with developers][How tooassociate groups with developers].</span></span>

## <span data-ttu-id="e7b29-120"><a name="invite-developer"></a>Egy fejlesztő meghívása</span><span class="sxs-lookup"><span data-stu-id="e7b29-120"><a name="invite-developer"> </a>Invite a developer</span></span>
<span data-ttu-id="e7b29-121">tooinvite egy fejlesztő kattintson **felhasználók** a hello **API Management** hello maradt, és válassza a menü **hívhat meg felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="e7b29-121">tooinvite a developer, click **Users** from hello **API Management** menu on hello left, and then click **Invite User**.</span></span>

![Fejlesztői meghívása][api-management-invite-developer]

<span data-ttu-id="e7b29-123">Adja meg a hello fejlesztői hello név és e-mail címét, majd kattintson a **meghívása**.</span><span class="sxs-lookup"><span data-stu-id="e7b29-123">Enter hello name and email address of hello developer, and click **Invite**.</span></span>

![Fejlesztői meghívása][api-management-invite-developer-window]

<span data-ttu-id="e7b29-125">Egy megerősítő üzenet jelenik meg, de újonnan meghívott hello fejlesztői nem jelenik meg amíg hello lista után hello meghívó elfogadja őket.</span><span class="sxs-lookup"><span data-stu-id="e7b29-125">A confirmation message is displayed, but hello newly invited developer does not appear in hello list until after they accept hello invitation.</span></span> 

![Jóváhagyás meghívása][api-management-invite-developer-confirmation]

<span data-ttu-id="e7b29-127">Amikor egy fejlesztő felkérik, egy e-mailt küld toohello fejlesztői.</span><span class="sxs-lookup"><span data-stu-id="e7b29-127">When a developer is invited, an email is sent toohello developer.</span></span> <span data-ttu-id="e7b29-128">Ez az e-mail sablon használatával jön létre, és testre szabható.</span><span class="sxs-lookup"><span data-stu-id="e7b29-128">This email is generated using a template and is customizable.</span></span> <span data-ttu-id="e7b29-129">További információkért lásd: [konfigurálás e-mail sablonok][Configure email templates].</span><span class="sxs-lookup"><span data-stu-id="e7b29-129">For more information, see [Configure email templates][Configure email templates].</span></span>

<span data-ttu-id="e7b29-130">Hello felkérés elfogadása után hello fiók aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="e7b29-130">Once hello invitation is accepted, hello account becomes active.</span></span>

## <span data-ttu-id="e7b29-131"><a name="block-developer"></a> Inaktiválja vagy újraaktiválásához fejlesztői fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="e7b29-131"><a name="block-developer"> </a> Deactivate or reactivate a developer account</span></span>
<span data-ttu-id="e7b29-132">Alapértelmezés szerint az újonnan létrehozott vagy a meghívott fejlesztői fiókok vannak **aktív**.</span><span class="sxs-lookup"><span data-stu-id="e7b29-132">By default, newly created or invited developer accounts are **Active**.</span></span> <span data-ttu-id="e7b29-133">fejlesztői fiók létrehozása toodeactivate kattintson **blokk**.</span><span class="sxs-lookup"><span data-stu-id="e7b29-133">toodeactivate a developer account, click **Block**.</span></span> <span data-ttu-id="e7b29-134">letiltott fejlesztői fiók létrehozása tooreactivate kattintson **aktiválás**.</span><span class="sxs-lookup"><span data-stu-id="e7b29-134">tooreactivate a blocked developer account, click **Activate**.</span></span> <span data-ttu-id="e7b29-135">Letiltott fejlesztői fiók létrehozása nem hello fejlesztői portálján érheti el és bármely API-k hívása.</span><span class="sxs-lookup"><span data-stu-id="e7b29-135">A blocked developer account can not access hello developer portal or call any APIs.</span></span> <span data-ttu-id="e7b29-136">toodelete egy felhasználói fiókot, kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="e7b29-136">toodelete a user account, click **Delete**.</span></span>

![Blokk fejlesztői][api-management-new-developer]

## <a name="reset-a-user-password"></a><span data-ttu-id="e7b29-138">Felhasználói jelszó alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="e7b29-138">Reset a user password</span></span>
<span data-ttu-id="e7b29-139">tooreset hello jelszavát egy felhasználói fiókot, kattintson a hello hello fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="e7b29-139">tooreset hello password for a user account, click hello name of hello account.</span></span>

![Új jelszó létrehozása][api-management-view-developer]

<span data-ttu-id="e7b29-141">Kattintson a **jelszó-átállítási** toosend egy hivatkozás toohello felhasználói tooreset a jelszavát.</span><span class="sxs-lookup"><span data-stu-id="e7b29-141">Click **Reset password** toosend a link toohello user tooreset their password.</span></span>

![Új jelszó létrehozása][api-management-reset-password]

<span data-ttu-id="e7b29-143">tooprogrammatically együttműködnek a felhasználói fiókok, lásd: hello [felhasználói entitás](https://msdn.microsoft.com/library/azure/dn776330.aspx) hello dokumentáció [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="e7b29-143">tooprogrammatically work with user accounts, see hello [User entity](https://msdn.microsoft.com/library/azure/dn776330.aspx) documentation in hello [API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) reference.</span></span> <span data-ttu-id="e7b29-144">egy felhasználói fiók jelszavát tooa érték tooreset, hello használható [egy felhasználó frissítéséhez](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) művelet, és adja meg a hello kívánt jelszót.</span><span class="sxs-lookup"><span data-stu-id="e7b29-144">tooreset a user account password tooa specific value, you can use hello [Update a user](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) operation and specify hello desired password.</span></span>

## <a name="pending-verification"></a><span data-ttu-id="e7b29-145">Függőben lévő ellenőrzési</span><span class="sxs-lookup"><span data-stu-id="e7b29-145">Pending verification</span></span>
![Függőben lévő ellenőrzési][api-management-pending-verification]

## <span data-ttu-id="e7b29-147"><a name="next-steps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e7b29-147"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="e7b29-148">A fejlesztői fiók létrehozása után szerepkörökhöz társítandó, és tooproducts és API-k fizessen elő.</span><span class="sxs-lookup"><span data-stu-id="e7b29-148">Once a developer account is created, you can associate it with roles and subscribe it tooproducts and APIs.</span></span> <span data-ttu-id="e7b29-149">További információkért lásd: [hogyan toocreate és -felhasználási csoportok][How toocreate and use groups].</span><span class="sxs-lookup"><span data-stu-id="e7b29-149">For more information, see [How toocreate and use groups][How toocreate and use groups].</span></span>

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
[How toocreate and use groups]: api-management-howto-create-groups.md
[How tooassociate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Configure email templates]: api-management-howto-configure-notifications.md#email-templates
