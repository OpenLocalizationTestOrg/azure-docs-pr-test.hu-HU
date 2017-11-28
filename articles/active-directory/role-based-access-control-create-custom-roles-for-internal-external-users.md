---
title: "Egyéni szerepköralapú hozzáférés-vezérlés szerepköröket hozhat létre, és rendelje hozzá a belső és külső felhasználók számára az Azure-ban |} Microsoft Docs"
description: "PowerShell és a parancssori felület használatával a belső és külső felhasználók számára létrehozott egyéni RBAC-szerepkörök hozzárendelése"
services: active-directory
documentationcenter: 
author: andreicradu
manager: catadinu
editor: kgremban
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/10/2017
ms.author: a-crradu
ms.openlocfilehash: d687f94bebfd0b6c1ec0690da798be5409640954
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
## <a name="intro-on-role-based-access-control"></a><span data-ttu-id="96e9d-103">A szerepköralapú hozzáférés-vezérlés – bevezetés</span><span class="sxs-lookup"><span data-stu-id="96e9d-103">Intro on role-based access control</span></span>

<span data-ttu-id="96e9d-104">Szerepköralapú hozzáférés-vezérlés az Azure portál csak szolgáltatása: így tulajdonosai előfizetés részletes szerepkörök hozzárendelése más felhasználók akik kezelhetik a környezetükben meghatározott erőforrás-hatókörök.</span><span class="sxs-lookup"><span data-stu-id="96e9d-104">Role-based access control is an Azure portal only feature allowing the owners of a subscription to assign granular roles to other users who can manage specific resource scopes in their environment.</span></span>

<span data-ttu-id="96e9d-105">Az RBAC lehetővé teszi, hogy jobb biztonságkezelés nagy méretű szervezeteknek, és az SMB-khez működik-e külső közreműködő, szállítókkal és freelancers, amelyhez hozzá kell férniük az adott környezetben meghatározott erőforrás azonban nem feltétlenül a teljes infrastruktúra vagy bármely a számlázással kapcsolatos hatókörök.</span><span class="sxs-lookup"><span data-stu-id="96e9d-105">RBAC allows better security management for large organizations and for SMBs working with external collaborators, vendors or freelancers which need access to specific resources in your environment but not necessarily to the entire infrastructure or any billing-related scopes.</span></span> <span data-ttu-id="96e9d-106">Az RBAC lehetővé teszi, hogy egy Azure-előfizetéssel rendelkező rugalmasan kezeli a rendszergazdai fiókot (szolgáltatás-rendszergazda szerepkörrel egy előfizetés szintjén), és több felhasználók meghívást az azonos előfizetésben, de bármilyen rendszergazdai jogosultságok nélkül működik az .</span><span class="sxs-lookup"><span data-stu-id="96e9d-106">RBAC allows the flexibility of owning one Azure subscription managed by the administrator account (service administrator role at a subscription level) and have multiple users invited to work under the same subscription but without any administrative rights for it.</span></span> <span data-ttu-id="96e9d-107">Felügyeleti és számlázási szempontból az RBAC funkció bizonyul egy idő és a felügyeleti hatékony beállítása a különböző forgatókönyvekben Azure használatával.</span><span class="sxs-lookup"><span data-stu-id="96e9d-107">From a management and billing perspective, the RBAC feature proves to be a time and management efficient option for using Azure in various scenarios.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="96e9d-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="96e9d-108">Prerequisites</span></span>
<span data-ttu-id="96e9d-109">Az RBAC használata az Azure környezetben van szükség:</span><span class="sxs-lookup"><span data-stu-id="96e9d-109">Using RBAC in the Azure environment requires:</span></span>

