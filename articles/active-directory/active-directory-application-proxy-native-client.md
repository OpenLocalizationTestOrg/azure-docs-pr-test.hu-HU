---
title: "Natív ügyfél alkalmazások – az Azure AD közzététele |} Microsoft Docs"
description: "Bemutatja, hogyan adhat kommunikálni az Azure AD alkalmazásproxy-összekötő a helyszíni alkalmazások biztonságos távoli hozzáférést biztosítanak a natív ügyfél alkalmazások engedélyezése."
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
ms.openlocfilehash: bdaa5af6ff5331bc310499586615b48a864c3c5e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a><span data-ttu-id="793c7-103">Hogyan kommunikál a proxy alkalmazások natív ügyfél alkalmazások engedélyezése</span><span class="sxs-lookup"><span data-stu-id="793c7-103">How to enable native client apps to interact with proxy Applications</span></span>

<span data-ttu-id="793c7-104">Mellett a webes alkalmazásokhoz Azure Active Directory Alkalmazásproxyjával használhatók natív ügyfél alkalmazásokat közzétenni.</span><span class="sxs-lookup"><span data-stu-id="793c7-104">In addition to web applications, Azure Active Directory Application Proxy can also be used to publish native client apps.</span></span> <span data-ttu-id="793c7-105">Natív ügyfél alkalmazások eltérnek a webalkalmazások, mert telepítik őket egy eszközt, amíg a böngészőn keresztül elért webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="793c7-105">Native client apps differ from web apps because they are installed on a device, while web apps are accessed through a browser.</span></span> 

<span data-ttu-id="793c7-106">Alkalmazásproxy által kiadott jogkivonatokat, amelyek szabványos engedélyezik HTTP-fejlécek elküldése megtörténjen fogadja el az Azure AD natív ügyfél alkalmazásokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="793c7-106">Application Proxy supports native client apps by accepting Azure AD issued tokens that are sent in standard Authorize HTTP headers.</span></span>

![A végfelhasználók, az Azure Active Directory és a közzétett alkalmazások közötti kapcsolat](./media/active-directory-application-proxy-native-client/richclientflow.png)

