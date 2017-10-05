---
title: "Engedélyezi a fejlesztői fiókok Azure Active Directory - Azure API Management használata |} Microsoft Docs"
description: "Útmutató az Azure Active Directoryval az API Management felhasználók hitelesítéséhez."
services: api-management
documentationcenter: API Management
author: steved0x
manager: erikre
editor: 
ms.assetid: 33a69a83-94f2-4e4e-9cef-f2a5af3c9732
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 7637e6419d17a2d75904fbe63df5f27d4be4bbe3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a><span data-ttu-id="257ee-103">Hogyan szeretné engedélyekkel felruházni fejlesztői fiókok Azure Active Directory használatával az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="257ee-103">How to authorize developer accounts using Azure Active Directory in Azure API Management</span></span>
## <a name="overview"></a><span data-ttu-id="257ee-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="257ee-104">Overview</span></span>
<span data-ttu-id="257ee-105">Ez az útmutató bemutatja, hogyan engedélyezze a hozzáférést a fejlesztői portálhoz, a felhasználók számára az Azure Active Directoryból.</span><span class="sxs-lookup"><span data-stu-id="257ee-105">This guide shows you how to enable access to the developer portal for users from Azure Active Directory.</span></span> <span data-ttu-id="257ee-106">Ez az útmutató is bemutatja, hogyan Azure Active Directory-felhasználók kezelése külső csoportok, amelyek tartalmazzák az Azure Active Directory a felhasználók hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="257ee-106">This guide also shows you how to manage groups of Azure Active Directory users by adding external groups that contain the users of an Azure Active Directory.</span></span>

> <span data-ttu-id="257ee-107">Ez az útmutató lépéseinek végrehajtásához először egy Azure Active Directory használandó hozzon létre egy alkalmazást kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="257ee-107">To complete the steps in this guide you must first have an Azure Active Directory in which to create an application.</span></span>
> 
> 

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a><span data-ttu-id="257ee-108">Hogyan szeretné engedélyekkel felruházni fejlesztői fiókok Azure Active Directory használatával</span><span class="sxs-lookup"><span data-stu-id="257ee-108">How to authorize developer accounts using Azure Active Directory</span></span>
<span data-ttu-id="257ee-109">A kezdéshez kattintson **Publisher portal** az API Management szolgáltatás az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="257ee-109">To get started, click **Publisher portal** in the Azure portal for your API Management service.</span></span> <span data-ttu-id="257ee-110">Ezzel továbblép az API Management közzétevő portáljára.</span><span class="sxs-lookup"><span data-stu-id="257ee-110">This takes you to the API Management publisher portal.</span></span>

![Közzétevő portál][api-management-management-console]

> <span data-ttu-id="257ee-112">Ha még nem hozott létre API Management szolgáltatáspéldányt, tekintse meg az [Ismerkedés az Azure API Management szolgáltatással][Get started with Azure API Management] oktatóanyag [API Management szolgáltatáspéldány létrehozása][Create an API Management service instance] című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="257ee-112">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="257ee-113">Kattintson a **biztonsági** a a **API Management** a bal oldalon, majd kattintson a menü **külső identitások**.</span><span class="sxs-lookup"><span data-stu-id="257ee-113">Click **Security** from the **API Management** menu on the left and click **External Identities**.</span></span>

![Külső identitások][api-management-security-external-identities]

<span data-ttu-id="257ee-115">Kattintson a **az Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="257ee-115">Click **Azure Active Directory**.</span></span> <span data-ttu-id="257ee-116">Jegyezze fel a **átirányítási URL-cím** és átválthat a klasszikus Azure-portálon az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="257ee-116">Make a note of the **Redirect URL** and switch over to your Azure Active Directory in the Azure Classic Portal.</span></span>

![Külső identitások][api-management-security-aad-new]

