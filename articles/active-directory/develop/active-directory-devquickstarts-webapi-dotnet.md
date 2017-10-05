---
title: "Az Azure AD .NET webes API bevezetés |} Microsoft Docs"
description: "Hogyan hozhat létre a .NET MVC webes API-k, amely az Azure AD a hitelesítéshez és engedélyezéshez."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 67e74774-1748-43ea-8130-55275a18320f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: f44d75f45073a5d9aa9b1863ed227aba4efcf785
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="help-protect-a-web-api-by-using-bearer-tokens-from-azure-ad"></a><span data-ttu-id="94819-103">A webes API védelme az Azure AD tulajdonosi jogkivonatok segítségével</span><span class="sxs-lookup"><span data-stu-id="94819-103">Help protect a web API by using bearer tokens from Azure AD</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="94819-104">Ha a védett erőforrásokhoz való hozzáférést biztosít az alkalmazás most felépítése, meg kell ismernie történő jogosulatlan hozzáférés elkerülése érdekében ezeket az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="94819-104">If you’re building an application that provides access to protected resources, you need to know how to prevent unwarranted access to those resources.</span></span>
<span data-ttu-id="94819-105">Az Azure Active Directory (Azure AD) elérhetővé válnak az egyszerű és magától értetődő védelme egy webes API-t OAuth 2.0 tulajdonosi hozzáférés segítségével csak néhány sornyi kódot a jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="94819-105">Azure Active Directory (Azure AD) makes it simple and straightforward to help protect a web API by using OAuth 2.0 bearer access tokens with only a few lines of code.</span></span>

<span data-ttu-id="94819-106">Az ASP.NET web apps a közösségi szerkesztésű OWIN köztes a .NET-keretrendszer 4.5 része a Microsoft általi implementációja használatával végezheti el a védelmet.</span><span class="sxs-lookup"><span data-stu-id="94819-106">In ASP.NET web apps, you can accomplish this protection by using the Microsoft implementation of the community-driven OWIN middleware included in .NET Framework 4.5.</span></span> <span data-ttu-id="94819-107">Itt az OWIN fogjuk használni, amely "Teendőlista" webes API-k létrehozása:</span><span class="sxs-lookup"><span data-stu-id="94819-107">Here we’ll use OWIN to build a "To Do List" web API that:</span></span>

* <span data-ttu-id="94819-108">Jelöl ki, amelyek API-k védett.</span><span class="sxs-lookup"><span data-stu-id="94819-108">Designates which APIs are protected.</span></span>
* <span data-ttu-id="94819-109">Ellenőrzi, hogy a webes API-hívások tartalmaznak egy érvényes jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="94819-109">Validates that the web API calls contain a valid access token.</span></span>

<span data-ttu-id="94819-110">A való tegye lista API létrehozásához először kell:</span><span class="sxs-lookup"><span data-stu-id="94819-110">To build the To Do List API, you first need to:</span></span>

1. <span data-ttu-id="94819-111">Alkalmazás regisztrálása az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="94819-111">Register an application with Azure AD.</span></span>
2. <span data-ttu-id="94819-112">Az alkalmazás beállítása az OWIN hitelesítési folyamatot használja.</span><span class="sxs-lookup"><span data-stu-id="94819-112">Set up the app to use the OWIN authentication pipeline.</span></span>
3. <span data-ttu-id="94819-113">A webes API hívásához ügyfélalkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="94819-113">Configure a client application to call the web API.</span></span>

