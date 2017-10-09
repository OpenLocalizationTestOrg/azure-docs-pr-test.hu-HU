---
title: "aaaAzure IoT Suite és az Azure Active Directory |} Microsoft Docs"
description: "Ismerteti, hogyan használja az Azure IoT Suite az Azure Active Directory-toomanage engedélyek."
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
ms.openlocfilehash: 4768630f2de4bb431731fbd4e8929232bc80b9f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="permissions-on-hello-azureiotsuitecom-site"></a><span data-ttu-id="17f18-103">Engedélyek hello azureiotsuite.com webhelyen</span><span class="sxs-lookup"><span data-stu-id="17f18-103">Permissions on hello azureiotsuite.com site</span></span>

## <a name="what-happens-when-you-sign-in"></a><span data-ttu-id="17f18-104">Mi történik, amikor bejelentkezik</span><span class="sxs-lookup"><span data-stu-id="17f18-104">What happens when you sign in</span></span>

<span data-ttu-id="17f18-105">első bejelentkezéskor, hello [azureiotsuite.com][lnk-azureiotsuite], hello hely határozza meg hello jogosultsági szintek rendelkezik alapján hello jelenleg kijelölt Azure Active Directory (AAD) bérlőt és Azure előfizetés.</span><span class="sxs-lookup"><span data-stu-id="17f18-105">hello first time you sign in at [azureiotsuite.com][lnk-azureiotsuite], hello site determines hello permission levels you have based on hello currently selected Azure Active Directory (AAD) tenant and Azure subscription.</span></span>

1. <span data-ttu-id="17f18-106">Először látott bérlők következő tooyour felhasználónév toopopulate hello listája, hello hely szerzi meg az Azure-ból mely AAD-bérlő tartozik.</span><span class="sxs-lookup"><span data-stu-id="17f18-106">First, toopopulate hello list of tenants seen next tooyour username, hello site finds out from Azure which AAD tenants you belong to.</span></span> <span data-ttu-id="17f18-107">Jelenleg hello hely csak szerezhet be egy bérlői felhasználói jogkivonatokhoz egyszerre.</span><span class="sxs-lookup"><span data-stu-id="17f18-107">Currently, hello site can only obtain user tokens for one tenant at a time.</span></span> <span data-ttu-id="17f18-108">Ezért amikor bérlők hello legördülő lista használatával hello jobb felső sarokban, hello hely rendszer toothat bérlői tooobtain hello jogkivonatokba, hogy a bérlő számára.</span><span class="sxs-lookup"><span data-stu-id="17f18-108">Therefore, when you switch tenants using hello dropdown in hello top right corner, hello site logs you in toothat tenant tooobtain hello tokens for that tenant.</span></span>

2. <span data-ttu-id="17f18-109">A következő hello hely szerzi meg az Azure melyik előfizetések hello társított kiválasztott bérlő.</span><span class="sxs-lookup"><span data-stu-id="17f18-109">Next, hello site finds out from Azure which subscriptions you have associated with hello selected tenant.</span></span> <span data-ttu-id="17f18-110">Amikor létrehoz egy új előkonfigurált megoldást hello az elérhető előfizetések láthatja.</span><span class="sxs-lookup"><span data-stu-id="17f18-110">You see hello available subscriptions when you create a new preconfigured solution.</span></span>

3. <span data-ttu-id="17f18-111">Végül hello hely hello előfizetésekhez hello erőforrásokat kéri le és erőforráscsoportok előkonfigurált megoldásokat címkével rendelkeznek, és feltölti a hello csempék hello kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="17f18-111">Finally, hello site retrieves all hello resources in hello subscriptions and resource groups tagged as preconfigured solutions and populates hello tiles on hello home page.</span></span>

<span data-ttu-id="17f18-112">a következő szakaszok hello hello szerepkörök, hozzáférési előre konfigurált toohello megoldásokban szabályozó ismertetik.</span><span class="sxs-lookup"><span data-stu-id="17f18-112">hello following sections describe hello roles that control access toohello preconfigured solutions.</span></span>

## <a name="aad-roles"></a><span data-ttu-id="17f18-113">Az AAD-szerepkörök</span><span class="sxs-lookup"><span data-stu-id="17f18-113">AAD roles</span></span>

