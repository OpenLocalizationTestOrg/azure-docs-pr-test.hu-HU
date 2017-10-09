---
title: "aaaUse Azure Active Directory tooauthenticate Azure Batch szolgáltatás megoldások |} Microsoft Docs"
description: "Kötegelt támogatja az Azure AD a Batch szolgáltatás hello hitelesítéshez."
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
ms.openlocfilehash: 6c825c30f1c80bb059a797a2e78367e599acd109
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-batch-service-solutions-with-active-directory"></a><span data-ttu-id="463ee-103">Kötegelt szolgáltatási megoldások és az Active Directory hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="463ee-103">Authenticate Batch service solutions with Active Directory</span></span>

<span data-ttu-id="463ee-104">Az Azure Batch támogatja a hitelesítési [Azure Active Directory] [ aad_about] (az Azure AD).</span><span class="sxs-lookup"><span data-stu-id="463ee-104">Azure Batch supports authentication with [Azure Active Directory][aad_about] (Azure AD).</span></span> <span data-ttu-id="463ee-105">Az Azure AD a Microsoft több-bérlős felhőalapú címtár és az identity management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="463ee-105">Azure AD is Microsoft’s multi-tenant cloud based directory and identity management service.</span></span> <span data-ttu-id="463ee-106">Azure maga használja az Azure AD tooauthenticate, az ügyfelek, a szolgáltatás-rendszergazdák és a szervezeti felhasználók.</span><span class="sxs-lookup"><span data-stu-id="463ee-106">Azure itself uses Azure AD tooauthenticate its customers, service administrators, and organizational users.</span></span>

<span data-ttu-id="463ee-107">Az Azure AD-alapú hitelesítés használata az Azure Batch, az alábbi két módszer egyikével hitelesítheti:</span><span class="sxs-lookup"><span data-stu-id="463ee-107">When using Azure AD authentication with Azure Batch, you can authenticate in one of two ways:</span></span>

- <span data-ttu-id="463ee-108">A **integrált hitelesítés** tooauthenticate egy olyan felhasználó, hello alkalmazás dolgozik.</span><span class="sxs-lookup"><span data-stu-id="463ee-108">By using **integrated authentication** tooauthenticate a user that is interacting with hello application.</span></span> <span data-ttu-id="463ee-109">Integrált hitelesítést használó alkalmazások a felhasználói hitelesítő adatokat gyűjt, és ezen hitelesítő adatok tooauthenticate hozzáférés tooBatch erőforrást használ.</span><span class="sxs-lookup"><span data-stu-id="463ee-109">An application using integrated authentication gathers a user's credentials and uses those credentials tooauthenticate access tooBatch resources.</span></span>
- <span data-ttu-id="463ee-110">Használatával egy **egyszerű** tooauthenticate egy felügyelet nélküli alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="463ee-110">By using a **service principal** tooauthenticate an unattended application.</span></span> <span data-ttu-id="463ee-111">Egy egyszerű definiálja a hello házirendet és egy alkalmazás engedélyeinek rendelés toorepresent hello alkalmazás futásidőben erőforrásokhoz való hozzáféréskor.</span><span class="sxs-lookup"><span data-stu-id="463ee-111">A service principal defines hello policy and permissions for an application in order toorepresent hello application when accessing resources at runtime.</span></span>

