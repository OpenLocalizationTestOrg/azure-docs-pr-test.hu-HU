---
title: "csoportok használata az Azure API Management aaaManage fejlesztői fiókok |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage fejlesztői fiókok csoportok az Azure API Management"
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 33660b45-442f-44be-9a4a-1899d7f699b0
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: c46e010e41d9705ae161dcd60d734a76d19c9e93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-use-groups-toomanage-developer-accounts-in-azure-api-management"></a><span data-ttu-id="d13f2-103">Hogyan toocreate és -felhasználási csoportok toomanage fejlesztői fiókok az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="d13f2-103">How toocreate and use groups toomanage developer accounts in Azure API Management</span></span>
<span data-ttu-id="d13f2-104">Az API Management csoportok olyan termékek toodevelopers használt toomanage hello láthatóságát.</span><span class="sxs-lookup"><span data-stu-id="d13f2-104">In API Management, groups are used toomanage hello visibility of products toodevelopers.</span></span> <span data-ttu-id="d13f2-105">Termékek első készült látható toogroups, és ezeket a csoportokat a fejlesztők megtekintheti és toohello termékek hello csoportokhoz társított előfizetés majd.</span><span class="sxs-lookup"><span data-stu-id="d13f2-105">Products are first made visible toogroups, and then developers in those groups can view and subscribe toohello products that are associated with hello groups.</span></span> 

<span data-ttu-id="d13f2-106">Az API Management követően nem módosítható rendszer csoportok hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d13f2-106">API Management has hello following immutable system groups.</span></span>

* <span data-ttu-id="d13f2-107">**Rendszergazdák** – A csoportot Azure-előfizető rendszergazdák alkotják.</span><span class="sxs-lookup"><span data-stu-id="d13f2-107">**Administrators** - Azure subscription administrators are members of this group.</span></span> <span data-ttu-id="d13f2-108">Rendszergazdák kezelése az API Management szolgáltatáspéldány, API-k, műveletek és a fejlesztők által használt termékek létrehozása hello.</span><span class="sxs-lookup"><span data-stu-id="d13f2-108">Administrators manage API Management service instances, creating hello APIs, operations, and products that are used by developers.</span></span>
* <span data-ttu-id="d13f2-109">**Fejlesztők** – A fejlesztői portál hitelesített felhasználói tartoznak ebbe a csoportba.</span><span class="sxs-lookup"><span data-stu-id="d13f2-109">**Developers** - Authenticated developer portal users fall into this group.</span></span> <span data-ttu-id="d13f2-110">A fejlesztők olyan alkalmazásokat, az API-k használatával készíthetnek hello ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="d13f2-110">Developers are hello customers that build applications using your APIs.</span></span> <span data-ttu-id="d13f2-111">A fejlesztők kapnak hozzáférést toohello fejlesztői portálján, és alkalmazások összeállítását, amelyek hívja az API-k hello működésére.</span><span class="sxs-lookup"><span data-stu-id="d13f2-111">Developers are granted access toohello developer portal and build applications that call hello operations of an API.</span></span>
* <span data-ttu-id="d13f2-112">**A vendégek** -hitelesítés nélküli developer portálon a felhasználóknak, például a lehetséges ügyfelek látogató hello fejlesztői portálján az API Management példány alá esik az ebbe a csoportba.</span><span class="sxs-lookup"><span data-stu-id="d13f2-112">**Guests** - Unauthenticated developer portal users, such as prospective customers visiting hello developer portal of an API Management instance fall into this group.</span></span> <span data-ttu-id="d13f2-113">Ezek is hozzáférést bizonyos csak olvasható, például a hello képességét tooview API-k, de nem hívható meg őket.</span><span class="sxs-lookup"><span data-stu-id="d13f2-113">They can be granted certain read-only access, such as hello ability tooview APIs but not call them.</span></span>

<span data-ttu-id="d13f2-114">Toothese rendszer csoportok hozzáadása, a rendszergazdák egyéni csoportot hozhat létre vagy [kihasználja a társított Azure Active Directory-bérlők külső csoportok][leverage external groups in associated Azure Active Directory tenants].</span><span class="sxs-lookup"><span data-stu-id="d13f2-114">In addition toothese system groups, administrators can create custom groups or [leverage external groups in associated Azure Active Directory tenants][leverage external groups in associated Azure Active Directory tenants].</span></span> <span data-ttu-id="d13f2-115">Egyéni és külső csoportok rendszer csoportok jogosultságot ad a fejlesztők számára látható együtt is használható, és tooAPI terméket.</span><span class="sxs-lookup"><span data-stu-id="d13f2-115">Custom and external groups can be used alongside system groups in giving developers visibility and access tooAPI products.</span></span> <span data-ttu-id="d13f2-116">Például létrehozhat egy egyéni csoport tagja-e egy adott fejlesztők fiókpartner szervezetében dolgozó és a hozzáférés engedélyezése csak megfelelő API-k tartalmazó az API-k toohello.</span><span class="sxs-lookup"><span data-stu-id="d13f2-116">For example, you could create one custom group for developers affiliated with a specific partner organization and allow them access toohello APIs from a product containing relevant APIs only.</span></span> <span data-ttu-id="d13f2-117">Egy felhasználó egyszerre több csoport tagja is lehet.</span><span class="sxs-lookup"><span data-stu-id="d13f2-117">A user can be a member of more than one group.</span></span>

