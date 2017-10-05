---
title: "Hozzon létre Azure-alkalmazás identitását portálon |} Microsoft Docs"
description: "Hozzon létre egy új Azure Active Directory-alkalmazás és szolgáltatás egyszerű erőforrásokhoz való hozzáférés kezelésére használható a szerepköralapú hozzáférés-vezérlés az Azure Resource Manager ismerteti."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 7068617b-ac5e-47b3-a1de-a18c918297b6
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: 5d24fb99e1095d53e5ea547e53b80178d9cb77c0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="use-portal-to-create-an-azure-active-directory-application-and-service-principal-that-can-access-resources"></a><span data-ttu-id="85408-103">Hozzon létre egy Azure Active Directory-alkalmazást és egy egyszerű szolgáltatást, amely erőforrások eléréséhez a portál használatával</span><span class="sxs-lookup"><span data-stu-id="85408-103">Use portal to create an Azure Active Directory application and service principal that can access resources</span></span>

<span data-ttu-id="85408-104">Ha egy alkalmazás eléréséhez, vagy módosítsa az erőforrások igénylő, állítson be egy Azure Active Directory (AD) alkalmazást, és a szükséges engedélyek hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="85408-104">When you have an application that needs to access or modify resources, you must set up an Azure Active Directory (AD) application and assign the required permissions to it.</span></span> <span data-ttu-id="85408-105">Ez a megközelítés célszerű a saját credentials az alkalmazást futtató, mert:</span><span class="sxs-lookup"><span data-stu-id="85408-105">This approach is preferable to running the app under your own credentials because:</span></span>

* <span data-ttu-id="85408-106">Engedélyeket rendelhet a saját engedélyek eltérő identitását.</span><span class="sxs-lookup"><span data-stu-id="85408-106">You can assign permissions to the app identity that are different than your own permissions.</span></span> <span data-ttu-id="85408-107">Ezek az engedélyek általában korlátozódik, hogy mit az alkalmazás kell tennie.</span><span class="sxs-lookup"><span data-stu-id="85408-107">Typically, these permissions are restricted to exactly what the app needs to do.</span></span>
* <span data-ttu-id="85408-108">Nem kell módosítani az alkalmazás hitelesítő adatokat, ha az Ön feladatkörei módosítása.</span><span class="sxs-lookup"><span data-stu-id="85408-108">You do not have to change the app's credentials if your responsibilities change.</span></span> 
* <span data-ttu-id="85408-109">Tanúsítvány segítségével automatizálhatja a hitelesítést egy felügyelet nélküli parancsfájl végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="85408-109">You can use a certificate to automate authentication when executing an unattended script.</span></span>

<span data-ttu-id="85408-110">Ez a témakör bemutatja, hogyan hajtsa végre ezeket a lépéseket a portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="85408-110">This topic shows you how to perform those steps through the portal.</span></span> <span data-ttu-id="85408-111">A single-bérlői alkalmazások, ahol az alkalmazás futtatásához csak egy szervezeten belül olyan összpontosít.</span><span class="sxs-lookup"><span data-stu-id="85408-111">It focuses on a single-tenant application where the application is intended to run within only one organization.</span></span> <span data-ttu-id="85408-112">Általában egy bérlői alkalmazásokat használ futó üzleti alkalmazásokhoz a szervezeten belül.</span><span class="sxs-lookup"><span data-stu-id="85408-112">You typically use single-tenant applications for line-of-business applications that run within your organization.</span></span>
 
## <a name="required-permissions"></a><span data-ttu-id="85408-113">Szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="85408-113">Required permissions</span></span>
<span data-ttu-id="85408-114">Ez a témakör befejezéséhez megfelelő engedélyekkel rendelkezik alkalmazás regisztrálása az Azure AD-bérlőn, és az alkalmazást egy szerepkörhöz rendelhető az Azure-előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="85408-114">To complete this topic, you must have sufficient permissions to register an application with your Azure AD tenant, and assign the application to a role in your Azure subscription.</span></span> <span data-ttu-id="85408-115">Ellenőrizze, hogy a fenti lépések végrehajtásához a megfelelő engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="85408-115">Let's make sure you have the right permissions to perform those steps.</span></span>

