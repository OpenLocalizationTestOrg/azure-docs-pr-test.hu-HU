---
title: "Az Azure Active Directoryt hitelesítéséhez az Azure Batch szolgáltatás megoldások |} Microsoft Docs"
description: "Kötegelt az Azure AD a Batch szolgáltatás hitelesítést támogatja."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/20/2017
ms.author: tamram
ms.openlocfilehash: 9c03bde919c46cd301229255c0b12ee69dda6f78
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="authenticate-batch-service-solutions-with-active-directory"></a><span data-ttu-id="581b4-103">Kötegelt szolgáltatási megoldások és az Active Directory hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="581b4-103">Authenticate Batch service solutions with Active Directory</span></span>

<span data-ttu-id="581b4-104">Az Azure Batch támogatja a hitelesítési [Azure Active Directory] [ aad_about] (az Azure AD).</span><span class="sxs-lookup"><span data-stu-id="581b4-104">Azure Batch supports authentication with [Azure Active Directory][aad_about] (Azure AD).</span></span> <span data-ttu-id="581b4-105">Az Azure AD a Microsoft több-bérlős felhőalapú címtár és az identity management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="581b4-105">Azure AD is Microsoft’s multi-tenant cloud based directory and identity management service.</span></span> <span data-ttu-id="581b4-106">Azure magát a felhasználók, a szolgáltatás-rendszergazdák és a szervezeti felhasználók hitelesítéséhez használja az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="581b4-106">Azure itself uses Azure AD to authenticate its customers, service administrators, and organizational users.</span></span>

<span data-ttu-id="581b4-107">Az Azure AD-alapú hitelesítés használata az Azure Batch, az alábbi két módszer egyikével hitelesítheti:</span><span class="sxs-lookup"><span data-stu-id="581b4-107">When using Azure AD authentication with Azure Batch, you can authenticate in one of two ways:</span></span>

- <span data-ttu-id="581b4-108">A **integrált hitelesítés** , amely az alkalmazás dolgozik a felhasználó hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="581b4-108">By using **integrated authentication** to authenticate a user that is interacting with the application.</span></span> <span data-ttu-id="581b4-109">Integrált hitelesítést használó alkalmazások a felhasználói hitelesítő adatokat gyűjt, és ezeket a hitelesítő adatokat használ kötegelt erőforrásokhoz való hozzáférés hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="581b4-109">An application using integrated authentication gathers a user's credentials and uses those credentials to authenticate access to Batch resources.</span></span>
- <span data-ttu-id="581b4-110">Használatával egy **egyszerű** egy felügyelet nélküli alkalmazás hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="581b4-110">By using a **service principal** to authenticate an unattended application.</span></span> <span data-ttu-id="581b4-111">Egy egyszerű szolgáltatást határozza meg a házirend és megadásával az alkalmazás ahhoz, hogy az alkalmazás futásidőben erőforrásokhoz való hozzáféréskor.</span><span class="sxs-lookup"><span data-stu-id="581b4-111">A service principal defines the policy and permissions for an application in order to represent the application when accessing resources at runtime.</span></span>

