---
title: "Az Azure IoT Suite és az Azure Active Directory |} Microsoft Docs"
description: "Ismerteti, hogyan Azure IoT Suite az Azure Active Directory használatával kezeli az engedélyeket."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 246228ba-954a-4d96-b6d6-e53e4590cb4f
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/09/2017
ms.author: dobett
ms.openlocfilehash: 518e6a481ab6385b03dd3ddc2e155fb724e677fe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="permissions-on-the-azureiotsuitecom-site"></a><span data-ttu-id="464e5-103">Engedélyek az azureiotsuite.com webhelyen</span><span class="sxs-lookup"><span data-stu-id="464e5-103">Permissions on the azureiotsuite.com site</span></span>

## <a name="what-happens-when-you-sign-in"></a><span data-ttu-id="464e5-104">Mi történik, amikor bejelentkezik</span><span class="sxs-lookup"><span data-stu-id="464e5-104">What happens when you sign in</span></span>

<span data-ttu-id="464e5-105">Az első bejelentkezéskor, [azureiotsuite.com][lnk-azureiotsuite], a hely határozza meg a jogosultsági szintek rendelkezik a kijelölt Azure Active Directory (AAD) bérlői és az Azure-előfizetés alapján.</span><span class="sxs-lookup"><span data-stu-id="464e5-105">The first time you sign in at [azureiotsuite.com][lnk-azureiotsuite], the site determines the permission levels you have based on the currently selected Azure Active Directory (AAD) tenant and Azure subscription.</span></span>

1. <span data-ttu-id="464e5-106">Először töltse fel adatokkal a bérlők mellett a felhasználónévnek látható listáját, a hely szerzi meg az Azure-ból mely AAD-bérlő tartozik.</span><span class="sxs-lookup"><span data-stu-id="464e5-106">First, to populate the list of tenants seen next to your username, the site finds out from Azure which AAD tenants you belong to.</span></span> <span data-ttu-id="464e5-107">Jelenleg a hely csak szerezhet be egy bérlői felhasználói jogkivonatokhoz egyszerre.</span><span class="sxs-lookup"><span data-stu-id="464e5-107">Currently, the site can only obtain user tokens for one tenant at a time.</span></span> <span data-ttu-id="464e5-108">Ezért amikor a bérlők a legördülő lista használatával jobb felső sarokban, a hely naplózza a arra a bérlőre, hogy a bérlő a tokenek beszerzése.</span><span class="sxs-lookup"><span data-stu-id="464e5-108">Therefore, when you switch tenants using the dropdown in the top right corner, the site logs you in to that tenant to obtain the tokens for that tenant.</span></span>

2. <span data-ttu-id="464e5-109">A hely a következő megkeresése, melyik előfizetések vannak társítva a kiválasztott bérlő az Azure-ból.</span><span class="sxs-lookup"><span data-stu-id="464e5-109">Next, the site finds out from Azure which subscriptions you have associated with the selected tenant.</span></span> <span data-ttu-id="464e5-110">Megjelenik az elérhető előfizetésekkel, amikor létrehoz egy új előkonfigurált megoldást.</span><span class="sxs-lookup"><span data-stu-id="464e5-110">You see the available subscriptions when you create a new preconfigured solution.</span></span>

3. <span data-ttu-id="464e5-111">Végezetül a hely veszi át összes lévő erőforrásokat a előfizetésekhez és erőforráscsoportokhoz előkonfigurált megoldásokat címkével rendelkeznek, és feltölti a csempék a kezdőlapon.</span><span class="sxs-lookup"><span data-stu-id="464e5-111">Finally, the site retrieves all the resources in the subscriptions and resource groups tagged as preconfigured solutions and populates the tiles on the home page.</span></span>

<span data-ttu-id="464e5-112">A következő szakaszok ismertetik a szerepköröket, amelyek hozzáférést az előre konfigurált megoldásokhoz.</span><span class="sxs-lookup"><span data-stu-id="464e5-112">The following sections describe the roles that control access to the preconfigured solutions.</span></span>

## <a name="aad-roles"></a><span data-ttu-id="464e5-113">Az AAD-szerepkörök</span><span class="sxs-lookup"><span data-stu-id="464e5-113">AAD roles</span></span>

