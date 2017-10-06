---
title: "aaaAzure AD .NET webes API bevezetés |} Microsoft Docs"
description: "Hogyan toobuild egy .NET MVC webes API-t, amely integrálható az Azure AD a hitelesítéshez és engedélyezéshez."
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
ms.openlocfilehash: 91c93e1fe18855f5648076e59e2ccf081eec34bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="help-protect-a-web-api-by-using-bearer-tokens-from-azure-ad"></a><span data-ttu-id="b7f91-103">A webes API védelme az Azure AD tulajdonosi jogkivonatok segítségével</span><span class="sxs-lookup"><span data-stu-id="b7f91-103">Help protect a web API by using bearer tokens from Azure AD</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="b7f91-104">Ha egy alkalmazás, amely hozzáférést biztosít a tooprotected erőforrások még felépítése, szükség van-e tooknow hogyan férhetnek hozzá a jogosulatlan tooprevent toothose erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="b7f91-104">If you’re building an application that provides access tooprotected resources, you need tooknow how tooprevent unwarranted access toothose resources.</span></span>
<span data-ttu-id="b7f91-105">Azure Active Directory (Azure AD) egyszerűen és egyszerű toohelp webes API-k védelme csak néhány sornyi kód OAuth 2.0 tulajdonosi hozzáférési jogkivonatok használatával.</span><span class="sxs-lookup"><span data-stu-id="b7f91-105">Azure Active Directory (Azure AD) makes it simple and straightforward toohelp protect a web API by using OAuth 2.0 bearer access tokens with only a few lines of code.</span></span>

<span data-ttu-id="b7f91-106">Az ASP.NET web apps Microsoft általi implementációja hello hello közösségi szerkesztésű OWIN köztes szerepel a .NET-keretrendszer 4.5 használatával végezheti el a védelmet.</span><span class="sxs-lookup"><span data-stu-id="b7f91-106">In ASP.NET web apps, you can accomplish this protection by using hello Microsoft implementation of hello community-driven OWIN middleware included in .NET Framework 4.5.</span></span> <span data-ttu-id="b7f91-107">Itt a "lista tooDo" webes API OWIN toobuild fogjuk használni, amelyek:</span><span class="sxs-lookup"><span data-stu-id="b7f91-107">Here we’ll use OWIN toobuild a "tooDo List" web API that:</span></span>

* <span data-ttu-id="b7f91-108">Jelöl ki, amelyek API-k védett.</span><span class="sxs-lookup"><span data-stu-id="b7f91-108">Designates which APIs are protected.</span></span>
* <span data-ttu-id="b7f91-109">Ellenőrzi, hogy hello webes API-hívások tartalmaznak egy érvényes jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="b7f91-109">Validates that hello web API calls contain a valid access token.</span></span>

<span data-ttu-id="b7f91-110">toobuild hello tooDo lista API, először kell:</span><span class="sxs-lookup"><span data-stu-id="b7f91-110">toobuild hello tooDo List API, you first need to:</span></span>

1. <span data-ttu-id="b7f91-111">Alkalmazás regisztrálása az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="b7f91-111">Register an application with Azure AD.</span></span>
2. <span data-ttu-id="b7f91-112">Állítson be hello app toouse hello OWIN hitelesítési folyamatot.</span><span class="sxs-lookup"><span data-stu-id="b7f91-112">Set up hello app toouse hello OWIN authentication pipeline.</span></span>
3. <span data-ttu-id="b7f91-113">Ügyfél alkalmazás toocall hello webes API-k konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="b7f91-113">Configure a client application toocall hello web API.</span></span>

