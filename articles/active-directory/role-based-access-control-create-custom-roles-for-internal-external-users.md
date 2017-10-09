---
title: "egyéni szerepkörök aaaCreate a szerepköralapú hozzáférés-vezérlés, és rendelje hozzá a toointernal és a külső felhasználók az Azure-ban |} Microsoft Docs"
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
ms.openlocfilehash: 26793a66d6ca2f771338eed87d10ce2b3b431841
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="intro-on-role-based-access-control"></a><span data-ttu-id="51ac4-103">A szerepköralapú hozzáférés-vezérlés – bevezetés</span><span class="sxs-lookup"><span data-stu-id="51ac4-103">Intro on role-based access control</span></span>

<span data-ttu-id="51ac4-104">Szerepköralapú hozzáférés-vezérlés az Azure portál csak szolgáltatása: lehetővé előfizetés hello tulajdonosainak tooassign részletes szerepkörök tooother felügyelheti az adott erőforrás hatókörök a környezetükben.</span><span class="sxs-lookup"><span data-stu-id="51ac4-104">Role-based access control is an Azure portal only feature allowing hello owners of a subscription tooassign granular roles tooother users who can manage specific resource scopes in their environment.</span></span>

<span data-ttu-id="51ac4-105">Szerepalapú lehetővé teszi, hogy jobb biztonságkezelés nagy méretű szervezeteknek, és az SMB-khez külső közreműködő, a szállítók vagy freelancers, amelyhez kell hozzáférhet a környezetben, de nem feltétlenül toohello teljes infrastruktúrát és bármely toospecific erőforrások használata a számlázással kapcsolatos hatókörök.</span><span class="sxs-lookup"><span data-stu-id="51ac4-105">RBAC allows better security management for large organizations and for SMBs working with external collaborators, vendors or freelancers which need access toospecific resources in your environment but not necessarily toohello entire infrastructure or any billing-related scopes.</span></span> <span data-ttu-id="51ac4-106">Szerepalapú lehetővé teszi, hogy a hello rugalmasan hello rendszergazdai fiókot (szolgáltatás-rendszergazda szerepkörrel egy előfizetés szintjén) által kezelt egy Azure-előfizetéssel rendelkező és rendelkezik a több meghívott felhasználók toowork hello ugyanahhoz az előfizetéshez de minden felügyeleti jogosultságok az.</span><span class="sxs-lookup"><span data-stu-id="51ac4-106">RBAC allows hello flexibility of owning one Azure subscription managed by hello administrator account (service administrator role at a subscription level) and have multiple users invited toowork under hello same subscription but without any administrative rights for it.</span></span> <span data-ttu-id="51ac4-107">Felügyeleti és számlázási szempontból hello RBAC szolgáltatás egy idő és a felügyeleti hatékony beállítása Azure használatával a különböző forgatókönyvekben bizonyítja toobe.</span><span class="sxs-lookup"><span data-stu-id="51ac4-107">From a management and billing perspective, hello RBAC feature proves toobe a time and management efficient option for using Azure in various scenarios.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="51ac4-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="51ac4-108">Prerequisites</span></span>
<span data-ttu-id="51ac4-109">Az RBAC használatát hello Azure-környezetéhez szükséges:</span><span class="sxs-lookup"><span data-stu-id="51ac4-109">Using RBAC in hello Azure environment requires:</span></span>

