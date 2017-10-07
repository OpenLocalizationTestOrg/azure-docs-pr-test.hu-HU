---
title: "egy webes API háttéralkalmazás, az Azure Active Directory és az API Management aaaProtect |} Microsoft Docs"
description: "Ismerje meg, hogy egy webes API háttéralkalmazás, az Azure Active Directory és az API Management tooprotect."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: f856ff03-64a1-4548-9ec4-c0ec4cc1600f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: f4b323034354aa09579c643bade47257fbf1e5c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooprotect-a-web-api-backend-with-azure-active-directory-and-api-management"></a><span data-ttu-id="825d2-103">Hogyan tooprotect egy webes API háttéralkalmazás, az Azure Active Directory és az API Management</span><span class="sxs-lookup"><span data-stu-id="825d2-103">How tooprotect a Web API backend with Azure Active Directory and API Management</span></span>
<span data-ttu-id="825d2-104">a következő videó bemutatja hogyan hello toobuild egy webes API háttéralkalmazás, és megvédhetik azokat az Azure Active Directory és az API Management OAuth 2.0 protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="825d2-104">hello following video shows how toobuild a Web API backend and protect it using OAuth 2.0 protocol with Azure Active Directory and API Management.</span></span>  <span data-ttu-id="825d2-105">Ez a cikk áttekintést nyújt, és további információk a hello hello videó lépéseket.</span><span class="sxs-lookup"><span data-stu-id="825d2-105">This article provides an overview and additional information for hello steps in hello video.</span></span> <span data-ttu-id="825d2-106">A 24 perces videó bemutatja, hogyan számára:</span><span class="sxs-lookup"><span data-stu-id="825d2-106">This 24 minute video shows you how to:</span></span>

* <span data-ttu-id="825d2-107">Hozza létre egy webes API háttéralkalmazás, és biztosíthatja az AAD - kezdő pozíció: 1:30-ben</span><span class="sxs-lookup"><span data-stu-id="825d2-107">Build a Web API backend and secure it with AAD - starting at 1:30</span></span>
* <span data-ttu-id="825d2-108">Az API Management - 7:10 kezdődő hello API importálása</span><span class="sxs-lookup"><span data-stu-id="825d2-108">Import hello API into API Management - starting at 7:10</span></span>
* <span data-ttu-id="825d2-109">Hello Developer portálon toocall hello API – 9:09 kezdődő konfigurálása</span><span class="sxs-lookup"><span data-stu-id="825d2-109">Configure hello Developer portal toocall hello API - starting at 9:09</span></span>
* <span data-ttu-id="825d2-110">Egy asztali alkalmazás toocall hello API – 18:08 kezdődő konfigurálása</span><span class="sxs-lookup"><span data-stu-id="825d2-110">Configure a desktop application toocall hello API - starting at 18:08</span></span>
* <span data-ttu-id="825d2-111">Konfigurálja a JWT-ellenőrzési házirend toopre-kérelmek - 20:47 kezdődő engedélyezése</span><span class="sxs-lookup"><span data-stu-id="825d2-111">Configure a JWT validation policy toopre-authorize requests - starting at 20:47</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Protecting-Web-API-Backend-with-Azure-Active-Directory-and-API-Management/player]
> 
> 

