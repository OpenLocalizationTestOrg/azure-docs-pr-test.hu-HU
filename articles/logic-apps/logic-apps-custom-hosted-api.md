---
title: "Üzembe helyezéséhez, a hívja, és a webes API-k & REST API-k hitelesítéséhez az Azure Logic Apps |} Microsoft Docs"
description: "Üzembe helyezéséhez, a hitelesítés és a rendszer Integrációk az Azure Logic Apps-munkafolyamatok webes API-k & REST API-k hívás"
keywords: "webes API-k, a REST API-k, a összekötőket, a munkafolyamatok, a rendszer integrációja, hitelesítéshez"
services: logic-apps
author: stepsic-microsoft-com
manager: anneta
editor: 
documentationcenter: 
ms.assetid: f113005d-0ba6-496b-8230-c1eadbd6dbb9
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: LADocs; stepsic
ms.openlocfilehash: 88c62d5ab046d8cf4bce5d23b776e517bb0e1d87
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-call-and-authenticate-custom-apis-as-connectors-for-logic-apps"></a><span data-ttu-id="6eebe-104">Üzembe helyezéséhez, hívja, és egyéni API-k hitelesítse magát a logic apps összekötők</span><span class="sxs-lookup"><span data-stu-id="6eebe-104">Deploy, call, and authenticate custom APIs as connectors for logic apps</span></span>