<span data-ttu-id="17f18-114">hello AAD szerepkörök hello képes előre konfigurált rendelkezés megoldások szabályozza, és előre konfigurált megoldás a felhasználók kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="17f18-114">hello AAD roles control hello ability provision preconfigured solutions and manage users in a preconfigured solution.</span></span>

<span data-ttu-id="17f18-115">Tudnivalók a rendszergazdai szerepkörökről további információkat talál az aad-ben a [rendszergazdai szerepkörök hozzárendelése az Azure AD][lnk-aad-admin].</span><span class="sxs-lookup"><span data-stu-id="17f18-115">You can find more information about administrator roles in AAD in [Assigning administrator roles in Azure AD][lnk-aad-admin].</span></span> <span data-ttu-id="17f18-116">hello aktuális cikk foglalkozik a hello **globális rendszergazda** és hello **felhasználói** directory szerepkörök hello által használt, előre konfigurált megoldások.</span><span class="sxs-lookup"><span data-stu-id="17f18-116">hello current article focuses on hello **Global Administrator** and hello **User** directory roles as used by hello preconfigured solutions.</span></span>

### <a name="global-administrator"></a><span data-ttu-id="17f18-117">Globális rendszergazda</span><span class="sxs-lookup"><span data-stu-id="17f18-117">Global administrator</span></span>

<span data-ttu-id="17f18-118">Az AAD bérlőnként globális rendszergazdák közül sokan lehet:</span><span class="sxs-lookup"><span data-stu-id="17f18-118">There can be many global administrators per AAD tenant:</span></span>