## <a name="create-an-azure-ad-directory"></a><span data-ttu-id="825d2-112">Az Azure AD-címtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="825d2-112">Create an Azure AD directory</span></span>
<span data-ttu-id="825d2-113">a Web API biztonsági másolatot az Azure Active Directoryval kell toosecure egy AAD-bérlőt.</span><span class="sxs-lookup"><span data-stu-id="825d2-113">toosecure your Web API backed using Azure Active Directory you must first have a an AAD tenant.</span></span> <span data-ttu-id="825d2-114">Ez a videó egy bérlő nevű **APIMDemo** szolgál.</span><span class="sxs-lookup"><span data-stu-id="825d2-114">In this video a tenant named **APIMDemo** is used.</span></span> <span data-ttu-id="825d2-115">toocreate egy AAD-bérlőt bejelentkezési toohello [klasszikus Azure portál](https://manage.windowsazure.com) kattintson **új**->**alkalmazásszolgáltatások**->**aktív Directory**->**Directory**->**egyéni létrehozás**.</span><span class="sxs-lookup"><span data-stu-id="825d2-115">toocreate an AAD tenant, sign-in toohello [Azure Classic Portal](https://manage.windowsazure.com) and click **New**->**App Services**->**Active Directory**->**Directory**->**Custom Create**.</span></span> 

![Azure Active Directory][api-management-create-aad-menu]

<span data-ttu-id="825d2-117">Ebben a példában egy nevű könyvtár **APIMDemo** létrejön egy alapértelmezett tartomány nevű **DemoAPIM.onmicrosoft.com**. Ez a könyvtár használja a rendszer hello videó keresztül.</span><span class="sxs-lookup"><span data-stu-id="825d2-117">In this example a directory named **APIMDemo** is created with a default domain named **DemoAPIM.onmicrosoft.com**. This directory is used throughout hello video.</span></span>

![Azure Active Directory][api-management-create-aad]

## <a name="create-a-web-api-service-secured-by-azure-active-directory"></a><span data-ttu-id="825d2-119">Azure Active Directory által biztosított webes API szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="825d2-119">Create a Web API service secured by Azure Active Directory</span></span>
<span data-ttu-id="825d2-120">Ebben a lépésben egy webes API háttéralkalmazás létrehozása a Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="825d2-120">In this step, a Web API backend is created using Visual Studio 2013.</span></span> <span data-ttu-id="825d2-121">Ez a lépés a videó hello kezdődik, 1:30.</span><span class="sxs-lookup"><span data-stu-id="825d2-121">This step of hello video starts at 1:30.</span></span> <span data-ttu-id="825d2-122">toocreate webes API háttéralkalmazás-projekt a Visual Studio kattintson **fájl**->**új**->**projekt**, és válassza a **ASP.NET webalkalmazás Alkalmazás** a hello **webes** sablonok listájának.</span><span class="sxs-lookup"><span data-stu-id="825d2-122">toocreate Web API backend project in Visual Studio click **File**->**New**->**Project**, and choose **ASP.NET Web Application** from hello **Web** templates list.</span></span> <span data-ttu-id="825d2-123">Ez a videó hello projekt neve **APIMAADDemo**.</span><span class="sxs-lookup"><span data-stu-id="825d2-123">In this video hello project is named **APIMAADDemo**.</span></span> <span data-ttu-id="825d2-124">Kattintson a **OK** toocreate hello projekt.</span><span class="sxs-lookup"><span data-stu-id="825d2-124">Click **OK** toocreate hello project.</span></span> 

![Visual Studio][api-management-new-web-app]

<span data-ttu-id="825d2-126">Kattintson a **Web API** a hello **jelöljön ki egy sablon listát** toocreate Web API-projektet.</span><span class="sxs-lookup"><span data-stu-id="825d2-126">Click **Web API** from hello **Select a template list** toocreate a Web API project.</span></span> <span data-ttu-id="825d2-127">Kattintson az Azure Directory hitelesítési tooconfigure **hitelesítés módosítása**.</span><span class="sxs-lookup"><span data-stu-id="825d2-127">tooconfigure Azure Directory Authentication click **Change Authentication**.</span></span>

![Új projekt][api-management-new-project]

<span data-ttu-id="825d2-129">Kattintson a **munkahelyi és iskolai fiókok**, és adja meg a hello **tartomány** , az AAD-bérlőt.</span><span class="sxs-lookup"><span data-stu-id="825d2-129">Click **Organizational Accounts**, and specify hello **Domain** of your AAD tenant.</span></span> <span data-ttu-id="825d2-130">A példa hello a tartomány pedig **DemoAPIM.onmicrosoft.com**. a címtár hello tartomány hello lehet lekérni **tartományok** a címtár lapján.</span><span class="sxs-lookup"><span data-stu-id="825d2-130">In this example hello domain is **DemoAPIM.onmicrosoft.com**. hello domain of your directory can be obtained from hello **Domains** tab of your directory.</span></span>

![Tartományok][api-management-aad-domains]

<span data-ttu-id="825d2-132">Hello szükséges beállítások konfigurálása a hello **hitelesítés módosítása** párbeszédpanel megnyitásához, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="825d2-132">Configure hello desired settings in hello **Change Authentication** dialog box and click **OK**.</span></span>

![Hitelesítés módosítása][api-management-change-authentication]

<span data-ttu-id="825d2-134">Amikor rákattint **OK** Visual Studio megpróbál tooregister az Azure AD-címtár az alkalmazásba, és előfordulhat, hogy a Visual Studio által kért toosign.</span><span class="sxs-lookup"><span data-stu-id="825d2-134">When you click **OK** Visual Studio will attempt tooregister your application with your Azure AD directory and you may be prompted toosign in by Visual Studio.</span></span> <span data-ttu-id="825d2-135">Jelentkezzen be egy rendszergazdai fiókkal a címtáron.</span><span class="sxs-lookup"><span data-stu-id="825d2-135">Sign in using an administrative account for your directory.</span></span>

![Jelentkezzen be tooVisual Studio][api-management-sign-in-vidual-studio]

<span data-ttu-id="825d2-137">Ebben a projektben, mint egy Azure webes API-ellenőrzés hello mezőjének tooconfigure **hello felhőben lévő gazdagéphez** majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="825d2-137">tooconfigure this project as an Azure Web API check hello box for **Host in hello cloud** and then click **OK**.</span></span>

![Új projekt][api-management-new-project-cloud]

<span data-ttu-id="825d2-139">Előfordulhat, hogy a tooAzure rákérdezéses toosign, és a Web App hello beállításához használhatja.</span><span class="sxs-lookup"><span data-stu-id="825d2-139">You may be prompted toosign in tooAzure, and then you can configure hello Web App.</span></span>

![Konfigurálás][api-management-configure-web-app]

<span data-ttu-id="825d2-141">Ebben a példában új **App Service-csomag** nevű **APIMAADDemo** van megadva.</span><span class="sxs-lookup"><span data-stu-id="825d2-141">In this example a new **App Service plan** named **APIMAADDemo** is specified.</span></span>

<span data-ttu-id="825d2-142">Kattintson a **OK** tooconfigure hello Web App és hello projekt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="825d2-142">Click **OK** tooconfigure hello Web App and create hello project.</span></span>

## <a name="add-hello-code-toohello-web-api-project"></a><span data-ttu-id="825d2-143">Hello kód toohello Web API-projekt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="825d2-143">Add hello code toohello Web API project</span></span>
<span data-ttu-id="825d2-144">hello következő lépése hello videó hello kód toohello Web API-projektet ad hozzá.</span><span class="sxs-lookup"><span data-stu-id="825d2-144">hello next step in hello video adds hello code toohello Web API project.</span></span> <span data-ttu-id="825d2-145">Ez a lépés 4:35 kezdődik.</span><span class="sxs-lookup"><span data-stu-id="825d2-145">This step starts at 4:35.</span></span>

<span data-ttu-id="825d2-146">hello Web API ebben a példában egy modell és a vezérlő alapvető Számológép szolgáltatás megvalósítja.</span><span class="sxs-lookup"><span data-stu-id="825d2-146">hello Web API in this example implements a basic calculator service using a model and a controller.</span></span> <span data-ttu-id="825d2-147">tooadd hello modellt hello szolgáltatáshoz, kattintson a jobb gombbal **modellek** a **Megoldáskezelőben** válassza **Hozzáadás**, **osztály**.</span><span class="sxs-lookup"><span data-stu-id="825d2-147">tooadd hello model for hello service, right-click **Models** in **Solution Explorer** and choose **Add**, **Class**.</span></span> <span data-ttu-id="825d2-148">Hello osztály neve `CalcInput` kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="825d2-148">Name hello class `CalcInput` and click **Add**.</span></span>

<span data-ttu-id="825d2-149">Adja hozzá a következő hello `using` utasítás toohello felső részén hello `CalcInput.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="825d2-149">Add hello following `using` statement toohello top of hello `CalcInput.cs` file.</span></span>

```c#
using Newtonsoft.Json;
```

<span data-ttu-id="825d2-150">Cserélje le a következő kód hello generált hello osztály.</span><span class="sxs-lookup"><span data-stu-id="825d2-150">Replace hello generated class with hello following code.</span></span>

```c#
public class CalcInput
{
    [JsonProperty(PropertyName = "a")]
    public int a;

    [JsonProperty(PropertyName = "b")]
    public int b;
}
```

<span data-ttu-id="825d2-151">Kattintson a jobb gombbal **tartományvezérlők** a **Megoldáskezelőben** válassza **Hozzáadás**->**vezérlő**.</span><span class="sxs-lookup"><span data-stu-id="825d2-151">Right-click **Controllers** in **Solution Explorer** and choose **Add**->**Controller**.</span></span> <span data-ttu-id="825d2-152">Válasszon **Web API 2 vezérlő - üres** kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="825d2-152">Choose **Web API 2 Controller - Empty** and click **Add**.</span></span> <span data-ttu-id="825d2-153">Típus **CalcController** hello, tartományvezérlői nevet, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="825d2-153">Type **CalcController** for hello Controller name and click **Add**.</span></span>

![Vezérlő hozzáadása][api-management-add-controller]

<span data-ttu-id="825d2-155">Adja hozzá a következő hello `using` utasítás toohello felső részén hello `CalcController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="825d2-155">Add hello following `using` statement toohello top of hello `CalcController.cs` file.</span></span>

```c#
using System.IO;
using System.Web;
using APIMAADDemo.Models;
```

<span data-ttu-id="825d2-156">Generált hello vezérlőosztály cserélje le a következő kód hello.</span><span class="sxs-lookup"><span data-stu-id="825d2-156">Replace hello generated controller class with hello following code.</span></span> <span data-ttu-id="825d2-157">Ez a kód megvalósítja hello `Add`, `Subtract`, `Multiply`, és `Divide` hello alapvető Számológép API működésére.</span><span class="sxs-lookup"><span data-stu-id="825d2-157">This code implements hello `Add`, `Subtract`, `Multiply`, and `Divide` operations of hello Basic Calculator API.</span></span>

```c#
[Authorize]
public class CalcController : ApiController
{
    [Route("api/add")]
    [HttpGet]
    public HttpResponseMessage GetSum([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a + b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/sub")]
    [HttpGet]
    public HttpResponseMessage GetDiff([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a - b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/mul")]
    [HttpGet]
    public HttpResponseMessage GetProduct([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a * b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/div")]
    [HttpGet]
    public HttpResponseMessage GetDiv([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a / b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }
}
```

<span data-ttu-id="825d2-158">Nyomja le az **F6** toobuild, és ellenőrizze a hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="825d2-158">Press **F6** toobuild and verify hello solution.</span></span>

## <a name="publish-hello-project-tooazure"></a><span data-ttu-id="825d2-159">Hello projekt tooAzure közzététele</span><span class="sxs-lookup"><span data-stu-id="825d2-159">Publish hello project tooAzure</span></span>
<span data-ttu-id="825d2-160">Ez a lépés hello Visual Studio projekt közzétett tooAzure.</span><span class="sxs-lookup"><span data-stu-id="825d2-160">In this step hello Visual Studio project is published tooAzure.</span></span> <span data-ttu-id="825d2-161">Ez a lépés a videó hello 5:45 kezdődik.</span><span class="sxs-lookup"><span data-stu-id="825d2-161">This step of hello video starts at 5:45.</span></span>

<span data-ttu-id="825d2-162">toopublish hello projekt tooAzure, kattintson a jobb gombbal a hello **APIMAADDemo** a Visual Studio projekt, és válassza a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="825d2-162">toopublish hello project tooAzure, right-click hello **APIMAADDemo** project in Visual Studio and choose **Publish**.</span></span> <span data-ttu-id="825d2-163">Hello alapértelmezett beállítások megtartásához a hello **webhely közzététele** párbeszédpanel megnyitásához, és kattintson **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="825d2-163">Keep hello default settings in hello **Publish Web** dialog box and click **Publish**.</span></span>

![Webes közzététel][api-management-web-publish]

## <a name="grant-permissions-toohello-azure-ad-backend-service-application"></a><span data-ttu-id="825d2-165">Engedélyek toohello az Azure AD szolgáltatás a háttéralkalmazás</span><span class="sxs-lookup"><span data-stu-id="825d2-165">Grant permissions toohello Azure AD backend service application</span></span>
<span data-ttu-id="825d2-166">Egy új hello háttérszolgáltatás-alkalmazást az Azure AD-címtár és a webes API-projekt közzétételi folyamat hello konfigurálása részét képező jön létre.</span><span class="sxs-lookup"><span data-stu-id="825d2-166">A new application for hello backend service is created in your Azure AD directory as part of hello configuring and publishing process of your Web API project.</span></span> <span data-ttu-id="825d2-167">Ebben a lépésben hello videót 6:13, kezdődő engedélyekkel toohello webes API háttéralkalmazás.</span><span class="sxs-lookup"><span data-stu-id="825d2-167">In this step of hello video, starting at 6:13, permissions are granted toohello Web API backend.</span></span>

![Alkalmazás][api-management-aad-backend-app]

<span data-ttu-id="825d2-169">Kattintson a szükséges hello tooconfigure hello Alkalmazásengedélyek hello nevére.</span><span class="sxs-lookup"><span data-stu-id="825d2-169">Click hello name of hello application tooconfigure hello required permissions.</span></span> <span data-ttu-id="825d2-170">Keresse meg a toohello **konfigurálása** lapot, és görgessen lefelé toohello **engedélyek tooother alkalmazások** szakasz.</span><span class="sxs-lookup"><span data-stu-id="825d2-170">Navigate toohello **Configure** tab and scroll down toohello **permissions tooother applications** section.</span></span> <span data-ttu-id="825d2-171">Kattintson a hello **Alkalmazásengedélyek** melletti legördülő **Windows** **Azure Active Directory**, hello jelölőnégyzetet a **címtáradatokolvasása**, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="825d2-171">Click hello **Application Permissions** drop-down beside **Windows** **Azure Active Directory**, check hello box for **Read directory data**, and click **Save**.</span></span>

![Engedélyek hozzáadása][api-management-aad-add-permissions]

> [!NOTE]
> <span data-ttu-id="825d2-173">Ha **Windows** **Azure Active Directory** van engedélyek tooother alkalmazások nem szereplő, kattintson a **alkalmazás hozzáadása** , és adja hozzá a hello listából.</span><span class="sxs-lookup"><span data-stu-id="825d2-173">If **Windows** **Azure Active Directory** is not listed under permissions tooother applications, click **Add application** and add it from hello list.</span></span>
> 
> 

<span data-ttu-id="825d2-174">Jegyezze fel a hello **App Id URI** használható egy későbbi lépésben, ha egy Azure AD-alkalmazást úgy van konfigurálva hello API Management fejlesztői portálján.</span><span class="sxs-lookup"><span data-stu-id="825d2-174">Make a note of hello **App Id URI** for use in a subsequent step when an Azure AD application is configured for hello API Management developer portal.</span></span>

![App Id URI][api-management-aad-sso-uri]

## <a name="import-hello-web-api-into-api-management"></a><span data-ttu-id="825d2-176">Webes API hello importálja az API Management</span><span class="sxs-lookup"><span data-stu-id="825d2-176">Import hello Web API into API Management</span></span>
<span data-ttu-id="825d2-177">API-k hello Azure portálon keresztül érhető el hello API publisher portálon vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="825d2-177">APIs are configured from hello API publisher portal, which is accessed through hello Azure Portal.</span></span> <span data-ttu-id="825d2-178">tooreach, kattintson a **Publisher portal** hello eszköztáron az API Management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="825d2-178">tooreach it, click **Publisher portal** from hello toolbar of your API Management service.</span></span> <span data-ttu-id="825d2-179">Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [kezelése az első API] [ Manage your first API] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="825d2-179">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Manage your first API][Manage your first API] tutorial.</span></span>

