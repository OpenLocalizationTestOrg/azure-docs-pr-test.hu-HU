---
title: "Csoportok használata az Azure API Management fejlesztői fiókok kezelése |} Microsoft Docs"
description: "Csoportok használata az Azure API Management fejlesztői fiókok kezelése"
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
ms.openlocfilehash: b4d71cdfbab535b02542fbb26c7555265e5f9c37
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-and-use-groups-to-manage-developer-accounts-in-azure-api-management"></a><span data-ttu-id="ee928-103">Hozzon létre és csoportoknak a segítségével az Azure API Management fejlesztői fiókok kezelése</span><span class="sxs-lookup"><span data-stu-id="ee928-103">How to create and use groups to manage developer accounts in Azure API Management</span></span>
<span data-ttu-id="ee928-104">Az API Management szolgáltatásban csoportok használatával szabályozható a fejlesztők hozzáférése a termékhez.</span><span class="sxs-lookup"><span data-stu-id="ee928-104">In API Management, groups are used to manage the visibility of products to developers.</span></span> <span data-ttu-id="ee928-105">Termékek vannak először láthatóvá válnak az csoportok, és ezeket a csoportokat a fejlesztők megtekintheti és a termékek, a csoportokhoz tartozó előfizetés majd.</span><span class="sxs-lookup"><span data-stu-id="ee928-105">Products are first made visible to groups, and then developers in those groups can view and subscribe to the products that are associated with the groups.</span></span> 

<span data-ttu-id="ee928-106">Az API Management az alábbi megváltoztathatatlan rendszercsoportokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ee928-106">API Management has the following immutable system groups.</span></span>

* <span data-ttu-id="ee928-107">**Rendszergazdák** – A csoportot Azure-előfizető rendszergazdák alkotják.</span><span class="sxs-lookup"><span data-stu-id="ee928-107">**Administrators** - Azure subscription administrators are members of this group.</span></span> <span data-ttu-id="ee928-108">A rendszergazdák kezelik az API Management szolgáltatáspéldányokat, valamint ők hozzák létre az API-kat, a műveleteket és a fejlesztők által használt termékeket.</span><span class="sxs-lookup"><span data-stu-id="ee928-108">Administrators manage API Management service instances, creating the APIs, operations, and products that are used by developers.</span></span>
* <span data-ttu-id="ee928-109">**Fejlesztők** – A fejlesztői portál hitelesített felhasználói tartoznak ebbe a csoportba.</span><span class="sxs-lookup"><span data-stu-id="ee928-109">**Developers** - Authenticated developer portal users fall into this group.</span></span> <span data-ttu-id="ee928-110">A fejlesztők olyan ügyfelek, akik alkalmazásokat hoznak létre az API-k segítségével.</span><span class="sxs-lookup"><span data-stu-id="ee928-110">Developers are the customers that build applications using your APIs.</span></span> <span data-ttu-id="ee928-111">A fejlesztők hozzáférhetnek a fejlesztői portálhoz, és olyan alkalmazásokat készíthetnek, amelyek egy API műveleteit hívják meg.</span><span class="sxs-lookup"><span data-stu-id="ee928-111">Developers are granted access to the developer portal and build applications that call the operations of an API.</span></span>
* <span data-ttu-id="ee928-112">**Vendégek** – A fejlesztői portál nem hitelesített felhasználói tartoznak ebbe a csoportba, például az egyik API Management példány fejlesztői portálját meglátogató leendő ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="ee928-112">**Guests** - Unauthenticated developer portal users, such as prospective customers visiting the developer portal of an API Management instance fall into this group.</span></span> <span data-ttu-id="ee928-113">A vendégek kaphatnak bizonyos szintű, csak olvasási hozzáférést, például megtekinthetnek API-kat, de nem hívhatják meg őket.</span><span class="sxs-lookup"><span data-stu-id="ee928-113">They can be granted certain read-only access, such as the ability to view APIs but not call them.</span></span>

<span data-ttu-id="ee928-114">A rendszer csoportokban mellett a rendszergazdák egyéni csoportot hozhat létre vagy [kihasználja a társított Azure Active Directory-bérlők külső csoportok][leverage external groups in associated Azure Active Directory tenants].</span><span class="sxs-lookup"><span data-stu-id="ee928-114">In addition to these system groups, administrators can create custom groups or [leverage external groups in associated Azure Active Directory tenants][leverage external groups in associated Azure Active Directory tenants].</span></span> <span data-ttu-id="ee928-115">A fejlesztők mellett az egyéni és külső csoportoknak is lehet adni láthatóságot és hozzáférést az API-termékekhez.</span><span class="sxs-lookup"><span data-stu-id="ee928-115">Custom and external groups can be used alongside system groups in giving developers visibility and access to API products.</span></span> <span data-ttu-id="ee928-116">Például egy adott partnerszervezet fejlesztői számára létre lehet hozni egy egyéni csoportot, és hozzáférést lehet nekik biztosítani a megfelelő API-kat tartalmazó termék API-jaihoz.</span><span class="sxs-lookup"><span data-stu-id="ee928-116">For example, you could create one custom group for developers affiliated with a specific partner organization and allow them access to the APIs from a product containing relevant APIs only.</span></span> <span data-ttu-id="ee928-117">Egy felhasználó egyszerre több csoport tagja is lehet.</span><span class="sxs-lookup"><span data-stu-id="ee928-117">A user can be a member of more than one group.</span></span>