* <span data-ttu-id="51ac4-110">Önálló rendelkezik Azure-előfizetés hozzárendelt toohello felhasználó tulajdonosa (előfizetés szerepkör)</span><span class="sxs-lookup"><span data-stu-id="51ac4-110">Having a standalone Azure subscription assigned toohello user as owner (subscription role)</span></span>
* <span data-ttu-id="51ac4-111">Hello tulajdonos szerepe hello Azure-előfizetéssel rendelkezik</span><span class="sxs-lookup"><span data-stu-id="51ac4-111">Have hello Owner role of hello Azure subscription</span></span>
* <span data-ttu-id="51ac4-112">Hozzáférés toohello rendelkezik [Azure-portálon](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="51ac4-112">Have access toohello [Azure portal](https://portal.azure.com)</span></span>
* <span data-ttu-id="51ac4-113">Ellenőrizze, hogy a következő erőforrás-szolgáltató toohave hello hello felhasználói előfizetés regisztrálva: **Microsoft.Authorization**.</span><span class="sxs-lookup"><span data-stu-id="51ac4-113">Make sure toohave hello following Resource Providers registered for hello user subscription: **Microsoft.Authorization**.</span></span> <span data-ttu-id="51ac4-114">Hogyan tooregister hello erőforrás-szolgáltató további információkért lásd: [Resource Manager szolgáltatók, régiók, API verziók és sémák](/azure-resource-manager/resource-manager-supported-services.md).</span><span class="sxs-lookup"><span data-stu-id="51ac4-114">For more information on how tooregister hello resource providers, see [Resource Manager providers, regions, API versions and schemas](/azure-resource-manager/resource-manager-supported-services.md).</span></span>

> [!NOTE]
> <span data-ttu-id="51ac4-115">Office 365-előfizetéssel, vagy az Azure Active Directory-licencek (például: tooAzure Active Directory eléréséhez) kiosztott hello Office 365 portálon nem minőségi az RBAC használata.</span><span class="sxs-lookup"><span data-stu-id="51ac4-115">Office 365 subscriptions or Azure Active Directory licenses (for example: Access tooAzure Active Directory) provisioned from hello O365 portal don't quality for using RBAC.</span></span>

## <a name="how-can-rbac-be-used"></a><span data-ttu-id="51ac4-116">Hogyan lehet használni az RBAC</span><span class="sxs-lookup"><span data-stu-id="51ac4-116">How can RBAC be used</span></span>
<span data-ttu-id="51ac4-117">Az RBAC alkalmazni lehet az Azure-ban három különböző hatóköröket.</span><span class="sxs-lookup"><span data-stu-id="51ac4-117">RBAC can be applied at three different scopes in Azure.</span></span> <span data-ttu-id="51ac4-118">A hello legmagasabb hatókör toohello legalacsonyabb egyet akkor a következők:</span><span class="sxs-lookup"><span data-stu-id="51ac4-118">From hello highest scope toohello lowest one, they are as follows:</span></span>

* <span data-ttu-id="51ac4-119">Előfizetés (legmagasabb)</span><span class="sxs-lookup"><span data-stu-id="51ac4-119">Subscription (highest)</span></span>
* <span data-ttu-id="51ac4-120">Erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="51ac4-120">Resource group</span></span>
* <span data-ttu-id="51ac4-121">Erőforrás hatókör (hello legalacsonyabb hozzáférési szint ajánlat célzott engedélyek tooan egyes Azure-erőforrás hatókör)</span><span class="sxs-lookup"><span data-stu-id="51ac4-121">Resource scope (hello lowest access level offering targeted permissions tooan individual Azure resource scope)</span></span>

## <a name="assign-rbac-roles-at-hello-subscription-scope"></a><span data-ttu-id="51ac4-122">Hello előfizetés hatókörből RBAC-szerepkörök hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="51ac4-122">Assign RBAC roles at hello subscription scope</span></span>
<span data-ttu-id="51ac4-123">Nincsenek két gyakori példán RBAC használja (de nem kizárólagosan):</span><span class="sxs-lookup"><span data-stu-id="51ac4-123">There are two common examples when RBAC is used (but not limited to):</span></span>

* <span data-ttu-id="51ac4-124">Külső felhasználók hello szervezetek (nem hello rendszergazdai jogú felhasználó Azure Active Directory-bérlő része) rendelkező meghívott toomanage, bizonyos erőforrások vagy hello teljes előfizetés</span><span class="sxs-lookup"><span data-stu-id="51ac4-124">Having external users from hello organizations (not part of hello admin user's Azure Active Directory tenant) invited toomanage certain resources or hello whole subscription</span></span>
* <span data-ttu-id="51ac4-125">A felhasználók hello szervezet (részét képezik hello felhasználó Azure Active Directory-bérlő), de a különböző csapatok vagy csoportokat, amelyek részletes hozzáférésre van szükségük belső használata vagy toohello teljes előfizetés vagy toocertain erőforráscsoportok vagy az erőforrás hatóköröket a hello környezet</span><span class="sxs-lookup"><span data-stu-id="51ac4-125">Working with users inside hello organization (they are part of hello user's Azure Active Directory tenant) but part of different teams or groups which need granular access either toohello whole subscription or toocertain resource groups or resource scopes in hello environment</span></span>

## <a name="grant-access-at-a-subscription-level-for-a-user-outside-of-azure-active-directory"></a><span data-ttu-id="51ac4-126">Hozzáférés egy felhasználó Azure Active Directory kívül egy előfizetés szintjén</span><span class="sxs-lookup"><span data-stu-id="51ac4-126">Grant access at a subscription level for a user outside of Azure Active Directory</span></span>
<span data-ttu-id="51ac4-127">Az RBAC-szerepkörök csak akkor adhatók **tulajdonosok** hello előfizetés ezért hello rendszergazdai jogú felhasználó kell bejelentkeznie az előzetes szerepkörrel vagy hello Azure-előfizetést hozott létre egy felhasználónévvel.</span><span class="sxs-lookup"><span data-stu-id="51ac4-127">RBAC roles can be granted only by **Owners** of hello subscription therefore hello admin user must be logged with a username which has this role pre-assigned or has created hello Azure subscription.</span></span>

<span data-ttu-id="51ac4-128">Hello Azure-portálon, a követően bejelentkezhet rendszergazdaként, válassza ki "Előfizetések" és a kiválasztott hello kívánt egyet.</span><span class="sxs-lookup"><span data-stu-id="51ac4-128">From hello Azure portal, after you sign-in as admin, select “Subscriptions” and chose hello desired one.</span></span>
<span data-ttu-id="51ac4-129">![előfizetés panel az Azure-portálon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/0.png) alapértelmezés szerint ha hello rendszergazdai jogú felhasználó megvásárolta hello Azure-előfizetésre, hello felhasználói állapotúként jelenik meg **Fiókadminisztrátor**, a hello előfizetés szerepkör alatt.</span><span class="sxs-lookup"><span data-stu-id="51ac4-129">![subscription blade in Azure portal](./media/role-based-access-control-create-custom-roles-for-internal-external-users/0.png) By default, if hello admin user has purchased hello Azure subscription, hello user will show up as **Account Admin**, this being hello subscription role.</span></span> <span data-ttu-id="51ac4-130">Hello Azure-előfizetés szerepkörök további részletekért lásd: [kezelése hello előfizetés vagy szolgáltatások hozzáadása vagy módosítása Azure-rendszergazdai szerepkörök](/billing/billing-add-change-azure-subscription-administrator.md).</span><span class="sxs-lookup"><span data-stu-id="51ac4-130">For more details on hello Azure subscription roles, see [Add or change Azure administrator roles that manage hello subscription or services](/billing/billing-add-change-azure-subscription-administrator.md).</span></span>

<span data-ttu-id="51ac4-131">Ebben a példában a felhasználó hello "alflanigan@outlook.com" hello van **tulajdonos** az "Ingyenes" hello hello aad-ben az előfizetéshez bérlői "Alapértelmezett bérlőt Azure".</span><span class="sxs-lookup"><span data-stu-id="51ac4-131">In this example, hello user "alflanigan@outlook.com" is hello **Owner** of hello "Free Trial" subscription in hello AAD tenant "Default tenant Azure".</span></span> <span data-ttu-id="51ac4-132">Mivel ez a felhasználó hello Azure-előfizetéssel rendelkező hello hello létrehozója kezdeti Microsoft Account "Outlook" (Microsoft Account = Outlook, a működés közbeni stb.) hello alapértelmezett tartomány nevét ennél a bérlőnél a hozzáadott összes többi felhasználó számára lesz **"@alflaniganuoutlook.onmicrosoft.com"**.</span><span class="sxs-lookup"><span data-stu-id="51ac4-132">Since this user is hello creator of hello Azure subscription with hello initial Microsoft Account “Outlook” (Microsoft Account = Outlook, Live etc.) hello default domain name for all other users added in this tenant will be **"@alflaniganuoutlook.onmicrosoft.com"**.</span></span> <span data-ttu-id="51ac4-133">Úgy lett kialakítva, hello szintaxis hello új tartomány által létrehozott hello bérlői és hozzáadását hello bővítmény hello felhasználó felhasználónevét és tartományát neve hello üzembe formátuma **". onmicrosoft.com"**.</span><span class="sxs-lookup"><span data-stu-id="51ac4-133">By design, hello syntax of hello new domain is formed by putting together hello username and domain name of hello user who created hello tenant and adding hello extension **".onmicrosoft.com"**.</span></span>
<span data-ttu-id="51ac4-134">Ezenkívül felhasználók is jelentkezzen be egy egyéni tartománynevet hello bérlői hozzáadása és az új bérlő hello ellenőrzése után.</span><span class="sxs-lookup"><span data-stu-id="51ac4-134">Furthermore, users can sign-in with a custom domain name in hello tenant after adding and verifying it for hello new tenant.</span></span> <span data-ttu-id="51ac4-135">Hogyan tooverify egy egyéni tartománynevet, Azure Active Directory-bérlő: vonatkozó részletes információért [adja hozzá ezeket az egyéni tartomány nevét tooyour](/active-directory/active-directory-add-domain).</span><span class="sxs-lookup"><span data-stu-id="51ac4-135">For more details on how tooverify a custom domain name in an Azure Active Directory tenant, see [Add a custom domain name tooyour directory](/active-directory/active-directory-add-domain).</span></span>

<span data-ttu-id="51ac4-136">Ebben a példában hello "alapértelmezett bérlőt Azure" címtár csak a felhasználók hello tartománynévvel tartalmaz "@alflanigan.onmicrosoft.com".</span><span class="sxs-lookup"><span data-stu-id="51ac4-136">In this example, hello "Default tenant Azure" directory contains only users with hello domain name "@alflanigan.onmicrosoft.com".</span></span>

<span data-ttu-id="51ac4-137">Hello előfizetés kiválasztása után kattintson kell az hello rendszergazdai jogú felhasználó **hozzáférés-vezérlés (IAM)** , majd **új szerepkör hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="51ac4-137">After selecting hello subscription, hello admin user must click **Access Control (IAM)** and then **Add a new role**.</span></span>





![hozzáférés-vezérlési IAM funkciója Azure-portálon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/1.png)





![Új felhasználó hozzáadása a hozzáférés-vezérlési IAM funkciója Azure-portálon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/2.png)

<span data-ttu-id="51ac4-140">hello következő lépésre tooselect hello szerepkör toobe hozzárendelt és hello felhasználó, akinek hello RBAC szerepkör rendeli hozzá.</span><span class="sxs-lookup"><span data-stu-id="51ac4-140">hello next step is tooselect hello role toobe assigned and hello user whom hello RBAC role will be assigned to.</span></span> <span data-ttu-id="51ac4-141">A hello **szerepkör** legördülő menü hello rendszergazdai jogú felhasználó számára megjelenített csak hello beépített RBAC szerepkörök, amelyek elérhetők az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="51ac4-141">In hello **Role** dropdown menu hello admin user sees only hello built-in RBAC roles which are available in Azure.</span></span> <span data-ttu-id="51ac4-142">Részletesebb ismereteket szeretnének elsajátítani a minden egyes szerepkör és a hozzárendelhető hatókörök, lásd: [átruházásához hozzáférés-vezérlés beépített szerepkörök](/active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="51ac4-142">For more detailed explanations of each role and their assignable scopes, see [Built-in roles for Azure Role-Based Access Control](/active-directory/role-based-access-built-in-roles.md).</span></span>

<span data-ttu-id="51ac4-143">hello rendszergazdai jogú felhasználó majd kell hello külső felhasználó tooadd hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="51ac4-143">hello admin user then needs tooadd hello email address of hello external user.</span></span> <span data-ttu-id="51ac4-144">hello várt viselkedés hello külső felhasználói toonot belül megjelennek a hello meglévő bérlő számára.</span><span class="sxs-lookup"><span data-stu-id="51ac4-144">hello expected behavior is for hello external user toonot show up in hello existing tenant.</span></span> <span data-ttu-id="51ac4-145">Után hello külső felhasználói kérték, ő lesz látható a **előfizetések > hozzáférés-vezérlés (IAM)** már hozzá vannak rendelve egy Szerepalapú szerepkör hello előfizetési hatókört, amely minden hello aktuális felhasználókkal.</span><span class="sxs-lookup"><span data-stu-id="51ac4-145">After hello external user has been invited, he will be visible under **Subscriptions > Access Control (IAM)** with all hello current users which are currently assigned an RBAC role at hello Subscription scope.</span></span>





![engedélyek toonew RBAC-szerepkör hozzáadása](./media/role-based-access-control-create-custom-roles-for-internal-external-users/3.png)





![előfizetés szintjén RBAC-szerepkörök listája](./media/role-based-access-control-create-custom-roles-for-internal-external-users/4.png)

<span data-ttu-id="51ac4-148">hello felhasználói "chessercarlton@gmail.com" meghívott toobe lett egy **tulajdonos** hello "Ingyenes" előfizetés esetében.</span><span class="sxs-lookup"><span data-stu-id="51ac4-148">hello user "chessercarlton@gmail.com" has been invited toobe an **Owner** for hello “Free Trial” subscription.</span></span> <span data-ttu-id="51ac4-149">Hello meghívót küld, miután hello külső felhasználó kap egy e-mailes megerősítés egy aktiválási hivatkozást tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="51ac4-149">After sending hello invitation, hello external user will receive an email confirmation with an activation link.</span></span>
<span data-ttu-id="51ac4-150">![e-mailek meghívó RBAC-szerepkör](./media/role-based-access-control-create-custom-roles-for-internal-external-users/5.png)</span><span class="sxs-lookup"><span data-stu-id="51ac4-150">![email invitation for RBAC role](./media/role-based-access-control-create-custom-roles-for-internal-external-users/5.png)</span></span>

<span data-ttu-id="51ac4-151">Külső toohello szervezet folyamatban, hello új felhasználó nem rendelkezik meglévő attribútuma hello "Alapértelmezett bérlőt Azure" könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="51ac4-151">Being external toohello organization, hello new user does not have any existing attributes in hello "Default tenant Azure" directory.</span></span> <span data-ttu-id="51ac4-152">Miután hello külső felhasználói hozzájárulás toobe hello előfizetéshez, amely rendelt szerepkör hello directory rögzített adott létrejön.</span><span class="sxs-lookup"><span data-stu-id="51ac4-152">They will be created after hello external user has given consent toobe recorded in hello directory which is associated with hello subscription which he has been assigned a role to.</span></span>





![meghívót e-mailt az RBAC-szerepkör](./media/role-based-access-control-create-custom-roles-for-internal-external-users/6.png)

<span data-ttu-id="51ac4-154">hello Azure Active Directory-bérlő mostantól külső felhasználóként hello külső felhasználó látható, és ez hello Azure-portál és a klasszikus portálon hello lehet megtekinteni.</span><span class="sxs-lookup"><span data-stu-id="51ac4-154">hello external user shows in hello Azure Active Directory tenant from now on as external user and this can be viewed both in hello Azure portal and in hello classic portal.</span></span>





![felhasználók panel az azure active Directoryval Azure-portálon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/7.png)





![felhasználók panel az azure active Directoryval a klasszikus Azure portálon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/8.png)

<span data-ttu-id="51ac4-157">A hello **felhasználók** mindkét portálok hello külső felhasználók nézet felismeri:</span><span class="sxs-lookup"><span data-stu-id="51ac4-157">In hello **Users** view in both portals hello external users can be recognized by:</span></span>

* <span data-ttu-id="51ac4-158">hello más ikon típusának hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="51ac4-158">hello different icon type in hello Azure portal</span></span>
* <span data-ttu-id="51ac4-159">hello különböző forrás pont hello klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="51ac4-159">hello different sourcing point in hello classic portal</span></span>

<span data-ttu-id="51ac4-160">Azonban biztosítása **tulajdonos** vagy **közreműködő** hozzáférés felhasználói tooan-külső hello **előfizetés** hatókörét, nem engedélyezi a hello hozzáférés toohello rendszergazda felhasználó mappájába, Ha hello **globális rendszergazda** lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="51ac4-160">However, granting **Owner** or **Contributor** access tooan external user at hello **Subscription** scope, does not allow hello access toohello admin user's directory, unless hello **Global Admin** allows it.</span></span> <span data-ttu-id="51ac4-161">Hello felhasználói tulajdonságokat, a hello **felhasználótípust** két közös paramétert tartalmaz **tag** és **vendég** azonosítható legyen.</span><span class="sxs-lookup"><span data-stu-id="51ac4-161">In hello user proprieties,  hello **User Type** which has two common parameters, **Member** and **Guest** can be identified.</span></span> <span data-ttu-id="51ac4-162">A tagja a felhasználó, amely hello könyvtárban regisztrálva van, míg a Vendég a meghívott felhasználó toohello directory külső forrásból.</span><span class="sxs-lookup"><span data-stu-id="51ac4-162">A member is a user which is registered in hello directory while a guest is a user invited toohello directory from an external source.</span></span> <span data-ttu-id="51ac4-163">További információkért lásd: [hogyan Azure Active Directory rendszergazdák hozzá B2B együttműködés felhasználók](/active-directory/active-directory-b2b-admin-add-users).</span><span class="sxs-lookup"><span data-stu-id="51ac4-163">For more information, see [How do Azure Active Directory admins add B2B collaboration users](/active-directory/active-directory-b2b-admin-add-users).</span></span>

> [!NOTE]
> <span data-ttu-id="51ac4-164">Győződjön meg arról, hogy után hello hitelesítő adatok megadása hello portálon, hello külső felhasználó hello megfelelő directory toosign a választ.</span><span class="sxs-lookup"><span data-stu-id="51ac4-164">Make sure that after entering hello credentials in hello portal, hello external user selects hello correct directory toosign-in to.</span></span> <span data-ttu-id="51ac4-165">hello ugyanahhoz a felhasználóhoz is hozzáférés toomultiple könyvtárai és is hello felhasználónév a hello Azure-portálon jobb oldalán felső hello kattintva válassza ki bármelyik egyik és válassza hello megfelelő directory hello legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="51ac4-165">hello same user can have access toomultiple directories and can select either one of  them by clicking hello username in hello top right-hand side in hello Azure portal and then choose hello appropriate directory from hello dropdown list.</span></span>

<span data-ttu-id="51ac4-166">A Vendég hello könyvtárban, miközben hello külső felhasználó hello Azure-előfizetéshez tartozó összes erőforrást is kezelheti, de hello könyvtár nem hozzáférhető.</span><span class="sxs-lookup"><span data-stu-id="51ac4-166">While being a guest in hello directory, hello external user can manage all resources for hello Azure subscription, but can't access hello directory.</span></span>





![hozzáférés tiltott tooazure active-directory Azure-portálon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/9.png)

<span data-ttu-id="51ac4-168">Az Azure Active Directory és az Azure-előfizetés nem rendelkezik egy szülő-gyermek kapcsolat, például a más Azure-erőforrások (például: virtuális gépek, virtuális hálózatok, a webalkalmazások, tárolási stb.) az Azure-előfizetéssel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="51ac4-168">Azure Active Directory and an Azure subscription don't have a child-parent relation like other Azure resources (for example: virtual machines, virtual networks, web apps, storage etc.) have with an Azure subscription.</span></span> <span data-ttu-id="51ac4-169">Ez utóbbi összes hello létrehozott, felügyelt, és amíg Azure-előfizetés használt toomanage hello hozzáférés tooan Azure-címtár az Azure-előfizetés számlázva.</span><span class="sxs-lookup"><span data-stu-id="51ac4-169">All hello latter is created, managed and billed under an Azure subscription while an Azure subscription is used toomanage hello access tooan Azure directory.</span></span> <span data-ttu-id="51ac4-170">További részletekért lásd: [hogyan Azure-előfizetés, kapcsolódó tooAzure AD](/active-directory/active-directory-how-subscriptions-associated-directory).</span><span class="sxs-lookup"><span data-stu-id="51ac4-170">For more details, see [How an Azure subscription is related tooAzure AD](/active-directory/active-directory-how-subscriptions-associated-directory).</span></span>

<span data-ttu-id="51ac4-171">Az összes hello beépített RBAC szerepkörből **tulajdonos** és **közreműködő** kínálnak a teljes felügyeleti hozzáférés tooall erőforrások hello környezetben, hello különbség folyamatban, amely egy közreműködői nem hozható létre, és új törlése Az RBAC-szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="51ac4-171">From all hello built-in RBAC roles, **Owner** and **Contributor** offer full management access tooall resources in hello environment, hello difference being that a Contributor can't create and delete new RBAC roles.</span></span> <span data-ttu-id="51ac4-172">hello más beépített szerepkörök, például **virtuális gép közreműködő** összes felügyeleti hozzáférés csak a megjelölt hello neve, függetlenül attól, hello toohello erőforrásokat kínálnak **erőforráscsoport** azok létrehozása .</span><span class="sxs-lookup"><span data-stu-id="51ac4-172">hello other built-in roles like **Virtual Machine Contributor** offer full management access only toohello resources indicated by hello name, regardless of hello **Resource Group** they are being created into.</span></span>

<span data-ttu-id="51ac4-173">Hozzárendelése hello beépített RBAC szerepe **virtuális gép közreműködő** egy előfizetés szintjén azt jelenti, hogy hello felhasználó lehet hozzárendelve hello szerepkör:</span><span class="sxs-lookup"><span data-stu-id="51ac4-173">Assigning hello built-in RBAC role of **Virtual Machine Contributor** at a subscription level, means that hello user assigned hello role:</span></span>

* <span data-ttu-id="51ac4-174">Megtekintheti az összes virtuális gép függetlenül azok telepítési dátumát és hello erőforráscsoportok részét képezik</span><span class="sxs-lookup"><span data-stu-id="51ac4-174">Can view all virtual machines regardless their deployment date and hello resource groups they are part of</span></span>
* <span data-ttu-id="51ac4-175">Összes felügyeleti hozzáférés toohello virtuális gépe van, a hello előfizetés</span><span class="sxs-lookup"><span data-stu-id="51ac4-175">Has full management access toohello virtual machines in hello subscription</span></span>
* <span data-ttu-id="51ac4-176">Nem lehet megtekinteni, bármilyen más típusú erőforrások hello az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="51ac4-176">Can't view any other resource types in hello subscription</span></span>
* <span data-ttu-id="51ac4-177">A módosításokat a számlázási szempontjából nem tud működni.</span><span class="sxs-lookup"><span data-stu-id="51ac4-177">Can't operate any changes from a billing perspective</span></span>

> [!NOTE]
> <span data-ttu-id="51ac4-178">Az RBAC alatt az Azure portál csak szolgáltatásai, azt nem adja meg hozzáférés toohello klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="51ac4-178">RBAC being an Azure portal only feature, it doesn't grant access toohello classic portal.</span></span>

## <a name="assign-a-built-in-rbac-role-tooan-external-user"></a><span data-ttu-id="51ac4-179">Beépített RBAC szerepkör tooan külső felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="51ac4-179">Assign a built-in RBAC role tooan external user</span></span>
<span data-ttu-id="51ac4-180">Ez a vizsgálat a különböző forgatókönyvek esetében a külső felhasználó hello "alflanigan@gmail.com" meg van adva egy **virtuális gép közreműködő**.</span><span class="sxs-lookup"><span data-stu-id="51ac4-180">For a different scenario in this test, hello external user "alflanigan@gmail.com" is added as a **Virtual Machine Contributor**.</span></span>




![virtuális gép közreműködő beépített szerepkör](./media/role-based-access-control-create-custom-roles-for-internal-external-users/11.png)

<span data-ttu-id="51ac4-182">hello ezek normál viselkedése a külső felhasználó beépített szerepkörrel rendelkező toosee, és csak a virtuális gépek és a szomszédos erőforrás-kezelő csak szükséges erőforrások másikban kezelése.</span><span class="sxs-lookup"><span data-stu-id="51ac4-182">hello normal behavior for this external user with this built-in role is toosee and manage only virtual machines and their adjacent Resource Manager only resources necessary while deploying.</span></span> <span data-ttu-id="51ac4-183">Úgy lett kialakítva, a korlátozott szerepkörök kínálnak hozzáférés csak tootheir levelező erőforrások hello Azure-portálon létrehozott, attól függetlenül történik néhány továbbra is telepíthető a hello, valamint a klasszikus portálon (például: virtuális gépek).</span><span class="sxs-lookup"><span data-stu-id="51ac4-183">By design, these limited roles offer access only tootheir correspondent resources created in hello Azure portal, regardless some can still be deployed in hello classic portal as well (for example: virtual machines).</span></span>





![virtuális gép közreműködői szerepkör áttekintése azure-portálon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/12.png)

## <a name="grant-access-at-a-subscription-level-for-a-user-in-hello-same-directory"></a><span data-ttu-id="51ac4-185">Azonos hello felhasználójának előfizetés szintű hozzáférés GRANT könyvtár</span><span class="sxs-lookup"><span data-stu-id="51ac4-185">Grant access at a subscription level for a user in hello same directory</span></span>
<span data-ttu-id="51ac4-186">hello folyamata azonos tooadding külső felhasználó, mind hello admin perspektíva támogatást nyújtó hello RBAC szerepkör, valamint a hozzáférés toohello szerepkör engedély hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="51ac4-186">hello process flow is identical tooadding an external user, both from hello admin perspective granting hello RBAC role as well as hello user being granted access toohello role.</span></span> <span data-ttu-id="51ac4-187">hello itt különbség, hogy hello meghívott felhasználó nem kap minden e-mailek meghívókat, a bejelentkezés után minden hello erőforrás hatókör hello előfizetésen belül hello irányítópult elérhető lesz.</span><span class="sxs-lookup"><span data-stu-id="51ac4-187">hello difference here is that hello invited user will not receive any email invitations as all hello resource scopes within hello subscription will be available in hello dashboard after signing in.</span></span>

## <a name="assign-rbac-roles-at-hello-resource-group-scope"></a><span data-ttu-id="51ac4-188">Hello erőforrás csoport hatókörű RBAC-szerepkörök hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="51ac4-188">Assign RBAC roles at hello resource group scope</span></span>
<span data-ttu-id="51ac4-189">Hozzárendelése egy Szerepalapú szerepkör a(z) egy **erőforráscsoport** hatókörben van egy azonos folyamata hello előfizetés szintjén, a felhasználók – külső vagy belső mindkét típusú hello szerepkör hozzárendelése (hello része könyvtárába).</span><span class="sxs-lookup"><span data-stu-id="51ac4-189">Assigning an RBAC role at a **Resource Group** scope has an identical process for assigning hello role at hello subscription level, for both types of users - either external or internal (part of hello same directory).</span></span> <span data-ttu-id="51ac4-190">hello hello RBAC szerepkörhöz hozzárendelt felhasználók csak a környezetükben toosee hello erőforráscsoportban van hozzájuk rendelve hozzáférés a hello **erőforráscsoportok** hello Azure-portálon ikonra.</span><span class="sxs-lookup"><span data-stu-id="51ac4-190">hello users which are assigned hello RBAC role is toosee in their environment only hello resource group they have been assigned access from hello **Resource Groups** icon in hello Azure portal.</span></span>

## <a name="assign-rbac-roles-at-hello-resource-scope"></a><span data-ttu-id="51ac4-191">Hello erőforrás hatókörből RBAC-szerepkörök hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="51ac4-191">Assign RBAC roles at hello resource scope</span></span>
<span data-ttu-id="51ac4-192">Az Azure erőforrás-hatókörben egy Szerepalapú szerepkör hozzárendelése hello előfizetés szintjén vagy hello erőforráscsoport szintjén hello szerepkör hozzárendelése egy azonos folyamata, a következő hello azonos mindkét forgatókönyvet munkafolyamata.</span><span class="sxs-lookup"><span data-stu-id="51ac4-192">Assigning an RBAC role at a resource scope in Azure has an identical process for assigning hello role at hello subscription level or at hello resource group level, following hello same workflow for both scenarios.</span></span> <span data-ttu-id="51ac4-193">Ebben az esetben hello hello RBAC szerepkörhöz hozzárendelt értesülhet, hogy azok hozzárendelt hozzáférés, vagy a hello csak hello elemek **összes erőforrás** lapon vagy közvetlenül az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="51ac4-193">Again, hello users which are assigned hello RBAC role can see only hello items that they have been assigned access to, either in hello **All Resources** tab or directly in their dashboard.</span></span>

<span data-ttu-id="51ac4-194">Az RBAC egyaránt erőforrás csoporthatókör vagy erőforrás hatókör fontos eleme hello felhasználók toomake toohello meg arról, hogy toosign-a megfelelő könyvtárban van.</span><span class="sxs-lookup"><span data-stu-id="51ac4-194">An important aspect for RBAC both at resource group scope or resource scope is for hello users toomake sure toosign-in toohello correct directory.</span></span>





![Directory bejelentkezés az Azure-portálon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/13.png)

## <a name="assign-rbac-roles-for-an-azure-active-directory-group"></a><span data-ttu-id="51ac4-196">Egy Azure Active Directory csoport RBAC-szerepkörök hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="51ac4-196">Assign RBAC roles for an Azure Active Directory group</span></span>
<span data-ttu-id="51ac4-197">Az RBAC használata a következő három különböző hatókörök hello Azure-ajánlat hello jogosultság kezelése, a központi telepítését és a különböző erőforrások felügyelete nélkül hello hozzárendelt felhasználóként az összes hello forgatókönyv kell a személyes előfizetés kezelésére.</span><span class="sxs-lookup"><span data-stu-id="51ac4-197">All hello scenarios using RBAC at hello three different scopes in Azure offer hello privilege of managing, deploying and administering various resources as an assigned user without hello need of managing a personal subscription.</span></span> <span data-ttu-id="51ac4-198">Függetlenül attól, hogy hello RBAC szerepkör tartozik előfizetés, erőforráscsoporthoz vagy erőforrás-hatókör, tovább hello hozzárendelt felhasználók által létrehozott összes hello erőforrások számlázása a hello egy Azure-előfizetéssel ahol hello felhasználók férhetnek hozzá.</span><span class="sxs-lookup"><span data-stu-id="51ac4-198">Regardless hello RBAC role is assigned for a subscription, resource group or resource scope, all hello resources created further on by hello assigned users are billed under hello one Azure subscription where hello users have access to.</span></span> <span data-ttu-id="51ac4-199">Ezzel a módszerrel hello számlázási a teljes Azure-előfizetéshez rendszergazdai jogosultságokkal rendelkező felhasználók rendelkezik teljes áttekintése hello fogyasztás, függetlenül attól, hogy a kezelő hello erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="51ac4-199">This way, hello users who have billing administrator permissions for that entire Azure subscription has a complete overview on hello consumption, regardless who is managing hello resources.</span></span>

<span data-ttu-id="51ac4-200">Nagyobb szervezetek számára az RBAC-szerepkörök alkalmazhatók a hello annak eldöntéséhez, hogy a hello perspektíva hello rendszergazda felhasználó Active Directory-csoportok ugyanúgy toogrant hello részletes hozzáférést igényel a csapatok vagy teljes, nem különállóan minden felhasználó részlegei, így annak eldöntéséhez, hogy ez rendkívül idő és a felügyeleti hatékony lehetőség.</span><span class="sxs-lookup"><span data-stu-id="51ac4-200">For larger organizations, RBAC roles can be applied in hello same way for Azure Active Directory groups considering hello perspective that hello admin user wants toogrant hello granular access for teams or entire departments, not individually for each user, thus considering it as an extremely time and management efficient option.</span></span> <span data-ttu-id="51ac4-201">tooillustrate ezt a példát, hello **közreműködő** szerepkör hozzá lett adva a hello csoportok tooone hello bérlői hello előfizetés szintjén.</span><span class="sxs-lookup"><span data-stu-id="51ac4-201">tooillustrate this example, hello **Contributor** role has been added tooone of hello groups in hello tenant at hello subscription level.</span></span>





![az AAD-csoportokat RBAC-szerepkör hozzáadása](./media/role-based-access-control-create-custom-roles-for-internal-external-users/14.png)

<span data-ttu-id="51ac4-203">Ezek a csoportok üzembe helyezve, és csak az Azure Active Directoryban felügyelni biztonsági csoportokat.</span><span class="sxs-lookup"><span data-stu-id="51ac4-203">These groups are security groups which are provisioned and managed only within Azure Active Directory.</span></span>

## <a name="create-a-custom-rbac-role-tooopen-support-requests-using-powershell"></a><span data-ttu-id="51ac4-204">Hozzon létre egyéni RBAC szerepkör tooopen támogatási kérelmek PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="51ac4-204">Create a custom RBAC role tooopen support requests using PowerShell</span></span>
<span data-ttu-id="51ac4-205">hello beépített RBAC szerepkörök az Azure-ban rendelkezésre álló gondoskodjon arról, hogy bizonyos jogosultsági szintek hello a hello környezetben elérhető erőforrások alapján.</span><span class="sxs-lookup"><span data-stu-id="51ac4-205">hello built-in RBAC roles which are available in Azure ensure certain permission levels based on hello available resources in hello environment.</span></span> <span data-ttu-id="51ac4-206">Azonban ezek a szerepkörök egyike hello rendszergazdai jogú felhasználó saját igényeinek megfelelően, ha nincs hello beállítás toolimit hozzáférés még több egyéni RBAC-szerepkörök létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="51ac4-206">However, if none of these roles suit hello admin user's needs, there is hello option toolimit access even more by creating custom RBAC roles.</span></span>

<span data-ttu-id="51ac4-207">Egyéni RBAC-szerepkörök létrehozásához szükséges tootake egy beépített szerepkör, szerkesztheti, majd importálja vissza hello környezetben.</span><span class="sxs-lookup"><span data-stu-id="51ac4-207">Creating custom RBAC roles requires tootake one built-in role, edit it and then import it back in hello environment.</span></span> <span data-ttu-id="51ac4-208">hello letöltése és a feltöltés hello szerepkör kezelése a PowerShell vagy a parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="51ac4-208">hello download and upload of hello role are managed using either PowerShell or CLI.</span></span>

<span data-ttu-id="51ac4-209">Fontos toounderstand hello Előfeltételek létre egy egyéni biztonsági szerepkört is, hogy a támogatási kérelmek megnyitása hello meghívott felhasználó hello rugalmasságát és részletes hozzáférést hello előfizetés szintjén.</span><span class="sxs-lookup"><span data-stu-id="51ac4-209">It is important toounderstand hello prerequisites of creating a custom role which can grant granular access at hello subscription level and also allow hello invited user hello flexibility of opening support requests.</span></span>

<span data-ttu-id="51ac4-210">A példa hello beépített szerepkör **olvasó** amely lehetővé teszi a felhasználók hozzáférési tooview összes hello erőforrás hatóköröket azonban nem tooedit őket, vagy hozzon létre újakat lett testre szabott tooallow hello felhasználói hello lehetőség a támogatási kérelmek megnyitása.</span><span class="sxs-lookup"><span data-stu-id="51ac4-210">For this example hello built-in role **Reader** which allows users access tooview all hello resource scopes but not tooedit them or create new ones has been customized tooallow hello user hello option of opening support requests.</span></span>

<span data-ttu-id="51ac4-211">első művelet hello exportáló hello **olvasó** emelt jogosultsági szintű rendszergazdaként futtatott szerepkör igények toobe PowerShell befejeződött.</span><span class="sxs-lookup"><span data-stu-id="51ac4-211">hello first action of exporting hello **Reader** role needs toobe completed in PowerShell ran with elevated permissions as administrator.</span></span>

```
Login-AzureRMAccount

Get-AzureRMRoleDefinition -Name "Reader"

Get-AzureRMRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\rbacrole2.json

```





![Az olvasó RBAC szerepkör PowerShell képernyőképe](./media/role-based-access-control-create-custom-roles-for-internal-external-users/15.png)

<span data-ttu-id="51ac4-213">Majd tooextract hello JSON-sablon hello szerepkör van szüksége.</span><span class="sxs-lookup"><span data-stu-id="51ac4-213">Then you need tooextract hello JSON template of hello role.</span></span>





![Egyéni olvasó RBAC szerepkör JSON-sablon](./media/role-based-access-control-create-custom-roles-for-internal-external-users/16.png)

<span data-ttu-id="51ac4-215">Egy tipikus RBAC szerepkör áll kívül három fő szakasz **műveletek**, **NotActions** és **AssignableScopes**.</span><span class="sxs-lookup"><span data-stu-id="51ac4-215">A typical RBAC role is composed out of three main sections, **Actions**, **NotActions** and **AssignableScopes**.</span></span>

<span data-ttu-id="51ac4-216">A hello **művelet** szakaszban felsorolt összes hello a szerepkörhöz tartozó engedélyezett műveletek.</span><span class="sxs-lookup"><span data-stu-id="51ac4-216">In hello **Action** section are listed all hello permitted operations for this role.</span></span> <span data-ttu-id="51ac4-217">Minden egyes művelethez hozzárendelt erőforrás-szolgáltató fontos toounderstand.</span><span class="sxs-lookup"><span data-stu-id="51ac4-217">It's important toounderstand that each action is assigned from a resource provider.</span></span> <span data-ttu-id="51ac4-218">Ebben az esetben a támogatási jegyeket hello létrehozására vonatkozó **Microsoft.Support** szerepelnie kell az erőforrás-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="51ac4-218">In this case, for creating support tickets hello **Microsoft.Support** resource provider must be listed.</span></span>

<span data-ttu-id="51ac4-219">toobe képes toosee összes hello érhető el, és az előfizetéshez regisztrált erőforrás-szolgáltató, a PowerShell segítségével.</span><span class="sxs-lookup"><span data-stu-id="51ac4-219">toobe able toosee all hello resource providers available and registered in your subscription, you can use PowerShell.</span></span>
```
Get-AzureRMResourceProvider

```
<span data-ttu-id="51ac4-220">Ezenkívül ellenőrizheti a hello összes hello elérhető PowerShell parancsmagok toomanage hello erőforrás-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="51ac4-220">Additionally, you can check for hello all hello available PowerShell cmdlets toomanage hello resource providers.</span></span>
    <span data-ttu-id="51ac4-221">![Az erőforrás-szolgáltató management PowerShell képernyőképe](./media/role-based-access-control-create-custom-roles-for-internal-external-users/17.png)</span><span class="sxs-lookup"><span data-stu-id="51ac4-221">![PowerShell screenshot for resource provider management](./media/role-based-access-control-create-custom-roles-for-internal-external-users/17.png)</span></span>

<span data-ttu-id="51ac4-222">egy adott RBAC-szerepkör erőforrás-szolgáltatók hello szakaszban felsorolt műveleteket hello összes toorestrict **NotActions**.</span><span class="sxs-lookup"><span data-stu-id="51ac4-222">toorestrict all hello actions for a particular RBAC role, resource providers are listed under hello section **NotActions**.</span></span>
<span data-ttu-id="51ac4-223">Utolsó akkor kötelező hello RBAC szerepkörhöz hello explicit előfizetési azonosítók felhasználási tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="51ac4-223">Last, it's mandatory that hello RBAC role contains hello explicit subscription IDs where it is used.</span></span> <span data-ttu-id="51ac4-224">hello előfizetési azonosítók a hello **AssignableScopes**, egyéb, nem engedélyezett tooimport hello szerepkör az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="51ac4-224">hello subscription IDs are listed under hello **AssignableScopes**, otherwise you will not be allowed tooimport hello role in your subscription.</span></span>

<span data-ttu-id="51ac4-225">Létrehozása és testreszabása hello RBAC szerepkör, után toobe importált hátsó hello környezet szükséges.</span><span class="sxs-lookup"><span data-stu-id="51ac4-225">After creating and customizing hello RBAC role, it needs toobe imported back hello environment.</span></span>

```
New-AzureRMRoleDefinition -InputFile "C:\rbacrole2.json"

```

<span data-ttu-id="51ac4-226">Ebben a példában a Szerepalapú szerepkör hello egyéni nevét "Olvasó támogatási jegyek hozzáférési szint" lehetővé tevő hello felhasználói tooview mindent hello előfizetés, és tooopen támogatási kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="51ac4-226">In this example, hello custom name for this RBAC role is "Reader support tickets access level" allowing hello user tooview everything in hello subscription and also tooopen support requests.</span></span>

> [!NOTE]
> <span data-ttu-id="51ac4-227">hello a támogatási kérelmek nyitó hello művelet engedélyezése csak két beépített RBAC szerepkörök vannak **tulajdonos** és **közreműködő**.</span><span class="sxs-lookup"><span data-stu-id="51ac4-227">hello only two built-in RBAC roles allowing hello action of opening of support requests are **Owner** and **Contributor**.</span></span> <span data-ttu-id="51ac4-228">Egy felhasználói toobe képes tooopen támogatási kérelmek ő szerepkört kell hozzárendelni egy Szerepalapú csak hatókörben hello előfizetés, mert minden támogatási kérelmek létrehozása az Azure-előfizetés alapján.</span><span class="sxs-lookup"><span data-stu-id="51ac4-228">For a user toobe able tooopen support requests, he must be assigned an RBAC role only at hello subscription scope, because all support requests are created based on an Azure subscription.</span></span>

<span data-ttu-id="51ac4-229">Az új egyéni szerepkör van hozzárendelve a hello tooan felhasználói ugyanabban a könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="51ac4-229">This new custom role has been assigned tooan user from hello same directory.</span></span>





![képernyőfelvétel a hello Azure-portálon az importált egyéni RBAC szerepkör](./media/role-based-access-control-create-custom-roles-for-internal-external-users/18.png)





![Képernyőkép a hozzárendelése egyéni importált RBAC szerepkör toouser hello az ugyanabban a könyvtárban](./media/role-based-access-control-create-custom-roles-for-internal-external-users/19.png)





![egyéni importált RBAC szerepkör engedélyeinek képernyőképe](./media/role-based-access-control-create-custom-roles-for-internal-external-users/20.png)

<span data-ttu-id="51ac4-233">hello példa lett további részletes tooemphasize hello korlátok egyéni RBAC-szerepkörhöz az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="51ac4-233">hello example has been further detailed tooemphasize hello limits of this custom RBAC role as follows:</span></span>
* <span data-ttu-id="51ac4-234">Új támogatási kérelmek hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="51ac4-234">Can create new support requests</span></span>
* <span data-ttu-id="51ac4-235">Nem hozható létre új erőforrás hatókörök (például: virtuális gép)</span><span class="sxs-lookup"><span data-stu-id="51ac4-235">Can't create new resource scopes (for example: virtual machine)</span></span>
* <span data-ttu-id="51ac4-236">Nem hozható létre új erőforrás-csoportok</span><span class="sxs-lookup"><span data-stu-id="51ac4-236">Can't create new resource groups</span></span>





![Képernyőkép a támogatási kérelmek létrehozása egyéni RBAC szerepkör](./media/role-based-access-control-create-custom-roles-for-internal-external-users/21.png)





![Képernyőkép a Szerepalapú egyéni szerepkör nem tudja toocreate virtuális gépek](./media/role-based-access-control-create-custom-roles-for-internal-external-users/22.png)





![Képernyőkép a Szerepalapú egyéni szerepkör nem tudja toocreate új RGs](./media/role-based-access-control-create-custom-roles-for-internal-external-users/23.png)

## <a name="create-a-custom-rbac-role-tooopen-support-requests-using-azure-cli"></a><span data-ttu-id="51ac4-240">Hozzon létre egy egyéni RBAC szerepkör tooopen támogatási kérelmek Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="51ac4-240">Create a custom RBAC role tooopen support requests using Azure CLI</span></span>
<span data-ttu-id="51ac4-241">Mac számítógépen, és anélkül, hogy hozzáférési tooPowerShell fut, az Azure parancssori felület hello módon toogo.</span><span class="sxs-lookup"><span data-stu-id="51ac4-241">Running on a Mac and without having access tooPowerShell, Azure CLI is hello way toogo.</span></span>

<span data-ttu-id="51ac4-242">hello lépéseket toocreate egy egyéni biztonsági szerepkört, hello egyedüli kivétel, hogy parancssori felület használatával hello szerepkör nem lehet letölteni egy JSON-sablon, de megtekinthetők az hello CLI hello.</span><span class="sxs-lookup"><span data-stu-id="51ac4-242">hello steps toocreate a custom role are hello same, with hello sole exception that using CLI hello role can't be downloaded in a JSON template, but it can be viewed in hello CLI.</span></span>

<span data-ttu-id="51ac4-243">Ehhez a példához I hello beépített szerepkör a kiválasztott **biztonsági mentés olvasó**.</span><span class="sxs-lookup"><span data-stu-id="51ac4-243">For this example I have chosen hello built-in role of **Backup Reader**.</span></span>

```

azure role show "backup reader" --json

```





![Parancssori felület Képernyőkép a biztonsági mentési olvasó szerepkört megjelenítése](./media/role-based-access-control-create-custom-roles-for-internal-external-users/24.png)

<span data-ttu-id="51ac4-245">A Visual Studio hello szerepkör módosítása hello tulajdonságokat a JSON-sablon másolása után hello **Microsoft.Support** erőforrás-szolgáltató hozzá lett adva hello **műveletek** , hogy a felhasználó megnyithatja részek támogatási kérelmek toobe hello mentési tárolók olvasójának folytatása közben.</span><span class="sxs-lookup"><span data-stu-id="51ac4-245">Editing hello role in Visual Studio after copying hello proprieties in a JSON template, hello **Microsoft.Support** resource provider has been added in hello **Actions** sections so that this user can open support requests while continuing toobe a reader for hello backup vaults.</span></span> <span data-ttu-id="51ac4-246">Újra szükség tooadd hello előfizetés-azonosító amelyeken ezt a szerepkört kell használni a hello **AssignableScopes** szakasz.</span><span class="sxs-lookup"><span data-stu-id="51ac4-246">Again it is necessary tooadd hello subscription ID where this role will be used in hello **AssignableScopes** section.</span></span>

```

azure role create --inputfile <path>

```





![Egyéni RBAC szerepkör importálása CLI képernyőképe](./media/role-based-access-control-create-custom-roles-for-internal-external-users/25.png)

<span data-ttu-id="51ac4-248">hello új szerepkör most hello Azure-portálon elérhető és hello hozzárendeléseket folyamat van hello ugyanaz, mint hello előző példák.</span><span class="sxs-lookup"><span data-stu-id="51ac4-248">hello new role is now available in hello Azure portal and hello assignation process is hello same as in hello previous examples.</span></span>





![Az Azure portál Képernyőkép a CLI 1.0 használatával létrehozott egyéni RBAC szerepkör](./media/role-based-access-control-create-custom-roles-for-internal-external-users/26.png)

<span data-ttu-id="51ac4-250">Hello frissítésétől legújabb Build 2017, hello Azure Cloud rendszerhéj általánosan elérhető.</span><span class="sxs-lookup"><span data-stu-id="51ac4-250">As of hello latest Build 2017, hello Azure Cloud Shell is generally available.</span></span> <span data-ttu-id="51ac4-251">Azure Cloud rendszerhéj egy a komplemens számnak tooIDE és hello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="51ac4-251">Azure Cloud Shell is a complement tooIDE and hello Azure Portal.</span></span> <span data-ttu-id="51ac4-252">Ezzel a szolgáltatással hitelesítése és Azure-ban üzemeltetett egy webböngésző-alapú rendszerhéj kap, és használhatja a számítógépen telepített parancssori felület helyett.</span><span class="sxs-lookup"><span data-stu-id="51ac4-252">With this service, you get a browser-based shell that is authenticated and hosted within Azure and you can use it instead of CLI installed on your machine.</span></span>





![Azure-felhőbe rendszerhéj](./media/role-based-access-control-create-custom-roles-for-internal-external-users/27.png)