![Közzétevő portál][api-management-management-console]

<span data-ttu-id="825d2-181">A műveleteket lehet [tooAPIs manuálisan hozzáadni](api-management-howto-add-operations.md), vagy az importálható lesz.</span><span class="sxs-lookup"><span data-stu-id="825d2-181">Operations can be [added tooAPIs manually](api-management-howto-add-operations.md), or they can be imported.</span></span> <span data-ttu-id="825d2-182">Ez a videó műveletek importált 6:40 kezdődő Swagger formátumú.</span><span class="sxs-lookup"><span data-stu-id="825d2-182">In this video, operations are imported in Swagger format starting at 6:40.</span></span>

<span data-ttu-id="825d2-183">Hozzon létre egy fájlt `calcapi.json` a következő tartalmát, és mentse azt tooyour számítógép.</span><span class="sxs-lookup"><span data-stu-id="825d2-183">Create a file named `calcapi.json` with following contents and save it tooyour computer.</span></span> <span data-ttu-id="825d2-184">Győződjön meg arról, hogy hello `host` attribútum tooyour webes API háttéralkalmazás mutat.</span><span class="sxs-lookup"><span data-stu-id="825d2-184">Ensure that hello `host` attribute points tooyour Web API backend.</span></span> <span data-ttu-id="825d2-185">Ebben a példában `"host": "apimaaddemo.azurewebsites.net"` szolgál.</span><span class="sxs-lookup"><span data-stu-id="825d2-185">In this example `"host": "apimaaddemo.azurewebsites.net"` is used.</span></span>

