---
title: "Azure-portál alkalmazás aaaCreate identitást |} Microsoft Docs"
description: "Ismerteti, hogyan toocreate egy új Azure Active Directory-alkalmazás és egyszerű, amely használható az Azure Resource Manager toomanage hello szerepköralapú hozzáférés-vezérlés szolgáltatás hozzáférés tooresources."
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
ms.openlocfilehash: 9624715ac612f42df6f9e9e67b8233bd4b914174
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-portal-toocreate-an-azure-active-directory-application-and-service-principal-that-can-access-resources"></a><span data-ttu-id="42461-103">Portál toocreate használja az Azure Active Directory-alkalmazást és egy egyszerű szolgáltatást, amely erőforrások eléréséhez</span><span class="sxs-lookup"><span data-stu-id="42461-103">Use portal toocreate an Azure Active Directory application and service principal that can access resources</span></span>

<span data-ttu-id="42461-104">Ha egy alkalmazás, amely tooaccess kell, vagy módosítsa az erőforrásokat, állítson be egy Azure Active Directory (AD) alkalmazás, és rendelje hozzá a szükséges hello engedélyek tooit.</span><span class="sxs-lookup"><span data-stu-id="42461-104">When you have an application that needs tooaccess or modify resources, you must set up an Azure Active Directory (AD) application and assign hello required permissions tooit.</span></span> <span data-ttu-id="42461-105">Ez a módszer előnyösebb toorunning hello alkalmazás a saját credentials azért, mert:</span><span class="sxs-lookup"><span data-stu-id="42461-105">This approach is preferable toorunning hello app under your own credentials because:</span></span>

* <span data-ttu-id="42461-106">Engedélyeket rendelhet a saját engedélyeit eltérő toohello identitását.</span><span class="sxs-lookup"><span data-stu-id="42461-106">You can assign permissions toohello app identity that are different than your own permissions.</span></span> <span data-ttu-id="42461-107">Ezek az engedélyek általában korlátozott tooexactly milyen hello alkalmazásnak kell toodo.</span><span class="sxs-lookup"><span data-stu-id="42461-107">Typically, these permissions are restricted tooexactly what hello app needs toodo.</span></span>
* <span data-ttu-id="42461-108">Ha módosítja az Ön feladatkörei nincs toochange hello alkalmazás hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="42461-108">You do not have toochange hello app's credentials if your responsibilities change.</span></span> 
* <span data-ttu-id="42461-109">Használhatja a tanúsítványhitelesítés tooautomate egy felügyelet nélküli parancsfájl végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="42461-109">You can use a certificate tooautomate authentication when executing an unattended script.</span></span>

<span data-ttu-id="42461-110">Ez a témakör bemutatja, hogyan tooperform azokat lépésről-lépésre hello portálon.</span><span class="sxs-lookup"><span data-stu-id="42461-110">This topic shows you how tooperform those steps through hello portal.</span></span> <span data-ttu-id="42461-111">A single-bérlő alkalmazás hello alkalmazás esetén csak egy szervezeten belül tervezett toorun összpontosít.</span><span class="sxs-lookup"><span data-stu-id="42461-111">It focuses on a single-tenant application where hello application is intended toorun within only one organization.</span></span> <span data-ttu-id="42461-112">Általában egy bérlői alkalmazásokat használ futó üzleti alkalmazásokhoz a szervezeten belül.</span><span class="sxs-lookup"><span data-stu-id="42461-112">You typically use single-tenant applications for line-of-business applications that run within your organization.</span></span>
 
## <a name="required-permissions"></a><span data-ttu-id="42461-113">Szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="42461-113">Required permissions</span></span>
<span data-ttu-id="42461-114">toocomplete ebben a témakörben kell, hogy megfelelő engedélyek tooregister egy alkalmazást az Azure AD-bérlőről, és hello alkalmazás tooa szerepkör hozzárendelése az Azure-előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="42461-114">toocomplete this topic, you must have sufficient permissions tooregister an application with your Azure AD tenant, and assign hello application tooa role in your Azure subscription.</span></span> <span data-ttu-id="42461-115">Most Meggyőződünk arról rendelkezik megfelelő engedélyekkel tooperform hello ezeket a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="42461-115">Let's make sure you have hello right permissions tooperform those steps.</span></span>

