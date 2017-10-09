---
title: "natív ügyfél aaaPublish - alkalmazások az Azure AD |} Microsoft Docs"
description: "Ismerteti, hogyan tooenable natív ügyfél alkalmazások toocommunicate az Azure AD alkalmazásproxy-összekötő tooprovide biztonságos távoli hozzáférés tooyour a helyszíni alkalmazások."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: f0cae145-e346-4126-948f-3f699747b96e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 0ed2be217bf992f034d8321d5e66569b4cace24f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooenable-native-client-apps-toointeract-with-proxy-applications"></a><span data-ttu-id="c5df7-103">Hogyan tooenable natív ügyfél alkalmazások toointeract proxyval alkalmazások</span><span class="sxs-lookup"><span data-stu-id="c5df7-103">How tooenable native client apps toointeract with proxy Applications</span></span>

<span data-ttu-id="c5df7-104">Továbbá tooweb alkalmazásokban Azure Active Directory Alkalmazásproxyjával is használt toopublish natív ügyfél alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="c5df7-104">In addition tooweb applications, Azure Active Directory Application Proxy can also be used toopublish native client apps.</span></span> <span data-ttu-id="c5df7-105">Natív ügyfél alkalmazások eltérnek a webalkalmazások, mert telepítik őket egy eszközt, amíg a böngészőn keresztül elért webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="c5df7-105">Native client apps differ from web apps because they are installed on a device, while web apps are accessed through a browser.</span></span> 

<span data-ttu-id="c5df7-106">Alkalmazásproxy által kiadott jogkivonatokat, amelyek szabványos engedélyezik HTTP-fejlécek elküldése megtörténjen fogadja el az Azure AD natív ügyfél alkalmazásokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="c5df7-106">Application Proxy supports native client apps by accepting Azure AD issued tokens that are sent in standard Authorize HTTP headers.</span></span>

![A végfelhasználók, az Azure Active Directory és a közzétett alkalmazások közötti kapcsolat](./media/active-directory-application-proxy-native-client/richclientflow.png)