```json
{
  "swagger": "2.0",
  "info": {
    "title": "Calculator",
    "description": "Arithmetics over HTTP!",
    "version": "1.0"
  },
  "host": "apimaaddemo.azurewebsites.net",
  "basePath": "/api",
  "schemes": [
    "http"
  ],
  "paths": {
    "/add?a={a}&b={b}": {
      "get": {
        "description": "Responds with a sum of two numbers.",
        "operationId": "Add two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>51</code>.",
            "required": true,
            "type": "string",
            "default": "51",
            "enum": [
              "51"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>49</code>.",
            "required": true,
            "type": "string",
            "default": "49",
            "enum": [
              "49"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/sub?a={a}&b={b}": {
      "get": {
        "description": "Responds with a difference between two numbers.",
        "operationId": "Subtract two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>100</code>.",
            "required": true,
            "type": "string",
            "default": "100",
            "enum": [
              "100"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>50</code>.",
            "required": true,
            "type": "string",
            "default": "50",
            "enum": [
              "50"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/div?a={a}&b={b}": {
      "get": {
        "description": "Responds with a quotient of two numbers.",
        "operationId": "Divide two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>100</code>.",
            "required": true,
            "type": "string",
            "default": "100",
            "enum": [
              "100"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>20</code>.",
            "required": true,
            "type": "string",
            "default": "20",
            "enum": [
              "20"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/mul?a={a}&b={b}": {
      "get": {
        "description": "Responds with a product of two numbers.",
        "operationId": "Multiply two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>20</code>.",
            "required": true,
            "type": "string",
            "default": "20",
            "enum": [
              "20"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>5</code>.",
            "required": true,
            "type": "string",
            "default": "5",
            "enum": [
              "5"
            ]
          }
        ],
        "responses": { }
      }
    }
  }
}
```

<span data-ttu-id="825d2-186">tooimport hello Számológép API, kattintson a **API-k** a hello **API Management** hello maradt, és válassza a menü **importálási API**.</span><span class="sxs-lookup"><span data-stu-id="825d2-186">tooimport hello calculator API, click **APIs** from hello **API Management** menu on hello left, and then click **Import API**.</span></span>

![API importálása gomb][api-management-import-api]

<span data-ttu-id="825d2-188">Hajtsa végre a következő lépéseket tooconfigure hello Számológép API hello.</span><span class="sxs-lookup"><span data-stu-id="825d2-188">Perform hello following steps tooconfigure hello calculator API.</span></span>

1. <span data-ttu-id="825d2-189">Kattintson a **fájlból**, keresse meg a toohello `calculator.json` fájlt mentette, majd kattintson a hello **Swagger** választógombot.</span><span class="sxs-lookup"><span data-stu-id="825d2-189">Click **From file**, browse toohello `calculator.json` file you saved, and click hello **Swagger** radio button.</span></span>
2. <span data-ttu-id="825d2-190">Típus **Számológép** történő hello **webes API URL-címe utótag** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="825d2-190">Type **calc** into hello **Web API URL suffix** textbox.</span></span>
3. <span data-ttu-id="825d2-191">Kattintson a hello **(nem kötelező) termékek** listát, és válassza **alapszintű**.</span><span class="sxs-lookup"><span data-stu-id="825d2-191">Click in hello **Products (optional)** box and choose **Starter**.</span></span>
4. <span data-ttu-id="825d2-192">Kattintson a **mentése** tooimport hello API.</span><span class="sxs-lookup"><span data-stu-id="825d2-192">Click **Save** tooimport hello API.</span></span>

![Új API hozzáadása][api-management-import-new-api]

<span data-ttu-id="825d2-194">Hello API importálása után hello hello API az összefoglalás lapon megjelenik hello publisher portálon.</span><span class="sxs-lookup"><span data-stu-id="825d2-194">Once hello API is imported, hello summary page for hello API is displayed in hello publisher portal.</span></span>

## <a name="call-hello-api-unsuccessfully-from-hello-developer-portal"></a><span data-ttu-id="825d2-195">Hello API sikertelenül hívható hello developer portálról</span><span class="sxs-lookup"><span data-stu-id="825d2-195">Call hello API unsuccessfully from hello developer portal</span></span>
<span data-ttu-id="825d2-196">Ezen a ponton hello API API Management importálták, de nem még hívható sikeresen hello developer portálról mert hello háttérszolgáltatást védi az Azure AD-alapú hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="825d2-196">At this point, hello API has been imported into API Management, but cannot yet be called successfully from hello developer portal because hello backend service is protected with Azure AD authentication.</span></span> <span data-ttu-id="825d2-197">Ezt mutatják be hello videó kezdődő 7:40 hello lépések használatával.</span><span class="sxs-lookup"><span data-stu-id="825d2-197">This is demonstrated in hello video starting at 7:40 using hello following steps.</span></span>

<span data-ttu-id="825d2-198">Kattintson a **fejlesztői portálján** hello publisher portál jobb felső oldalán hello.</span><span class="sxs-lookup"><span data-stu-id="825d2-198">Click **Developer portal** from hello top-right side of hello publisher portal.</span></span>

![Fejlesztői portál][api-management-developer-portal-menu]

<span data-ttu-id="825d2-200">Kattintson a **API-k** hello kattintson **Számológép** API.</span><span class="sxs-lookup"><span data-stu-id="825d2-200">Click **APIs** and click hello **Calculator** API.</span></span>

![Fejlesztői portál][api-management-dev-portal-apis]

<span data-ttu-id="825d2-202">Kattintson a **kipróbálás**.</span><span class="sxs-lookup"><span data-stu-id="825d2-202">Click **Try it**.</span></span>

![Kipróbálom][api-management-dev-portal-try-it]

<span data-ttu-id="825d2-204">Kattintson a **küldése** meg és jegyezze fel a hello állapotú **401 nem engedélyezett**.</span><span class="sxs-lookup"><span data-stu-id="825d2-204">Click **Send** and note hello response status of **401 Unauthorized**.</span></span>

![Küldés][api-management-dev-portal-send-401]

