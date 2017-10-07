---
title: "aaaAuthorize fejlesztői fiókok Azure Active Directory - Azure API Management használatával |} Microsoft Docs"
description: "Ismerje meg, hogy az Azure Active Directoryval az API Management tooauthorize felhasználók."
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
ms.openlocfilehash: ebf5447a509a47df35e4262138bfcf423cb1dd5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooauthorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a><span data-ttu-id="6b67b-103">Hogyan tooauthorize fejlesztői fiókok Azure Active Directory, az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="6b67b-103">How tooauthorize developer accounts using Azure Active Directory in Azure API Management</span></span>
## <a name="overview"></a><span data-ttu-id="6b67b-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="6b67b-104">Overview</span></span>
<span data-ttu-id="6b67b-105">Ez az útmutató bemutatja, hogyan tooenable elérni az Azure Active Directory toohello developer portálon a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="6b67b-105">This guide shows you how tooenable access toohello developer portal for users from Azure Active Directory.</span></span> <span data-ttu-id="6b67b-106">Ez az útmutató azt is bemutatja, hogyan toomanage csoportok az Azure Active Directory-felhasználók külső tartalmazó csoportok hozzáadásával hello egy Azure Active Directory felhasználók.</span><span class="sxs-lookup"><span data-stu-id="6b67b-106">This guide also shows you how toomanage groups of Azure Active Directory users by adding external groups that contain hello users of an Azure Active Directory.</span></span>

> <span data-ttu-id="6b67b-107">a jelen útmutató lépéseit toocomplete hello kell egy Azure Active Directory mely toocreate a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="6b67b-107">toocomplete hello steps in this guide you must first have an Azure Active Directory in which toocreate an application.</span></span>
> 
> 

## <a name="how-tooauthorize-developer-accounts-using-azure-active-directory"></a><span data-ttu-id="6b67b-108">Hogyan tooauthorize fejlesztői fiókok Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="6b67b-108">How tooauthorize developer accounts using Azure Active Directory</span></span>
<span data-ttu-id="6b67b-109">tooget elindítani, kattintson a **Publisher portal** a hello Azure-portálon az API Management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6b67b-109">tooget started, click **Publisher portal** in hello Azure portal for your API Management service.</span></span> <span data-ttu-id="6b67b-110">Ezzel megnyitná toohello API Management publisher portálon.</span><span class="sxs-lookup"><span data-stu-id="6b67b-110">This takes you toohello API Management publisher portal.</span></span>

![Közzétevő portál][api-management-management-console]

> <span data-ttu-id="6b67b-112">Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="6b67b-112">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="6b67b-113">Kattintson a **biztonsági** a hello **API Management** elemre, majd hello bal oldali menü **külső identitások**.</span><span class="sxs-lookup"><span data-stu-id="6b67b-113">Click **Security** from hello **API Management** menu on hello left and click **External Identities**.</span></span>

![Külső identitások][api-management-security-external-identities]

<span data-ttu-id="6b67b-115">Kattintson a **az Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6b67b-115">Click **Azure Active Directory**.</span></span> <span data-ttu-id="6b67b-116">Jegyezze fel a hello **átirányítási URL-cím** és a klasszikus Azure portál hello Azure Active Directory tooyour keresztül.</span><span class="sxs-lookup"><span data-stu-id="6b67b-116">Make a note of hello **Redirect URL** and switch over tooyour Azure Active Directory in hello Azure Classic Portal.</span></span>

![Külső identitások][api-management-security-aad-new]

<span data-ttu-id="6b67b-118">Kattintson a hello **Hozzáadás** toocreate egy új Azure Active Directory-alkalmazás gombra, és válassza a **a szerveztem által fejlesztett alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="6b67b-118">Click hello **Add** button toocreate a new Azure Active Directory application, and choose **Add an application my organization is developing**.</span></span>

![Új Azure Active Directory-alkalmazás hozzáadása][api-management-new-aad-application-menu]

<span data-ttu-id="6b67b-120">Adjon meg egy nevet hello alkalmazás, jelölje be **webes alkalmazáshoz és/vagy webes API**, majd hello Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="6b67b-120">Enter a name for hello application, select **Web application and/or Web API**, and click hello next button.</span></span>