<span data-ttu-id="463ee-112">További információ az Azure AD toolearn lásd: hello [Azure Active Directory dokumentációjának](https://docs.microsoft.com/azure/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="463ee-112">toolearn more about Azure AD, see hello [Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span>

## <a name="authentication-and-pool-allocation-mode"></a><span data-ttu-id="463ee-113">Hitelesítés és a tárolókészlet foglalási mód</span><span class="sxs-lookup"><span data-stu-id="463ee-113">Authentication and pool allocation mode</span></span>

<span data-ttu-id="463ee-114">Batch-fiók létrehozásakor megadhatja, ahol fiók létrehozott készleteket kell kiosztani.</span><span class="sxs-lookup"><span data-stu-id="463ee-114">When you create a Batch account, you can specify where pools created for that account should be allocated.</span></span> <span data-ttu-id="463ee-115">Választhat tooallocate készletek hello alapértelmezett Batch szolgáltatás előfizetés vagy a felhasználó az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="463ee-115">You can choose tooallocate pools either in hello default Batch service subscription or in a user subscription.</span></span> <span data-ttu-id="463ee-116">A kiválasztott érinti Önt a hitelesítéshez, hogyan fiókhoz tartozó hozzáférési tooresources.</span><span class="sxs-lookup"><span data-stu-id="463ee-116">Your choice affects how you authenticate access tooresources in that account.</span></span>

- <span data-ttu-id="463ee-117">**A Batch-szolgáltatás előfizetésének**.</span><span class="sxs-lookup"><span data-stu-id="463ee-117">**Batch service subscription**.</span></span> <span data-ttu-id="463ee-118">Alapértelmezés szerint a Batch szolgáltatás előfizetés kötegelt készletek foglal le.</span><span class="sxs-lookup"><span data-stu-id="463ee-118">By default, Batch pools are allocated in a Batch service subscription.</span></span> <span data-ttu-id="463ee-119">Ha ezzel a beállítással hitelesítheti fiókhoz tartozó hozzáférési tooresources vagy [megosztott kulcsos](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service) vagy az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="463ee-119">If you choose this option, you can authenticate access tooresources in that account either with [Shared Key](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service) or with Azure AD.</span></span>
- <span data-ttu-id="463ee-120">**Felhasználó-előfizetés.**</span><span class="sxs-lookup"><span data-stu-id="463ee-120">**User subscription.**</span></span> <span data-ttu-id="463ee-121">Választhat tooallocate kötegelt készletek egy megadott felhasználó az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="463ee-121">You can choose tooallocate Batch pools in a user subscription that you specify.</span></span> <span data-ttu-id="463ee-122">Ha ezt a lehetőséget választja, hitelesítenie kell az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="463ee-122">If you choose this option, you must authenticate with Azure AD.</span></span>

## <a name="endpoints-for-authentication"></a><span data-ttu-id="463ee-123">A hitelesítési végpontot</span><span class="sxs-lookup"><span data-stu-id="463ee-123">Endpoints for authentication</span></span>

<span data-ttu-id="463ee-124">tooinclude szüksége tooauthenticate Batch-alkalmazások az Azure ad-vel, néhány jól ismert végpontok a kódban.</span><span class="sxs-lookup"><span data-stu-id="463ee-124">tooauthenticate Batch applications with Azure AD, you need tooinclude some well-known endpoints in your code.</span></span>

### <a name="azure-ad-endpoint"></a><span data-ttu-id="463ee-125">Azure AD-végpont</span><span class="sxs-lookup"><span data-stu-id="463ee-125">Azure AD endpoint</span></span>

<span data-ttu-id="463ee-126">Alapszintű hello Azure AD-szolgáltató végpont esetében:</span><span class="sxs-lookup"><span data-stu-id="463ee-126">hello base Azure AD authority endpoint is:</span></span>

`https://login.microsoftonline.com/`

<span data-ttu-id="463ee-127">tooauthenticate az Azure ad-vel, ehhez a végponthoz hello Bérlőazonosító (directory ID) együtt használja.</span><span class="sxs-lookup"><span data-stu-id="463ee-127">tooauthenticate with Azure AD, you use this endpoint together with hello tenant ID (directory ID).</span></span> <span data-ttu-id="463ee-128">hello Bérlőazonosító hello Azure AD bérlő toouse hitelesítéshez azonosítja.</span><span class="sxs-lookup"><span data-stu-id="463ee-128">hello tenant ID identifies hello Azure AD tenant toouse for authentication.</span></span> <span data-ttu-id="463ee-129">tooretrieve hello Bérlőazonosító, hello című rész lépéseit követve [hello bérlői azonosító beszerzése az Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span><span class="sxs-lookup"><span data-stu-id="463ee-129">tooretrieve hello tenant ID, follow hello steps outlined in [Get hello tenant ID for your Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span></span>

`https://login.microsoftonline.com/<tenant-id>`

> [!NOTE] 
> <span data-ttu-id="463ee-130">hello bérlői-specifikus végpont a hitelesítést egy egyszerű szolgáltatás használata esetén szükséges.</span><span class="sxs-lookup"><span data-stu-id="463ee-130">hello tenant-specific endpoint is required when you authenticate using a service principal.</span></span> 
> 
> <span data-ttu-id="463ee-131">hello bérlői-specifikus végpont Önt a hitelesítéshez integrált hitelesítés használata kötelező, de ajánlott.</span><span class="sxs-lookup"><span data-stu-id="463ee-131">hello tenant-specific endpoint is optional when you authenticate using integrated authentication, but recommended.</span></span> <span data-ttu-id="463ee-132">Azonban használhatja hello Azure AD közös végpontot.</span><span class="sxs-lookup"><span data-stu-id="463ee-132">However, you can also use hello Azure AD common endpoint.</span></span> <span data-ttu-id="463ee-133">hello közös végpont összegyűjtéséhez felületet, ha nincs megadva egy adott bérlő általános hitelesítő adatokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="463ee-133">hello common endpoint provides a generic credential gathering interface when a specific tenant is not provided.</span></span> <span data-ttu-id="463ee-134">hello közös végpont `https://login.microsoftonline.com/common`.</span><span class="sxs-lookup"><span data-stu-id="463ee-134">hello common endpoint is `https://login.microsoftonline.com/common`.</span></span>
>
>

<span data-ttu-id="463ee-135">Az Azure AD-végpontok kapcsolatos további információkért lásd: [hitelesítési forgatókönyvek az Azure AD][aad_auth_scenarios].</span><span class="sxs-lookup"><span data-stu-id="463ee-135">For more information about Azure AD endpoints, see [Authentication Scenarios for Azure AD][aad_auth_scenarios].</span></span>

### <a name="batch-resource-endpoint"></a><span data-ttu-id="463ee-136">Kötegelt erőforrás végpont</span><span class="sxs-lookup"><span data-stu-id="463ee-136">Batch resource endpoint</span></span>

<span data-ttu-id="463ee-137">Használjon hello **Azure Batch erőforrás végpont** tooacquire hitelesítéséhez jogkivonat kérelmek toohello Batch szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="463ee-137">Use hello **Azure Batch resource endpoint** tooacquire a token for authenticating requests toohello Batch service:</span></span>

`https://batch.core.windows.net/`

## <a name="register-your-application-with-a-tenant"></a><span data-ttu-id="463ee-138">Az alkalmazás regisztrálása a bérlő</span><span class="sxs-lookup"><span data-stu-id="463ee-138">Register your application with a tenant</span></span>

<span data-ttu-id="463ee-139">hello első lépése az Azure AD tooauthenticate használatával regisztrálja az alkalmazást az Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="463ee-139">hello first step in using Azure AD tooauthenticate is registering your application in an Azure AD tenant.</span></span> <span data-ttu-id="463ee-140">Az alkalmazás regisztrálása lehetővé teszi a toocall hello Azure [Active Directory Authentication Library] [ aad_adal] (ADAL) a felhasználói kódból.</span><span class="sxs-lookup"><span data-stu-id="463ee-140">Registering your application enables you toocall hello Azure [Active Directory Authentication Library][aad_adal] (ADAL) from your code.</span></span> <span data-ttu-id="463ee-141">hello adal-t az API-k biztosít az alkalmazásból az Azure AD-val hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="463ee-141">hello ADAL provides an API for authenticating with Azure AD from your application.</span></span> <span data-ttu-id="463ee-142">Az alkalmazás regisztrálása szükség, hogy azt tervezi, hogy integrált hitelesítés toouse vagy egy egyszerű szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="463ee-142">Registering your application is required whether you plan toouse integrated authentication or a service principal.</span></span>

<span data-ttu-id="463ee-143">Ha regisztrálja az alkalmazást, adja meg információkat az alkalmazás tooAzure AD kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="463ee-143">When you register your application, you supply information about your application tooAzure AD.</span></span> <span data-ttu-id="463ee-144">Az Azure AD majd biztosít az Alkalmazásazonosító használt tooassociate az alkalmazás az Azure ad-val futásidőben.</span><span class="sxs-lookup"><span data-stu-id="463ee-144">Azure AD then provides an application ID that you use tooassociate your application with Azure AD at runtime.</span></span> <span data-ttu-id="463ee-145">toolearn hello Alkalmazásazonosító, kapcsolatos további információkért lásd: [alkalmazás és szolgáltatás egyszerű objektumok az Azure Active Directoryban](../active-directory/develop/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="463ee-145">toolearn more about hello application ID, see [Application and service principal objects in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span></span>

<span data-ttu-id="463ee-146">tooregister a kötegelt kérelem hello hello lépéseit kövesse [egy alkalmazás hozzáadása](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) szakasz [alkalmazások integrálása az Azure Active Directory][aad_integrate].</span><span class="sxs-lookup"><span data-stu-id="463ee-146">tooregister your Batch application, follow hello steps in hello [Adding an Application](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) section in [Integrating applications with Azure Active Directory][aad_integrate].</span></span> <span data-ttu-id="463ee-147">Ha natív alkalmazás regisztrálnia az alkalmazást, megadhat bármilyen érvényes URI-JÁNAK hello **átirányítási URI-**.</span><span class="sxs-lookup"><span data-stu-id="463ee-147">If you register your application as a Native Application, you can specify any valid URI for hello **Redirect URI**.</span></span> <span data-ttu-id="463ee-148">Nem kell valódi végpont toobe.</span><span class="sxs-lookup"><span data-stu-id="463ee-148">It does not need toobe a real endpoint.</span></span>

<span data-ttu-id="463ee-149">Az alkalmazás regisztrálása után hello Alkalmazásazonosító jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="463ee-149">After you've registered your application, you'll see hello application ID:</span></span>

![A kötegelt alkalmazás regisztrálása az Azure ad szolgáltatással](./media/batch-aad-auth/app-registration-data-plane.png)

<span data-ttu-id="463ee-151">Egy alkalmazás regisztrálása az Azure ad-vel kapcsolatos további információkért lásd: [hitelesítési forgatókönyvek az Azure AD](../active-directory/develop/active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="463ee-151">For more information about registering an application with Azure AD, see [Authentication Scenarios for Azure AD](../active-directory/develop/active-directory-authentication-scenarios.md).</span></span>

## <a name="get-hello-tenant-id-for-your-active-directory"></a><span data-ttu-id="463ee-152">Az Active Directory hello Bérlőazonosító beszerzése</span><span class="sxs-lookup"><span data-stu-id="463ee-152">Get hello tenant ID for your Active Directory</span></span>

<span data-ttu-id="463ee-153">hello Bérlőazonosító hello Azure AD-bérlő hitelesítési szolgáltatások tooyour alkalmazás biztosító azonosítja.</span><span class="sxs-lookup"><span data-stu-id="463ee-153">hello tenant ID identifies hello Azure AD tenant that provides authentication services tooyour application.</span></span> <span data-ttu-id="463ee-154">tooget hello Bérlőazonosító, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="463ee-154">tooget hello tenant ID, follow these steps:</span></span>

1. <span data-ttu-id="463ee-155">Hello Azure-portálon válassza ki az Active Directory szolgáltatásba.</span><span class="sxs-lookup"><span data-stu-id="463ee-155">In hello Azure portal, select your Active Directory.</span></span>
2. <span data-ttu-id="463ee-156">Kattintson a **Tulajdonságok** elemre.</span><span class="sxs-lookup"><span data-stu-id="463ee-156">Click **Properties**.</span></span>
3. <span data-ttu-id="463ee-157">Másolja át a megadott hello directory azonosítóhoz GUID-érték hello</span><span class="sxs-lookup"><span data-stu-id="463ee-157">Copy hello GUID value provided for hello directory ID.</span></span> <span data-ttu-id="463ee-158">Ez az érték rövidítése hello bérlő azonosítója.</span><span class="sxs-lookup"><span data-stu-id="463ee-158">This value is also called hello tenant ID.</span></span>

![Másolja a hello könyvtár-azonosítója](./media/batch-aad-auth/aad-directory-id.png)


## <a name="use-integrated-authentication"></a><span data-ttu-id="463ee-160">Integrált hitelesítés használata</span><span class="sxs-lookup"><span data-stu-id="463ee-160">Use integrated authentication</span></span>

<span data-ttu-id="463ee-161">tooauthenticate az integrált hitelesítés, az alkalmazás engedélyek tooconnect toohello Batch szolgáltatás API toogrant szüksége.</span><span class="sxs-lookup"><span data-stu-id="463ee-161">tooauthenticate with integrated authentication, you need toogrant your application permissions tooconnect toohello Batch service API.</span></span> <span data-ttu-id="463ee-162">Ez a lépés lehetővé teszi, hogy az alkalmazás tooauthenticate hívások toohello Batch szolgáltatás API az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="463ee-162">This step enables your application tooauthenticate calls toohello Batch service API with Azure AD.</span></span>

<span data-ttu-id="463ee-163">Miután megismerte [az alkalmazás regisztrálva](#register-your-application-with-an-azure-ad-tenant), kövesse az alábbi lépéseket a hello toohello Batch szolgáltatás elérni az Azure portál toogrant:</span><span class="sxs-lookup"><span data-stu-id="463ee-163">Once you've [registered your application](#register-your-application-with-an-azure-ad-tenant), follow these steps in hello Azure portal toogrant it access toohello Batch service:</span></span>

1. <span data-ttu-id="463ee-164">Azure-portálon hello hello bal oldali navigációs ablaktábláján válassza **több szolgáltatások**, kattintson a **App regisztrációk**.</span><span class="sxs-lookup"><span data-stu-id="463ee-164">In hello left-hand navigation pane of hello Azure portal, choose **More Services**, click **App Registrations**.</span></span>
2. <span data-ttu-id="463ee-165">Keresse meg az alkalmazás hello listában app regisztrációk hello nevét:</span><span class="sxs-lookup"><span data-stu-id="463ee-165">Search for hello name of your application in hello list of app registrations:</span></span>

    ![Keresse meg az alkalmazás neve](./media/batch-aad-auth/search-app-registration.png)

3. <span data-ttu-id="463ee-167">Nyissa meg hello **beállítások** panel az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="463ee-167">Open hello **Settings** blade for your application.</span></span> <span data-ttu-id="463ee-168">A hello **API-hozzáférés** szakaszban jelölje be **szükséges engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="463ee-168">In hello **API Access** section, select **Required permissions**.</span></span>
4. <span data-ttu-id="463ee-169">A hello **szükséges engedélyek** panelen kattintson a hello **hozzáadása** gomb.</span><span class="sxs-lookup"><span data-stu-id="463ee-169">In hello **Required permissions** blade, click hello **Add** button.</span></span>
5. <span data-ttu-id="463ee-170">Az 1. lépésben keressen hello kötegelt API.</span><span class="sxs-lookup"><span data-stu-id="463ee-170">In step 1, search for hello Batch API.</span></span> <span data-ttu-id="463ee-171">Ezek a karakterláncok, amíg meg nem látja hello API mindegyikének keresése:</span><span class="sxs-lookup"><span data-stu-id="463ee-171">Search for each of these strings until you find hello API:</span></span>
    1. <span data-ttu-id="463ee-172">**MicrosoftAzureBatch**.</span><span class="sxs-lookup"><span data-stu-id="463ee-172">**MicrosoftAzureBatch**.</span></span>
    2. <span data-ttu-id="463ee-173">**Microsoft Azure Batch**.</span><span class="sxs-lookup"><span data-stu-id="463ee-173">**Microsoft Azure Batch**.</span></span> <span data-ttu-id="463ee-174">Újabb Azure AD-bérlők ezt a nevet használhatják.</span><span class="sxs-lookup"><span data-stu-id="463ee-174">Newer Azure AD tenants may use this name.</span></span>
    3. <span data-ttu-id="463ee-175">**ddbf3205-c6bd-46ae-8127-60eb93363864** hello azonosítója hello kötegelt API.</span><span class="sxs-lookup"><span data-stu-id="463ee-175">**ddbf3205-c6bd-46ae-8127-60eb93363864** is hello ID for hello Batch API.</span></span> 
6. <span data-ttu-id="463ee-176">Miután megtalálta hello kötegelt API, válassza ki azt, majd kattintson a hello **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="463ee-176">Once you find hello Batch API, select it and click hello **Select** button.</span></span>
6. <span data-ttu-id="463ee-177">A 2. lépésben, válassza ki hello jelölőnégyzet mellett túl**Azure Batch szolgáltatás** hello kattintson **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="463ee-177">In step 2, select hello check box next too**Access Azure Batch Service** and click hello **Select** button.</span></span>
7. <span data-ttu-id="463ee-178">Kattintson a hello **végzett** gombra.</span><span class="sxs-lookup"><span data-stu-id="463ee-178">Click hello **Done** button.</span></span>

<span data-ttu-id="463ee-179">Hello **szükséges engedélyek** most azt mutatja, hogy az Azure AD alkalmazás tooboth ADAL eléréséhez, és a Batch szolgáltatás API hello panelen.</span><span class="sxs-lookup"><span data-stu-id="463ee-179">hello **Required Permissions** blade now shows that your Azure AD application has access tooboth ADAL and hello Batch service API.</span></span> <span data-ttu-id="463ee-180">Engedélyekkel tooADAL automatikusan regisztrációt az alkalmazás az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="463ee-180">Permissions are granted tooADAL automatically when you first register your app with Azure AD.</span></span>

![Támogatás API engedélyei](./media/batch-aad-auth/required-permissions-data-plane.png)

## <a name="use-a-service-principal"></a><span data-ttu-id="463ee-182">Egy egyszerű szolgáltatás használata</span><span class="sxs-lookup"><span data-stu-id="463ee-182">Use a service principal</span></span> 

<span data-ttu-id="463ee-183">tooauthenticate felügyelet nélküli futó alkalmazást, használhat egy egyszerű szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="463ee-183">tooauthenticate an application that runs unattended, you use a service principal.</span></span> <span data-ttu-id="463ee-184">Az alkalmazás regisztrálása után kövesse az alábbi lépéseket az Azure portál tooconfigure egy egyszerű szolgáltatásnév hello:</span><span class="sxs-lookup"><span data-stu-id="463ee-184">After you've registered your application, follow these steps in hello Azure portal tooconfigure a service principal:</span></span>

1. <span data-ttu-id="463ee-185">Egy alkalmazás titkos kulcs kérelmet.</span><span class="sxs-lookup"><span data-stu-id="463ee-185">Request a secret key for your application.</span></span>
2. <span data-ttu-id="463ee-186">Az RBAC szerepkör tooyour alkalmazás hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="463ee-186">Assign an RBAC role tooyour application.</span></span>

### <a name="request-a-secret-key-for-your-application"></a><span data-ttu-id="463ee-187">A kérelem egy alkalmazás titkos kulcs</span><span class="sxs-lookup"><span data-stu-id="463ee-187">Request a secret key for your application</span></span>

<span data-ttu-id="463ee-188">Ha az alkalmazás egyszerű szolgáltatás végzi a hitelesítést, küldi hello Alkalmazásazonosítót és a titkos kulcs tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="463ee-188">When your application authenticates with a service principal, it sends both hello application ID and a secret key tooAzure AD.</span></span> <span data-ttu-id="463ee-189">Kell toocreate kell és titkos kulcs toouse hello másolni a kódot.</span><span class="sxs-lookup"><span data-stu-id="463ee-189">You'll need toocreate and copy hello secret key toouse from your code.</span></span>

<span data-ttu-id="463ee-190">Kövesse az alábbi lépéseket a hello Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="463ee-190">Follow these steps in hello Azure portal:</span></span>

1. <span data-ttu-id="463ee-191">Azure-portálon hello hello bal oldali navigációs ablaktábláján válassza **több szolgáltatások**, kattintson a **App regisztrációk**.</span><span class="sxs-lookup"><span data-stu-id="463ee-191">In hello left-hand navigation pane of hello Azure portal, choose **More Services**, click **App Registrations**.</span></span>
2. <span data-ttu-id="463ee-192">Keresse meg az alkalmazás hello listában app regisztrációk hello nevét.</span><span class="sxs-lookup"><span data-stu-id="463ee-192">Search for hello name of your application in hello list of app registrations.</span></span>
3. <span data-ttu-id="463ee-193">Megjelenítési hello **beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="463ee-193">Display hello **Settings** blade.</span></span> <span data-ttu-id="463ee-194">A hello **API-hozzáférés** szakaszban jelölje be **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="463ee-194">In hello **API Access** section, select **Keys**.</span></span>
4. <span data-ttu-id="463ee-195">toocreate egy kulcsot, adja meg a hello kulcs leírását.</span><span class="sxs-lookup"><span data-stu-id="463ee-195">toocreate a key, enter a description for hello key.</span></span> <span data-ttu-id="463ee-196">Ezután válasszon egy időtartamot hello kulcs egy vagy két év.</span><span class="sxs-lookup"><span data-stu-id="463ee-196">Then select a duration for hello key of either one or two years.</span></span> 
5. <span data-ttu-id="463ee-197">Kattintson a hello **mentése** toocreate gombra, és megjeleníti a hello kulcs.</span><span class="sxs-lookup"><span data-stu-id="463ee-197">Click hello **Save** button toocreate and display hello key.</span></span> <span data-ttu-id="463ee-198">Nem fogja tudni tooaccess hello kulcsérték tooa biztonságos helyen, másolja azt követően újra hello panelen hagyja.</span><span class="sxs-lookup"><span data-stu-id="463ee-198">Copy hello key value tooa safe place, as you won't be able tooaccess it again after you leave hello blade.</span></span> 

    ![A titkos kulcs létrehozása](./media/batch-aad-auth/secret-key.png)

### <a name="assign-an-rbac-role-tooyour-application"></a><span data-ttu-id="463ee-200">Az RBAC szerepkör tooyour alkalmazás hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="463ee-200">Assign an RBAC role tooyour application</span></span>

<span data-ttu-id="463ee-201">az egyszerű szolgáltatás tooauthenticate, kell tooassign RBAC szerepkör tooyour kérelmet.</span><span class="sxs-lookup"><span data-stu-id="463ee-201">tooauthenticate with a service principal, you need tooassign an RBAC role tooyour application.</span></span> <span data-ttu-id="463ee-202">Kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="463ee-202">Follow these steps:</span></span>

1. <span data-ttu-id="463ee-203">Hello Azure-portálon keresse meg az alkalmazás által használt toohello Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="463ee-203">In hello Azure portal, navigate toohello Batch account used by your application.</span></span>
2. <span data-ttu-id="463ee-204">A hello **beállítások** hello Batch-fiók, jelölje be a panelt **hozzáférés-vezérlés (IAM)**.</span><span class="sxs-lookup"><span data-stu-id="463ee-204">In hello **Settings** blade for hello Batch account, select **Access Control (IAM)**.</span></span>
3. <span data-ttu-id="463ee-205">Kattintson a hello **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="463ee-205">Click hello **Add** button.</span></span> 
4. <span data-ttu-id="463ee-206">A hello **szerepkör** legördülő listából válassza ki bármelyik hello _közreműködő_ vagy _olvasó_ szerepkör az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="463ee-206">From hello **Role** drop-down, choose either hello _Contributor_ or _Reader_ role for your application.</span></span> <span data-ttu-id="463ee-207">Ezek a szerepkörök további információkért lásd: [Ismerkedés a szerepköralapú hozzáférés-vezérlés az Azure-portálon hello](../active-directory/role-based-access-control-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="463ee-207">For more information on these roles, see [Get started with Role-Based Access Control in hello Azure portal](../active-directory/role-based-access-control-what-is.md).</span></span>  
5. <span data-ttu-id="463ee-208">A hello **válasszon** mezőbe írja be az alkalmazás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="463ee-208">In hello **Select** field, enter hello name of your application.</span></span> <span data-ttu-id="463ee-209">Válassza ki az alkalmazás hello listából, majd kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="463ee-209">Select your application from hello list, and click **Save**.</span></span>

<span data-ttu-id="463ee-210">Az alkalmazás ekkor meg kell jelennie a hozzáférés-vezérlési beállításokkal hozzárendelt RBAC szerepkörrel rendelkező.</span><span class="sxs-lookup"><span data-stu-id="463ee-210">Your application should now appear in your access control settings with an RBAC role assigned.</span></span> 

![Az RBAC szerepkör tooyour alkalmazás hozzárendelése](./media/batch-aad-auth/app-rbac-role.png)

### <a name="get-hello-tenant-id-for-your-azure-active-directory"></a><span data-ttu-id="463ee-212">Az Azure Active Directory hello Bérlőazonosító beszerzése</span><span class="sxs-lookup"><span data-stu-id="463ee-212">Get hello tenant ID for your Azure Active Directory</span></span>

<span data-ttu-id="463ee-213">hello Bérlőazonosító hello Azure AD-bérlő hitelesítési szolgáltatások tooyour alkalmazás biztosító azonosítja.</span><span class="sxs-lookup"><span data-stu-id="463ee-213">hello tenant ID identifies hello Azure AD tenant that provides authentication services tooyour application.</span></span> <span data-ttu-id="463ee-214">tooget hello Bérlőazonosító, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="463ee-214">tooget hello tenant ID, follow these steps:</span></span>

1. <span data-ttu-id="463ee-215">Hello Azure-portálon válassza ki az Active Directory szolgáltatásba.</span><span class="sxs-lookup"><span data-stu-id="463ee-215">In hello Azure portal, select your Active Directory.</span></span>
2. <span data-ttu-id="463ee-216">Kattintson a **Tulajdonságok** elemre.</span><span class="sxs-lookup"><span data-stu-id="463ee-216">Click **Properties**.</span></span>
3. <span data-ttu-id="463ee-217">Másolja át a megadott hello directory azonosítóhoz GUID-érték hello</span><span class="sxs-lookup"><span data-stu-id="463ee-217">Copy hello GUID value provided for hello directory ID.</span></span> <span data-ttu-id="463ee-218">Ez az érték rövidítése hello bérlő azonosítója.</span><span class="sxs-lookup"><span data-stu-id="463ee-218">This value is also called hello tenant ID.</span></span>

![Másolja a hello könyvtár-azonosítója](./media/batch-aad-auth/aad-directory-id.png)


## <a name="code-examples"></a><span data-ttu-id="463ee-220">Kódpéldák</span><span class="sxs-lookup"><span data-stu-id="463ee-220">Code examples</span></span>

<span data-ttu-id="463ee-221">hello ebben a szakaszban szereplő példák megjelenítése az Azure AD használatával tooauthenticate integrált hitelesítés és az egyszerű szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="463ee-221">hello code examples in this section show how tooauthenticate with Azure AD using integrated authentication and with a service principal.</span></span> <span data-ttu-id="463ee-222">A Kódminták .NET használja, de hello fogalmak hasonló, ha a többi nyelvet.</span><span class="sxs-lookup"><span data-stu-id="463ee-222">These code examples use .NET, but hello concepts are similar for other languages.</span></span>

> [!NOTE]
> <span data-ttu-id="463ee-223">Egy Azure AD hitelesítési tokent egy óra múlva lejár.</span><span class="sxs-lookup"><span data-stu-id="463ee-223">An Azure AD authentication token expires after one hour.</span></span> <span data-ttu-id="463ee-224">A hosszú élettartamú használatakor **BatchClient** objektum, azt javasoljuk, hogy a jogkivonat lekérése az ADAL az összes kérelem tooensure mindig van egy érvényes tokent.</span><span class="sxs-lookup"><span data-stu-id="463ee-224">When using a long-lived **BatchClient** object, we recommend that you retrieve a token from ADAL on every request tooensure you always have a valid token.</span></span> 
>
>
> <span data-ttu-id="463ee-225">tooachieve a .NET, ez egy olyan metódus írása, hogy beolvassa hello Azure ad-token, és adja át az adott metódus tooa **BatchTokenCredentials** meghatalmazottként objektum.</span><span class="sxs-lookup"><span data-stu-id="463ee-225">tooachieve this in .NET, write a method that retrieves hello token from Azure AD and pass that method tooa **BatchTokenCredentials** object as a delegate.</span></span> <span data-ttu-id="463ee-226">hello delegáltmetódus minden kérelem toohello Batch szolgáltatás tooensure, amely egy érvényes jogkivonat van megadva a neve.</span><span class="sxs-lookup"><span data-stu-id="463ee-226">hello delegate method is called on every request toohello Batch service tooensure that a valid token is provided.</span></span> <span data-ttu-id="463ee-227">Alapértelmezés szerint a ADAL jogkivonatokat, gyorsítótárazza, így egy új jogkivonatot az Azure AD kéri le a rendszer csak akkor, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="463ee-227">By default ADAL caches tokens, so a new token is retrieved from Azure AD only when necessary.</span></span> <span data-ttu-id="463ee-228">Az Azure AD-jogkivonatokat kapcsolatos további információkért lásd: [hitelesítési forgatókönyvek az Azure AD][aad_auth_scenarios].</span><span class="sxs-lookup"><span data-stu-id="463ee-228">For more information about tokens in Azure AD, see [Authentication Scenarios for Azure AD][aad_auth_scenarios].</span></span>
>
>

### <a name="code-example-using-azure-ad-integrated-authentication-with-batch-net"></a><span data-ttu-id="463ee-229">Példa: az Azure AD hitelesítési integrálva Batch .NET</span><span class="sxs-lookup"><span data-stu-id="463ee-229">Code example: Using Azure AD integrated authentication with Batch .NET</span></span>

<span data-ttu-id="463ee-230">a Batch .NET-, referencia hello integrált hitelesítés tooauthenticate [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) csomag- és hello [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) csomag.</span><span class="sxs-lookup"><span data-stu-id="463ee-230">tooauthenticate with integrated authentication from Batch .NET, reference hello [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) package and hello [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) package.</span></span>

<span data-ttu-id="463ee-231">Adja meg a következőket hello `using` utasítások a kódban:</span><span class="sxs-lookup"><span data-stu-id="463ee-231">Include hello following `using` statements in your code:</span></span>

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

<span data-ttu-id="463ee-232">Az Azure AD-végpont a kódban, beleértve a hello hivatkozás hello bérlői azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="463ee-232">Reference hello Azure AD endpoint in your code, including hello tenant ID.</span></span> <span data-ttu-id="463ee-233">tooretrieve hello Bérlőazonosító, hello című rész lépéseit követve [hello bérlői azonosító beszerzése az Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span><span class="sxs-lookup"><span data-stu-id="463ee-233">tooretrieve hello tenant ID, follow hello steps outlined in [Get hello tenant ID for your Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span></span>

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

<span data-ttu-id="463ee-234">Referencia erőforrás hello kötegelt szolgáltatásvégpont:</span><span class="sxs-lookup"><span data-stu-id="463ee-234">Reference hello Batch service resource endpoint:</span></span>

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

<span data-ttu-id="463ee-235">A Batch-fiók hivatkozási:</span><span class="sxs-lookup"><span data-stu-id="463ee-235">Reference your Batch account:</span></span>

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

<span data-ttu-id="463ee-236">Adja meg a hello alkalmazás Azonosítóját (ügyfél-azonosító) az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="463ee-236">Specify hello application ID (client ID) for your application.</span></span> <span data-ttu-id="463ee-237">az alkalmazás hello Azure-portálon való regisztráció a hello Alkalmazásazonosító érhető el:</span><span class="sxs-lookup"><span data-stu-id="463ee-237">hello application ID is available from your app registration in hello Azure portal:</span></span>

```csharp
private const string ClientId = "<application-id>";
```

<span data-ttu-id="463ee-238">Hello átirányítási URI-t hello regisztrációs folyamat során megadott szeretne másolni.</span><span class="sxs-lookup"><span data-stu-id="463ee-238">Also copy hello redirect URI that you specified during hello registration process.</span></span> <span data-ttu-id="463ee-239">az átirányítási URI a kódban megadott meg kell egyeznie a hello átirányítási URI-t hello alkalmazás regisztrálásakor megadott hello:</span><span class="sxs-lookup"><span data-stu-id="463ee-239">hello redirect URI specified in your code must match hello redirect URI that you provided when you registered hello application:</span></span>

```csharp
private const string RedirectUri = "http://mybatchdatasample";
```

<span data-ttu-id="463ee-240">A visszahívási metódus tooacquire hello hitelesítésére szolgáló jogkivonat írni az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="463ee-240">Write a callback method tooacquire hello authentication token from Azure AD.</span></span> <span data-ttu-id="463ee-241">Hello **GetAuthenticationTokenAsync** itt látható visszahívási metódus meghívja ADAL tooauthenticate a felhasználó, aki hello alkalmazás dolgozik.</span><span class="sxs-lookup"><span data-stu-id="463ee-241">hello **GetAuthenticationTokenAsync** callback method shown here calls ADAL tooauthenticate a user who is interacting with hello application.</span></span> <span data-ttu-id="463ee-242">Hello **AcquireTokenAsync** metódus által adal-t kér hello felhasználói a hitelesítő adatait, és hello alkalmazás halad egyszer hello felhasználói eszközeivel (kivéve, ha már gyorsítótárazott hitelesítő adatok):</span><span class="sxs-lookup"><span data-stu-id="463ee-242">hello **AcquireTokenAsync** method provided by ADAL prompts hello user for their credentials, and hello application proceeds once hello user provides them (unless it has already cached credentials):</span></span>

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    var authContext = new AuthenticationContext(AuthorityUri);

    // Acquire hello authentication token from Azure AD.
    var authResult = await authContext.AcquireTokenAsync(BatchResourceUri, 
                                                        ClientId, 
                                                        new Uri(RedirectUri), 
                                                        new PlatformParameters(PromptBehavior.Auto));

    return authResult.AccessToken;
}
```

<span data-ttu-id="463ee-243">Összeállíthatja a **BatchTokenCredentials** objektum, amely hello delegált paramétert fogad.</span><span class="sxs-lookup"><span data-stu-id="463ee-243">Construct a **BatchTokenCredentials** object that takes hello delegate as a parameter.</span></span> <span data-ttu-id="463ee-244">Használja ezeket a hitelesítő adatok tooopen egy **BatchClient** objektum.</span><span class="sxs-lookup"><span data-stu-id="463ee-244">Use those credentials tooopen a **BatchClient** object.</span></span> <span data-ttu-id="463ee-245">Is használhatja, amely **BatchClient** objektum a Batch szolgáltatás hello további műveleteket:</span><span class="sxs-lookup"><span data-stu-id="463ee-245">You can use that **BatchClient** object for subsequent operations against hello Batch service:</span></span>

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

### <a name="code-example-using-an-azure-ad-service-principal-with-batch-net"></a><span data-ttu-id="463ee-246">Példa: az Azure AD szolgáltatás egyszerű Batch .NET használatával</span><span class="sxs-lookup"><span data-stu-id="463ee-246">Code example: Using an Azure AD service principal with Batch .NET</span></span>

<span data-ttu-id="463ee-247">az egyszerű szolgáltatás a Batch .NET-, referencia hello tooauthenticate [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) csomag- és hello [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) csomag.</span><span class="sxs-lookup"><span data-stu-id="463ee-247">tooauthenticate with a service principal from Batch .NET, reference hello [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) package and hello [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) package.</span></span>

<span data-ttu-id="463ee-248">Adja meg a következőket hello `using` utasítások a kódban:</span><span class="sxs-lookup"><span data-stu-id="463ee-248">Include hello following `using` statements in your code:</span></span>

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

<span data-ttu-id="463ee-249">Az Azure AD-végpont a kódban, beleértve a hello hivatkozás hello bérlői azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="463ee-249">Reference hello Azure AD endpoint in your code, including hello tenant ID.</span></span> <span data-ttu-id="463ee-250">Egy egyszerű szolgáltatás használata esetén meg kell adnia egy bérlő-specifikus végpontot.</span><span class="sxs-lookup"><span data-stu-id="463ee-250">When using a service principal, you must provide a tenant-specific endpoint.</span></span> <span data-ttu-id="463ee-251">tooretrieve hello Bérlőazonosító, hello című rész lépéseit követve [hello bérlői azonosító beszerzése az Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span><span class="sxs-lookup"><span data-stu-id="463ee-251">tooretrieve hello tenant ID, follow hello steps outlined in [Get hello tenant ID for your Azure Active Directory](#get-the-tenant-id-for-your-active-directory):</span></span>

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

<span data-ttu-id="463ee-252">Referencia erőforrás hello kötegelt szolgáltatásvégpont:</span><span class="sxs-lookup"><span data-stu-id="463ee-252">Reference hello Batch service resource endpoint:</span></span>  

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

<span data-ttu-id="463ee-253">A Batch-fiók hivatkozási:</span><span class="sxs-lookup"><span data-stu-id="463ee-253">Reference your Batch account:</span></span>

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

<span data-ttu-id="463ee-254">Adja meg a hello alkalmazás Azonosítóját (ügyfél-azonosító) az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="463ee-254">Specify hello application ID (client ID) for your application.</span></span> <span data-ttu-id="463ee-255">az alkalmazás hello Azure-portálon való regisztráció a hello Alkalmazásazonosító érhető el:</span><span class="sxs-lookup"><span data-stu-id="463ee-255">hello application ID is available from your app registration in hello Azure portal:</span></span>

```csharp
private const string ClientId = "<application-id>";
```

<span data-ttu-id="463ee-256">Adja meg a titkos kulcs hello hello Azure-portálon fájlból másolt:</span><span class="sxs-lookup"><span data-stu-id="463ee-256">Specify hello secret key that you copied from hello Azure portal:</span></span>

```csharp
private const string ClientKey = "<secret-key>";
```

<span data-ttu-id="463ee-257">A visszahívási metódus tooacquire hello hitelesítésére szolgáló jogkivonat írni az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="463ee-257">Write a callback method tooacquire hello authentication token from Azure AD.</span></span> <span data-ttu-id="463ee-258">Hello **GetAuthenticationTokenAsync** visszahívási metódus hívások az adal TÁRAT a felügyelet nélküli hitelesítéshez itt látható:</span><span class="sxs-lookup"><span data-stu-id="463ee-258">hello **GetAuthenticationTokenAsync** callback method shown here calls ADAL for unattended authentication:</span></span>

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
    AuthenticationResult authResult = await authContext.AcquireTokenAsync(BatchResourceUri, new ClientCredential(ClientId, ClientKey));

    return authResult.AccessToken;
}
```

<span data-ttu-id="463ee-259">Összeállíthatja a **BatchTokenCredentials** objektum, amely hello delegált paramétert fogad.</span><span class="sxs-lookup"><span data-stu-id="463ee-259">Construct a **BatchTokenCredentials** object that takes hello delegate as a parameter.</span></span> <span data-ttu-id="463ee-260">Használja ezeket a hitelesítő adatok tooopen egy **BatchClient** objektum.</span><span class="sxs-lookup"><span data-stu-id="463ee-260">Use those credentials tooopen a **BatchClient** object.</span></span> <span data-ttu-id="463ee-261">Ezután használhatja, amely **BatchClient** objektum a Batch szolgáltatás hello további műveleteket:</span><span class="sxs-lookup"><span data-stu-id="463ee-261">You can then use that **BatchClient** object for subsequent operations against hello Batch service:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="463ee-262">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="463ee-262">Next steps</span></span>

<span data-ttu-id="463ee-263">További információ az Azure AD toolearn lásd: hello [Azure Active Directory dokumentációjának](https://docs.microsoft.com/azure/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="463ee-263">toolearn more about Azure AD, see hello [Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span> <span data-ttu-id="463ee-264">Hogyan toouse ADAL találhatók hello bemutató részletes példák [Azure mintakódok](https://azure.microsoft.com/resources/samples/?service=active-directory) könyvtár.</span><span class="sxs-lookup"><span data-stu-id="463ee-264">In-depth examples showing how toouse ADAL are available in hello [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=active-directory) library.</span></span>

<span data-ttu-id="463ee-265">toolearn szolgáltatásnevekről, kapcsolatos további információkért lásd: [alkalmazás és szolgáltatás egyszerű objektumok az Azure Active Directoryban](../active-directory/develop/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="463ee-265">toolearn more about service principals, see [Application and service principal objects in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span></span> <span data-ttu-id="463ee-266">a szolgáltatás egyszerű használatával toocreate hello Azure portál, lásd: [portál toocreate Active Directory-alkalmazást és egy egyszerű szolgáltatást, amely erőforrások eléréséhez használjon](../resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="463ee-266">toocreate a service principal using hello Azure portal, see [Use portal toocreate Active Directory application and service principal that can access resources](../resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="463ee-267">A szolgáltatás egyszerű PowerShell vagy az Azure parancssori felület is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="463ee-267">You can also create a service principal with PowerShell or Azure CLI.</span></span> 

<span data-ttu-id="463ee-268">tooauthenticate kötegelt felügyeleti alkalmazások az Azure AD, lásd: [hitelesítéséhez kötegelt megoldásokat az Active Directory](batch-aad-auth-management.md).</span><span class="sxs-lookup"><span data-stu-id="463ee-268">tooauthenticate Batch Management applications using Azure AD, see [Authenticate Batch Management solutions with Active Directory](batch-aad-auth-management.md).</span></span> 

[aad_about]: ../active-directory/active-directory-whatis.md "Mi az Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Az Azure Active Directory hitelesítési forgatókönyvei"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Alkalmazások integrálása az Azure Active Directoryval"
[azure_portal]: http://portal.azure.com