<span data-ttu-id="825d2-206">hello kérelem nem engedélyezett, mert hello háttér-API Azure Active Directory védi.</span><span class="sxs-lookup"><span data-stu-id="825d2-206">hello request is unauthorized because hello backend API is protected by Azure Active Directory.</span></span> <span data-ttu-id="825d2-207">Sikeresen megtörtént a hello API hívása előtt hello fejlesztői portálján konfigurált tooauthorize fejlesztők OAuth 2.0 használatával kell lennie.</span><span class="sxs-lookup"><span data-stu-id="825d2-207">Before successfully calling hello API hello developer portal must be configured tooauthorize developers using OAuth 2.0.</span></span> <span data-ttu-id="825d2-208">Ez a folyamat a következő szakaszok hello leírását.</span><span class="sxs-lookup"><span data-stu-id="825d2-208">This process is described in hello following sections.</span></span>

## <a name="register-hello-developer-portal-as-an-aad-application"></a><span data-ttu-id="825d2-209">Hello fejlesztői portálján regisztrálható egy AAD-alkalmazást</span><span class="sxs-lookup"><span data-stu-id="825d2-209">Register hello developer portal as an AAD application</span></span>
<span data-ttu-id="825d2-210">hello első lépés az OAuth 2.0 használatával hello developer portálon tooauthorize fejlesztők tooregister hello fejlesztői portálján, mint egy AAD-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="825d2-210">hello first step in configuring hello developer portal tooauthorize developers using OAuth 2.0 is tooregister hello developer portal as an AAD application.</span></span> <span data-ttu-id="825d2-211">Ezt mutatják 8:27 hello videó kezdve.</span><span class="sxs-lookup"><span data-stu-id="825d2-211">This is demonstrated starting at 8:27 in hello video.</span></span>

<span data-ttu-id="825d2-212">Keresse meg az ebben a példában ez a videó első lépése hello toohello az Azure AD bérlő **APIMDemo** , és keresse meg a toohello **alkalmazások** fülre.</span><span class="sxs-lookup"><span data-stu-id="825d2-212">Navigate toohello Azure AD tenant from hello first step of this video, in this example **APIMDemo** and navigate toohello **Applications** tab.</span></span>

![Új alkalmazás][api-management-aad-new-application-devportal]

<span data-ttu-id="825d2-214">Kattintson a hello **Hozzáadás** toocreate egy új Azure Active Directory-alkalmazás gombra, és válassza a **a szerveztem által fejlesztett alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="825d2-214">Click hello **Add** button toocreate a new Azure Active Directory application, and choose **Add an application my organization is developing**.</span></span>

![Új alkalmazás][api-management-new-aad-application-menu]

<span data-ttu-id="825d2-216">Válasszon **webes alkalmazáshoz és/vagy webes API**, adjon meg egy nevet, majd kattintson a Tovább nyílra hello.</span><span class="sxs-lookup"><span data-stu-id="825d2-216">Choose **Web application and/or Web API**, enter a name, and click hello next arrow.</span></span> <span data-ttu-id="825d2-217">Ebben a példában **APIMDeveloperPortal** szolgál.</span><span class="sxs-lookup"><span data-stu-id="825d2-217">In this example **APIMDeveloperPortal** is used.</span></span>

![Új alkalmazás][api-management-aad-new-application-devportal-1]

<span data-ttu-id="825d2-219">A **bejelentkezési URL-cím** meg hello URL-címet a API Management szolgáltatás és a hozzáfűző `/signin`.</span><span class="sxs-lookup"><span data-stu-id="825d2-219">For **Sign-on URL** enter hello URL of your API Management service and append `/signin`.</span></span> <span data-ttu-id="825d2-220">Ebben a példában `https://contoso5.portal.azure-api.net/signin` szolgál.</span><span class="sxs-lookup"><span data-stu-id="825d2-220">In this example `https://contoso5.portal.azure-api.net/signin` is used.</span></span>

<span data-ttu-id="825d2-221">A **azonosító URL-címet** meg hello URL-címet a API Management szolgáltatás és a hozzáfűző néhány egyedi karaktert.</span><span class="sxs-lookup"><span data-stu-id="825d2-221">For **App Id URL** enter hello URL of your API Management service and append some unique characters.</span></span> <span data-ttu-id="825d2-222">Ezek lehetnek a kívánt karaktereket, és ebben a példában `https://contoso5.portal.azure-api.net/dp` szolgál.</span><span class="sxs-lookup"><span data-stu-id="825d2-222">These can be any desired characters and in this example `https://contoso5.portal.azure-api.net/dp` is used.</span></span> <span data-ttu-id="825d2-223">Ha hello szükséges **alkalmazás tulajdonságainak** vannak konfigurálva, kattintson a hello pipa toocreate hello alkalmazásra.</span><span class="sxs-lookup"><span data-stu-id="825d2-223">When hello  desired **App properties** are configured, click hello check mark toocreate hello application.</span></span>

![Új alkalmazás][api-management-aad-new-application-devportal-2]

## <a name="configure-an-api-management-oauth-20-authorization-server"></a><span data-ttu-id="825d2-225">Az API Management OAuth 2.0 hitelesítési kiszolgáló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="825d2-225">Configure an API Management OAuth 2.0 authorization server</span></span>
<span data-ttu-id="825d2-226">következő lépés hello tooconfigure az OAuth 2.0 hitelesítési kiszolgáló az API Management.</span><span class="sxs-lookup"><span data-stu-id="825d2-226">hello next step is tooconfigure an OAuth 2.0 authorization server in API Management.</span></span> <span data-ttu-id="825d2-227">Ez a lépés mutatják be hello videó 9:43 kezdve.</span><span class="sxs-lookup"><span data-stu-id="825d2-227">This step is demonstrated in hello video starting at 9:43.</span></span>

<span data-ttu-id="825d2-228">Kattintson a **biztonsági** hello bal oldali hello API Management menüben kattintson **OAuth 2.0**, és kattintson a **adja hozzá az engedélyezési** kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="825d2-228">Click **Security** from hello API Management menu on hello left, click **OAuth 2.0**, and then click **Add authorization** server.</span></span>

![Engedélyezési kiszolgáló hozzáadása][api-management-add-authorization-server]

<span data-ttu-id="825d2-230">Adjon meg egy nevet és leírást nem kötelező hello **neve** és **leírás** mezőket.</span><span class="sxs-lookup"><span data-stu-id="825d2-230">Enter a name and an optional description in hello **Name** and **Description** fields.</span></span> <span data-ttu-id="825d2-231">A mezők kitöltése használt tooidentify hello OAuth 2.0 hitelesítési kiszolgáló belül hello API Management service-példány.</span><span class="sxs-lookup"><span data-stu-id="825d2-231">These fields are used tooidentify hello OAuth 2.0 authorization server within hello API Management service instance.</span></span> <span data-ttu-id="825d2-232">Ebben a példában **engedélyezési server bemutató** szolgál.</span><span class="sxs-lookup"><span data-stu-id="825d2-232">In this example **Authorization server demo** is used.</span></span> <span data-ttu-id="825d2-233">Később egy API-hitelesítéshez használt OAuth 2.0-kiszolgáló toobe megadásakor kiválaszthatja ezt a nevet.</span><span class="sxs-lookup"><span data-stu-id="825d2-233">Later when you specify an OAuth 2.0 server toobe used for authentication for an API, you will select this name.</span></span>