![Új Azure Active Directory-alkalmazás][api-management-new-aad-application-1]

<span data-ttu-id="6b67b-122">A **bejelentkezési URL-cím**, adja meg a hello bejelentkezési URL-CÍMÉT a fejlesztői portálján.</span><span class="sxs-lookup"><span data-stu-id="6b67b-122">For **Sign-on URL**, enter hello sign-on URL of your developer portal.</span></span> <span data-ttu-id="6b67b-123">Ebben a példában hello **bejelentkezési URL-cím** van `https://aad03.portal.current.int-azure-api.net/signin`.</span><span class="sxs-lookup"><span data-stu-id="6b67b-123">In this example, hello **Sign-on URL** is `https://aad03.portal.current.int-azure-api.net/signin`.</span></span> 

<span data-ttu-id="6b67b-124">A hello **azonosító URL-címet**, adja meg vagy hello alapértelmezett vagy egyéni tartomány hello Azure Active Directoryban, és egy egyedi karakterlánc tooit hozzáfűzése.</span><span class="sxs-lookup"><span data-stu-id="6b67b-124">For hello **App ID URL**, enter either hello default domain or a custom domain for hello Azure Active Directory, and append a unique string tooit.</span></span> <span data-ttu-id="6b67b-125">Ebben a példában az alapértelmezett tartomány hello **https://contoso5api.onmicrosoft.com** hello utótagja használt **/api** megadott.</span><span class="sxs-lookup"><span data-stu-id="6b67b-125">In this example, hello default domain of **https://contoso5api.onmicrosoft.com** is used with hello suffix of **/api** specified.</span></span>

![Új Azure Active Directory-alkalmazás tulajdonságai][api-management-new-aad-application-2]

<span data-ttu-id="6b67b-127">Hello ellenőrzése gombra toosave kattintson hello alkalmazás létrehozása és toohello kapcsoló **konfigurálása** tooconfigure hello új alkalmazás lapon.</span><span class="sxs-lookup"><span data-stu-id="6b67b-127">Click hello check button toosave and create hello application, and switch toohello **Configure** tab tooconfigure hello new application.</span></span>

![Új Azure Active Directory-alkalmazás létrehozása][api-management-new-aad-app-created]

<span data-ttu-id="6b67b-129">Ha több aktív Azure-könyvtárak fog használni az alkalmazás toobe, kattintson a **Igen** a **alkalmazás több-bérlős**.</span><span class="sxs-lookup"><span data-stu-id="6b67b-129">If multiple Azure Active Directories are going toobe used for this application, click **Yes** for **Application is multi-tenant**.</span></span> <span data-ttu-id="6b67b-130">hello alapértelmezett érték a **nem**.</span><span class="sxs-lookup"><span data-stu-id="6b67b-130">hello default is **No**.</span></span>

![Alkalmazás több-bérlős][api-management-aad-app-multi-tenant]

<span data-ttu-id="6b67b-132">Másolás hello **átirányítási URL-cím** a hello **Azure Active Directory** hello szakasza **külső identitások** hello publisher portálon lapra, majd illessze be hello **Válasz URL-CÍMEN** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="6b67b-132">Copy hello **Redirect URL** from hello **Azure Active Directory** section of hello **External Identities** tab in hello publisher portal and paste it into hello **Reply URL** text box.</span></span> 

![Válasz URL-címe][api-management-aad-reply-url]

<span data-ttu-id="6b67b-134">Görgessen toohello alsó részén hello konfigurálása lapon válassza hello **Alkalmazásengedélyek** legördülő, és ellenőrizze **címtáradatok olvasása**.</span><span class="sxs-lookup"><span data-stu-id="6b67b-134">Scroll toohello bottom of hello configure tab, select hello **Application Permissions** drop-down, and check **Read directory data**.</span></span>

![Alkalmazásengedélyek][api-management-aad-app-permissions]