<span data-ttu-id="c5df7-108">Azure AD Authentication Library, amely gondoskodik a hitelesítésről és sok ügyfél környezetek és natív alkalmazások toopublish hello használata.</span><span class="sxs-lookup"><span data-stu-id="c5df7-108">Use hello Azure AD Authentication Library, which takes care of authentication and supports many client environments, toopublish native applications.</span></span> <span data-ttu-id="c5df7-109">Alkalmazásproxy illeszkedik hello [natív alkalmazás tooWeb API forgatókönyv](develop/active-directory-authentication-scenarios.md#native-application-to-web-api).</span><span class="sxs-lookup"><span data-stu-id="c5df7-109">Application Proxy fits into hello [Native Application tooWeb API scenario](develop/active-directory-authentication-scenarios.md#native-application-to-web-api).</span></span> <span data-ttu-id="c5df7-110">Ez a cikk bemutatja, hogyan hello négy lépést toopublish alkalmazásproxy és hello Azure AD Authentication Library egy natív alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c5df7-110">This article walks you through hello four steps toopublish a native application with Application Proxy and hello Azure AD Authentication Library.</span></span> 

## <a name="step-1-publish-your-application"></a><span data-ttu-id="c5df7-111">1. lépés: Az alkalmazás közzétételére.</span><span class="sxs-lookup"><span data-stu-id="c5df7-111">Step 1: Publish your application</span></span>
<span data-ttu-id="c5df7-112">A proxy alkalmazás közzététele, mint bármilyen más alkalmazást, és rendelje hozzá a felhasználók tooaccess az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c5df7-112">Publish your proxy application as you would any other application and assign users tooaccess your application.</span></span> <span data-ttu-id="c5df7-113">További információkért lásd: [alkalmazások közzétételére az alkalmazásproxy](active-directory-application-proxy-publish.md).</span><span class="sxs-lookup"><span data-stu-id="c5df7-113">For more information, see [Publish applications with Application Proxy](active-directory-application-proxy-publish.md).</span></span>

## <a name="step-2-configure-your-application"></a><span data-ttu-id="c5df7-114">2. lépés: Az alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c5df7-114">Step 2: Configure your application</span></span>
<span data-ttu-id="c5df7-115">A natív alkalmazások konfigurálása az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c5df7-115">Configure your native application as follows:</span></span>

1. <span data-ttu-id="c5df7-116">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c5df7-116">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="c5df7-117">Keresse meg a túl**Azure Active Directory** > **App regisztrációk**.</span><span class="sxs-lookup"><span data-stu-id="c5df7-117">Navigate too**Azure Active Directory** > **App registrations**.</span></span>
3. <span data-ttu-id="c5df7-118">Válassza ki **új alkalmazás regisztrációja**.</span><span class="sxs-lookup"><span data-stu-id="c5df7-118">Select **New application registration**.</span></span>
4. <span data-ttu-id="c5df7-119">Adjon meg egy nevet az alkalmazásnak, válassza **natív** hello alkalmazás típusa, és adja meg a hello átirányítási URI-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c5df7-119">Specify a name for your application, select **Native** as hello application type, and provide hello Redirect URI for your application.</span></span> 

   ![Hozzon létre egy új alkalmazás regisztrálása](./media/active-directory-application-proxy-native-client/create.png)
5. <span data-ttu-id="c5df7-121">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c5df7-121">Select **Create**.</span></span>

<span data-ttu-id="c5df7-122">Új alkalmazás regisztrációjának létrehozásával kapcsolatos információért, lásd: [alkalmazások integrálása az Azure Active Directory](.//develop/active-directory-integrating-applications.md).</span><span class="sxs-lookup"><span data-stu-id="c5df7-122">For more detailed information about creating a new app registration, see [Integrating applications with Azure Active Directory](.//develop/active-directory-integrating-applications.md).</span></span>


## <a name="step-3-grant-access-tooother-applications"></a><span data-ttu-id="c5df7-123">3. lépés: Grant tooother alkalmazásokat</span><span class="sxs-lookup"><span data-stu-id="c5df7-123">Step 3: Grant access tooother applications</span></span>
<span data-ttu-id="c5df7-124">Hello natív alkalmazás kitett toobe tooother alkalmazások a címtárban engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="c5df7-124">Enable hello native application toobe exposed tooother applications in your directory:</span></span>

1. <span data-ttu-id="c5df7-125">Még mindig **App regisztrációk**, válassza ki az imént létrehozott új natív alkalmazás hello.</span><span class="sxs-lookup"><span data-stu-id="c5df7-125">Still in **App registrations**, select hello new native application that you just created.</span></span>
2. <span data-ttu-id="c5df7-126">Válassza ki **szükséges engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="c5df7-126">Select **Required permissions**.</span></span>
3. <span data-ttu-id="c5df7-127">Válassza a **Hozzáadás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="c5df7-127">Select **Add**.</span></span>
4. <span data-ttu-id="c5df7-128">Nyissa meg hello első lépésként **API kiválasztása**.</span><span class="sxs-lookup"><span data-stu-id="c5df7-128">Open hello first step, **Select an API**.</span></span>
5. <span data-ttu-id="c5df7-129">Hello keresési sáv toofind hello alkalmazásproxy alkalmazás használata közzétett hello első szakaszában.</span><span class="sxs-lookup"><span data-stu-id="c5df7-129">Use hello search bar toofind hello Application Proxy app that you published in hello first section.</span></span> <span data-ttu-id="c5df7-130">Válassza ki, hogy az alkalmazást, majd kattintson az **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="c5df7-130">Choose that app, then click **Select**.</span></span> 

   ![Keresse meg hello proxy alkalmazást](./media/active-directory-application-proxy-native-client/select_api.png)
6. <span data-ttu-id="c5df7-132">Nyissa meg hello a második lépésben **engedélyként válassza**.</span><span class="sxs-lookup"><span data-stu-id="c5df7-132">Open hello second step, **Select permissions**.</span></span>
7. <span data-ttu-id="c5df7-133">Használjon hello jelölőnégyzet toogrant a natív alkalmazás access tooyour proxy alkalmazást, majd kattintson a **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="c5df7-133">Use hello checkbox toogrant your native application access tooyour proxy application, then click **Select**.</span></span>

   ![Adjon hozzáférést tooproxy alkalmazás](./media/active-directory-application-proxy-native-client/select_perms.png)
8. <span data-ttu-id="c5df7-135">Válassza ki **végzett**.</span><span class="sxs-lookup"><span data-stu-id="c5df7-135">Select **Done**.</span></span>


## <a name="step-4-edit-hello-active-directory-authentication-library"></a><span data-ttu-id="c5df7-136">4. lépés: Az Active Directory Authentication Library hello szerkesztése</span><span class="sxs-lookup"><span data-stu-id="c5df7-136">Step 4: Edit hello Active Directory Authentication Library</span></span>
<span data-ttu-id="c5df7-137">Hello natív alkalmazáskód hello hitelesítési környezetében hello Active Directory Authentication Library (ADAL) tooinclude hello a következő szöveg szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="c5df7-137">Edit hello native application code in hello authentication context of hello Active Directory Authentication Library (ADAL) tooinclude hello following text:</span></span>

```
// Acquire Access Token from AAD for Proxy Application
AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<Tenant ID>");
AuthenticationResult result = authContext.AcquireToken("< External Url of Proxy App >",
        "<App ID of hello Native app>",
        new Uri("<Redirect Uri of hello Native App>"),
        PromptBehavior.Never);

//Use hello Access Token tooaccess hello Proxy Application
HttpClient httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");
```

<span data-ttu-id="c5df7-138">hello mintakód hello változók ki kell cserélni az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c5df7-138">hello variables in hello sample code should be replaced as follows:</span></span>

* <span data-ttu-id="c5df7-139">**A bérlői azonosító** hello Azure-portálon található.</span><span class="sxs-lookup"><span data-stu-id="c5df7-139">**Tenant ID** can be found in hello Azure portal.</span></span> <span data-ttu-id="c5df7-140">Keresse meg a túl**Azure Active Directory** > **tulajdonságok** és másolási hello Directory azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="c5df7-140">Navigate too**Azure Active Directory** > **Properties** and copy hello Directory ID.</span></span> 
* <span data-ttu-id="c5df7-141">**Külső URL-cím** hello előtér URL-cím megadott hello Proxy alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c5df7-141">**External URL** is hello front-end URL you entered in hello Proxy Application.</span></span> <span data-ttu-id="c5df7-142">toofind Ez érték, keresse meg a toohello **alkalmazásproxy** hello proxy app szakasza.</span><span class="sxs-lookup"><span data-stu-id="c5df7-142">toofind this value, navigate toohello **Application proxy** section of hello proxy app.</span></span>
* <span data-ttu-id="c5df7-143">**Alkalmazásazonosító** hello a natív alkalmazással megtalálja az hello **tulajdonságok** hello natív alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="c5df7-143">**App ID** of hello native app can be found on hello **Properties** page of hello native application.</span></span>
* <span data-ttu-id="c5df7-144">**Átirányítási URI hello natív alkalmazás** hello található **átirányítási URI-azonosítók** hello natív alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="c5df7-144">**Redirect URI of hello native app** can be found on hello **Redirect URIs** page of hello native application.</span></span>


## <a name="see-also"></a><span data-ttu-id="c5df7-145">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="c5df7-145">See also</span></span>

<span data-ttu-id="c5df7-146">Hello natív alkalmazási folyamatot kapcsolatos további információkért lásd: [natív alkalmazás tooweb API](develop/active-directory-authentication-scenarios.md#native-application-to-web-api)</span><span class="sxs-lookup"><span data-stu-id="c5df7-146">For more information about hello native application flow, see [Native application tooweb API](develop/active-directory-authentication-scenarios.md#native-application-to-web-api)</span></span>

<span data-ttu-id="c5df7-147">Hello legfrissebb híreket és frissítéseket, tekintse meg a hello [alkalmazásproxy blog](http://blogs.technet.com/b/applicationproxyblog/)</span><span class="sxs-lookup"><span data-stu-id="c5df7-147">For hello latest news and updates, check out hello [Application Proxy blog](http://blogs.technet.com/b/applicationproxyblog/)</span></span>