<span data-ttu-id="825d2-234">A hello **ügyfél regisztrációs URL-címe** adja meg például a helyőrző értékét `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="825d2-234">For hello **Client registration page URL** enter a placeholder value such as `http://localhost`.</span></span>  <span data-ttu-id="825d2-235">Hello **ügyfél regisztrációs URL-címe** toohello lapon, hogy a felhasználók pontok toocreate használhatja és a saját felhasználói konfigurálása a felhasználói fiókok kezelését támogató OAuth 2.0-s szolgáltatók.</span><span class="sxs-lookup"><span data-stu-id="825d2-235">hello **Client registration page URL** points toohello page that users can use toocreate and configure their own accounts for OAuth 2.0 providers that support user management of accounts.</span></span> <span data-ttu-id="825d2-236">Ebben a példában a felhasználók létrehozása és nem a saját felhasználói konfigurálása, így helyőrzőjeként szolgál.</span><span class="sxs-lookup"><span data-stu-id="825d2-236">In this example users do not create and configure their own accounts so a placeholder is used.</span></span>

![Engedélyezési kiszolgáló hozzáadása][api-management-add-authorization-server-1]

<span data-ttu-id="825d2-238">Ezt követően adja meg **engedélyezési végpont URL-címet** és **végponti URL-cím Token**.</span><span class="sxs-lookup"><span data-stu-id="825d2-238">Next, specify **Authorization endpoint URL** and **Token endpoint URL**.</span></span>

![engedélyezési kiszolgáló][api-management-add-authorization-server-1a]

<span data-ttu-id="825d2-240">Ezek az értékek lekérhetők hello **App végpontok** hello hello fejlesztői portálon létrehozott AAD-alkalmazást oldalán.</span><span class="sxs-lookup"><span data-stu-id="825d2-240">These values can be retrieved from hello **App Endpoints** page of hello AAD application you created for hello developer portal.</span></span> <span data-ttu-id="825d2-241">tooaccess hello végpontok lépjen toohello **konfigurálása** lapján hello AAD-alkalmazást, és kattintson a **végpontok megtekintése**.</span><span class="sxs-lookup"><span data-stu-id="825d2-241">tooaccess hello endpoints navigate toohello **Configure** tab for hello AAD application and click **View endpoints**.</span></span>

![Alkalmazás][api-management-aad-devportal-application]

![Végpontok megtekintése][api-management-aad-view-endpoints]

<span data-ttu-id="825d2-244">Másolás hello **OAuth 2.0 hitelesítési végpont** és illessze be hello **engedélyezési végpont URL-címet** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="825d2-244">Copy hello **OAuth 2.0 authorization endpoint** and paste it into hello **Authorization endpoint URL** textbox.</span></span>

![Engedélyezési kiszolgáló hozzáadása][api-management-add-authorization-server-2]

<span data-ttu-id="825d2-246">Másolás hello **OAuth 2.0 token-végpont** és illessze be hello **végponti URL-cím Token** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="825d2-246">Copy hello **OAuth 2.0 token endpoint** and paste it into hello **Token endpoint URL** textbox.</span></span>

![Engedélyezési kiszolgáló hozzáadása][api-management-add-authorization-server-2a]

<span data-ttu-id="825d2-248">Továbbá a jogkivonat végpontjához hello toopasting hozzáadása egy további válaszüzenettörzs-paramétert nevű **erőforrás** és hello értékhez használja a hello **App Id URI** hello hello háttérszolgáltatást, de a AAD-alkalmazást a Visual Studio-projekt hello közzétételekor létrehozott.</span><span class="sxs-lookup"><span data-stu-id="825d2-248">In addition toopasting in hello token endpoint, add an additional body parameter named **resource** and for hello value use hello **App Id URI** from hello AAD application for hello backend service that was created when hello Visual Studio project was published.</span></span>

![App Id URI][api-management-aad-sso-uri]

<span data-ttu-id="825d2-250">Ezt követően adja meg a hello ügyfél hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="825d2-250">Next, specify hello client credentials.</span></span> <span data-ttu-id="825d2-251">Ezek a kívánt erőforrás hello hello hitelesítő adatainak tooaccess, ebben az esetben hello fejlesztői portálján.</span><span class="sxs-lookup"><span data-stu-id="825d2-251">These are hello credentials for hello resource you want tooaccess, in this case hello developer portal.</span></span>

![Ügyfél hitelesítő adatait][api-management-client-credentials]

<span data-ttu-id="825d2-253">tooget hello **ügyfél-azonosító**, keresse meg a toohello **konfigurálása** hello hello developer portálon, és másolja hello AAD-alkalmazást lapján **ügyfél-azonosító**.</span><span class="sxs-lookup"><span data-stu-id="825d2-253">tooget hello **Client Id**, navigate toohello **Configure** tab of hello AAD application for hello developer portal and copy hello **Client Id**.</span></span>

<span data-ttu-id="825d2-254">tooget hello **Ügyfélkulcs** hello kattintson **válassza ki a duration** hello legördülő **kulcsok** szakaszt, és adjon meg egy időközt.</span><span class="sxs-lookup"><span data-stu-id="825d2-254">tooget hello **Client Secret** click hello **Select duration** drop-down in hello **Keys** section and specify an interval.</span></span> <span data-ttu-id="825d2-255">Ebben a példában 1 év szolgál.</span><span class="sxs-lookup"><span data-stu-id="825d2-255">In this example 1 year is used.</span></span>

![Ügyfél-azonosító][api-management-aad-client-id]

<span data-ttu-id="825d2-257">Kattintson a **mentése** toosave hello konfigurációs és megjelenítési hello kulcs.</span><span class="sxs-lookup"><span data-stu-id="825d2-257">Click **Save** toosave hello configuration and display hello key.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="825d2-258">Jegyezze fel ezt a kulcsot.</span><span class="sxs-lookup"><span data-stu-id="825d2-258">Make a note of this key.</span></span> <span data-ttu-id="825d2-259">Hello Azure Active Directory konfigurációs ablak bezárása után hello kulcs nem jeleníthető meg újra.</span><span class="sxs-lookup"><span data-stu-id="825d2-259">Once you close hello Azure Active Directory configuration window, hello key cannot be displayed again.</span></span>
> 
> 

<span data-ttu-id="825d2-260">Másolás hello kulcs toohello vágólap, kapcsoló hátsó toohello publisher portal hello kulcs beillesztése hello **Ügyfélkulcs** szövegmező, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="825d2-260">Copy hello key toohello clipboard, switch back toohello publisher portal, paste hello key into hello **Client Secret** textbox, and click **Save**.</span></span>

![Engedélyezési kiszolgáló hozzáadása][api-management-add-authorization-server-3]