<span data-ttu-id="6eebe-105">Miután [hozzon létre egyéni API-k](./logic-apps-create-api-app.md) biztosító műveletek vagy eseményindítók logic apps-munkafolyamatok használatához, az API-kat is telepítenie kell őket hívása előtt.</span><span class="sxs-lookup"><span data-stu-id="6eebe-105">After you [create custom APIs](./logic-apps-create-api-app.md) that provide actions or triggers to use in logic apps workflows, you must deploy your APIs before you can call them.</span></span> <span data-ttu-id="6eebe-106">Bár kérjen API-k a logikai alkalmazás, a legjobb élményt, adja hozzá a [Swagger-metaadatok](http://swagger.io/specification/) , amely az API-műveleteket és paramétereket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="6eebe-106">And although you can call any API from a logic app, for the best experience, add [Swagger metadata](http://swagger.io/specification/) that describes your API's operations and parameters.</span></span> <span data-ttu-id="6eebe-107">A Swagger-fájl az API-együttműködését, és könnyebben integrálhatja a logic apps segítségével.</span><span class="sxs-lookup"><span data-stu-id="6eebe-107">This Swagger file helps your API work better and integrate more easily with logic apps.</span></span>

<span data-ttu-id="6eebe-108">Telepítheti az API-k [webalkalmazások](../app-service-web/app-service-web-overview.md), de az API-k érdemes megfontolni [API-alkalmazások](../app-service-api/app-service-api-apps-why-best-platform.md), amely megkönnyítheti a projekt felépítéséhez, tárolására és felhasználása a felhőben és helyszíni API-k.</span><span class="sxs-lookup"><span data-stu-id="6eebe-108">You can deploy your APIs as [web apps](../app-service-web/app-service-web-overview.md), but consider deploying your APIs as [API apps](../app-service-api/app-service-api-apps-why-best-platform.md), which can make your job easier when you build, host, and consume APIs in the cloud and on premises.</span></span> <span data-ttu-id="6eebe-109">Nincs módosíthatja bármely kódot az API-k – csupán telepítse kódját egy API-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="6eebe-109">You don't have to change any code in your APIs -- just deploy your code to an API app.</span></span> <span data-ttu-id="6eebe-110">Az API-kat tárolhatja a [Azure App Service](../app-service/app-service-value-prop-what-is.md), a platform-,-a-szolgáltatásajánlat (PaaS), amely biztosítja a legjobb, a legegyszerűbb és a leginkább méretezhető módszerek valamelyikével API üzemeltetéséhez.</span><span class="sxs-lookup"><span data-stu-id="6eebe-110">You can host your APIs on [Azure App Service](../app-service/app-service-value-prop-what-is.md), a platform-as-a-service (PaaS) offering that provides one of the best, easiest, and most scalable ways for API hosting.</span></span>

<span data-ttu-id="6eebe-111">Az API-kat a logic Apps alkalmazásokból hívások hitelesítéséhez, állíthat be Azure Active Directory az Azure portálon így nem kell frissíteni a kódot.</span><span class="sxs-lookup"><span data-stu-id="6eebe-111">To authenticate calls from logic apps to your APIs, you can set up Azure Active Directory in the Azure portal so you don't have to update your code.</span></span> <span data-ttu-id="6eebe-112">Vagy igényelnek, és kényszeríteni a hitelesítést az API-kódban keresztül.</span><span class="sxs-lookup"><span data-stu-id="6eebe-112">Or, you can require and enforce authentication through your API's code.</span></span>

## <a name="deploy-your-api-as-a-web-app-or-api-app"></a><span data-ttu-id="6eebe-113">Az API-webalkalmazást vagy API-alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="6eebe-113">Deploy your API as a web app or API app</span></span>

<span data-ttu-id="6eebe-114">Az egyéni API-t a logikai alkalmazás hívása előtt telepítse az API-webalkalmazást vagy API-alkalmazás az Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="6eebe-114">Before you can call your custom API from a logic app, deploy your API as a web app or API app to Azure App Service.</span></span> <span data-ttu-id="6eebe-115">Is, így a Swagger-dokumentum a Logic App Designer által is olvasható, API definition tulajdonságainak beállításához és kapcsolja be a [eltérő eredetű erőforrások megosztása (CORS)](../app-service-api/app-service-api-cors-consume-javascript.md#corsconfig) a webes alkalmazás vagy API-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="6eebe-115">Also, to make your Swagger document readable by the Logic App Designer, set the API definition properties and turn on [cross-origin resource sharing (CORS)](../app-service-api/app-service-api-cors-consume-javascript.md#corsconfig) for your web app or API app.</span></span>

1. <span data-ttu-id="6eebe-116">Válassza ki a webalkalmazás és az API-alkalmazások az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="6eebe-116">In the Azure portal, select your web app or API app.</span></span>

2. <span data-ttu-id="6eebe-117">A panelen, amely megnyitja a **API**, válassza a **API-definíció**.</span><span class="sxs-lookup"><span data-stu-id="6eebe-117">In the blade that opens, under **API**, choose **API definition**.</span></span> <span data-ttu-id="6eebe-118">Állítsa be a **API definition hely** a swagger.json fájl URL-címet.</span><span class="sxs-lookup"><span data-stu-id="6eebe-118">Set the **API definition location** to the URL for your swagger.json file.</span></span>

   <span data-ttu-id="6eebe-119">Általában az URL-cím jelenik meg, a következő formátumban:`https://{name}.azurewebsites.net/swagger/docs/v1)`</span><span class="sxs-lookup"><span data-stu-id="6eebe-119">Usually, the URL appears in this format: `https://{name}.azurewebsites.net/swagger/docs/v1)`</span></span>

   ![Az egyéni API-Swagger-fájl csatolása](media/logic-apps-custom-hosted-api/custom-api-swagger-url.png)

3. <span data-ttu-id="6eebe-121">A **API**, válassza a **CORS**.</span><span class="sxs-lookup"><span data-stu-id="6eebe-121">Under **API**, choose **CORS**.</span></span> <span data-ttu-id="6eebe-122">Állítsa be a CORS-házirend a **engedélyezett eredeteket** való  **"*"** (minden engedélyezése).</span><span class="sxs-lookup"><span data-stu-id="6eebe-122">Set the CORS policy for **Allowed origins** to **'*'** (allow all).</span></span>

   <span data-ttu-id="6eebe-123">Ez a beállítás lehetővé teszi a kérelmek az Logic App-tervezőből.</span><span class="sxs-lookup"><span data-stu-id="6eebe-123">This setting permits requests from Logic App Designer.</span></span>

   ![A Logic App Designer kérések engedélyezése az egyéni API](media/logic-apps-custom-hosted-api/custom-api-cors.png)

<span data-ttu-id="6eebe-125">További információkért lásd: ezek a cikkek:</span><span class="sxs-lookup"><span data-stu-id="6eebe-125">For more information, see these articles:</span></span>

* [<span data-ttu-id="6eebe-126">Adja hozzá a Swagger-metaadatokat az ASP.NET web API-k</span><span class="sxs-lookup"><span data-stu-id="6eebe-126">Add Swagger metadata for ASP.NET web APIs</span></span>](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui)
* [<span data-ttu-id="6eebe-127">ASP.NET web API-k telepítése az Azure App Service</span><span class="sxs-lookup"><span data-stu-id="6eebe-127">Deploy ASP.NET web APIs to Azure App Service</span></span>](../app-service-api/app-service-api-dotnet-get-started.md)

## <a name="call-your-custom-api-from-logic-app-workflows"></a><span data-ttu-id="6eebe-128">Az egyéni API-t hívja a logic app munkafolyamatok</span><span class="sxs-lookup"><span data-stu-id="6eebe-128">Call your custom API from logic app workflows</span></span>

<span data-ttu-id="6eebe-129">Az API-definíció tulajdonságait és a CORS beállítása után az egyéni API-eseményindítók és műveletek közé tartozik a logic app munkafolyamat is kell válnia.</span><span class="sxs-lookup"><span data-stu-id="6eebe-129">After you set up the API definition properties and CORS, your custom API's triggers and actions should become available for you to include in your logic app workflow.</span></span> 

*  <span data-ttu-id="6eebe-130">A Logic Apps-tervezőben az előfizetés webhelyek tallózással elemre kattintva megtekintheti a webhelyek, amelyek Swagger URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="6eebe-130">To view the websites that have Swagger URLs, you can browse your subscription websites in the Logic Apps Designer.</span></span>

*  <span data-ttu-id="6eebe-131">A Swagger-dokumentum húzni elérhető műveletek és a bemeneti adatok megtekintéséhez használja a [HTTP + Swagger műveleti](../connectors/connectors-native-http-swagger.md).</span><span class="sxs-lookup"><span data-stu-id="6eebe-131">To view available actions and inputs by pointing at a Swagger document, use the [HTTP + Swagger action](../connectors/connectors-native-http-swagger.md).</span></span>

*  <span data-ttu-id="6eebe-132">API-k, beleértve az API-k, amelyek nem rendelkeznek, vagy teszi közzé a Swagger-dokumentum hívására bármikor létrehozhat egy kérelmet a [HTTP-művelet](../connectors/connectors-native-http.md).</span><span class="sxs-lookup"><span data-stu-id="6eebe-132">To call any API, including APIs that don't have or expose a Swagger document, you can always create a request with the [HTTP action](../connectors/connectors-native-http.md).</span></span>

## <a name="authenticate-calls-to-your-custom-api"></a><span data-ttu-id="6eebe-133">Az egyéni API-hívások hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="6eebe-133">Authenticate calls to your custom API</span></span>

<span data-ttu-id="6eebe-134">Az alábbi módszerek az egyéni API-hívások biztonságossá teheti:</span><span class="sxs-lookup"><span data-stu-id="6eebe-134">You can secure calls to your custom API in these ways:</span></span>

* <span data-ttu-id="6eebe-135">[Nincs kódmódosításokat](#no-code): az API-t védelme [Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) az Azure-portálon, így nem kell frissítse a kódot, vagy telepítse újra az API-t.</span><span class="sxs-lookup"><span data-stu-id="6eebe-135">[No code changes](#no-code): Protect your API with [Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) through the Azure portal, so you don't have to update your code or redeploy your API.</span></span>

  > [!NOTE]
  > <span data-ttu-id="6eebe-136">Alapértelmezés szerint az Azure AD-alapú hitelesítés, amely az Azure portálon kapcsolja be a minden részletre kiterjedő engedélyezési nem biztosít.</span><span class="sxs-lookup"><span data-stu-id="6eebe-136">By default, the Azure AD authentication that you turn on in the Azure portal doesn't provide fine-grained authorization.</span></span> <span data-ttu-id="6eebe-137">Ez a hitelesítés például az API-t egy adott bérlő, nem pedig egy adott felhasználó vagy alkalmazás zárolja.</span><span class="sxs-lookup"><span data-stu-id="6eebe-137">For example, this authentication locks your API to just a specific tenant, not to a specific user or app.</span></span> 

* <span data-ttu-id="6eebe-138">[Frissítse az API-kódot](#update-code): az API védelme kényszerítésével [tanúsítványhitelesítés](#certificate), [az egyszerű hitelesítés](#basic), vagy [az Azure AD-alapú hitelesítés](#azure-ad-code) kód.</span><span class="sxs-lookup"><span data-stu-id="6eebe-138">[Update your API's code](#update-code): Protect your API by enforcing [certificate authentication](#certificate), [basic authentication](#basic), or [Azure AD authentication](#azure-ad-code) through code.</span></span>

<a name="no-code"></a>

### <a name="authenticate-calls-to-your-api-without-changing-code"></a><span data-ttu-id="6eebe-139">Az API-hívások hitelesítéséhez programkód módosítása nélkül</span><span class="sxs-lookup"><span data-stu-id="6eebe-139">Authenticate calls to your API without changing code</span></span>

<span data-ttu-id="6eebe-140">Ez a metódus általános lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6eebe-140">Here's the general steps for this method:</span></span>

1. <span data-ttu-id="6eebe-141">Hozzon létre két [Azure Active Directory (Azure AD) alkalmazás identitások](../app-service-api/app-service-api-dotnet-service-principal-auth.md): egy a Logic Apps alkalmazást, egy, a webes alkalmazás (vagy API-alkalmazás).</span><span class="sxs-lookup"><span data-stu-id="6eebe-141">Create two [Azure Active Directory (Azure AD) application identities](../app-service-api/app-service-api-dotnet-service-principal-auth.md): one for your logic app and one for your web app (or API app).</span></span>

2. <span data-ttu-id="6eebe-142">Az API-hívások hitelesítést végezni, a hitelesítő adatok használata (ügyfél-Azonosítót és titkos kulcs) a a [egyszerű](../app-service-api/app-service-api-dotnet-service-principal-auth.md) , amely a Logic Apps alkalmazást az Azure AD alkalmazás identitásának van társítva.</span><span class="sxs-lookup"><span data-stu-id="6eebe-142">To authenticate calls to your API, use the credentials (client ID and secret) for the [service principal](../app-service-api/app-service-api-dotnet-service-principal-auth.md) that's associated with the Azure AD application identity for your logic app.</span></span>

3. <span data-ttu-id="6eebe-143">Vegye fel a Alkalmazásazonosítók a logic app-definíciót.</span><span class="sxs-lookup"><span data-stu-id="6eebe-143">Include the application IDs in your logic app definition.</span></span>

#### <a name="part-1-create-an-azure-ad-application-identity-for-your-logic-app"></a><span data-ttu-id="6eebe-144">1. lépés: Hozzon létre egy Azure AD alkalmazás-azonosítót a logikai alkalmazásnak</span><span class="sxs-lookup"><span data-stu-id="6eebe-144">Part 1: Create an Azure AD application identity for your logic app</span></span>

<span data-ttu-id="6eebe-145">A Logic Apps alkalmazást az Azure AD alkalmazás identitását használja az Azure AD hitelesítő.</span><span class="sxs-lookup"><span data-stu-id="6eebe-145">Your logic app uses this Azure AD application identity to authenticate against Azure AD.</span></span> <span data-ttu-id="6eebe-146">Csak kell beállítania a identitást egy alkalommal a címtáron.</span><span class="sxs-lookup"><span data-stu-id="6eebe-146">You only have to set up this identity one time for your directory.</span></span> <span data-ttu-id="6eebe-147">Például is választja a logic apps ugyanazzal az identitással használandó annak ellenére, hogy minden logikai alkalmazás egyedi azonosítók hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="6eebe-147">For example, you can choose to use the same identity for all your logic apps, even though you can create unique identities for each logic app.</span></span> <span data-ttu-id="6eebe-148">Ezeket az identitásokat az Azure-portálon állíthatja [a klasszikus Azure portálon](#app-identity-logic-classic), vagy használjon [PowerShell](#powershell).</span><span class="sxs-lookup"><span data-stu-id="6eebe-148">You can set up these identities in the Azure portal, [Azure classic portal](#app-identity-logic-classic), or use [PowerShell](#powershell).</span></span>

<span data-ttu-id="6eebe-149">**A logikai alkalmazásnak az alkalmazásazonosító létrehozása az Azure-portálon**</span><span class="sxs-lookup"><span data-stu-id="6eebe-149">**Create the application identity for your logic app in the Azure portal**</span></span>

1. <span data-ttu-id="6eebe-150">Az a [Azure-portálon](https://portal.azure.com "https://portal.azure.com"), válassza a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6eebe-150">In the [Azure portal](https://portal.azure.com "https://portal.azure.com"), choose **Azure Active Directory**.</span></span> 

2. <span data-ttu-id="6eebe-151">Győződjön meg arról, hogy a webes alkalmazás vagy API-alkalmazás könyvtárába van-e.</span><span class="sxs-lookup"><span data-stu-id="6eebe-151">Confirm that you're in the same directory as your web app or API app.</span></span>

   > [!TIP]
   > <span data-ttu-id="6eebe-152">Könyvtárak váltáshoz kattintson a profil, és válasszon másik címtárat.</span><span class="sxs-lookup"><span data-stu-id="6eebe-152">To switch directories, click your profile and select another directory.</span></span> <span data-ttu-id="6eebe-153">Másik lehetőségként válasszon **áttekintése** > **kapcsoló directory**.</span><span class="sxs-lookup"><span data-stu-id="6eebe-153">Or, choose **Overview** > **Switch directory**.</span></span>

3. <span data-ttu-id="6eebe-154">A directory menü alatt **kezelése**, válassza a **App regisztrációk** > **új alkalmazás regisztrációja**.</span><span class="sxs-lookup"><span data-stu-id="6eebe-154">On the directory menu, under **Manage**, choose **App registrations** > **New application registration**.</span></span>

   > [!TIP]
   > <span data-ttu-id="6eebe-155">Alapértelmezés szerint a regisztrációk alkalmazáslistájában összes alkalmazás regisztrációja megjelenik a könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="6eebe-155">By default, the app registrations list shows all app registrations in your directory.</span></span> <span data-ttu-id="6eebe-156">Válassza ki, ha csak az alkalmazás regisztrációk mellett a keresőmezőbe, **alkalmazásaimat**.</span><span class="sxs-lookup"><span data-stu-id="6eebe-156">To view only your app registrations, next to the search box, select **My apps**.</span></span> 

   ![Hozzon létre új alkalmazás regisztrálása](./media/logic-apps-custom-hosted-api/new-app-registration-azure-portal.png)

4. <span data-ttu-id="6eebe-158">Nevezze el az alkalmazás azonosítóját, hagyja **alkalmazástípus** beállítása **webalkalmazás / API**, adjon meg egy egyedi karakterláncot formázni egy tartományt a **bejelentkezési URL-cím**, és válassza a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="6eebe-158">Give your application identity a name, leave **Application type** set to **Web app / API**, provide a unique string formatted as a domain for **Sign-on URL**, and choose **Create**.</span></span>

   ![Adjon meg nevet és a bejelentkezés URL-címet az alkalmazás-azonosító](./media/logic-apps-custom-hosted-api/logic-app-identity-azure-portal.png)

   <span data-ttu-id="6eebe-160">Az alkalmazás azonosítóját a Logic Apps alkalmazást most létrehozott alkalmazást regisztrációk listájában jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6eebe-160">The application identity that you created for your logic app now appears in the app registrations list.</span></span>

   ![A logikai alkalmazásnak identitása](./media/logic-apps-custom-hosted-api/logic-app-identity-created.png)

5. <span data-ttu-id="6eebe-162">Az alkalmazás regisztrációk listában válassza ki az új alkalmazás azonosítója.</span><span class="sxs-lookup"><span data-stu-id="6eebe-162">In the app registrations list, select your new application identity.</span></span> <span data-ttu-id="6eebe-163">Másolja ki és mentse a **Alkalmazásazonosító** használja az "ügyfél-azonosító" a Logic Apps alkalmazást a 3. rész.</span><span class="sxs-lookup"><span data-stu-id="6eebe-163">Copy and save the **Application ID** to use as the "client ID" for your logic app in Part 3.</span></span>

   ![Másolja ki és mentse a logikai alkalmazás azonosítója](./media/logic-apps-custom-hosted-api/logic-app-application-id.png)

6. <span data-ttu-id="6eebe-165">Ha az alkalmazás identitását beállításait nem látható, válassza a **beállítások** vagy **összes beállítás**.</span><span class="sxs-lookup"><span data-stu-id="6eebe-165">If your application identity settings aren't visible, choose **Settings** or **All settings**.</span></span>

7. <span data-ttu-id="6eebe-166">A **API-hozzáférés**, válassza a **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="6eebe-166">Under **API Access**, choose **Keys**.</span></span> <span data-ttu-id="6eebe-167">A **leírás**, adja meg a kulcs nevét.</span><span class="sxs-lookup"><span data-stu-id="6eebe-167">Under **Description**, provide a name for your key.</span></span> <span data-ttu-id="6eebe-168">A **Expires**, válasszon egy időtartamot a kulcshoz.</span><span class="sxs-lookup"><span data-stu-id="6eebe-168">Under **Expires**, select a duration for your key.</span></span>

   <span data-ttu-id="6eebe-169">A létrehozni kívánt kulcs az Alkalmazásidentitás "titkos" vagy a logikai alkalmazásnak jelszó funkcionál.</span><span class="sxs-lookup"><span data-stu-id="6eebe-169">The key that you're creating acts as the application identity's "secret" or password for your logic app.</span></span>

   ![Logic app identitás-kulcs létrehozása](./media/logic-apps-custom-hosted-api/create-logic-app-identity-key-secret-password.png)

8. <span data-ttu-id="6eebe-171">Az eszköztáron válassza **mentése**.</span><span class="sxs-lookup"><span data-stu-id="6eebe-171">On the toolbar, choose **Save**.</span></span> <span data-ttu-id="6eebe-172">A **érték**, a kulcs csomópontként jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6eebe-172">Under **Value**, your key now appears.</span></span> 
<span data-ttu-id="6eebe-173">**Ügyeljen arra, hogy másolja ki és mentse a kulcsot** későbbi használatra, mert a kulcs rejtett ha hagyja a fő panelen.</span><span class="sxs-lookup"><span data-stu-id="6eebe-173">**Make sure to copy and save your key** for later use because the key is hidden when you leave the key blade.</span></span>

   <span data-ttu-id="6eebe-174">A Logic Apps alkalmazást a 3. rész konfigurálásakor meg kell adnia ezt a kulcsot, mint a "secret" vagy a jelszó.</span><span class="sxs-lookup"><span data-stu-id="6eebe-174">When you configure your logic app in Part 3, you specify this key as the "secret" or password.</span></span>

   ![Másolja ki és mentse a kulcsot későbbi használatra](./media/logic-apps-custom-hosted-api/logic-app-copy-key-secret-password.png)

<a name="app-identity-logic-classic"></a>

<span data-ttu-id="6eebe-176">**Az Alkalmazásidentitás a logikai alkalmazás létrehozása a klasszikus Azure portálon**</span><span class="sxs-lookup"><span data-stu-id="6eebe-176">**Create the application identity for your logic app in the Azure classic portal**</span></span>

1. <span data-ttu-id="6eebe-177">A klasszikus Azure portálon, válassza ki a [ **Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span><span class="sxs-lookup"><span data-stu-id="6eebe-177">In the Azure classic portal, choose [**Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span></span>

2. <span data-ttu-id="6eebe-178">Válassza ki a ugyanabban a könyvtárban, amely a webes alkalmazás vagy API-alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="6eebe-178">Select the same directory that you use for your web app or API app.</span></span>

3. <span data-ttu-id="6eebe-179">Az a **alkalmazások** lapra, majd **Hozzáadás** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="6eebe-179">On the **Applications** tab, choose **Add** at the bottom of the page.</span></span>

4. <span data-ttu-id="6eebe-180">Nevezze el az alkalmazás azonosítóját, és válassza a **következő** (jobbra).</span><span class="sxs-lookup"><span data-stu-id="6eebe-180">Give your application identity a name, and choose **Next** (right arrow).</span></span>

5. <span data-ttu-id="6eebe-181">A **alkalmazás tulajdonságainak**, adjon meg egy egyedi karakterláncot formázni egy tartományt a **bejelentkezési URL-cím** és **App ID URI**, és válassza ki **Complete** (jelölő).</span><span class="sxs-lookup"><span data-stu-id="6eebe-181">Under **App properties**, provide a unique string formatted as a domain for **Sign-on URL** and **App ID URI**, and choose **Complete** (checkmark).</span></span>

6. <span data-ttu-id="6eebe-182">Az a **konfigurálása** fülre, másolja ki és mentse a **ügyfél-azonosító** a 3. rész használandó logikai alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="6eebe-182">On the **Configure** tab, copy and save the **Client ID** for your logic app to use in Part 3.</span></span>

7. <span data-ttu-id="6eebe-183">A **kulcsok**, nyissa meg a **válassza ki a duration** listája.</span><span class="sxs-lookup"><span data-stu-id="6eebe-183">Under **Keys**, open the **Select duration** list.</span></span> <span data-ttu-id="6eebe-184">Válasszon egy időtartamot a kulcshoz.</span><span class="sxs-lookup"><span data-stu-id="6eebe-184">Select a duration for your key.</span></span>

   <span data-ttu-id="6eebe-185">A létrehozni kívánt kulcs az Alkalmazásidentitás "titkos" vagy a logikai alkalmazásnak jelszó funkcionál.</span><span class="sxs-lookup"><span data-stu-id="6eebe-185">The key that you're creating acts as the application identity's "secret" or password for your logic app.</span></span>

8. <span data-ttu-id="6eebe-186">A lap alján válassza **mentése**.</span><span class="sxs-lookup"><span data-stu-id="6eebe-186">At the bottom of the page, choose **Save**.</span></span> <span data-ttu-id="6eebe-187">Lehetséges, hogy Várjon néhány másodpercig.</span><span class="sxs-lookup"><span data-stu-id="6eebe-187">You might have to wait a few seconds.</span></span>

9. <span data-ttu-id="6eebe-188">A **kulcsok**, feltétlenül másolja, és mentse a kulcsot, ami már jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6eebe-188">Under **Keys**, make sure to copy and save the key that now appears.</span></span> 

   <span data-ttu-id="6eebe-189">A Logic Apps alkalmazást a 3. rész konfigurálásakor meg kell adnia ezt a kulcsot, mint a "secret" vagy a jelszó.</span><span class="sxs-lookup"><span data-stu-id="6eebe-189">When you configure your logic app in Part 3, you specify this key as the "secret" or password.</span></span>

<span data-ttu-id="6eebe-190">További információ megtudhatja, hogyan [konfigurálása az App Service alkalmazás használhatja az Azure Active Directory bejelentkezési](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="6eebe-190">For more information, learn how to [configure your App Service application to use Azure Active Directory login](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).</span></span>

<a name="powershell"></a>

<span data-ttu-id="6eebe-191">**A logikai alkalmazásnak az alkalmazásazonosító létrehozása a PowerShell**</span><span class="sxs-lookup"><span data-stu-id="6eebe-191">**Create the application identity for your logic app in PowerShell**</span></span>

<span data-ttu-id="6eebe-192">Ez a feladat keresztül Azure Resource Manager PowerShell használatával végezheti el.</span><span class="sxs-lookup"><span data-stu-id="6eebe-192">You can perform this task through Azure Resource Manager with PowerShell.</span></span> <span data-ttu-id="6eebe-193">A PowerShellben futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="6eebe-193">In PowerShell, run these commands:</span></span>

1. `Switch-AzureMode AzureResourceManager`

2. `Add-AzureAccount`

3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://mydomain.tld" -IdentifierUris "http://mydomain.tld" -Password "identity-password"`

4. <span data-ttu-id="6eebe-194">Ügyeljen arra, hogy másolja a **Bérlőazonosító** (az Azure AD-bérlő GUID), a **Alkalmazásazonosító**, és a használt jelszót.</span><span class="sxs-lookup"><span data-stu-id="6eebe-194">Make sure to copy the **Tenant ID** (GUID for your Azure AD tenant), the **Application ID**, and the password that you used.</span></span>

<span data-ttu-id="6eebe-195">További információ megtudhatja, hogyan [egyszerű szolgáltatás létrehozása a PowerShell segítségével férhetnek hozzá az erőforrásokhoz](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="6eebe-195">For more information, learn how to [create a service principal with PowerShell to access resources](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span></span>

#### <a name="part-2-create-an-azure-ad-application-identity-for-your-web-app-or-api-app"></a><span data-ttu-id="6eebe-196">2. lépés: Hozzon létre egy Azure AD alkalmazás-azonosítót a webes alkalmazás vagy API-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="6eebe-196">Part 2: Create an Azure AD application identity for your web app or API app</span></span>

<span data-ttu-id="6eebe-197">Ha a webes alkalmazás vagy API-alkalmazás már telepítve van, kapcsolja be a hitelesítés, és az alkalmazásazonosító létrehozása az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="6eebe-197">If your web app or API app is already deployed, you can turn on authentication and create the application identity in the Azure portal.</span></span> <span data-ttu-id="6eebe-198">Ellenkező esetben is [Azure Resource Manager-sablon telepítésekor hitelesítés bekapcsolásához](#authen-deploy).</span><span class="sxs-lookup"><span data-stu-id="6eebe-198">Otherwise, you can [turn on authentication when you deploy with an Azure Resource Manager template](#authen-deploy).</span></span> 

<span data-ttu-id="6eebe-199">**Az alkalmazásazonosító létrehozásához, és központilag telepített alkalmazások az Azure portálon hitelesítés bekapcsolása**</span><span class="sxs-lookup"><span data-stu-id="6eebe-199">**Create the application identity and turn on authentication in the Azure portal for deployed apps**</span></span>

1. <span data-ttu-id="6eebe-200">Az a [Azure-portálon](https://portal.azure.com "https://portal.azure.com"), található, és válassza ki a webalkalmazás vagy API-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="6eebe-200">In the [Azure portal](https://portal.azure.com "https://portal.azure.com"), find and select your web app or API app.</span></span> 

2. <span data-ttu-id="6eebe-201">A **beállítások**, válassza a **hitelesítési/engedélyezési**.</span><span class="sxs-lookup"><span data-stu-id="6eebe-201">Under **Settings**, choose **Authentication/Authorization**.</span></span> <span data-ttu-id="6eebe-202">A **App Service hitelesítés**, kapcsolja be a hitelesítési **a**.</span><span class="sxs-lookup"><span data-stu-id="6eebe-202">Under **App Service Authentication**, turn authentication **On**.</span></span> <span data-ttu-id="6eebe-203">A **hitelesítésszolgáltatókat**, válassza a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6eebe-203">Under **Authentication Providers**, choose **Azure Active Directory**.</span></span>

   ![Hitelesítés bekapcsolása](./media/logic-apps-custom-hosted-api/custom-web-api-app-authentication.png)

3. <span data-ttu-id="6eebe-205">Most létrehoz egy alkalmazás azonosítóját, a webes alkalmazás vagy API-alkalmazás, ahogy az itt látható.</span><span class="sxs-lookup"><span data-stu-id="6eebe-205">Now create an application identity for your web app or API app as shown here.</span></span> <span data-ttu-id="6eebe-206">Az a **Azure Active Directory beállításai** panelen állítsa **üzemmód** való **Express**.</span><span class="sxs-lookup"><span data-stu-id="6eebe-206">On the **Azure Active Directory Settings** blade, set **Management mode** to **Express**.</span></span> <span data-ttu-id="6eebe-207">Válasszon **új AD-alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="6eebe-207">Choose **Create New AD App**.</span></span> <span data-ttu-id="6eebe-208">Nevezze el az alkalmazás azonosítóját, és válassza a **OK**.</span><span class="sxs-lookup"><span data-stu-id="6eebe-208">Give your application identity a name, and choose **OK**.</span></span> 

   ![Az Alkalmazásidentitás a webes alkalmazás vagy API-alkalmazás létrehozása](./media/logic-apps-custom-hosted-api/custom-api-application-identity.png)

4. <span data-ttu-id="6eebe-210">Az a **hitelesítési / engedélyezési panel**, válassza a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="6eebe-210">On the **Authentication / Authorization blade**, choose **Save**.</span></span>

<span data-ttu-id="6eebe-211">Most meg kell keresnie az ügyfél és bérlői azonosító az alkalmazásazonosító, amely a webes alkalmazás vagy API-alkalmazás van társítva.</span><span class="sxs-lookup"><span data-stu-id="6eebe-211">Now you must find the client ID and tenant ID for the application identity that's associated with your web app or API app.</span></span> <span data-ttu-id="6eebe-212">Ezek az azonosítók használhatja a 3. rész.</span><span class="sxs-lookup"><span data-stu-id="6eebe-212">You use these IDs in Part 3.</span></span> <span data-ttu-id="6eebe-213">Így folytatja ezeket a lépéseket az Azure-portálon vagy [a klasszikus Azure portálon](#find-id-classic).</span><span class="sxs-lookup"><span data-stu-id="6eebe-213">So continue with these steps for the Azure portal or [Azure classic portal](#find-id-classic).</span></span>

<span data-ttu-id="6eebe-214">**Alkalmazás azonosítója ügyfél-azonosító és Bérlőazonosító keresése a webes alkalmazás vagy API-alkalmazás az Azure-portálon**</span><span class="sxs-lookup"><span data-stu-id="6eebe-214">**Find application identity's client ID and tenant ID for your web app or API app in the Azure portal**</span></span>

1. <span data-ttu-id="6eebe-215">A **hitelesítésszolgáltatókat**, válassza a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6eebe-215">Under **Authentication Providers**, choose **Azure Active Directory**.</span></span> 

   ![Válassza ki a "Az Azure Active Directory"](./media/logic-apps-custom-hosted-api/custom-api-app-identity-client-id-tenant-id.png)

2. <span data-ttu-id="6eebe-217">Az a **Azure Active Directory beállításai** panelen állítsa **üzemmód** való **speciális**.</span><span class="sxs-lookup"><span data-stu-id="6eebe-217">On the **Azure Active Directory Settings** blade, set **Management mode** to **Advanced**.</span></span>

3. <span data-ttu-id="6eebe-218">Másolás a **ügyfél-azonosító**, és mentse a GUID azonosítóját használja a 3. rész.</span><span class="sxs-lookup"><span data-stu-id="6eebe-218">Copy the **Client ID**, and save that GUID for use in Part 3.</span></span>

   > [!TIP] 
   > <span data-ttu-id="6eebe-219">Ha **ügyfél-azonosító** és **kiállítójának URL-címe** nem jelenik meg, próbálja meg frissíteni az Azure-portálon, és ismételje meg az 1.</span><span class="sxs-lookup"><span data-stu-id="6eebe-219">If **Client ID** and **Issuer Url** don't appear, try refreshing the Azure portal, and repeat Step 1.</span></span>

4. <span data-ttu-id="6eebe-220">A **kiállítójának URL-címe**, másolja ki és mentse a GUID csak a 3. rész.</span><span class="sxs-lookup"><span data-stu-id="6eebe-220">Under **Issuer Url**, copy and save just the GUID for Part 3.</span></span> <span data-ttu-id="6eebe-221">Is használhatja a GUID a webalkalmazás vagy API-alkalmazás központi telepítési sablont, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="6eebe-221">You can also use this GUID in your web app or API app's deployment template, if necessary.</span></span>

   <span data-ttu-id="6eebe-222">A GUID az adott bérlő GUID-azonosító ("bérlő azonosítója"), és meg kell jelennie az URL-cím:`https://sts.windows.net/{GUID}`</span><span class="sxs-lookup"><span data-stu-id="6eebe-222">This GUID is your specific tenant's GUID ("tenant ID") and should appear in this URL: `https://sts.windows.net/{GUID}`</span></span>

5. <span data-ttu-id="6eebe-223">A módosítások mentése nélkül zárja be a **Azure Active Directory beállításai** panelen.</span><span class="sxs-lookup"><span data-stu-id="6eebe-223">Without saving your changes, close the **Azure Active Directory Settings** blade.</span></span>

<a name="find-id-classic"></a>

<span data-ttu-id="6eebe-224">**Alkalmazás azonosítója ügyfél-azonosító és Bérlőazonosító keresése a webes alkalmazás vagy API-alkalmazás a klasszikus Azure portálon**</span><span class="sxs-lookup"><span data-stu-id="6eebe-224">**Find application identity's client ID and tenant ID for your web app or API app in the Azure classic portal**</span></span>

1. <span data-ttu-id="6eebe-225">A klasszikus Azure portálon, válassza ki a [ **Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span><span class="sxs-lookup"><span data-stu-id="6eebe-225">In the Azure classic portal, choose [**Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span></span>

2.  <span data-ttu-id="6eebe-226">Válassza ki a címtárban, ami a webalkalmazás vagy API-alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="6eebe-226">Select the directory that you use for your web app or API app.</span></span>

3. <span data-ttu-id="6eebe-227">Az a **keresési** mezőben található, és válassza ki a webalkalmazás vagy API-alkalmazás az alkalmazásidentitás.</span><span class="sxs-lookup"><span data-stu-id="6eebe-227">In the **Search** box, find and select the application identity for your web app or API app.</span></span>

4. <span data-ttu-id="6eebe-228">Az a **konfigurálása** fülre, másolja a **ügyfél-azonosító**, és mentse a GUID azonosítóját használja a 3. rész.</span><span class="sxs-lookup"><span data-stu-id="6eebe-228">On the **Configure** tab, copy the **Client ID**, and save that GUID for use in Part 3.</span></span>

5. <span data-ttu-id="6eebe-229">Miután beszerezte az ügyfél-azonosító alján a **konfigurálása** lapra, majd **végpontok megtekintése**.</span><span class="sxs-lookup"><span data-stu-id="6eebe-229">After you get the client ID, at the bottom of the **Configure** tab, choose **View endpoints**.</span></span>

6. <span data-ttu-id="6eebe-230">Másolja az URL-címet **összevonási metaadat-dokumentum**, és keresse meg a URL-címet.</span><span class="sxs-lookup"><span data-stu-id="6eebe-230">Copy the URL for **Federation Metadata Document**, and browse to that URL.</span></span>

7. <span data-ttu-id="6eebe-231">A metaadat-dokumentum, amely megnyitja, keresse meg a legfelső szintű **EntityDescriptor azonosító** elemet, amely rendelkezik egy **entityid beállítást** attribútum ezen a képernyőn:`https://sts.windows.net/{GUID}`</span><span class="sxs-lookup"><span data-stu-id="6eebe-231">In the metadata document that opens, find the root **EntityDescriptor ID** element, which has an **entityID** attribute in this form: `https://sts.windows.net/{GUID}`</span></span> 

      <span data-ttu-id="6eebe-232">Ez az attribútum a GUID az adott bérlő GUID (bérlő azonosító).</span><span class="sxs-lookup"><span data-stu-id="6eebe-232">The GUID in this attribute is your specific tenant's GUID (tenant ID).</span></span>

8. <span data-ttu-id="6eebe-233">A bérlő azonosítója másolja ki és mentse használatra azonosító a 3. rész és használni a webalkalmazás vagy API-alkalmazás központi telepítési sablont, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="6eebe-233">Copy the tenant ID and save that ID for use in Part 3, and also to use in your web app or API app's deployment template, if necessary.</span></span>

<span data-ttu-id="6eebe-234">További információ az alábbi témakörökben talál:</span><span class="sxs-lookup"><span data-stu-id="6eebe-234">For more information, see these topics:</span></span>

* [<span data-ttu-id="6eebe-235">Az Azure App Service API-alkalmazások felhasználói hitelesítésének</span><span class="sxs-lookup"><span data-stu-id="6eebe-235">User authentication for API apps in Azure App Service</span></span>](../app-service-api/app-service-api-dotnet-user-principal-auth.md)
* [<span data-ttu-id="6eebe-236">Hitelesítési és engedélyezési az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="6eebe-236">Authentication and authorization in Azure App Service</span></span>](../app-service/app-service-authentication-overview.md)

<a name="authen-deploy"></a>

<span data-ttu-id="6eebe-237">**Amikor telepít egy Azure Resource Manager sablonnal hitelesítés bekapcsolása**</span><span class="sxs-lookup"><span data-stu-id="6eebe-237">**Turn on authentication when you deploy with an Azure Resource Manager template**</span></span>

<span data-ttu-id="6eebe-238">A logikai alkalmazásnak az alkalmazás identitását továbbra is készítsen a webes alkalmazás vagy API-alkalmazást, amely eltér az Azure AD alkalmazás identitást.</span><span class="sxs-lookup"><span data-stu-id="6eebe-238">You must still create an Azure AD application identity for your web app or API app that differs from the app identity for your logic app.</span></span> <span data-ttu-id="6eebe-239">Az alkalmazásazonosító létrehozásához kövesse az előző lépést a 2. rész a az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="6eebe-239">To create the application identity, follow the previous steps in Part 2 for the Azure portal.</span></span> <span data-ttu-id="6eebe-240">Is is hajtsa végre az 1. rész utasításait, de ügyeljen arra, hogy a webes alkalmazás vagy API-alkalmazás tényleges `https://{URL}` a **bejelentkezési URL-cím** és **App ID URI**.</span><span class="sxs-lookup"><span data-stu-id="6eebe-240">You can also follow the steps in Part 1, but make sure to use your web app or API app's actual `https://{URL}` for **Sign-on URL** and **App ID URI**.</span></span> <span data-ttu-id="6eebe-241">A fenti lépéseket hogy menti az ügyfél-azonosító és a bérlő azonosítója használható az alkalmazás központi telepítési sablont és a 3. rész.</span><span class="sxs-lookup"><span data-stu-id="6eebe-241">From these steps, you have to save both the client ID and tenant ID for use in your app's deployment template and also for Part 3.</span></span>

> [!NOTE]
> <span data-ttu-id="6eebe-242">Az Azure AD identitása a webes alkalmazás vagy API-alkalmazás létrehozásakor az Azure-portálon vagy a klasszikus Azure portálon, nem pedig PowerShell kell használnia.</span><span class="sxs-lookup"><span data-stu-id="6eebe-242">When you create the Azure AD application identity for your web app or API app, you must use the Azure portal or Azure classic portal, rather than PowerShell.</span></span> <span data-ttu-id="6eebe-243">A PowerShell-parancsmag segítségével nem állítsa be a szükséges engedélyekkel a felhasználók bejelentkeznek egy webhelyet.</span><span class="sxs-lookup"><span data-stu-id="6eebe-243">The PowerShell commandlet doesn't set up the required permissions to sign users into a website.</span></span>

<span data-ttu-id="6eebe-244">Az ügyfél és bérlői azonosító kap, miután tartalmazza ezek az azonosítók egy subresource a webalkalmazás vagy a központi telepítési sablont API-alkalmazásba:</span><span class="sxs-lookup"><span data-stu-id="6eebe-244">After you get the client ID and tenant ID, include these IDs as a subresource of your web app or API app in your deployment template:</span></span>

   ```
   "resources": [
       {
           "apiVersion": "2015-08-01",
           "name": "web",
           "type": "config",
           "dependsOn": ["[concat('Microsoft.Web/sites/','parameters('webAppName'))]"],
           "properties": {
               "siteAuthEnabled": true,
               "siteAuthSettings": {
                 "clientId": "{client-ID}",
                 "issuer": "https://sts.windows.net/{tenant-ID}/",
               }
           }
       }
   ]
   ```

<span data-ttu-id="6eebe-245">Automatikus telepítése egy üres web app és az Azure Active Directory-hitelesítés, és logikai alkalmazás [sablon megjelenítése a teljes Itt](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json), vagy kattintson a **az Azure telepítéséhez** itt:</span><span class="sxs-lookup"><span data-stu-id="6eebe-245">To automatically deploy a blank web app and a logic app together with Azure Active Directory authentication, [view the complete template here](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json), or click **Deploy to Azure** here:</span></span>

<span data-ttu-id="6eebe-246">[![Üzembe helyezés az Azure-ban](media/logic-apps-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="6eebe-246">[![Deploy to Azure](media/logic-apps-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)</span></span>

#### <a name="part-3-populate-the-authorization-section-in-your-logic-app"></a><span data-ttu-id="6eebe-247">3. lépés: A Logic Apps alkalmazást engedélyezési című feltöltése</span><span class="sxs-lookup"><span data-stu-id="6eebe-247">Part 3: Populate the Authorization section in your logic app</span></span>

<span data-ttu-id="6eebe-248">Az előző címsablonnak már beállítása engedélyezési ismertetjük, de ha közvetlenül a logic app, meg kell adni a teljes körű engedéllyel szakaszban.</span><span class="sxs-lookup"><span data-stu-id="6eebe-248">The previous template already has this authorization section set up, but if you are directly authoring the logic app, you must include the full authorization section.</span></span>

<span data-ttu-id="6eebe-249">Nyissa meg a logic app-definíciót a kód nézetre, lépjen a **HTTP** művelet a szakaszban található a **engedélyezési** szakaszt, és ezt a sort tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="6eebe-249">Open your logic app definition in code view, go to the **HTTP** action section, find the **Authorization** section, and include this line:</span></span>

`{"tenant": "{tenant-ID}", "audience": "{client-ID-from-Part-2-web-app-or-API app}", "clientId": "{client-ID-from-Part-1-logic-app}", "secret": "{key-from-Part-1-logic-app}", "type": "ActiveDirectoryOAuth" }`

| <span data-ttu-id="6eebe-250">Elem</span><span class="sxs-lookup"><span data-stu-id="6eebe-250">Element</span></span> | <span data-ttu-id="6eebe-251">Leírás</span><span class="sxs-lookup"><span data-stu-id="6eebe-251">Description</span></span> |
| ------- | ----------- |
| <span data-ttu-id="6eebe-252">Bérlői</span><span class="sxs-lookup"><span data-stu-id="6eebe-252">tenant</span></span> |<span data-ttu-id="6eebe-253">Az Azure AD-bérlő GUID azonosítója</span><span class="sxs-lookup"><span data-stu-id="6eebe-253">The GUID for the Azure AD tenant</span></span> |
| <span data-ttu-id="6eebe-254">Célközönség</span><span class="sxs-lookup"><span data-stu-id="6eebe-254">audience</span></span> |<span data-ttu-id="6eebe-255">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="6eebe-255">Required.</span></span> <span data-ttu-id="6eebe-256">A GUID azonosítóját a cél erőforráson, amely a használni kívánt - az alkalmazásidentitás a webes alkalmazás vagy API-alkalmazás az ügyfél-azonosító</span><span class="sxs-lookup"><span data-stu-id="6eebe-256">The GUID for the target resource that you want to access - the client ID from the application identity for your web app or API app</span></span> |
| <span data-ttu-id="6eebe-257">clientId</span><span class="sxs-lookup"><span data-stu-id="6eebe-257">clientId</span></span> |<span data-ttu-id="6eebe-258">A hozzáférés - az ügyfél-Azonosítót a logikai alkalmazásnak az alkalmazásidentitás a kérő ügyfél GUID azonosítója</span><span class="sxs-lookup"><span data-stu-id="6eebe-258">The GUID for the client requesting access - the client ID from the application identity for your logic app</span></span> |
| <span data-ttu-id="6eebe-259">titkos kulcs</span><span class="sxs-lookup"><span data-stu-id="6eebe-259">secret</span></span> |<span data-ttu-id="6eebe-260">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="6eebe-260">Required.</span></span> <span data-ttu-id="6eebe-261">A kulcs és a jelszót az alkalmazásazonosító, hogy az ügyfél által kért a hozzáférési jogkivonat</span><span class="sxs-lookup"><span data-stu-id="6eebe-261">The key or password from the application identity for the client that's requesting the access token</span></span> |
| <span data-ttu-id="6eebe-262">type</span><span class="sxs-lookup"><span data-stu-id="6eebe-262">type</span></span> |<span data-ttu-id="6eebe-263">A hitelesítési típus.</span><span class="sxs-lookup"><span data-stu-id="6eebe-263">The authentication type.</span></span> <span data-ttu-id="6eebe-264">ActiveDirectoryOAuth hitelesítéshez értéke `ActiveDirectoryOAuth`.</span><span class="sxs-lookup"><span data-stu-id="6eebe-264">For ActiveDirectoryOAuth authentication, the value is `ActiveDirectoryOAuth`.</span></span> |

<span data-ttu-id="6eebe-265">Példa:</span><span class="sxs-lookup"><span data-stu-id="6eebe-265">For example:</span></span>

```
{
   ...
   "actions": {
      "some-action": {
         "conditions": [],
         "inputs": {
            "method": "post",
            "uri": "https://your-api-azurewebsites.net/api/your-method",
            "authentication": {
               "tenant": "tenant-ID",
               "audience": "client-ID-from-azure-ad-app-for-web-app-or-api-app",
               "clientId": "client-ID-from-azure-ad-app-for-logic-app",
               "secret": "key-from-azure-ad-app-for-logic-app",
               "type": "ActiveDirectoryOAuth"
            }
         },
      }
   }
}
```

<a name="update-code"></a>

### <a name="secure-api-calls-through-code"></a><span data-ttu-id="6eebe-266">Kód biztonságos API-hívások</span><span class="sxs-lookup"><span data-stu-id="6eebe-266">Secure API calls through code</span></span>

<a name="certificate"></a>

#### <a name="certificate-authentication"></a><span data-ttu-id="6eebe-267">Tanúsítványhitelesítés</span><span class="sxs-lookup"><span data-stu-id="6eebe-267">Certificate authentication</span></span>

<span data-ttu-id="6eebe-268">A bejövő kéréseket a Logic Apps alkalmazást-webalkalmazást vagy API-alkalmazás az érvényesítéshez ügyfél tanúsítványokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="6eebe-268">To validate the incoming requests from your logic app to your web app or API app, you can use client certificates.</span></span> <span data-ttu-id="6eebe-269">Állítsa be a kódját, ismerje meg [TLS kölcsönös hitelesítés konfigurálásával](../app-service-web/app-service-web-configure-tls-mutual-auth.md).</span><span class="sxs-lookup"><span data-stu-id="6eebe-269">To set up your code, learn [how to configure TLS mutual authentication](../app-service-web/app-service-web-configure-tls-mutual-auth.md).</span></span>

<span data-ttu-id="6eebe-270">Az a **engedélyezési** szakaszban, ezt a sort tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="6eebe-270">In the **Authorization** section, include this line:</span></span> 

`{"type": "clientcertificate", "password": "password", "pfx": "long-pfx-key"}`

| <span data-ttu-id="6eebe-271">Elem</span><span class="sxs-lookup"><span data-stu-id="6eebe-271">Element</span></span> | <span data-ttu-id="6eebe-272">Leírás</span><span class="sxs-lookup"><span data-stu-id="6eebe-272">Description</span></span> |
| ------- | ----------- |
| <span data-ttu-id="6eebe-273">type</span><span class="sxs-lookup"><span data-stu-id="6eebe-273">type</span></span> |<span data-ttu-id="6eebe-274">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="6eebe-274">Required.</span></span> <span data-ttu-id="6eebe-275">A hitelesítési típus.</span><span class="sxs-lookup"><span data-stu-id="6eebe-275">The authentication type.</span></span> <span data-ttu-id="6eebe-276">Az ügyfél SSL-tanúsítványok, az értéknek kell lennie `ClientCertificate`.</span><span class="sxs-lookup"><span data-stu-id="6eebe-276">For SSL client certificates, the value must be `ClientCertificate`.</span></span> |
| <span data-ttu-id="6eebe-277">jelszó</span><span class="sxs-lookup"><span data-stu-id="6eebe-277">password</span></span> |<span data-ttu-id="6eebe-278">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="6eebe-278">Required.</span></span> <span data-ttu-id="6eebe-279">Az ügyféltanúsítvány (PFX-fájl) eléréséhez szükséges jelszót</span><span class="sxs-lookup"><span data-stu-id="6eebe-279">The password for accessing the client certificate (PFX file)</span></span> |
| <span data-ttu-id="6eebe-280">PFX</span><span class="sxs-lookup"><span data-stu-id="6eebe-280">pfx</span></span> |<span data-ttu-id="6eebe-281">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="6eebe-281">Required.</span></span> <span data-ttu-id="6eebe-282">Base64-kódolású tartalmak az ügyféltanúsítvány (PFX-fájl)</span><span class="sxs-lookup"><span data-stu-id="6eebe-282">Base64-encoded contents of the client certificate (PFX file)</span></span> |

<a name="basic"></a>

#### <a name="basic-authentication"></a><span data-ttu-id="6eebe-283">Az egyszerű hitelesítés</span><span class="sxs-lookup"><span data-stu-id="6eebe-283">Basic authentication</span></span>

<span data-ttu-id="6eebe-284">Ellenőrizze a webalkalmazás vagy API-alkalmazás a Logic Apps alkalmazást a bejövő kéréseket, alapszintű hitelesítés, mint például a felhasználónév és jelszó is használhat.</span><span class="sxs-lookup"><span data-stu-id="6eebe-284">To validate incoming requests from your logic app to your web app or API app, you can use basic authentication, such as a username and password.</span></span> <span data-ttu-id="6eebe-285">Az egyszerű hitelesítés egy közös mintát, és a hitelesítés a webalkalmazás vagy API-alkalmazás létrehozásához használt nyelvi is használhatja.</span><span class="sxs-lookup"><span data-stu-id="6eebe-285">Basic authentication is a common pattern, and you can use this authentication in any language used to build your web app or API app.</span></span>

<span data-ttu-id="6eebe-286">Az a **engedélyezési** szakaszban, ezt a sort tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="6eebe-286">In the **Authorization** section, include this line:</span></span>

<span data-ttu-id="6eebe-287">`{"type": "basic", "username": "username", "password": "password"}`.</span><span class="sxs-lookup"><span data-stu-id="6eebe-287">`{"type": "basic", "username": "username", "password": "password"}`.</span></span>

| <span data-ttu-id="6eebe-288">Elem</span><span class="sxs-lookup"><span data-stu-id="6eebe-288">Element</span></span> | <span data-ttu-id="6eebe-289">Leírás</span><span class="sxs-lookup"><span data-stu-id="6eebe-289">Description</span></span> |
| --- | --- |
| <span data-ttu-id="6eebe-290">type</span><span class="sxs-lookup"><span data-stu-id="6eebe-290">type</span></span> |<span data-ttu-id="6eebe-291">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="6eebe-291">Required.</span></span> <span data-ttu-id="6eebe-292">A hitelesítési típus.</span><span class="sxs-lookup"><span data-stu-id="6eebe-292">The authentication type.</span></span> <span data-ttu-id="6eebe-293">Az alapszintű hitelesítés, az értéknek kell lennie `Basic`.</span><span class="sxs-lookup"><span data-stu-id="6eebe-293">For basic authentication, the value must be `Basic`.</span></span> |
| <span data-ttu-id="6eebe-294">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="6eebe-294">username</span></span> |<span data-ttu-id="6eebe-295">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="6eebe-295">Required.</span></span> <span data-ttu-id="6eebe-296">A felhasználónév-hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="6eebe-296">The username for authentication</span></span> |
| <span data-ttu-id="6eebe-297">jelszó</span><span class="sxs-lookup"><span data-stu-id="6eebe-297">password</span></span> |<span data-ttu-id="6eebe-298">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="6eebe-298">Required.</span></span> <span data-ttu-id="6eebe-299">A jelszó a hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="6eebe-299">The password for authentication</span></span> |

<a name="azure-ad-code"></a>

#### <a name="azure-active-directory-authentication-through-code"></a><span data-ttu-id="6eebe-300">Az Azure Active Directory hitelesítési kód</span><span class="sxs-lookup"><span data-stu-id="6eebe-300">Azure Active Directory authentication through code</span></span>

<span data-ttu-id="6eebe-301">Alapértelmezés szerint az Azure AD-alapú hitelesítés, amely az Azure portálon kapcsolja be a minden részletre kiterjedő engedélyezési nem biztosít.</span><span class="sxs-lookup"><span data-stu-id="6eebe-301">By default, the Azure AD authentication that you turn on in the Azure portal doesn't provide fine-grained authorization.</span></span> <span data-ttu-id="6eebe-302">Ez a hitelesítés például az API-t egy adott bérlő, nem pedig egy adott felhasználó vagy alkalmazás zárolja.</span><span class="sxs-lookup"><span data-stu-id="6eebe-302">For example, this authentication locks your API to just a specific tenant, not to a specific user or app.</span></span> 

<span data-ttu-id="6eebe-303">API-hozzáférés korlátozása kód a Logic Apps alkalmazást, bontsa ki a fejlécet, amely rendelkezik a JSON webes jogkivonat (JWT).</span><span class="sxs-lookup"><span data-stu-id="6eebe-303">To restrict API access to your logic app through code, extract the header that has the JSON web token (JWT).</span></span> <span data-ttu-id="6eebe-304">Ellenőrizze a hívó identitását, és nem egyezik a kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="6eebe-304">Check the caller's identity, and reject requests that don't match.</span></span>

<span data-ttu-id="6eebe-305">Továbblép, a hitelesítés megvalósításához teljes mértékben az Ön saját kódját, és nem használható az Azure-portálon megtudhatja, hogyan [hitelesítik magukat a helyszíni Active Directory, az Azure alkalmazásban](../app-service-web/web-sites-authentication-authorization.md).</span><span class="sxs-lookup"><span data-stu-id="6eebe-305">Going further, to implement this authentication entirely in your own code, and not use the Azure portal, learn how to [authenticate with on-premises Active Directory in your Azure app](../app-service-web/web-sites-authentication-authorization.md).</span></span>

<span data-ttu-id="6eebe-306">Hozzon létre egy alkalmazás-azonosítót a logikai alkalmazásnak, és identitás segítségével az API-hívás, kövesse az előző lépéseket.</span><span class="sxs-lookup"><span data-stu-id="6eebe-306">To create an application identity for your logic app and use that identity to call your API, you must follow the previous steps.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6eebe-307">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6eebe-307">Next steps</span></span>

* [<span data-ttu-id="6eebe-308">Ellenőrizze, hogy logic app teljesítményében diagnosztikai naplók és riasztások</span><span class="sxs-lookup"><span data-stu-id="6eebe-308">Check logic app performance with diagnostic logs and alerts</span></span>](logic-apps-monitor-your-logic-apps.md)