<span data-ttu-id="257ee-118">Kattintson a **Hozzáadás** gombra kattintva hozzon létre egy új Azure Active Directory-alkalmazást, és válassza a **a szerveztem által fejlesztett alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="257ee-118">Click the **Add** button to create a new Azure Active Directory application, and choose **Add an application my organization is developing**.</span></span>

![Új Azure Active Directory-alkalmazás hozzáadása][api-management-new-aad-application-menu]

<span data-ttu-id="257ee-120">Adjon meg egy nevet az alkalmazásnak, válassza a **webes alkalmazáshoz és/vagy webes API**, és kattintson a Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="257ee-120">Enter a name for the application, select **Web application and/or Web API**, and click the next button.</span></span>

![Új Azure Active Directory-alkalmazás][api-management-new-aad-application-1]

<span data-ttu-id="257ee-122">A **bejelentkezési URL-cím**, adja meg a bejelentkezési URL-CÍMÉT a fejlesztői portálján.</span><span class="sxs-lookup"><span data-stu-id="257ee-122">For **Sign-on URL**, enter the sign-on URL of your developer portal.</span></span> <span data-ttu-id="257ee-123">Ebben a példában a **bejelentkezési URL-cím** van `https://aad03.portal.current.int-azure-api.net/signin`.</span><span class="sxs-lookup"><span data-stu-id="257ee-123">In this example, the **Sign-on URL** is `https://aad03.portal.current.int-azure-api.net/signin`.</span></span> 

<span data-ttu-id="257ee-124">Az a **azonosító URL-címet**, adja meg az alapértelmezett tartomány vagy egyéni tartományhoz az Azure Active Directoryban, és egyedi hozzáfűzése.</span><span class="sxs-lookup"><span data-stu-id="257ee-124">For the **App ID URL**, enter either the default domain or a custom domain for the Azure Active Directory, and append a unique string to it.</span></span> <span data-ttu-id="257ee-125">Ebben a példában az alapértelmezett tartomány **https://contoso5api.onmicrosoft.com** utótagjával rendelkező használt **/api** megadott.</span><span class="sxs-lookup"><span data-stu-id="257ee-125">In this example, the default domain of **https://contoso5api.onmicrosoft.com** is used with the suffix of **/api** specified.</span></span>

![Új Azure Active Directory-alkalmazás tulajdonságai][api-management-new-aad-application-2]

<span data-ttu-id="257ee-127">Kattintson a pipa gombra mentéséhez és az alkalmazás létrehozása, majd átváltása a **konfigurálása** lapján adja meg az új alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="257ee-127">Click the check button to save and create the application, and switch to the **Configure** tab to configure the new application.</span></span>

![Új Azure Active Directory-alkalmazás létrehozása][api-management-new-aad-app-created]

<span data-ttu-id="257ee-129">Ha több aktív Azure-könyvtárak nem használható ehhez az alkalmazáshoz, kattintson a **Igen** a **alkalmazás több-bérlős**.</span><span class="sxs-lookup"><span data-stu-id="257ee-129">If multiple Azure Active Directories are going to be used for this application, click **Yes** for **Application is multi-tenant**.</span></span> <span data-ttu-id="257ee-130">Az alapértelmezett érték **nem**.</span><span class="sxs-lookup"><span data-stu-id="257ee-130">The default is **No**.</span></span>

![Alkalmazás több-bérlős][api-management-aad-app-multi-tenant]

<span data-ttu-id="257ee-132">Másolás a **átirányítási URL-cím** a a **Azure Active Directory** szakasza a **külső identitások** a közzétevő portálon lapra, és illessze be azt a **válasz URL-cím** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="257ee-132">Copy the **Redirect URL** from the **Azure Active Directory** section of the **External Identities** tab in the publisher portal and paste it into the **Reply URL** text box.</span></span> 

![Válasz URL-címe][api-management-aad-reply-url]