<span data-ttu-id="6b67b-136">Jelölje be hello **delegált engedélyek** legördülő menüben, és ellenőrizze, **bejelentkezéssel, és olvassa el a felhasználói profilokon**.</span><span class="sxs-lookup"><span data-stu-id="6b67b-136">Select hello **Delegate Permissions** drop-down, and check **Enable sign-on and read users' profiles**.</span></span>

![Delegált engedélyek][api-management-aad-delegated-permissions]

> <span data-ttu-id="6b67b-138">További információ az alkalmazás és a delegált jogosultságokkal sikeresen telepítették: [elérése hello Graph API][Accessing hello Graph API].</span><span class="sxs-lookup"><span data-stu-id="6b67b-138">For more information about application and delegated permissions, see [Accessing hello Graph API][Accessing hello Graph API].</span></span>
> 
> 

<span data-ttu-id="6b67b-139">Másolás hello **ügyfél-azonosító** toohello vágólapra.</span><span class="sxs-lookup"><span data-stu-id="6b67b-139">Copy hello **Client Id** toohello clipboard.</span></span>

![Ügyfél-azonosító][api-management-aad-app-client-id]

<span data-ttu-id="6b67b-141">Váltson vissza toohello publisher portálon, és illessze be a hello **ügyfél-azonosító** hello Azure Active Directory-alkalmazás konfigurációja átmásolva.</span><span class="sxs-lookup"><span data-stu-id="6b67b-141">Switch back toohello publisher portal and paste in hello **Client Id** copied from hello Azure Active Directory application configuration.</span></span>

![Ügyfél-azonosító][api-management-client-id]

<span data-ttu-id="6b67b-143">Váltson vissza toohello Azure Active Directory konfigurációját, majd kattintson a hello **válassza ki a duration** hello legördülő **kulcsok** szakaszt, és adjon meg egy időközt.</span><span class="sxs-lookup"><span data-stu-id="6b67b-143">Switch back toohello Azure Active Directory configuration, and click hello **Select duration** drop-down in hello **Keys** section and specify an interval.</span></span> <span data-ttu-id="6b67b-144">Ebben a példában **1 év** szolgál.</span><span class="sxs-lookup"><span data-stu-id="6b67b-144">In this example, **1 year** is used.</span></span>

![Kulcs][api-management-aad-key-before-save]

<span data-ttu-id="6b67b-146">Kattintson a **mentése** toosave hello konfigurációs és megjelenítési hello kulcs.</span><span class="sxs-lookup"><span data-stu-id="6b67b-146">Click **Save** toosave hello configuration and display hello key.</span></span> <span data-ttu-id="6b67b-147">Hello kulcs toohello vágólapra másolása.</span><span class="sxs-lookup"><span data-stu-id="6b67b-147">Copy hello key toohello clipboard.</span></span>

> <span data-ttu-id="6b67b-148">Jegyezze fel ezt a kulcsot.</span><span class="sxs-lookup"><span data-stu-id="6b67b-148">Make a note of this key.</span></span> <span data-ttu-id="6b67b-149">Hello Azure Active Directory konfigurációs ablak bezárása után hello kulcs nem jeleníthető meg újra.</span><span class="sxs-lookup"><span data-stu-id="6b67b-149">Once you close hello Azure Active Directory configuration window, hello key cannot be displayed again.</span></span>
> 
> 

![Kulcs][api-management-aad-key-after-save]

<span data-ttu-id="6b67b-151">Kapcsoló hátsó toohello publisher portál és a Beillesztés hello kulcs be hello **Ügyfélkulcs** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="6b67b-151">Switch back toohello publisher portal and paste hello key into hello **Client Secret** text box.</span></span>

![Ügyfélkulcs][api-management-client-secret]

<span data-ttu-id="6b67b-153">**Bérlők engedélyezett** határozza meg, mely könyvtárak rendelkezik hozzáférési toohello API-k hello API Management service-példány.</span><span class="sxs-lookup"><span data-stu-id="6b67b-153">**Allowed Tenants** specifies which directories have access toohello APIs of hello API Management service instance.</span></span> <span data-ttu-id="6b67b-154">Az Azure Active Directory-példányok toowhich szeretne hozzáférni toogrant hello hello tartományok megadása.</span><span class="sxs-lookup"><span data-stu-id="6b67b-154">Specify hello domains of hello Azure Active Directory instances toowhich you want toogrant access.</span></span> <span data-ttu-id="6b67b-155">Elkülönítheti a több tartomány ágyazódjanak, szóközzel vagy vesszővel válassza el egymástól.</span><span class="sxs-lookup"><span data-stu-id="6b67b-155">You can separate multiple domains with newlines, spaces, or commas.</span></span>