<span data-ttu-id="b7f91-114">elindult, tooget [hello app vázat letöltése](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) vagy [letöltése befejeződött hello minta](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="b7f91-114">tooget started, [download hello app skeleton](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) or [download hello completed sample](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="b7f91-115">Egy Visual Studio 2013 megoldást.</span><span class="sxs-lookup"><span data-stu-id="b7f91-115">Each is a Visual Studio 2013 solution.</span></span> <span data-ttu-id="b7f91-116">Szükség mely tooregister az Azure AD-bérlő az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b7f91-116">You also need an Azure AD tenant in which tooregister your application.</span></span> <span data-ttu-id="b7f91-117">Ha Ön nem rendelkezik ilyennel, [megtudhatja, hogyan egy tooget](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="b7f91-117">If you don't have one already, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

## <a name="step-1-register-an-application-with-azure-ad"></a><span data-ttu-id="b7f91-118">1. lépés: Egy alkalmazás regisztrálása az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="b7f91-118">Step 1: Register an application with Azure AD</span></span>
<span data-ttu-id="b7f91-119">toohelp az alkalmazás biztonságos, akkor először kell toocreate egy alkalmazás az Ön bérlőjében, és adjon meg néhány kulcsfontosságú adatokat az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b7f91-119">toohelp secure your application, you first need toocreate an application in your tenant and provide Azure AD with a few key pieces of information.</span></span>

1. <span data-ttu-id="b7f91-120">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b7f91-120">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="b7f91-121">Hello felső sávon kattintson a fiókját.</span><span class="sxs-lookup"><span data-stu-id="b7f91-121">On hello top bar, click your account.</span></span> <span data-ttu-id="b7f91-122">A hello **Directory** menüben válassza ki a kívánt tooregister hello Azure AD-bérlő az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b7f91-122">In hello **Directory** list, choose hello Azure AD tenant where you want tooregister your application.</span></span>

3. <span data-ttu-id="b7f91-123">Kattintson a **több szolgáltatások** hello bal oldali ablaktáblán, és válassza ki **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="b7f91-123">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>

4. <span data-ttu-id="b7f91-124">Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="b7f91-124">Click **App registrations**, and then select **Add**.</span></span>

5. <span data-ttu-id="b7f91-125">Hello utasításokat követve, és hozzon létre egy új **webalkalmazás és/vagy webes API**.</span><span class="sxs-lookup"><span data-stu-id="b7f91-125">Follow hello prompts and create a new **Web Application and/or Web API**.</span></span>
  * <span data-ttu-id="b7f91-126">**Név** az alkalmazás toousers ismerteti.</span><span class="sxs-lookup"><span data-stu-id="b7f91-126">**Name** describes your application toousers.</span></span> <span data-ttu-id="b7f91-127">Adja meg **tooDo lista szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="b7f91-127">Enter **tooDo List Service**.</span></span>
  * <span data-ttu-id="b7f91-128">**Átirányítási Uri** által az Azure AD használt a jogkivonatokat, hogy az alkalmazásban kérte tooreturn protokollt és a karakterlánc kombinációját.</span><span class="sxs-lookup"><span data-stu-id="b7f91-128">**Redirect Uri** is a scheme and string combination that Azure AD uses tooreturn any tokens that your app has requested.</span></span> <span data-ttu-id="b7f91-129">Adja meg `https://localhost:44321/` ehhez az értékhez.</span><span class="sxs-lookup"><span data-stu-id="b7f91-129">Enter `https://localhost:44321/` for this value.</span></span>

6. <span data-ttu-id="b7f91-130">A hello **beállítások** -> **tulajdonságok** az alkalmazás lapján hello App ID URI frissítése.</span><span class="sxs-lookup"><span data-stu-id="b7f91-130">From hello **Settings** -> **Properties** page for your application, update hello App ID URI.</span></span> <span data-ttu-id="b7f91-131">Adjon meg egy bérlő-specifikus azonosítója.</span><span class="sxs-lookup"><span data-stu-id="b7f91-131">Enter a tenant-specific identifier.</span></span> <span data-ttu-id="b7f91-132">Adja meg például a következőt: `https://contoso.onmicrosoft.com/TodoListService`.</span><span class="sxs-lookup"><span data-stu-id="b7f91-132">For example, enter `https://contoso.onmicrosoft.com/TodoListService`.</span></span>

7. <span data-ttu-id="b7f91-133">Hello konfigurációjának mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="b7f91-133">Save hello configuration.</span></span> <span data-ttu-id="b7f91-134">Hagyja nyitva, hello portal, mert azt is el kell tooregister az ügyfélalkalmazást hamarosan.</span><span class="sxs-lookup"><span data-stu-id="b7f91-134">Leave hello portal open, because you'll also need tooregister your client application shortly.</span></span>

## <a name="step-2-set-up-hello-app-toouse-hello-owin-authentication-pipeline"></a><span data-ttu-id="b7f91-135">2. lépés: Hello app toouse hello OWIN hitelesítési folyamatot beállítása</span><span class="sxs-lookup"><span data-stu-id="b7f91-135">Step 2: Set up hello app toouse hello OWIN authentication pipeline</span></span>
<span data-ttu-id="b7f91-136">toovalidate bejövő kérelmek és jogkivonatok, tooset kell az alkalmazás toocommunicate az Azure AD-be.</span><span class="sxs-lookup"><span data-stu-id="b7f91-136">toovalidate incoming requests and tokens, you need tooset up your application toocommunicate with Azure AD.</span></span>

1. <span data-ttu-id="b7f91-137">toobegin, nyissa meg hello megoldást, és adja hozzá a hello OWIN köztes NuGet csomagjainak toohello TodoListService projekt hello Csomagkezelő konzol használatával.</span><span class="sxs-lookup"><span data-stu-id="b7f91-137">toobegin, open hello solution and add hello OWIN middleware NuGet packages toohello TodoListService project by using hello Package Manager Console.</span></span>

    ```
    PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
    PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
    ```

2. <span data-ttu-id="b7f91-138">Adja hozzá egy OWIN indítási osztály toohello TodoListService projekt nevű `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="b7f91-138">Add an OWIN Startup class toohello TodoListService project called `Startup.cs`.</span></span>  <span data-ttu-id="b7f91-139">Kattintson a jobb gombbal hello projektre, válassza **Hozzáadás** > **új elem**, majd keresse meg a **OWIN**.</span><span class="sxs-lookup"><span data-stu-id="b7f91-139">Right-click hello project, select **Add** > **New Item**, and then search for **OWIN**.</span></span> <span data-ttu-id="b7f91-140">hello OWIN köztes által aktivált hello `Configuration(…)` módszer az alkalmazás indításakor.</span><span class="sxs-lookup"><span data-stu-id="b7f91-140">hello OWIN middleware will invoke hello `Configuration(…)` method when your app starts.</span></span>

3. <span data-ttu-id="b7f91-141">Hello osztálydeklaráció túl módosítása`public partial class Startup`.</span><span class="sxs-lookup"><span data-stu-id="b7f91-141">Change hello class declaration too`public partial class Startup`.</span></span> <span data-ttu-id="b7f91-142">Azt korábban már megvalósított Ez az osztály tartozik, egy másik fájlban.</span><span class="sxs-lookup"><span data-stu-id="b7f91-142">We’ve already implemented part of this class for you in another file.</span></span> <span data-ttu-id="b7f91-143">A hello `Configuration(…)` módszer, túl hívás`ConfgureAuth(…)` tooset hitelesítés a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b7f91-143">In hello `Configuration(…)` method, make a call too`ConfgureAuth(…)` tooset up authentication for your web app.</span></span>

    ```C#
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
    ```

4. <span data-ttu-id="b7f91-144">Nyissa meg hello fájl `App_Start\Startup.Auth.cs` és valósíthatnak meg hello `ConfigureAuth(…)` metódust.</span><span class="sxs-lookup"><span data-stu-id="b7f91-144">Open hello file `App_Start\Startup.Auth.cs` and implement hello `ConfigureAuth(…)` method.</span></span> <span data-ttu-id="b7f91-145">hello által a paraméterek `WindowsAzureActiveDirectoryBearerAuthenticationOptions` erre a célra az Azure ad alkalmazás toocommunicate koordinátáit.</span><span class="sxs-lookup"><span data-stu-id="b7f91-145">hello parameters that you provide in `WindowsAzureActiveDirectoryBearerAuthenticationOptions` will serve as coordinates for your app toocommunicate with Azure AD.</span></span>

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

5. <span data-ttu-id="b7f91-146">Mostantól a `[Authorize]` attribútumok toohelp megvédje a tartományvezérlőket és a műveletek JSON webes jogkivonat (JWT) tulajdonosi hitelesítéssel.</span><span class="sxs-lookup"><span data-stu-id="b7f91-146">Now you can use `[Authorize]` attributes toohelp protect your controllers and actions with JSON Web Token (JWT) bearer authentication.</span></span> <span data-ttu-id="b7f91-147">Adja a hello `Controllers\TodoListController.cs` osztály engedélyezés címke használatával.</span><span class="sxs-lookup"><span data-stu-id="b7f91-147">Decorate hello `Controllers\TodoListController.cs` class with an authorize tag.</span></span> <span data-ttu-id="b7f91-148">Ezzel kikényszeríti a hello felhasználói toosign a lap elérése előtt.</span><span class="sxs-lookup"><span data-stu-id="b7f91-148">This will force hello user toosign in before accessing that page.</span></span>

    ```C#
    [Authorize]
    public class TodoListController : ApiController
    {
    ```

    <span data-ttu-id="b7f91-149">Amikor egy jogosult hívó sikeresen hív meg egyik hello `TodoListController` API-k, hello művelet is kell hozzáférhetnek tooinformation hello hívó kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="b7f91-149">When an authorized caller successfully invokes one of hello `TodoListController` APIs, hello action might need access tooinformation about hello caller.</span></span> <span data-ttu-id="b7f91-150">OWIN hozzáférés toohello jogcímeket belül hello tulajdonosi jogkivonattal hello keresztül biztosít `ClaimsPrincpal` objektum.</span><span class="sxs-lookup"><span data-stu-id="b7f91-150">OWIN provides access toohello claims inside hello bearer token via hello `ClaimsPrincpal` object.</span></span>  

6. <span data-ttu-id="b7f91-151">Kötelező megadni a webes API-k toovalidate hello "hatókör" hello lexikális elem szerepel.</span><span class="sxs-lookup"><span data-stu-id="b7f91-151">A common requirement for web APIs is toovalidate hello "scopes" present in hello token.</span></span> <span data-ttu-id="b7f91-152">Ez biztosítja, hogy hello a felhasználó hozzájárult toohello engedélyek szükséges tooaccess hello tooDo lista szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b7f91-152">This ensures that hello user has consented toohello permissions required tooaccess hello tooDo List Service.</span></span>

    ```C#
    public IEnumerable<TodoItem> Get()
    {
        // user_impersonation is hello default permission exposed by applications in Azure AD
        if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
        {
            throw new HttpResponseException(new HttpResponseMessage {
              StatusCode = HttpStatusCode.Unauthorized,
              ReasonPhrase = "hello Scope claim does not contain 'user_impersonation' or scope claim not found"
            });
        }
        ...
    }
    ```

7. <span data-ttu-id="b7f91-153">Nyissa meg hello `web.config` hello TodoListService projekt gyökerében hello fájlt, és adja meg a konfigurációs értékek hello `<appSettings>` szakasz.</span><span class="sxs-lookup"><span data-stu-id="b7f91-153">Open hello `web.config` file in hello root of hello TodoListService project, and enter your configuration values in hello `<appSettings>` section.</span></span>
  * <span data-ttu-id="b7f91-154">`ida:Tenant`az Azure AD-bérlőn – például a contoso.onmicrosoft.com hello név.</span><span class="sxs-lookup"><span data-stu-id="b7f91-154">`ida:Tenant` is hello name of your Azure AD tenant--for example, contoso.onmicrosoft.com.</span></span>
  * <span data-ttu-id="b7f91-155">`ida:Audience`az App ID URI hello alkalmazás hello Azure-portálon megadott hello.</span><span class="sxs-lookup"><span data-stu-id="b7f91-155">`ida:Audience` is hello App ID URI of hello application that you entered in hello Azure portal.</span></span>

## <a name="step-3-configure-a-client-application-and-run-hello-service"></a><span data-ttu-id="b7f91-156">3. lépés: Ügyfélalkalmazás konfigurálása és hello szolgáltatás futtatásához</span><span class="sxs-lookup"><span data-stu-id="b7f91-156">Step 3: Configure a client application and run hello service</span></span>
<span data-ttu-id="b7f91-157">Ahhoz, hogy hello tooDo lista szolgáltatás működés közben, tooconfigure hello tooDo lista ügyfél szüksége, hogy azok jogkivonatokhoz az Azure AD, és ellenőrizze a hívások toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b7f91-157">Before you can see hello tooDo List Service in action, you need tooconfigure hello tooDo List client so it can get tokens from Azure AD and make calls toohello service.</span></span>

1. <span data-ttu-id="b7f91-158">Lépjen vissza toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b7f91-158">Go back toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="b7f91-159">Új alkalmazás létrehozása az Azure AD-bérlőn, és válassza ki **natív ügyfélalkalmazás** hello eredményül kapott kér.</span><span class="sxs-lookup"><span data-stu-id="b7f91-159">Create a new application in your Azure AD tenant, and select **Native Client Application** in hello resulting prompt.</span></span>
  * <span data-ttu-id="b7f91-160">**Név** az alkalmazás toousers ismerteti.</span><span class="sxs-lookup"><span data-stu-id="b7f91-160">**Name** describes your application toousers.</span></span>
  * <span data-ttu-id="b7f91-161">Adja meg `http://TodoListClient/` a hello **átirányítási Uri-** érték.</span><span class="sxs-lookup"><span data-stu-id="b7f91-161">Enter `http://TodoListClient/` for hello **Redirect Uri** value.</span></span>

3. <span data-ttu-id="b7f91-162">Regisztráció befejezése után az Azure AD egy egyedi alkalmazás azonosítója tooyour app rendeli hozzá.</span><span class="sxs-lookup"><span data-stu-id="b7f91-162">After you finish registration, Azure AD assigns a unique application ID tooyour app.</span></span> <span data-ttu-id="b7f91-163">Ez az érték kell a következő lépésekkel hello, ezért másolja hello alkalmazás oldalról.</span><span class="sxs-lookup"><span data-stu-id="b7f91-163">You’ll need this value in hello next steps, so copy it from hello application page.</span></span>

4. <span data-ttu-id="b7f91-164">A hello **beállítások** lapon jelölje be **szükséges engedélyek**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="b7f91-164">From hello **Settings** page, select **Required Permissions**, and then select **Add**.</span></span> <span data-ttu-id="b7f91-165">Keresse meg és válassza ki a lista szolgáltatás hello tooDo, adja hozzá a hello **hozzáférés TodoListService** engedélyt a **delegált engedélyek**, és kattintson a **végzett**.</span><span class="sxs-lookup"><span data-stu-id="b7f91-165">Locate and select hello tooDo List Service, add hello **Access TodoListService** permission under **Delegated Permissions**, and then click **Done**.</span></span>

5. <span data-ttu-id="b7f91-166">A Visual Studióban nyissa meg a `App.config` hello TodoListClient projektet, és adja meg a konfigurációs értékek hello `<appSettings>` szakasz.</span><span class="sxs-lookup"><span data-stu-id="b7f91-166">In Visual Studio, open `App.config` in hello TodoListClient project, and then enter your configuration values in hello `<appSettings>` section.</span></span>

  * <span data-ttu-id="b7f91-167">`ida:Tenant`az Azure AD-bérlőn – például a contoso.onmicrosoft.com hello név.</span><span class="sxs-lookup"><span data-stu-id="b7f91-167">`ida:Tenant` is hello name of your Azure AD tenant--for example, contoso.onmicrosoft.com.</span></span>
  * <span data-ttu-id="b7f91-168">`ida:ClientId`a rendszer hello Azure-portálon fájlból másolt hello alkalmazás Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="b7f91-168">`ida:ClientId` is hello app ID that you copied from hello Azure portal.</span></span>
  * <span data-ttu-id="b7f91-169">`todo:TodoListResourceId`hello App ID URI-azonosítója hello tooDo hello Azure-portálon megadott lista szolgáltatásalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b7f91-169">`todo:TodoListResourceId` is hello App ID URI of hello tooDo List Service application that you entered in hello Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b7f91-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b7f91-170">Next steps</span></span>
<span data-ttu-id="b7f91-171">Végül törlése, elkészítéséhez és futtatása minden olyan projekthez.</span><span class="sxs-lookup"><span data-stu-id="b7f91-171">Finally, clean, build, and run each project.</span></span> <span data-ttu-id="b7f91-172">Ha még nem tette meg, most már áll hello idő toocreate az Ön bérlőjében rendelkező új felhasználót a *. onmicrosoft.com tartományt.</span><span class="sxs-lookup"><span data-stu-id="b7f91-172">If you haven’t already, now is hello time toocreate a new user in your tenant with a *.onmicrosoft.com domain.</span></span> <span data-ttu-id="b7f91-173">Toohello tooDo lista ügyfél számára, hogy a felhasználó bejelentkezhet, és adja hozzá az egyes feladatok toohello felhasználók feladatlistáit.</span><span class="sxs-lookup"><span data-stu-id="b7f91-173">Sign in toohello tooDo List client with that user, and add some tasks toohello user's to-do list.</span></span>

<span data-ttu-id="b7f91-174">Referenciaként befejeződött hello mintát (a konfigurációs értékek nélkül) érhető el a [GitHub](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="b7f91-174">For reference, hello completed sample (without your configuration values) is available in [GitHub](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="b7f91-175">Most már továbbléphet toomore identitás forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="b7f91-175">You can now move on toomore identity scenarios.</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