<span data-ttu-id="464e5-114">Az AAD-szerepkörök szabályozhatja a képes előre konfigurált rendelkezés megoldások, és előre konfigurált megoldás a felhasználók kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="464e5-114">The AAD roles control the ability provision preconfigured solutions and manage users in a preconfigured solution.</span></span>

<span data-ttu-id="464e5-115">Tudnivalók a rendszergazdai szerepkörökről további információkat talál az aad-ben a [rendszergazdai szerepkörök hozzárendelése az Azure AD][lnk-aad-admin].</span><span class="sxs-lookup"><span data-stu-id="464e5-115">You can find more information about administrator roles in AAD in [Assigning administrator roles in Azure AD][lnk-aad-admin].</span></span> <span data-ttu-id="464e5-116">A jelen cikkben összpontosít a **globális rendszergazda** és a **felhasználói** directory szerepkörök az előkonfigurált megoldásokat által használt.</span><span class="sxs-lookup"><span data-stu-id="464e5-116">The current article focuses on the **Global Administrator** and the **User** directory roles as used by the preconfigured solutions.</span></span>

### <a name="global-administrator"></a><span data-ttu-id="464e5-117">Globális rendszergazda</span><span class="sxs-lookup"><span data-stu-id="464e5-117">Global administrator</span></span>

<span data-ttu-id="464e5-118">Az AAD bérlőnként globális rendszergazdák közül sokan lehet:</span><span class="sxs-lookup"><span data-stu-id="464e5-118">There can be many global administrators per AAD tenant:</span></span>