<span data-ttu-id="d13f2-118">Ez az útmutató bemutatja, hogyan API Management példány rendszergazdák új csoportok hozzáadása és rendelheti őket hozzá a termékek és a fejlesztők számára.</span><span class="sxs-lookup"><span data-stu-id="d13f2-118">This guide shows how administrators of an API Management instance can add new groups and associate them with products and developers.</span></span>

> [!NOTE]
> <span data-ttu-id="d13f2-119">Ezenkívül toocreating és hello publisher portálon csoportok kezelése, akkor hozhat létre és kezelhet a csoportok hello API Management REST API használatával [csoport](https://msdn.microsoft.com/library/azure/dn776329.aspx) entitás.</span><span class="sxs-lookup"><span data-stu-id="d13f2-119">In addition toocreating and managing groups in hello publisher portal, you can create and manage your groups using hello API Management REST API [Group](https://msdn.microsoft.com/library/azure/dn776329.aspx) entity.</span></span>
> 
> 

## <span data-ttu-id="d13f2-120"><a name="create-group"></a>Hozzon létre egy csoportot</span><span class="sxs-lookup"><span data-stu-id="d13f2-120"><a name="create-group"> </a>Create a group</span></span>
<span data-ttu-id="d13f2-121">egy új csoport toocreate kattintson **Publisher portal** hello Azure portál, az API-kezelés szolgáltatás a.</span><span class="sxs-lookup"><span data-stu-id="d13f2-121">toocreate a new group, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span> <span data-ttu-id="d13f2-122">Ezzel megnyitná toohello API Management publisher portálon.</span><span class="sxs-lookup"><span data-stu-id="d13f2-122">This takes you toohello API Management publisher portal.</span></span>

![Közzétevő portál][api-management-management-console]

> <span data-ttu-id="d13f2-124">Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="d13f2-124">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="d13f2-125">Kattintson a **csoportok** a hello **API Management** hello maradt, és válassza a menü **csoport hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="d13f2-125">Click **Groups** from hello **API Management** menu on hello left, and then click **Add Group**.</span></span>

![Új csoport hozzáadása][api-management-add-group]

<span data-ttu-id="d13f2-127">Adjon meg egy egyedi nevet hello csoport és az opcionális leírást, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="d13f2-127">Enter a unique name for hello group and an optional description, and click **Save**.</span></span>

![Új csoport hozzáadása][api-management-add-group-window]

<span data-ttu-id="d13f2-129">hello új csoport megjelenik a(z) hello csoportok esetén külön-külön tooedit hello **neve** vagy **leírás** hello csoport, kattintson a hello csoport hello listában hello nevét.</span><span class="sxs-lookup"><span data-stu-id="d13f2-129">hello new group is displayed in hello groups tab. tooedit hello **Name** or **Description** of hello group, click hello name of hello group in hello list.</span></span> <span data-ttu-id="d13f2-130">toodelete hello csoportjában kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="d13f2-130">toodelete hello group, click **Delete**.</span></span>

![Csoport hozzáadva][api-management-new-group]

<span data-ttu-id="d13f2-132">Most, hogy hello csoport jön létre, termékek és a fejlesztők kapcsolódó lehet.</span><span class="sxs-lookup"><span data-stu-id="d13f2-132">Now that hello group is created, it can be associated with products and developers.</span></span>

## <span data-ttu-id="d13f2-133"><a name="associate-group-product"></a>Társítsa a termék egy csoportot</span><span class="sxs-lookup"><span data-stu-id="d13f2-133"><a name="associate-group-product"> </a>Associate a group with a product</span></span>
<span data-ttu-id="d13f2-134">a csoportban található, a termék tooassociate kattintson **termékek** a hello **API Management** hello menüjének balra, és kattintson a kívánt termék hello hello nevét.</span><span class="sxs-lookup"><span data-stu-id="d13f2-134">tooassociate a group with a product, click **Products** from hello **API Management** menu on hello left, and then click hello name of hello desired product.</span></span>

![Láthatóság megadása][api-management-add-group-to-product]

<span data-ttu-id="d13f2-136">Jelölje be hello **látható** tooadd lapra, és távolítsa el a csoportokból és tooview hello aktuális hello termékhez.</span><span class="sxs-lookup"><span data-stu-id="d13f2-136">Select hello **Visibility** tab tooadd and remove groups, and tooview hello current groups for hello product.</span></span> <span data-ttu-id="d13f2-137">tooadd vagy csoportok, ellenőrizze, vagy törölje a hello négyzet jelölését, hello szükségeskonfiguráció-csoportokat, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="d13f2-137">tooadd or remove groups, check or uncheck hello checkboxes for hello desired groups and click **Save**.</span></span>

![Láthatóság megadása][api-management-add-group-to-product-visibility]

> [!NOTE]
> <span data-ttu-id="d13f2-139">Active Directory-csoportok tooadd, lásd: [hogyan tooauthorize fejlesztői fiókok Azure Active Directory, az Azure API Management](api-management-howto-aad.md).</span><span class="sxs-lookup"><span data-stu-id="d13f2-139">tooadd Azure Active Directory groups, see [How tooauthorize developer accounts using Azure Active Directory in Azure API Management](api-management-howto-aad.md).</span></span>
> 
> <span data-ttu-id="d13f2-140">hello tooconfigure csoportjait **látható** termék lapra, majd **csoportok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="d13f2-140">tooconfigure groups from hello **Visibility** tab for a product, click **Manage Groups**.</span></span>
> 
> 

<span data-ttu-id="d13f2-141">Ha a termék egy csoporthoz tartozik, fejlesztők csoport megtekintheti és toohello termék előfizetés.</span><span class="sxs-lookup"><span data-stu-id="d13f2-141">Once a product is associated with a group, developers in that group can view and subscribe toohello product.</span></span>

## <span data-ttu-id="d13f2-142"><a name="associate-group-developer"></a>Csoportok társítani a fejlesztők számára</span><span class="sxs-lookup"><span data-stu-id="d13f2-142"><a name="associate-group-developer"> </a>Associate groups with developers</span></span>
<span data-ttu-id="d13f2-143">tooassociate csoportok fejlesztőkkel, kattintson a **felhasználók** a hello **API Management** hello maradt, és hello fejlesztők melletti jelölőnégyzet hello menüjének kívánja tooassociate egy csoportba.</span><span class="sxs-lookup"><span data-stu-id="d13f2-143">tooassociate groups with developers, click **Users** from hello **API Management** menu on hello left, and then check hello box beside hello developers you wish tooassociate with a group.</span></span>

![Fejlesztői toogroup hozzáadása][api-management-add-group-to-developer]

<span data-ttu-id="d13f2-145">Ha hello szükséges, hogy a fejlesztők a rendszer ellenőrzi, kattintson a kívánt csoport hello hello **tooGroup hozzáadása** legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="d13f2-145">Once hello desired developers are checked, click hello desired group in hello **Add tooGroup** drop-down.</span></span> <span data-ttu-id="d13f2-146">Csoportok fejlesztők eltávolítható hello segítségével **távolítsa el a csoportból** legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="d13f2-146">Developers can be removed from groups by using hello **Remove from Group** drop-down.</span></span> 

![Fejlesztők][api-management-add-group-to-developer-saved]

<span data-ttu-id="d13f2-148">Hello fejlesztői és hello csoport közötti hello társítás hozzáadása után megtekintheti az hello **felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="d13f2-148">Once hello association is added between hello developer and hello group, you can view it in hello **Users** tab.</span></span>

## <span data-ttu-id="d13f2-149"><a name="next-steps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d13f2-149"><a name="next-steps"> </a>Next steps</span></span>
* <span data-ttu-id="d13f2-150">Egy fejlesztő tooa csoport hozzáadása után tekinthet meg és a csoport toohello termékek előfizetés.</span><span class="sxs-lookup"><span data-stu-id="d13f2-150">Once a developer is added tooa group, they can view and subscribe toohello products associated with that group.</span></span> <span data-ttu-id="d13f2-151">További információkért lásd: [hogyan létrehozása, és a termék közzététele az Azure API Management][How create and publish a product in Azure API Management],</span><span class="sxs-lookup"><span data-stu-id="d13f2-151">For more information, see [How create and publish a product in Azure API Management][How create and publish a product in Azure API Management],</span></span>
* <span data-ttu-id="d13f2-152">Ezenkívül toocreating és hello publisher portálon csoportok kezelése, akkor hozhat létre és kezelhet a csoportok hello API Management REST API használatával [csoport](https://msdn.microsoft.com/library/azure/dn776329.aspx) entitás.</span><span class="sxs-lookup"><span data-stu-id="d13f2-152">In addition toocreating and managing groups in hello publisher portal, you can create and manage your groups using hello API Management REST API [Group](https://msdn.microsoft.com/library/azure/dn776329.aspx) entity.</span></span>

[api-management-management-console]: ./media/api-management-howto-create-groups/api-management-management-console.png
[api-management-add-group]: ./media/api-management-howto-create-groups/api-management-add-group.png
[api-management-add-group-window]: ./media/api-management-howto-create-groups/api-management-add-group-window.png
[api-management-new-group]: ./media/api-management-howto-create-groups/api-management-new-group.png
[api-management-add-group-to-product]: ./media/api-management-howto-create-groups/api-management-add-group-to-product.png
[api-management-add-group-to-product-visibility]: ./media/api-management-howto-create-groups/api-management-add-group-to-product-visibility.png
[api-management-add-group-to-developer]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer.png
[api-management-add-group-to-developer-saved]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer-saved.png

[api-management-]: ./media/api-management-howto-create-groups/api-management-.png

[Create a group]: #create-group
[Associate a group with a product]: #associate-group-product
[Associate groups with developers]: #associate-group-developer
[Next steps]: #next-steps

[How create and publish a product in Azure API Management]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[leverage external groups in associated Azure Active Directory tenants]: api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group