<span data-ttu-id="257ee-134">Görgessen az alsó részén a konfigurálása lapon válassza a **Alkalmazásengedélyek** legördülő, és ellenőrizze, **címtáradatok olvasása**.</span><span class="sxs-lookup"><span data-stu-id="257ee-134">Scroll to the bottom of the configure tab, select the **Application Permissions** drop-down, and check **Read directory data**.</span></span>

![Alkalmazásengedélyek][api-management-aad-app-permissions]

<span data-ttu-id="257ee-136">Válassza ki a **engedélyek delegálása** legördülő, és ellenőrizze, **bejelentkezéssel, és olvassa el a felhasználói profilokon**.</span><span class="sxs-lookup"><span data-stu-id="257ee-136">Select the **Delegate Permissions** drop-down, and check **Enable sign-on and read users' profiles**.</span></span>

![Delegált engedélyek][api-management-aad-delegated-permissions]

> <span data-ttu-id="257ee-138">További információ az alkalmazás és a delegált jogosultságokkal sikeresen telepítették: [fér hozzá a Graph API][Accessing the Graph API].</span><span class="sxs-lookup"><span data-stu-id="257ee-138">For more information about application and delegated permissions, see [Accessing the Graph API][Accessing the Graph API].</span></span>
> 
> 

<span data-ttu-id="257ee-139">Másolás a **ügyfél-azonosító** a vágólapra.</span><span class="sxs-lookup"><span data-stu-id="257ee-139">Copy the **Client Id** to the clipboard.</span></span>

![Ügyfél-azonosító][api-management-aad-app-client-id]

<span data-ttu-id="257ee-141">Lépjen vissza a közzétevő portálon, és illessze be a **ügyfél-azonosító** másolta át az Azure Active Directory-alkalmazás konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="257ee-141">Switch back to the publisher portal and paste in the **Client Id** copied from the Azure Active Directory application configuration.</span></span>

![Ügyfél-azonosító][api-management-client-id]

<span data-ttu-id="257ee-143">Váltson vissza az az Azure Active Directory konfigurációját, majd kattintson a **válassza ki a duration** a legördülő a **kulcsok** szakaszt, és adjon meg egy időközt.</span><span class="sxs-lookup"><span data-stu-id="257ee-143">Switch back to the Azure Active Directory configuration, and click the **Select duration** drop-down in the **Keys** section and specify an interval.</span></span> <span data-ttu-id="257ee-144">Ebben a példában **1 év** szolgál.</span><span class="sxs-lookup"><span data-stu-id="257ee-144">In this example, **1 year** is used.</span></span>

![Kulcs][api-management-aad-key-before-save]

<span data-ttu-id="257ee-146">Kattintson a **mentése** a konfiguráció mentéséhez, és a kulcs megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="257ee-146">Click **Save** to save the configuration and display the key.</span></span> <span data-ttu-id="257ee-147">Másolja a vágólapra a kulcsot.</span><span class="sxs-lookup"><span data-stu-id="257ee-147">Copy the key to the clipboard.</span></span>

> <span data-ttu-id="257ee-148">Jegyezze fel ezt a kulcsot.</span><span class="sxs-lookup"><span data-stu-id="257ee-148">Make a note of this key.</span></span> <span data-ttu-id="257ee-149">Az Azure Active Directory konfigurációs ablak bezárása után a kulcs nem jeleníthető meg újra.</span><span class="sxs-lookup"><span data-stu-id="257ee-149">Once you close the Azure Active Directory configuration window, the key cannot be displayed again.</span></span>
> 
> 

![Kulcs][api-management-aad-key-after-save]

<span data-ttu-id="257ee-151">Váltás a közzétevő portal, majd illessze be a kulcs a **Ügyfélkulcs** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="257ee-151">Switch back to the publisher portal and paste the key into the **Client Secret** text box.</span></span>

![Ügyfélkulcs][api-management-client-secret]