* <span data-ttu-id="96e9d-110">Önálló rendelkezik Azure-előfizetés a felhasználóhoz rendelt tulajdonosa (előfizetés szerepkör)</span><span class="sxs-lookup"><span data-stu-id="96e9d-110">Having a standalone Azure subscription assigned to the user as owner (subscription role)</span></span>
* <span data-ttu-id="96e9d-111">Rendelkezik a tulajdonosi szerepkört, az Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="96e9d-111">Have the Owner role of the Azure subscription</span></span>
* <span data-ttu-id="96e9d-112">Rendelkezik hozzáféréssel a [Azure-portálon](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="96e9d-112">Have access to the [Azure portal](https://portal.azure.com)</span></span>
* <span data-ttu-id="96e9d-113">Győződjön meg arról, hogy a következő erőforrás-szolgáltató regisztrálva a felhasználó az előfizetéshez: **Microsoft.Authorization**.</span><span class="sxs-lookup"><span data-stu-id="96e9d-113">Make sure to have the following Resource Providers registered for the user subscription: **Microsoft.Authorization**.</span></span> <span data-ttu-id="96e9d-114">Az erőforrás-szolgáltatók regisztrálásával kapcsolatos további információkért lásd: [Resource Manager szolgáltatók, régiók, API verziók és sémák](/azure-resource-manager/resource-manager-supported-services.md).</span><span class="sxs-lookup"><span data-stu-id="96e9d-114">For more information on how to register the resource providers, see [Resource Manager providers, regions, API versions and schemas](/azure-resource-manager/resource-manager-supported-services.md).</span></span>

> [!NOTE]
> <span data-ttu-id="96e9d-115">Office 365-előfizetéssel, vagy az Azure Active Directory-licencek (például: Azure Active Directory eléréséhez) kiosztott az Office 365 portálon nem minőségi az RBAC használata.</span><span class="sxs-lookup"><span data-stu-id="96e9d-115">Office 365 subscriptions or Azure Active Directory licenses (for example: Access to Azure Active Directory) provisioned from the O365 portal don't quality for using RBAC.</span></span>

## <a name="how-can-rbac-be-used"></a><span data-ttu-id="96e9d-116">Hogyan lehet használni az RBAC</span><span class="sxs-lookup"><span data-stu-id="96e9d-116">How can RBAC be used</span></span>
<span data-ttu-id="96e9d-117">Az RBAC alkalmazni lehet az Azure-ban három különböző hatóköröket.</span><span class="sxs-lookup"><span data-stu-id="96e9d-117">RBAC can be applied at three different scopes in Azure.</span></span> <span data-ttu-id="96e9d-118">A legalacsonyabb egy legmagasabb hatálya alá akkor a következők:</span><span class="sxs-lookup"><span data-stu-id="96e9d-118">From the highest scope to the lowest one, they are as follows:</span></span>

* <span data-ttu-id="96e9d-119">Előfizetés (legmagasabb)</span><span class="sxs-lookup"><span data-stu-id="96e9d-119">Subscription (highest)</span></span>
* <span data-ttu-id="96e9d-120">Erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="96e9d-120">Resource group</span></span>
* <span data-ttu-id="96e9d-121">Erőforrás hatókör (a hozzáférési legalacsonyabb kínál az egyes Azure-erőforrás hatókör célként megadott engedélyeket)</span><span class="sxs-lookup"><span data-stu-id="96e9d-121">Resource scope (the lowest access level offering targeted permissions to an individual Azure resource scope)</span></span>

## <a name="assign-rbac-roles-at-the-subscription-scope"></a><span data-ttu-id="96e9d-122">Az előfizetés hatókörből RBAC-szerepkörök hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="96e9d-122">Assign RBAC roles at the subscription scope</span></span>
<span data-ttu-id="96e9d-123">Nincsenek két gyakori példán RBAC használja (de nem kizárólagosan):</span><span class="sxs-lookup"><span data-stu-id="96e9d-123">There are two common examples when RBAC is used (but not limited to):</span></span>

* <span data-ttu-id="96e9d-124">Hogy a szervezetek a külső felhasználók (nem a rendszergazda felhasználó Azure Active Directory-bérlő része) meghívót, hogy bizonyos erőforrások vagy a teljes előfizetés kezelése</span><span class="sxs-lookup"><span data-stu-id="96e9d-124">Having external users from the organizations (not part of the admin user's Azure Active Directory tenant) invited to manage certain resources or the whole subscription</span></span>
* <span data-ttu-id="96e9d-125">A felhasználók a szervezet (részét képezik a felhasználó Azure Active Directory-bérlő), de a különböző csapatok vagy csoportokat, amelyek a teljes előfizetés vagy egy bizonyos erőforráscsoportok vagy az erőforrás-hatókörök részletes hozzáférésre van szükségük belső használata a környezet</span><span class="sxs-lookup"><span data-stu-id="96e9d-125">Working with users inside the organization (they are part of the user's Azure Active Directory tenant) but part of different teams or groups which need granular access either to the whole subscription or to certain resource groups or resource scopes in the environment</span></span>

## <a name="grant-access-at-a-subscription-level-for-a-user-outside-of-azure-active-directory"></a><span data-ttu-id="96e9d-126">Hozzáférés egy felhasználó Azure Active Directory kívül egy előfizetés szintjén</span><span class="sxs-lookup"><span data-stu-id="96e9d-126">Grant access at a subscription level for a user outside of Azure Active Directory</span></span>
<span data-ttu-id="96e9d-127">Az RBAC-szerepkörök csak akkor adhatók **tulajdonosok** az előfizetés ezért a rendszergazdai jogú felhasználó kell bejelentkeznie, amely rendelkezik-e előre szerepkörrel, vagy az Azure-előfizetés hozott létre egy felhasználónévvel.</span><span class="sxs-lookup"><span data-stu-id="96e9d-127">RBAC roles can be granted only by **Owners** of the subscription therefore the admin user must be logged with a username which has this role pre-assigned or has created the Azure subscription.</span></span>

<span data-ttu-id="96e9d-128">Az Azure-portálon után bejelentkezés rendszergazdaként, válassza ki "Előfizetések", és válassza a kívánt egy.</span><span class="sxs-lookup"><span data-stu-id="96e9d-128">From the Azure portal, after you sign-in as admin, select “Subscriptions” and chose the desired one.</span></span>
<span data-ttu-id="96e9d-129">![előfizetés panel az Azure-portálon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/0.png) alapértelmezés szerint a rendszergazdai jogú felhasználó rendelkezik vásárolt Azure-előfizetést, ha a felhasználó fog megjelenni **Fiókadminisztrátor**, ez az előfizetés szerepkör alatt.</span><span class="sxs-lookup"><span data-stu-id="96e9d-129">![subscription blade in Azure portal](./media/role-based-access-control-create-custom-roles-for-internal-external-users/0.png) By default, if the admin user has purchased the Azure subscription, the user will show up as **Account Admin**, this being the subscription role.</span></span> <span data-ttu-id="96e9d-130">Az Azure-előfizetés szerepkörök további részletekért lásd: [hozzáadása vagy módosítása, hogy az előfizetés vagy a szolgáltatások kezelése az Azure rendszergazdai szerepkörök](/billing/billing-add-change-azure-subscription-administrator.md).</span><span class="sxs-lookup"><span data-stu-id="96e9d-130">For more details on the Azure subscription roles, see [Add or change Azure administrator roles that manage the subscription or services](/billing/billing-add-change-azure-subscription-administrator.md).</span></span>

<span data-ttu-id="96e9d-131">Ebben a példában a felhasználó a "alflanigan@outlook.com" van a **tulajdonos** az "Ingyenes" az aad-ben az előfizetéshez bérlői "Alapértelmezett bérlőt Azure".</span><span class="sxs-lookup"><span data-stu-id="96e9d-131">In this example, the user "alflanigan@outlook.com" is the **Owner** of the "Free Trial" subscription in the AAD tenant "Default tenant Azure".</span></span> <span data-ttu-id="96e9d-132">Mivel ez a felhasználó hozta létre a kezdeti Microsoft Account "Outlook" az Azure-előfizetés (Microsoft Account = Outlook, a működés közbeni stb.) az alapértelmezett tartomány nevét ennél a bérlőnél a hozzáadott összes többi felhasználó számára lesz **"@alflaniganuoutlook.onmicrosoft.com"**.</span><span class="sxs-lookup"><span data-stu-id="96e9d-132">Since this user is the creator of the Azure subscription with the initial Microsoft Account “Outlook” (Microsoft Account = Outlook, Live etc.) the default domain name for all other users added in this tenant will be **"@alflaniganuoutlook.onmicrosoft.com"**.</span></span> <span data-ttu-id="96e9d-133">Úgy lett kialakítva, a szintaxist, az új tartomány formátuma kiépítésekor a bérlő létrehozó felhasználó felhasználónevét és tartományát nevét, és vegye fel a bővítmény **". onmicrosoft.com"**.</span><span class="sxs-lookup"><span data-stu-id="96e9d-133">By design, the syntax of the new domain is formed by putting together the username and domain name of the user who created the tenant and adding the extension **".onmicrosoft.com"**.</span></span>
<span data-ttu-id="96e9d-134">Ezenkívül felhasználók is jelentkezzen be a bérlő egy egyéni tartománynév hozzáadása, és azt az új bérlő ellenőrzése után.</span><span class="sxs-lookup"><span data-stu-id="96e9d-134">Furthermore, users can sign-in with a custom domain name in the tenant after adding and verifying it for the new tenant.</span></span> <span data-ttu-id="96e9d-135">Az Azure Active Directory-bérlő egyéni tartománynév ellenőrzése a további részletekért lásd: [egyéni tartománynév hozzáadása a címtárhoz](/active-directory/active-directory-add-domain).</span><span class="sxs-lookup"><span data-stu-id="96e9d-135">For more details on how to verify a custom domain name in an Azure Active Directory tenant, see [Add a custom domain name to your directory](/active-directory/active-directory-add-domain).</span></span>

<span data-ttu-id="96e9d-136">Ebben a példában a "Alapértelmezett bérlőt Azure" könyvtárban található csak azokat a felhasználókat, a tartomány nevét "@alflanigan.onmicrosoft.com".</span><span class="sxs-lookup"><span data-stu-id="96e9d-136">In this example, the "Default tenant Azure" directory contains only users with the domain name "@alflanigan.onmicrosoft.com".</span></span>

<span data-ttu-id="96e9d-137">Az előfizetés kiválasztása után a rendszergazda felhasználó kattintson kell **hozzáférés-vezérlés (IAM)** , majd **új szerepkör hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="96e9d-137">After selecting the subscription, the admin user must click **Access Control (IAM)** and then **Add a new role**.</span></span>





![hozzáférés-vezérlési IAM funkciója Azure-portálon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/1.png)





![Új felhasználó hozzáadása a hozzáférés-vezérlési IAM funkciója Azure-portálon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/2.png)

<span data-ttu-id="96e9d-140">A következő lépés, hogy válassza ki hozzá kell rendelni a szerepkört és a felhasználó, akinek a Szerepalapú szerepkör rendeli hozzá.</span><span class="sxs-lookup"><span data-stu-id="96e9d-140">The next step is to select the role to be assigned and the user whom the RBAC role will be assigned to.</span></span> <span data-ttu-id="96e9d-141">Az a **szerepkör** legördülő menüjében a rendszergazdai jogú felhasználó számára megjelenített csak a beépített RBAC szerepkörök, amelyek elérhetők az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="96e9d-141">In the **Role** dropdown menu the admin user sees only the built-in RBAC roles which are available in Azure.</span></span> <span data-ttu-id="96e9d-142">Részletesebb ismereteket szeretnének elsajátítani a minden egyes szerepkör és a hozzárendelhető hatókörök, lásd: [átruházásához hozzáférés-vezérlés beépített szerepkörök](/active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="96e9d-142">For more detailed explanations of each role and their assignable scopes, see [Built-in roles for Azure Role-Based Access Control](/active-directory/role-based-access-built-in-roles.md).</span></span>

<span data-ttu-id="96e9d-143">A rendszergazdai jogú felhasználó majd hozzá kell a külső felhasználó e-mail címe.</span><span class="sxs-lookup"><span data-stu-id="96e9d-143">The admin user then needs to add the email address of the external user.</span></span> <span data-ttu-id="96e9d-144">A várt működése a külső felhasználó számára nem jelenik meg a meglévő bérlő.</span><span class="sxs-lookup"><span data-stu-id="96e9d-144">The expected behavior is for the external user to not show up in the existing tenant.</span></span> <span data-ttu-id="96e9d-145">Után a külső felhasználó kérték, ő lesz látható a **előfizetések > hozzáférés-vezérlés (IAM)** az aktuális felhasználókkal, amely már hozzá vannak rendelve az RBAC szerepkört az előfizetés hatókörben.</span><span class="sxs-lookup"><span data-stu-id="96e9d-145">After the external user has been invited, he will be visible under **Subscriptions > Access Control (IAM)** with all the current users which are currently assigned an RBAC role at the Subscription scope.</span></span>





![engedélyek hozzáadása új RBAC-szerepkör](./media/role-based-access-control-create-custom-roles-for-internal-external-users/3.png)





![előfizetés szintjén RBAC-szerepkörök listája](./media/role-based-access-control-create-custom-roles-for-internal-external-users/4.png)

<span data-ttu-id="96e9d-148">A felhasználó "chessercarlton@gmail.com" kell meghívott egy **tulajdonos** az "Ingyenes" előfizetés.</span><span class="sxs-lookup"><span data-stu-id="96e9d-148">The user "chessercarlton@gmail.com" has been invited to be an **Owner** for the “Free Trial” subscription.</span></span> <span data-ttu-id="96e9d-149">Meghívást, követően a külső felhasználó kap egy e-mailes megerősítés egy aktiválási hivatkozást tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="96e9d-149">After sending the invitation, the external user will receive an email confirmation with an activation link.</span></span>
<span data-ttu-id="96e9d-150">![e-mailek meghívó RBAC-szerepkör](./media/role-based-access-control-create-custom-roles-for-internal-external-users/5.png)</span><span class="sxs-lookup"><span data-stu-id="96e9d-150">![email invitation for RBAC role](./media/role-based-access-control-create-custom-roles-for-internal-external-users/5.png)</span></span>

<span data-ttu-id="96e9d-151">Folyamatban a szervezeten kívül, az új felhasználó nem rendelkezik meglévő attribútuma "Alapértelmezett bérlőt Azure" könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="96e9d-151">Being external to the organization, the new user does not have any existing attributes in the "Default tenant Azure" directory.</span></span> <span data-ttu-id="96e9d-152">Azok a rendszer létrehozza, miután a külső felhasználó hozzájárult rögzítsen a rendszer a könyvtárban, amely az előfizetéshez, amely rendelt szerepkör.</span><span class="sxs-lookup"><span data-stu-id="96e9d-152">They will be created after the external user has given consent to be recorded in the directory which is associated with the subscription which he has been assigned a role to.</span></span>





![meghívót e-mailt az RBAC-szerepkör](./media/role-based-access-control-create-custom-roles-for-internal-external-users/6.png)

<span data-ttu-id="96e9d-154">A külső felhasználó azt mutatja be az Azure Active Directory-bérlő mostantól külső felhasználóként, és ez az Azure portálon, és a klasszikus portálon tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="96e9d-154">The external user shows in the Azure Active Directory tenant from now on as external user and this can be viewed both in the Azure portal and in the classic portal.</span></span>





![felhasználók panel az azure active Directoryval Azure-portálon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/7.png)





![felhasználók panel az azure active Directoryval a klasszikus Azure portálon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/8.png)

<span data-ttu-id="96e9d-157">Az a **felhasználók** mindkét portálok a külső felhasználók felismeri a nézet:</span><span class="sxs-lookup"><span data-stu-id="96e9d-157">In the **Users** view in both portals the external users can be recognized by:</span></span>

* <span data-ttu-id="96e9d-158">A különböző ikon írja be az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="96e9d-158">The different icon type in the Azure portal</span></span>
* <span data-ttu-id="96e9d-159">A különböző sourcing pont a klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="96e9d-159">The different sourcing point in the classic portal</span></span>

<span data-ttu-id="96e9d-160">Azonban biztosítása **tulajdonos** vagy **közreműködő** a külső felhasználók elérését a **előfizetés** hatókörét, nem engedélyezi a hozzáférést a rendszergazda felhasználó könyvtárába, kivéve, ha a **Globális rendszergazda** lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="96e9d-160">However, granting **Owner** or **Contributor** access to an external user at the **Subscription** scope, does not allow the access to the admin user's directory, unless the **Global Admin** allows it.</span></span> <span data-ttu-id="96e9d-161">A felhasználói tulajdonságokat a **felhasználótípust** két közös paramétert tartalmaz **tag** és **vendég** azonosítható legyen.</span><span class="sxs-lookup"><span data-stu-id="96e9d-161">In the user proprieties,  the **User Type** which has two common parameters, **Member** and **Guest** can be identified.</span></span> <span data-ttu-id="96e9d-162">Egy tag a felhasználó, amely a címtárban regisztrálva van, míg a Vendég egy meghívót, hogy a könyvtár külső forrásból származó felhasználói.</span><span class="sxs-lookup"><span data-stu-id="96e9d-162">A member is a user which is registered in the directory while a guest is a user invited to the directory from an external source.</span></span> <span data-ttu-id="96e9d-163">További információkért lásd: [hogyan Azure Active Directory rendszergazdák hozzá B2B együttműködés felhasználók](/active-directory/active-directory-b2b-admin-add-users).</span><span class="sxs-lookup"><span data-stu-id="96e9d-163">For more information, see [How do Azure Active Directory admins add B2B collaboration users](/active-directory/active-directory-b2b-admin-add-users).</span></span>

> [!NOTE]
> <span data-ttu-id="96e9d-164">Győződjön meg arról, hogy a portál a hitelesítő adatok megadása után, a külső felhasználó kiválasztja a megfelelő könyvtárban való bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="96e9d-164">Make sure that after entering the credentials in the portal, the external user selects the correct directory to sign-in to.</span></span> <span data-ttu-id="96e9d-165">Azonos is van a több könyvtárak eléréséhez és az Azure-portálon jobb felső felhasználónév kattintva válassza ki vagy az egyik és a felhasználónak a legördülő listából válassza ki a megfelelő könyvtár.</span><span class="sxs-lookup"><span data-stu-id="96e9d-165">The same user can have access to multiple directories and can select either one of  them by clicking the username in the top right-hand side in the Azure portal and then choose the appropriate directory from the dropdown list.</span></span>

<span data-ttu-id="96e9d-166">A Vendég a címtárban, miközben a külső felhasználó kezelheti az Azure-előfizetéshez tartozó összes erőforrást, de a könyvtár nem hozzáférhető.</span><span class="sxs-lookup"><span data-stu-id="96e9d-166">While being a guest in the directory, the external user can manage all resources for the Azure subscription, but can't access the directory.</span></span>





![az azure active Directoryval az Azure-portálhoz korlátozott hozzáférés](./media/role-based-access-control-create-custom-roles-for-internal-external-users/9.png)

<span data-ttu-id="96e9d-168">Az Azure Active Directory és az Azure-előfizetés nem rendelkezik egy szülő-gyermek kapcsolat, például a más Azure-erőforrások (például: virtuális gépek, virtuális hálózatok, a webalkalmazások, tárolási stb.) az Azure-előfizetéssel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="96e9d-168">Azure Active Directory and an Azure subscription don't have a child-parent relation like other Azure resources (for example: virtual machines, virtual networks, web apps, storage etc.) have with an Azure subscription.</span></span> <span data-ttu-id="96e9d-169">Minden az utóbbi létrehozott, kezelt és fizetni az Azure-előfizetéssel, míg az Azure-előfizetések az Azure-címtár való hozzáférés kezelése.</span><span class="sxs-lookup"><span data-stu-id="96e9d-169">All the latter is created, managed and billed under an Azure subscription while an Azure subscription is used to manage the access to an Azure directory.</span></span> <span data-ttu-id="96e9d-170">További részletekért lásd: [hogyan egy Azure-előfizetéshez az Azure AD kapcsolódó](/active-directory/active-directory-how-subscriptions-associated-directory).</span><span class="sxs-lookup"><span data-stu-id="96e9d-170">For more details, see [How an Azure subscription is related to Azure AD](/active-directory/active-directory-how-subscriptions-associated-directory).</span></span>

<span data-ttu-id="96e9d-171">Az összes a beépített RBAC szerepkörből **tulajdonos** és **közreműködő** minden erőforrásoknak a környezetben, a különbség a teljes felügyeleti hozzáférést nyújtanak, előfordulhat, hogy egy közreműködői nem hozható létre, majd új RBAC-szerepkörök törlése .</span><span class="sxs-lookup"><span data-stu-id="96e9d-171">From all the built-in RBAC roles, **Owner** and **Contributor** offer full management access to all resources in the environment, the difference being that a Contributor can't create and delete new RBAC roles.</span></span> <span data-ttu-id="96e9d-172">A beépített szerepkörök, például **virtuális gép közreműködő** csak az erőforrásokat, függetlenül attól, hogy a név által jelzett összes felügyeleti hozzáférést nyújtanak a **erőforráscsoport** történő létrehozásuk alatt.</span><span class="sxs-lookup"><span data-stu-id="96e9d-172">The other built-in roles like **Virtual Machine Contributor** offer full management access only to the resources indicated by the name, regardless of the **Resource Group** they are being created into.</span></span>

<span data-ttu-id="96e9d-173">A beépített RBAC szerepe hozzárendelése **virtuális gép közreműködő** egy előfizetés szintjén azt jelenti, hogy a felhasználó a szerepét:</span><span class="sxs-lookup"><span data-stu-id="96e9d-173">Assigning the built-in RBAC role of **Virtual Machine Contributor** at a subscription level, means that the user assigned the role:</span></span>

* <span data-ttu-id="96e9d-174">Megtekintheti az összes virtuális gép függetlenül azok telepítési dátumát és az erőforráscsoportok részét képezik</span><span class="sxs-lookup"><span data-stu-id="96e9d-174">Can view all virtual machines regardless their deployment date and the resource groups they are part of</span></span>
* <span data-ttu-id="96e9d-175">A virtuális gépek összes felügyeleti hozzáféréssel rendelkezik az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="96e9d-175">Has full management access to the virtual machines in the subscription</span></span>
* <span data-ttu-id="96e9d-176">Az előfizetést nem lehet megtekinteni, bármilyen más típusú erőforrások</span><span class="sxs-lookup"><span data-stu-id="96e9d-176">Can't view any other resource types in the subscription</span></span>
* <span data-ttu-id="96e9d-177">A módosításokat a számlázási szempontjából nem tud működni.</span><span class="sxs-lookup"><span data-stu-id="96e9d-177">Can't operate any changes from a billing perspective</span></span>

> [!NOTE]
> <span data-ttu-id="96e9d-178">Az RBAC alatt az Azure portál csak szolgáltatásai, azt nem adja meg a klasszikus portál eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="96e9d-178">RBAC being an Azure portal only feature, it doesn't grant access to the classic portal.</span></span>

## <a name="assign-a-built-in-rbac-role-to-an-external-user"></a><span data-ttu-id="96e9d-179">Beépített RBAC szerepkör hozzárendelése egy külső felhasználó</span><span class="sxs-lookup"><span data-stu-id="96e9d-179">Assign a built-in RBAC role to an external user</span></span>
<span data-ttu-id="96e9d-180">A különböző esetén ez a vizsgálat, a külső felhasználó a "alflanigan@gmail.com" meg van adva egy **virtuális gép közreműködő**.</span><span class="sxs-lookup"><span data-stu-id="96e9d-180">For a different scenario in this test, the external user "alflanigan@gmail.com" is added as a **Virtual Machine Contributor**.</span></span>




![virtuális gép közreműködő beépített szerepkör](./media/role-based-access-control-create-custom-roles-for-internal-external-users/11.png)

<span data-ttu-id="96e9d-182">A normál beépített szerephez a külső felhasználó működése áttekinthetők és felügyelhetők a csak virtuális gépek és a szomszédos erőforrás-kezelő csak szükséges erőforrások telepítése közben.</span><span class="sxs-lookup"><span data-stu-id="96e9d-182">The normal behavior for this external user with this built-in role is to see and manage only virtual machines and their adjacent Resource Manager only resources necessary while deploying.</span></span> <span data-ttu-id="96e9d-183">Úgy lett kialakítva, a korlátozott szerepkörök ajánlat csak a saját Azure-portálon létrehozott levelező erőforrásokhoz való hozzáférés, attól függetlenül történik néhány továbbra is telepíthető a klasszikus portálon (például: virtuális gépek).</span><span class="sxs-lookup"><span data-stu-id="96e9d-183">By design, these limited roles offer access only to their correspondent resources created in the Azure portal, regardless some can still be deployed in the classic portal as well (for example: virtual machines).</span></span>





![virtuális gép közreműködői szerepkör áttekintése azure-portálon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/12.png)

## <a name="grant-access-at-a-subscription-level-for-a-user-in-the-same-directory"></a><span data-ttu-id="96e9d-185">Hozzáférés egy felhasználó ugyanabban a könyvtárban egy előfizetés szintjén</span><span class="sxs-lookup"><span data-stu-id="96e9d-185">Grant access at a subscription level for a user in the same directory</span></span>
<span data-ttu-id="96e9d-186">A folyamat megegyezik a külső felhasználók hozzáadása, mind a felügyelet szempontjából megadását a Szerepalapú szerepkör, valamint a felhasználói hozzáférést megkapják a szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="96e9d-186">The process flow is identical to adding an external user, both from the admin perspective granting the RBAC role as well as the user being granted access to the role.</span></span> <span data-ttu-id="96e9d-187">Itt különbség, hogy a meghívott felhasználók nem kapják meg minden e-mailek meghívókat, a bejelentkezés után az előfizetésen belüli összes erőforrás hatókör az irányítópult elérhető lesz.</span><span class="sxs-lookup"><span data-stu-id="96e9d-187">The difference here is that the invited user will not receive any email invitations as all the resource scopes within the subscription will be available in the dashboard after signing in.</span></span>

## <a name="assign-rbac-roles-at-the-resource-group-scope"></a><span data-ttu-id="96e9d-188">Az erőforrás csoport hatókörű RBAC-szerepkörök hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="96e9d-188">Assign RBAC roles at the resource group scope</span></span>
<span data-ttu-id="96e9d-189">Hozzárendelése a Szerepalapú szerepet egy **erőforráscsoport** hatókörben van egy azonos folyamata hozzárendelése a szerepkört az előfizetés szintjén mindkét típusú felhasználók – külső vagy belső (könyvtárába része).</span><span class="sxs-lookup"><span data-stu-id="96e9d-189">Assigning an RBAC role at a **Resource Group** scope has an identical process for assigning the role at the subscription level, for both types of users - either external or internal (part of the same directory).</span></span> <span data-ttu-id="96e9d-190">A felhasználók, amelyek az RBAC-szerepkör, hogy tekintse meg a környezetben csak az erőforráscsoport van hozzájuk rendelve való hozzáférést a **erőforráscsoportok** ikon az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="96e9d-190">The users which are assigned the RBAC role is to see in their environment only the resource group they have been assigned access from the **Resource Groups** icon in the Azure portal.</span></span>

## <a name="assign-rbac-roles-at-the-resource-scope"></a><span data-ttu-id="96e9d-191">Az erőforrás-hatókörben RBAC-szerepkörök hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="96e9d-191">Assign RBAC roles at the resource scope</span></span>
<span data-ttu-id="96e9d-192">Hozzárendelése az Azure-erőforrás hatókörre RBAC szerepet van egy azonos folyamata hozzárendelése a szerepkört az előfizetés szintjén vagy az erőforráscsoport szintjén, a következő mindkét forgatókönyvet ugyanabban a munkafolyamatban.</span><span class="sxs-lookup"><span data-stu-id="96e9d-192">Assigning an RBAC role at a resource scope in Azure has an identical process for assigning the role at the subscription level or at the resource group level, following the same workflow for both scenarios.</span></span> <span data-ttu-id="96e9d-193">Ebben az esetben a felhasználók, amelyek az RBAC-szerepkör csak akkor hozzárendelt hozzáférés, vagy elemek látható a **összes erőforrás** lapon vagy közvetlenül az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="96e9d-193">Again, the users which are assigned the RBAC role can see only the items that they have been assigned access to, either in the **All Resources** tab or directly in their dashboard.</span></span>

<span data-ttu-id="96e9d-194">Az RBAC egyaránt erőforrás csoporthatókör vagy erőforrás hatókör fontos eleme a felhasználók számára, ügyeljen arra, hogy jelentkezzen be a megfelelő könyvtárban van.</span><span class="sxs-lookup"><span data-stu-id="96e9d-194">An important aspect for RBAC both at resource group scope or resource scope is for the users to make sure to sign-in to the correct directory.</span></span>





![Directory bejelentkezés az Azure-portálon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/13.png)

## <a name="assign-rbac-roles-for-an-azure-active-directory-group"></a><span data-ttu-id="96e9d-196">Egy Azure Active Directory csoport RBAC-szerepkörök hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="96e9d-196">Assign RBAC roles for an Azure Active Directory group</span></span>
<span data-ttu-id="96e9d-197">Az RBAC használata az Azure-ban három különböző hatóköröket, minden helyzet kezeléséhez, a központi telepítését és a különböző erőforrások felügyelete személyes előfizetés kezelése igénye nélkül hozzárendelt felhasználóként jogosultság kínálnak.</span><span class="sxs-lookup"><span data-stu-id="96e9d-197">All the scenarios using RBAC at the three different scopes in Azure offer the privilege of managing, deploying and administering various resources as an assigned user without the need of managing a personal subscription.</span></span> <span data-ttu-id="96e9d-198">Attól függetlenül történik az RBAC szerepkör tartozik előfizetés, erőforráscsoporthoz vagy erőforrás hatókör, az összes az erőforrásokat a hozzárendelt felhasználók által létrehozott további a számlázása alatt, ahol a felhasználók rendelkeznek-e a hozzáférést egy Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="96e9d-198">Regardless the RBAC role is assigned for a subscription, resource group or resource scope, all the resources created further on by the assigned users are billed under the one Azure subscription where the users have access to.</span></span> <span data-ttu-id="96e9d-199">Ezzel a módszerrel számlázási a teljes Azure-előfizetéshez rendszergazdai jogosultságokkal rendelkező felhasználók rendelkezik teljes áttekintése a felhasználás, függetlenül az erőforrások kezel.</span><span class="sxs-lookup"><span data-stu-id="96e9d-199">This way, the users who have billing administrator permissions for that entire Azure subscription has a complete overview on the consumption, regardless who is managing the resources.</span></span>

<span data-ttu-id="96e9d-200">Nagyobb szervezeteknek figyelembe véve, hogy a felhasználót a rendszergazda hozzáférést szeretne biztosítani a részletes csoportok vagy részlegek teljes, nem különállóan minden felhasználónak, így annak eldöntéséhez, hogy a terv az Active Directory-csoportok esetében ugyanúgy alkalmazható RBAC-szerepkörök Ez különösen idő és a felügyeleti hatékony lehetőség.</span><span class="sxs-lookup"><span data-stu-id="96e9d-200">For larger organizations, RBAC roles can be applied in the same way for Azure Active Directory groups considering the perspective that the admin user wants to grant the granular access for teams or entire departments, not individually for each user, thus considering it as an extremely time and management efficient option.</span></span> <span data-ttu-id="96e9d-201">Ebben a példában mutatja be a **közreműködő** szerepkört az előfizetés szintjén a bérlő csoportok egyikéhez bővült.</span><span class="sxs-lookup"><span data-stu-id="96e9d-201">To illustrate this example, the **Contributor** role has been added to one of the groups in the tenant at the subscription level.</span></span>





![az AAD-csoportokat RBAC-szerepkör hozzáadása](./media/role-based-access-control-create-custom-roles-for-internal-external-users/14.png)

<span data-ttu-id="96e9d-203">Ezek a csoportok üzembe helyezve, és csak az Azure Active Directoryban felügyelni biztonsági csoportokat.</span><span class="sxs-lookup"><span data-stu-id="96e9d-203">These groups are security groups which are provisioned and managed only within Azure Active Directory.</span></span>

## <a name="create-a-custom-rbac-role-to-open-support-requests-using-powershell"></a><span data-ttu-id="96e9d-204">Nyissa meg a támogatási kérelmek PowerShell használatával történő egyéni RBAC szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="96e9d-204">Create a custom RBAC role to open support requests using PowerShell</span></span>
<span data-ttu-id="96e9d-205">A beépített RBAC-szerepkörök az Azure-ban rendelkezésre álló gondoskodjon arról, hogy bizonyos jogosultsági szintek a környezetben elérhető erőforrások alapján.</span><span class="sxs-lookup"><span data-stu-id="96e9d-205">The built-in RBAC roles which are available in Azure ensure certain permission levels based on the available resources in the environment.</span></span> <span data-ttu-id="96e9d-206">Azonban ezek a szerepkörök egyike a rendszergazdai jogú felhasználó saját igényeinek megfelelően, ha nincs a beállítás a egyéni RBAC-szerepkörök létrehozásával még jobban hozzáférés korlátozásához.</span><span class="sxs-lookup"><span data-stu-id="96e9d-206">However, if none of these roles suit the admin user's needs, there is the option to limit access even more by creating custom RBAC roles.</span></span>

<span data-ttu-id="96e9d-207">Egyéni RBAC-szerepkörök létrehozásához egy beépített szerep, szerkesztheti, majd importálja vissza a környezetben.</span><span class="sxs-lookup"><span data-stu-id="96e9d-207">Creating custom RBAC roles requires to take one built-in role, edit it and then import it back in the environment.</span></span> <span data-ttu-id="96e9d-208">A letöltés és a szerepkör feltöltése felügyelt PowerShell vagy a parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="96e9d-208">The download and upload of the role are managed using either PowerShell or CLI.</span></span>

<span data-ttu-id="96e9d-209">Fontos megismerni az előfeltételeket részletes hozzáférést biztosíthat az előfizetés szintjén és is lehetővé teszi a meghívott felhasználó rugalmasan megnyitása támogatási kérelmek létrehozása egy egyéni biztonsági szerepkört.</span><span class="sxs-lookup"><span data-stu-id="96e9d-209">It is important to understand the prerequisites of creating a custom role which can grant granular access at the subscription level and also allow the invited user the flexibility of opening support requests.</span></span>

<span data-ttu-id="96e9d-210">Ehhez a példához a beépített szerepkör **olvasó** engedélyezése a felhasználó a lehetőség a támogatási kérelmek megnyitása testreszabása lehetővé teszi a felhasználók elérését a erőforrás hatókörök megtekintése, de nem szerkesztheti azokat, vagy hozzon létre újakat.</span><span class="sxs-lookup"><span data-stu-id="96e9d-210">For this example the built-in role **Reader** which allows users access to view all the resource scopes but not to edit them or create new ones has been customized to allow the user the option of opening support requests.</span></span>

<span data-ttu-id="96e9d-211">Az első műveletet exportáló a **olvasó** emelt jogosultsági szintű rendszergazdaként futtatott szerepkör kell PowerShell leforgása alatt elvégezhetők.</span><span class="sxs-lookup"><span data-stu-id="96e9d-211">The first action of exporting the **Reader** role needs to be completed in PowerShell ran with elevated permissions as administrator.</span></span>

```
Login-AzureRMAccount

Get-AzureRMRoleDefinition -Name "Reader"

Get-AzureRMRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\rbacrole2.json

```





![Az olvasó RBAC szerepkör PowerShell képernyőképe](./media/role-based-access-control-create-custom-roles-for-internal-external-users/15.png)

<span data-ttu-id="96e9d-213">Majd, vissza kell fejteni a JSON-sablon a szerepkör.</span><span class="sxs-lookup"><span data-stu-id="96e9d-213">Then you need to extract the JSON template of the role.</span></span>





![Egyéni olvasó RBAC szerepkör JSON-sablon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/16.png)

<span data-ttu-id="96e9d-215">Egy tipikus RBAC szerepkör áll kívül három fő szakasz **műveletek**, **NotActions** és **AssignableScopes**.</span><span class="sxs-lookup"><span data-stu-id="96e9d-215">A typical RBAC role is composed out of three main sections, **Actions**, **NotActions** and **AssignableScopes**.</span></span>

<span data-ttu-id="96e9d-216">Az a **művelet** című szakaszában felsorolt összes engedélyezett műveletek ehhez a szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="96e9d-216">In the **Action** section are listed all the permitted operations for this role.</span></span> <span data-ttu-id="96e9d-217">Fontos megérteni, hogy minden műveletet olyan erőforrás-szolgáltató van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="96e9d-217">It's important to understand that each action is assigned from a resource provider.</span></span> <span data-ttu-id="96e9d-218">Ebben az esetben a támogatási jegyek létrehozásának képességét a **Microsoft.Support** szerepelnie kell az erőforrás-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="96e9d-218">In this case, for creating support tickets the **Microsoft.Support** resource provider must be listed.</span></span>

<span data-ttu-id="96e9d-219">Ahhoz, hogy összes az erőforrás-szolgáltató elérhető és az előfizetéshez regisztrált, használhatja a PowerShell.</span><span class="sxs-lookup"><span data-stu-id="96e9d-219">To be able to see all the resource providers available and registered in your subscription, you can use PowerShell.</span></span>
```
Get-AzureRMResourceProvider

```
<span data-ttu-id="96e9d-220">Ezenkívül ellenőrizheti az összes elérhető PowerShell parancsmagjainak az erőforrás-szolgáltatók kezelése.</span><span class="sxs-lookup"><span data-stu-id="96e9d-220">Additionally, you can check for the all the available PowerShell cmdlets to manage the resource providers.</span></span>
    <span data-ttu-id="96e9d-221">![Az erőforrás-szolgáltató management PowerShell képernyőképe](./media/role-based-access-control-create-custom-roles-for-internal-external-users/17.png)</span><span class="sxs-lookup"><span data-stu-id="96e9d-221">![PowerShell screenshot for resource provider management](./media/role-based-access-control-create-custom-roles-for-internal-external-users/17.png)</span></span>

<span data-ttu-id="96e9d-222">Az adott RBAC-szerepkörök a műveletek korlátozása, erőforrás-szolgáltatók szakaszban felsorolt **NotActions**.</span><span class="sxs-lookup"><span data-stu-id="96e9d-222">To restrict all the actions for a particular RBAC role, resource providers are listed under the section **NotActions**.</span></span>
<span data-ttu-id="96e9d-223">Utolsó akkor kötelező, hogy a Szerepalapú szerepkör tartalmazza a explicit előfizetési azonosítók felhasználási.</span><span class="sxs-lookup"><span data-stu-id="96e9d-223">Last, it's mandatory that the RBAC role contains the explicit subscription IDs where it is used.</span></span> <span data-ttu-id="96e9d-224">Az előfizetési azonosítók alatt a **AssignableScopes**, ellenkező esetben azt nem használhatók az előfizetésében szerepkört importálni.</span><span class="sxs-lookup"><span data-stu-id="96e9d-224">The subscription IDs are listed under the **AssignableScopes**, otherwise you will not be allowed to import the role in your subscription.</span></span>

<span data-ttu-id="96e9d-225">Után létrehozása és testreszabása az RBAC-szerepkör, importálandók kell biztonsági másolatot a környezet.</span><span class="sxs-lookup"><span data-stu-id="96e9d-225">After creating and customizing the RBAC role, it needs to be imported back the environment.</span></span>

```
New-AzureRMRoleDefinition -InputFile "C:\rbacrole2.json"

```

<span data-ttu-id="96e9d-226">Ebben a példában a Szerepalapú szerepkörhöz tartozó egyéni neve "Olvasó támogatási jegyek hozzáférési szint" a felhasználó az előfizetéshez mindent megtekinthetnek és nyissa meg a támogatási kérelmek.</span><span class="sxs-lookup"><span data-stu-id="96e9d-226">In this example, the custom name for this RBAC role is "Reader support tickets access level" allowing the user to view everything in the subscription and also to open support requests.</span></span>

> [!NOTE]
> <span data-ttu-id="96e9d-227">A támogatási kérelmek nyitó művelet engedélyezése csak két beépített RBAC szerep **tulajdonos** és **közreműködő**.</span><span class="sxs-lookup"><span data-stu-id="96e9d-227">The only two built-in RBAC roles allowing the action of opening of support requests are **Owner** and **Contributor**.</span></span> <span data-ttu-id="96e9d-228">A felhasználó megnyithatja a támogatási kérelmek ő szerepkört kell hozzárendelni egy Szerepalapú csak az előfizetési hatókört, mert összes támogatási kérelmek létrehozása az Azure-előfizetés alapján.</span><span class="sxs-lookup"><span data-stu-id="96e9d-228">For a user to be able to open support requests, he must be assigned an RBAC role only at the subscription scope, because all support requests are created based on an Azure subscription.</span></span>

<span data-ttu-id="96e9d-229">Az új egyéni szerepkör van rendelve egy felhasználó, az ugyanabban a könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="96e9d-229">This new custom role has been assigned to an user from the same directory.</span></span>





![az Azure portálon importálni egyéni RBAC szerepkör képernyőképe](./media/role-based-access-control-create-custom-roles-for-internal-external-users/18.png)





![Képernyőkép a egyéni importált RBAC szerepkör hozzárendelése felhasználói ugyanabban a könyvtárban](./media/role-based-access-control-create-custom-roles-for-internal-external-users/19.png)





![egyéni importált RBAC szerepkör engedélyeinek képernyőképe](./media/role-based-access-control-create-custom-roles-for-internal-external-users/20.png)

<span data-ttu-id="96e9d-233">A példa további részletes hogy az egyéni RBAC szerepkör határain hangsúlyozzák az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="96e9d-233">The example has been further detailed to emphasize the limits of this custom RBAC role as follows:</span></span>
* <span data-ttu-id="96e9d-234">Új támogatási kérelmek hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="96e9d-234">Can create new support requests</span></span>
* <span data-ttu-id="96e9d-235">Nem hozható létre új erőforrás hatókörök (például: virtuális gép)</span><span class="sxs-lookup"><span data-stu-id="96e9d-235">Can't create new resource scopes (for example: virtual machine)</span></span>
* <span data-ttu-id="96e9d-236">Nem hozható létre új erőforrás-csoportok</span><span class="sxs-lookup"><span data-stu-id="96e9d-236">Can't create new resource groups</span></span>





![Képernyőkép a támogatási kérelmek létrehozása egyéni RBAC szerepkör](./media/role-based-access-control-create-custom-roles-for-internal-external-users/21.png)





![nem sikerült létrehozni a virtuális gépek egyéni RBAC szerepkör képernyőképe](./media/role-based-access-control-create-custom-roles-for-internal-external-users/22.png)





![nem sikerült létrehozni az új RGs egyéni RBAC szerepkör képernyőképe](./media/role-based-access-control-create-custom-roles-for-internal-external-users/23.png)

## <a name="create-a-custom-rbac-role-to-open-support-requests-using-azure-cli"></a><span data-ttu-id="96e9d-240">Nyissa meg a támogatási kérelmek Azure parancssori felület használatával történő egyéni RBAC szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="96e9d-240">Create a custom RBAC role to open support requests using Azure CLI</span></span>
<span data-ttu-id="96e9d-241">A Mac és a PowerShell való hozzáférés nélkül fut, az Azure parancssori felület módja a nyissa meg.</span><span class="sxs-lookup"><span data-stu-id="96e9d-241">Running on a Mac and without having access to PowerShell, Azure CLI is the way to go.</span></span>

<span data-ttu-id="96e9d-242">Egy egyéni biztonsági szerepkört létrehozásának a lépései megegyeznek, CLI-vel a szerepkör nem tölti le a JSON-sablon, de a a parancssori Felülettel megtekinthetők kivételével.</span><span class="sxs-lookup"><span data-stu-id="96e9d-242">The steps to create a custom role are the same, with the sole exception that using CLI the role can't be downloaded in a JSON template, but it can be viewed in the CLI.</span></span>

<span data-ttu-id="96e9d-243">Ehhez a példához I választotta, a beépített szerepkör **biztonsági mentés olvasó**.</span><span class="sxs-lookup"><span data-stu-id="96e9d-243">For this example I have chosen the built-in role of **Backup Reader**.</span></span>

```

azure role show "backup reader" --json

```





![Parancssori felület Képernyőkép a biztonsági mentési olvasó szerepkört megjelenítése](./media/role-based-access-control-create-custom-roles-for-internal-external-users/24.png)

<span data-ttu-id="96e9d-245">A szerepkör a Visual Studio módosítása után a tulajdonságokat a JSON-sablon másolása a **Microsoft.Support** erőforrás-szolgáltató hozzá lett adva a **műveletek** részek, hogy a felhasználó megnyithatja támogatása kérelmek, miközben továbbra is a mentési tárolók olvasójának lehet.</span><span class="sxs-lookup"><span data-stu-id="96e9d-245">Editing the role in Visual Studio after copying the proprieties in a JSON template, the **Microsoft.Support** resource provider has been added in the **Actions** sections so that this user can open support requests while continuing to be a reader for the backup vaults.</span></span> <span data-ttu-id="96e9d-246">Újra kell hozzáadnia az előfizetés-azonosító, amelyeken ezt a szerepkört kell használni a rendszer a **AssignableScopes** szakasz.</span><span class="sxs-lookup"><span data-stu-id="96e9d-246">Again it is necessary to add the subscription ID where this role will be used in the **AssignableScopes** section.</span></span>

```

azure role create --inputfile <path>

```





![Egyéni RBAC szerepkör importálása CLI képernyőképe](./media/role-based-access-control-create-custom-roles-for-internal-external-users/25.png)

<span data-ttu-id="96e9d-248">Az új szerepkör már elérhető az Azure portálon, és a hozzárendeléseket folyamat megegyezik az előző példához hasonlóan.</span><span class="sxs-lookup"><span data-stu-id="96e9d-248">The new role is now available in the Azure portal and the assignation process is the same as in the previous examples.</span></span>





![Az Azure portál Képernyőkép a CLI 1.0 használatával létrehozott egyéni RBAC szerepkör](./media/role-based-access-control-create-custom-roles-for-internal-external-users/26.png)

<span data-ttu-id="96e9d-250">A legújabb Build 2017 frissítésétől az Azure-felhő rendszerhéj általánosan elérhető.</span><span class="sxs-lookup"><span data-stu-id="96e9d-250">As of the latest Build 2017, the Azure Cloud Shell is generally available.</span></span> <span data-ttu-id="96e9d-251">Azure Cloud rendszerhéj egészíti ki IDE és az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="96e9d-251">Azure Cloud Shell is a complement to IDE and the Azure Portal.</span></span> <span data-ttu-id="96e9d-252">Ezzel a szolgáltatással hitelesítése és Azure-ban üzemeltetett egy webböngésző-alapú rendszerhéj kap, és használhatja a számítógépen telepített parancssori felület helyett.</span><span class="sxs-lookup"><span data-stu-id="96e9d-252">With this service, you get a browser-based shell that is authenticated and hosted within Azure and you can use it instead of CLI installed on your machine.</span></span>





![Azure-felhőbe rendszerhéj](./media/role-based-access-control-create-custom-roles-for-internal-external-users/27.png)
