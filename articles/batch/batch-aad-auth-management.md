---
title: "Azure Active Directory segítségével hitelesítheti a kötegelt megoldások |} Microsoft Docs"
description: "Alkalmazások az Azure resource manager-val készült, és az Azure AD hitelesíti a kötegelt erőforrás-szolgáltató."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/27/2017
ms.author: tamram
ms.openlocfilehash: 26d4adf4f74f9aacc4cf8cf24be293ebdb4d63c8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="authenticate-batch-management-solutions-with-active-directory"></a><span data-ttu-id="51e07-103">Hitelesítés kötegelt megoldásokat az Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="51e07-103">Authenticate Batch Management solutions with Active Directory</span></span>

<span data-ttu-id="51e07-104">A hitelesítést az Azure Batch Management szolgáltatás alkalmazások [Azure Active Directory] [ aad_about] (az Azure AD).</span><span class="sxs-lookup"><span data-stu-id="51e07-104">Applications that call the Azure Batch Management service authenticate with [Azure Active Directory][aad_about] (Azure AD).</span></span> <span data-ttu-id="51e07-105">Az Azure AD a Microsoft több-bérlős felhőalapú címtár és az identity management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="51e07-105">Azure AD is Microsoft’s multi-tenant cloud based directory and identity management service.</span></span> <span data-ttu-id="51e07-106">Azure magát, a a felhasználók, a szolgáltatás-rendszergazdák és a szervezeti felhasználók az Azure AD használ.</span><span class="sxs-lookup"><span data-stu-id="51e07-106">Azure itself uses Azure AD for the authentication of its customers, service administrators, and organizational users.</span></span>

<span data-ttu-id="51e07-107">A Batch Management .NET kódtár típusok kötegelt fiókok, kulcsait, alkalmazások és csomagok való munkához tesz elérhetővé.</span><span class="sxs-lookup"><span data-stu-id="51e07-107">The Batch Management .NET library exposes types for working with Batch accounts, account keys, applications, and application packages.</span></span> <span data-ttu-id="51e07-108">A Batch Management .NET könyvtár az Azure-erőforrás-szolgáltató ügyfél, és együtt használni [Azure Resource Manager] [ resman_overview] programozott módon kezelheti ezeket az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="51e07-108">The Batch Management .NET library is an Azure resource provider client, and is used together with [Azure Resource Manager][resman_overview] to manage these resources programmatically.</span></span> <span data-ttu-id="51e07-109">Az Azure AD bármely Azure-erőforrás szolgáltató ügyfél, beleértve a Batch Management .NET kódtár és révén kérelmek hitelesítéséhez szükséges [Azure Resource Manager][resman_overview].</span><span class="sxs-lookup"><span data-stu-id="51e07-109">Azure AD is required to authenticate requests made through any Azure resource provider client, including the Batch Management .NET library, and through [Azure Resource Manager][resman_overview].</span></span>

<span data-ttu-id="51e07-110">Ebben a cikkben azt megismerkedhet az Azure AD a Batch Management .NET kódtárral libraryt használó alkalmazások hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="51e07-110">In this article, we explore using Azure AD to authenticate from applications that use the Batch Management .NET library.</span></span> <span data-ttu-id="51e07-111">Megmutatjuk, hogyan egy előfizetés rendszergazdának vagy társadminisztrátornak, integrált hitelesítés hitelesítéséhez az Azure AD használja.</span><span class="sxs-lookup"><span data-stu-id="51e07-111">We show how to use Azure AD to authenticate a subscription administrator or co-administrator, using integrated authentication.</span></span> <span data-ttu-id="51e07-112">Használjuk a [AccountManagment] [ acct_mgmt_sample] mintaprojektet, elérhető a Githubon, az Azure AD a Batch Management .NET kódtárral könyvtárhoz bízná.</span><span class="sxs-lookup"><span data-stu-id="51e07-112">We use the [AccountManagment][acct_mgmt_sample] sample project, available on GitHub, to walk through using Azure AD with the Batch Management .NET library.</span></span>