<span data-ttu-id="257ee-153">**Bérlők engedélyezett** határozza meg, mely könyvtárak van az API Management service-példány az API-k eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="257ee-153">**Allowed Tenants** specifies which directories have access to the APIs of the API Management service instance.</span></span> <span data-ttu-id="257ee-154">Adja meg a tartomány, amelyhez hozzáférést szeretne az Azure Active Directory-példányok.</span><span class="sxs-lookup"><span data-stu-id="257ee-154">Specify the domains of the Azure Active Directory instances to which you want to grant access.</span></span> <span data-ttu-id="257ee-155">Elkülönítheti a több tartomány ágyazódjanak, szóközzel vagy vesszővel válassza el egymástól.</span><span class="sxs-lookup"><span data-stu-id="257ee-155">You can separate multiple domains with newlines, spaces, or commas.</span></span>

![Engedélyezett bérlők][api-management-client-allowed-tenants]


<span data-ttu-id="257ee-157">Ha a szükséges konfiguráció van megadva, kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="257ee-157">Once the desired configuration is specified, click **Save**.</span></span>

![Mentés][api-management-client-allowed-tenants-save]

<span data-ttu-id="257ee-159">Ha menti a módosításokat, a felhasználók a megadott Azure Active Directoryban bármikor beléphet a fejlesztői portálhoz lépéseit követve [jelentkezzen be egy Azure Active Directory-fiókot a fejlesztői portálra] [ Log in to the Developer portal using an Azure Active Directory account].</span><span class="sxs-lookup"><span data-stu-id="257ee-159">Once the changes are saved, the users in the specified Azure Active Directory can sign in to the Developer portal by following the steps in [Log in to the Developer portal using an Azure Active Directory account][Log in to the Developer portal using an Azure Active Directory account].</span></span>