<span data-ttu-id="825d2-262">Azonnal hello ügyfél hitelesítő adatait a következő kód egy hitelesítésengedélyezési.</span><span class="sxs-lookup"><span data-stu-id="825d2-262">Immediately following hello client credentials is an authorization code grant.</span></span> <span data-ttu-id="825d2-263">Az engedélyezési kód másolja, majd kapcsoló hátsó tooyour az Azure AD fejlesztői portál alkalmazás konfigurálása lap, és hello hitelesítésengedélyezési beillesztheti hello **válasz URL-CÍMEN** mezőben, majd kattintson a **mentése** újra.</span><span class="sxs-lookup"><span data-stu-id="825d2-263">Copy this authorization code and switch back tooyour Azure AD developer portal application configure page, and paste hello authorization grant into hello **Reply URL** field, and click **Save** again.</span></span>

![Válasz URL-címe][api-management-aad-reply-url]

<span data-ttu-id="825d2-265">következő lépés hello tooconfigure hello engedélyeinek hello fejlesztői portálján AAD-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="825d2-265">hello next step is tooconfigure hello permissions for hello developer portal AAD application.</span></span> <span data-ttu-id="825d2-266">Kattintson a **Alkalmazásengedélyek** és hello jelölőnégyzetet a **címtáradatok olvasása**.</span><span class="sxs-lookup"><span data-stu-id="825d2-266">Click **Application Permissions** and check hello box for **Read directory data**.</span></span> <span data-ttu-id="825d2-267">Kattintson a **mentése** toosave ez módosítsa, majd kattintson a **alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="825d2-267">Click **Save** toosave this change, and then click **Add application**.</span></span>

![Engedélyek hozzáadása][api-management-add-devportal-permissions]

<span data-ttu-id="825d2-269">Hello keresés ikonra, kattintson típus **APIM** történő hello kezdve mezőben, válassza ki **APIMAADDemo**, és kattintson a pipa jelre toosave hello.</span><span class="sxs-lookup"><span data-stu-id="825d2-269">Click hello search icon, type **APIM** into hello Starting with box, select **APIMAADDemo**, and click hello check mark toosave.</span></span>

![Engedélyek hozzáadása][api-management-aad-add-app-permissions]

<span data-ttu-id="825d2-271">Kattintson a **delegált engedélyek** a **APIMAADDemo** és hello jelölőnégyzetet a **hozzáférés APIMAADDemo**, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="825d2-271">Click **Delegated Permissions** for **APIMAADDemo** and check hello box for **Access APIMAADDemo**, and click **Save**.</span></span> <span data-ttu-id="825d2-272">Ez lehetővé teszi, hogy hello fejlesztői portálalkalmazás tooaccess hello háttérszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="825d2-272">This allows hello developer portal application tooaccess hello backend service.</span></span>

![Engedélyek hozzáadása][api-management-aad-add-delegated-permissions]

## <a name="enable-oauth-20-user-authorization-for-hello-calculator-api"></a><span data-ttu-id="825d2-274">A Számológép API hello OAuth 2.0 felhasználó engedélyezése</span><span class="sxs-lookup"><span data-stu-id="825d2-274">Enable OAuth 2.0 user authorization for hello Calculator API</span></span>
<span data-ttu-id="825d2-275">Most, hogy hello OAuth 2.0-kiszolgálót, adja meg azt a biztonsági beállítások hello az API-t.</span><span class="sxs-lookup"><span data-stu-id="825d2-275">Now that hello OAuth 2.0 server is configured, you can specify it in hello security settings for your API.</span></span> <span data-ttu-id="825d2-276">Ez a lépés mutatják be hello videó kezdő pozíció: 14:30.</span><span class="sxs-lookup"><span data-stu-id="825d2-276">This step is demonstrated in hello video starting at 14:30.</span></span>

<span data-ttu-id="825d2-277">Kattintson a **API-k** a bal oldali menü hello, és kattintson a **Számológép** tooview, és konfigurálja a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="825d2-277">Click **APIs** in hello left menu, and click  **Calculator** tooview and configure its settings.</span></span>

![A Számológép API][api-management-calc-api]

<span data-ttu-id="825d2-279">Keresse meg a toohello **biztonsági** fület, ellenőrizze a hello **OAuth 2.0** jelölőnégyzetet, jelölje be hello kívánt engedélyezési kiszolgáló hello **engedélyezési kiszolgáló** legördülő, kattintson **Mentése**.</span><span class="sxs-lookup"><span data-stu-id="825d2-279">Navigate toohello **Security** tab, check hello **OAuth 2.0** checkbox, select hello desired authorization server from hello **Authorization server** drop-down, and click **Save**.</span></span>

![A Számológép API][api-management-enable-aad-calculator]

## <a name="successfully-call-hello-calculator-api-from-hello-developer-portal"></a><span data-ttu-id="825d2-281">Sikeresen meg tudja hívni hello Számológép API hello developer portálról</span><span class="sxs-lookup"><span data-stu-id="825d2-281">Successfully call hello Calculator API from hello developer portal</span></span>
<span data-ttu-id="825d2-282">Most, hogy hello OAuth 2.0 hitelesítési hello API konfigurálva van, műveletei sikeresen hívhatók hello fejlesztői központból.</span><span class="sxs-lookup"><span data-stu-id="825d2-282">Now that hello OAuth 2.0 authorization is configured on hello API, its operations can be successfully called from hello developer center.</span></span> <span data-ttu-id="825d2-283">Ez a lépés mutatják be hello videó 15:00-tól kezdve.</span><span class="sxs-lookup"><span data-stu-id="825d2-283">THis step is demonstrated in hello video starting at 15:00.</span></span>

<span data-ttu-id="825d2-284">Keresse meg a visszafelé toohello **két egész számok hozzáadása** hello Számológép szolgáltatás hello fejlesztői portálján, majd kattintson a művelet **kipróbálás**.</span><span class="sxs-lookup"><span data-stu-id="825d2-284">Navigate back toohello **Add two integers** operation of hello calculator service in hello developer portal and click **Try it**.</span></span> <span data-ttu-id="825d2-285">Megjegyzés: hello új elem a hello **engedélyezési** szakasz megfelelő toohello engedélyezési kiszolgáló az előzőekben adott hozzá.</span><span class="sxs-lookup"><span data-stu-id="825d2-285">Note hello new item in hello **Authorization** section corresponding toohello authorization server you just added.</span></span>

![A Számológép API][api-management-calc-authorization-server]

<span data-ttu-id="825d2-287">Válassza ki **engedélyezési kód** hello engedély legördülő listában, és adja meg hello fiók toouse hello hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="825d2-287">Select **Authorization code** from hello authorization drop-down list and enter hello credentials of hello account toouse.</span></span> <span data-ttu-id="825d2-288">Ha már nem kérheti hello fiókkal bejelentkezve.</span><span class="sxs-lookup"><span data-stu-id="825d2-288">If you are already signed in with hello account you may not be prompted.</span></span>

![A Számológép API][api-management-devportal-authorization-code]

<span data-ttu-id="825d2-290">Kattintson a **küldése** és megjegyzés hello **válaszállapot** a **200 OK** és hello eredmények hello művelet hello válasz tartalma.</span><span class="sxs-lookup"><span data-stu-id="825d2-290">Click **Send** and note hello **Response status** of **200 OK** and hello results of hello operation in hello response content.</span></span>

![A Számológép API][api-management-devportal-response]