<span data-ttu-id="51e07-113">A Batch Management .NET kódtárral és a AccountManagement minta használatával kapcsolatos további tudnivalókért lásd: [kezelése Batch fiókjainak és kvótáinak, a Batch Management ügyféloldali kódtára a .NET](batch-management-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="51e07-113">To learn more about using the Batch Management .NET library and the AccountManagement sample, see [Manage Batch accounts and quotas with the Batch Management client library for .NET](batch-management-dotnet.md).</span></span>

## <a name="register-your-application-with-azure-ad"></a><span data-ttu-id="51e07-114">Az alkalmazás regisztrálása az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="51e07-114">Register your application with Azure AD</span></span>

<span data-ttu-id="51e07-115">Az Azure [Active Directory Authentication Library] [ aad_adal] (ADAL) programozott felülete lehetőséget nyújt az Azure AD az alkalmazások belüli használatra.</span><span class="sxs-lookup"><span data-stu-id="51e07-115">The Azure [Active Directory Authentication Library][aad_adal] (ADAL) provides a programmatic interface to Azure AD for use within your applications.</span></span> <span data-ttu-id="51e07-116">Az alkalmazás adal-t hívja, regisztrálnia kell az alkalmazást az Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="51e07-116">To call ADAL from your application, you must register your application in an Azure AD tenant.</span></span> <span data-ttu-id="51e07-117">Ha a regisztrálnia az alkalmazást, adja meg az adatokat az Azure AD az alkalmazásról, többek között a nevét, az Azure AD-bérlő belül.</span><span class="sxs-lookup"><span data-stu-id="51e07-117">When you register your application, you supply Azure AD with information about your application, including a name for it within the Azure AD tenant.</span></span> <span data-ttu-id="51e07-118">Az Azure AD majd biztosít, amelyekkel az alkalmazás társítása az Azure AD futásidőben Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="51e07-118">Azure AD then provides an application ID that you use to associate your application with Azure AD at runtime.</span></span> <span data-ttu-id="51e07-119">Az Alkalmazásazonosító kapcsolatos további információkért lásd: [alkalmazás és szolgáltatás egyszerű objektumok az Azure Active Directoryban](../active-directory/develop/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="51e07-119">To learn more about the application ID, see [Application and service principal objects in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span></span>

<span data-ttu-id="51e07-120">A AccountManagement mintaalkalmazás regisztrálásához kövesse a [egy alkalmazás hozzáadása](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) szakasz [alkalmazások integrálása az Azure Active Directory] [ aad_integrate].</span><span class="sxs-lookup"><span data-stu-id="51e07-120">To register the AccountManagement sample application, follow the steps in the [Adding an Application](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) section in [Integrating applications with Azure Active Directory][aad_integrate].</span></span> <span data-ttu-id="51e07-121">Adja meg **natív ügyfélalkalmazás** az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="51e07-121">Specify **Native Client Application** for the type of application.</span></span> <span data-ttu-id="51e07-122">Az iparági szabványos OAuth 2.0 URI-Azonosítóját a **átirányítási URI-** van `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="51e07-122">The industry standard OAuth 2.0 URI for the **Redirect URI** is `urn:ietf:wg:oauth:2.0:oob`.</span></span> <span data-ttu-id="51e07-123">Azonban megadhat bármilyen érvényes URI-azonosító (például `http://myaccountmanagementsample`) az a **átirányítási URI-**, mert nem kell a tényleges végpontnak lennie:</span><span class="sxs-lookup"><span data-stu-id="51e07-123">However, you can specify any valid URI (such as `http://myaccountmanagementsample`) for the **Redirect URI**, as it does not need to be a real endpoint:</span></span>

![](./media/batch-aad-auth-management/app-registration-management-plane.png)

<span data-ttu-id="51e07-124">A regisztrációs folyamat befejezése után megjelenik az alkalmazás Azonosítóját és az alkalmazás felsorolt Objektumazonosító (egyszerű szolgáltatásnév).</span><span class="sxs-lookup"><span data-stu-id="51e07-124">Once you complete the registration process, you'll see the application ID and the object (service principal) ID listed for your application.</span></span>  

![](./media/batch-aad-auth-management/app-registration-client-id.png)

## <a name="grant-the-azure-resource-manager-api-access-to-your-application"></a><span data-ttu-id="51e07-125">Az Azure Resource Manager API hozzáférést biztosíthat az alkalmazás</span><span class="sxs-lookup"><span data-stu-id="51e07-125">Grant the Azure Resource Manager API access to your application</span></span>

<span data-ttu-id="51e07-126">Ezt követően az alkalmazás az Azure Resource Manager API-hoz való hozzáférés delegálására lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="51e07-126">Next, you'll need to delegate access to your application to the Azure Resource Manager API.</span></span> <span data-ttu-id="51e07-127">A Resource Manager API-hoz az Azure AD-azonosító **Windows Azure szolgáltatásfelügyeleti API**.</span><span class="sxs-lookup"><span data-stu-id="51e07-127">The Azure AD identifier for the Resource Manager API is **Windows Azure Service Management API**.</span></span>

<span data-ttu-id="51e07-128">Kövesse az alábbi lépéseket az Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="51e07-128">Follow these steps in the Azure portal:</span></span>

1. <span data-ttu-id="51e07-129">Az Azure portál bal oldali navigációs ablaktábláján válassza **több szolgáltatások**, kattintson a **App regisztrációk**, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="51e07-129">In the left-hand navigation pane of the Azure portal, choose **More Services**, click **App Registrations**, and click **Add**.</span></span>
2. <span data-ttu-id="51e07-130">Keresse meg az alkalmazás regisztrációk a listában az alkalmazás nevét:</span><span class="sxs-lookup"><span data-stu-id="51e07-130">Search for the name of your application in the list of app registrations:</span></span>

    ![Keresse meg az alkalmazás neve](./media/batch-aad-auth-management/search-app-registration.png)

3. <span data-ttu-id="51e07-132">Megjelenítés a **beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="51e07-132">Display the **Settings** blade.</span></span> <span data-ttu-id="51e07-133">Az a **API-hozzáférés** szakaszban jelölje be **szükséges engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="51e07-133">In the **API Access** section, select **Required permissions**.</span></span>
4. <span data-ttu-id="51e07-134">Kattintson a **Hozzáadás** hozzáadása egy új szükséges engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="51e07-134">Click **Add** to add a new required permission.</span></span> 
5. <span data-ttu-id="51e07-135">Adja meg az 1. lépésben **Windows Azure szolgáltatásfelügyeleti API**, jelölje ki, hogy az API az eredménylistában, majd kattintson a **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="51e07-135">In step 1, enter **Windows Azure Service Management API**, select that API from the list of results, and click the **Select** button.</span></span>
6. <span data-ttu-id="51e07-136">A 2. lépésben, jelölje be a jelölőnégyzetet a **Access Azure klasszikus üzembe helyezési modellel, mint a szervezeti felhasználók**, és kattintson a **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="51e07-136">In step 2, select the check box next to **Access Azure classic deployment model as organization users**, and click the **Select** button.</span></span>
7. <span data-ttu-id="51e07-137">Kattintson a **végzett** gombra.</span><span class="sxs-lookup"><span data-stu-id="51e07-137">Click the **Done** button.</span></span>

<span data-ttu-id="51e07-138">A **szükséges engedélyek** panelen látható, hogy az alkalmazás engedélyekkel az adal-t és a Resource Manager API-k.</span><span class="sxs-lookup"><span data-stu-id="51e07-138">The **Required Permissions** blade now shows that permissions to your application are granted to both the ADAL and Resource Manager APIs.</span></span> <span data-ttu-id="51e07-139">Engedélyekkel az adal-ra alapértelmezés szerint amikor az alkalmazás regisztrálására az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="51e07-139">Permissions are granted to ADAL by default when you first register your app with Azure AD.</span></span>

![Az Azure Resource Manager API engedélyének delegálása](./media/batch-aad-auth-management/required-permissions-management-plane.png)

## <a name="azure-ad-endpoints"></a><span data-ttu-id="51e07-141">Azure AD-végpontok</span><span class="sxs-lookup"><span data-stu-id="51e07-141">Azure AD endpoints</span></span>

<span data-ttu-id="51e07-142">A kötegelt megoldásokat az Azure AD hitelesíti, két jól ismert végpontok lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="51e07-142">To authenticate your Batch Management solutions with Azure AD, you'll need two well-known endpoints.</span></span>

- <span data-ttu-id="51e07-143">A **az Azure AD közös végpont** összegyűjtéséhez felület egy adott bérlő nem ad meg, amikor integrált hitelesítés esetében általános hitelesítő adatokat biztosít:</span><span class="sxs-lookup"><span data-stu-id="51e07-143">The **Azure AD common endpoint** provides a generic credential gathering interface when a specific tenant is not provided, as in the case of integrated authentication:</span></span>

    `https://login.microsoftonline.com/common`

- <span data-ttu-id="51e07-144">A **Azure Resource Manager-végpont** segítségével szerezzen be egy tokent a Batch szolgáltatás kérelmek hitelesítéséhez:</span><span class="sxs-lookup"><span data-stu-id="51e07-144">The **Azure Resource Manager endpoint** is used to acquire a token for authenticating requests to the Batch management service:</span></span>

    `https://management.core.windows.net/`

<span data-ttu-id="51e07-145">A AccountManagement mintaalkalmazás állandókat ezeket a végpontokat határoz meg.</span><span class="sxs-lookup"><span data-stu-id="51e07-145">The AccountManagement sample application defines constants for these endpoints.</span></span> <span data-ttu-id="51e07-146">Ezek állandók változatlanul hagyja:</span><span class="sxs-lookup"><span data-stu-id="51e07-146">Leave these constants unchanged:</span></span>

```csharp
// Azure Active Directory "common" endpoint.
private const string AuthorityUri = "https://login.microsoftonline.com/common";
// Azure Resource Manager endpoint 
private const string ResourceUri = "https://management.core.windows.net/";
```

## <a name="reference-your-application-id"></a><span data-ttu-id="51e07-147">Az Alkalmazásazonosító hivatkozik</span><span class="sxs-lookup"><span data-stu-id="51e07-147">Reference your application ID</span></span> 

<span data-ttu-id="51e07-148">Az ügyfélalkalmazást (más néven az ügyfél-azonosító) Alkalmazásazonosítót használ a futási időben az Azure AD eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="51e07-148">Your client application uses the application ID (also referred to as the client ID) to access Azure AD at runtime.</span></span> <span data-ttu-id="51e07-149">Az alkalmazás már regisztrált az Azure portálon, frissíti a kódot, és az Azure ad-val a regisztrált alkalmazás által biztosított Alkalmazásazonosítót használja.</span><span class="sxs-lookup"><span data-stu-id="51e07-149">Once you've registered your application in the Azure portal, update your code to use the application ID provided by Azure AD for your registered application.</span></span> <span data-ttu-id="51e07-150">A AccountManagement mintaalkalmazást másolja az alkalmazás azonosítója az Azure-portálon a megfelelő állandó:</span><span class="sxs-lookup"><span data-stu-id="51e07-150">In the AccountManagement sample application, copy your application ID from the Azure portal to the appropriate constant:</span></span>

```csharp
// Specify the unique identifier (the "Client ID") for your application. This is required so that your
// native client application (i.e. this sample) can access the Microsoft Azure AD Graph API. For information
// about registering an application in Azure Active Directory, please see "Adding an Application" here:
// https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
private const string ClientId = "<application-id>";
```
<span data-ttu-id="51e07-151">Az átirányítási URI-t a regisztráció során megadott szeretne másolni.</span><span class="sxs-lookup"><span data-stu-id="51e07-151">Also copy the redirect URI that you specified during the registration process.</span></span> <span data-ttu-id="51e07-152">Az átirányítási URI a kódban megadott meg kell egyeznie az átirányítási URI-t az alkalmazás regisztrálásakor kapott.</span><span class="sxs-lookup"><span data-stu-id="51e07-152">The redirect URI specified in your code must match the redirect URI that you provided when you registered the application.</span></span>

```csharp
// The URI to which Azure AD will redirect in response to an OAuth 2.0 request. This value is
// specified by you when you register an application with AAD (see ClientId comment). It does not
// need to be a real endpoint, but must be a valid URI (e.g. https://accountmgmtsampleapp).
private const string RedirectUri = "http://myaccountmanagementsample";
```

## <a name="acquire-an-azure-ad-authentication-token"></a><span data-ttu-id="51e07-153">Az Azure AD hitelesítési jogkivonat</span><span class="sxs-lookup"><span data-stu-id="51e07-153">Acquire an Azure AD authentication token</span></span>

<span data-ttu-id="51e07-154">Miután a AccountManagement minta regisztrálni kell az Azure AD bérlője, és frissítse az adatforrás példakód az értékek, a minta készen áll a hitelesítés az Azure AD használatával.</span><span class="sxs-lookup"><span data-stu-id="51e07-154">After you register the AccountManagement sample in the Azure AD tenant and update the sample source code with your values, the sample is ready to authenticate using Azure AD.</span></span> <span data-ttu-id="51e07-155">A minta futtatásakor az ADAL megkísérli egy hitelesítési jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="51e07-155">When you run the sample, the ADAL attempts to acquire an authentication token.</span></span> <span data-ttu-id="51e07-156">Ebben a lépésben megkérdezi a Microsoft hitelesítő adatait:</span><span class="sxs-lookup"><span data-stu-id="51e07-156">At this step, it prompts you for your Microsoft credentials:</span></span> 

```csharp
// Obtain an access token using the "common" AAD resource. This allows the application
// to query AAD for information that lies outside the application's tenant (such as for
// querying subscription information in your Azure account).
AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
AuthenticationResult authResult = authContext.AcquireToken(ResourceUri,
                                                        ClientId,
                                                        new Uri(RedirectUri),
                                                        PromptBehavior.Auto);
```

<span data-ttu-id="51e07-157">Miután megadta a hitelesítő adatait, a mintaalkalmazást a hitelesített kérelmeket kiadni a Batch szolgáltatás lépne.</span><span class="sxs-lookup"><span data-stu-id="51e07-157">After you provide your credentials, the sample application can proceed to issue authenticated requests to the Batch management service.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="51e07-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="51e07-158">Next steps</span></span>

<span data-ttu-id="51e07-159">További információ a futó a [AccountManagement mintaalkalmazás][acct_mgmt_sample], lásd: [kezelése Batch fiókjainak és kvótáinak, a Batch Management ügyféloldali kódtára a .NET](batch-management-dotnet.md) .</span><span class="sxs-lookup"><span data-stu-id="51e07-159">For more information on running the [AccountManagement sample application][acct_mgmt_sample], see [Manage Batch accounts and quotas with the Batch Management client library for .NET](batch-management-dotnet.md).</span></span>

<span data-ttu-id="51e07-160">Az Azure AD kapcsolatos további tudnivalókért tekintse meg a [Azure Active Directory dokumentációjának](https://docs.microsoft.com/azure/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="51e07-160">To learn more about Azure AD, see the [Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span> <span data-ttu-id="51e07-161">Részletes példa bemutatja, hogyan adal-t használó érhetők el a [Azure mintakódok](https://azure.microsoft.com/resources/samples/?service=active-directory) könyvtár.</span><span class="sxs-lookup"><span data-stu-id="51e07-161">In-depth examples showing how to use ADAL are available in the [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=active-directory) library.</span></span>

<span data-ttu-id="51e07-162">A Batch szolgáltatás alkalmazások az Azure AD hitelesítéshez lásd: [hitelesítéséhez kötegelt szolgáltatási megoldások és az Active Directory](batch-aad-auth.md).</span><span class="sxs-lookup"><span data-stu-id="51e07-162">To authenticate Batch service applications using Azure AD, see [Authenticate Batch service solutions with Active Directory](batch-aad-auth.md).</span></span> 


<span data-ttu-id="51e07-163">[aad_about]: ../active-directory/active-directory-whatis.md "Mi az Azure Active Directory?"</span><span class="sxs-lookup"><span data-stu-id="51e07-163">[aad_about]: ../active-directory/active-directory-whatis.md "What is Azure Active Directory?"</span></span>
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
<span data-ttu-id="51e07-164">[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Az Azure Active Directory hitelesítési forgatókönyvei"</span><span class="sxs-lookup"><span data-stu-id="51e07-164">[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Authentication Scenarios for Azure AD"</span></span>
<span data-ttu-id="51e07-165">[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Alkalmazások integrálása az Azure Active Directoryval"</span><span class="sxs-lookup"><span data-stu-id="51e07-165">[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrating Applications with Azure Active Directory"</span></span>
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[azure_portal]: http://portal.azure.com
[resman_overview]: ../azure-resource-manager/resource-group-overview.md
