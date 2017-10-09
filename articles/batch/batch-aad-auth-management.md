---
title: "aaaUse Azure Active Directory tooauthenticate kötegelt megoldások |} Microsoft Docs"
description: "Az Azure resource manager-val készült alkalmazások, és az Azure AD hitelesíti hello kötegelt erőforrás-szolgáltató."
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
ms.openlocfilehash: 192aa9f8d7cbfc0282a4a0c33ab1659f1f351525
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-batch-management-solutions-with-active-directory"></a><span data-ttu-id="52214-103">Hitelesítés kötegelt megoldásokat az Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="52214-103">Authenticate Batch Management solutions with Active Directory</span></span>

<span data-ttu-id="52214-104">A hitelesítést hello Azure Batch Management szolgáltatás alkalmazások [Azure Active Directory] [ aad_about] (az Azure AD).</span><span class="sxs-lookup"><span data-stu-id="52214-104">Applications that call hello Azure Batch Management service authenticate with [Azure Active Directory][aad_about] (Azure AD).</span></span> <span data-ttu-id="52214-105">Az Azure AD a Microsoft több-bérlős felhőalapú címtár és az identity management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="52214-105">Azure AD is Microsoft’s multi-tenant cloud based directory and identity management service.</span></span> <span data-ttu-id="52214-106">Azure hello hitelesítésére az ügyfelek, a szolgáltatás-rendszergazdák és a szervezeti felhasználók az Azure AD használ a saját magát.</span><span class="sxs-lookup"><span data-stu-id="52214-106">Azure itself uses Azure AD for hello authentication of its customers, service administrators, and organizational users.</span></span>

<span data-ttu-id="52214-107">hello Batch Management .NET kódtár típusok kötegelt fiókok, kulcsait, alkalmazások és csomagok tesz elérhetővé.</span><span class="sxs-lookup"><span data-stu-id="52214-107">hello Batch Management .NET library exposes types for working with Batch accounts, account keys, applications, and application packages.</span></span> <span data-ttu-id="52214-108">hello Batch Management .NET könyvtár az Azure-erőforrás-szolgáltató ügyfél, és együtt használni [Azure Resource Manager] [ resman_overview] toomanage ezeket az erőforrásokat programozott módon.</span><span class="sxs-lookup"><span data-stu-id="52214-108">hello Batch Management .NET library is an Azure resource provider client, and is used together with [Azure Resource Manager][resman_overview] toomanage these resources programmatically.</span></span> <span data-ttu-id="52214-109">Az Azure AD szükség tooauthenticate kérelmet bármely Azure-erőforrás szolgáltató ügyfél hello Batch Management .NET kódtárral dokumentumtár, beleértve és révén [Azure Resource Manager][resman_overview].</span><span class="sxs-lookup"><span data-stu-id="52214-109">Azure AD is required tooauthenticate requests made through any Azure resource provider client, including hello Batch Management .NET library, and through [Azure Resource Manager][resman_overview].</span></span>

<span data-ttu-id="52214-110">Ebben a cikkben azt megismerkedhet az Azure AD tooauthenticate hello Batch Management .NET kódtárral libraryt használó alkalmazások segítségével.</span><span class="sxs-lookup"><span data-stu-id="52214-110">In this article, we explore using Azure AD tooauthenticate from applications that use hello Batch Management .NET library.</span></span> <span data-ttu-id="52214-111">Megmutatjuk, hogyan toouse az Azure AD tooauthenticate egy előfizetés rendszergazdai vagy társadminisztrátori, segítségével integrált hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="52214-111">We show how toouse Azure AD tooauthenticate a subscription administrator or co-administrator, using integrated authentication.</span></span> <span data-ttu-id="52214-112">Hello használjuk [AccountManagment] [ acct_mgmt_sample] mintaprojektet, elérhető a Githubon, toowalk keresztül hello Batch Management .NET könyvtár az Azure AD használatával.</span><span class="sxs-lookup"><span data-stu-id="52214-112">We use hello [AccountManagment][acct_mgmt_sample] sample project, available on GitHub, toowalk through using Azure AD with hello Batch Management .NET library.</span></span>