* <span data-ttu-id="17f18-119">Amikor létrehoz egy AAD-bérlőt, alapértelmezett hello globális rendszergazdának, hogy a bérlő áll.</span><span class="sxs-lookup"><span data-stu-id="17f18-119">When you create an AAD tenant, you are by default hello global administrator of that tenant.</span></span>
* <span data-ttu-id="17f18-120">globális rendszergazda hello előkonfigurált megoldást hozhat létre, és hozzá van rendelve egy **Admin** szerepkör hello alkalmazás az AAD-bérlőt belül.</span><span class="sxs-lookup"><span data-stu-id="17f18-120">hello global administrator can provision a preconfigured solution and is assigned an **Admin** role for hello application inside their AAD tenant.</span></span>
* <span data-ttu-id="17f18-121">Ha egy másik felhasználója hello azonos AAD-bérlőt alkalmazást hoz létre, hello alapértelmezett nyújtott toohello globális rendszergazdai szerepköre **ReadOnly**.</span><span class="sxs-lookup"><span data-stu-id="17f18-121">If another user in hello same AAD tenant creates an application, hello default role granted toohello global administrator is **ReadOnly**.</span></span>
* <span data-ttu-id="17f18-122">A globális rendszergazda felhasználók tooroles hello használó alkalmazások esetében rendelhet [Azure-portálon][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="17f18-122">A global administrator can assign users tooroles for applications using hello [Azure portal][lnk-portal].</span></span>

### <a name="domain-user"></a><span data-ttu-id="17f18-123">Tartományi felhasználó</span><span class="sxs-lookup"><span data-stu-id="17f18-123">Domain user</span></span>

<span data-ttu-id="17f18-124">AAD-bérlőt sok tartományi felhasználók lehet:</span><span class="sxs-lookup"><span data-stu-id="17f18-124">There can be many domain users per AAD tenant:</span></span>

* <span data-ttu-id="17f18-125">A tartományi felhasználók építhető ki hello előkonfigurált megoldást [azureiotsuite.com] [ lnk-azureiotsuite] hely.</span><span class="sxs-lookup"><span data-stu-id="17f18-125">A domain user can provision a preconfigured solution through hello [azureiotsuite.com][lnk-azureiotsuite] site.</span></span> <span data-ttu-id="17f18-126">Alapértelmezés szerint hello tartomány kap hello **Admin** hello szerepköre kiépített alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="17f18-126">By default, hello domain user is granted hello **Admin** role in hello provisioned application.</span></span>
* <span data-ttu-id="17f18-127">Egy olyan tartományi felhasználó hozhat létre egy alkalmazást, az hello build.cmd parancsfájl a hello [azure iot-távoli-ellenőrző][lnk-rm-github-repo], [azure iot – prediktív-karbantartás] [ lnk-pm-github-repo], vagy [azure iot-csatlakoztatott-gyári] [ lnk-cf-github-repo] tárházba.</span><span class="sxs-lookup"><span data-stu-id="17f18-127">A domain user can create an application using hello build.cmd script in hello [azure-iot-remote-monitoring][lnk-rm-github-repo],  [azure-iot-predictive-maintenance][lnk-pm-github-repo], or [azure-iot-connected-factory][lnk-cf-github-repo] repository.</span></span> <span data-ttu-id="17f18-128">Azonban hello alapértelmezett szerepkör toohello tartományi felhasználó kap-e **ReadOnly**, mert egy olyan tartományi felhasználó nem rendelkezik engedéllyel tooassign szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="17f18-128">However, hello default role granted toohello domain user is **ReadOnly**, because a domain user does not have permission tooassign roles.</span></span>
* <span data-ttu-id="17f18-129">Az AAD-bérlőt hello egy másik felhasználó alkalmazást hoz létre, ha a hello tartományi felhasználó van-e hozzárendelve a hello **ReadOnly** szerepkör az alkalmazás alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="17f18-129">If another user in hello AAD tenant creates an application, hello domain user is assigned hello **ReadOnly** role by default for that application.</span></span>
* <span data-ttu-id="17f18-130">Egy olyan tartományi felhasználó nem rendelhető hozzá a szerepkört az alkalmazások számára. ezért egy olyan tartományi felhasználó nem vehető fel felhasználók vagy szerepkörök a felhasználók számára az alkalmazás akkor is, ha azok kiépítése azt.</span><span class="sxs-lookup"><span data-stu-id="17f18-130">A domain user cannot assign roles for applications; therefore a domain user cannot add users or roles for users for an application even if they provisioned it.</span></span>

### <a name="guest-user"></a><span data-ttu-id="17f18-131">Vendég felhasználó</span><span class="sxs-lookup"><span data-stu-id="17f18-131">Guest User</span></span>

<span data-ttu-id="17f18-132">Az AAD bérlőnként számos Vendég felhasználó lehet.</span><span class="sxs-lookup"><span data-stu-id="17f18-132">There can be many guest users per AAD tenant.</span></span> <span data-ttu-id="17f18-133">Vendégfelhasználók rendelkeznek korlátozott jogok hello AAD-bérlőben.</span><span class="sxs-lookup"><span data-stu-id="17f18-133">Guest users have a limited set of rights in hello AAD tenant.</span></span> <span data-ttu-id="17f18-134">Ennek eredményeképpen a vendégfelhasználók hello AAD-bérlőt az előkonfigurált megoldás nem használhatók.</span><span class="sxs-lookup"><span data-stu-id="17f18-134">As a result, guest users cannot provision a preconfigured solution in hello AAD tenant.</span></span>

<span data-ttu-id="17f18-135">Felhasználók és szerepkörök az aad-ben kapcsolatos további információkért tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="17f18-135">For more information about users and roles in AAD, see hello following resources:</span></span>

* <span data-ttu-id="17f18-136">[Felhasználók létrehozása az Azure ad-ben][lnk-create-edit-users]</span><span class="sxs-lookup"><span data-stu-id="17f18-136">[Create users in Azure AD][lnk-create-edit-users]</span></span>
* <span data-ttu-id="17f18-137">[Felhasználók tooapps hozzárendelése][lnk-assign-app-roles]</span><span class="sxs-lookup"><span data-stu-id="17f18-137">[Assign users tooapps][lnk-assign-app-roles]</span></span>

## <a name="azure-subscription-administrator-roles"></a><span data-ttu-id="17f18-138">Azure-előfizetéshez rendszergazdai szerepkörök</span><span class="sxs-lookup"><span data-stu-id="17f18-138">Azure subscription administrator roles</span></span>

<span data-ttu-id="17f18-139">hello Azure rendszergazdai szerepkörök hello képességét toomap az Azure-előfizetés tooan AD-bérlő szabályozzák.</span><span class="sxs-lookup"><span data-stu-id="17f18-139">hello Azure admin roles control hello ability toomap an Azure subscription tooan AD tenant.</span></span>

<span data-ttu-id="17f18-140">További információk hello Azure rendszergazdai szerepkörök hello cikkben [hogyan tooadd, vagy módosítsa az Azure Társadminisztrátoraként, a szolgáltatás-rendszergazda és a fiók rendszergazdájához][lnk-admin-roles].</span><span class="sxs-lookup"><span data-stu-id="17f18-140">Find out more about hello Azure admin roles in hello article [How tooadd or change Azure Co-Administrator, Service Administrator, and Account Administrator][lnk-admin-roles].</span></span>

## <a name="application-roles"></a><span data-ttu-id="17f18-141">Alkalmazás-szerepkörök</span><span class="sxs-lookup"><span data-stu-id="17f18-141">Application roles</span></span>

<span data-ttu-id="17f18-142">hello alkalmazási szerepköröknek hozzáférés toodevices az előkonfigurált megoldás szabályozzák.</span><span class="sxs-lookup"><span data-stu-id="17f18-142">hello application roles control access toodevices in your preconfigured solution.</span></span>

<span data-ttu-id="17f18-143">Két megadott szerepkörök és meghatározott egy telepített alkalmazás egy implicit szerepkör vannak:</span><span class="sxs-lookup"><span data-stu-id="17f18-143">There are two defined roles and one implicit role defined in a provisioned application:</span></span>

* <span data-ttu-id="17f18-144">**Felügyeleti:** rendelkezik teljes vezérlés tooadd, felügyeletéhez, távolítsa el az eszközök és beállítások módosítása.</span><span class="sxs-lookup"><span data-stu-id="17f18-144">**Admin:** Has full control tooadd, manage, remove devices, and modify settings.</span></span>
* <span data-ttu-id="17f18-145">**Csak olvasható:** eszközök, szabályok, műveletek, feladatok és telemetriai adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="17f18-145">**ReadOnly:** Can view devices, rules, actions, jobs, and telemetry.</span></span>

<span data-ttu-id="17f18-146">Hello jogosultságokkal hello tooeach szerepkör található [RolePermissions.cs] [ lnk-resource-cs] forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="17f18-146">You can find hello permissions assigned tooeach role in hello [RolePermissions.cs][lnk-resource-cs] source file.</span></span>

### <a name="changing-application-roles-for-a-user"></a><span data-ttu-id="17f18-147">A felhasználó alkalmazás-szerepkörök módosítása</span><span class="sxs-lookup"><span data-stu-id="17f18-147">Changing application roles for a user</span></span>

<span data-ttu-id="17f18-148">A következő eljárás toomake az Active Directoryban az előkonfigurált megoldás egy rendszergazda felhasználó hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="17f18-148">You can use hello following procedure toomake a user in your Active Directory an administrator of your preconfigured solution.</span></span>

<span data-ttu-id="17f18-149">Egy felhasználó egy aad-ben globális rendszergazda toochange szerepköreinek kell lennie:</span><span class="sxs-lookup"><span data-stu-id="17f18-149">You must be an AAD global administrator toochange roles for a user:</span></span>

1. <span data-ttu-id="17f18-150">Nyissa meg toohello [Azure-portálon][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="17f18-150">Go toohello [Azure portal][lnk-portal].</span></span>
2. <span data-ttu-id="17f18-151">Válassza ki **az Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="17f18-151">Select **Azure Active Directory**.</span></span>
3. <span data-ttu-id="17f18-152">Ellenőrizze, hogy használ hello directory úgy döntött, hogy a azureiotsuite.com létesített a megoldás.</span><span class="sxs-lookup"><span data-stu-id="17f18-152">Make sure you are using hello directory you chose on azureiotsuite.com when you provisioned your solution.</span></span> <span data-ttu-id="17f18-153">Ha az Ön előfizetéséhez rendelve több címtárral rendelkezik, közöttük, ha hello jobb felső részén hello portálon, a fiók nevére kattintva válthat.</span><span class="sxs-lookup"><span data-stu-id="17f18-153">If you have multiple directories associated with your subscription, you can switch between them if you click your account name at hello top-right of hello portal.</span></span>
4. <span data-ttu-id="17f18-154">Kattintson a **vállalati alkalmazások**, majd **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="17f18-154">Click **Enterprise applications**, then **All applications**.</span></span>
4. <span data-ttu-id="17f18-155">Megjelenítése **összes alkalmazás** rendelkező **bármely** állapotát.</span><span class="sxs-lookup"><span data-stu-id="17f18-155">Show **All applications** with **Any** status.</span></span> <span data-ttu-id="17f18-156">Majd keresse meg az előkonfigurált megoldás nevű kérelmet.</span><span class="sxs-lookup"><span data-stu-id="17f18-156">Then search for an application with name of your preconfigured solution.</span></span>
5. <span data-ttu-id="17f18-157">Kattintson a hello alkalmazás, amely megfelel az előkonfigurált megoldás neve hello nevét.</span><span class="sxs-lookup"><span data-stu-id="17f18-157">Click hello name of hello application that matches your preconfigured solution name.</span></span>
6. <span data-ttu-id="17f18-158">Kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="17f18-158">Click **Users and groups**.</span></span>
7. <span data-ttu-id="17f18-159">Válassza ki a kívánt tooswitch szerepkörök hello felhasználót.</span><span class="sxs-lookup"><span data-stu-id="17f18-159">Select hello user you want tooswitch roles.</span></span>
8. <span data-ttu-id="17f18-160">Kattintson a **hozzárendelése** és select hello szerepkör (például **Admin**) tooassign toohello felhasználói, milyen kattintson pipára hello.</span><span class="sxs-lookup"><span data-stu-id="17f18-160">Click **Assign** and select hello role (such as **Admin**) you'd like tooassign toohello user, click hello check mark.</span></span>

## <a name="faq"></a><span data-ttu-id="17f18-161">GYIK</span><span class="sxs-lookup"><span data-stu-id="17f18-161">FAQ</span></span>

### <a name="im-a-service-administrator-and-id-like-toochange-hello-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-complete-this-task"></a><span data-ttu-id="17f18-162">A szolgáltatás-rendszergazdáknak vagyok, és szeretnék toochange hello könyvtár leképezése az előfizetésem és egy adott AAD-bérlőt között.</span><span class="sxs-lookup"><span data-stu-id="17f18-162">I'm a service administrator and I'd like toochange hello directory mapping between my subscription and a specific AAD tenant.</span></span> <span data-ttu-id="17f18-163">Hogyan hajthatja végre ezt a feladatot?</span><span class="sxs-lookup"><span data-stu-id="17f18-163">How do I complete this task?</span></span>

1. <span data-ttu-id="17f18-164">Toohello lépjen [a klasszikus Azure portálon][lnk-classic-portal], kattintson a **beállítások** hello hello bal oldalán szolgáltatások közül.</span><span class="sxs-lookup"><span data-stu-id="17f18-164">Go toohello [Azure classic portal][lnk-classic-portal], click **Settings** in hello list of services on hello left-hand side.</span></span>
2. <span data-ttu-id="17f18-165">Válassza ki azt szeretné, hogy toochange hello könyvtár leképezése a hello előfizetést.</span><span class="sxs-lookup"><span data-stu-id="17f18-165">Select hello subscription you'd like toochange hello directory mapping to.</span></span>
3. <span data-ttu-id="17f18-166">Kattintson a **könyvtár**.</span><span class="sxs-lookup"><span data-stu-id="17f18-166">Click **Edit Directory**.</span></span>
4. <span data-ttu-id="17f18-167">Jelölje be hello **Directory** hello legördülő toouse szeretné.</span><span class="sxs-lookup"><span data-stu-id="17f18-167">Select hello **Directory** you would like toouse in hello dropdown.</span></span> <span data-ttu-id="17f18-168">Hello előre nyílra.</span><span class="sxs-lookup"><span data-stu-id="17f18-168">Click hello forward arrow.</span></span>
5. <span data-ttu-id="17f18-169">Erősítse meg a hello könyvtár leképezése és társrendszergazdák hatással.</span><span class="sxs-lookup"><span data-stu-id="17f18-169">Confirm hello directory mapping and affected co-administrators.</span></span> <span data-ttu-id="17f18-170">Ha áthelyez egy másik címtárból, a rendszer eltávolítja minden hello társrendszergazdák hello eredeti könyvtárból.</span><span class="sxs-lookup"><span data-stu-id="17f18-170">If you are moving from another directory, all hello co-administrators from hello original directory are removed.</span></span>

### <a name="im-a-domain-usermember-on-hello-aad-tenant-and-ive-created-a-preconfigured-solution-how-do-i-get-assigned-a-role-for-my-application"></a><span data-ttu-id="17f18-171">Egy tartományi felhasználói/tagot hello AAD-bérlőt vagyok, és létrehozott egy előre konfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="17f18-171">I'm a domain user/member on hello AAD tenant and I've created a preconfigured solution.</span></span> <span data-ttu-id="17f18-172">Hogyan tegye beolvasása kiosztott egy szerepkört az alkalmazáshoz?</span><span class="sxs-lookup"><span data-stu-id="17f18-172">How do I get assigned a role for my application?</span></span>

<span data-ttu-id="17f18-173">Kérje meg egy globális rendszergazda toomake, egy globális rendszergazdának hello aad-ben a bérlői, majd rendelje hozzá szerepkörök toousers magát.</span><span class="sxs-lookup"><span data-stu-id="17f18-173">Ask a global administrator toomake you a global administrator on hello AAD tenant and then assign roles toousers yourself.</span></span> <span data-ttu-id="17f18-174">Azt is megteheti, kérje meg egy globális rendszergazda tooassign meg közvetlenül egy szerepkört.</span><span class="sxs-lookup"><span data-stu-id="17f18-174">Alternatively, ask a global administrator tooassign you a role directly.</span></span> <span data-ttu-id="17f18-175">Ha szeretné toochange hello AAD-bérlőt az előkonfigurált megoldás már alkalmazva van, tekintse meg a következő kérdés hello.</span><span class="sxs-lookup"><span data-stu-id="17f18-175">If you'd like toochange hello AAD tenant your preconfigured solution has been deployed to, see hello next question.</span></span>

### <a name="how-do-i-switch-hello-aad-tenant-my-remote-monitoring-preconfigured-solution-and-application-are-assigned-to"></a><span data-ttu-id="17f18-176">A távoli felügyeleti előkonfigurált megoldás és az alkalmazás hozzárendelni hello AAD-bérlőt átváltása?</span><span class="sxs-lookup"><span data-stu-id="17f18-176">How do I switch hello AAD tenant my remote monitoring preconfigured solution and application are assigned to?</span></span>

<span data-ttu-id="17f18-177">A felhő üzembe helyezése a futtatása <https://github.com/Azure/azure-iot-remote-monitoring> és telepítse újra az újonnan létrehozott AAD-bérlőt.</span><span class="sxs-lookup"><span data-stu-id="17f18-177">You can run a cloud deployment from <https://github.com/Azure/azure-iot-remote-monitoring> and redeploy with a newly created AAD tenant.</span></span> <span data-ttu-id="17f18-178">Mivel, alapértelmezés szerint egy AAD-bérlőt létrehozásakor a globális rendszergazdai engedélyek tooadd felhasználói és szerepkörök toothose felhasználók hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="17f18-178">Because you are, by default, a global administrator when you create an AAD tenant, you have permissions tooadd users and assign roles toothose users.</span></span>

1. <span data-ttu-id="17f18-179">Hozzon létre egy AAD-címtárában hello [Azure-portálon][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="17f18-179">Create an AAD directory in hello [Azure portal][lnk-portal].</span></span>
2. <span data-ttu-id="17f18-180">Nyissa meg túl<https://github.com/Azure/azure-iot-remote-monitoring>.</span><span class="sxs-lookup"><span data-stu-id="17f18-180">Go too<https://github.com/Azure/azure-iot-remote-monitoring>.</span></span>
3. <span data-ttu-id="17f18-181">Futtatás `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (például `build.cmd cloud debug myRMSolution`)</span><span class="sxs-lookup"><span data-stu-id="17f18-181">Run `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (For example, `build.cmd cloud debug myRMSolution`)</span></span>
4. <span data-ttu-id="17f18-182">Amikor a rendszer kéri, állítsa be a hello **tenantid** toobe az újonnan létrehozott bérlő az előző bérlő helyett.</span><span class="sxs-lookup"><span data-stu-id="17f18-182">When prompted, set hello **tenantid** toobe your newly created tenant instead of your previous tenant.</span></span>

### <a name="i-want-toochange-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a><span data-ttu-id="17f18-183">A szolgáltatás rendszergazdai vagy társadminisztrátori amikor egy szervezeti fiókkal jelentkezik be toochange kívánt</span><span class="sxs-lookup"><span data-stu-id="17f18-183">I want toochange a Service Administrator or Co-Administrator when logged in with an organisational account</span></span>

<span data-ttu-id="17f18-184">Lásd: hello támogatási cikk [módosítása szolgáltatás-rendszergazda és Társadminisztrátoraként, amikor egy szervezeti fiókkal jelentkezik be][lnk-service-admins].</span><span class="sxs-lookup"><span data-stu-id="17f18-184">See hello support article [Changing Service Administrator and Co-Administrator when logged in with an organisational account][lnk-service-admins].</span></span>

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-hello-proper-permissions-toocreate-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a><span data-ttu-id="17f18-185">Miért jelenik meg ezt a hibát?</span><span class="sxs-lookup"><span data-stu-id="17f18-185">Why am I seeing this error?</span></span> <span data-ttu-id="17f18-186">"A fiók nem rendelkezik megfelelő engedélyekkel toocreate hello megoldást.</span><span class="sxs-lookup"><span data-stu-id="17f18-186">"Your account does not have hello proper permissions toocreate a solution.</span></span> <span data-ttu-id="17f18-187">Ellenőrizze a fiók rendszergazdájához, vagy próbálkozzon egy másik fiókkal."</span><span class="sxs-lookup"><span data-stu-id="17f18-187">Please check with your account administrator or try with a different account."</span></span>

<span data-ttu-id="17f18-188">Tekintse meg a következő diagram útmutatót hello:</span><span class="sxs-lookup"><span data-stu-id="17f18-188">Look at hello following diagram for guidance:</span></span>

![][img-flowchart]

> [!NOTE]
> <span data-ttu-id="17f18-189">Ha Ön most folytatja, toosee hello hiba ellenőrzése után egy globális rendszergazdája hello AAD-bérlőt és egy közös hello előfizetés rendszergazdája, a fiók rendszergazdájához, távolítsa el a hello felhasználói és újbóli hozzárendelése a szükséges engedélyekkel az itt megadott sorrendben kell.</span><span class="sxs-lookup"><span data-stu-id="17f18-189">If you continue toosee hello error after validating you are a global administrator on hello AAD tenant and a co-administrator on hello subscription, have your account administrator remove hello user and reassign necessary permissions in this order.</span></span> <span data-ttu-id="17f18-190">Először globális rendszergazdaként hello felhasználó hozzáadása, és adja hozzá a felhasználó hello Azure-előfizetés társadminisztrátoraként.</span><span class="sxs-lookup"><span data-stu-id="17f18-190">First, add hello user as a global administrator and then add user as a co-administrator on hello Azure subscription.</span></span> <span data-ttu-id="17f18-191">Ha a probléma továbbra is fennáll, forduljon a [Súgó és támogatás][lnk-help-support].</span><span class="sxs-lookup"><span data-stu-id="17f18-191">If issues persist, contact [Help & Support][lnk-help-support].</span></span>

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-toocreate-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a><span data-ttu-id="17f18-192">Miért jelenik meg a hiba, ha az Azure-előfizetésre van?</span><span class="sxs-lookup"><span data-stu-id="17f18-192">Why am I seeing this error when I have an Azure subscription?</span></span> <span data-ttu-id="17f18-193">"Az Azure-előfizetés nem szükséges toocreate előkonfigurált megoldásokat.</span><span class="sxs-lookup"><span data-stu-id="17f18-193">"An Azure subscription is required toocreate pre-configured solutions.</span></span> <span data-ttu-id="17f18-194">Létrehozhat egy ingyenes próbafiók néhány percig."</span><span class="sxs-lookup"><span data-stu-id="17f18-194">You can create a free trial account in just a couple of minutes."</span></span>

<span data-ttu-id="17f18-195">Ha bizonyos Azure-előfizetéssel rendelkezik, az előfizetéshez tartozó leképezési hello bérlői, és ellenőrizheti hello megfelelő bérlői hello legördülő van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="17f18-195">If you're certain you have an Azure subscription, validate hello tenant mapping for your subscription and ensure hello correct tenant is selected in hello dropdown.</span></span> <span data-ttu-id="17f18-196">Ha ellenőrizte, hogy hello szükséges bérlői helyességéről, kövesse az előző ábrán hello és az előfizetés és az AAD-bérlőt hello leképezés érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="17f18-196">If you’ve validated hello desired tenant is correct, follow hello preceding diagram and validate hello mapping of your subscription and this AAD tenant.</span></span>

## <a name="next-steps"></a><span data-ttu-id="17f18-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="17f18-197">Next steps</span></span>
<span data-ttu-id="17f18-198">IoT Suite megtanulni toocontinue tekintse meg, hogyan zajlik [előkonfigurált megoldás testreszabása][lnk-customize].</span><span class="sxs-lookup"><span data-stu-id="17f18-198">toocontinue learning about IoT Suite, see how you can [customize a preconfigured solution][lnk-customize].</span></span>

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