<span data-ttu-id="94819-114">A kezdéshez [töltse le az alkalmazás vázat](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) vagy [töltse le az elkészült mintát](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="94819-114">To get started, [download the app skeleton](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) or [download the completed sample](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="94819-115">Egy Visual Studio 2013 megoldást.</span><span class="sxs-lookup"><span data-stu-id="94819-115">Each is a Visual Studio 2013 solution.</span></span> <span data-ttu-id="94819-116">Akkor is kell, ahol regisztrálhatja alkalmazását Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="94819-116">You also need an Azure AD tenant in which to register your application.</span></span> <span data-ttu-id="94819-117">Ha Ön nem rendelkezik ilyennel, [beszerzéséről egy](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="94819-117">If you don't have one already, [learn how to get one](active-directory-howto-tenant.md).</span></span>

## <a name="step-1-register-an-application-with-azure-ad"></a><span data-ttu-id="94819-118">1. lépés: Egy alkalmazás regisztrálása az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="94819-118">Step 1: Register an application with Azure AD</span></span>
<span data-ttu-id="94819-119">Biztonságossá az alkalmazásról, először létrehoz egy alkalmazást az Ön bérlőjében, és adjon meg néhány kulcsfontosságú adatokat az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="94819-119">To help secure your application, you first need to create an application in your tenant and provide Azure AD with a few key pieces of information.</span></span>

1. <span data-ttu-id="94819-120">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="94819-120">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="94819-121">A felső eszköztáron kattintson a fiókját.</span><span class="sxs-lookup"><span data-stu-id="94819-121">On the top bar, click your account.</span></span> <span data-ttu-id="94819-122">Az a **Directory** menüben válassza ki az Azure AD-bérlőt, ahová az alkalmazás regisztrálásához.</span><span class="sxs-lookup"><span data-stu-id="94819-122">In the **Directory** list, choose the Azure AD tenant where you want to register your application.</span></span>

3. <span data-ttu-id="94819-123">Kattintson a **több szolgáltatások** a bal oldali ablaktáblán, és válassza a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="94819-123">Click **More Services** in the left pane, and then select **Azure Active Directory**.</span></span>

4. <span data-ttu-id="94819-124">Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="94819-124">Click **App registrations**, and then select **Add**.</span></span>

5. <span data-ttu-id="94819-125">Kövesse az utasításokat, és hozzon létre egy új **webalkalmazás és/vagy webes API**.</span><span class="sxs-lookup"><span data-stu-id="94819-125">Follow the prompts and create a new **Web Application and/or Web API**.</span></span>
  * <span data-ttu-id="94819-126">**Név** az alkalmazás a felhasználók számára ismerteti.</span><span class="sxs-lookup"><span data-stu-id="94819-126">**Name** describes your application to users.</span></span> <span data-ttu-id="94819-127">Adja meg **teendőlista szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="94819-127">Enter **To Do List Service**.</span></span>
  * <span data-ttu-id="94819-128">**Átirányítási Uri** protokollt és a karakterlánc kombinációját, amely az Azure AD szükséges az alkalmazás által kért jogkivonatokhoz adja vissza.</span><span class="sxs-lookup"><span data-stu-id="94819-128">**Redirect Uri** is a scheme and string combination that Azure AD uses to return any tokens that your app has requested.</span></span> <span data-ttu-id="94819-129">Adja meg `https://localhost:44321/` ehhez az értékhez.</span><span class="sxs-lookup"><span data-stu-id="94819-129">Enter `https://localhost:44321/` for this value.</span></span>

6. <span data-ttu-id="94819-130">Az a **beállítások** -> **tulajdonságok** az alkalmazás lapján frissítse a App ID URI.</span><span class="sxs-lookup"><span data-stu-id="94819-130">From the **Settings** -> **Properties** page for your application, update the App ID URI.</span></span> <span data-ttu-id="94819-131">Adjon meg egy bérlő-specifikus azonosítója.</span><span class="sxs-lookup"><span data-stu-id="94819-131">Enter a tenant-specific identifier.</span></span> <span data-ttu-id="94819-132">Adja meg például a következőt: `https://contoso.onmicrosoft.com/TodoListService`.</span><span class="sxs-lookup"><span data-stu-id="94819-132">For example, enter `https://contoso.onmicrosoft.com/TodoListService`.</span></span>

7. <span data-ttu-id="94819-133">A konfiguráció mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="94819-133">Save the configuration.</span></span> <span data-ttu-id="94819-134">Hagyja megnyitva, a portál, mert biztosítani kell az ügyfélalkalmazás hamarosan regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="94819-134">Leave the portal open, because you'll also need to register your client application shortly.</span></span>

## <a name="step-2-set-up-the-app-to-use-the-owin-authentication-pipeline"></a><span data-ttu-id="94819-135">2. lépés: Az alkalmazás használatának beállítása a OWIN hitelesítési folyamatot</span><span class="sxs-lookup"><span data-stu-id="94819-135">Step 2: Set up the app to use the OWIN authentication pipeline</span></span>
<span data-ttu-id="94819-136">Érvényesíteni a bejövő kérelmeket és jogkivonatok, akkor be kell állítania az alkalmazás kommunikálni az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="94819-136">To validate incoming requests and tokens, you need to set up your application to communicate with Azure AD.</span></span>

1. <span data-ttu-id="94819-137">Első lépésként nyissa meg a megoldást, és az OWIN köztes NuGet-csomagok hozzáadása a TodoListService projekthez a Csomagkezelő konzol használatával.</span><span class="sxs-lookup"><span data-stu-id="94819-137">To begin, open the solution and add the OWIN middleware NuGet packages to the TodoListService project by using the Package Manager Console.</span></span>

    ```
    PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
    PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
    ```

2. <span data-ttu-id="94819-138">Egy OWIN indítási osztály hozzáadása a TodoListService projekt neve `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="94819-138">Add an OWIN Startup class to the TodoListService project called `Startup.cs`.</span></span>  <span data-ttu-id="94819-139">Kattintson jobb gombbal a projektre, válassza ki **Hozzáadás** > **új elem**, majd keresse meg a **OWIN**.</span><span class="sxs-lookup"><span data-stu-id="94819-139">Right-click the project, select **Add** > **New Item**, and then search for **OWIN**.</span></span> <span data-ttu-id="94819-140">Az OWIN közbenső szoftver meghívja a `Configuration(…)` metódust az alkalmazás indulásakor.</span><span class="sxs-lookup"><span data-stu-id="94819-140">The OWIN middleware will invoke the `Configuration(…)` method when your app starts.</span></span>

3. <span data-ttu-id="94819-141">Módosítsa az osztálydeklaráció való `public partial class Startup`.</span><span class="sxs-lookup"><span data-stu-id="94819-141">Change the class declaration to `public partial class Startup`.</span></span> <span data-ttu-id="94819-142">Azt korábban már megvalósított Ez az osztály tartozik, egy másik fájlban.</span><span class="sxs-lookup"><span data-stu-id="94819-142">We’ve already implemented part of this class for you in another file.</span></span> <span data-ttu-id="94819-143">Az a `Configuration(…)` metódus hívása legyen `ConfgureAuth(…)` hitelesítés a webalkalmazás beállítása.</span><span class="sxs-lookup"><span data-stu-id="94819-143">In the `Configuration(…)` method, make a call to `ConfgureAuth(…)` to set up authentication for your web app.</span></span>

    ```C#
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
    ```

4. <span data-ttu-id="94819-144">Nyissa meg a fájlt `App_Start\Startup.Auth.cs` és megvalósítását a `ConfigureAuth(…)` metódust.</span><span class="sxs-lookup"><span data-stu-id="94819-144">Open the file `App_Start\Startup.Auth.cs` and implement the `ConfigureAuth(…)` method.</span></span> <span data-ttu-id="94819-145">A megadott paraméterek `WindowsAzureActiveDirectoryBearerAuthenticationOptions` koordináták az alkalmazás az Azure AD kommunikálni fog szolgálni.</span><span class="sxs-lookup"><span data-stu-id="94819-145">The parameters that you provide in `WindowsAzureActiveDirectoryBearerAuthenticationOptions` will serve as coordinates for your app to communicate with Azure AD.</span></span>

    ```C#
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseWindowsAzureActiveDirectoryBearerAuthentication(
            new WindowsAzureActiveDirectoryBearerAuthenticationOptions
            {
                Audience = ConfigurationManager.AppSettings["ida:Audience"],
                Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
            });
    }
    ```

5. <span data-ttu-id="94819-146">Mostantól a `[Authorize]` segít megvédeni a tartományvezérlőket és a JSON webes jogkivonat (JWT) tulajdonosi hitelesítéssel műveletek attribútumait.</span><span class="sxs-lookup"><span data-stu-id="94819-146">Now you can use `[Authorize]` attributes to help protect your controllers and actions with JSON Web Token (JWT) bearer authentication.</span></span> <span data-ttu-id="94819-147">Adja a `Controllers\TodoListController.cs` osztály engedélyezés címke használatával.</span><span class="sxs-lookup"><span data-stu-id="94819-147">Decorate the `Controllers\TodoListController.cs` class with an authorize tag.</span></span> <span data-ttu-id="94819-148">Ezzel kikényszeríti a felhasználót, hogy jelentkezzen be a lap elérése előtt.</span><span class="sxs-lookup"><span data-stu-id="94819-148">This will force the user to sign in before accessing that page.</span></span>

    ```C#
    [Authorize]
    public class TodoListController : ApiController
    {
    ```

    <span data-ttu-id="94819-149">Amikor egy jogosult hívó sikeresen hív meg, egy a `TodoListController` API-k, a művelet módosítania kell információkhoz juthat a hívóról.</span><span class="sxs-lookup"><span data-stu-id="94819-149">When an authorized caller successfully invokes one of the `TodoListController` APIs, the action might need access to information about the caller.</span></span> <span data-ttu-id="94819-150">A jogcímek belül a tulajdonosi jogkivonattal keresztül hozzáférést biztosít az OWIN a `ClaimsPrincpal` objektum.</span><span class="sxs-lookup"><span data-stu-id="94819-150">OWIN provides access to the claims inside the bearer token via the `ClaimsPrincpal` object.</span></span>  

6. <span data-ttu-id="94819-151">Általános követelmény a webes API-khoz a jogkivonatban jelen lévő „hatókörök” érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="94819-151">A common requirement for web APIs is to validate the "scopes" present in the token.</span></span> <span data-ttu-id="94819-152">Ez biztosítja, hogy a felhasználó hozzájárult a való tegye lista szolgáltatás eléréséhez szükséges engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="94819-152">This ensures that the user has consented to the permissions required to access the To Do List Service.</span></span>

    ```C#
    public IEnumerable<TodoItem> Get()
    {
        // user_impersonation is the default permission exposed by applications in Azure AD
        if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
        {
            throw new HttpResponseException(new HttpResponseMessage {
              StatusCode = HttpStatusCode.Unauthorized,
              ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found"
            });
        }
        ...
    }
    ```

7. <span data-ttu-id="94819-153">Nyissa meg a `web.config` a TodoListService projekt gyökérkönyvtárában található fájlt, és adja meg a konfigurációs értékeit a `<appSettings>` szakasz.</span><span class="sxs-lookup"><span data-stu-id="94819-153">Open the `web.config` file in the root of the TodoListService project, and enter your configuration values in the `<appSettings>` section.</span></span>
  * <span data-ttu-id="94819-154">`ida:Tenant`az Azure AD-bérlő – például a contoso.onmicrosoft.com neve van.</span><span class="sxs-lookup"><span data-stu-id="94819-154">`ida:Tenant` is the name of your Azure AD tenant--for example, contoso.onmicrosoft.com.</span></span>
  * <span data-ttu-id="94819-155">`ida:Audience`az App ID URI az alkalmazás az Azure-portálon megadott van.</span><span class="sxs-lookup"><span data-stu-id="94819-155">`ida:Audience` is the App ID URI of the application that you entered in the Azure portal.</span></span>

## <a name="step-3-configure-a-client-application-and-run-the-service"></a><span data-ttu-id="94819-156">3. lépés: Ügyfélalkalmazás konfigurálása és a szolgáltatás futtatásához</span><span class="sxs-lookup"><span data-stu-id="94819-156">Step 3: Configure a client application and run the service</span></span>
<span data-ttu-id="94819-157">Ahhoz, hogy a To tegye lista Service működés közben, a teendőlista-ügyfél konfigurálása, hogy azok jogkivonatokhoz az Azure AD és a szolgáltatás meghíváshoz kell.</span><span class="sxs-lookup"><span data-stu-id="94819-157">Before you can see the To Do List Service in action, you need to configure the To Do List client so it can get tokens from Azure AD and make calls to the service.</span></span>

1. <span data-ttu-id="94819-158">Lépjen vissza a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="94819-158">Go back to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="94819-159">Új alkalmazás létrehozása az Azure AD-bérlőn, és válassza ki **natív ügyfélalkalmazás** az eredményül kapott kér.</span><span class="sxs-lookup"><span data-stu-id="94819-159">Create a new application in your Azure AD tenant, and select **Native Client Application** in the resulting prompt.</span></span>
  * <span data-ttu-id="94819-160">**Név** az alkalmazás a felhasználók számára ismerteti.</span><span class="sxs-lookup"><span data-stu-id="94819-160">**Name** describes your application to users.</span></span>
  * <span data-ttu-id="94819-161">Adja meg `http://TodoListClient/` a a **átirányítási Uri-** érték.</span><span class="sxs-lookup"><span data-stu-id="94819-161">Enter `http://TodoListClient/` for the **Redirect Uri** value.</span></span>

3. <span data-ttu-id="94819-162">Regisztráció befejezése után az Azure AD egy egyedi alkalmazás Azonosítót rendel az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="94819-162">After you finish registration, Azure AD assigns a unique application ID to your app.</span></span> <span data-ttu-id="94819-163">Ez az érték kell a következő lépésekben, ezért másolja az alkalmazás oldalról.</span><span class="sxs-lookup"><span data-stu-id="94819-163">You’ll need this value in the next steps, so copy it from the application page.</span></span>

4. <span data-ttu-id="94819-164">Az a **beállítások** lapon jelölje be **szükséges engedélyek**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="94819-164">From the **Settings** page, select **Required Permissions**, and then select **Add**.</span></span> <span data-ttu-id="94819-165">Keresse meg és válassza ki a való tegye lista szolgáltatást, vegye fel a **hozzáférés TodoListService** engedélyt a **delegált engedélyek**, és kattintson a **végzett**.</span><span class="sxs-lookup"><span data-stu-id="94819-165">Locate and select the To Do List Service, add the **Access TodoListService** permission under **Delegated Permissions**, and then click **Done**.</span></span>

5. <span data-ttu-id="94819-166">A Visual Studióban nyissa meg a `App.config` a TodoListClient a projektre, és írja be a konfigurációs értékeit a `<appSettings>` szakasz.</span><span class="sxs-lookup"><span data-stu-id="94819-166">In Visual Studio, open `App.config` in the TodoListClient project, and then enter your configuration values in the `<appSettings>` section.</span></span>

  * <span data-ttu-id="94819-167">`ida:Tenant`az Azure AD-bérlő – például a contoso.onmicrosoft.com neve van.</span><span class="sxs-lookup"><span data-stu-id="94819-167">`ida:Tenant` is the name of your Azure AD tenant--for example, contoso.onmicrosoft.com.</span></span>
  * <span data-ttu-id="94819-168">`ida:ClientId`a rendszer az Azure-portálról másolt alkalmazás Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="94819-168">`ida:ClientId` is the app ID that you copied from the Azure portal.</span></span>
  * <span data-ttu-id="94819-169">`todo:TodoListResourceId`az App ID URI számára tegye lista szolgáltatásalkalmazás az Azure-portálon megadott van.</span><span class="sxs-lookup"><span data-stu-id="94819-169">`todo:TodoListResourceId` is the App ID URI of the To Do List Service application that you entered in the Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="94819-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="94819-170">Next steps</span></span>
<span data-ttu-id="94819-171">Végül törlése, elkészítéséhez és futtatása minden olyan projekthez.</span><span class="sxs-lookup"><span data-stu-id="94819-171">Finally, clean, build, and run each project.</span></span> <span data-ttu-id="94819-172">Ha még nem tette meg, most már az új felhasználó létrehozása a bérlő az idő a *. onmicrosoft.com tartományt.</span><span class="sxs-lookup"><span data-stu-id="94819-172">If you haven’t already, now is the time to create a new user in your tenant with a *.onmicrosoft.com domain.</span></span> <span data-ttu-id="94819-173">Jelentkezzen be, hogy a felhasználó a teendőlista ügyfélnek, és bizonyos feladatok hozzáadása a felhasználói feladatlistában.</span><span class="sxs-lookup"><span data-stu-id="94819-173">Sign in to the To Do List client with that user, and add some tasks to the user's to-do list.</span></span>

<span data-ttu-id="94819-174">Referenciaként az elkészült mintát (a konfigurációs értékek nélkül) érhető el a [GitHub](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="94819-174">For reference, the completed sample (without your configuration values) is available in [GitHub](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="94819-175">Most már továbbléphet további identitás forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="94819-175">You can now move on to more identity scenarios.</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