### <a name="check-azure-active-directory-permissions"></a><span data-ttu-id="42461-116">Azure Active Directory-engedélyek ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="42461-116">Check Azure Active Directory permissions</span></span>
1. <span data-ttu-id="42461-117">Jelentkezzen be Azure-fiók tooyour keresztül hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="42461-117">Log in tooyour Azure Account through hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="42461-118">Válassza ki **az Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="42461-118">Select **Azure Active Directory**.</span></span>

     ![Válassza ki az azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)
3. <span data-ttu-id="42461-120">Válassza ki az Azure Active Directoryban, **felhasználói beállítások**.</span><span class="sxs-lookup"><span data-stu-id="42461-120">In Azure Active Directory, select **User settings**.</span></span>

     ![Válassza ki a felhasználói beállítások](./media/resource-group-create-service-principal-portal/select-user-settings.png)
4. <span data-ttu-id="42461-122">Ellenőrizze a hello **App regisztrációk** beállítást.</span><span class="sxs-lookup"><span data-stu-id="42461-122">Check hello **App registrations** setting.</span></span> <span data-ttu-id="42461-123">Ha túl beállítása**Igen**, nem rendszergazdai felhasználók regisztrálhatják AD alkalmazásaiban.</span><span class="sxs-lookup"><span data-stu-id="42461-123">If set too**Yes**, non-admin users can register AD apps.</span></span> <span data-ttu-id="42461-124">Ez a beállítás azt jelenti, hogy minden olyan felhasználó, az Azure AD-bérlő hello regisztrálhatja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="42461-124">This setting means any user in hello Azure AD tenant can register an app.</span></span> <span data-ttu-id="42461-125">A Folytatás túl[Azure ellenőrizze előfizetése engedélyei között](#check-azure-subscription-permissions).</span><span class="sxs-lookup"><span data-stu-id="42461-125">You can proceed too[Check Azure subscription permissions](#check-azure-subscription-permissions).</span></span>

     ![alkalmazás-regisztrációk megtekintése](./media/resource-group-create-service-principal-portal/view-app-registrations.png)
5. <span data-ttu-id="42461-127">Ha hello app regisztrációk beállítás értéke túl**nem**, csak a rendszergazda felhasználók regisztrálhatják az alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="42461-127">If hello app registrations setting is set too**No**, only admin users can register apps.</span></span> <span data-ttu-id="42461-128">Toocheck kell-e a fiók egy rendszergazda hello Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="42461-128">You need toocheck whether your account is an admin for hello Azure AD tenant.</span></span> <span data-ttu-id="42461-129">Válassza ki **áttekintése** és **keresse meg azt a felhasználót** a gyorsan elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="42461-129">Select **Overview** and **Find a user** from Quick tasks.</span></span>

     ![felhasználó keresése](./media/resource-group-create-service-principal-portal/find-user.png)
6. <span data-ttu-id="42461-131">Keresse meg a fiókját, és válassza ki, ha azt.</span><span class="sxs-lookup"><span data-stu-id="42461-131">Search for your account, and select it when you find it.</span></span>

     ![felhasználó keresése](./media/resource-group-create-service-principal-portal/show-user.png)
7. <span data-ttu-id="42461-133">Válassza ki a fiókot, **Directory szerepkör**.</span><span class="sxs-lookup"><span data-stu-id="42461-133">For your account, select **Directory role**.</span></span> 

     ![Directory szerepkör](./media/resource-group-create-service-principal-portal/select-directory-role.png)
8. <span data-ttu-id="42461-135">A hozzárendelt directory szerepkör megtekintéséhez az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="42461-135">View your assigned directory role in Azure AD.</span></span> <span data-ttu-id="42461-136">Ha a fiókjához toohello felhasználói szerepkör van hozzárendelve, de hello regisztrációs Alkalmazásbeállítás (az előző lépésekben hello) korlátozott tooadmin felhasználók, kérje a rendszergazda tooeither rendelje hozzá, akkor tooan rendszergazdai szerepkör, illetve tooenable felhasználók tooregister alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="42461-136">If your account is assigned toohello User role, but hello app registration setting (from hello preceding steps) is limited tooadmin users, ask your administrator tooeither assign you tooan administrator role, or tooenable users tooregister apps.</span></span>

     ![nézet szerepkör](./media/resource-group-create-service-principal-portal/view-role.png)

### <a name="check-azure-subscription-permissions"></a><span data-ttu-id="42461-138">Azure-előfizetés engedélyek ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="42461-138">Check Azure subscription permissions</span></span>
<span data-ttu-id="42461-139">Az Azure-előfizetéséhez, a fiók rendelkezik `Microsoft.Authorization/*/Write` tooassign AD-alkalmazás tooa szerepet eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="42461-139">In your Azure subscription, your account must have `Microsoft.Authorization/*/Write` access tooassign an AD app tooa role.</span></span> <span data-ttu-id="42461-140">Ez a művelet biztosítja hello [tulajdonos](../active-directory/role-based-access-built-in-roles.md#owner) szerepkör vagy [felhasználói hozzáférés adminisztrátora](../active-directory/role-based-access-built-in-roles.md#user-access-administrator) szerepkör.</span><span class="sxs-lookup"><span data-stu-id="42461-140">This action is granted through hello [Owner](../active-directory/role-based-access-built-in-roles.md#owner) role or [User Access Administrator](../active-directory/role-based-access-built-in-roles.md#user-access-administrator) role.</span></span> <span data-ttu-id="42461-141">Ha a fiók hozzá van rendelve a toohello **közreműködő** szerepkör, Önnek nincs megfelelő engedélye.</span><span class="sxs-lookup"><span data-stu-id="42461-141">If your account is assigned toohello **Contributor** role, you do not have adequate permission.</span></span> <span data-ttu-id="42461-142">Egy hibaüzenetet fog kapni, tooassign hello szolgáltatás egyszerű tooa szerepkör megkísérlése során.</span><span class="sxs-lookup"><span data-stu-id="42461-142">You will receive an error when attempting tooassign hello service principal tooa role.</span></span> 

<span data-ttu-id="42461-143">toocheck előfizetése engedélyei között:</span><span class="sxs-lookup"><span data-stu-id="42461-143">toocheck your subscription permissions:</span></span>

1. <span data-ttu-id="42461-144">Ha nem már nézi, az Azure AD-fiókot az előző lépésekben hello, válassza ki a **Azure Active Directory** hello bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="42461-144">If you are not already looking at your Azure AD account from hello preceding steps, select **Azure Active Directory** from hello left pane.</span></span>

2. <span data-ttu-id="42461-145">Keresse meg az Azure AD-fiókot.</span><span class="sxs-lookup"><span data-stu-id="42461-145">Find your Azure AD account.</span></span> <span data-ttu-id="42461-146">Válassza ki **áttekintése** és **keresse meg azt a felhasználót** a gyorsan elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="42461-146">Select **Overview** and **Find a user** from Quick tasks.</span></span>

     ![felhasználó keresése](./media/resource-group-create-service-principal-portal/find-user.png)
2. <span data-ttu-id="42461-148">Keresse meg a fiókját, és válassza ki, ha azt.</span><span class="sxs-lookup"><span data-stu-id="42461-148">Search for your account, and select it when you find it.</span></span>

     ![felhasználó keresése](./media/resource-group-create-service-principal-portal/show-user.png) 
     
3. <span data-ttu-id="42461-150">Válassza ki **Azure-erőforrások**.</span><span class="sxs-lookup"><span data-stu-id="42461-150">Select **Azure resources**.</span></span>

     ![Erőforrások kiválasztása](./media/resource-group-create-service-principal-portal/select-azure-resources.png) 
3. <span data-ttu-id="42461-152">A hozzárendelt szerepkörök megtekintése, és határozza meg, ha rendelkezik megfelelő engedélyekkel tooassign AD-alkalmazás tooa szerepet.</span><span class="sxs-lookup"><span data-stu-id="42461-152">View your assigned roles, and determine if you have adequate permissions tooassign an AD app tooa role.</span></span> <span data-ttu-id="42461-153">Ha nem, kérje meg az előfizetés rendszergazdája tooadd meg tooUser hozzáférési rendszergazdai szerepkört.</span><span class="sxs-lookup"><span data-stu-id="42461-153">If not, ask your subscription administrator tooadd you tooUser Access Administrator role.</span></span> <span data-ttu-id="42461-154">A következő kép hello hello felhasználó, hozzárendelt toohello tulajdonosi szerepkör két elő, ami azt jelenti, hogy a felhasználó a megfelelő engedélyekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="42461-154">In hello following image, hello user is assigned toohello Owner role for two subscriptions, which means that user has adequate permissions.</span></span> 

     ![engedélyek megjelenítése](./media/resource-group-create-service-principal-portal/view-assigned-roles.png)

## <a name="create-an-azure-active-directory-application"></a><span data-ttu-id="42461-156">Egy Azure Active Directory-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="42461-156">Create an Azure Active Directory application</span></span>
1. <span data-ttu-id="42461-157">Jelentkezzen be Azure-fiók tooyour keresztül hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="42461-157">Log in tooyour Azure Account through hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="42461-158">Válassza ki **az Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="42461-158">Select **Azure Active Directory**.</span></span>

     ![Válassza ki az azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)

4. <span data-ttu-id="42461-160">Válassza ki **App regisztrációk**.</span><span class="sxs-lookup"><span data-stu-id="42461-160">Select **App registrations**.</span></span>   

     ![Válassza ki az alkalmazás-regisztráció](./media/resource-group-create-service-principal-portal/select-app-registrations.png)
5. <span data-ttu-id="42461-162">Válassza a **Hozzáadás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="42461-162">Select **Add**.</span></span>

     ![alkalmazás hozzáadása](./media/resource-group-create-service-principal-portal/select-add-app.png)

6. <span data-ttu-id="42461-164">Adjon meg egy nevet és egy URL-cím hello alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="42461-164">Provide a name and URL for hello application.</span></span> <span data-ttu-id="42461-165">Válassza ki vagy **Web app / API** vagy **natív** hello alkalmazástípus toocreate keresi.</span><span class="sxs-lookup"><span data-stu-id="42461-165">Select either **Web app / API** or **Native** for hello type of application you want toocreate.</span></span> <span data-ttu-id="42461-166">Miután beállította a hello értékek, válassza ki a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="42461-166">After setting hello values, select **Create**.</span></span>

     ![alkalmazás neve](./media/resource-group-create-service-principal-portal/create-app.png)

<span data-ttu-id="42461-168">Az alkalmazás hozott létre.</span><span class="sxs-lookup"><span data-stu-id="42461-168">You have created your application.</span></span>

## <a name="get-application-id-and-authentication-key"></a><span data-ttu-id="42461-169">Alkalmazás azonosítója és a hitelesítési kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="42461-169">Get application ID and authentication key</span></span>
<span data-ttu-id="42461-170">Bejelentkezéskor programozott módon, az alkalmazás és egy hitelesítési kulcsot kell hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="42461-170">When programmatically logging in, you need hello ID for your application and an authentication key.</span></span> <span data-ttu-id="42461-171">tooget megadott értékeket használja hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="42461-171">tooget those values, use hello following steps:</span></span>

1. <span data-ttu-id="42461-172">A **App regisztrációk** az Azure Active Directoryban, válassza ki az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="42461-172">From **App registrations** in Azure Active Directory, select your application.</span></span>

     ![alkalmazás kiválasztása](./media/resource-group-create-service-principal-portal/select-app.png)
2. <span data-ttu-id="42461-174">Másolás hello **Alkalmazásazonosító** és az alkalmazás kódjában tárolja.</span><span class="sxs-lookup"><span data-stu-id="42461-174">Copy hello **Application ID** and store it in your application code.</span></span> <span data-ttu-id="42461-175">hello alkalmazások hello [mintaalkalmazást](#sample-applications) szakasz tekintse meg a toothis értékével megegyező hello ügyfél-azonosító.</span><span class="sxs-lookup"><span data-stu-id="42461-175">hello applications in hello [sample applications](#sample-applications) section refer toothis value as hello client id.</span></span>

     ![ügyfél-azonosító](./media/resource-group-create-service-principal-portal/copy-app-id.png)
3. <span data-ttu-id="42461-177">egy hitelesítési kulcs toogenerate válasszon **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="42461-177">toogenerate an authentication key, select **Keys**.</span></span>

     ![Válassza ki a kulcsok](./media/resource-group-create-service-principal-portal/select-keys.png)
4. <span data-ttu-id="42461-179">Adja meg a hello-kulccsal és egy időtartamot hello kulcs leírását.</span><span class="sxs-lookup"><span data-stu-id="42461-179">Provide a description of hello key, and a duration for hello key.</span></span> <span data-ttu-id="42461-180">Ha elkészült, válassza ki a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="42461-180">When done, select **Save**.</span></span>

     ![kulcs mentése](./media/resource-group-create-service-principal-portal/save-key.png)

     <span data-ttu-id="42461-182">Hello kulcs mentése, után hello hello kulcs értéke jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="42461-182">After saving hello key, hello value of hello key is displayed.</span></span> <span data-ttu-id="42461-183">Másolja ezt az értéket, mert nem tudja tooretrieve hello kulcs később.</span><span class="sxs-lookup"><span data-stu-id="42461-183">Copy this value because you are not able tooretrieve hello key later.</span></span> <span data-ttu-id="42461-184">Hello alkalmazás azonosítója toolog a hello kulcsérték rendelkezésére hello alkalmazásként.</span><span class="sxs-lookup"><span data-stu-id="42461-184">You provide hello key value with hello application ID toolog in as hello application.</span></span> <span data-ttu-id="42461-185">Tárolja a hello kulcs értékét, ahol az alkalmazás kérheti le.</span><span class="sxs-lookup"><span data-stu-id="42461-185">Store hello key value where your application can retrieve it.</span></span>

     ![kulcs mentése](./media/resource-group-create-service-principal-portal/copy-key.png)

## <a name="get-tenant-id"></a><span data-ttu-id="42461-187">-Bérlőazonosító beszerzése</span><span class="sxs-lookup"><span data-stu-id="42461-187">Get tenant ID</span></span>
<span data-ttu-id="42461-188">Bejelentkezéskor programozott módon, toopass hello Bérlőazonosító van szüksége a hitelesítési kérelmet.</span><span class="sxs-lookup"><span data-stu-id="42461-188">When programmatically logging in, you need toopass hello tenant ID with your authentication request.</span></span> 

1. <span data-ttu-id="42461-189">tooget hello Bérlőazonosító, válassza ki **tulajdonságok** az Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="42461-189">tooget hello tenant ID, select **Properties** for your Azure AD tenant.</span></span> 

     ![Válassza ki az Azure AD-tulajdonságok](./media/resource-group-create-service-principal-portal/select-ad-properties.png)

2. <span data-ttu-id="42461-191">Másolás hello **könyvtár-azonosítója**.</span><span class="sxs-lookup"><span data-stu-id="42461-191">Copy hello **Directory ID**.</span></span> <span data-ttu-id="42461-192">Ez az érték az a bérlő azonosítója.</span><span class="sxs-lookup"><span data-stu-id="42461-192">This value is your tenant ID.</span></span>

     ![bérlő azonosítója](./media/resource-group-create-service-principal-portal/copy-directory-id.png)

## <a name="assign-application-toorole"></a><span data-ttu-id="42461-194">Rendelje hozzá az alkalmazás toorole</span><span class="sxs-lookup"><span data-stu-id="42461-194">Assign application toorole</span></span>
<span data-ttu-id="42461-195">tooaccess erőforrást az előfizetésében, hozzá kell rendelnie hello alkalmazás tooa szerepkör.</span><span class="sxs-lookup"><span data-stu-id="42461-195">tooaccess resources in your subscription, you must assign hello application tooa role.</span></span> <span data-ttu-id="42461-196">Döntse el, melyik szerepkör hello a megfelelő engedélyekkel a hello alkalmazás jelöli.</span><span class="sxs-lookup"><span data-stu-id="42461-196">Decide which role represents hello right permissions for hello application.</span></span> <span data-ttu-id="42461-197">toolearn hello elérhető szerepkörök, lásd: [RBAC: beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="42461-197">toolearn about hello available roles, see [RBAC: Built in Roles](../active-directory/role-based-access-built-in-roles.md).</span></span>

<span data-ttu-id="42461-198">Hello hatókör hello előfizetés, az erőforráscsoportot, vagy az erőforrás hello szinten állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="42461-198">You can set hello scope at hello level of hello subscription, resource group, or resource.</span></span> <span data-ttu-id="42461-199">Az engedélyek hatóköre örökölt toolower szintű is.</span><span class="sxs-lookup"><span data-stu-id="42461-199">Permissions are inherited toolower levels of scope.</span></span> <span data-ttu-id="42461-200">Például hozzáadása egy alkalmazáshoz toohello olvasó szerepkört erőforráscsoport azt jelenti, hogy az tud olvasni hello erőforráscsoport és a benne található erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="42461-200">For example, adding an application toohello Reader role for a resource group means it can read hello resource group and any resources it contains.</span></span>

1. <span data-ttu-id="42461-201">Keresse meg a kívánt tooassign hello alkalmazás hatókör toohello szintjét.</span><span class="sxs-lookup"><span data-stu-id="42461-201">Navigate toohello level of scope you wish tooassign hello application to.</span></span> <span data-ttu-id="42461-202">Például tooassign hello előfizetés hatókörben szerepkör kiválasztása **előfizetések**.</span><span class="sxs-lookup"><span data-stu-id="42461-202">For example, tooassign a role at hello subscription scope, select **Subscriptions**.</span></span> <span data-ttu-id="42461-203">Ehelyett kiválaszthatja egy erőforráscsoport vagy az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="42461-203">You could instead select a resource group or resource.</span></span>

     ![Válassza ki az előfizetést](./media/resource-group-create-service-principal-portal/select-subscription.png)

2. <span data-ttu-id="42461-205">Válassza ki a hello adott előfizetés (erőforráscsoport vagy erőforrás) tooassign hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="42461-205">Select hello particular subscription (resource group or resource) tooassign hello application to.</span></span>

     ![Válassza ki az előfizetést a hozzárendeléshez](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

3. <span data-ttu-id="42461-207">Válassza ki **hozzáférés-vezérlés (IAM)**.</span><span class="sxs-lookup"><span data-stu-id="42461-207">Select **Access Control (IAM)**.</span></span>

     ![Jelölje be a hozzáférés](./media/resource-group-create-service-principal-portal/select-access-control.png)

4. <span data-ttu-id="42461-209">Válassza a **Hozzáadás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="42461-209">Select **Add**.</span></span>

     ![Válassza ki hozzáadása](./media/resource-group-create-service-principal-portal/select-add.png)
6. <span data-ttu-id="42461-211">Válassza ki a hello szerepkör tooassign toohello alkalmazás kívánja.</span><span class="sxs-lookup"><span data-stu-id="42461-211">Select hello role you wish tooassign toohello application.</span></span> <span data-ttu-id="42461-212">hello következő kép bemutatja hello **olvasó** szerepkör.</span><span class="sxs-lookup"><span data-stu-id="42461-212">hello following image shows hello **Reader** role.</span></span>

     ![szerepkör kiválasztása](./media/resource-group-create-service-principal-portal/select-role.png)

8. <span data-ttu-id="42461-214">Keresse meg az alkalmazást, és válassza ki azt.</span><span class="sxs-lookup"><span data-stu-id="42461-214">Search for your application, and select it.</span></span>

     ![alkalmazások keresése](./media/resource-group-create-service-principal-portal/search-app.png)
9. <span data-ttu-id="42461-216">Válassza ki **OK** toofinish hello szerepkör hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="42461-216">Select **OK** toofinish assigning hello role.</span></span> <span data-ttu-id="42461-217">Megjelenik a felhasználók tooa szerepkörrel rendelkeznek az adott hatókörnél hello lista alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="42461-217">You see your application in hello list of users assigned tooa role for that scope.</span></span>

## <a name="log-in-as-hello-application"></a><span data-ttu-id="42461-218">Jelentkezzen be hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="42461-218">Log in as hello application</span></span>

<span data-ttu-id="42461-219">Az alkalmazás most már be van állítva az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="42461-219">Your application is now set up in Azure Active Directory.</span></span> <span data-ttu-id="42461-220">Rendelkezik egy Azonosítót és a kulcs toouse hello alkalmazásként aláíráshoz.</span><span class="sxs-lookup"><span data-stu-id="42461-220">You have an ID and key toouse for signing in as hello application.</span></span> <span data-ttu-id="42461-221">hello alkalmazás szerepét tooa, mely bizonyos műveleteket hajthat végre műveleteket.</span><span class="sxs-lookup"><span data-stu-id="42461-221">hello application is assigned tooa role that gives it certain actions it can perform.</span></span> <span data-ttu-id="42461-222">Különböző platformokon keresztül hello alkalmazásként naplózás kapcsolatos információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="42461-222">For information about logging in as hello application through different platforms, see:</span></span>

* [<span data-ttu-id="42461-223">PowerShell</span><span class="sxs-lookup"><span data-stu-id="42461-223">PowerShell</span></span>](resource-group-authenticate-service-principal.md#provide-credentials-through-powershell)
* [<span data-ttu-id="42461-224">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="42461-224">Azure CLI</span></span>](resource-group-authenticate-service-principal-cli.md#provide-credentials-through-azure-cli)
* [<span data-ttu-id="42461-225">REST</span><span class="sxs-lookup"><span data-stu-id="42461-225">REST</span></span>](/rest/api/#create-the-request)
* [<span data-ttu-id="42461-226">.NET</span><span class="sxs-lookup"><span data-stu-id="42461-226">.NET</span></span>](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [<span data-ttu-id="42461-227">Java</span><span class="sxs-lookup"><span data-stu-id="42461-227">Java</span></span>](/java/azure/java-sdk-azure-authenticate)
* [<span data-ttu-id="42461-228">Node.js</span><span class="sxs-lookup"><span data-stu-id="42461-228">Node.js</span></span>](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [<span data-ttu-id="42461-229">Python</span><span class="sxs-lookup"><span data-stu-id="42461-229">Python</span></span>](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [<span data-ttu-id="42461-230">Ruby</span><span class="sxs-lookup"><span data-stu-id="42461-230">Ruby</span></span>](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## <a name="next-steps"></a><span data-ttu-id="42461-231">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="42461-231">Next steps</span></span>
* <span data-ttu-id="42461-232">egy több-bérlős alkalmazást, be tooset lásd: [– fejlesztői útmutató tooauthorization hello Azure Resource Manager API-val rendelkező](resource-manager-api-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="42461-232">tooset up a multi-tenant application, see [Developer's guide tooauthorization with hello Azure Resource Manager API](resource-manager-api-authentication.md).</span></span>
* <span data-ttu-id="42461-233">toolearn biztonsági házirendek meghatározásával kapcsolatban lásd: [Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="42461-233">toolearn about specifying security policies, see [Azure Role-based Access Control](../active-directory/role-based-access-control-configure.md).</span></span>  
* <span data-ttu-id="42461-234">Az elérhető műveleteket, lehet megadott vagy toousers megtagadva listájáért lásd: [Azure Resource Manager erőforrás-szolgáltató műveletek](../active-directory/role-based-access-control-resource-provider-operations.md).</span><span class="sxs-lookup"><span data-stu-id="42461-234">For a list of available actions that can be granted or denied toousers, see [Azure Resource Manager Resource Provider operations](../active-directory/role-based-access-control-resource-provider-operations.md).</span></span>
