---
title: "aaaDeploy, hívja, és a webes API-k & REST API-k hitelesítéshez az Azure Logic Apps |} Microsoft Docs"
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
ms.openlocfilehash: ca1e4f28196b21f43b7c9ab94029684121e36f63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-call-and-authenticate-custom-apis-as-connectors-for-logic-apps"></a><span data-ttu-id="226f6-104">Üzembe helyezéséhez, hívja, és egyéni API-k hitelesítse magát a logic apps összekötők</span><span class="sxs-lookup"><span data-stu-id="226f6-104">Deploy, call, and authenticate custom APIs as connectors for logic apps</span></span>

<span data-ttu-id="226f6-105">Miután [hozzon létre egyéni API-k](./logic-apps-create-api-app.md) , amely biztosít műveletek vagy eseményindítók toouse a logic apps munkafolyamatok, telepítenie kell az API-k hívása előtt be őket.</span><span class="sxs-lookup"><span data-stu-id="226f6-105">After you [create custom APIs](./logic-apps-create-api-app.md) that provide actions or triggers toouse in logic apps workflows, you must deploy your APIs before you can call them.</span></span> <span data-ttu-id="226f6-106">Bár a logic App hello legjobb élmény meghívhatja API-k, adja hozzá [Swagger-metaadatok](http://swagger.io/specification/) , amely az API-műveleteket és paramétereket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="226f6-106">And although you can call any API from a logic app, for hello best experience, add [Swagger metadata](http://swagger.io/specification/) that describes your API's operations and parameters.</span></span> <span data-ttu-id="226f6-107">A Swagger-fájl az API-együttműködését, és könnyebben integrálhatja a logic apps segítségével.</span><span class="sxs-lookup"><span data-stu-id="226f6-107">This Swagger file helps your API work better and integrate more easily with logic apps.</span></span>

<span data-ttu-id="226f6-108">Telepítheti az API-k [webalkalmazások](../app-service-web/app-service-web-overview.md), de az API-k érdemes megfontolni [API-alkalmazások](../app-service-api/app-service-api-apps-why-best-platform.md), amely megkönnyítheti a projekt felépítéséhez, üzemeltetni, és felhasználhatják az API-k hello felhőben és a helyszíni.</span><span class="sxs-lookup"><span data-stu-id="226f6-108">You can deploy your APIs as [web apps](../app-service-web/app-service-web-overview.md), but consider deploying your APIs as [API apps](../app-service-api/app-service-api-apps-why-best-platform.md), which can make your job easier when you build, host, and consume APIs in hello cloud and on premises.</span></span> <span data-ttu-id="226f6-109">Nincs toochange összes kódot az API-k előnyeit – csupán telepítse a kód tooan API-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="226f6-109">You don't have toochange any code in your APIs -- just deploy your code tooan API app.</span></span> <span data-ttu-id="226f6-110">Az API-kat tárolhatja a [Azure App Service](../app-service/app-service-value-prop-what-is.md), a platform,--szolgáltatás (PaaS) ajánlat, amely nyújt hello legjobb legegyszerűbb és legjobban méretezhető módszerek valamelyikével API üzemeltetéséhez.</span><span class="sxs-lookup"><span data-stu-id="226f6-110">You can host your APIs on [Azure App Service](../app-service/app-service-value-prop-what-is.md), a platform-as-a-service (PaaS) offering that provides one of hello best, easiest, and most scalable ways for API hosting.</span></span>

<span data-ttu-id="226f6-111">logic apps tooyour API-k tooauthenticate hívást, állíthat be Azure Active Directory hello az Azure-portálon így nem kell tooupdate a kódot.</span><span class="sxs-lookup"><span data-stu-id="226f6-111">tooauthenticate calls from logic apps tooyour APIs, you can set up Azure Active Directory in hello Azure portal so you don't have tooupdate your code.</span></span> <span data-ttu-id="226f6-112">Vagy igényelnek, és kényszeríteni a hitelesítést az API-kódban keresztül.</span><span class="sxs-lookup"><span data-stu-id="226f6-112">Or, you can require and enforce authentication through your API's code.</span></span>

## <a name="deploy-your-api-as-a-web-app-or-api-app"></a><span data-ttu-id="226f6-113">Az API-webalkalmazást vagy API-alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="226f6-113">Deploy your API as a web app or API app</span></span>

<span data-ttu-id="226f6-114">Az egyéni API-t a logikai alkalmazás hívása előtt telepítse a webes alkalmazás vagy API app tooAzure App Service API.</span><span class="sxs-lookup"><span data-stu-id="226f6-114">Before you can call your custom API from a logic app, deploy your API as a web app or API app tooAzure App Service.</span></span> <span data-ttu-id="226f6-115">Ezenkívül toomake a Swagger-dokumentum hello Logic App Designer által is olvasható hello API-definíció tulajdonságainak beállítása, és kapcsolja be a [eltérő eredetű erőforrások megosztása (CORS)](../app-service-api/app-service-api-cors-consume-javascript.md#corsconfig) a webes alkalmazás vagy API-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="226f6-115">Also, toomake your Swagger document readable by hello Logic App Designer, set hello API definition properties and turn on [cross-origin resource sharing (CORS)](../app-service-api/app-service-api-cors-consume-javascript.md#corsconfig) for your web app or API app.</span></span>

1. <span data-ttu-id="226f6-116">Hello Azure-portálon válassza ki a webalkalmazás vagy API-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="226f6-116">In hello Azure portal, select your web app or API app.</span></span>

2. <span data-ttu-id="226f6-117">Hello panelen, amely megnyitja a **API**, válassza a **API-definíció**.</span><span class="sxs-lookup"><span data-stu-id="226f6-117">In hello blade that opens, under **API**, choose **API definition**.</span></span> <span data-ttu-id="226f6-118">Set hello **API definition hely** toohello a swagger.json fájl URL-címe.</span><span class="sxs-lookup"><span data-stu-id="226f6-118">Set hello **API definition location** toohello URL for your swagger.json file.</span></span>

   <span data-ttu-id="226f6-119">Általában hello URL-cím jelenik meg, a következő formátumban:`https://{name}.azurewebsites.net/swagger/docs/v1)`</span><span class="sxs-lookup"><span data-stu-id="226f6-119">Usually, hello URL appears in this format: `https://{name}.azurewebsites.net/swagger/docs/v1)`</span></span>

   ![Az egyéni API-hivatkozás tooSwagger fájlja](media/logic-apps-custom-hosted-api/custom-api-swagger-url.png)

3. <span data-ttu-id="226f6-121">A **API**, válassza a **CORS**.</span><span class="sxs-lookup"><span data-stu-id="226f6-121">Under **API**, choose **CORS**.</span></span> <span data-ttu-id="226f6-122">Állítsa be a CORS-házirend hello a **engedélyezett eredeteket** túl**"*"** (minden engedélyezése).</span><span class="sxs-lookup"><span data-stu-id="226f6-122">Set hello CORS policy for **Allowed origins** too**'*'** (allow all).</span></span>

   <span data-ttu-id="226f6-123">Ez a beállítás lehetővé teszi a kérelmek az Logic App-tervezőből.</span><span class="sxs-lookup"><span data-stu-id="226f6-123">This setting permits requests from Logic App Designer.</span></span>

   ![A Logic App Designer tooyour egyéni API kérések engedélyezéséhez](media/logic-apps-custom-hosted-api/custom-api-cors.png)

<span data-ttu-id="226f6-125">További információkért lásd: ezek a cikkek:</span><span class="sxs-lookup"><span data-stu-id="226f6-125">For more information, see these articles:</span></span>

* [<span data-ttu-id="226f6-126">Adja hozzá a Swagger-metaadatokat az ASP.NET web API-k</span><span class="sxs-lookup"><span data-stu-id="226f6-126">Add Swagger metadata for ASP.NET web APIs</span></span>](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui)
* [<span data-ttu-id="226f6-127">Az ASP.NET web API-k tooAzure App Service telepítése</span><span class="sxs-lookup"><span data-stu-id="226f6-127">Deploy ASP.NET web APIs tooAzure App Service</span></span>](../app-service-api/app-service-api-dotnet-get-started.md)

## <a name="call-your-custom-api-from-logic-app-workflows"></a><span data-ttu-id="226f6-128">Az egyéni API-t hívja a logic app munkafolyamatok</span><span class="sxs-lookup"><span data-stu-id="226f6-128">Call your custom API from logic app workflows</span></span>

<span data-ttu-id="226f6-129">Hello API-definíció tulajdonságait és a CORS beállítása után az egyéni API-eseményindítók és műveletek kell válnia tooinclude a logic app munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="226f6-129">After you set up hello API definition properties and CORS, your custom API's triggers and actions should become available for you tooinclude in your logic app workflow.</span></span> 

*  <span data-ttu-id="226f6-130">tooview hello webhelyeket, amelyeken a Swagger URL-címek, megkeresheti a Logic Apps Designer hello előfizetés webhelyek.</span><span class="sxs-lookup"><span data-stu-id="226f6-130">tooview hello websites that have Swagger URLs, you can browse your subscription websites in hello Logic Apps Designer.</span></span>

*  <span data-ttu-id="226f6-131">tooview elérhető műveletek és a bemenetek húzni a Swagger-dokumentum, használja a hello [HTTP + Swagger műveleti](../connectors/connectors-native-http-swagger.md).</span><span class="sxs-lookup"><span data-stu-id="226f6-131">tooview available actions and inputs by pointing at a Swagger document, use hello [HTTP + Swagger action](../connectors/connectors-native-http-swagger.md).</span></span>

*  <span data-ttu-id="226f6-132">toocall API-k, beleértve az API-k, amelyek nem rendelkeznek, vagy teszi közzé a Swagger-dokumentum mindig hozhat létre egy kérelem hello [HTTP-művelet](../connectors/connectors-native-http.md).</span><span class="sxs-lookup"><span data-stu-id="226f6-132">toocall any API, including APIs that don't have or expose a Swagger document, you can always create a request with hello [HTTP action](../connectors/connectors-native-http.md).</span></span>

## <a name="authenticate-calls-tooyour-custom-api"></a><span data-ttu-id="226f6-133">Hitelesítés hívások tooyour egyéni API</span><span class="sxs-lookup"><span data-stu-id="226f6-133">Authenticate calls tooyour custom API</span></span>

<span data-ttu-id="226f6-134">Biztonságossá teheti a hívások tooyour egyéni API a következő lehetőségeket biztosítva:</span><span class="sxs-lookup"><span data-stu-id="226f6-134">You can secure calls tooyour custom API in these ways:</span></span>

* <span data-ttu-id="226f6-135">[Nincs kódmódosításokat](#no-code): az API-t védelme [Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) keresztül hello Azure-portálon, ezért nem van tooupdate a kódot, vagy telepítse újra az API-t.</span><span class="sxs-lookup"><span data-stu-id="226f6-135">[No code changes](#no-code): Protect your API with [Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) through hello Azure portal, so you don't have tooupdate your code or redeploy your API.</span></span>

  > [!NOTE]
  > <span data-ttu-id="226f6-136">Alapértelmezés szerint hello Azure AD-alapú hitelesítés, amely a hello Azure-portálon kapcsolja be a minden részletre kiterjedő engedélyezési nem biztosít.</span><span class="sxs-lookup"><span data-stu-id="226f6-136">By default, hello Azure AD authentication that you turn on in hello Azure portal doesn't provide fine-grained authorization.</span></span> <span data-ttu-id="226f6-137">Ez a hitelesítés például zárolja a API toojust, egy adott bérlő, nem tooa adott felhasználó vagy alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="226f6-137">For example, this authentication locks your API toojust a specific tenant, not tooa specific user or app.</span></span> 

* <span data-ttu-id="226f6-138">[Frissítse az API-kódot](#update-code): az API védelme kényszerítésével [tanúsítványhitelesítés](#certificate), [az egyszerű hitelesítés](#basic), vagy [az Azure AD-alapú hitelesítés](#azure-ad-code) kód.</span><span class="sxs-lookup"><span data-stu-id="226f6-138">[Update your API's code](#update-code): Protect your API by enforcing [certificate authentication](#certificate), [basic authentication](#basic), or [Azure AD authentication](#azure-ad-code) through code.</span></span>

<a name="no-code"></a>

### <a name="authenticate-calls-tooyour-api-without-changing-code"></a><span data-ttu-id="226f6-139">Hitelesítés hívások tooyour API programkód módosítása nélkül</span><span class="sxs-lookup"><span data-stu-id="226f6-139">Authenticate calls tooyour API without changing code</span></span>

<span data-ttu-id="226f6-140">Ez a módszer hello általános lépései a következő:</span><span class="sxs-lookup"><span data-stu-id="226f6-140">Here's hello general steps for this method:</span></span>

1. <span data-ttu-id="226f6-141">Hozzon létre két [Azure Active Directory (Azure AD) alkalmazás identitások](../app-service-api/app-service-api-dotnet-service-principal-auth.md): egy a Logic Apps alkalmazást, egy, a webes alkalmazás (vagy API-alkalmazás).</span><span class="sxs-lookup"><span data-stu-id="226f6-141">Create two [Azure Active Directory (Azure AD) application identities](../app-service-api/app-service-api-dotnet-service-principal-auth.md): one for your logic app and one for your web app (or API app).</span></span>

2. <span data-ttu-id="226f6-142">tooauthenticate hívások tooyour API-t hello hitelesítő adatait (ügyfél-Azonosítót és titkos kulcs) használni hello [egyszerű](../app-service-api/app-service-api-dotnet-service-principal-auth.md) , amelyhez társítva hello a Logic Apps alkalmazást az Azure AD alkalmazás azonosítója.</span><span class="sxs-lookup"><span data-stu-id="226f6-142">tooauthenticate calls tooyour API, use hello credentials (client ID and secret) for hello [service principal](../app-service-api/app-service-api-dotnet-service-principal-auth.md) that's associated with hello Azure AD application identity for your logic app.</span></span>

3. <span data-ttu-id="226f6-143">Hello alkalmazás azonosítók szerepeljenek a logic app-definíciót.</span><span class="sxs-lookup"><span data-stu-id="226f6-143">Include hello application IDs in your logic app definition.</span></span>

#### <a name="part-1-create-an-azure-ad-application-identity-for-your-logic-app"></a><span data-ttu-id="226f6-144">1. lépés: Hozzon létre egy Azure AD alkalmazás-azonosítót a logikai alkalmazásnak</span><span class="sxs-lookup"><span data-stu-id="226f6-144">Part 1: Create an Azure AD application identity for your logic app</span></span>

<span data-ttu-id="226f6-145">A Logic Apps alkalmazást az Azure AD alkalmazás identitás tooauthenticate szemben az Azure AD használja.</span><span class="sxs-lookup"><span data-stu-id="226f6-145">Your logic app uses this Azure AD application identity tooauthenticate against Azure AD.</span></span> <span data-ttu-id="226f6-146">Csak akkor kell tooset ezzel az identitással be egyszer a címtáron.</span><span class="sxs-lookup"><span data-stu-id="226f6-146">You only have tooset up this identity one time for your directory.</span></span> <span data-ttu-id="226f6-147">Például kiválaszthatja toouse hello azonos identitást a logic apps, annak ellenére, hogy minden logikai alkalmazás egyedi azonosítók hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="226f6-147">For example, you can choose toouse hello same identity for all your logic apps, even though you can create unique identities for each logic app.</span></span> <span data-ttu-id="226f6-148">Ezeket az identitásokat az Azure-portálon hello állíthat be [a klasszikus Azure portálon](#app-identity-logic-classic), vagy használjon [PowerShell](#powershell).</span><span class="sxs-lookup"><span data-stu-id="226f6-148">You can set up these identities in hello Azure portal, [Azure classic portal](#app-identity-logic-classic), or use [PowerShell](#powershell).</span></span>

<span data-ttu-id="226f6-149">**Hozzon létre a logikai alkalmazásnak hello Alkalmazásidentitás hello Azure-portálon**</span><span class="sxs-lookup"><span data-stu-id="226f6-149">**Create hello application identity for your logic app in hello Azure portal**</span></span>

1. <span data-ttu-id="226f6-150">A hello [Azure-portálon](https://portal.azure.com "https://portal.azure.com"), válassza a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="226f6-150">In hello [Azure portal](https://portal.azure.com "https://portal.azure.com"), choose **Azure Active Directory**.</span></span> 

2. <span data-ttu-id="226f6-151">Győződjön meg arról, hogy hello bejelentkezés a webes alkalmazás vagy API-alkalmazás könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="226f6-151">Confirm that you're in hello same directory as your web app or API app.</span></span>

   > [!TIP]
   > <span data-ttu-id="226f6-152">tooswitch könyvtárak, kattintson a profil, és válasszon másik címtárat.</span><span class="sxs-lookup"><span data-stu-id="226f6-152">tooswitch directories, click your profile and select another directory.</span></span> <span data-ttu-id="226f6-153">Másik lehetőségként válasszon **áttekintése** > **kapcsoló directory**.</span><span class="sxs-lookup"><span data-stu-id="226f6-153">Or, choose **Overview** > **Switch directory**.</span></span>

3. <span data-ttu-id="226f6-154">Hello directory menü alatt **kezelése**, válassza a **App regisztrációk** > **új alkalmazás regisztrációja**.</span><span class="sxs-lookup"><span data-stu-id="226f6-154">On hello directory menu, under **Manage**, choose **App registrations** > **New application registration**.</span></span>

   > [!TIP]
   > <span data-ttu-id="226f6-155">Alapértelmezés szerint hello app regisztrációk listán látható az összes alkalmazás regisztrációja a címtárban.</span><span class="sxs-lookup"><span data-stu-id="226f6-155">By default, hello app registrations list shows all app registrations in your directory.</span></span> <span data-ttu-id="226f6-156">tooview csak az alkalmazás regisztrációk, tovább toohello keresőmezőbe, válasszon **alkalmazásaimat**.</span><span class="sxs-lookup"><span data-stu-id="226f6-156">tooview only your app registrations, next toohello search box, select **My apps**.</span></span> 

   ![Hozzon létre új alkalmazás regisztrálása](./media/logic-apps-custom-hosted-api/new-app-registration-azure-portal.png)

4. <span data-ttu-id="226f6-158">Nevezze el az alkalmazás azonosítóját, hagyja **alkalmazástípus** túl beállítása**Web app / API**, adjon meg egy egyedi karakterláncot formázni egy tartományt a **bejelentkezési URL-cím**, Válassza **Hozzon létre**.</span><span class="sxs-lookup"><span data-stu-id="226f6-158">Give your application identity a name, leave **Application type** set too**Web app / API**, provide a unique string formatted as a domain for **Sign-on URL**, and choose **Create**.</span></span>

   ![Adjon meg nevet és a bejelentkezés URL-címet az alkalmazás-azonosító](./media/logic-apps-custom-hosted-api/logic-app-identity-azure-portal.png)

   <span data-ttu-id="226f6-160">a Logic Apps alkalmazást most létrehozott hello Alkalmazásidentitás hello app regisztrációk listájában jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="226f6-160">hello application identity that you created for your logic app now appears in hello app registrations list.</span></span>

   ![A logikai alkalmazásnak identitása](./media/logic-apps-custom-hosted-api/logic-app-identity-created.png)

5. <span data-ttu-id="226f6-162">Hello app regisztrációk listában válassza ki az új alkalmazás azonosítója.</span><span class="sxs-lookup"><span data-stu-id="226f6-162">In hello app registrations list, select your new application identity.</span></span> <span data-ttu-id="226f6-163">Másolja ki és mentse a hello **Alkalmazásazonosító** toouse, hello "ügyfél-azonosító" a logikai alkalmazásnak a 3. rész.</span><span class="sxs-lookup"><span data-stu-id="226f6-163">Copy and save hello **Application ID** toouse as hello "client ID" for your logic app in Part 3.</span></span>

   ![Másolja ki és mentse a logikai alkalmazás azonosítója](./media/logic-apps-custom-hosted-api/logic-app-application-id.png)

6. <span data-ttu-id="226f6-165">Ha az alkalmazás identitását beállításait nem látható, válassza a **beállítások** vagy **összes beállítás**.</span><span class="sxs-lookup"><span data-stu-id="226f6-165">If your application identity settings aren't visible, choose **Settings** or **All settings**.</span></span>

7. <span data-ttu-id="226f6-166">A **API-hozzáférés**, válassza a **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="226f6-166">Under **API Access**, choose **Keys**.</span></span> <span data-ttu-id="226f6-167">A **leírás**, adja meg a kulcs nevét.</span><span class="sxs-lookup"><span data-stu-id="226f6-167">Under **Description**, provide a name for your key.</span></span> <span data-ttu-id="226f6-168">A **Expires**, válasszon egy időtartamot a kulcshoz.</span><span class="sxs-lookup"><span data-stu-id="226f6-168">Under **Expires**, select a duration for your key.</span></span>

   <span data-ttu-id="226f6-169">hello kulcs, amely a létrehozni kívánt hello alkalmazás identitása "titkos" vagy a logikai alkalmazásnak jelszó funkcionál.</span><span class="sxs-lookup"><span data-stu-id="226f6-169">hello key that you're creating acts as hello application identity's "secret" or password for your logic app.</span></span>

   ![Logic app identitás-kulcs létrehozása](./media/logic-apps-custom-hosted-api/create-logic-app-identity-key-secret-password.png)

8. <span data-ttu-id="226f6-171">A hello eszköztárában kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="226f6-171">On hello toolbar, choose **Save**.</span></span> <span data-ttu-id="226f6-172">A **érték**, a kulcs csomópontként jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="226f6-172">Under **Value**, your key now appears.</span></span> 
<span data-ttu-id="226f6-173">**Győződjön meg arról, hogy toocopy, és mentse a kulcsot** későbbi használatra mert rejtett hello kulcs ha hagyja hello fő paneljén.</span><span class="sxs-lookup"><span data-stu-id="226f6-173">**Make sure toocopy and save your key** for later use because hello key is hidden when you leave hello key blade.</span></span>

   <span data-ttu-id="226f6-174">A Logic Apps alkalmazást a 3. rész konfigurálásakor adja meg, ez a kulcs mint hello "titkos" vagy a jelszó.</span><span class="sxs-lookup"><span data-stu-id="226f6-174">When you configure your logic app in Part 3, you specify this key as hello "secret" or password.</span></span>

   ![Másolja ki és mentse a kulcsot későbbi használatra](./media/logic-apps-custom-hosted-api/logic-app-copy-key-secret-password.png)

<a name="app-identity-logic-classic"></a>

<span data-ttu-id="226f6-176">**Hozzon létre a logikai alkalmazásnak hello Alkalmazásidentitás hello a klasszikus Azure portálon**</span><span class="sxs-lookup"><span data-stu-id="226f6-176">**Create hello application identity for your logic app in hello Azure classic portal**</span></span>

1. <span data-ttu-id="226f6-177">A klasszikus Azure portálon hello, válassza a [ **Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span><span class="sxs-lookup"><span data-stu-id="226f6-177">In hello Azure classic portal, choose [**Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span></span>

2. <span data-ttu-id="226f6-178">Válassza ki hello ugyanabban a könyvtárban, amely a webes alkalmazás vagy API-alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="226f6-178">Select hello same directory that you use for your web app or API app.</span></span>

3. <span data-ttu-id="226f6-179">A hello **alkalmazások** lapra, majd **Hozzáadás** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="226f6-179">On hello **Applications** tab, choose **Add** at hello bottom of hello page.</span></span>

4. <span data-ttu-id="226f6-180">Nevezze el az alkalmazás azonosítóját, és válassza a **következő** (jobbra).</span><span class="sxs-lookup"><span data-stu-id="226f6-180">Give your application identity a name, and choose **Next** (right arrow).</span></span>

5. <span data-ttu-id="226f6-181">A **alkalmazás tulajdonságainak**, adjon meg egy egyedi karakterláncot formázni egy tartományt a **bejelentkezési URL-cím** és **App ID URI**, és válassza ki **Complete** (jelölő).</span><span class="sxs-lookup"><span data-stu-id="226f6-181">Under **App properties**, provide a unique string formatted as a domain for **Sign-on URL** and **App ID URI**, and choose **Complete** (checkmark).</span></span>

6. <span data-ttu-id="226f6-182">A hello **konfigurálása** fülre, másolja ki és mentse a hello **ügyfél-azonosító** a logic app toouse a 3. rész esetében.</span><span class="sxs-lookup"><span data-stu-id="226f6-182">On hello **Configure** tab, copy and save hello **Client ID** for your logic app toouse in Part 3.</span></span>

7. <span data-ttu-id="226f6-183">A **kulcsok**, nyissa meg hello **válassza ki a duration** listája.</span><span class="sxs-lookup"><span data-stu-id="226f6-183">Under **Keys**, open hello **Select duration** list.</span></span> <span data-ttu-id="226f6-184">Válasszon egy időtartamot a kulcshoz.</span><span class="sxs-lookup"><span data-stu-id="226f6-184">Select a duration for your key.</span></span>

   <span data-ttu-id="226f6-185">hello kulcs, amely a létrehozni kívánt hello alkalmazás identitása "titkos" vagy a logikai alkalmazásnak jelszó funkcionál.</span><span class="sxs-lookup"><span data-stu-id="226f6-185">hello key that you're creating acts as hello application identity's "secret" or password for your logic app.</span></span>

8. <span data-ttu-id="226f6-186">Hello lap hello alján válassza **mentése**.</span><span class="sxs-lookup"><span data-stu-id="226f6-186">At hello bottom of hello page, choose **Save**.</span></span> <span data-ttu-id="226f6-187">Lehetséges, hogy toowait néhány másodpercig.</span><span class="sxs-lookup"><span data-stu-id="226f6-187">You might have toowait a few seconds.</span></span>

9. <span data-ttu-id="226f6-188">A **kulcsok**, győződjön meg arról, hogy toocopy, és mentse hello kulcsot, amely csomópontként jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="226f6-188">Under **Keys**, make sure toocopy and save hello key that now appears.</span></span> 

   <span data-ttu-id="226f6-189">A Logic Apps alkalmazást a 3. rész konfigurálásakor adja meg, ez a kulcs mint hello "titkos" vagy a jelszó.</span><span class="sxs-lookup"><span data-stu-id="226f6-189">When you configure your logic app in Part 3, you specify this key as hello "secret" or password.</span></span>

<span data-ttu-id="226f6-190">További információ megtudhatja, hogyan túl [konfigurálása az App Service alkalmazás toouse Azure Active Directory bejelentkezési](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="226f6-190">For more information, learn how too [configure your App Service application toouse Azure Active Directory login](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).</span></span>

<a name="powershell"></a>

<span data-ttu-id="226f6-191">**A logikai alkalmazásnak hello Alkalmazásidentitás létrehozása a PowerShell**</span><span class="sxs-lookup"><span data-stu-id="226f6-191">**Create hello application identity for your logic app in PowerShell**</span></span>

<span data-ttu-id="226f6-192">Ez a feladat keresztül Azure Resource Manager PowerShell használatával végezheti el.</span><span class="sxs-lookup"><span data-stu-id="226f6-192">You can perform this task through Azure Resource Manager with PowerShell.</span></span> <span data-ttu-id="226f6-193">A PowerShellben futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="226f6-193">In PowerShell, run these commands:</span></span>

1. `Switch-AzureMode AzureResourceManager`

2. `Add-AzureAccount`

3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://mydomain.tld" -IdentifierUris "http://mydomain.tld" -Password "identity-password"`

4. <span data-ttu-id="226f6-194">Győződjön meg arról, hogy toocopy hello **Bérlőazonosító** (az Azure AD-bérlő GUID), hello **Alkalmazásazonosító**, és hello használt jelszót.</span><span class="sxs-lookup"><span data-stu-id="226f6-194">Make sure toocopy hello **Tenant ID** (GUID for your Azure AD tenant), hello **Application ID**, and hello password that you used.</span></span>

<span data-ttu-id="226f6-195">További információ megtudhatja, hogyan túl [hozzon létre egy egyszerű PowerShell tooaccess erőforrások](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="226f6-195">For more information, learn how too [create a service principal with PowerShell tooaccess resources](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span></span>

#### <a name="part-2-create-an-azure-ad-application-identity-for-your-web-app-or-api-app"></a><span data-ttu-id="226f6-196">2. lépés: Hozzon létre egy Azure AD alkalmazás-azonosítót a webes alkalmazás vagy API-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="226f6-196">Part 2: Create an Azure AD application identity for your web app or API app</span></span>

<span data-ttu-id="226f6-197">Ha a webes alkalmazás vagy API-alkalmazás már üzembe van helyezve, hitelesítés bekapcsolása, és hozzon létre hello Alkalmazásidentitás hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="226f6-197">If your web app or API app is already deployed, you can turn on authentication and create hello application identity in hello Azure portal.</span></span> <span data-ttu-id="226f6-198">Ellenkező esetben is [Azure Resource Manager-sablon telepítésekor hitelesítés bekapcsolásához](#authen-deploy).</span><span class="sxs-lookup"><span data-stu-id="226f6-198">Otherwise, you can [turn on authentication when you deploy with an Azure Resource Manager template](#authen-deploy).</span></span> 

<span data-ttu-id="226f6-199">**Hello Alkalmazásidentitás létrehozása, és kapcsolja be a hitelesítés a hello Azure-portál a telepített alkalmazások**</span><span class="sxs-lookup"><span data-stu-id="226f6-199">**Create hello application identity and turn on authentication in hello Azure portal for deployed apps**</span></span>

1. <span data-ttu-id="226f6-200">A hello [Azure-portálon](https://portal.azure.com "https://portal.azure.com"), található, és válassza ki a webalkalmazás vagy API-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="226f6-200">In hello [Azure portal](https://portal.azure.com "https://portal.azure.com"), find and select your web app or API app.</span></span> 

2. <span data-ttu-id="226f6-201">A **beállítások**, válassza a **hitelesítési/engedélyezési**.</span><span class="sxs-lookup"><span data-stu-id="226f6-201">Under **Settings**, choose **Authentication/Authorization**.</span></span> <span data-ttu-id="226f6-202">A **App Service hitelesítés**, kapcsolja be a hitelesítési **a**.</span><span class="sxs-lookup"><span data-stu-id="226f6-202">Under **App Service Authentication**, turn authentication **On**.</span></span> <span data-ttu-id="226f6-203">A **hitelesítésszolgáltatókat**, válassza a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="226f6-203">Under **Authentication Providers**, choose **Azure Active Directory**.</span></span>

   ![Hitelesítés bekapcsolása](./media/logic-apps-custom-hosted-api/custom-web-api-app-authentication.png)

3. <span data-ttu-id="226f6-205">Most létrehoz egy alkalmazás azonosítóját, a webes alkalmazás vagy API-alkalmazás, ahogy az itt látható.</span><span class="sxs-lookup"><span data-stu-id="226f6-205">Now create an application identity for your web app or API app as shown here.</span></span> <span data-ttu-id="226f6-206">A hello **Azure Active Directory beállításai** panelen állítsa **üzemmód** túl**Express**.</span><span class="sxs-lookup"><span data-stu-id="226f6-206">On hello **Azure Active Directory Settings** blade, set **Management mode** too**Express**.</span></span> <span data-ttu-id="226f6-207">Válasszon **új AD-alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="226f6-207">Choose **Create New AD App**.</span></span> <span data-ttu-id="226f6-208">Nevezze el az alkalmazás azonosítóját, és válassza a **OK**.</span><span class="sxs-lookup"><span data-stu-id="226f6-208">Give your application identity a name, and choose **OK**.</span></span> 

   ![Az Alkalmazásidentitás a webes alkalmazás vagy API-alkalmazás létrehozása](./media/logic-apps-custom-hosted-api/custom-api-application-identity.png)

4. <span data-ttu-id="226f6-210">A hello **hitelesítési / engedélyezési panel**, válassza a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="226f6-210">On hello **Authentication / Authorization blade**, choose **Save**.</span></span>

<span data-ttu-id="226f6-211">Most kell hello ügyfél-azonosító található, és bérlői azonosító hello alkalmazás identitás, amely a webes alkalmazás vagy API-alkalmazás van társítva.</span><span class="sxs-lookup"><span data-stu-id="226f6-211">Now you must find hello client ID and tenant ID for hello application identity that's associated with your web app or API app.</span></span> <span data-ttu-id="226f6-212">Ezek az azonosítók használhatja a 3. rész.</span><span class="sxs-lookup"><span data-stu-id="226f6-212">You use these IDs in Part 3.</span></span> <span data-ttu-id="226f6-213">Így folytatja ezeket a lépéseket az Azure-portálon hello vagy [a klasszikus Azure portálon](#find-id-classic).</span><span class="sxs-lookup"><span data-stu-id="226f6-213">So continue with these steps for hello Azure portal or [Azure classic portal](#find-id-classic).</span></span>

<span data-ttu-id="226f6-214">**Alkalmazás azonosítója ügyfél-azonosító található, és a bérlői azonosító a webes alkalmazás vagy API-alkalmazásba hello Azure-portálon**</span><span class="sxs-lookup"><span data-stu-id="226f6-214">**Find application identity's client ID and tenant ID for your web app or API app in hello Azure portal**</span></span>

1. <span data-ttu-id="226f6-215">A **hitelesítésszolgáltatókat**, válassza a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="226f6-215">Under **Authentication Providers**, choose **Azure Active Directory**.</span></span> 

   ![Válassza ki a "Az Azure Active Directory"](./media/logic-apps-custom-hosted-api/custom-api-app-identity-client-id-tenant-id.png)

2. <span data-ttu-id="226f6-217">A hello **Azure Active Directory beállításai** panelen állítsa **üzemmód** túl**speciális**.</span><span class="sxs-lookup"><span data-stu-id="226f6-217">On hello **Azure Active Directory Settings** blade, set **Management mode** too**Advanced**.</span></span>

3. <span data-ttu-id="226f6-218">Másolás hello **ügyfél-azonosító**, és mentse a GUID azonosítóját használja a 3. rész.</span><span class="sxs-lookup"><span data-stu-id="226f6-218">Copy hello **Client ID**, and save that GUID for use in Part 3.</span></span>

   > [!TIP] 
   > <span data-ttu-id="226f6-219">Ha **ügyfél-azonosító** és **kiállítójának URL-címe** nem jelenik meg, próbálja meg frissíteni az hello Azure-portálon, és ismételje meg az 1.</span><span class="sxs-lookup"><span data-stu-id="226f6-219">If **Client ID** and **Issuer Url** don't appear, try refreshing hello Azure portal, and repeat Step 1.</span></span>

4. <span data-ttu-id="226f6-220">A **kiállítójának URL-címe**, másolja és mentse csak hello GUID 3. rész.</span><span class="sxs-lookup"><span data-stu-id="226f6-220">Under **Issuer Url**, copy and save just hello GUID for Part 3.</span></span> <span data-ttu-id="226f6-221">Is használhatja a GUID a webalkalmazás vagy API-alkalmazás központi telepítési sablont, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="226f6-221">You can also use this GUID in your web app or API app's deployment template, if necessary.</span></span>

   <span data-ttu-id="226f6-222">A GUID az adott bérlő GUID-azonosító ("bérlő azonosítója"), és meg kell jelennie az URL-cím:`https://sts.windows.net/{GUID}`</span><span class="sxs-lookup"><span data-stu-id="226f6-222">This GUID is your specific tenant's GUID ("tenant ID") and should appear in this URL: `https://sts.windows.net/{GUID}`</span></span>

5. <span data-ttu-id="226f6-223">A módosítások mentése nélkül zárja be a hello **Azure Active Directory beállításai** panelen.</span><span class="sxs-lookup"><span data-stu-id="226f6-223">Without saving your changes, close hello **Azure Active Directory Settings** blade.</span></span>

<a name="find-id-classic"></a>

<span data-ttu-id="226f6-224">**Alkalmazás azonosítója ügyfél-azonosító található, és a bérlői azonosító a webalkalmazás vagy a klasszikus Azure portálon hello API-alkalmazás**</span><span class="sxs-lookup"><span data-stu-id="226f6-224">**Find application identity's client ID and tenant ID for your web app or API app in hello Azure classic portal**</span></span>

1. <span data-ttu-id="226f6-225">A klasszikus Azure portálon hello, válassza a [ **Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span><span class="sxs-lookup"><span data-stu-id="226f6-225">In hello Azure classic portal, choose [**Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).</span></span>

2.  <span data-ttu-id="226f6-226">Válassza ki a webalkalmazás vagy API-alkalmazást használó hello könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="226f6-226">Select hello directory that you use for your web app or API app.</span></span>

3. <span data-ttu-id="226f6-227">A hello **keresési** mezőben található, és válassza ki a webalkalmazás vagy API-alkalmazás hello alkalmazásidentitás.</span><span class="sxs-lookup"><span data-stu-id="226f6-227">In hello **Search** box, find and select hello application identity for your web app or API app.</span></span>

4. <span data-ttu-id="226f6-228">A hello **konfigurálása** lap, a Másolás hello **ügyfél-azonosító**, és mentse a GUID azonosítóját használja a 3. rész.</span><span class="sxs-lookup"><span data-stu-id="226f6-228">On hello **Configure** tab, copy hello **Client ID**, and save that GUID for use in Part 3.</span></span>

5. <span data-ttu-id="226f6-229">Miután beszerezte a hello ügyfél-azonosító hello hello alján **konfigurálása** lapra, majd **végpontok megtekintése**.</span><span class="sxs-lookup"><span data-stu-id="226f6-229">After you get hello client ID, at hello bottom of hello **Configure** tab, choose **View endpoints**.</span></span>

6. <span data-ttu-id="226f6-230">Hello URL-Címének másolása **összevonási metaadat-dokumentum**, és keresse meg a toothat URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="226f6-230">Copy hello URL for **Federation Metadata Document**, and browse toothat URL.</span></span>

7. <span data-ttu-id="226f6-231">Hello metaadat-dokumentum, amely megnyitja, keresse meg hello legfelső szintű **EntityDescriptor azonosító** elemet, amely rendelkezik egy **entityid beállítást** attribútum ezen a képernyőn:`https://sts.windows.net/{GUID}`</span><span class="sxs-lookup"><span data-stu-id="226f6-231">In hello metadata document that opens, find hello root **EntityDescriptor ID** element, which has an **entityID** attribute in this form: `https://sts.windows.net/{GUID}`</span></span> 

      <span data-ttu-id="226f6-232">Ez az attribútum a GUID hello az adott bérlő GUID (bérlő azonosító).</span><span class="sxs-lookup"><span data-stu-id="226f6-232">hello GUID in this attribute is your specific tenant's GUID (tenant ID).</span></span>

8. <span data-ttu-id="226f6-233">Hello Bérlőazonosító másolja ki és mentse használatra azonosító 3. rész, továbbá a webalkalmazás vagy API-alkalmazás központi telepítési sablont, toouse, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="226f6-233">Copy hello tenant ID and save that ID for use in Part 3, and also toouse in your web app or API app's deployment template, if necessary.</span></span>

<span data-ttu-id="226f6-234">További információ az alábbi témakörökben talál:</span><span class="sxs-lookup"><span data-stu-id="226f6-234">For more information, see these topics:</span></span>

* [<span data-ttu-id="226f6-235">Az Azure App Service API-alkalmazások felhasználói hitelesítésének</span><span class="sxs-lookup"><span data-stu-id="226f6-235">User authentication for API apps in Azure App Service</span></span>](../app-service-api/app-service-api-dotnet-user-principal-auth.md)
* [<span data-ttu-id="226f6-236">Hitelesítési és engedélyezési az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="226f6-236">Authentication and authorization in Azure App Service</span></span>](../app-service/app-service-authentication-overview.md)

<a name="authen-deploy"></a>

<span data-ttu-id="226f6-237">**Amikor telepít egy Azure Resource Manager sablonnal hitelesítés bekapcsolása**</span><span class="sxs-lookup"><span data-stu-id="226f6-237">**Turn on authentication when you deploy with an Azure Resource Manager template**</span></span>

<span data-ttu-id="226f6-238">A logikai alkalmazásnak hello app identitásból továbbra is készítsen a webes alkalmazás vagy API-alkalmazást, amely eltér az Azure AD alkalmazás identitást.</span><span class="sxs-lookup"><span data-stu-id="226f6-238">You must still create an Azure AD application identity for your web app or API app that differs from hello app identity for your logic app.</span></span> <span data-ttu-id="226f6-239">toocreate hello Alkalmazásidentitás, hajtsa végre hello előző lépések a 2. rész hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="226f6-239">toocreate hello application identity, follow hello previous steps in Part 2 for hello Azure portal.</span></span> <span data-ttu-id="226f6-240">Is is hajtsa végre az 1. rész hello lépéseket, de győződjön meg arról, hogy toouse a webes alkalmazás vagy az API-alkalmazás tényleges `https://{URL}` a **bejelentkezési URL-cím** és **App ID URI**.</span><span class="sxs-lookup"><span data-stu-id="226f6-240">You can also follow hello steps in Part 1, but make sure toouse your web app or API app's actual `https://{URL}` for **Sign-on URL** and **App ID URI**.</span></span> <span data-ttu-id="226f6-241">A fenti lépéseket hogy toosave mindkét hello ügyfél és az alkalmazás központi telepítési sablont a használatra, valamint a 3. rész bérlő-azonosító.</span><span class="sxs-lookup"><span data-stu-id="226f6-241">From these steps, you have toosave both hello client ID and tenant ID for use in your app's deployment template and also for Part 3.</span></span>

> [!NOTE]
> <span data-ttu-id="226f6-242">Hello Azure AD alkalmazás azonosítóját a webes alkalmazás vagy API-alkalmazás létrehozásakor hello Azure-portálon vagy a klasszikus Azure portálon, nem pedig PowerShell kell használnia.</span><span class="sxs-lookup"><span data-stu-id="226f6-242">When you create hello Azure AD application identity for your web app or API app, you must use hello Azure portal or Azure classic portal, rather than PowerShell.</span></span> <span data-ttu-id="226f6-243">hello PowerShell-parancsmag segítségével nem hello szükséges engedélyek beállítása toosign felhasználók webhelyen.</span><span class="sxs-lookup"><span data-stu-id="226f6-243">hello PowerShell commandlet doesn't set up hello required permissions toosign users into a website.</span></span>

<span data-ttu-id="226f6-244">Miután beszerezte a hello ügyfél-azonosító és a bérlő azonosítója, ezek az azonosítók egy subresource a webes alkalmazás vagy API-alkalmazás központi telepítési sablonba tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="226f6-244">After you get hello client ID and tenant ID, include these IDs as a subresource of your web app or API app in your deployment template:</span></span>

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

<span data-ttu-id="226f6-245">tooautomatically telepítése egy üres web app és az Azure Active Directory-hitelesítés, és logikai alkalmazás [hello teljes sablon megtekintése Itt](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json), vagy kattintson a **tooAzure telepítése** itt:</span><span class="sxs-lookup"><span data-stu-id="226f6-245">tooautomatically deploy a blank web app and a logic app together with Azure Active Directory authentication, [view hello complete template here](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json), or click **Deploy tooAzure** here:</span></span>

<span data-ttu-id="226f6-246">[![TooAzure telepítése](media/logic-apps-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="226f6-246">[![Deploy tooAzure](media/logic-apps-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)</span></span>

#### <a name="part-3-populate-hello-authorization-section-in-your-logic-app"></a><span data-ttu-id="226f6-247">3. rész: Hello engedélyezési szakaszában, a logikai alkalmazás feltöltése</span><span class="sxs-lookup"><span data-stu-id="226f6-247">Part 3: Populate hello Authorization section in your logic app</span></span>

<span data-ttu-id="226f6-248">hello előző címsablonnak már ez a hitelesítési szakasz beállítása, de ha közvetlenül hello logic app, meg kell adni a hello teljes engedélyezési szakasz.</span><span class="sxs-lookup"><span data-stu-id="226f6-248">hello previous template already has this authorization section set up, but if you are directly authoring hello logic app, you must include hello full authorization section.</span></span>

<span data-ttu-id="226f6-249">Nyissa meg a logic app-definíciót a kód nézetre, nyissa meg toohello **HTTP** művelet szakaszban, a keresés hello **engedélyezési** szakaszt, és ezt a sort tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="226f6-249">Open your logic app definition in code view, go toohello **HTTP** action section, find hello **Authorization** section, and include this line:</span></span>

`{"tenant": "{tenant-ID}", "audience": "{client-ID-from-Part-2-web-app-or-API app}", "clientId": "{client-ID-from-Part-1-logic-app}", "secret": "{key-from-Part-1-logic-app}", "type": "ActiveDirectoryOAuth" }`

| <span data-ttu-id="226f6-250">Elem</span><span class="sxs-lookup"><span data-stu-id="226f6-250">Element</span></span> | <span data-ttu-id="226f6-251">Leírás</span><span class="sxs-lookup"><span data-stu-id="226f6-251">Description</span></span> |
| ------- | ----------- |
| <span data-ttu-id="226f6-252">Bérlői</span><span class="sxs-lookup"><span data-stu-id="226f6-252">tenant</span></span> |<span data-ttu-id="226f6-253">hello GUID hello Azure AD-bérlő számára</span><span class="sxs-lookup"><span data-stu-id="226f6-253">hello GUID for hello Azure AD tenant</span></span> |
| <span data-ttu-id="226f6-254">Célközönség</span><span class="sxs-lookup"><span data-stu-id="226f6-254">audience</span></span> |<span data-ttu-id="226f6-255">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="226f6-255">Required.</span></span> <span data-ttu-id="226f6-256">hello GUID hello cél erőforrás, amelyet az tooaccess - hello hello alkalmazásidentitás a webes alkalmazás vagy API-alkalmazás az ügyfél-azonosító</span><span class="sxs-lookup"><span data-stu-id="226f6-256">hello GUID for hello target resource that you want tooaccess - hello client ID from hello application identity for your web app or API app</span></span> |
| <span data-ttu-id="226f6-257">clientId</span><span class="sxs-lookup"><span data-stu-id="226f6-257">clientId</span></span> |<span data-ttu-id="226f6-258">a hozzáférés - hello alkalmazásidentitás a logikai alkalmazásnak az ügyfél-azonosító hello kérő hello ügyfél GUID hello</span><span class="sxs-lookup"><span data-stu-id="226f6-258">hello GUID for hello client requesting access - hello client ID from hello application identity for your logic app</span></span> |
| <span data-ttu-id="226f6-259">titkos kulcs</span><span class="sxs-lookup"><span data-stu-id="226f6-259">secret</span></span> |<span data-ttu-id="226f6-260">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="226f6-260">Required.</span></span> <span data-ttu-id="226f6-261">hello kulcs vagy jelszó hello Alkalmazásidentitás hello hozzáférési jogkivonatot kérő hello ügyfél</span><span class="sxs-lookup"><span data-stu-id="226f6-261">hello key or password from hello application identity for hello client that's requesting hello access token</span></span> |
| <span data-ttu-id="226f6-262">type</span><span class="sxs-lookup"><span data-stu-id="226f6-262">type</span></span> |<span data-ttu-id="226f6-263">hello hitelesítési típus.</span><span class="sxs-lookup"><span data-stu-id="226f6-263">hello authentication type.</span></span> <span data-ttu-id="226f6-264">ActiveDirectoryOAuth hitelesítéshez hello értéke `ActiveDirectoryOAuth`.</span><span class="sxs-lookup"><span data-stu-id="226f6-264">For ActiveDirectoryOAuth authentication, hello value is `ActiveDirectoryOAuth`.</span></span> |

<span data-ttu-id="226f6-265">Példa:</span><span class="sxs-lookup"><span data-stu-id="226f6-265">For example:</span></span>

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

### <a name="secure-api-calls-through-code"></a><span data-ttu-id="226f6-266">Kód biztonságos API-hívások</span><span class="sxs-lookup"><span data-stu-id="226f6-266">Secure API calls through code</span></span>

<a name="certificate"></a>

#### <a name="certificate-authentication"></a><span data-ttu-id="226f6-267">Tanúsítványhitelesítés</span><span class="sxs-lookup"><span data-stu-id="226f6-267">Certificate authentication</span></span>

<span data-ttu-id="226f6-268">toovalidate hello érkező kéréseket a logic app tooyour webalkalmazás vagy API-alkalmazás, ügyfél-tanúsítványok is használhatja.</span><span class="sxs-lookup"><span data-stu-id="226f6-268">toovalidate hello incoming requests from your logic app tooyour web app or API app, you can use client certificates.</span></span> <span data-ttu-id="226f6-269">Ismerje meg, a kód tooset [hogyan tooconfigure TLS kölcsönös hitelesítés](../app-service-web/app-service-web-configure-tls-mutual-auth.md).</span><span class="sxs-lookup"><span data-stu-id="226f6-269">tooset up your code, learn [how tooconfigure TLS mutual authentication](../app-service-web/app-service-web-configure-tls-mutual-auth.md).</span></span>

<span data-ttu-id="226f6-270">A hello **engedélyezési** szakaszban, ezt a sort tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="226f6-270">In hello **Authorization** section, include this line:</span></span> 

`{"type": "clientcertificate", "password": "password", "pfx": "long-pfx-key"}`

| <span data-ttu-id="226f6-271">Elem</span><span class="sxs-lookup"><span data-stu-id="226f6-271">Element</span></span> | <span data-ttu-id="226f6-272">Leírás</span><span class="sxs-lookup"><span data-stu-id="226f6-272">Description</span></span> |
| ------- | ----------- |
| <span data-ttu-id="226f6-273">type</span><span class="sxs-lookup"><span data-stu-id="226f6-273">type</span></span> |<span data-ttu-id="226f6-274">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="226f6-274">Required.</span></span> <span data-ttu-id="226f6-275">hello hitelesítési típus.</span><span class="sxs-lookup"><span data-stu-id="226f6-275">hello authentication type.</span></span> <span data-ttu-id="226f6-276">Az ügyfél SSL-tanúsítványok, hello értéknek kell lennie `ClientCertificate`.</span><span class="sxs-lookup"><span data-stu-id="226f6-276">For SSL client certificates, hello value must be `ClientCertificate`.</span></span> |
| <span data-ttu-id="226f6-277">jelszó</span><span class="sxs-lookup"><span data-stu-id="226f6-277">password</span></span> |<span data-ttu-id="226f6-278">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="226f6-278">Required.</span></span> <span data-ttu-id="226f6-279">hello jelszót hello ügyféltanúsítvány (PFX-fájl)</span><span class="sxs-lookup"><span data-stu-id="226f6-279">hello password for accessing hello client certificate (PFX file)</span></span> |
| <span data-ttu-id="226f6-280">PFX</span><span class="sxs-lookup"><span data-stu-id="226f6-280">pfx</span></span> |<span data-ttu-id="226f6-281">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="226f6-281">Required.</span></span> <span data-ttu-id="226f6-282">Base64-kódolású tartalmak hello ügyféltanúsítvány (PFX-fájl)</span><span class="sxs-lookup"><span data-stu-id="226f6-282">Base64-encoded contents of hello client certificate (PFX file)</span></span> |

<a name="basic"></a>

#### <a name="basic-authentication"></a><span data-ttu-id="226f6-283">Az egyszerű hitelesítés</span><span class="sxs-lookup"><span data-stu-id="226f6-283">Basic authentication</span></span>

<span data-ttu-id="226f6-284">a logic app tooyour webalkalmazás vagy API-alkalmazás toovalidate bejövő kérelmet, alapszintű hitelesítés, mint például a felhasználónév és jelszó is használhatja.</span><span class="sxs-lookup"><span data-stu-id="226f6-284">toovalidate incoming requests from your logic app tooyour web app or API app, you can use basic authentication, such as a username and password.</span></span> <span data-ttu-id="226f6-285">Az egyszerű hitelesítés egy megszokott mintát követi, és használhatja a bármely használt nyelv toobuild a hitelesítést, a webes alkalmazás vagy API-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="226f6-285">Basic authentication is a common pattern, and you can use this authentication in any language used toobuild your web app or API app.</span></span>

<span data-ttu-id="226f6-286">A hello **engedélyezési** szakaszban, ezt a sort tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="226f6-286">In hello **Authorization** section, include this line:</span></span>

<span data-ttu-id="226f6-287">`{"type": "basic", "username": "username", "password": "password"}`.</span><span class="sxs-lookup"><span data-stu-id="226f6-287">`{"type": "basic", "username": "username", "password": "password"}`.</span></span>

| <span data-ttu-id="226f6-288">Elem</span><span class="sxs-lookup"><span data-stu-id="226f6-288">Element</span></span> | <span data-ttu-id="226f6-289">Leírás</span><span class="sxs-lookup"><span data-stu-id="226f6-289">Description</span></span> |
| --- | --- |
| <span data-ttu-id="226f6-290">type</span><span class="sxs-lookup"><span data-stu-id="226f6-290">type</span></span> |<span data-ttu-id="226f6-291">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="226f6-291">Required.</span></span> <span data-ttu-id="226f6-292">hello hitelesítési típus.</span><span class="sxs-lookup"><span data-stu-id="226f6-292">hello authentication type.</span></span> <span data-ttu-id="226f6-293">Az egyszerű hitelesítés hello értéknek kell lennie `Basic`.</span><span class="sxs-lookup"><span data-stu-id="226f6-293">For basic authentication, hello value must be `Basic`.</span></span> |
| <span data-ttu-id="226f6-294">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="226f6-294">username</span></span> |<span data-ttu-id="226f6-295">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="226f6-295">Required.</span></span> <span data-ttu-id="226f6-296">hitelesítés hello felhasználóneve</span><span class="sxs-lookup"><span data-stu-id="226f6-296">hello username for authentication</span></span> |
| <span data-ttu-id="226f6-297">jelszó</span><span class="sxs-lookup"><span data-stu-id="226f6-297">password</span></span> |<span data-ttu-id="226f6-298">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="226f6-298">Required.</span></span> <span data-ttu-id="226f6-299">hello jelszó a hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="226f6-299">hello password for authentication</span></span> |

<a name="azure-ad-code"></a>

#### <a name="azure-active-directory-authentication-through-code"></a><span data-ttu-id="226f6-300">Az Azure Active Directory hitelesítési kód</span><span class="sxs-lookup"><span data-stu-id="226f6-300">Azure Active Directory authentication through code</span></span>

<span data-ttu-id="226f6-301">Alapértelmezés szerint hello Azure AD-alapú hitelesítés, amely a hello Azure-portálon kapcsolja be a minden részletre kiterjedő engedélyezési nem biztosít.</span><span class="sxs-lookup"><span data-stu-id="226f6-301">By default, hello Azure AD authentication that you turn on in hello Azure portal doesn't provide fine-grained authorization.</span></span> <span data-ttu-id="226f6-302">Ez a hitelesítés például zárolja a API toojust, egy adott bérlő, nem tooa adott felhasználó vagy alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="226f6-302">For example, this authentication locks your API toojust a specific tenant, not tooa specific user or app.</span></span> 

<span data-ttu-id="226f6-303">toorestrict API access tooyour logikai alkalmazás kód, bontsa ki a hello fejlécet, amelynek hello JSON webes jogkivonat (JWT).</span><span class="sxs-lookup"><span data-stu-id="226f6-303">toorestrict API access tooyour logic app through code, extract hello header that has hello JSON web token (JWT).</span></span> <span data-ttu-id="226f6-304">Ellenőrizze a hello hívó identitását, és a kérelmeket, amelyek nem egyeznek.</span><span class="sxs-lookup"><span data-stu-id="226f6-304">Check hello caller's identity, and reject requests that don't match.</span></span>

<span data-ttu-id="226f6-305">Ha folytatja, tooimplement teljes egészében a saját kódot, és nem használható hello Azure-portálon, a hitelesítés megtudhatja, hogyan túl [hitelesítik magukat a helyszíni Active Directory, az Azure alkalmazásban](../app-service-web/web-sites-authentication-authorization.md).</span><span class="sxs-lookup"><span data-stu-id="226f6-305">Going further, tooimplement this authentication entirely in your own code, and not use hello Azure portal, learn how too [authenticate with on-premises Active Directory in your Azure app](../app-service-web/web-sites-authentication-authorization.md).</span></span>

<span data-ttu-id="226f6-306">Az Alkalmazásidentitás a logikai alkalmazásnak toocreate és, hogy identitás toocall az API-t használnak, akkor kövesse az előző lépéseket hello.</span><span class="sxs-lookup"><span data-stu-id="226f6-306">toocreate an application identity for your logic app and use that identity toocall your API, you must follow hello previous steps.</span></span>

## <a name="next-steps"></a><span data-ttu-id="226f6-307">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="226f6-307">Next steps</span></span>

* [<span data-ttu-id="226f6-308">Ellenőrizze, hogy logic app teljesítményében diagnosztikai naplók és riasztások</span><span class="sxs-lookup"><span data-stu-id="226f6-308">Check logic app performance with diagnostic logs and alerts</span></span>](logic-apps-monitor-your-logic-apps.md)