* <span data-ttu-id="464e5-119">Amikor létrehoz egy AAD-bérlőt, áll alapértelmezés szerint, hogy a bérlő globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="464e5-119">When you create an AAD tenant, you are by default the global administrator of that tenant.</span></span>
* <span data-ttu-id="464e5-120">A globális rendszergazda előkonfigurált megoldást hozhat létre, és hozzá van rendelve egy **Admin** szerepkör az alkalmazás az AAD-bérlőt belül.</span><span class="sxs-lookup"><span data-stu-id="464e5-120">The global administrator can provision a preconfigured solution and is assigned an **Admin** role for the application inside their AAD tenant.</span></span>
* <span data-ttu-id="464e5-121">Ha egy másik felhasználója ugyanazt az AAD-bérlőt alkalmazást hoz létre, az alapértelmezett számára a globális rendszergazdai szerepköre **ReadOnly**.</span><span class="sxs-lookup"><span data-stu-id="464e5-121">If another user in the same AAD tenant creates an application, the default role granted to the global administrator is **ReadOnly**.</span></span>
* <span data-ttu-id="464e5-122">A globális rendszergazda felhasználók hozzárendelése a szerepkörök használó alkalmazások esetében a [Azure-portálon][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="464e5-122">A global administrator can assign users to roles for applications using the [Azure portal][lnk-portal].</span></span>

### <a name="domain-user"></a><span data-ttu-id="464e5-123">Tartományi felhasználó</span><span class="sxs-lookup"><span data-stu-id="464e5-123">Domain user</span></span>

<span data-ttu-id="464e5-124">AAD-bérlőt sok tartományi felhasználók lehet:</span><span class="sxs-lookup"><span data-stu-id="464e5-124">There can be many domain users per AAD tenant:</span></span>

* <span data-ttu-id="464e5-125">A tartományi felhasználók építhető ki előkonfigurált megoldást a [azureiotsuite.com] [ lnk-azureiotsuite] hely.</span><span class="sxs-lookup"><span data-stu-id="464e5-125">A domain user can provision a preconfigured solution through the [azureiotsuite.com][lnk-azureiotsuite] site.</span></span> <span data-ttu-id="464e5-126">Alapértelmezés szerint a tartományi felhasználó számára biztosított a **Admin** szerepkör a kiépített alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="464e5-126">By default, the domain user is granted the **Admin** role in the provisioned application.</span></span>
* <span data-ttu-id="464e5-127">Egy olyan tartományi felhasználó hozhat létre egy alkalmazást a build.cmd parancsfájl a [azure iot-távoli-ellenőrző][lnk-rm-github-repo], [azure iot – prediktív-karbantartás] [ lnk-pm-github-repo], vagy [azure iot-csatlakoztatott-gyári] [ lnk-cf-github-repo] tárházba.</span><span class="sxs-lookup"><span data-stu-id="464e5-127">A domain user can create an application using the build.cmd script in the [azure-iot-remote-monitoring][lnk-rm-github-repo],  [azure-iot-predictive-maintenance][lnk-pm-github-repo], or [azure-iot-connected-factory][lnk-cf-github-repo] repository.</span></span> <span data-ttu-id="464e5-128">Azonban az alapértelmezett a tartományi felhasználó számára biztosított szerepköre **ReadOnly**, mert egy olyan tartományi felhasználó nem jogosult rendelhet szerepköröket.</span><span class="sxs-lookup"><span data-stu-id="464e5-128">However, the default role granted to the domain user is **ReadOnly**, because a domain user does not have permission to assign roles.</span></span>
* <span data-ttu-id="464e5-129">Az AAD-bérlőt egy másik felhasználója alkalmazást hoz létre, ha a tartományi felhasználó van-e hozzárendelve a **ReadOnly** szerepkör az alkalmazás alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="464e5-129">If another user in the AAD tenant creates an application, the domain user is assigned the **ReadOnly** role by default for that application.</span></span>
* <span data-ttu-id="464e5-130">Egy olyan tartományi felhasználó nem rendelhető hozzá a szerepkört az alkalmazások számára. ezért egy olyan tartományi felhasználó nem vehető fel felhasználók vagy szerepkörök a felhasználók számára az alkalmazás akkor is, ha azok kiépítése azt.</span><span class="sxs-lookup"><span data-stu-id="464e5-130">A domain user cannot assign roles for applications; therefore a domain user cannot add users or roles for users for an application even if they provisioned it.</span></span>

### <a name="guest-user"></a><span data-ttu-id="464e5-131">Vendég felhasználó</span><span class="sxs-lookup"><span data-stu-id="464e5-131">Guest User</span></span>

<span data-ttu-id="464e5-132">Az AAD bérlőnként számos Vendég felhasználó lehet.</span><span class="sxs-lookup"><span data-stu-id="464e5-132">There can be many guest users per AAD tenant.</span></span> <span data-ttu-id="464e5-133">Vendégfelhasználók rendelkeznek korlátozott jogok az AAD-bérlőben.</span><span class="sxs-lookup"><span data-stu-id="464e5-133">Guest users have a limited set of rights in the AAD tenant.</span></span> <span data-ttu-id="464e5-134">Ennek eredményeképpen a vendégfelhasználók az AAD-bérlőt az előkonfigurált megoldás nem használhatók.</span><span class="sxs-lookup"><span data-stu-id="464e5-134">As a result, guest users cannot provision a preconfigured solution in the AAD tenant.</span></span>

<span data-ttu-id="464e5-135">Felhasználók és szerepkörök az aad-ben kapcsolatos további információkért lásd a következőket:</span><span class="sxs-lookup"><span data-stu-id="464e5-135">For more information about users and roles in AAD, see the following resources:</span></span>

* <span data-ttu-id="464e5-136">[Felhasználók létrehozása az Azure ad-ben][lnk-create-edit-users]</span><span class="sxs-lookup"><span data-stu-id="464e5-136">[Create users in Azure AD][lnk-create-edit-users]</span></span>
* <span data-ttu-id="464e5-137">[Felhasználók hozzárendelése alkalmazásokhoz][lnk-assign-app-roles]</span><span class="sxs-lookup"><span data-stu-id="464e5-137">[Assign users to apps][lnk-assign-app-roles]</span></span>

## <a name="azure-subscription-administrator-roles"></a><span data-ttu-id="464e5-138">Azure-előfizetéshez rendszergazdai szerepkörök</span><span class="sxs-lookup"><span data-stu-id="464e5-138">Azure subscription administrator roles</span></span>

<span data-ttu-id="464e5-139">Az Azure rendszergazdai szerepkörök az Azure-előfizetés van leképezve az AD-bérlő általi szabályozása.</span><span class="sxs-lookup"><span data-stu-id="464e5-139">The Azure admin roles control the ability to map an Azure subscription to an AD tenant.</span></span>

<span data-ttu-id="464e5-140">További információk a cikkben az Azure rendszergazdai szerepkörök [hozzáadása vagy módosítása az Azure Társadminisztrátoraként, a szolgáltatás-rendszergazda és a fiók rendszergazdájához][lnk-admin-roles].</span><span class="sxs-lookup"><span data-stu-id="464e5-140">Find out more about the Azure admin roles in the article [How to add or change Azure Co-Administrator, Service Administrator, and Account Administrator][lnk-admin-roles].</span></span>

## <a name="application-roles"></a><span data-ttu-id="464e5-141">Alkalmazás-szerepkörök</span><span class="sxs-lookup"><span data-stu-id="464e5-141">Application roles</span></span>

<span data-ttu-id="464e5-142">A szerepkörökhöz az előkonfigurált megoldás eszközökre való hozzáférés szabályozása.</span><span class="sxs-lookup"><span data-stu-id="464e5-142">The application roles control access to devices in your preconfigured solution.</span></span>

<span data-ttu-id="464e5-143">Két megadott szerepkörök és meghatározott egy telepített alkalmazás egy implicit szerepkör vannak:</span><span class="sxs-lookup"><span data-stu-id="464e5-143">There are two defined roles and one implicit role defined in a provisioned application:</span></span>

* <span data-ttu-id="464e5-144">**Felügyeleti:** hozzáadása, kezelése, távolítsa el az eszközök és beállítások módosítása teljes körű vezérléssel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="464e5-144">**Admin:** Has full control to add, manage, remove devices, and modify settings.</span></span>
* <span data-ttu-id="464e5-145">**Csak olvasható:** eszközök, szabályok, műveletek, feladatok és telemetriai adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="464e5-145">**ReadOnly:** Can view devices, rules, actions, jobs, and telemetry.</span></span>

<span data-ttu-id="464e5-146">A jogosultságait az egyes szerepkör található a [RolePermissions.cs] [ lnk-resource-cs] forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="464e5-146">You can find the permissions assigned to each role in the [RolePermissions.cs][lnk-resource-cs] source file.</span></span>

### <a name="changing-application-roles-for-a-user"></a><span data-ttu-id="464e5-147">A felhasználó alkalmazás-szerepkörök módosítása</span><span class="sxs-lookup"><span data-stu-id="464e5-147">Changing application roles for a user</span></span>

<span data-ttu-id="464e5-148">Az alábbi eljárás segítségével végezze el a felhasználó az Active Directory rendszergazdája az előkonfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="464e5-148">You can use the following procedure to make a user in your Active Directory an administrator of your preconfigured solution.</span></span>

<span data-ttu-id="464e5-149">Egy felhasználó szerepköreinek módosítása egy aad-ben globális rendszergazdának kell lennie:</span><span class="sxs-lookup"><span data-stu-id="464e5-149">You must be an AAD global administrator to change roles for a user:</span></span>

1. <span data-ttu-id="464e5-150">Nyissa meg az [Azure Portalt][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="464e5-150">Go to the [Azure portal][lnk-portal].</span></span>
2. <span data-ttu-id="464e5-151">Válassza ki **az Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="464e5-151">Select **Azure Active Directory**.</span></span>
3. <span data-ttu-id="464e5-152">Ellenőrizze, hogy használ a könyvtárat úgy döntött, hogy a azureiotsuite.com létesített a megoldás.</span><span class="sxs-lookup"><span data-stu-id="464e5-152">Make sure you are using the directory you chose on azureiotsuite.com when you provisioned your solution.</span></span> <span data-ttu-id="464e5-153">Ha az Ön előfizetéséhez rendelve több címtárral rendelkezik, válthat őket, ha a fiók nevére, a portál felső – jobb kattint.</span><span class="sxs-lookup"><span data-stu-id="464e5-153">If you have multiple directories associated with your subscription, you can switch between them if you click your account name at the top-right of the portal.</span></span>
4. <span data-ttu-id="464e5-154">Kattintson a **vállalati alkalmazások**, majd **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="464e5-154">Click **Enterprise applications**, then **All applications**.</span></span>
4. <span data-ttu-id="464e5-155">Megjelenítése **összes alkalmazás** rendelkező **bármely** állapotát.</span><span class="sxs-lookup"><span data-stu-id="464e5-155">Show **All applications** with **Any** status.</span></span> <span data-ttu-id="464e5-156">Majd keresse meg az előkonfigurált megoldás nevű kérelmet.</span><span class="sxs-lookup"><span data-stu-id="464e5-156">Then search for an application with name of your preconfigured solution.</span></span>
5. <span data-ttu-id="464e5-157">Kattintson a nevére, az alkalmazás, amely megfelel az előkonfigurált megoldás neve.</span><span class="sxs-lookup"><span data-stu-id="464e5-157">Click the name of the application that matches your preconfigured solution name.</span></span>
6. <span data-ttu-id="464e5-158">Kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="464e5-158">Click **Users and groups**.</span></span>
7. <span data-ttu-id="464e5-159">Válassza ki a használni kívánt szerepkörök felhasználót.</span><span class="sxs-lookup"><span data-stu-id="464e5-159">Select the user you want to switch roles.</span></span>
8. <span data-ttu-id="464e5-160">Kattintson a **hozzárendelése** , válassza ki a szerepkört (például **Admin**) szeretné rendelni a felhasználóhoz, kattintson a pipa jelre.</span><span class="sxs-lookup"><span data-stu-id="464e5-160">Click **Assign** and select the role (such as **Admin**) you'd like to assign to the user, click the check mark.</span></span>

## <a name="faq"></a><span data-ttu-id="464e5-161">GYIK</span><span class="sxs-lookup"><span data-stu-id="464e5-161">FAQ</span></span>

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-complete-this-task"></a><span data-ttu-id="464e5-162">A szolgáltatás-rendszergazdáknak vagyok, és szeretnék módosítani a könyvtár leképezése az előfizetésem és egy adott AAD-bérlőt között.</span><span class="sxs-lookup"><span data-stu-id="464e5-162">I'm a service administrator and I'd like to change the directory mapping between my subscription and a specific AAD tenant.</span></span> <span data-ttu-id="464e5-163">Hogyan hajthatja végre ezt a feladatot?</span><span class="sxs-lookup"><span data-stu-id="464e5-163">How do I complete this task?</span></span>

1. <span data-ttu-id="464e5-164">Lépjen a [a klasszikus Azure portálon][lnk-classic-portal], kattintson a **beállítások** a bal oldalon található szolgáltatások közül.</span><span class="sxs-lookup"><span data-stu-id="464e5-164">Go to the [Azure classic portal][lnk-classic-portal], click **Settings** in the list of services on the left-hand side.</span></span>
2. <span data-ttu-id="464e5-165">Válassza ki az előfizetést, módosítsa a könyvtárat a leképezés csak akkor szeretne.</span><span class="sxs-lookup"><span data-stu-id="464e5-165">Select the subscription you'd like to change the directory mapping to.</span></span>
3. <span data-ttu-id="464e5-166">Kattintson a **könyvtár**.</span><span class="sxs-lookup"><span data-stu-id="464e5-166">Click **Edit Directory**.</span></span>
4. <span data-ttu-id="464e5-167">Válassza ki a **Directory** szeretné használni a legördülő listában.</span><span class="sxs-lookup"><span data-stu-id="464e5-167">Select the **Directory** you would like to use in the dropdown.</span></span> <span data-ttu-id="464e5-168">A továbbítási nyílra.</span><span class="sxs-lookup"><span data-stu-id="464e5-168">Click the forward arrow.</span></span>
5. <span data-ttu-id="464e5-169">Erősítse meg a könyvtár leképezése és társrendszergazdák hatással.</span><span class="sxs-lookup"><span data-stu-id="464e5-169">Confirm the directory mapping and affected co-administrators.</span></span> <span data-ttu-id="464e5-170">Ha áthelyez egy másik címtárból, a rendszer eltávolítja minden társrendszergazdák az eredeti könyvtárból.</span><span class="sxs-lookup"><span data-stu-id="464e5-170">If you are moving from another directory, all the co-administrators from the original directory are removed.</span></span>

### <a name="im-a-domain-usermember-on-the-aad-tenant-and-ive-created-a-preconfigured-solution-how-do-i-get-assigned-a-role-for-my-application"></a><span data-ttu-id="464e5-171">Egy tartományi felhasználói/tagja az AAD-bérlőt vagyok, és létrehozott egy előre konfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="464e5-171">I'm a domain user/member on the AAD tenant and I've created a preconfigured solution.</span></span> <span data-ttu-id="464e5-172">Hogyan tegye beolvasása kiosztott egy szerepkört az alkalmazáshoz?</span><span class="sxs-lookup"><span data-stu-id="464e5-172">How do I get assigned a role for my application?</span></span>

<span data-ttu-id="464e5-173">Kérje meg a globális rendszergazdák is képesek adja meg az AAD-bérlőt a globális rendszergazdaként, majd rendelje hozzá szerepkörök a felhasználók számára saját maga.</span><span class="sxs-lookup"><span data-stu-id="464e5-173">Ask a global administrator to make you a global administrator on the AAD tenant and then assign roles to users yourself.</span></span> <span data-ttu-id="464e5-174">Azt is megteheti kérje meg egy globális rendszergazda, hogy rendeljen Önhöz közvetlenül egy szerepkört.</span><span class="sxs-lookup"><span data-stu-id="464e5-174">Alternatively, ask a global administrator to assign you a role directly.</span></span> <span data-ttu-id="464e5-175">Ha szeretné módosítani az AAD-bérlőt az előkonfigurált megoldás már alkalmazva van, tekintse meg a következő kérdés.</span><span class="sxs-lookup"><span data-stu-id="464e5-175">If you'd like to change the AAD tenant your preconfigured solution has been deployed to, see the next question.</span></span>

### <a name="how-do-i-switch-the-aad-tenant-my-remote-monitoring-preconfigured-solution-and-application-are-assigned-to"></a><span data-ttu-id="464e5-176">Az AAD-bérlőt, a távoli felügyeleti előkonfigurált megoldás és az alkalmazás hozzárendelni átváltása?</span><span class="sxs-lookup"><span data-stu-id="464e5-176">How do I switch the AAD tenant my remote monitoring preconfigured solution and application are assigned to?</span></span>

<span data-ttu-id="464e5-177">A felhő üzembe helyezése a futtatása <https://github.com/Azure/azure-iot-remote-monitoring> és telepítse újra az újonnan létrehozott AAD-bérlőt.</span><span class="sxs-lookup"><span data-stu-id="464e5-177">You can run a cloud deployment from <https://github.com/Azure/azure-iot-remote-monitoring> and redeploy with a newly created AAD tenant.</span></span> <span data-ttu-id="464e5-178">Mert, alapértelmezés szerint egy globális rendszergazda, amikor létrehoz egy AAD-bérlőt Ön jogosult felhasználók hozzáadásához és szerepkörök hozzárendelése azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="464e5-178">Because you are, by default, a global administrator when you create an AAD tenant, you have permissions to add users and assign roles to those users.</span></span>

1. <span data-ttu-id="464e5-179">Hozzon létre egy AAD-címtárában lévő a [Azure-portálon][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="464e5-179">Create an AAD directory in the [Azure portal][lnk-portal].</span></span>
2. <span data-ttu-id="464e5-180">Ugrás a <https://github.com/Azure/azure-iot-remote-monitoring>.</span><span class="sxs-lookup"><span data-stu-id="464e5-180">Go to <https://github.com/Azure/azure-iot-remote-monitoring>.</span></span>
3. <span data-ttu-id="464e5-181">Futtatás `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (például `build.cmd cloud debug myRMSolution`)</span><span class="sxs-lookup"><span data-stu-id="464e5-181">Run `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (For example, `build.cmd cloud debug myRMSolution`)</span></span>
4. <span data-ttu-id="464e5-182">Amikor a rendszer kéri, állítsa be a **tenantid** kell lennie az újonnan létrehozott bérlő az előző bérlő helyett.</span><span class="sxs-lookup"><span data-stu-id="464e5-182">When prompted, set the **tenantid** to be your newly created tenant instead of your previous tenant.</span></span>

### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a><span data-ttu-id="464e5-183">A szolgáltatás rendszergazdai vagy társadminisztrátori Ha egy szervezeti fiókkal jelentkezik módosítása</span><span class="sxs-lookup"><span data-stu-id="464e5-183">I want to change a Service Administrator or Co-Administrator when logged in with an organisational account</span></span>

<span data-ttu-id="464e5-184">Tekintse meg a támogatási cikk [módosítása szolgáltatás-rendszergazda és Társadminisztrátoraként, amikor egy szervezeti fiókkal jelentkezik be][lnk-service-admins].</span><span class="sxs-lookup"><span data-stu-id="464e5-184">See the support article [Changing Service Administrator and Co-Administrator when logged in with an organisational account][lnk-service-admins].</span></span>

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a><span data-ttu-id="464e5-185">Miért jelenik meg ezt a hibát?</span><span class="sxs-lookup"><span data-stu-id="464e5-185">Why am I seeing this error?</span></span> <span data-ttu-id="464e5-186">"A fiók nem rendelkezik a megoldás létrehozása a megfelelő engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="464e5-186">"Your account does not have the proper permissions to create a solution.</span></span> <span data-ttu-id="464e5-187">Ellenőrizze a fiók rendszergazdájához, vagy próbálkozzon egy másik fiókkal."</span><span class="sxs-lookup"><span data-stu-id="464e5-187">Please check with your account administrator or try with a different account."</span></span>

<span data-ttu-id="464e5-188">Tekintse meg az alábbi ábrában útmutatást:</span><span class="sxs-lookup"><span data-stu-id="464e5-188">Look at the following diagram for guidance:</span></span>

![][img-flowchart]

> [!NOTE]
> <span data-ttu-id="464e5-189">Ha továbbra is megjelenik a hiba akkor ellenőrzése után egy globális rendszergazdája az AAD-bérlőt és egy az előfizetés társadminisztrátoraként, a fiók rendszergazdájához, távolítsa el a felhasználót, és újra hozzárendelnie az itt megadott sorrendben szükséges engedélyekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="464e5-189">If you continue to see the error after validating you are a global administrator on the AAD tenant and a co-administrator on the subscription, have your account administrator remove the user and reassign necessary permissions in this order.</span></span> <span data-ttu-id="464e5-190">Először adja hozzá a felhasználót egy globális rendszergazdaként, és adja hozzá a felhasználó az Azure-előfizetés társadminisztrátoraként.</span><span class="sxs-lookup"><span data-stu-id="464e5-190">First, add the user as a global administrator and then add user as a co-administrator on the Azure subscription.</span></span> <span data-ttu-id="464e5-191">Ha a probléma továbbra is fennáll, forduljon a [Súgó és támogatás][lnk-help-support].</span><span class="sxs-lookup"><span data-stu-id="464e5-191">If issues persist, contact [Help & Support][lnk-help-support].</span></span>

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-to-create-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a><span data-ttu-id="464e5-192">Miért jelenik meg a hiba, ha az Azure-előfizetésre van?</span><span class="sxs-lookup"><span data-stu-id="464e5-192">Why am I seeing this error when I have an Azure subscription?</span></span> <span data-ttu-id="464e5-193">"Előkonfigurált megoldások létrehozásához Azure-előfizetés szükséges.</span><span class="sxs-lookup"><span data-stu-id="464e5-193">"An Azure subscription is required to create pre-configured solutions.</span></span> <span data-ttu-id="464e5-194">Létrehozhat egy ingyenes próbafiók néhány percig."</span><span class="sxs-lookup"><span data-stu-id="464e5-194">You can create a free trial account in just a couple of minutes."</span></span>

<span data-ttu-id="464e5-195">Ha bizonyos Azure-előfizetéssel rendelkezik, a bérlő hozzárendelése az előfizetéshez, és ellenőrizheti a megfelelő bérlő a legördülő listában kiválasztott.</span><span class="sxs-lookup"><span data-stu-id="464e5-195">If you're certain you have an Azure subscription, validate the tenant mapping for your subscription and ensure the correct tenant is selected in the dropdown.</span></span> <span data-ttu-id="464e5-196">Ha ellenőrizte, hogy a kívánt bérlő megfelelő, kövesse a fenti ábrán és az előfizetés és az AAD-bérlőt a leképezés érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="464e5-196">If you’ve validated the desired tenant is correct, follow the preceding diagram and validate the mapping of your subscription and this AAD tenant.</span></span>

## <a name="next-steps"></a><span data-ttu-id="464e5-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="464e5-197">Next steps</span></span>
<span data-ttu-id="464e5-198">És folytathatja az IoT Suite, tekintse meg, hogyan zajlik [előkonfigurált megoldás testreszabása][lnk-customize].</span><span class="sxs-lookup"><span data-stu-id="464e5-198">To continue learning about IoT Suite, see how you can [customize a preconfigured solution][lnk-customize].</span></span>

[img-flowchart]: media/iot-suite-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-cf-github-repo]: https://github.com/Azure/azure-iot-connected-factory
[lnk-aad-admin]: ../active-directory/active-directory-assign-admin-roles.md
[lnk-classic-portal]: https://manage.windowsazure.com/
[lnk-portal]: https://portal.azure.com/
[lnk-create-edit-users]: ../active-directory/active-directory-create-users.md
[lnk-assign-app-roles]: ../active-directory/active-directory-coreapps-assign-user-azure-portal.md
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: ../billing/billing-add-change-azure-subscription-administrator.md
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