![Engedélyezett bérlők][api-management-client-allowed-tenants]


<span data-ttu-id="6b67b-157">Amikor hello szükségeskonfiguráció-konfigurációt ad meg, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="6b67b-157">Once hello desired configuration is specified, click **Save**.</span></span>

![Mentés][api-management-client-allowed-tenants-save]

<span data-ttu-id="6b67b-159">Miután hello módosítások mentésekor hello hello felhasználók megadott Azure Active Directory bejelentkezés toohello fejlesztői portálján hello utasításait követve [toohello fejlesztői portálján egy Azure Active Directory-fiókot használ-e jelentkezni] [Log in toohello Developer portal using an Azure Active Directory account].</span><span class="sxs-lookup"><span data-stu-id="6b67b-159">Once hello changes are saved, hello users in hello specified Azure Active Directory can sign in toohello Developer portal by following hello steps in [Log in toohello Developer portal using an Azure Active Directory account][Log in toohello Developer portal using an Azure Active Directory account].</span></span>

<span data-ttu-id="6b67b-160">Több tartomány is megadhatók hello **engedélyezett bérlők** szakasz.</span><span class="sxs-lookup"><span data-stu-id="6b67b-160">Multiple domains can be specified in hello **Allowed Tenants** section.</span></span> <span data-ttu-id="6b67b-161">Mielőtt bármely felhasználó is jelentkezik be egy tartományból hello eredeti ahol hello alkalmazás lett regisztrálva, hello ugyanaz a tartományi globális rendszergazdájának kell engedélyt hello alkalmazás tooaccess címtáradatok.</span><span class="sxs-lookup"><span data-stu-id="6b67b-161">Before any user can log in from a different domain than hello original domain where hello application was registered, a global administrator of hello different domain must grant permission for hello application tooaccess directory data.</span></span> <span data-ttu-id="6b67b-162">toogrant engedély hello globális rendszergazda léphetik túl`https://<URL of your developer portal>/aadadminconsent` (például https://contoso.portal.azure-api.net/aadadminconsent), hello toogive hozzáférés tooand kívánják Active Directory-bérlő hello tartománynevet írja be a Küldés gombra.</span><span class="sxs-lookup"><span data-stu-id="6b67b-162">toogrant permission, hello global administrator should go too`https://<URL of your developer portal>/aadadminconsent` (for example, https://contoso.portal.azure-api.net/aadadminconsent), type in hello domain name of hello Active Directory tenant they want toogive access tooand click Submit.</span></span> <span data-ttu-id="6b67b-163">Hello alábbi példa, a globális rendszergazdájának `miaoaad.onmicrosoft.com` próbál toogive engedély toothis adott fejlesztői portálján.</span><span class="sxs-lookup"><span data-stu-id="6b67b-163">In hello following example, a global administrator from `miaoaad.onmicrosoft.com` is trying toogive permission toothis particular developer portal.</span></span> 

![Engedélyek][api-management-aad-consent]

<span data-ttu-id="6b67b-165">Hello következő képernyőn hello globális rendszergazdája lesz felszólító tooconfirm hello engedélyt adjon.</span><span class="sxs-lookup"><span data-stu-id="6b67b-165">In hello next screen, hello global administrator will be prompted tooconfirm giving hello permission.</span></span> 

![Engedélyek][api-management-permissions-form]

> <span data-ttu-id="6b67b-167">Ha nem globális rendszergazda kísérli meg egy globális rendszergazda által nyújtott toolog az engedélyek előtt, hello bejelentkezési kísérlet sikertelen lesz, és egy hiba történt a képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6b67b-167">If a non-global administrator tries toolog in before permissions are granted by a global administrator, hello login attempt fails and an error screen is displayed.</span></span>
> 
> 

## <a name="how-tooadd-an-external-azure-active-directory-group"></a><span data-ttu-id="6b67b-168">Hogyan tooadd egy külső az Azure Active Directory csoport</span><span class="sxs-lookup"><span data-stu-id="6b67b-168">How tooadd an external Azure Active Directory Group</span></span>
<span data-ttu-id="6b67b-169">Miután engedélyezte a hozzáférést a felhasználók számára egy Azure Active Directory, adhat hozzá az API Management toomore Active Directory-csoportok, könnyedén kezelhető szükséges hello termékekkel hello hello fejlesztők hello csoport hozzárendelését.</span><span class="sxs-lookup"><span data-stu-id="6b67b-169">After enabling access for users in an Azure Active Directory, you can add Azure Active Directory groups into API Management toomore easily manage hello association of hello developers in hello group with hello desired products.</span></span>

> <span data-ttu-id="6b67b-170">egy külső Azure Active Directory-csoportot, hello Azure Active Directory tooconfigure először konfigurálni kell hello identitások lapon hello előző szakaszban hello eljárást követve.</span><span class="sxs-lookup"><span data-stu-id="6b67b-170">tooconfigure an external Azure Active Directory group, hello Azure Active Directory must first be configured in hello Identities tab by following hello procedure in hello previous section.</span></span> 
> 
> 

<span data-ttu-id="6b67b-171">Külső Active Directory-csoportok hozzáadott hello **látható** , amelynek toogrant hozzáférési toohello csoport kívánja hello termék fülre.</span><span class="sxs-lookup"><span data-stu-id="6b67b-171">External Azure Active Directory groups are added from hello **Visibility** tab of hello product for which you wish toogrant access toohello group.</span></span> <span data-ttu-id="6b67b-172">Kattintson a **termékek**, majd kattintson a kívánt termék hello hello nevét.</span><span class="sxs-lookup"><span data-stu-id="6b67b-172">Click **Products**, and then click hello name of hello desired product.</span></span>

![Termék konfigurálása][api-management-configure-product]

<span data-ttu-id="6b67b-174">Váltás toohello **látható** fülre, majd **csoportok hozzáadása az Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6b67b-174">Switch toohello **Visibility** tab, and click **Add Groups from Azure Active Directory**.</span></span>

![Csoportok hozzáadása][api-management-add-groups]

<span data-ttu-id="6b67b-176">Jelölje be hello **Azure Active Directory-bérlő** hello legördülő listából, és majd hello típusnév hello kívánt csoport hello **csoportok** toobe hozzáadott szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="6b67b-176">Select hello **Azure Active Directory Tenant** from hello drop-down list, and then type hello name of hello desired group in hello **Groups** toobe added text box.</span></span>

![Válassza ki a csoport][api-management-select-group]

<span data-ttu-id="6b67b-178">A csoport neve megtalálható hello **csoportok** az Azure Active Directoryban, ahogy az alábbi példa hello listájában.</span><span class="sxs-lookup"><span data-stu-id="6b67b-178">This group name can be found in hello **Groups** list for your Azure Active Directory, as shown in hello following example.</span></span>

![Az Azure Active Directory-csoportok listája][api-management-aad-groups-list]

<span data-ttu-id="6b67b-180">Kattintson a **Hozzáadás** toovalidate hello csoport nevét, majd adja hozzá hello csoportot.</span><span class="sxs-lookup"><span data-stu-id="6b67b-180">Click **Add** toovalidate hello group name and add hello group.</span></span> <span data-ttu-id="6b67b-181">Ebben a példában hello **Contoso 5 fejlesztők** külső csoport hozzá van adva.</span><span class="sxs-lookup"><span data-stu-id="6b67b-181">In this example, hello **Contoso 5 Developers** external group is added.</span></span> 

![Csoport hozzáadva][api-management-aad-group-added]

<span data-ttu-id="6b67b-183">Kattintson a **mentése** toosave hello új csoport kijelölése.</span><span class="sxs-lookup"><span data-stu-id="6b67b-183">Click **Save** toosave hello new group selection.</span></span>

<span data-ttu-id="6b67b-184">Egy Azure Active Directory csoport konfigurációja egy terméket, ha már be van jelölve, a hello elérhető toobe **látható** lapján más termékek hello hello API Management service-példányban.</span><span class="sxs-lookup"><span data-stu-id="6b67b-184">Once an Azure Active Directory group has been configured from one product, it is available toobe checked on hello **Visibility** tab for hello other products in hello API Management service instance.</span></span>

<span data-ttu-id="6b67b-185">tooreview és konfigurálni hello tulajdonságait külső csoportot azok hozzáadott, kattintson a hello hello csoport hello neve **csoportok** fülre.</span><span class="sxs-lookup"><span data-stu-id="6b67b-185">tooreview and configure hello properties for external groups once they have been added, click hello name of hello group from hello **Groups** tab.</span></span>

![Csoportok kezelése][api-management-groups]

<span data-ttu-id="6b67b-187">Itt szerkesztheti hello **neve** és hello **leírás** hello csoport.</span><span class="sxs-lookup"><span data-stu-id="6b67b-187">From here you can edit hello **Name** and hello **Description** of hello group.</span></span>

![Csoport szerkesztése][api-management-edit-group]

<span data-ttu-id="6b67b-189">Hello felhasználók konfigurált Azure Active Directory toohello fejlesztői portálján és nézet bejelentkezhet és előfizetés tooany csoportok amelyeknél látható a következő szakasz hello hello utasításait követve.</span><span class="sxs-lookup"><span data-stu-id="6b67b-189">Users from hello configured Azure Active Directory can sign in toohello Developer portal and view and subscribe tooany groups for which they have visibility by following hello instructions in hello following section.</span></span>

## <a name="how-toolog-in-toohello-developer-portal-using-an-azure-active-directory-account"></a><span data-ttu-id="6b67b-190">Hogyan toolog toohello Developer portálon az Azure Active Directory-fiókkal</span><span class="sxs-lookup"><span data-stu-id="6b67b-190">How toolog in toohello Developer portal using an Azure Active Directory account</span></span>
<span data-ttu-id="6b67b-191">hello fejlesztői portálra hello korábbi szakaszokban, konfigurált Azure Active Directory-fiókkal történő toolog nyisson meg egy új böngészőablakot, hello segítségével **bejelentkezési URL-cím** hello Active Directory-alkalmazás konfigurációját, majd kattintson a **Az azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6b67b-191">toolog into hello Developer portal using an Azure Active Directory account configured in hello previous sections, open a new browser window using hello **Sign-on URL** from hello Active Directory application configuration, and click **Azure Active Directory**.</span></span>

![Fejlesztői portálján][api-management-dev-portal-signin]

<span data-ttu-id="6b67b-193">Adja meg a hello hitelesítő adatokat az egyik hello felhasználók az Azure Active Directoryban, majd kattintson **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6b67b-193">Enter hello credentials of one of hello users in your Azure Active Directory, and click **Sign in**.</span></span>

![Bejelentkezés][api-management-aad-signin]

<span data-ttu-id="6b67b-195">Kérheti a regisztrációs űrlap Ha bármilyen további információkra szükség.</span><span class="sxs-lookup"><span data-stu-id="6b67b-195">You may be prompted with a registration form if any additional information is required.</span></span> <span data-ttu-id="6b67b-196">Hello regisztrációs űrlap kitöltése és kattintson a **regisztráljon**.</span><span class="sxs-lookup"><span data-stu-id="6b67b-196">Complete hello registration form and click **Sign up**.</span></span>

![Regisztráció][api-management-complete-registration]

<span data-ttu-id="6b67b-198">A felhasználók most már be legyen jelentkezve az API Management szolgáltatáspéldány hello fejlesztői portálján.</span><span class="sxs-lookup"><span data-stu-id="6b67b-198">Your user is now logged into hello developer portal for your API Management service instance.</span></span>

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

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How tooadd and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs tooa product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing hello Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API toouse OAuth 2.0 user authorization]: #step2
[Test hello OAuth 2.0 user authorization in hello Developer Portal]: #step3
[Next steps]: #next-steps

[Log in toohello Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account