### <a name="check-azure-active-directory-permissions"></a><span data-ttu-id="85408-116">Azure Active Directory-engedélyek ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="85408-116">Check Azure Active Directory permissions</span></span>
1. <span data-ttu-id="85408-117">Az Azure-fiók használatával jelentkezzen be a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="85408-117">Log in to your Azure Account through the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="85408-118">Válassza ki **az Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="85408-118">Select **Azure Active Directory**.</span></span>

     ![Válassza ki az azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)
3. <span data-ttu-id="85408-120">Válassza ki az Azure Active Directoryban, **felhasználói beállítások**.</span><span class="sxs-lookup"><span data-stu-id="85408-120">In Azure Active Directory, select **User settings**.</span></span>

     ![Válassza ki a felhasználói beállítások](./media/resource-group-create-service-principal-portal/select-user-settings.png)
4. <span data-ttu-id="85408-122">Ellenőrizze a **App regisztrációk** beállítást.</span><span class="sxs-lookup"><span data-stu-id="85408-122">Check the **App registrations** setting.</span></span> <span data-ttu-id="85408-123">Ha beállítása **Igen**, nem rendszergazdai felhasználók regisztrálhatják AD alkalmazásaiban.</span><span class="sxs-lookup"><span data-stu-id="85408-123">If set to **Yes**, non-admin users can register AD apps.</span></span> <span data-ttu-id="85408-124">Ez a beállítás azt jelenti, hogy egyetlen felhasználóhoz sem az Azure AD-bérlő regisztrálhatja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="85408-124">This setting means any user in the Azure AD tenant can register an app.</span></span> <span data-ttu-id="85408-125">Lépne [Azure ellenőrizze előfizetése engedélyei között](#check-azure-subscription-permissions).</span><span class="sxs-lookup"><span data-stu-id="85408-125">You can proceed to [Check Azure subscription permissions](#check-azure-subscription-permissions).</span></span>

     ![alkalmazás-regisztrációk megtekintése](./media/resource-group-create-service-principal-portal/view-app-registrations.png)
5. <span data-ttu-id="85408-127">Ha az alkalmazás regisztrációk beállítás értéke **nem**, csak a rendszergazda felhasználók regisztrálhatják az alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="85408-127">If the app registrations setting is set to **No**, only admin users can register apps.</span></span> <span data-ttu-id="85408-128">Ellenőrizze, hogy a fiók egy rendszergazda az Azure AD-bérlő kell.</span><span class="sxs-lookup"><span data-stu-id="85408-128">You need to check whether your account is an admin for the Azure AD tenant.</span></span> <span data-ttu-id="85408-129">Válassza ki **áttekintése** és **keresse meg azt a felhasználót** a gyorsan elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="85408-129">Select **Overview** and **Find a user** from Quick tasks.</span></span>

     ![felhasználó keresése](./media/resource-group-create-service-principal-portal/find-user.png)
6. <span data-ttu-id="85408-131">Keresse meg a fiókját, és válassza ki, ha azt.</span><span class="sxs-lookup"><span data-stu-id="85408-131">Search for your account, and select it when you find it.</span></span>

     ![felhasználó keresése](./media/resource-group-create-service-principal-portal/show-user.png)
7. <span data-ttu-id="85408-133">Válassza ki a fiókot, **Directory szerepkör**.</span><span class="sxs-lookup"><span data-stu-id="85408-133">For your account, select **Directory role**.</span></span> 

     ![Directory szerepkör](./media/resource-group-create-service-principal-portal/select-directory-role.png)
8. <span data-ttu-id="85408-135">A hozzárendelt directory szerepkör megtekintéséhez az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="85408-135">View your assigned directory role in Azure AD.</span></span> <span data-ttu-id="85408-136">Ha a fiók hozzá van rendelve a felhasználói szerepkör, de a regisztrációs alkalmazásbeállításhoz (az előző lépések) korlátozott rendszergazdai jogosultságú felhasználókhoz, kérje meg a rendszergazdát, vagy hogy rendeljen Önhöz egy rendszergazdai szerepkört, vagy lehetővé teszi a felhasználók alkalmazásokat regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="85408-136">If your account is assigned to the User role, but the app registration setting (from the preceding steps) is limited to admin users, ask your administrator to either assign you to an administrator role, or to enable users to register apps.</span></span>

     ![nézet szerepkör](./media/resource-group-create-service-principal-portal/view-role.png)

### <a name="check-azure-subscription-permissions"></a><span data-ttu-id="85408-138">Azure-előfizetés engedélyek ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="85408-138">Check Azure subscription permissions</span></span>
<span data-ttu-id="85408-139">Az Azure-előfizetéséhez, a fiók rendelkezik `Microsoft.Authorization/*/Write` egy AD-alkalmazás hozzárendelése szerepkörhöz a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="85408-139">In your Azure subscription, your account must have `Microsoft.Authorization/*/Write` access to assign an AD app to a role.</span></span> <span data-ttu-id="85408-140">Ez a művelet biztosítja a [tulajdonos](../active-directory/role-based-access-built-in-roles.md#owner) szerepkör vagy [felhasználói hozzáférés adminisztrátora](../active-directory/role-based-access-built-in-roles.md#user-access-administrator) szerepkör.</span><span class="sxs-lookup"><span data-stu-id="85408-140">This action is granted through the [Owner](../active-directory/role-based-access-built-in-roles.md#owner) role or [User Access Administrator](../active-directory/role-based-access-built-in-roles.md#user-access-administrator) role.</span></span> <span data-ttu-id="85408-141">Ha a fiók hozzá van rendelve a **közreműködő** szerepkör, Önnek nincs megfelelő engedélye.</span><span class="sxs-lookup"><span data-stu-id="85408-141">If your account is assigned to the **Contributor** role, you do not have adequate permission.</span></span> <span data-ttu-id="85408-142">Egy hibaüzenetet fog kapni, megkísérlésekor. a szolgáltatás egyszerű hozzárendelése egy szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="85408-142">You will receive an error when attempting to assign the service principal to a role.</span></span> 

<span data-ttu-id="85408-143">Ellenőrizze előfizetése engedélyei között:</span><span class="sxs-lookup"><span data-stu-id="85408-143">To check your subscription permissions:</span></span>

1. <span data-ttu-id="85408-144">Ha nem már nézi, az Azure AD-fiókot az előző lépéseiből, válassza ki a **Azure Active Directory** a bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="85408-144">If you are not already looking at your Azure AD account from the preceding steps, select **Azure Active Directory** from the left pane.</span></span>

2. <span data-ttu-id="85408-145">Keresse meg az Azure AD-fiókot.</span><span class="sxs-lookup"><span data-stu-id="85408-145">Find your Azure AD account.</span></span> <span data-ttu-id="85408-146">Válassza ki **áttekintése** és **keresse meg azt a felhasználót** a gyorsan elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="85408-146">Select **Overview** and **Find a user** from Quick tasks.</span></span>

     ![felhasználó keresése](./media/resource-group-create-service-principal-portal/find-user.png)
2. <span data-ttu-id="85408-148">Keresse meg a fiókját, és válassza ki, ha azt.</span><span class="sxs-lookup"><span data-stu-id="85408-148">Search for your account, and select it when you find it.</span></span>

     ![felhasználó keresése](./media/resource-group-create-service-principal-portal/show-user.png) 
     
3. <span data-ttu-id="85408-150">Válassza ki **Azure-erőforrások**.</span><span class="sxs-lookup"><span data-stu-id="85408-150">Select **Azure resources**.</span></span>

     ![Erőforrások kiválasztása](./media/resource-group-create-service-principal-portal/select-azure-resources.png) 
3. <span data-ttu-id="85408-152">A hozzárendelt szerepkörök megtekintése, és határozza meg, hogy van-e megfelelő engedélyekkel az AD-alkalmazás hozzárendelése szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="85408-152">View your assigned roles, and determine if you have adequate permissions to assign an AD app to a role.</span></span> <span data-ttu-id="85408-153">Ha nem, kérje meg a előfizetési rendszergazda, akkor a felhasználói hozzáférés adminisztrátora szerepkörbe való felvételre.</span><span class="sxs-lookup"><span data-stu-id="85408-153">If not, ask your subscription administrator to add you to User Access Administrator role.</span></span> <span data-ttu-id="85408-154">Az alábbi ábrán a felhasználó rendelve a tulajdonosi szerepkört, a két előfizetésekhez, ami azt jelenti, hogy a felhasználó a megfelelő engedélyekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="85408-154">In the following image, the user is assigned to the Owner role for two subscriptions, which means that user has adequate permissions.</span></span> 

     ![engedélyek megjelenítése](./media/resource-group-create-service-principal-portal/view-assigned-roles.png)

## <a name="create-an-azure-active-directory-application"></a><span data-ttu-id="85408-156">Egy Azure Active Directory-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="85408-156">Create an Azure Active Directory application</span></span>
1. <span data-ttu-id="85408-157">Az Azure-fiók használatával jelentkezzen be a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="85408-157">Log in to your Azure Account through the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="85408-158">Válassza ki **az Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="85408-158">Select **Azure Active Directory**.</span></span>

     ![Válassza ki az azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)

4. <span data-ttu-id="85408-160">Válassza ki **App regisztrációk**.</span><span class="sxs-lookup"><span data-stu-id="85408-160">Select **App registrations**.</span></span>   

     ![Válassza ki az alkalmazás-regisztráció](./media/resource-group-create-service-principal-portal/select-app-registrations.png)
5. <span data-ttu-id="85408-162">Válassza a **Hozzáadás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="85408-162">Select **Add**.</span></span>

     ![alkalmazás hozzáadása](./media/resource-group-create-service-principal-portal/select-add-app.png)

6. <span data-ttu-id="85408-164">Adjon meg egy nevet és egy URL-címet az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="85408-164">Provide a name and URL for the application.</span></span> <span data-ttu-id="85408-165">Válassza ki vagy **Web app / API** vagy **natív** a létrehozandó alkalmazás típusától.</span><span class="sxs-lookup"><span data-stu-id="85408-165">Select either **Web app / API** or **Native** for the type of application you want to create.</span></span> <span data-ttu-id="85408-166">Miután beállította az értékeket, válassza ki a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="85408-166">After setting the values, select **Create**.</span></span>

     ![alkalmazás neve](./media/resource-group-create-service-principal-portal/create-app.png)

<span data-ttu-id="85408-168">Az alkalmazás hozott létre.</span><span class="sxs-lookup"><span data-stu-id="85408-168">You have created your application.</span></span>

## <a name="get-application-id-and-authentication-key"></a><span data-ttu-id="85408-169">Alkalmazás azonosítója és a hitelesítési kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="85408-169">Get application ID and authentication key</span></span>
<span data-ttu-id="85408-170">Bejelentkezéskor programozott módon, a azonosító szükséges az alkalmazás és egy hitelesítési kulcs.</span><span class="sxs-lookup"><span data-stu-id="85408-170">When programmatically logging in, you need the ID for your application and an authentication key.</span></span> <span data-ttu-id="85408-171">Ahhoz, hogy ezeket az értékeket, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="85408-171">To get those values, use the following steps:</span></span>

1. <span data-ttu-id="85408-172">A **App regisztrációk** az Azure Active Directoryban, válassza ki az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="85408-172">From **App registrations** in Azure Active Directory, select your application.</span></span>

     ![alkalmazás kiválasztása](./media/resource-group-create-service-principal-portal/select-app.png)
2. <span data-ttu-id="85408-174">Másolás a **Alkalmazásazonosító** és az alkalmazás kódjában tárolja.</span><span class="sxs-lookup"><span data-stu-id="85408-174">Copy the **Application ID** and store it in your application code.</span></span> <span data-ttu-id="85408-175">Az alkalmazások a [mintaalkalmazást](#sample-applications) szakasz tekintse meg ezt az értéket az ügyfél-azonosító.</span><span class="sxs-lookup"><span data-stu-id="85408-175">The applications in the [sample applications](#sample-applications) section refer to this value as the client id.</span></span>

     ![ügyfél-azonosító](./media/resource-group-create-service-principal-portal/copy-app-id.png)
3. <span data-ttu-id="85408-177">A hitelesítési kulcs létrehozásához válasszon **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="85408-177">To generate an authentication key, select **Keys**.</span></span>

     ![Válassza ki a kulcsok](./media/resource-group-create-service-principal-portal/select-keys.png)
4. <span data-ttu-id="85408-179">Adja meg a kulcsot, és egy időtartamot a kulcs leírását.</span><span class="sxs-lookup"><span data-stu-id="85408-179">Provide a description of the key, and a duration for the key.</span></span> <span data-ttu-id="85408-180">Ha elkészült, válassza ki a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="85408-180">When done, select **Save**.</span></span>

     ![kulcs mentése](./media/resource-group-create-service-principal-portal/save-key.png)

     <span data-ttu-id="85408-182">A kulcs mentése után a kulcsnak az értéke megjelenik.</span><span class="sxs-lookup"><span data-stu-id="85408-182">After saving the key, the value of the key is displayed.</span></span> <span data-ttu-id="85408-183">Másolja ezt az értéket, mert nem sikerült beolvasni a a kulcs később.</span><span class="sxs-lookup"><span data-stu-id="85408-183">Copy this value because you are not able to retrieve the key later.</span></span> <span data-ttu-id="85408-184">A kulcs értéke az alkalmazás azonosítójával jelentkezzen be az alkalmazás számára adja meg.</span><span class="sxs-lookup"><span data-stu-id="85408-184">You provide the key value with the application ID to log in as the application.</span></span> <span data-ttu-id="85408-185">Tárolja a kulcs értékét, ahol az alkalmazás kérheti le.</span><span class="sxs-lookup"><span data-stu-id="85408-185">Store the key value where your application can retrieve it.</span></span>

     ![kulcs mentése](./media/resource-group-create-service-principal-portal/copy-key.png)

## <a name="get-tenant-id"></a><span data-ttu-id="85408-187">-Bérlőazonosító beszerzése</span><span class="sxs-lookup"><span data-stu-id="85408-187">Get tenant ID</span></span>
<span data-ttu-id="85408-188">Bejelentkezéskor programozott módon, kell átadni a hitelesítési kéréshez a bérlő azonosítója.</span><span class="sxs-lookup"><span data-stu-id="85408-188">When programmatically logging in, you need to pass the tenant ID with your authentication request.</span></span> 

1. <span data-ttu-id="85408-189">Válassza ki ahhoz, hogy a bérlő azonosítója, **tulajdonságok** az Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="85408-189">To get the tenant ID, select **Properties** for your Azure AD tenant.</span></span> 

     ![Válassza ki az Azure AD-tulajdonságok](./media/resource-group-create-service-principal-portal/select-ad-properties.png)

2. <span data-ttu-id="85408-191">Másolás a **könyvtár-azonosítója**.</span><span class="sxs-lookup"><span data-stu-id="85408-191">Copy the **Directory ID**.</span></span> <span data-ttu-id="85408-192">Ez az érték az a bérlő azonosítója.</span><span class="sxs-lookup"><span data-stu-id="85408-192">This value is your tenant ID.</span></span>

     ![bérlő azonosítója](./media/resource-group-create-service-principal-portal/copy-directory-id.png)

## <a name="assign-application-to-role"></a><span data-ttu-id="85408-194">Szerepkör alkalmazást</span><span class="sxs-lookup"><span data-stu-id="85408-194">Assign application to role</span></span>
<span data-ttu-id="85408-195">Az előfizetés az erőforrások eléréséhez az alkalmazást egy szerepkörhöz kell rendelni.</span><span class="sxs-lookup"><span data-stu-id="85408-195">To access resources in your subscription, you must assign the application to a role.</span></span> <span data-ttu-id="85408-196">Döntse el, melyik szerepkört jelöli a megfelelő engedélyekkel az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="85408-196">Decide which role represents the right permissions for the application.</span></span> <span data-ttu-id="85408-197">A rendelkezésre álló szerepkörökkel kapcsolatos további tudnivalókért lásd: [RBAC: beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="85408-197">To learn about the available roles, see [RBAC: Built in Roles](../active-directory/role-based-access-built-in-roles.md).</span></span>

<span data-ttu-id="85408-198">A hatókör szintjén található az előfizetés, erőforráscsoportból vagy erőforrás állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="85408-198">You can set the scope at the level of the subscription, resource group, or resource.</span></span> <span data-ttu-id="85408-199">Engedélyek hatókör alacsonyabb szintre származnak.</span><span class="sxs-lookup"><span data-stu-id="85408-199">Permissions are inherited to lower levels of scope.</span></span> <span data-ttu-id="85408-200">Például egy alkalmazást az olvasó szerepkört erőforráscsoport hozzáadása azt jelenti, hogy ez az erőforráscsoport és a benne található erőforrásokat tudja olvasni.</span><span class="sxs-lookup"><span data-stu-id="85408-200">For example, adding an application to the Reader role for a resource group means it can read the resource group and any resources it contains.</span></span>

1. <span data-ttu-id="85408-201">Nyissa meg a szint szeretne hozzárendelni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="85408-201">Navigate to the level of scope you wish to assign the application to.</span></span> <span data-ttu-id="85408-202">Például egy szerepkört az előfizetés hatókörből rendeléséhez jelölje **előfizetések**.</span><span class="sxs-lookup"><span data-stu-id="85408-202">For example, to assign a role at the subscription scope, select **Subscriptions**.</span></span> <span data-ttu-id="85408-203">Ehelyett kiválaszthatja egy erőforráscsoport vagy az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="85408-203">You could instead select a resource group or resource.</span></span>

     ![Válassza ki az előfizetést](./media/resource-group-create-service-principal-portal/select-subscription.png)

2. <span data-ttu-id="85408-205">Válassza az adott előfizetés (erőforráscsoport vagy erőforrás) az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="85408-205">Select the particular subscription (resource group or resource) to assign the application to.</span></span>

     ![Válassza ki az előfizetést a hozzárendeléshez](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

3. <span data-ttu-id="85408-207">Válassza ki **hozzáférés-vezérlés (IAM)**.</span><span class="sxs-lookup"><span data-stu-id="85408-207">Select **Access Control (IAM)**.</span></span>

     ![Jelölje be a hozzáférés](./media/resource-group-create-service-principal-portal/select-access-control.png)

4. <span data-ttu-id="85408-209">Válassza a **Hozzáadás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="85408-209">Select **Add**.</span></span>

     ![Válassza ki hozzáadása](./media/resource-group-create-service-principal-portal/select-add.png)
6. <span data-ttu-id="85408-211">Jelölje ki az alkalmazáshoz hozzárendelni kívánt szerepkört.</span><span class="sxs-lookup"><span data-stu-id="85408-211">Select the role you wish to assign to the application.</span></span> <span data-ttu-id="85408-212">Az alábbi képen látható a **olvasó** szerepkör.</span><span class="sxs-lookup"><span data-stu-id="85408-212">The following image shows the **Reader** role.</span></span>

     ![szerepkör kiválasztása](./media/resource-group-create-service-principal-portal/select-role.png)

8. <span data-ttu-id="85408-214">Keresse meg az alkalmazást, és válassza ki azt.</span><span class="sxs-lookup"><span data-stu-id="85408-214">Search for your application, and select it.</span></span>

     ![alkalmazások keresése](./media/resource-group-create-service-principal-portal/search-app.png)
9. <span data-ttu-id="85408-216">Válassza ki **OK** a szerepkör hozzárendelése befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="85408-216">Select **OK** to finish assigning the role.</span></span> <span data-ttu-id="85408-217">Megjelenik a listában, hogy a hatókör adott szerepkörhöz rendelt felhasználók az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="85408-217">You see your application in the list of users assigned to a role for that scope.</span></span>

## <a name="log-in-as-the-application"></a><span data-ttu-id="85408-218">Jelentkezzen be az alkalmazás</span><span class="sxs-lookup"><span data-stu-id="85408-218">Log in as the application</span></span>

<span data-ttu-id="85408-219">Az alkalmazás most már be van állítva az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="85408-219">Your application is now set up in Azure Active Directory.</span></span> <span data-ttu-id="85408-220">Rendelkezik egy Azonosítóját és kulcsát az alkalmazás aláíráshoz használandó.</span><span class="sxs-lookup"><span data-stu-id="85408-220">You have an ID and key to use for signing in as the application.</span></span> <span data-ttu-id="85408-221">Az alkalmazás hozzá van rendelve egy szerepkör, amely azt teszi bizonyos műveleteket hajthat végre műveleteket.</span><span class="sxs-lookup"><span data-stu-id="85408-221">The application is assigned to a role that gives it certain actions it can perform.</span></span> <span data-ttu-id="85408-222">Az alkalmazás a különböző platformokat, a bejelentkezés kapcsolatos információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="85408-222">For information about logging in as the application through different platforms, see:</span></span>

* [<span data-ttu-id="85408-223">PowerShell</span><span class="sxs-lookup"><span data-stu-id="85408-223">PowerShell</span></span>](resource-group-authenticate-service-principal.md#provide-credentials-through-powershell)
* [<span data-ttu-id="85408-224">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="85408-224">Azure CLI</span></span>](resource-group-authenticate-service-principal-cli.md#provide-credentials-through-azure-cli)
* [<span data-ttu-id="85408-225">REST</span><span class="sxs-lookup"><span data-stu-id="85408-225">REST</span></span>](/rest/api/#create-the-request)
* [<span data-ttu-id="85408-226">.NET</span><span class="sxs-lookup"><span data-stu-id="85408-226">.NET</span></span>](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [<span data-ttu-id="85408-227">Java</span><span class="sxs-lookup"><span data-stu-id="85408-227">Java</span></span>](/java/azure/java-sdk-azure-authenticate)
* [<span data-ttu-id="85408-228">Node.js</span><span class="sxs-lookup"><span data-stu-id="85408-228">Node.js</span></span>](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [<span data-ttu-id="85408-229">Python</span><span class="sxs-lookup"><span data-stu-id="85408-229">Python</span></span>](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [<span data-ttu-id="85408-230">Ruby</span><span class="sxs-lookup"><span data-stu-id="85408-230">Ruby</span></span>](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## <a name="next-steps"></a><span data-ttu-id="85408-231">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="85408-231">Next steps</span></span>
* <span data-ttu-id="85408-232">Egy több-bérlős alkalmazás beállításához tekintse meg a [fejlesztői útmutató az Azure Resource Manager API-val engedélyezési](resource-manager-api-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="85408-232">To set up a multi-tenant application, see [Developer's guide to authorization with the Azure Resource Manager API](resource-manager-api-authentication.md).</span></span>
* <span data-ttu-id="85408-233">Biztonsági házirendek megadásával kapcsolatos további tudnivalókért lásd: [Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="85408-233">To learn about specifying security policies, see [Azure Role-based Access Control](../active-directory/role-based-access-control-configure.md).</span></span>  
* <span data-ttu-id="85408-234">Elérhető műveleteket, lehet megadott vagy megtagadta a felhasználók listáját lásd: [Azure Resource Manager erőforrás-szolgáltató műveletek](../active-directory/role-based-access-control-resource-provider-operations.md).</span><span class="sxs-lookup"><span data-stu-id="85408-234">For a list of available actions that can be granted or denied to users, see [Azure Resource Manager Resource Provider operations](../active-directory/role-based-access-control-resource-provider-operations.md).</span></span>