<span data-ttu-id="793c7-108">Az Azure AD Authentication Library, amely gondoskodik a hitelesítésről, és támogatja a legtöbb ügyfél környezetben, használja a natív alkalmazások közzétételét.</span><span class="sxs-lookup"><span data-stu-id="793c7-108">Use the Azure AD Authentication Library, which takes care of authentication and supports many client environments, to publish native applications.</span></span> <span data-ttu-id="793c7-109">Alkalmazásproxy hogyan illik bele a [webes API-forgatókönyvet natív alkalmazás](develop/active-directory-authentication-scenarios.md#native-application-to-web-api).</span><span class="sxs-lookup"><span data-stu-id="793c7-109">Application Proxy fits into the [Native Application to Web API scenario](develop/active-directory-authentication-scenarios.md#native-application-to-web-api).</span></span> <span data-ttu-id="793c7-110">Ez a cikk végigvezeti a négy alkalmazásproxy és az Azure AD Authentication Library natív alkalmazás közzététele.</span><span class="sxs-lookup"><span data-stu-id="793c7-110">This article walks you through the four steps to publish a native application with Application Proxy and the Azure AD Authentication Library.</span></span> 

## <a name="step-1-publish-your-application"></a><span data-ttu-id="793c7-111">1. lépés: Az alkalmazás közzétételére.</span><span class="sxs-lookup"><span data-stu-id="793c7-111">Step 1: Publish your application</span></span>
<span data-ttu-id="793c7-112">A proxy alkalmazás közzététele, mint bármilyen más alkalmazást, és rendelje hozzá a felhasználók az alkalmazás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="793c7-112">Publish your proxy application as you would any other application and assign users to access your application.</span></span> <span data-ttu-id="793c7-113">További információkért lásd: [alkalmazások közzétételére az alkalmazásproxy](active-directory-application-proxy-publish.md).</span><span class="sxs-lookup"><span data-stu-id="793c7-113">For more information, see [Publish applications with Application Proxy](active-directory-application-proxy-publish.md).</span></span>

## <a name="step-2-configure-your-application"></a><span data-ttu-id="793c7-114">2. lépés: Az alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="793c7-114">Step 2: Configure your application</span></span>
<span data-ttu-id="793c7-115">A natív alkalmazások konfigurálása az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="793c7-115">Configure your native application as follows:</span></span>

1. <span data-ttu-id="793c7-116">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="793c7-116">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="793c7-117">Navigáljon a **Azure Active Directory** > **App regisztrációk**.</span><span class="sxs-lookup"><span data-stu-id="793c7-117">Navigate to **Azure Active Directory** > **App registrations**.</span></span>
3. <span data-ttu-id="793c7-118">Válassza ki **új alkalmazás regisztrációja**.</span><span class="sxs-lookup"><span data-stu-id="793c7-118">Select **New application registration**.</span></span>
4. <span data-ttu-id="793c7-119">Adjon meg egy nevet az alkalmazáshoz, jelölje be **natív** az alkalmazás-típusként, és adjon meg az alkalmazás átirányítási URI-t.</span><span class="sxs-lookup"><span data-stu-id="793c7-119">Specify a name for your application, select **Native** as the application type, and provide the Redirect URI for your application.</span></span> 

   ![Hozzon létre egy új alkalmazás regisztrálása](./media/active-directory-application-proxy-native-client/create.png)
5. <span data-ttu-id="793c7-121">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="793c7-121">Select **Create**.</span></span>

<span data-ttu-id="793c7-122">Új alkalmazás regisztrációjának létrehozásával kapcsolatos információért, lásd: [alkalmazások integrálása az Azure Active Directory](.//develop/active-directory-integrating-applications.md).</span><span class="sxs-lookup"><span data-stu-id="793c7-122">For more detailed information about creating a new app registration, see [Integrating applications with Azure Active Directory](.//develop/active-directory-integrating-applications.md).</span></span>


## <a name="step-3-grant-access-to-other-applications"></a><span data-ttu-id="793c7-123">3. lépés: A hozzáférés engedélyezése az egyéb alkalmazások</span><span class="sxs-lookup"><span data-stu-id="793c7-123">Step 3: Grant access to other applications</span></span>
<span data-ttu-id="793c7-124">Lehetővé teszik a natív alkalmazások számára elérhetővé tehető, hogy a címtárban lévő más alkalmazásokkal:</span><span class="sxs-lookup"><span data-stu-id="793c7-124">Enable the native application to be exposed to other applications in your directory:</span></span>

1. <span data-ttu-id="793c7-125">Még mindig **App regisztrációk**, válassza ki az imént létrehozott új natív alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="793c7-125">Still in **App registrations**, select the new native application that you just created.</span></span>
2. <span data-ttu-id="793c7-126">Válassza ki **szükséges engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="793c7-126">Select **Required permissions**.</span></span>
3. <span data-ttu-id="793c7-127">Válassza a **Hozzáadás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="793c7-127">Select **Add**.</span></span>
4. <span data-ttu-id="793c7-128">Nyissa meg az első lépés **API kiválasztása**.</span><span class="sxs-lookup"><span data-stu-id="793c7-128">Open the first step, **Select an API**.</span></span>
5. <span data-ttu-id="793c7-129">A keresősáv segítségével megkeresheti az alkalmazásproxy-alkalmazást, az első szakaszban közzétett.</span><span class="sxs-lookup"><span data-stu-id="793c7-129">Use the search bar to find the Application Proxy app that you published in the first section.</span></span> <span data-ttu-id="793c7-130">Válassza ki, hogy az alkalmazást, majd kattintson az **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="793c7-130">Choose that app, then click **Select**.</span></span> 

   ![Keresse meg a proxy alkalmazást](./media/active-directory-application-proxy-native-client/select_api.png)
6. <span data-ttu-id="793c7-132">Nyissa meg a második lépést **engedélyként válassza**.</span><span class="sxs-lookup"><span data-stu-id="793c7-132">Open the second step, **Select permissions**.</span></span>
7. <span data-ttu-id="793c7-133">A natív alkalmazás hozzáférést biztosíthat a proxy-alkalmazást, majd kattintson a jelölőnégyzet segítségével **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="793c7-133">Use the checkbox to grant your native application access to your proxy application, then click **Select**.</span></span>

   ![Hozzáférést biztosít proxy-alkalmazás](./media/active-directory-application-proxy-native-client/select_perms.png)
8. <span data-ttu-id="793c7-135">Válassza ki **végzett**.</span><span class="sxs-lookup"><span data-stu-id="793c7-135">Select **Done**.</span></span>


## <a name="step-4-edit-the-active-directory-authentication-library"></a><span data-ttu-id="793c7-136">4. lépés: Az Active Directory hitelesítési tár szerkesztése</span><span class="sxs-lookup"><span data-stu-id="793c7-136">Step 4: Edit the Active Directory Authentication Library</span></span>
<span data-ttu-id="793c7-137">A hitelesítési környezetet, az Active Directory Authentication Library (ADAL) az alábbi szöveget a natív alkalmazás kód szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="793c7-137">Edit the native application code in the authentication context of the Active Directory Authentication Library (ADAL) to include the following text:</span></span>

```
// Acquire Access Token from AAD for Proxy Application
AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<Tenant ID>");
AuthenticationResult result = authContext.AcquireToken("< External Url of Proxy App >",
        "<App ID of the Native app>",
        new Uri("<Redirect Uri of the Native App>"),
        PromptBehavior.Never);

//Use the Access Token to access the Proxy Application
HttpClient httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");
```

<span data-ttu-id="793c7-138">A változók példakód ki kell cserélni az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="793c7-138">The variables in the sample code should be replaced as follows:</span></span>

* <span data-ttu-id="793c7-139">**A bérlői azonosító** az Azure portálon található.</span><span class="sxs-lookup"><span data-stu-id="793c7-139">**Tenant ID** can be found in the Azure portal.</span></span> <span data-ttu-id="793c7-140">Navigáljon a **Azure Active Directory** > **tulajdonságok** , és másolja a könyvtár-azonosító.</span><span class="sxs-lookup"><span data-stu-id="793c7-140">Navigate to **Azure Active Directory** > **Properties** and copy the Directory ID.</span></span> 
* <span data-ttu-id="793c7-141">**Külső URL-cím** a Proxy alkalmazása a megadott előtér URL-címét.</span><span class="sxs-lookup"><span data-stu-id="793c7-141">**External URL** is the front-end URL you entered in the Proxy Application.</span></span> <span data-ttu-id="793c7-142">Ez az érték megkereséséhez nyissa meg a **alkalmazásproxy** a proxy app szakasza.</span><span class="sxs-lookup"><span data-stu-id="793c7-142">To find this value, navigate to the **Application proxy** section of the proxy app.</span></span>
* <span data-ttu-id="793c7-143">**Alkalmazásazonosító** a natív alkalmazás megtalálható a **tulajdonságok** a natív alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="793c7-143">**App ID** of the native app can be found on the **Properties** page of the native application.</span></span>
* <span data-ttu-id="793c7-144">**Átirányítási URI-címe a natív alkalmazás** található meg a **átirányítási URI-azonosítók** a natív alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="793c7-144">**Redirect URI of the native app** can be found on the **Redirect URIs** page of the native application.</span></span>


## <a name="see-also"></a><span data-ttu-id="793c7-145">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="793c7-145">See also</span></span>

<span data-ttu-id="793c7-146">A natív alkalmazás folyamattal kapcsolatos további információkért lásd: [natív alkalmazás webes API-hoz](develop/active-directory-authentication-scenarios.md#native-application-to-web-api)</span><span class="sxs-lookup"><span data-stu-id="793c7-146">For more information about the native application flow, see [Native application to web API](develop/active-directory-authentication-scenarios.md#native-application-to-web-api)</span></span>

<span data-ttu-id="793c7-147">A legújabb híreket és frissítéseket itt találja: [Alkalmazásproxy blog](http://blogs.technet.com/b/applicationproxyblog/).</span><span class="sxs-lookup"><span data-stu-id="793c7-147">For the latest news and updates, check out the [Application Proxy blog](http://blogs.technet.com/b/applicationproxyblog/)</span></span>