## <a name="configure-a-desktop-application-toocall-hello-api"></a><span data-ttu-id="825d2-292">Egy asztali alkalmazás toocall hello API konfigurálása</span><span class="sxs-lookup"><span data-stu-id="825d2-292">Configure a desktop application toocall hello API</span></span>
<span data-ttu-id="825d2-293">a videó hello hello a következő eljárással kezdődik, 16:30, és egy egyszerű asztali alkalmazás toocall hello API konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="825d2-293">hello next procedure in hello video starts at 16:30 and configures a simple desktop application toocall hello API.</span></span> <span data-ttu-id="825d2-294">hello első lépéseként tooregister hello asztali alkalmazás az Azure ad-ben, és adjon neki toohello könyvtár és toohello háttér szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="825d2-294">hello first step is tooregister hello desktop application in Azure AD and give it access toohello directory and toohello backend service.</span></span> <span data-ttu-id="825d2-295">18:25 nincs bemutatója hello asztali alkalmazás egy műveletet a hello Számológép API hívása.</span><span class="sxs-lookup"><span data-stu-id="825d2-295">At 18:25 there is a demonstration of hello desktop application calling an operation on hello calculator API.</span></span>

## <a name="configure-a-jwt-validation-policy-toopre-authorize-requests"></a><span data-ttu-id="825d2-296">Konfigurálja a JWT-ellenőrzési házirend toopre-kérések engedélyezésére</span><span class="sxs-lookup"><span data-stu-id="825d2-296">Configure a JWT validation policy toopre-authorize requests</span></span>
<span data-ttu-id="825d2-297">hello hello videó az utolsó eljárás 20:48 kezdődik, és bemutatja, hogyan toouse hello [érvényesítése JWT](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) házirend toopre-kérelmek hitelesítéséhez érvényesítésével megjeleníthető az egyes bejövő kérelmek hello hozzáférési jogkivonatok.</span><span class="sxs-lookup"><span data-stu-id="825d2-297">hello final procedure in hello video starts at 20:48 and shows you how toouse hello [Validate JWT](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) policy toopre-authorize requests by validating hello access tokens of each incoming request.</span></span> <span data-ttu-id="825d2-298">Hello kérelem nem érvényesítette hello érvényesítése JWT házirend, ha hello kérelem API Management le van tiltva, és nem kerül át toohello háttér mentén.</span><span class="sxs-lookup"><span data-stu-id="825d2-298">If hello request is not validated by hello Validate JWT policy, hello request is blocked by API Management and is not passed along toohello backend.</span></span>

```xml
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
    <openid-config url="https://login.microsoftonline.com/DemoAPIM.onmicrosoft.com/.well-known/openid-configuration" />
    <required-claims>
        <claim name="aud">
            <value>https://DemoAPIM.NOTonmicrosoft.com/APIMAADDemo</value>
        </claim>
    </required-claims>
</validate-jwt>
```

<span data-ttu-id="825d2-299">Egy másik bemutató konfigurálása, és ez a házirend használatával, lásd: [felhő fedik le a epizód 177: több API-felügyeleti funkciókat](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és too13:50 előre.</span><span class="sxs-lookup"><span data-stu-id="825d2-299">For another demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too13:50.</span></span> <span data-ttu-id="825d2-300">Gyors toosee hello házirendeket a Helyicsoportházirend-szerkesztő hello az too15:00 továbbítja, és majd a művelet hívásának hello developer portálról vagy anélkül hello bemutatója too18:50 szükséges engedélyezési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="825d2-300">Fast forward too15:00 toosee hello policies configured in hello policy editor and then too18:50 for a demonstration of calling an operation from hello developer portal both with and without hello required authorization token.</span></span>

## <a name="next-steps"></a><span data-ttu-id="825d2-301">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="825d2-301">Next steps</span></span>
* <span data-ttu-id="825d2-302">Tekintse meg több [videók](https://azure.microsoft.com/documentation/videos/index/?services=api-management) API-kezeléssel kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="825d2-302">Check out more [videos](https://azure.microsoft.com/documentation/videos/index/?services=api-management) about API Management.</span></span>
* <span data-ttu-id="825d2-303">A más módon toosecure a háttérszolgáltatás, lásd: [kölcsönös tanúsítványhitelesítés](api-management-howto-mutual-certificates.md).</span><span class="sxs-lookup"><span data-stu-id="825d2-303">For other ways toosecure your backend service, see [Mutual Certificate authentication](api-management-howto-mutual-certificates.md).</span></span>

[api-management-management-console]: ./media/api-management-howto-protect-backend-with-aad/api-management-management-console.png

[api-management-import-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-new-api.png
[api-management-create-aad-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad-menu.png
[api-management-create-aad]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad.png
[api-management-new-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-web-app.png
[api-management-new-project]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project.png
[api-management-new-project-cloud]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project-cloud.png
[api-management-change-authentication]: ./media/api-management-howto-protect-backend-with-aad/api-management-change-authentication.png
[api-management-sign-in-vidual-studio]: ./media/api-management-howto-protect-backend-with-aad/api-management-sign-in-vidual-studio.png
[api-management-configure-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-configure-web-app.png
[api-management-aad-domains]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-domains.png
[api-management-add-controller]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-controller.png
[api-management-web-publish]: ./media/api-management-howto-protect-backend-with-aad/api-management-web-publish.png
[api-management-aad-backend-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-backend-app.png
[api-management-aad-add-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-permissions.png
[api-management-developer-portal-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-developer-portal-menu.png
[api-management-dev-portal-apis]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-apis.png
[api-management-dev-portal-try-it]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-try-it.png
[api-management-dev-portal-send-401]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-send-401.png
[api-management-aad-new-application-devportal]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal.png
[api-management-aad-new-application-devportal-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-1.png
[api-management-aad-new-application-devportal-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-2.png
[api-management-aad-devportal-application]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-devportal-application.png
[api-management-add-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server.png
[api-management-aad-sso-uri]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-sso-uri.png
[api-management-aad-view-endpoints]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-view-endpoints.png
[api-management-aad-client-id]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-client-id.png
[api-management-add-authorization-server-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1.png
[api-management-add-authorization-server-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2.png
[api-management-add-authorization-server-2a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2a.png
[api-management-add-authorization-server-3]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-3.png
[api-management-aad-reply-url]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-reply-url.png
[api-management-add-devportal-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-devportal-permissions.png
[api-management-aad-add-app-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-app-permissions.png
[api-management-aad-add-delegated-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-delegated-permissions.png
[api-management-calc-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-api.png
[api-management-enable-aad-calculator]: ./media/api-management-howto-protect-backend-with-aad/api-management-enable-aad-calculator.png
[api-management-devportal-authorization-code]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-authorization-code.png
[api-management-devportal-response]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-response.png
[api-management-calc-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-authorization-server.png
[api-management-add-authorization-server-1a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1a.png
[api-management-client-credentials]: ./media/api-management-howto-protect-backend-with-aad/api-management-client-credentials.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-aad-application-menu.png

[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Manage your first API]: api-management-get-started.md