<span data-ttu-id="52214-113">toolearn több használatáról hello Batch Management .NET kódtárral kódtár és hello AccountManagement mintaalkalmazás, lásd: [kezelése Batch fiókjainak és kvótáinak hello kötegelt felügyeleti ügyféloldali kódtára a .NET-hez a](batch-management-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="52214-113">toolearn more about using hello Batch Management .NET library and hello AccountManagement sample, see [Manage Batch accounts and quotas with hello Batch Management client library for .NET](batch-management-dotnet.md).</span></span>

## <a name="register-your-application-with-azure-ad"></a><span data-ttu-id="52214-114">Az alkalmazás regisztrálása az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="52214-114">Register your application with Azure AD</span></span>

<span data-ttu-id="52214-115">hello Azure [Active Directory Authentication Library] [ aad_adal] (adal-t) biztosít programozási felület tooAzure használatra AD az alkalmazásokon belül.</span><span class="sxs-lookup"><span data-stu-id="52214-115">hello Azure [Active Directory Authentication Library][aad_adal] (ADAL) provides a programmatic interface tooAzure AD for use within your applications.</span></span> <span data-ttu-id="52214-116">toocall ADAL-nak az alkalmazásról, regisztrálnia kell az alkalmazást az Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="52214-116">toocall ADAL from your application, you must register your application in an Azure AD tenant.</span></span> <span data-ttu-id="52214-117">Ha a regisztrálnia az alkalmazást, az információk az alkalmazásról, beleértve azt a belül hello Azure AD-bérlő nevét adja meg információkat az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="52214-117">When you register your application, you supply Azure AD with information about your application, including a name for it within hello Azure AD tenant.</span></span> <span data-ttu-id="52214-118">Az Azure AD majd biztosít az Alkalmazásazonosító használt tooassociate az alkalmazás az Azure ad-val futásidőben.</span><span class="sxs-lookup"><span data-stu-id="52214-118">Azure AD then provides an application ID that you use tooassociate your application with Azure AD at runtime.</span></span> <span data-ttu-id="52214-119">toolearn hello Alkalmazásazonosító, kapcsolatos további információkért lásd: [alkalmazás és szolgáltatás egyszerű objektumok az Azure Active Directoryban](../active-directory/develop/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="52214-119">toolearn more about hello application ID, see [Application and service principal objects in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).</span></span>

<span data-ttu-id="52214-120">tooregister hello AccountManagement mintaalkalmazást, kövesse hello hello [egy alkalmazás hozzáadása](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) szakasz [alkalmazások integrálása az Azure Active Directory] [ aad_integrate].</span><span class="sxs-lookup"><span data-stu-id="52214-120">tooregister hello AccountManagement sample application, follow hello steps in hello [Adding an Application](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) section in [Integrating applications with Azure Active Directory][aad_integrate].</span></span> <span data-ttu-id="52214-121">Adja meg **natív ügyfélalkalmazás** hello típusú alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="52214-121">Specify **Native Client Application** for hello type of application.</span></span> <span data-ttu-id="52214-122">hello iparági szabványos OAuth 2.0 URI-JÁNAK hello **átirányítási URI-** van `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="52214-122">hello industry standard OAuth 2.0 URI for hello **Redirect URI** is `urn:ietf:wg:oauth:2.0:oob`.</span></span> <span data-ttu-id="52214-123">Azonban megadhat bármilyen érvényes URI-azonosító (például `http://myaccountmanagementsample`) a hello **átirányítási URI-**, mert nem kell valódi végpont toobe:</span><span class="sxs-lookup"><span data-stu-id="52214-123">However, you can specify any valid URI (such as `http://myaccountmanagementsample`) for hello **Redirect URI**, as it does not need toobe a real endpoint:</span></span>

![](./media/batch-aad-auth-management/app-registration-management-plane.png)

<span data-ttu-id="52214-124">Hello regisztrációs folyamat befejezése után kell lásd: hello Alkalmazásazonosítót és hello objektumazonosító (egyszerű szolgáltatásnév): az alkalmazás szerepel.</span><span class="sxs-lookup"><span data-stu-id="52214-124">Once you complete hello registration process, you'll see hello application ID and hello object (service principal) ID listed for your application.</span></span>  

![](./media/batch-aad-auth-management/app-registration-client-id.png)

## <a name="grant-hello-azure-resource-manager-api-access-tooyour-application"></a><span data-ttu-id="52214-125">Hello Azure Resource Manager API access tooyour alkalmazás</span><span class="sxs-lookup"><span data-stu-id="52214-125">Grant hello Azure Resource Manager API access tooyour application</span></span>

<span data-ttu-id="52214-126">A következő toodelegate hozzáférés tooyour alkalmazás toohello Azure Resource Manager API lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="52214-126">Next, you'll need toodelegate access tooyour application toohello Azure Resource Manager API.</span></span> <span data-ttu-id="52214-127">a Resource Manager API hello hello Azure AD-azonosító **Windows Azure szolgáltatásfelügyeleti API**.</span><span class="sxs-lookup"><span data-stu-id="52214-127">hello Azure AD identifier for hello Resource Manager API is **Windows Azure Service Management API**.</span></span>

<span data-ttu-id="52214-128">Kövesse az alábbi lépéseket a hello Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="52214-128">Follow these steps in hello Azure portal:</span></span>

1. <span data-ttu-id="52214-129">Hello Azure-portálon hello bal oldali navigációs ablaktábláján válassza **több szolgáltatások**, kattintson a **App regisztrációk**, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="52214-129">In hello left-hand navigation pane of hello Azure portal, choose **More Services**, click **App Registrations**, and click **Add**.</span></span>
2. <span data-ttu-id="52214-130">Keresse meg az alkalmazás hello listában app regisztrációk hello nevét:</span><span class="sxs-lookup"><span data-stu-id="52214-130">Search for hello name of your application in hello list of app registrations:</span></span>

    ![Keresse meg az alkalmazás neve](./media/batch-aad-auth-management/search-app-registration.png)

3. <span data-ttu-id="52214-132">Megjelenítési hello **beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="52214-132">Display hello **Settings** blade.</span></span> <span data-ttu-id="52214-133">A hello **API-hozzáférés** szakaszban jelölje be **szükséges engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="52214-133">In hello **API Access** section, select **Required permissions**.</span></span>
4. <span data-ttu-id="52214-134">Kattintson a **Hozzáadás** tooadd egy új szükséges engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="52214-134">Click **Add** tooadd a new required permission.</span></span> 
5. <span data-ttu-id="52214-135">Adja meg az 1. lépésben **Windows Azure szolgáltatásfelügyeleti API**, jelölje ki, hogy API eredmények hello listája, majd kattintson a hello **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="52214-135">In step 1, enter **Windows Azure Service Management API**, select that API from hello list of results, and click hello **Select** button.</span></span>
6. <span data-ttu-id="52214-136">A 2. lépésben, válassza ki hello jelölőnégyzet mellett túl**Access Azure klasszikus üzembe helyezési modellel, mint a szervezeti felhasználók**, hello kattintson **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="52214-136">In step 2, select hello check box next too**Access Azure classic deployment model as organization users**, and click hello **Select** button.</span></span>
7. <span data-ttu-id="52214-137">Kattintson a hello **végzett** gombra.</span><span class="sxs-lookup"><span data-stu-id="52214-137">Click hello **Done** button.</span></span>

<span data-ttu-id="52214-138">Hello **szükséges engedélyek** panel most mutat be, hogy engedélyeket tooyour alkalmazás kapnak tooboth hello adal-t és a Resource Manager API-kat.</span><span class="sxs-lookup"><span data-stu-id="52214-138">hello **Required Permissions** blade now shows that permissions tooyour application are granted tooboth hello ADAL and Resource Manager APIs.</span></span> <span data-ttu-id="52214-139">Engedélyekkel tooADAL alapértelmezés szerint amikor az alkalmazás regisztrálására az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="52214-139">Permissions are granted tooADAL by default when you first register your app with Azure AD.</span></span>

![Delegált engedélyek toohello Azure Resource Manager API](./media/batch-aad-auth-management/required-permissions-management-plane.png)

## <a name="azure-ad-endpoints"></a><span data-ttu-id="52214-141">Azure AD-végpontok</span><span class="sxs-lookup"><span data-stu-id="52214-141">Azure AD endpoints</span></span>

<span data-ttu-id="52214-142">tooauthenticate a kötegelt megoldásokat az Azure ad-vel, szüksége lesz a jól ismert végpontokat.</span><span class="sxs-lookup"><span data-stu-id="52214-142">tooauthenticate your Batch Management solutions with Azure AD, you'll need two well-known endpoints.</span></span>

- <span data-ttu-id="52214-143">Hello **az Azure AD közös végpont** összegyűjtéséhez felületet, amikor egy adott bérlő nem szerepel, mint hello integrált hitelesítés általános hitelesítő adatokat biztosít:</span><span class="sxs-lookup"><span data-stu-id="52214-143">hello **Azure AD common endpoint** provides a generic credential gathering interface when a specific tenant is not provided, as in hello case of integrated authentication:</span></span>

    `https://login.microsoftonline.com/common`

- <span data-ttu-id="52214-144">Hello **Azure Resource Manager-végpont** használt tooacquire kérelmek toohello Batch management szolgáltatás hitelesítéséhez jogkivonat van:</span><span class="sxs-lookup"><span data-stu-id="52214-144">hello **Azure Resource Manager endpoint** is used tooacquire a token for authenticating requests toohello Batch management service:</span></span>

    `https://management.core.windows.net/`

<span data-ttu-id="52214-145">hello AccountManagement mintaalkalmazás állandókat ezeket a végpontokat határoz meg.</span><span class="sxs-lookup"><span data-stu-id="52214-145">hello AccountManagement sample application defines constants for these endpoints.</span></span> <span data-ttu-id="52214-146">Ezek állandók változatlanul hagyja:</span><span class="sxs-lookup"><span data-stu-id="52214-146">Leave these constants unchanged:</span></span>

```csharp
// Azure Active Directory "common" endpoint.
private const string AuthorityUri = "https://login.microsoftonline.com/common";
// Azure Resource Manager endpoint 
private const string ResourceUri = "https://management.core.windows.net/";
```

## <a name="reference-your-application-id"></a><span data-ttu-id="52214-147">Az Alkalmazásazonosító hivatkozik</span><span class="sxs-lookup"><span data-stu-id="52214-147">Reference your application ID</span></span> 

<span data-ttu-id="52214-148">Az ügyfélalkalmazás hello application ID (is hivatkozott tooas hello ügyfél-azonosító) tooaccess az Azure AD futásidőben.</span><span class="sxs-lookup"><span data-stu-id="52214-148">Your client application uses hello application ID (also referred tooas hello client ID) tooaccess Azure AD at runtime.</span></span> <span data-ttu-id="52214-149">Az alkalmazás már regisztrált hello Azure-portálon, a kód toouse hello Alkalmazásazonosító regisztrált alkalmazás az Azure AD által biztosított frissíti.</span><span class="sxs-lookup"><span data-stu-id="52214-149">Once you've registered your application in hello Azure portal, update your code toouse hello application ID provided by Azure AD for your registered application.</span></span> <span data-ttu-id="52214-150">A hello AccountManagement mintaalkalmazást másolja az Alkalmazásazonosító hello Azure portál toohello megfelelő állandó:</span><span class="sxs-lookup"><span data-stu-id="52214-150">In hello AccountManagement sample application, copy your application ID from hello Azure portal toohello appropriate constant:</span></span>

```csharp
// Specify hello unique identifier (hello "Client ID") for your application. This is required so that your
// native client application (i.e. this sample) can access hello Microsoft Azure AD Graph API. For information
// about registering an application in Azure Active Directory, please see "Adding an Application" here:
// https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
private const string ClientId = "<application-id>";
```
<span data-ttu-id="52214-151">Hello átirányítási URI-t hello regisztrációs folyamat során megadott szeretne másolni.</span><span class="sxs-lookup"><span data-stu-id="52214-151">Also copy hello redirect URI that you specified during hello registration process.</span></span> <span data-ttu-id="52214-152">hello átirányítási URI a kódban megadott hello átirányítási URI-t hello alkalmazás regisztrálásakor megadott egyeznie kell.</span><span class="sxs-lookup"><span data-stu-id="52214-152">hello redirect URI specified in your code must match hello redirect URI that you provided when you registered hello application.</span></span>

```csharp
// hello URI toowhich Azure AD will redirect in response tooan OAuth 2.0 request. This value is
// specified by you when you register an application with AAD (see ClientId comment). It does not
// need toobe a real endpoint, but must be a valid URI (e.g. https://accountmgmtsampleapp).
private const string RedirectUri = "http://myaccountmanagementsample";
```

## <a name="acquire-an-azure-ad-authentication-token"></a><span data-ttu-id="52214-153">Az Azure AD hitelesítési jogkivonat</span><span class="sxs-lookup"><span data-stu-id="52214-153">Acquire an Azure AD authentication token</span></span>

<span data-ttu-id="52214-154">Miután hello AccountManagement minta regisztrálni kell az Azure AD-bérlő hello, és frissítse az hello forrás mintakódot az értékeket, hello minta készen áll az Azure AD tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="52214-154">After you register hello AccountManagement sample in hello Azure AD tenant and update hello sample source code with your values, hello sample is ready tooauthenticate using Azure AD.</span></span> <span data-ttu-id="52214-155">Hello minta futtatásához hello adal-t próbálja tooacquire egy hitelesítési jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="52214-155">When you run hello sample, hello ADAL attempts tooacquire an authentication token.</span></span> <span data-ttu-id="52214-156">Ebben a lépésben megkérdezi a Microsoft hitelesítő adatait:</span><span class="sxs-lookup"><span data-stu-id="52214-156">At this step, it prompts you for your Microsoft credentials:</span></span> 

```csharp
// Obtain an access token using hello "common" AAD resource. This allows hello application
// tooquery AAD for information that lies outside hello application's tenant (such as for
// querying subscription information in your Azure account).
AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
AuthenticationResult authResult = authContext.AcquireToken(ResourceUri,
                                                        ClientId,
                                                        new Uri(RedirectUri),
                                                        PromptBehavior.Auto);
```

<span data-ttu-id="52214-157">Miután megadta a hitelesítő adatait, hello mintaalkalmazás lépne tooissue hitelesített kérelmek toohello Batch management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="52214-157">After you provide your credentials, hello sample application can proceed tooissue authenticated requests toohello Batch management service.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="52214-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="52214-158">Next steps</span></span>

<span data-ttu-id="52214-159">További információ hello [AccountManagement mintaalkalmazás][acct_mgmt_sample], lásd: [kezelése Batch fiókjainak és kvótáinak a hello kötegelt felügyeleti ügyféloldali kódtára a .NET](batch-management-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="52214-159">For more information on running hello [AccountManagement sample application][acct_mgmt_sample], see [Manage Batch accounts and quotas with hello Batch Management client library for .NET](batch-management-dotnet.md).</span></span>

<span data-ttu-id="52214-160">További információ az Azure AD toolearn lásd: hello [Azure Active Directory dokumentációjának](https://docs.microsoft.com/azure/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="52214-160">toolearn more about Azure AD, see hello [Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span> <span data-ttu-id="52214-161">Hogyan toouse ADAL találhatók hello bemutató részletes példák [Azure mintakódok](https://azure.microsoft.com/resources/samples/?service=active-directory) könyvtár.</span><span class="sxs-lookup"><span data-stu-id="52214-161">In-depth examples showing how toouse ADAL are available in hello [Azure Code Samples](https://azure.microsoft.com/resources/samples/?service=active-directory) library.</span></span>

<span data-ttu-id="52214-162">tooauthenticate Batch szolgáltatás alkalmazások az Azure AD, lásd: [hitelesítéséhez kötegelt szolgáltatási megoldások és az Active Directory](batch-aad-auth.md).</span><span class="sxs-lookup"><span data-stu-id="52214-162">tooauthenticate Batch service applications using Azure AD, see [Authenticate Batch service solutions with Active Directory](batch-aad-auth.md).</span></span> 


[aad_about]: ../active-directory/active-directory-whatis.md "Mi az Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Az Azure Active Directory hitelesítési forgatókönyvei"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Alkalmazások integrálása az Azure Active Directoryval"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[azure_portal]: http://portal.azure.com
[resman_overview]: ../azure-resource-manager/resource-group-overview.md