<span data-ttu-id="ee928-118">Ez az útmutató bemutatja, hogyan API Management példány rendszergazdák új csoportok hozzáadása és rendelheti őket hozzá a termékek és a fejlesztők számára.</span><span class="sxs-lookup"><span data-stu-id="ee928-118">This guide shows how administrators of an API Management instance can add new groups and associate them with products and developers.</span></span>

> [!NOTE]
> <span data-ttu-id="ee928-119">Csoportok létrehozása és kezelése a közzétevő portálon, mellett hozhat létre és az API Management REST API használatával csoportok kezelése [csoport](https://msdn.microsoft.com/library/azure/dn776329.aspx) entitás.</span><span class="sxs-lookup"><span data-stu-id="ee928-119">In addition to creating and managing groups in the publisher portal, you can create and manage your groups using the API Management REST API [Group](https://msdn.microsoft.com/library/azure/dn776329.aspx) entity.</span></span>
> 
> 

## <span data-ttu-id="ee928-120"><a name="create-group"></a>Hozzon létre egy csoportot</span><span class="sxs-lookup"><span data-stu-id="ee928-120"><a name="create-group"> </a>Create a group</span></span>
<span data-ttu-id="ee928-121">Új csoport létrehozásához kattintson a **Publisher portal** az Azure portálon az API Management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ee928-121">To create a new group, click **Publisher portal** in the Azure Portal for your API Management service.</span></span> <span data-ttu-id="ee928-122">Ezzel továbblép az API Management közzétevő portáljára.</span><span class="sxs-lookup"><span data-stu-id="ee928-122">This takes you to the API Management publisher portal.</span></span>

![Közzétevő portál][api-management-management-console]

> <span data-ttu-id="ee928-124">Ha még nem hozott létre API Management szolgáltatáspéldányt, tekintse meg az [Ismerkedés az Azure API Management szolgáltatással][Get started with Azure API Management] oktatóanyag [API Management szolgáltatáspéldány létrehozása][Create an API Management service instance] című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="ee928-124">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="ee928-125">Kattintson a **csoportok** a a **API Management** a bal oldali menüben, majd **csoport hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="ee928-125">Click **Groups** from the **API Management** menu on the left, and then click **Add Group**.</span></span>

![Új csoport hozzáadása][api-management-add-group]

<span data-ttu-id="ee928-127">Adjon meg egy egyedi nevet a csoport és az opcionális leírást, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="ee928-127">Enter a unique name for the group and an optional description, and click **Save**.</span></span>

![Új csoport hozzáadása][api-management-add-group-window]

<span data-ttu-id="ee928-129">Az új csoport megjelenik a csoportok lapon.</span><span class="sxs-lookup"><span data-stu-id="ee928-129">The new group is displayed in the groups tab.</span></span> <span data-ttu-id="ee928-130">A módosítandó a **neve** vagy **leírás** csoportjában kattintson a listában a csoport nevét.</span><span class="sxs-lookup"><span data-stu-id="ee928-130">To edit the **Name** or **Description** of the group, click the name of the group in the list.</span></span> <span data-ttu-id="ee928-131">A csoport törléséhez kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="ee928-131">To delete the group, click **Delete**.</span></span>

![Csoport hozzáadva][api-management-new-group]

<span data-ttu-id="ee928-133">Most, hogy a csoport jön létre, termékek és a fejlesztők kapcsolódó lehet.</span><span class="sxs-lookup"><span data-stu-id="ee928-133">Now that the group is created, it can be associated with products and developers.</span></span>

## <span data-ttu-id="ee928-134"><a name="associate-group-product"></a>Társítsa a termék egy csoportot</span><span class="sxs-lookup"><span data-stu-id="ee928-134"><a name="associate-group-product"> </a>Associate a group with a product</span></span>
<span data-ttu-id="ee928-135">Társítsa a termék egy csoportot, kattintson a **termékek** a a **API Management** a bal oldali menü és kattintson a kívánt termék nevét.</span><span class="sxs-lookup"><span data-stu-id="ee928-135">To associate a group with a product, click **Products** from the **API Management** menu on the left, and then click the name of the desired product.</span></span>

![Láthatóság megadása][api-management-add-group-to-product]

<span data-ttu-id="ee928-137">Válassza ki a **látható** lap hozzáadása és eltávolítása a csoportok és a termék aktuális csoportok megtekintése.</span><span class="sxs-lookup"><span data-stu-id="ee928-137">Select the **Visibility** tab to add and remove groups, and to view the current groups for the product.</span></span> <span data-ttu-id="ee928-138">Adja hozzá, vagy távolítsa el a csoportokat, ellenőrizze, vagy törölje a négyzet jelölését, a kívánt csoportok számára, majd kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="ee928-138">To add or remove groups, check or uncheck the checkboxes for the desired groups and click **Save**.</span></span>

![Láthatóság megadása][api-management-add-group-to-product-visibility]

> [!NOTE]
> <span data-ttu-id="ee928-140">Azure Active Directory-csoportok hozzáadása, lásd: [hogyan szeretné engedélyekkel felruházni fejlesztői fiókok Azure Active Directory használatával az Azure API Management](api-management-howto-aad.md).</span><span class="sxs-lookup"><span data-stu-id="ee928-140">To add Azure Active Directory groups, see [How to authorize developer accounts using Azure Active Directory in Azure API Management](api-management-howto-aad.md).</span></span>
> 
> <span data-ttu-id="ee928-141">A csoportok konfigurálásához a **látható** termék lapra, majd **csoportok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="ee928-141">To configure groups from the **Visibility** tab for a product, click **Manage Groups**.</span></span>
> 
> 

<span data-ttu-id="ee928-142">Ha a termék egy csoporthoz tartozik, fejlesztők csoport megtekintheti és előfizetni a termék.</span><span class="sxs-lookup"><span data-stu-id="ee928-142">Once a product is associated with a group, developers in that group can view and subscribe to the product.</span></span>

## <span data-ttu-id="ee928-143"><a name="associate-group-developer"></a>Csoportok társítani a fejlesztők számára</span><span class="sxs-lookup"><span data-stu-id="ee928-143"><a name="associate-group-developer"> </a>Associate groups with developers</span></span>
<span data-ttu-id="ee928-144">Csoportok fejlesztőkkel való társításához kattintson **felhasználók** a a **API Management** a bal oldali menüben, majd ellenőrizze a fejlesztők a csoportba felvenni kívánt melletti mezőbe.</span><span class="sxs-lookup"><span data-stu-id="ee928-144">To associate groups with developers, click **Users** from the **API Management** menu on the left, and then check the box beside the developers you wish to associate with a group.</span></span>

![Fejlesztői hozzáadása csoporthoz][api-management-add-group-to-developer]

<span data-ttu-id="ee928-146">A kívánt fejlesztők is be van jelölve, kattintson a kívánt csoportot a **hozzáadása csoporthoz** legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="ee928-146">Once the desired developers are checked, click the desired group in the **Add to Group** drop-down.</span></span> <span data-ttu-id="ee928-147">A fejlesztők lehet eltávolítani a csoportok használatával a **távolítsa el a csoportból** legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="ee928-147">Developers can be removed from groups by using the **Remove from Group** drop-down.</span></span> 

![Fejlesztők][api-management-add-group-to-developer-saved]

<span data-ttu-id="ee928-149">A fejlesztői és a csoport között. a társítás hozzáadása után megtekintheti azokat a **felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="ee928-149">Once the association is added between the developer and the group, you can view it in the **Users** tab.</span></span>

## <span data-ttu-id="ee928-150"><a name="next-steps"> </a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ee928-150"><a name="next-steps"> </a>Next steps</span></span>
* <span data-ttu-id="ee928-151">Egy fejlesztő csoporthoz hozzáadott, tekinthet meg és a termékek, a csoport előfizetni.</span><span class="sxs-lookup"><span data-stu-id="ee928-151">Once a developer is added to a group, they can view and subscribe to the products associated with that group.</span></span> <span data-ttu-id="ee928-152">További információkért lásd: [hogyan létrehozása, és a termék közzététele az Azure API Management][How create and publish a product in Azure API Management],</span><span class="sxs-lookup"><span data-stu-id="ee928-152">For more information, see [How create and publish a product in Azure API Management][How create and publish a product in Azure API Management],</span></span>
* <span data-ttu-id="ee928-153">Csoportok létrehozása és kezelése a közzétevő portálon, mellett hozhat létre és az API Management REST API használatával csoportok kezelése [csoport](https://msdn.microsoft.com/library/azure/dn776329.aspx) entitás.</span><span class="sxs-lookup"><span data-stu-id="ee928-153">In addition to creating and managing groups in the publisher portal, you can create and manage your groups using the API Management REST API [Group](https://msdn.microsoft.com/library/azure/dn776329.aspx) entity.</span></span>

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