<span data-ttu-id="581b4-112">Az Azure AD kapcsolatos további tudnivalókért tekintse meg a [Azure Active Directory dokumentációjának](https://docs.microsoft.com/azure/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="581b4-112">To learn more about Azure AD, see the [Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span>

## <a name="authentication-and-pool-allocation-mode"></a><span data-ttu-id="581b4-113">Hitelesítés és a tárolókészlet foglalási mód</span><span class="sxs-lookup"><span data-stu-id="581b4-113">Authentication and pool allocation mode</span></span>

<span data-ttu-id="581b4-114">Batch-fiók létrehozásakor megadhatja, ahol fiók létrehozott készleteket kell kiosztani.</span><span class="sxs-lookup"><span data-stu-id="581b4-114">When you create a Batch account, you can specify where pools created for that account should be allocated.</span></span> <span data-ttu-id="581b4-115">Ha szeretné foglaljon le címkészleteket a alapértelmezett Batch szolgáltatás előfizetésével vagy egy felhasználó az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="581b4-115">You can choose to allocate pools either in the default Batch service subscription or in a user subscription.</span></span> <span data-ttu-id="581b4-116">A kiválasztott érinti Önt a hitelesítéshez, hogyan fiókhoz tartozó erőforrásokhoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="581b4-116">Your choice affects how you authenticate access to resources in that account.</span></span>

- <span data-ttu-id="581b4-117">**A Batch-szolgáltatás előfizetésének**.</span><span class="sxs-lookup"><span data-stu-id="581b4-117">**Batch service subscription**.</span></span> <span data-ttu-id="581b4-118">Alapértelmezés szerint a Batch szolgáltatás előfizetés kötegelt készletek foglal le.</span><span class="sxs-lookup"><span data-stu-id="581b4-118">By default, Batch pools are allocated in a Batch service subscription.</span></span> <span data-ttu-id="581b4-119">Ha ezzel a beállítással hitelesítheti fiókhoz tartozó erőforrások elérésére vagy [megosztott kulcsos](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service) vagy az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="581b4-119">If you choose this option, you can authenticate access to resources in that account either with [Shared Key](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service) or with Azure AD.</span></span>
- <span data-ttu-id="581b4-120">**Felhasználó-előfizetés.**</span><span class="sxs-lookup"><span data-stu-id="581b4-120">**User subscription.**</span></span> <span data-ttu-id="581b4-121">Kiválaszthatja, hogy egy felhasználó az előfizetéshez megadott kötegelt készletek lefoglalni.</span><span class="sxs-lookup"><span data-stu-id="581b4-121">You can choose to allocate Batch pools in a user subscription that you specify.</span></span> <span data-ttu-id="581b4-122">Ha ezt a lehetőséget választja, hitelesítenie kell az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="581b4-122">If you choose this option, you must authenticate with Azure AD.</span></span>

## <a name="endpoints-for-authentication"></a><span data-ttu-id="581b4-123">A hitelesítési végpontot</span><span class="sxs-lookup"><span data-stu-id="581b4-123">Endpoints for authentication</span></span>

<span data-ttu-id="581b4-124">Batch-alkalmazások az Azure AD hitelesíti, akkor is kell néhány jól ismert végpontok a kódban.</span><span class="sxs-lookup"><span data-stu-id="581b4-124">To authenticate Batch applications with Azure AD, you need to include some well-known endpoints in your code.</span></span>

### <a name="azure-ad-endpoint"></a><span data-ttu-id="581b4-125">Azure AD-végpont</span><span class="sxs-lookup"><span data-stu-id="581b4-125">Azure AD endpoint</span></span>

<span data-ttu-id="581b4-126">A következő szolgáltató az Azure AD-végpont esetében:</span><span class="sxs-lookup"><span data-stu-id="581b4-126">The base Azure AD authority endpoint is:</span></span>

`https://login.microsoftonline.com/`

<span data-ttu-id="581b4-127">Hitelesítés az Azure ad-vel, ehhez a végponthoz, a bérlő azonosítója (directory ID) együtt használja.</span><span class="sxs-lookup"><span data-stu-id="581b4-127">To authenticate with Azure AD, you use this endpoint together with the tenant ID (directory ID).</span></span> <span data-ttu-id="581b4-128">A bérlő azonosítója az Azure AD-bérlő a hitelesítéshez használandó azonosítja.</span><span class="sxs-lookup"><span data-stu-id="581b4-128">The tenant ID identifies the Azure AD tenant to use for authentication.</span></span> <span data-ttu-id="581b4-129">Bérlői azonosító lekéréséhez kövesse a témakörben ismertetett lépéseket [bérlői azonosító lekérése az Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span><span class="sxs-lookup"><span data-stu-id="581b4-129">To retrieve the tenant ID, follow the steps outlined in [Get the tenant ID for your Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span></span>

`https://login.microsoftonline.com/<tenant-id>`

> [!NOTE] 
> <span data-ttu-id="581b4-130">A bérlő-specifikus végpont a hitelesítést egy egyszerű szolgáltatás használata esetén szükséges.</span><span class="sxs-lookup"><span data-stu-id="581b4-130">The tenant-specific endpoint is required when you authenticate using a service principal.</span></span> 
> 
> <span data-ttu-id="581b4-131">A bérlő-specifikus-végpont esetében az Önt a hitelesítéshez integrált hitelesítés használata kötelező, de ajánlott.</span><span class="sxs-lookup"><span data-stu-id="581b4-131">The tenant-specific endpoint is optional when you authenticate using integrated authentication, but recommended.</span></span> <span data-ttu-id="581b4-132">Azonban is használhatja az Azure AD közös végpontot.</span><span class="sxs-lookup"><span data-stu-id="581b4-132">However, you can also use the Azure AD common endpoint.</span></span> <span data-ttu-id="581b4-133">A közös végpont összegyűjtéséhez felületet, ha nincs megadva egy adott bérlő általános hitelesítő adatokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="581b4-133">The common endpoint provides a generic credential gathering interface when a specific tenant is not provided.</span></span> <span data-ttu-id="581b4-134">A közös végpont `https://login.microsoftonline.com/common`.</span><span class="sxs-lookup"><span data-stu-id="581b4-134">The common endpoint is `https://login.microsoftonline.com/common`.</span></span>
>
>

<span data-ttu-id="581b4-135">Az Azure AD-végpontok kapcsolatos további információkért lásd: [hitelesítési forgatókönyvek az Azure AD][aad_auth_scenarios].</span><span class="sxs-lookup"><span data-stu-id="581b4-135">For more information about Azure AD endpoints, see [Authentication Scenarios for Azure AD][aad_auth_scenarios].</span></span>

### <a name="batch-resource-endpoint"></a><span data-ttu-id="581b4-136">Kötegelt erőforrás végpont</span><span class="sxs-lookup"><span data-stu-id="581b4-136">Batch resource endpoint</span></span>

<span data-ttu-id="581b4-137">Használja a **Azure Batch erőforrás végpont** a Batch szolgáltatás kérelmek hitelesítéséhez jogkivonat beszerzése:</span><span class="sxs-lookup"><span data-stu-id="581b4-137">Use the **Azure Batch resource endpoint** to acquire a token for authenticating requests to the Batch service:</span></span>

`https://batch.core.windows.net/`

## <a name="register-your-application-with-a-tenant"></a><span data-ttu-id="581b4-138">Az alkalmazás regisztrálása a bérlő</span><span class="sxs-lookup"><span data-stu-id="581b4-138">Register your application with a tenant</span></span>

<span data-ttu-id="581b4-139">Az első lépés az Azure AD hitelesítéséhez van az alkalmazás regisztrálása az Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="581b4-139">The first step in using Azure AD to authenticate is registering your application in an Azure AD tenant.</span></span> <span data-ttu-id="581b4-140">Az alkalmazás regisztrálása lehetővé teszi az Azure hívja [Active Directory Authentication Library] [ aad_adal] (ADAL) a felhasználói kódból.</span><span class="sxs-lookup"><span data-stu-id="581b4-140">Registering your application enables you to call the Azure [Active Directory Authentication Library][aad_adal] (ADAL) from your code.</span></span> <span data-ttu-id="581b4-141">Az adal-t az API-k biztosít hitelesítéséhez az Azure AD az alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="581b4-141">The ADAL provides an API for authenticating with Azure AD from your application.</span></span> <span data-ttu-id="581b4-142">Az alkalmazás regisztrálása kell hogy tervezi integrált hitelesítést vagy egy egyszerű szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="581b4-142">Registering your application is required whether you plan to use integrated authentication or a service principal.</span></span>

<span data-ttu-id="581b4-143">Ha regisztrálja az alkalmazást, adja meg információkat az Azure AD az alkalmazással kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="581b4-143">When you register your application, you supply information about your application to Azure AD.</span></span> <span data-ttu-id="581b4-144">Az Azure AD majd biztosít, amelyekkel az alkalmazás társítása az Azure AD futásidőben Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="581b4-144">Azure AD then provides an application ID that you use to associate your application with Azure AD at runtime.</span></span> <span data-ttu-id="581b4-145">Az Alkalmazásazonosító kapcsolatos további információkért lásd: [alkalmazás és szolgáltatás egyszerű objektumok az Azure Active Directoryban](../active-directory/develop/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="581b4-145">To learn more about the application ID, see [Application and service principal objects in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span></span>

<span data-ttu-id="581b4-146">A kötegelt alkalmazás regisztrálásához kövesse a [egy alkalmazás hozzáadása](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) szakasz [alkalmazások integrálása az Azure Active Directory][aad_integrate].</span><span class="sxs-lookup"><span data-stu-id="581b4-146">To register your Batch application, follow the steps in the [Adding an Application](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) section in [Integrating applications with Azure Active Directory][aad_integrate].</span></span> <span data-ttu-id="581b4-147">Natív alkalmazás regisztrálnia az alkalmazást, ha bármilyen érvényes URI-JÁNAK megadhatja a **átirányítási URI-**.</span><span class="sxs-lookup"><span data-stu-id="581b4-147">If you register your application as a Native Application, you can specify any valid URI for the **Redirect URI**.</span></span> <span data-ttu-id="581b4-148">Nem kell egy valódi végpontot bizonyult.</span><span class="sxs-lookup"><span data-stu-id="581b4-148">It does not need to be a real endpoint.</span></span>

<span data-ttu-id="581b4-149">Az alkalmazás regisztrálása után megjelenik az alkalmazás azonosítója:</span><span class="sxs-lookup"><span data-stu-id="581b4-149">After you've registered your application, you'll see the application ID:</span></span>

![A kötegelt alkalmazás regisztrálása az Azure ad szolgáltatással](./media/batch-aad-auth/app-registration-data-plane.png)

<span data-ttu-id="581b4-151">Egy alkalmazás regisztrálása az Azure ad-vel kapcsolatos további információkért lásd: [hitelesítési forgatókönyvek az Azure AD](../active-directory/develop/active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="581b4-151">For more information about registering an application with Azure AD, see [Authentication Scenarios for Azure AD](../active-directory/develop/active-directory-authentication-scenarios.md).</span></span>

## <a name="get-the-tenant-id-for-your-active-directory"></a><span data-ttu-id="581b4-152">Az Active Directory Bérlőazonosító beszerzése</span><span class="sxs-lookup"><span data-stu-id="581b4-152">Get the tenant ID for your Active Directory</span></span>

<span data-ttu-id="581b4-153">A bérlő azonosítója azonosítja az Azure AD-bérlő számára az alkalmazás hitelesítési szolgáltatásokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="581b4-153">The tenant ID identifies the Azure AD tenant that provides authentication services to your application.</span></span> <span data-ttu-id="581b4-154">Ahhoz, hogy a bérlő azonosítója, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="581b4-154">To get the tenant ID, follow these steps:</span></span>

1. <span data-ttu-id="581b4-155">Az Azure-portálon válassza ki az Active Directory.</span><span class="sxs-lookup"><span data-stu-id="581b4-155">In the Azure portal, select your Active Directory.</span></span>
2. <span data-ttu-id="581b4-156">Kattintson a **Tulajdonságok** elemre.</span><span class="sxs-lookup"><span data-stu-id="581b4-156">Click **Properties**.</span></span>
3. <span data-ttu-id="581b4-157">A megadott könyvtár-azonosítóhoz GUID-érték másolása</span><span class="sxs-lookup"><span data-stu-id="581b4-157">Copy the GUID value provided for the directory ID.</span></span> <span data-ttu-id="581b4-158">Ez az érték rövidítése a bérlő azonosítója.</span><span class="sxs-lookup"><span data-stu-id="581b4-158">This value is also called the tenant ID.</span></span>

![Másolja a könyvtár-azonosítója](./media/batch-aad-auth/aad-directory-id.png)


## <a name="use-integrated-authentication"></a><span data-ttu-id="581b4-160">Integrált hitelesítés használata</span><span class="sxs-lookup"><span data-stu-id="581b4-160">Use integrated authentication</span></span>

<span data-ttu-id="581b4-161">Hitelesítés integrált hitelesítés, kell biztosítania az alkalmazás engedélyeit a Batch szolgáltatás API való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="581b4-161">To authenticate with integrated authentication, you need to grant your application permissions to connect to the Batch service API.</span></span> <span data-ttu-id="581b4-162">Ez a lépés lehetővé teszi, hogy az alkalmazás az Azure ad-vel a Batch szolgáltatás API hívások hitelesítése.</span><span class="sxs-lookup"><span data-stu-id="581b4-162">This step enables your application to authenticate calls to the Batch service API with Azure AD.</span></span>

<span data-ttu-id="581b4-163">Miután megismerte [az alkalmazás regisztrálva](#register-your-application-with-an-azure-ad-tenant), kövesse az alábbi lépéseket az Azure-portálon való hozzáférést, a Batch szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="581b4-163">Once you've [registered your application](#register-your-application-with-an-azure-ad-tenant), follow these steps in the Azure portal to grant it access to the Batch service:</span></span>

1. <span data-ttu-id="581b4-164">Az Azure portál bal oldali navigációs ablaktábláján válassza **több szolgáltatások**, kattintson a **App regisztrációk**.</span><span class="sxs-lookup"><span data-stu-id="581b4-164">In the left-hand navigation pane of the Azure portal, choose **More Services**, click **App Registrations**.</span></span>
2. <span data-ttu-id="581b4-165">Keresse meg az alkalmazás regisztrációk a listában az alkalmazás nevét:</span><span class="sxs-lookup"><span data-stu-id="581b4-165">Search for the name of your application in the list of app registrations:</span></span>

    ![Keresse meg az alkalmazás neve](./media/batch-aad-auth/search-app-registration.png)

3. <span data-ttu-id="581b4-167">Nyissa meg a **beállítások** panel az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="581b4-167">Open the **Settings** blade for your application.</span></span> <span data-ttu-id="581b4-168">Az a **API-hozzáférés** szakaszban jelölje be **szükséges engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="581b4-168">In the **API Access** section, select **Required permissions**.</span></span>
4. <span data-ttu-id="581b4-169">Az a **szükséges engedélyek** panelen kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="581b4-169">In the **Required permissions** blade, click the **Add** button.</span></span>
5. <span data-ttu-id="581b4-170">1. lépésben a keresse meg a kötegelt API.</span><span class="sxs-lookup"><span data-stu-id="581b4-170">In step 1, search for the Batch API.</span></span> <span data-ttu-id="581b4-171">Keressen rá az egyes karakterláncokra, addig amíg meg nem találja az API-t:</span><span class="sxs-lookup"><span data-stu-id="581b4-171">Search for each of these strings until you find the API:</span></span>
    1. <span data-ttu-id="581b4-172">**MicrosoftAzureBatch**.</span><span class="sxs-lookup"><span data-stu-id="581b4-172">**MicrosoftAzureBatch**.</span></span>
    2. <span data-ttu-id="581b4-173">**Microsoft Azure Batch**.</span><span class="sxs-lookup"><span data-stu-id="581b4-173">**Microsoft Azure Batch**.</span></span> <span data-ttu-id="581b4-174">Újabb Azure AD-bérlők ezt a nevet használhatják.</span><span class="sxs-lookup"><span data-stu-id="581b4-174">Newer Azure AD tenants may use this name.</span></span>
    3. <span data-ttu-id="581b4-175">A **ddbf3205-c6bd-46ae-8127-60eb93363864** a Batch API azonosítója.</span><span class="sxs-lookup"><span data-stu-id="581b4-175">**ddbf3205-c6bd-46ae-8127-60eb93363864** is the ID for the Batch API.</span></span> 
6. <span data-ttu-id="581b4-176">Miután megtalálta a kötegelt API-t, jelölje ki, majd kattintson a **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="581b4-176">Once you find the Batch API, select it and click the **Select** button.</span></span>
6. <span data-ttu-id="581b4-177">A 2. lépésben, jelölje be a jelölőnégyzetet a **Azure Batch szolgáltatás** , és kattintson a **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="581b4-177">In step 2, select the check box next to **Access Azure Batch Service** and click the **Select** button.</span></span>
7. <span data-ttu-id="581b4-178">Kattintson a **végzett** gombra.</span><span class="sxs-lookup"><span data-stu-id="581b4-178">Click the **Done** button.</span></span>

<span data-ttu-id="581b4-179">A **szükséges engedélyek** most mutat be, hogy az Azure AD alkalmazás fér hozzá az adal-t, mind a Batch szolgáltatás API panelen.</span><span class="sxs-lookup"><span data-stu-id="581b4-179">The **Required Permissions** blade now shows that your Azure AD application has access to both ADAL and the Batch service API.</span></span> <span data-ttu-id="581b4-180">Engedélyekkel az adal-ra automatikusan regisztrációt az alkalmazás az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="581b4-180">Permissions are granted to ADAL automatically when you first register your app with Azure AD.</span></span>

![Támogatás API engedélyei](./media/batch-aad-auth/required-permissions-data-plane.png)

## <a name="use-a-service-principal"></a><span data-ttu-id="581b4-182">Egy egyszerű szolgáltatás használata</span><span class="sxs-lookup"><span data-stu-id="581b4-182">Use a service principal</span></span> 

<span data-ttu-id="581b4-183">Hitelesítést végezni egy alkalmazás, amely futtatja a felügyelet nélküli, használhat egy egyszerű szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="581b4-183">To authenticate an application that runs unattended, you use a service principal.</span></span> <span data-ttu-id="581b4-184">Az alkalmazás regisztrálása után kövesse az alábbi lépéseket egy egyszerű szolgáltatás konfigurálása az Azure portálon:</span><span class="sxs-lookup"><span data-stu-id="581b4-184">After you've registered your application, follow these steps in the Azure portal to configure a service principal:</span></span>

1. <span data-ttu-id="581b4-185">Egy alkalmazás titkos kulcs kérelmet.</span><span class="sxs-lookup"><span data-stu-id="581b4-185">Request a secret key for your application.</span></span>
2. <span data-ttu-id="581b4-186">Az RBAC szerepkör hozzárendelése az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="581b4-186">Assign an RBAC role to your application.</span></span>

### <a name="request-a-secret-key-for-your-application"></a><span data-ttu-id="581b4-187">A kérelem egy alkalmazás titkos kulcs</span><span class="sxs-lookup"><span data-stu-id="581b4-187">Request a secret key for your application</span></span>

<span data-ttu-id="581b4-188">Ha az alkalmazás egy egyszerű szolgáltatás végzi a hitelesítést, elküld az Alkalmazásazonosítót és a titkos kulcsot az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="581b4-188">When your application authenticates with a service principal, it sends both the application ID and a secret key to Azure AD.</span></span> <span data-ttu-id="581b4-189">Meg kell létrehozni, és másolja át a titkos kulcsot a kódból használni.</span><span class="sxs-lookup"><span data-stu-id="581b4-189">You'll need to create and copy the secret key to use from your code.</span></span>

<span data-ttu-id="581b4-190">Kövesse az alábbi lépéseket az Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="581b4-190">Follow these steps in the Azure portal:</span></span>

1. <span data-ttu-id="581b4-191">Az Azure portál bal oldali navigációs ablaktábláján válassza **több szolgáltatások**, kattintson a **App regisztrációk**.</span><span class="sxs-lookup"><span data-stu-id="581b4-191">In the left-hand navigation pane of the Azure portal, choose **More Services**, click **App Registrations**.</span></span>
2. <span data-ttu-id="581b4-192">Keresse meg az alkalmazás regisztrációk a listában az alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="581b4-192">Search for the name of your application in the list of app registrations.</span></span>
3. <span data-ttu-id="581b4-193">Megjelenítés a **beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="581b4-193">Display the **Settings** blade.</span></span> <span data-ttu-id="581b4-194">Az a **API-hozzáférés** szakaszban jelölje be **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="581b4-194">In the **API Access** section, select **Keys**.</span></span>
4. <span data-ttu-id="581b4-195">A kulcs létrehozásához adja meg a kulcs leírását.</span><span class="sxs-lookup"><span data-stu-id="581b4-195">To create a key, enter a description for the key.</span></span> <span data-ttu-id="581b4-196">Ezután válassza ki a kulcs egy vagy két éves időtartammal.</span><span class="sxs-lookup"><span data-stu-id="581b4-196">Then select a duration for the key of either one or two years.</span></span> 
5. <span data-ttu-id="581b4-197">Kattintson a **mentése** gombra kattintva hozzon létre, és megjeleníti a kulcsot.</span><span class="sxs-lookup"><span data-stu-id="581b4-197">Click the **Save** button to create and display the key.</span></span> <span data-ttu-id="581b4-198">Másolja egy biztonságos helyre, a kulcs értékét, akkor nem fog tudni férni újra Ha kilép a panelen.</span><span class="sxs-lookup"><span data-stu-id="581b4-198">Copy the key value to a safe place, as you won't be able to access it again after you leave the blade.</span></span> 

    ![A titkos kulcs létrehozása](./media/batch-aad-auth/secret-key.png)

### <a name="assign-an-rbac-role-to-your-application"></a><span data-ttu-id="581b4-200">Az RBAC szerepkör hozzárendelése az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="581b4-200">Assign an RBAC role to your application</span></span>

<span data-ttu-id="581b4-201">Egy egyszerű szolgáltatásnév végzett hitelesítéshez, akkor hozzá kell rendelni RBAC szerepet az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="581b4-201">To authenticate with a service principal, you need to assign an RBAC role to your application.</span></span> <span data-ttu-id="581b4-202">Kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="581b4-202">Follow these steps:</span></span>

1. <span data-ttu-id="581b4-203">Az Azure-portálon lépjen a Batch-fiók, amelyet az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="581b4-203">In the Azure portal, navigate to the Batch account used by your application.</span></span>
2. <span data-ttu-id="581b4-204">Az a **beállítások** a Batch-fiók, jelölje be a panelt **hozzáférés-vezérlés (IAM)**.</span><span class="sxs-lookup"><span data-stu-id="581b4-204">In the **Settings** blade for the Batch account, select **Access Control (IAM)**.</span></span>
3. <span data-ttu-id="581b4-205">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="581b4-205">Click the **Add** button.</span></span> 
4. <span data-ttu-id="581b4-206">Az a **szerepkör** legördülő listából válassza a _közreműködő_ vagy _olvasó_ szerepkör az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="581b4-206">From the **Role** drop-down, choose either the _Contributor_ or _Reader_ role for your application.</span></span> <span data-ttu-id="581b4-207">Ezek a szerepkörök további információkért lásd: [szerepköralapú hozzáférés-vezérlés az Azure-portálon az első lépései](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="581b4-207">For more information on these roles, see [Get started with Role-Based Access Control in the Azure portal](../active-directory/role-based-access-control-what-is.md).</span></span>  
5. <span data-ttu-id="581b4-208">Az a **válasszon** mezőbe írja be az alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="581b4-208">In the **Select** field, enter the name of your application.</span></span> <span data-ttu-id="581b4-209">Válassza ki az alkalmazást a listából, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="581b4-209">Select your application from the list, and click **Save**.</span></span>

<span data-ttu-id="581b4-210">Az alkalmazás ekkor meg kell jelennie a hozzáférés-vezérlési beállításokkal hozzárendelt RBAC szerepkörrel rendelkező.</span><span class="sxs-lookup"><span data-stu-id="581b4-210">Your application should now appear in your access control settings with an RBAC role assigned.</span></span> 

![Az RBAC szerepkör hozzárendelése az alkalmazáshoz](./media/batch-aad-auth/app-rbac-role.png)

### <a name="get-the-tenant-id-for-your-azure-active-directory"></a><span data-ttu-id="581b4-212">Az Azure Active Directory Bérlőazonosító beszerzése</span><span class="sxs-lookup"><span data-stu-id="581b4-212">Get the tenant ID for your Azure Active Directory</span></span>

<span data-ttu-id="581b4-213">A bérlő azonosítója azonosítja az Azure AD-bérlő számára az alkalmazás hitelesítési szolgáltatásokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="581b4-213">The tenant ID identifies the Azure AD tenant that provides authentication services to your application.</span></span> <span data-ttu-id="581b4-214">Ahhoz, hogy a bérlő azonosítója, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="581b4-214">To get the tenant ID, follow these steps:</span></span>

1. <span data-ttu-id="581b4-215">Az Azure-portálon válassza ki az Active Directory.</span><span class="sxs-lookup"><span data-stu-id="581b4-215">In the Azure portal, select your Active Directory.</span></span>
2. <span data-ttu-id="581b4-216">Kattintson a **Tulajdonságok** elemre.</span><span class="sxs-lookup"><span data-stu-id="581b4-216">Click **Properties**.</span></span>
3. <span data-ttu-id="581b4-217">A megadott könyvtár-azonosítóhoz GUID-érték másolása</span><span class="sxs-lookup"><span data-stu-id="581b4-217">Copy the GUID value provided for the directory ID.</span></span> <span data-ttu-id="581b4-218">Ez az érték rövidítése a bérlő azonosítója.</span><span class="sxs-lookup"><span data-stu-id="581b4-218">This value is also called the tenant ID.</span></span>

![Másolja a könyvtár-azonosítója](./media/batch-aad-auth/aad-directory-id.png)


## <a name="code-examples"></a><span data-ttu-id="581b4-220">Kódpéldák</span><span class="sxs-lookup"><span data-stu-id="581b4-220">Code examples</span></span>

<span data-ttu-id="581b4-221">Ebben a szakaszban szereplő példák bemutatják, hogyan hitelesítéséhez az Azure AD integrált hitelesítésen keresztül, és egy egyszerű szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="581b4-221">The code examples in this section show how to authenticate with Azure AD using integrated authentication and with a service principal.</span></span> <span data-ttu-id="581b4-222">A Kódminták .NET használja, de a fogalmakat hasonló, ha a többi nyelvet.</span><span class="sxs-lookup"><span data-stu-id="581b4-222">These code examples use .NET, but the concepts are similar for other languages.</span></span>

> [!NOTE]
> <span data-ttu-id="581b4-223">Egy Azure AD hitelesítési tokent egy óra múlva lejár.</span><span class="sxs-lookup"><span data-stu-id="581b4-223">An Azure AD authentication token expires after one hour.</span></span> <span data-ttu-id="581b4-224">A hosszú élettartamú használatakor **BatchClient** objektum, azt javasoljuk, hogy visszaállíthatja a jogkivonat az ADAL halasztása minden kérelemnél annak érdekében, hogy mindig legyen egy érvényes tokent.</span><span class="sxs-lookup"><span data-stu-id="581b4-224">When using a long-lived **BatchClient** object, we recommend that you retrieve a token from ADAL on every request to ensure you always have a valid token.</span></span> 
>
>
> <span data-ttu-id="581b4-225">Ennek érdekében a .NET olyan metódus írása, amely a token kikeresi az Azure AD, és adja át az adott metódust egy **BatchTokenCredentials** meghatalmazottként objektum.</span><span class="sxs-lookup"><span data-stu-id="581b4-225">To achieve this in .NET, write a method that retrieves the token from Azure AD and pass that method to a **BatchTokenCredentials** object as a delegate.</span></span> <span data-ttu-id="581b4-226">A delegált metódus lehívásra kerül halasztása minden kérelemnél a Batch szolgáltatás biztosításához, hogy egy érvényes jogkivonat van-e megadva.</span><span class="sxs-lookup"><span data-stu-id="581b4-226">The delegate method is called on every request to the Batch service to ensure that a valid token is provided.</span></span> <span data-ttu-id="581b4-227">Alapértelmezés szerint a ADAL jogkivonatokat, gyorsítótárazza, így egy új jogkivonatot az Azure AD kéri le a rendszer csak akkor, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="581b4-227">By default ADAL caches tokens, so a new token is retrieved from Azure AD only when necessary.</span></span> <span data-ttu-id="581b4-228">Az Azure AD-jogkivonatokat kapcsolatos további információkért lásd: [hitelesítési forgatókönyvek az Azure AD][aad_auth_scenarios].</span><span class="sxs-lookup"><span data-stu-id="581b4-228">For more information about tokens in Azure AD, see [Authentication Scenarios for Azure AD][aad_auth_scenarios].</span></span>
>
>

### <a name="code-example-using-azure-ad-integrated-authentication-with-batch-net"></a><span data-ttu-id="581b4-229">Példa: az Azure AD hitelesítési integrálva Batch .NET</span><span class="sxs-lookup"><span data-stu-id="581b4-229">Code example: Using Azure AD integrated authentication with Batch .NET</span></span>

<span data-ttu-id="581b4-230">A hitelesítéshez a Batch .NET-integrált hitelesítés hivatkozik a [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) csomag és a [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) csomag.</span><span class="sxs-lookup"><span data-stu-id="581b4-230">To authenticate with integrated authentication from Batch .NET, reference the [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) package and the [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) package.</span></span>

<span data-ttu-id="581b4-231">Adja meg a következőket `using` utasítások a kódban:</span><span class="sxs-lookup"><span data-stu-id="581b4-231">Include the following `using` statements in your code:</span></span>

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

<span data-ttu-id="581b4-232">Útmutató az Azure AD-végpont a kódban, beleértve a bérlő azonosítója.</span><span class="sxs-lookup"><span data-stu-id="581b4-232">Reference the Azure AD endpoint in your code, including the tenant ID.</span></span> <span data-ttu-id="581b4-233">Bérlői azonosító lekéréséhez kövesse a témakörben ismertetett lépéseket [bérlői azonosító lekérése az Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span><span class="sxs-lookup"><span data-stu-id="581b4-233">To retrieve the tenant ID, follow the steps outlined in [Get the tenant ID for your Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span></span>

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

<span data-ttu-id="581b4-234">A Batch szolgáltatás erőforrás végpont – referencia:</span><span class="sxs-lookup"><span data-stu-id="581b4-234">Reference the Batch service resource endpoint:</span></span>

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

<span data-ttu-id="581b4-235">A Batch-fiók hivatkozási:</span><span class="sxs-lookup"><span data-stu-id="581b4-235">Reference your Batch account:</span></span>

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

<span data-ttu-id="581b4-236">Adja meg az alkalmazás Azonosítóját (ügyfél-azonosító) az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="581b4-236">Specify the application ID (client ID) for your application.</span></span> <span data-ttu-id="581b4-237">Az Alkalmazásazonosítót a alkalmazás regisztrálása az Azure-portálon a érhető el:</span><span class="sxs-lookup"><span data-stu-id="581b4-237">The application ID is available from your app registration in the Azure portal:</span></span>

```csharp
private const string ClientId = "<application-id>";
```

<span data-ttu-id="581b4-238">Az átirányítási URI-t a regisztráció során megadott szeretne másolni.</span><span class="sxs-lookup"><span data-stu-id="581b4-238">Also copy the redirect URI that you specified during the registration process.</span></span> <span data-ttu-id="581b4-239">Az átirányítási URI a kódban megadott meg kell egyeznie az átirányítási URI-t az alkalmazás regisztrálásakor megadott:</span><span class="sxs-lookup"><span data-stu-id="581b4-239">The redirect URI specified in your code must match the redirect URI that you provided when you registered the application:</span></span>

```csharp
private const string RedirectUri = "http://mybatchdatasample";
```

<span data-ttu-id="581b4-240">Az Azure AD a hitelesítési jogkivonat beszerzése az egy visszahívási metódus írása.</span><span class="sxs-lookup"><span data-stu-id="581b4-240">Write a callback method to acquire the authentication token from Azure AD.</span></span> <span data-ttu-id="581b4-241">A **GetAuthenticationTokenAsync** visszahívási metódus itt látható a hívások adal-t hitelesítéshez a felhasználó, aki az alkalmazással együtt dolgozik.</span><span class="sxs-lookup"><span data-stu-id="581b4-241">The **GetAuthenticationTokenAsync** callback method shown here calls ADAL to authenticate a user who is interacting with the application.</span></span> <span data-ttu-id="581b4-242">A **AcquireTokenAsync** ADAL által megadott metódus bekéri a felhasználótól a hitelesítő adataikat, és az alkalmazás telepítése folytatódik, ha a felhasználó eszközeivel (kivéve, ha már gyorsítótárazott hitelesítő adatok):</span><span class="sxs-lookup"><span data-stu-id="581b4-242">The **AcquireTokenAsync** method provided by ADAL prompts the user for their credentials, and the application proceeds once the user provides them (unless it has already cached credentials):</span></span>

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    var authContext = new AuthenticationContext(AuthorityUri);

    // Acquire the authentication token from Azure AD.
    var authResult = await authContext.AcquireTokenAsync(BatchResourceUri, 
                                                        ClientId, 
                                                        new Uri(RedirectUri), 
                                                        new PlatformParameters(PromptBehavior.Auto));

    return authResult.AccessToken;
}
```

<span data-ttu-id="581b4-243">Összeállíthatja a **BatchTokenCredentials** objektum, amely a delegált paramétert fogad.</span><span class="sxs-lookup"><span data-stu-id="581b4-243">Construct a **BatchTokenCredentials** object that takes the delegate as a parameter.</span></span> <span data-ttu-id="581b4-244">Ezen hitelesítő adatok segítségével nyissa meg a **BatchClient** objektum.</span><span class="sxs-lookup"><span data-stu-id="581b4-244">Use those credentials to open a **BatchClient** object.</span></span> <span data-ttu-id="581b4-245">Is használhatja, amely **BatchClient** -objektumot a Batch szolgáltatás további műveleteket:</span><span class="sxs-lookup"><span data-stu-id="581b4-245">You can use that **BatchClient** object for subsequent operations against the Batch service:</span></span>

```csharp
public static async Task PerformBatchOperations()
{
    Func<Task<string>> tokenProvider = () => GetAuthenticationTokenAsync();

    using (var client = await BatchClient.OpenAsync(new BatchTokenCredentials(BatchAccountUrl, tokenProvider)))
    {
        await client.JobOperations.ListJobs().ToListAsync();
    }
}
```

### <a name="code-example-using-an-azure-ad-service-principal-with-batch-net"></a><span data-ttu-id="581b4-246">Példa: az Azure AD szolgáltatás egyszerű Batch .NET használatával</span><span class="sxs-lookup"><span data-stu-id="581b4-246">Code example: Using an Azure AD service principal with Batch .NET</span></span>

<span data-ttu-id="581b4-247">Egy egyszerű szolgáltatást Batch .NET hitelesítés, hivatkozzon a [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) csomag és a [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) csomag.</span><span class="sxs-lookup"><span data-stu-id="581b4-247">To authenticate with a service principal from Batch .NET, reference the [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) package and the [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) package.</span></span>

<span data-ttu-id="581b4-248">Adja meg a következőket `using` utasítások a kódban:</span><span class="sxs-lookup"><span data-stu-id="581b4-248">Include the following `using` statements in your code:</span></span>

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

<span data-ttu-id="581b4-249">Útmutató az Azure AD-végpont a kódban, beleértve a bérlő azonosítója.</span><span class="sxs-lookup"><span data-stu-id="581b4-249">Reference the Azure AD endpoint in your code, including the tenant ID.</span></span> <span data-ttu-id="581b4-250">Egy egyszerű szolgáltatás használata esetén meg kell adnia egy bérlő-specifikus végpontot.</span><span class="sxs-lookup"><span data-stu-id="581b4-250">When using a service principal, you must provide a tenant-specific endpoint.</span></span> <span data-ttu-id="581b4-251">Bérlői azonosító lekéréséhez kövesse a témakörben ismertetett lépéseket [bérlői azonosító lekérése az Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span><span class="sxs-lookup"><span data-stu-id="581b4-251">To retrieve the tenant ID, follow the steps outlined in [Get the tenant ID for your Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span></span>

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

<span data-ttu-id="581b4-252">A Batch szolgáltatás erőforrás végpont – referencia:</span><span class="sxs-lookup"><span data-stu-id="581b4-252">Reference the Batch service resource endpoint:</span></span>  

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

<span data-ttu-id="581b4-253">A Batch-fiók hivatkozási:</span><span class="sxs-lookup"><span data-stu-id="581b4-253">Reference your Batch account:</span></span>

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

<span data-ttu-id="581b4-254">Adja meg az alkalmazás Azonosítóját (ügyfél-azonosító) az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="581b4-254">Specify the application ID (client ID) for your application.</span></span> <span data-ttu-id="581b4-255">Az Alkalmazásazonosítót a alkalmazás regisztrálása az Azure-portálon a érhető el:</span><span class="sxs-lookup"><span data-stu-id="581b4-255">The application ID is available from your app registration in the Azure portal:</span></span>

```csharp
private const string ClientId = "<application-id>";
```

<span data-ttu-id="581b4-256">Adja meg a titkos kulcsot az Azure portálról másolt:</span><span class="sxs-lookup"><span data-stu-id="581b4-256">Specify the secret key that you copied from the Azure portal:</span></span>

```csharp
private const string ClientKey = "<secret-key>";
```

<span data-ttu-id="581b4-257">Az Azure AD a hitelesítési jogkivonat beszerzése az egy visszahívási metódus írása.</span><span class="sxs-lookup"><span data-stu-id="581b4-257">Write a callback method to acquire the authentication token from Azure AD.</span></span> <span data-ttu-id="581b4-258">A **GetAuthenticationTokenAsync** visszahívási metódus hívások az adal TÁRAT a felügyelet nélküli hitelesítéshez itt látható:</span><span class="sxs-lookup"><span data-stu-id="581b4-258">The **GetAuthenticationTokenAsync** callback method shown here calls ADAL for unattended authentication:</span></span>

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
    AuthenticationResult authResult = await authContext.AcquireTokenAsync(BatchResourceUri, new ClientCredential(ClientId, ClientKey));

    return authResult.AccessToken;
}
```

<span data-ttu-id="581b4-259">Összeállíthatja a **BatchTokenCredentials** objektum, amely a delegált paramétert fogad.</span><span class="sxs-lookup"><span data-stu-id="581b4-259">Construct a **BatchTokenCredentials** object that takes the delegate as a parameter.</span></span> <span data-ttu-id="581b4-260">Ezen hitelesítő adatok segítségével nyissa meg a **BatchClient** objektum.</span><span class="sxs-lookup"><span data-stu-id="581b4-260">Use those credentials to open a **BatchClient** object.</span></span> <span data-ttu-id="581b4-261">Ezután használhatja, amely **BatchClient** -objektumot a Batch szolgáltatás további műveleteket:</span><span class="sxs-lookup"><span data-stu-id="581b4-261">You can then use that **BatchClient** object for subsequent operations against the Batch service:</span></span>

```csharp
public static async Task PerformBatchOperations()
{
    Func<Task<string>> tokenProvider = () => GetAuthenticationTokenAsync();

    using (var client = await BatchClient.OpenAsync(new BatchTokenCredentials(BatchAccountUrl, tokenProvider)))
    {
        await client.JobOperations.ListJobs().ToListAsync();
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="581b4-262">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="581b4-262">Next steps</span></span>

<span data-ttu-id="581b4-263">Az Azure AD kapcsolatos további tudnivalókért tekintse meg a [Azure Active Directory dokumentációjának](https://docs.microsoft.com/azure/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="581b4-263">To learn more about Azure AD, see the [Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span> <span data-ttu-id="581b4-264">Részletes példa bemutatja, hogyan adal-t használó érhetők el a [Azure mintakódok](https://azure.microsoft.com/resources/samples/?service=active-directory) könyvtár.</span><span class="sxs-lookup"><span data-stu-id="581b4-264">In-depth examples showing how to use ADAL are available in the [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=active-directory) library.</span></span>

<span data-ttu-id="581b4-265">Szolgáltatásnevekről kapcsolatos további információkért lásd: [alkalmazás és szolgáltatás egyszerű objektumok az Azure Active Directoryban](../active-directory/develop/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="581b4-265">To learn more about service principals, see [Application and service principal objects in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span></span> <span data-ttu-id="581b4-266">Az Azure portál használatával egyszerű szolgáltatás létrehozása: [használata portal létrehozása az Active Directory erőforrásokat elérő alkalmazás és szolgáltatás egyszerű](../resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="581b4-266">To create a service principal using the Azure portal, see [Use portal to create Active Directory application and service principal that can access resources](../resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="581b4-267">A szolgáltatás egyszerű PowerShell vagy az Azure parancssori felület is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="581b4-267">You can also create a service principal with PowerShell or Azure CLI.</span></span> 

<span data-ttu-id="581b4-268">Kötegelt felügyeleti alkalmazások az Azure AD hitelesíti, lásd: [hitelesítéséhez kötegelt megoldásokat az Active Directory](batch-aad-auth-management.md).</span><span class="sxs-lookup"><span data-stu-id="581b4-268">To authenticate Batch Management applications using Azure AD, see [Authenticate Batch Management solutions with Active Directory](batch-aad-auth-management.md).</span></span> 

<span data-ttu-id="581b4-269">[aad_about]: ../active-directory/active-directory-whatis.md "Mi az Azure Active Directory?"</span><span class="sxs-lookup"><span data-stu-id="581b4-269">[aad_about]: ../active-directory/active-directory-whatis.md "What is Azure Active Directory?"</span></span>
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
<span data-ttu-id="581b4-270">[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Az Azure Active Directory hitelesítési forgatókönyvei"</span><span class="sxs-lookup"><span data-stu-id="581b4-270">[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Authentication Scenarios for Azure AD"</span></span>
<span data-ttu-id="581b4-271">[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Alkalmazások integrálása az Azure Active Directoryval"</span><span class="sxs-lookup"><span data-stu-id="581b4-271">[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrating Applications with Azure Active Directory"</span></span>
[azure_portal]: http://portal.azure.com