<span data-ttu-id="257ee-160">Több tartomány is megadhatók a **engedélyezett bérlők** szakasz.</span><span class="sxs-lookup"><span data-stu-id="257ee-160">Multiple domains can be specified in the **Allowed Tenants** section.</span></span> <span data-ttu-id="257ee-161">Előtt minden olyan felhasználó, mint az eredeti tartomány, ahol az alkalmazás regisztrálva lett más tartományokból is bejelentkezhetnek, ugyanaz a tartományi globális rendszergazdájának engedélyeznie kell az alkalmazás hozzáférési címtáradatok.</span><span class="sxs-lookup"><span data-stu-id="257ee-161">Before any user can log in from a different domain than the original domain where the application was registered, a global administrator of the different domain must grant permission for the application to access directory data.</span></span> <span data-ttu-id="257ee-162">Adja meg az engedélyt, a globális rendszergazdának kell lépjen `https://<URL of your developer portal>/aadadminconsent` (például https://contoso.portal.azure-api.net/aadadminconsent), írja be a tartomány nevét annak az Active Directory-bérlő szeretnék a hozzáférést, és kattintson a Küldés gombra.</span><span class="sxs-lookup"><span data-stu-id="257ee-162">To grant permission, the global administrator should go to `https://<URL of your developer portal>/aadadminconsent` (for example, https://contoso.portal.azure-api.net/aadadminconsent), type in the domain name of the Active Directory tenant they want to give access to and click Submit.</span></span> <span data-ttu-id="257ee-163">A következő példában a globális rendszergazdájának `miaoaad.onmicrosoft.com` próbál engedélyt az adott fejlesztői portálra.</span><span class="sxs-lookup"><span data-stu-id="257ee-163">In the following example, a global administrator from `miaoaad.onmicrosoft.com` is trying to give permission to this particular developer portal.</span></span> 

![Engedélyek][api-management-aad-consent]

<span data-ttu-id="257ee-165">A következő képernyőn a globális rendszergazdai jogosultságot ad az engedély megerősítéséhez kéri.</span><span class="sxs-lookup"><span data-stu-id="257ee-165">In the next screen, the global administrator will be prompted to confirm giving the permission.</span></span> 

![Engedélyek][api-management-permissions-form]

> <span data-ttu-id="257ee-167">Ha a nem globális rendszergazda megpróbál bejelentkezni, mielőtt a globális rendszergazda által megadott engedélyekkel, a bejelentkezési kísérlet sikertelen lesz, és egy hiba történt a képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="257ee-167">If a non-global administrator tries to log in before permissions are granted by a global administrator, the login attempt fails and an error screen is displayed.</span></span>
> 
> 

## <a name="how-to-add-an-external-azure-active-directory-group"></a><span data-ttu-id="257ee-168">Egy külső Azure Active Directory-csoport hozzáadása</span><span class="sxs-lookup"><span data-stu-id="257ee-168">How to add an external Azure Active Directory Group</span></span>
<span data-ttu-id="257ee-169">Miután engedélyezte a hozzáférést a felhasználók számára egy Azure Active Directory, Azure Active Directory-csoportok is hozzáadhat, az API Management könnyebben kezelhetik a kívánt termékek a fejlesztők a csoport társítását.</span><span class="sxs-lookup"><span data-stu-id="257ee-169">After enabling access for users in an Azure Active Directory, you can add Azure Active Directory groups into API Management to more easily manage the association of the developers in the group with the desired products.</span></span>

> <span data-ttu-id="257ee-170">Egy külső Azure Active Directory-csoport konfigurálásához, az Azure Active Directory először konfigurálni kell az identitások lapon az alábbi eljárást az előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="257ee-170">To configure an external Azure Active Directory group, the Azure Active Directory must first be configured in the Identities tab by following the procedure in the previous section.</span></span> 
> 
> 

<span data-ttu-id="257ee-171">Külső Active Directory-csoportok hozzáadott a **látható** a terméket, amelynek kíván hozzáférést biztosítson a csoport lapján.</span><span class="sxs-lookup"><span data-stu-id="257ee-171">External Azure Active Directory groups are added from the **Visibility** tab of the product for which you wish to grant access to the group.</span></span> <span data-ttu-id="257ee-172">Kattintson a **termékek**, majd kattintson a kívánt termék nevét.</span><span class="sxs-lookup"><span data-stu-id="257ee-172">Click **Products**, and then click the name of the desired product.</span></span>

![Termék konfigurálása][api-management-configure-product]

<span data-ttu-id="257ee-174">Váltás a **látható** fülre, majd **csoportok hozzáadása az Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="257ee-174">Switch to the **Visibility** tab, and click **Add Groups from Azure Active Directory**.</span></span>

![Csoportok hozzáadása][api-management-add-groups]

<span data-ttu-id="257ee-176">Válassza ki a **Azure Active Directory-bérlő** a legördülő listában, és írja be a kívánt csoport neve a **csoportok** adhatók hozzá a szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="257ee-176">Select the **Azure Active Directory Tenant** from the drop-down list, and then type the name of the desired group in the **Groups** to be added text box.</span></span>

![Válassza ki a csoport][api-management-select-group]

<span data-ttu-id="257ee-178">A csoport neve megtalálható a **csoportok** listában az Azure Active Directory, az alábbi példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="257ee-178">This group name can be found in the **Groups** list for your Azure Active Directory, as shown in the following example.</span></span>

![Az Azure Active Directory-csoportok listája][api-management-aad-groups-list]

<span data-ttu-id="257ee-180">Kattintson a **Hozzáadás** ellenőrzéséhez a csoport nevét, és vegye fel azt a csoportot.</span><span class="sxs-lookup"><span data-stu-id="257ee-180">Click **Add** to validate the group name and add the group.</span></span> <span data-ttu-id="257ee-181">Ebben a példában a **Contoso 5 fejlesztők** külső csoport hozzá van adva.</span><span class="sxs-lookup"><span data-stu-id="257ee-181">In this example, the **Contoso 5 Developers** external group is added.</span></span> 

![Csoport hozzáadva][api-management-aad-group-added]

<span data-ttu-id="257ee-183">Kattintson a **mentése** az új csoport mentéshez.</span><span class="sxs-lookup"><span data-stu-id="257ee-183">Click **Save** to save the new group selection.</span></span>

<span data-ttu-id="257ee-184">Miután egy Azure Active Directory csoport konfigurációja egy terméket, akkor érhető el ellenőrzi a **látható** az API Management szolgáltatáspéldány-egyéb termék a lapon.</span><span class="sxs-lookup"><span data-stu-id="257ee-184">Once an Azure Active Directory group has been configured from one product, it is available to be checked on the **Visibility** tab for the other products in the API Management service instance.</span></span>

<span data-ttu-id="257ee-185">Tekintse át, és azok hozzáadása után állítsa be a külső csoportok tulajdonságait, kattintson a csoport nevét a **csoportok** fülre.</span><span class="sxs-lookup"><span data-stu-id="257ee-185">To review and configure the properties for external groups once they have been added, click the name of the group from the **Groups** tab.</span></span>

![Csoportok kezelése][api-management-groups]

<span data-ttu-id="257ee-187">Itt szerkesztheti a **neve** és a **leírás** a csoport.</span><span class="sxs-lookup"><span data-stu-id="257ee-187">From here you can edit the **Name** and the **Description** of the group.</span></span>

![Csoport szerkesztése][api-management-edit-group]

<span data-ttu-id="257ee-189">A konfigurált Azure Active Directory felhasználók jelentkezzen be a fejlesztői portálján, tekintse meg, és minden olyan csoportot, amelyeknél látható a következő szakaszban található utasításokat követve előfizetni.</span><span class="sxs-lookup"><span data-stu-id="257ee-189">Users from the configured Azure Active Directory can sign in to the Developer portal and view and subscribe to any groups for which they have visibility by following the instructions in the following section.</span></span>

## <a name="how-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a><span data-ttu-id="257ee-190">A fejlesztői portálra egy Azure Active Directory-fiókot bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="257ee-190">How to log in to the Developer portal using an Azure Active Directory account</span></span>
<span data-ttu-id="257ee-191">A fejlesztői portálra a korábbi szakaszokban konfigurált Azure Active Directory-fiókkal bejelentkezni, nyisson meg egy új böngészőt ablak használatával a **bejelentkezési URL-cím** az Active Directory-alkalmazás konfigurációból kattintson**Az azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="257ee-191">To log into the Developer portal using an Azure Active Directory account configured in the previous sections, open a new browser window using the **Sign-on URL** from the Active Directory application configuration, and click **Azure Active Directory**.</span></span>

![Fejlesztői portálján][api-management-dev-portal-signin]

<span data-ttu-id="257ee-193">Adja meg az egyik felhasználó hitelesítő adatait az Azure Active Directoryban, és kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="257ee-193">Enter the credentials of one of the users in your Azure Active Directory, and click **Sign in**.</span></span>

![Bejelentkezés][api-management-aad-signin]

<span data-ttu-id="257ee-195">Kérheti a regisztrációs űrlap Ha bármilyen további információkra szükség.</span><span class="sxs-lookup"><span data-stu-id="257ee-195">You may be prompted with a registration form if any additional information is required.</span></span> <span data-ttu-id="257ee-196">Végezze el a regisztrációs képernyő, és kattintson a **regisztráljon**.</span><span class="sxs-lookup"><span data-stu-id="257ee-196">Complete the registration form and click **Sign up**.</span></span>

![Regisztráció][api-management-complete-registration]

<span data-ttu-id="257ee-198">A felhasználó most jelentkezett be a fejlesztői portálra, az API Management szolgáltatáspéldány.</span><span class="sxs-lookup"><span data-stu-id="257ee-198">Your user is now logged into the developer portal for your API Management service instance.</span></span>

![Regisztráció kész][api-management-registration-complete]

[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-security-external-identities.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-aad-consent]: ./media/api-management-howto-aad/api-management-aad-consent.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing the Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Log in to the Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account

