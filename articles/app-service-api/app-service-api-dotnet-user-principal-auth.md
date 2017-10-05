---
title: "Az Azure App Service API-alkalmazások felhasználói hitelesítésének |} Microsoft Docs"
description: "Ismerje meg, hogyan védi az Azure App Service API-alkalmazás hozzáférés csak a hitelesített felhasználókhoz."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 3896760d-46ff-4b67-b98a-edd233f24758
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 06/30/2016
ms.author: alkarche
ms.openlocfilehash: a5b713f3a60b3886d813438496d704f464d914e6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="user-authentication-for-api-apps-in-azure-app-service"></a><span data-ttu-id="1a4fb-103">Az Azure App Service API-alkalmazások felhasználói hitelesítésének</span><span class="sxs-lookup"><span data-stu-id="1a4fb-103">User authentication for API Apps in Azure App Service</span></span>
## <a name="overview"></a><span data-ttu-id="1a4fb-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="1a4fb-104">Overview</span></span>
<span data-ttu-id="1a4fb-105">Ez a cikk bemutatja, hogyan védi az Azure API-alkalmazások, hogy csak a hitelesített felhasználók is hívják.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-105">This article shows how to protect an Azure API app so that only authenticated users can call it.</span></span> <span data-ttu-id="1a4fb-106">A cikk feltételezi, hogy elolvasta a [Azure App Service hitelesítés áttekintése](../app-service/app-service-authentication-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1a4fb-106">The article assumes that you have read the [Azure App Service authentication overview](../app-service/app-service-authentication-overview.md).</span></span>

<span data-ttu-id="1a4fb-107">Az oktatóanyagból a következőket sajátíthatja el:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-107">You'll learn:</span></span>

* <span data-ttu-id="1a4fb-108">Hogyan konfigurálható egy hitelesítésszolgáltatót, az Azure Active Directory (Azure AD) adatokkal.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-108">How to configure an authentication provider, with details for Azure Active Directory (Azure AD).</span></span>
* <span data-ttu-id="1a4fb-109">Hogyan használják a védett API-alkalmazások a [Active Directory Authentication Library (ADAL) a JavaScript](https://github.com/AzureAD/azure-activedirectory-library-for-js).</span><span class="sxs-lookup"><span data-stu-id="1a4fb-109">How to consume a protected API app by using the [Active Directory Authentication Library (ADAL) for JavaScript](https://github.com/AzureAD/azure-activedirectory-library-for-js).</span></span>

<span data-ttu-id="1a4fb-110">A cikk két részből áll:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-110">The article contains two sections:</span></span>

* <span data-ttu-id="1a4fb-111">A [felhasználói hitelesítés beállítása az Azure App Service](#authconfig) szakasz általánosságban ismerteti bármely API-alkalmazások felhasználói hitelesítésének konfigurálása, és minden keretrendszerek App Service által támogatott keretrendszerre alkalmazható többek között a .NET, Node.js, és Java.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-111">The [How to configure user authentication in Azure App Service](#authconfig) section explains in general how to configure user authentication for any API app and applies equally to all frameworks supported by App Service, including .NET, Node.js, and Java.</span></span>
* <span data-ttu-id="1a4fb-112">Kezdve a [a .NET API-alkalmazások oktatóanyagok folytatása](#tutorialstart) szakaszban, a cikk végigvezeti a felhasználót, hogy az Azure Active Directory felhasználó számára a .NET háttérből és egy AngularJS előtér mintaalkalmazás konfigurálása hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-112">Starting with the [Continuing the .NET API Apps tutorials](#tutorialstart) section, the article guides you through configuring a sample application with a .NET back end and an AngularJS front end so that it uses Azure Active Directory for user authentication.</span></span> 

## <span data-ttu-id="1a4fb-113"><a id="authconfig"></a>Felhasználói hitelesítés beállítása az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="1a4fb-113"><a id="authconfig"></a> How to configure user authentication in Azure App Service</span></span>
<span data-ttu-id="1a4fb-114">Ez a témakör általános API-alkalmazásra vonatkozó utasításokat.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-114">This section provides general instructions that apply to any API app.</span></span> <span data-ttu-id="1a4fb-115">A lépések az adott való tegye lista .NET mintaalkalmazás, lásd [a .NET-bevezető oktatóanyagok folytatása](#tutorialstart).</span><span class="sxs-lookup"><span data-stu-id="1a4fb-115">For steps specific to the To Do List .NET sample application, go to [Continuing the .NET getting-started tutorials](#tutorialstart).</span></span>

1. <span data-ttu-id="1a4fb-116">Az a [Azure-portálon](https://portal.azure.com/), keresse meg a **beállítások** panelen az API-alkalmazás, amelyet szeretne védeni, keresse meg a **szolgáltatások** szakaszt, és kattintson a  **Hitelesítési / engedélyezési**.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-116">In the [Azure portal](https://portal.azure.com/), navigate to the **Settings** blade of the API app that you want to protect, find the **Features** section, and then click **Authentication/ Authorization**.</span></span>
   
    ![Azure-portálon hitelesítési/engedélyezési](./media/app-service-api-dotnet-user-principal-auth/features.png)
2. <span data-ttu-id="1a4fb-118">Az a **hitelesítési / engedélyezési** panelen kattintson a **a**.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-118">In the **Authentication / Authorization** blade, click **On**.</span></span>
3. <span data-ttu-id="1a4fb-119">A beállítások valamelyikét kell választani a **hitelesítetlen kérés esetén elvégzendő művelet** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-119">Select one of the options from the **Action to take when request is not authenticated** drop-down list.</span></span>
   
   * <span data-ttu-id="1a4fb-120">Ha azt szeretné, hogy csak hitelesített hívásokat az API-alkalmazás eléréséhez, válasszon egyet az a **jelentkezzen be az...**  beállítások.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-120">If you want only authenticated calls to reach your API app, choose one of the **Log in with ...** options.</span></span> <span data-ttu-id="1a4fb-121">Ez a beállítás lehetővé teszi az API-alkalmazások védelmét az azt futtató programozás nélkül.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-121">This option enables you to protect the API app without writing any code that runs in it.</span></span>
   * <span data-ttu-id="1a4fb-122">Ha azt szeretné elérni az API-alkalmazás összes hívások, **kérés engedélyezése (intézkedés)**.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-122">If you want all calls to reach your API app, choose **Allow request (no action)**.</span></span> <span data-ttu-id="1a4fb-123">Ez a beállítás segítségével egy kiválasztott hitelesítésszolgáltatók nem hitelesített hívóknak közvetlen.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-123">You can use this option to direct unauthenticated callers to a choice of authentication providers.</span></span> <span data-ttu-id="1a4fb-124">Ezzel a beállítással rendelkezik írhat kódot az engedélyezés kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-124">With this option you have to write code to handle authorization.</span></span>
     
     <span data-ttu-id="1a4fb-125">További információ: [Az API Apps hitelesítése és engedélyezése az Azure App Service-ben](app-service-api-authentication.md#multiple-protection-options).</span><span class="sxs-lookup"><span data-stu-id="1a4fb-125">For more information, see [Authentication and authorization for API Apps in Azure App Service](app-service-api-authentication.md#multiple-protection-options).</span></span>
4. <span data-ttu-id="1a4fb-126">Válasszon ki egy vagy több a **hitelesítésszolgáltatókat**.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-126">Select one or more of the **Authentication Providers**.</span></span>
   
    <span data-ttu-id="1a4fb-127">A képen látható összes hívó számára az Azure AD hitelesíti igénylő lehetőségek.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-127">The image shows    choices that require all callers to be authenticated by Azure AD.</span></span>
   
    ![Az Azure portál hitelesítési/engedélyezési panel](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
   
    <span data-ttu-id="1a4fb-129">Ha úgy dönt, hogy egy hitelesítésszolgáltató, a portál megjeleníti a konfigurációs panel az adott szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-129">When you choose an authentication provider, the portal displays a configuration blade for that provider.</span></span> 
   
    <span data-ttu-id="1a4fb-130">Azt ismertetik, hogyan konfigurálhatja a hitelesítési szolgáltató konfigurációs paneleken részletes útmutatás: [konfigurálása az App Service alkalmazás használhatja az Azure Active Directory bejelentkezési](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="1a4fb-130">For detailed instructions that explain how to configure the authentication provider configuration blades, see [How to configure your App Service application to use Azure Active Directory login](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).</span></span> <span data-ttu-id="1a4fb-131">(A hivatkozás az Azure AD ismertető cikkben ugrik, de magát a cikket, amelyek cikkekhez tartozó más hitelesítésszolgáltatók lapot tartalmaz,.)</span><span class="sxs-lookup"><span data-stu-id="1a4fb-131">(The link goes to an article about Azure AD, but the article itself contains tabs that link to articles for the other authentication providers.)</span></span>
5. <span data-ttu-id="1a4fb-132">Ha a hitelesítési szolgáltató konfigurációs panelen végzett, kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-132">When you're done with the authentication provider configuration blade, click **OK**.</span></span>
6. <span data-ttu-id="1a4fb-133">Az a **hitelesítési / engedélyezési** panelen kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-133">In the **Authentication / Authorization** blade, click **Save**.</span></span>

<span data-ttu-id="1a4fb-134">Ebben az esetben, ha az App Service API-hívások hitelesíti, az API-alkalmazás elérése előtti.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-134">When this is done, App Service authenticates all API calls before they reach the API app.</span></span> <span data-ttu-id="1a4fb-135">A hitelesítési szolgáltatások azonos az összes, az App service támogat, beleértve a .NET, Node.js és Java nyelven működik.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-135">The authentication services work the same for all languages that App service supports, including .NET, Node.js, and Java.</span></span> 

<span data-ttu-id="1a4fb-136">Ahhoz, hogy a hitelesített API-hívások, a hívó tartalmazza a hitelesítésszolgáltató OAuth 2.0 tulajdonosi jogkivonattal a HTTP-kérések hitelesítési fejlécéhez.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-136">To make authenticated API calls, the caller includes the authentication provider's OAuth 2.0 bearer token in the Authorization header of HTTP requests.</span></span> <span data-ttu-id="1a4fb-137">A token szerezhető: a hitelesítésszolgáltató SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-137">The token can be acquired by using the authentication provider's SDK.</span></span>

## <span data-ttu-id="1a4fb-138"><a id="tutorialstart"></a>A .NET API-alkalmazások oktatóanyagok folytatása</span><span class="sxs-lookup"><span data-stu-id="1a4fb-138"><a id="tutorialstart"></a> Continuing the .NET API Apps tutorials</span></span>
<span data-ttu-id="1a4fb-139">Ha a Node.js vagy Java API-alkalmazások oktatóanyag, ugorjon a következő cikk [szolgáltatás egyszerű hitelesítési API-alkalmazások](app-service-api-dotnet-service-principal-auth.md).</span><span class="sxs-lookup"><span data-stu-id="1a4fb-139">If you are following the Node.js or Java tutorials for API apps, skip to the next article, [service principal authentication for API apps](app-service-api-dotnet-service-principal-auth.md).</span></span> 

<span data-ttu-id="1a4fb-140">Ha a .NET API-alkalmazások oktatóanyag adatsorozat követi, és már telepítették a mintaalkalmazás megfelelően a [első](app-service-api-dotnet-get-started.md) és [második](app-service-api-cors-consume-javascript.md) oktatóanyagokat, ugorjon a [beállítása az App Service és az Azure AD hitelesítési](#azureauth) szakasz.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-140">If you are following the .NET tutorial series for API apps and have already deployed the sample application as directed in the [first](app-service-api-dotnet-get-started.md) and [second](app-service-api-cors-consume-javascript.md) tutorials, skip to the [Set up authentication in App Service and Azure AD](#azureauth) section.</span></span>

<span data-ttu-id="1a4fb-141">Ha azt szeretné, hogy ez az oktatóanyag az első és második oktatóanyagok áthaladás nélkül, hajtsa végre az alábbi lépéseket, melyek azt ismertetik, hogyan lásson a mintaalkalmazás telepítése egy automatikus folyamat segítségével.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-141">If you want to follow this tutorial without going through the first and second tutorials, do the following steps which explain how to get started by using an automated process to deploy the sample application.</span></span>

> [!NOTE]
> <span data-ttu-id="1a4fb-142">Az alábbi lépéseket megkeresheti az azonos kiindulási pont, mintha csak az első két oktatóanyagok, egy kivétellel tette: Visual Studio már nem tudja, melyik webalkalmazás vagy API-alkalmazás, amelynek minden projekt telepítése lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-142">The following steps get you to the same starting point as if you did the first two tutorials, with one exception: Visual Studio won't already know which web app or API app that each project gets deployed to.</span></span> <span data-ttu-id="1a4fb-143">Ez azt jelenti, hogy emiatt nem tud pontos útmutatásokat ebben az oktatóanyagban a megfelelő tárolók üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-143">That means you won't have exact instructions in this tutorial that explain how to deploy to the right targets.</span></span> <span data-ttu-id="1a4fb-144">Ha nem tudja az üzembe helyezés lépései alapján saját módjáról, a Feladatkezelő, érdemes az oktatóanyag adatsorozat kövesse a [első oktatóanyaga, amely](app-service-api-dotnet-get-started.md) mint kezdődnie Ez automatikus központi telepítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-144">If you're not comfortable with figuring out how to do the deployment steps on your own, it's better to follow the tutorial series from the [first tutorial](app-service-api-dotnet-get-started.md) than to start with this automated deployment process.</span></span>
> 
> 

1. <span data-ttu-id="1a4fb-145">Győződjön meg arról, hogy rendelkezik az összes felsorolt Előfeltételek a [első oktatóanyaga, amely](app-service-api-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="1a4fb-145">Make sure that you have all of the prerequisites listed in the [first tutorial](app-service-api-dotnet-get-started.md).</span></span> <span data-ttu-id="1a4fb-146">Mellett az előfeltételeket a hitelesítési oktatóanyagok azt feltételezik, hogy dolgozott App Service web apps és az API apps a Visual Studio és az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-146">In addition to the prerequisites listed, these authentication tutorials assume that you have worked with App Service web apps and API apps in Visual Studio and the Azure portal.</span></span>
2. <span data-ttu-id="1a4fb-147">Kattintson a **az Azure telepítéséhez** gombra a [To Do List minta tárház információs fájl](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/readme.md) az API apps és a webes alkalmazás központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-147">Click the **Deploy to Azure** button in the [To Do List sample repository readme file](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/readme.md) to deploy the API apps and the web app.</span></span> <span data-ttu-id="1a4fb-148">Jegyezze fel az Azure erőforráscsoport, amely lekérdezi létrehozni, mert ezzel újabb ellenőrizzék a web app és az API-alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-148">Make a note of the Azure resource group that gets created, as you can use this later to look up web app and API app names.</span></span>
3. <span data-ttu-id="1a4fb-149">Töltse le, vagy klónozza a [To Do List minta tárház](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) a kódot, amely meg fog dolgozni helyileg a Visual Studio segítségével.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-149">Download or clone the [To Do List sample repository](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) to get the code that you'll work with locally in Visual Studio.</span></span>

## <span data-ttu-id="1a4fb-150"><a id="azureauth"></a>Az App Service és az Azure AD hitelesítési beállítása</span><span class="sxs-lookup"><span data-stu-id="1a4fb-150"><a id="azureauth"></a> Set up authentication in App Service and Azure AD</span></span>
<span data-ttu-id="1a4fb-151">Most már rendelkezik anélkül, hogy a felhasználók hitelesítése az Azure App Service-ben futó alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-151">You now have the application running in Azure App Service without requiring that users be authenticated.</span></span> <span data-ttu-id="1a4fb-152">Ez a szakasz a hitelesítés a következő feladatok végrehajtásával hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-152">In this section you add authentication by doing the following tasks:</span></span>

* <span data-ttu-id="1a4fb-153">Azure Active Directory (Azure AD-) hitelesítés megkövetelése hívásakor a középső rétegbeli API-alkalmazást az App Service konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-153">Configure App Service to require Azure Active Directory (Azure AD) authentication for calling the middle tier API app.</span></span>
* <span data-ttu-id="1a4fb-154">Hozzon létre egy Azure AD-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-154">Create an Azure AD application.</span></span>
* <span data-ttu-id="1a4fb-155">Az Azure AD alkalmazás bejelentkezést követően a tulajdonosi jogkivonattal küldeni az AngularJS előtér beállítása.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-155">Configure the Azure AD application to send the bearer token after logon to the AngularJS front end.</span></span> 

<span data-ttu-id="1a4fb-156">Ha problémát tapasztal az oktatóanyag utasításait követve közben, olvassa el a [hibaelhárítás](#troubleshooting) szakasz az oktatóanyag végén.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-156">If you run into problems while following the tutorial directions, see the [Troubleshooting](#troubleshooting) section at the end of the tutorial.</span></span> 

### <a name="configure-authentication-for-the-middle-tier-api-app"></a><span data-ttu-id="1a4fb-157">A középső réteg API-alkalmazás-hitelesítés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1a4fb-157">Configure authentication for the middle tier API app</span></span>
1. <span data-ttu-id="1a4fb-158">Az a [Azure-portálon](https://portal.azure.com/), keresse meg a **beállítások** a ToDoListAPI projekt létrehozott API-alkalmazás paneljén található a **szolgáltatások** szakaszt, és kattintson a  **Hitelesítési / engedélyezési**.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-158">In the [Azure portal](https://portal.azure.com/), navigate to the **Settings** blade of the API app that you created for the ToDoListAPI project, find the **Features** section, and then click **Authentication / Authorization**.</span></span>
   
    ![Azure-portálon hitelesítési/engedélyezési](./media/app-service-api-dotnet-user-principal-auth/features.png)
2. <span data-ttu-id="1a4fb-160">Az a **hitelesítési / engedélyezési** panelen kattintson a **a**.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-160">In the **Authentication / Authorization** blade, click **On**.</span></span>
3. <span data-ttu-id="1a4fb-161">Az a **hitelesítetlen kérés esetén elvégzendő művelet** legördülő listában válassza **bejelentkezés Azure Active Directoryval**.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-161">In the **Action to take when request is not authenticated** drop-down list, select **Log in with Azure Active Directory**.</span></span>
   
    <span data-ttu-id="1a4fb-162">Ez a beállítás biztosítja, hogy unauathenticated kérelmek nem elérje az API-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-162">This option ensures that no unauathenticated requests will reach the API app.</span></span> 
4. <span data-ttu-id="1a4fb-163">A **hitelesítésszolgáltatókat**, kattintson a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-163">Under **Authentication Providers**, click **Azure Active Directory**.</span></span>
   
    ![Az Azure portál hitelesítési/engedélyezési panel](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
5. <span data-ttu-id="1a4fb-165">Az a **Azure Active Directory beállításai** paneljén kattintson **Express**</span><span class="sxs-lookup"><span data-stu-id="1a4fb-165">In the **Azure Active Directory Settings** blade, click **Express**</span></span>
   
    ![Az Azure portál hitelesítési/engedélyezési Express beállítás](./media/app-service-api-dotnet-user-principal-auth/aadsettings.png)
   
    <span data-ttu-id="1a4fb-167">Az a **Express** beállítás, az App Service automatikusan létrehozhat egy Azure AD-alkalmazást az Azure ad [bérlői](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant).</span><span class="sxs-lookup"><span data-stu-id="1a4fb-167">With the **Express** option, App Service can automatically create an Azure AD application in your Azure AD [tenant](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant).</span></span> 
   
    <span data-ttu-id="1a4fb-168">Nem rendelkezik a bérlő létrehozására, mert minden Azure-fiók automatikusan van egy.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-168">You don't have to create a tenant, because every Azure account automatically has one.</span></span>
6. <span data-ttu-id="1a4fb-169">A **üzemmód**, kattintson a **új AD-alkalmazás létrehozása** Ha nincs kiválasztva, és figyelje meg az értéket, amely a a **alkalmazás létrehozása** szövegmező; lesz meg az aad-ben alkalmazás a klasszikus Azure portálon később.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-169">Under **Management mode**, click **Create New AD App** if it isn't already selected, and notice the value that is in the **Create App** text box; you'll look up this AAD application in the Azure classic portal later.</span></span>
   
    ![Azure-portálon az Azure AD-beállítások](./media/app-service-api-dotnet-user-principal-auth/aadsettings2.png)
   
    <span data-ttu-id="1a4fb-171">Azure automatikusan létrehoz egy Azure AD-alkalmazást az Azure AD-bérlő jelzett nevű.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-171">Azure will automatically create an Azure AD application with the indicated name in your Azure AD tenant.</span></span> <span data-ttu-id="1a4fb-172">Alapértelmezés szerint az Azure AD-alkalmazás neve megegyezik az API-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-172">By default, the Azure AD application is named the same as the API app.</span></span> <span data-ttu-id="1a4fb-173">Tetszés szerint megadhat egy másik nevet.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-173">If you prefer, you can enter a different name.</span></span>
7. <span data-ttu-id="1a4fb-174">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-174">Click **OK**.</span></span>
8. <span data-ttu-id="1a4fb-175">Az a **hitelesítési / engedélyezési** panelen kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-175">In the **Authentication / Authorization** blade, click **Save**.</span></span>
   
    ![Kattintson a Save (Mentés) gombra.](./media/app-service-api-dotnet-user-principal-auth/authsave.png)

<span data-ttu-id="1a4fb-177">Most már csak az Azure AD-bérlő felhasználók meghívhatja az API-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-177">Now only users in your Azure AD tenant can call the API app.</span></span>

### <a name="optional-test-the-api-app"></a><span data-ttu-id="1a4fb-178">Választható lehetőség: Az API-alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="1a4fb-178">Optional: Test the API app</span></span>
1. <span data-ttu-id="1a4fb-179">Egy böngészőben nyissa meg az URL-címet a API-alkalmazás: az a **API-alkalmazás** panel az Azure-portálon kattintson a hivatkozásra kattintva **URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-179">In a browser go to the URL of the API app: in the **API app** blade in the Azure portal, click the link under **URL**.</span></span>  
   
    <span data-ttu-id="1a4fb-180">Ekkor megnyílik egy bejelentkezési képernyő, mert nem hitelesített kérelmek nem engedélyezettek az API-alkalmazás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-180">You are redirected to a login screen because unauthenticated requests are not allowed to reach the API app.</span></span>
   
    <span data-ttu-id="1a4fb-181">Ha a böngésző a "sikeres létrehozásról" lapra ugrik, a böngésző már kerülhettek naplózásra a – ebben az esetben, nyisson meg egy InPrivate vagy Incognito ablakot, és navigáljon az API-alkalmazás URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-181">If your browser goes to the "successfully created" page, the browser might already be logged on -- in that case, open an InPrivate or Incognito window and go to the API app's URL.</span></span>
2. <span data-ttu-id="1a4fb-182">Jelentkezzen be az Azure AD-bérlő a felhasználó hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-182">Log on using credentials for a user in your Azure AD tenant.</span></span>
   
    <span data-ttu-id="1a4fb-183">Ha van bejelentkezve, a "sikeres létrehozásról" tájékoztató oldal jelenik meg a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-183">When you're logged on, the "successfully created" page appears in the browser.</span></span>
3. <span data-ttu-id="1a4fb-184">Zárja be a böngészőt.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-184">Close the browser.</span></span>

### <a name="configure-the-azure-ad-application"></a><span data-ttu-id="1a4fb-185">Az Azure AD-alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1a4fb-185">Configure the Azure AD application</span></span>
<span data-ttu-id="1a4fb-186">Ha konfigurálta az Azure AD-alapú hitelesítés, az App Service létre egy Azure AD-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-186">When you configured Azure AD authentication, App Service created an Azure AD application for you.</span></span> <span data-ttu-id="1a4fb-187">Az új Azure AD alapértelmezés szerint az alkalmazás a tulajdonosi jogkivonat biztosításához az API-alkalmazás URL-cím lett beállítva.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-187">By default the new Azure AD application was configured to provide the bearer token to the API app's URL.</span></span> <span data-ttu-id="1a4fb-188">Ebben a szakaszban konfigurálása arra, hogy a tulajdonosi jogkivonatot az AngularJS előtér ahelyett, hogy közvetlenül a középső rétegbeli API-alkalmazást az Azure AD-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-188">In this section you configure the Azure AD application to provide the bearer token to the AngularJS front end instead of directly to the middle tier API app.</span></span> <span data-ttu-id="1a4fb-189">Az AngularJS előtér visszaküldi a jogkivonatot az API-alkalmazásba Ha meghívja az API-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-189">The AngularJS front end will send the token to the API app when it calls the API app.</span></span>

> [!NOTE]
> <span data-ttu-id="1a4fb-190">A folyamat tartsa a lehető egyszerű, a oktatóanyag használ egy egyetlen Azure AD alkalmazás az előtér- és a középső réteg API-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-190">To keep the process as simple as possible, this tutorial uses a single Azure AD application for both the front end and the middle tier API app.</span></span> <span data-ttu-id="1a4fb-191">Egy másik lehetőség, hogy két Azure AD-alkalmazások használja.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-191">Another option is to use two Azure AD applications.</span></span> <span data-ttu-id="1a4fb-192">Ebben az esetben kellene ahhoz, hogy megkapja az előtér az Azure AD alkalmazás számára a középső réteg az Azure AD-alkalmazást hívja.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-192">In that case you would have to give the front end's Azure AD application permission to call the middle tier's Azure AD application.</span></span> <span data-ttu-id="1a4fb-193">Ezt a módszert használja a több alkalmazást ad meg az egyes rétegek engedélyek több részletes szabályozását.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-193">This multi-application approach would give you more granular control over permissions to each tier.</span></span>
> 
> 

1. <span data-ttu-id="1a4fb-194">Az a [a klasszikus Azure portálon](https://manage.windowsazure.com/), és **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-194">In the [Azure classic portal](https://manage.windowsazure.com/), go to **Azure Active Directory**.</span></span>
   
   <span data-ttu-id="1a4fb-195">Akkor használja a klasszikus portálra, mert bizonyos Azure Active Directory-beállítások, amelyek hozzá kell férnie még nem állnak rendelkezésre az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-195">You have to use the classic portal because certain Azure Active Directory settings that you need access to are not yet available in the current Azure portal.</span></span>
2. <span data-ttu-id="1a4fb-196">Az a **Directory** lapra, majd az AAD-bérlőt.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-196">On the **Directory** tab, click your AAD tenant.</span></span>
   
   ![A klasszikus portálon az Azure AD](./media/app-service-api-dotnet-user-principal-auth/selecttenant.png)
3. <span data-ttu-id="1a4fb-198">Kattintson a **alkalmazások > a vállalat tulajdonában lévő alkalmazások**, majd kattintson a pipa jelre.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-198">Click **Applications > Applications my company owns**, and then click the check mark.</span></span>
   
   <span data-ttu-id="1a4fb-199">Is előfordulhat, hogy frissítse a lapot, melyen megtekintheti az új alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-199">You might also have to refresh the page to see the new application.</span></span>
4. <span data-ttu-id="1a4fb-200">Az alkalmazások listájának megtekintéséhez kattintson az Azure létrehozó meg, ha engedélyezte a hitelesítés az API-alkalmazás neve.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-200">In the list of applications, click the name of the one that Azure created for you when you enabled authentication for your API app.</span></span>
   
   ![Az Azure AD-alkalmazások lap](./media/app-service-api-dotnet-user-principal-auth/appstab.png)
5. <span data-ttu-id="1a4fb-202">Kattintson a **Configure** (Konfigurálás) elemre.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-202">Click **Configure**.</span></span>
   
   ![Az Azure AD konfigurálása lap](./media/app-service-api-dotnet-user-principal-auth/configure.png)
6. <span data-ttu-id="1a4fb-204">Állítsa be **bejelentkezési URL-cím** URL-címet a AngularJS webalkalmazás, nem végződhet perjellel.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-204">Set **Sign-on URL** to the URL for your AngularJS web app, no trailing slash.</span></span>
   
   <span data-ttu-id="1a4fb-205">Például: https://todolistangular.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="1a4fb-205">For example: https://todolistangular.azurewebsites.net</span></span>
   
   ![Bejelentkezési URL-cím](./media/app-service-api-dotnet-user-principal-auth/signonurlazure.png)
7. <span data-ttu-id="1a4fb-207">Állítsa be **válasz URL-CÍMEN** URL-címet a webalkalmazás, nem végződhet perjellel.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-207">Set **Reply URL** to the URL for your web app, no trailing slash.</span></span>
   
   <span data-ttu-id="1a4fb-208">Például: https://todolistsangular.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="1a4fb-208">For example: https://todolistsangular.azurewebsites.net</span></span>
8. <span data-ttu-id="1a4fb-209">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-209">Click **Save**.</span></span>
   
   ![Válasz URL-címe](./media/app-service-api-dotnet-user-principal-auth/replyurlazure.png)
9. <span data-ttu-id="1a4fb-211">Kattintson a lap alján **kezelése jegyzékfájl > Letöltés jegyzékfájl**.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-211">At the bottom of the page, click **Manage manifest > Download manifest**.</span></span>
   
   ![Töltse le a jegyzékben](./media/app-service-api-dotnet-user-principal-auth/downloadmanifest.png)
10. <span data-ttu-id="1a4fb-213">Töltse le a fájlt olyan helyre, ahol módosíthatja azt.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-213">Download the file to a location where you can edit it.</span></span>
11. <span data-ttu-id="1a4fb-214">A letöltött jegyzékfájl, keresse meg a `oauth2AllowImplicitFlow` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-214">In the downloaded manifest file, search for the  `oauth2AllowImplicitFlow` property.</span></span> <span data-ttu-id="1a4fb-215">Ez a tulajdonság értékének módosítása `false` való `true`, majd mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-215">Change the value of this property from `false` to `true`, and then save the file.</span></span>
    
    <span data-ttu-id="1a4fb-216">Ez a beállítás a hozzáférést a a JavaScript egyoldalas alkalmazások szükség.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-216">This setting is required for access from a JavaScript single-page application.</span></span> <span data-ttu-id="1a4fb-217">Lehetővé teszi a Oauth 2.0 tulajdonosi jogkivonatot az URL-töredéket adott vissza.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-217">It enables the Oauth 2.0 bearer token to be returned in the URL fragment.</span></span>
12. <span data-ttu-id="1a4fb-218">Kattintson a **kezelése jegyzékfájl > feltöltés jegyzékfájl**, és feltölteni a fájlt, hogy az előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-218">Click **Manage manifest > Upload manifest**, and upload the file that you updated in the preceding step.</span></span>
    
    ![Töltse fel a jegyzékben](./media/app-service-api-dotnet-user-principal-auth/uploadmanifest.png)
13. <span data-ttu-id="1a4fb-220">Másolás a **ügyfél-azonosító** értékét, és mentse valahol beszerezheti a később.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-220">Copy the **Client ID** value and save it somewhere you can get it from later.</span></span>

## <a name="configure-the-todolistangular-project-to-use-authentication"></a><span data-ttu-id="1a4fb-221">Hitelesítés használatára a ToDoListAngular projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1a4fb-221">Configure the ToDoListAngular project to use authentication</span></span>
<span data-ttu-id="1a4fb-222">Ebben a szakaszban módosíthatja az AngularJS előtér, hogy a bejelentkezett felhasználó az Azure AD egy tulajdonosi jogkivonatot szerezni az Active Directory Authentication Library (ADAL) a JS használja.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-222">In this section you change the AngularJS front end so that it uses Active Directory Authentication Library (ADAL) for JS to acquire a bearer token for the logged-on user from Azure AD.</span></span> <span data-ttu-id="1a4fb-223">A kódot tartalmazza a jogkivonatot a HTTP-kérések a középső réteg küldött az alábbi ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-223">The code will include the token in HTTP requests sent to the middle tier, as shown in the following diagram.</span></span> 

![Felhasználói hitelesítés diagramja](./media/app-service-api-dotnet-user-principal-auth/appdiagram.png)

<span data-ttu-id="1a4fb-225">A következő módosításokat a ToDoListAngular projekthez a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-225">Make the following changes to files in the ToDoListAngular project.</span></span>

1. <span data-ttu-id="1a4fb-226">Nyissa meg a *index.cshtml* fájlt.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-226">Open the *index.cshtml* file.</span></span>
2. <span data-ttu-id="1a4fb-227">Állítsa vissza az Active Directory Authentication Library (ADAL) JS parancsfájlok hivatkozó sorokat.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-227">Uncomment the lines that reference the Active Directory Authentication Library (ADAL) for JS scripts.</span></span>
   
        <script src="app/scripts/adal.js"></script>
        <script src="app/scripts/adal-angular.js"></script>
3. <span data-ttu-id="1a4fb-228">Nyissa meg a *app/scripts/app.js* fájlt.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-228">Open the *app/scripts/app.js* file.</span></span>
4. <span data-ttu-id="1a4fb-229">A "-hitelesítés nélkül" megjelölve kódblokkot megjegyzéssé, és állítsa vissza a kódblokkot az "a hitelesítés" jelölésű.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-229">Comment out the block of code marked for "without authentication" and uncomment the block of code marked for "with authentication".</span></span>
   
    <span data-ttu-id="1a4fb-230">Ez a változás az ADAL JS hitelesítésszolgáltatót és konfigurációs értékeket.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-230">This change references the ADAL JS authentication provider and provides configuration values to it.</span></span> <span data-ttu-id="1a4fb-231">A következő lépésekben állítsa be a konfigurációs értékeket az API-alkalmazás és az Azure AD-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-231">In the following steps you set the configuration values for your API app and Azure AD application.</span></span>
5. <span data-ttu-id="1a4fb-232">A kódban, amely beállítja a `endpoints` változót, az API-alkalmazás URL-címet a API URL-Címének beállítása a létre a ToDoListAPI projektet, és az ügyfél-azonosító, melyet a klasszikus Azure portálon kimásolt beállítása az Azure AD alkalmazás-azonosítója.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-232">In the code that sets the `endpoints` variable, set the API URL to the URL of the API app that you created for the ToDoListAPI project, and set the Azure AD application ID to the client ID that you copied from the Azure classic portal.</span></span>
   
    <span data-ttu-id="1a4fb-233">A kód mostantól az alábbi példához hasonló.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-233">The code is now similar to the following example.</span></span>
   
        var endpoints = {
            "https://todolistapi0121.azurewebsites.net/": "1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3"
        };
6. <span data-ttu-id="1a4fb-234">A hívásban `adalProvider.init`, beállíthatja `tenant` a bérlő neve és `clientId` ugyanarra az előző lépésben használt értékre.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-234">In the call to `adalProvider.init`, set `tenant` to your tenant name and `clientId` to same value you used in the previous step.</span></span>
   
    <span data-ttu-id="1a4fb-235">A kód mostantól az alábbi példához hasonló.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-235">The code is now similar to the following example.</span></span>
   
        adalProvider.init(
            {
                instance: 'https://login.microsoftonline.com/', 
                tenant: 'contoso.onmicrosoft.com',
                clientId: '1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3',
                extraQueryParameter: 'nux=1',
                endpoints: endpoints
            },
            $httpProvider
            );
   
    <span data-ttu-id="1a4fb-236">A módosítások a `app.js` adja meg, hogy a hívó kód és a hívott API legyenek-e az azonos Azure AD-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-236">These changes to `app.js` specify that the calling code and the called API are in the same Azure AD application.</span></span>
7. <span data-ttu-id="1a4fb-237">Nyissa meg a *app/scripts/homeCtrl.js* fájlt.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-237">Open the *app/scripts/homeCtrl.js* file.</span></span>
8. <span data-ttu-id="1a4fb-238">A "-hitelesítés nélkül" megjelölve kódblokkot megjegyzéssé, és állítsa vissza a kódblokkot az "a hitelesítés" jelölésű.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-238">Comment out the block of code marked for "without authentication" and uncomment the block of code marked for "with authentication".</span></span>
9. <span data-ttu-id="1a4fb-239">Nyissa meg a *app/scripts/indexCtrl.js* fájlt.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-239">Open the *app/scripts/indexCtrl.js* file.</span></span>
10. <span data-ttu-id="1a4fb-240">A "-hitelesítés nélkül" megjelölve kódblokkot megjegyzéssé, és állítsa vissza a kódblokkot az "a hitelesítés" jelölésű.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-240">Comment out the block of code marked for "without authentication" and uncomment the block of code marked for "with authentication".</span></span>

### <a name="deploy-the-todolistangular-project-to-azure"></a><span data-ttu-id="1a4fb-241">A ToDoListAngular projekt telepítése az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="1a4fb-241">Deploy the ToDoListAngular project to Azure</span></span>
1. <span data-ttu-id="1a4fb-242">A **Solution Explorer** (Megoldáskezelő) területén kattintson a jobb gombbal a ToDoListAngular projektre, majd kattintson a **Publish** (Közzététel) elemre.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-242">In **Solution Explorer**, right-click the ToDoListAngular project, and then click **Publish**.</span></span>
2. <span data-ttu-id="1a4fb-243">Kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-243">Click **Publish**.</span></span>
   
    <span data-ttu-id="1a4fb-244">A Visual Studio telepíti a projektet, és a webes alkalmazás alap URL-cím egy böngészőablakban megnyitja.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-244">Visual Studio deploys the project and opens a browser to the web app's base URL.</span></span> <span data-ttu-id="1a4fb-245">Ez azt mutatja majd 403-as hibalap, ez nem jelent problémát, a Web API alap URL-címet egy böngészőben nyissa tett kísérlet.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-245">This will show a 403 error page, which is normal for an attempt to go to a Web API base URL from a browser.</span></span>
   
    <span data-ttu-id="1a4fb-246">Továbbra is meg kell az alkalmazást tesztelheti előtt lehet módosítani a középső réteg API-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-246">You still have to make a change to the middle tier API app before you can test the application.</span></span>
3. <span data-ttu-id="1a4fb-247">Zárja be a böngészőt.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-247">Close the browser.</span></span>

## <a name="configure-the-todolistapi-project-to-use-authentication"></a><span data-ttu-id="1a4fb-248">Hitelesítés használatára a ToDoListAPI projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1a4fb-248">Configure the ToDoListAPI project to use authentication</span></span>
<span data-ttu-id="1a4fb-249">Jelenleg a ToDoListAPI projekt küld "*", a `owner` ToDoListDataAPI értéket.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-249">Currently the ToDoListAPI project sends "*" as the `owner` value to ToDoListDataAPI.</span></span> <span data-ttu-id="1a4fb-250">Ebben a szakaszban módosítsa a kód küldése a bejelentkezett felhasználó Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-250">In this section you change the code to send the ID of the logged-on user.</span></span>

<span data-ttu-id="1a4fb-251">A következő módosításokat a ToDoListAPI projektben.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-251">Make the following changes in the ToDoListAPI project.</span></span>

1. <span data-ttu-id="1a4fb-252">Nyissa meg a *Controllers/ToDoListController.cs* fájlt, és állítsa vissza a sor minden tartozó műveleti módszer, amely beállítja a `owner` az Azure ad `NameIdentifier` jogcím értéke.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-252">Open the *Controllers/ToDoListController.cs* file, and uncomment the line in each action method that sets `owner` to the Azure AD `NameIdentifier` claim value.</span></span> <span data-ttu-id="1a4fb-253">Példa:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-253">For example:</span></span>
   
        owner = ((ClaimsIdentity)User.Identity).FindFirst(ClaimTypes.NameIdentifier).Value;
   
    <span data-ttu-id="1a4fb-254">**Fontos**: ne állítsa vissza a kód a `ToDoListDataAPI` metódus; el kell végeznie, amely később a szolgáltatás egyszerű hitelesítés az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-254">**Important**: Don't uncomment code in the `ToDoListDataAPI` method; you'll do that later for the service principal authentication tutorial.</span></span>

### <a name="deploy-the-todolistapi-project-to-azure"></a><span data-ttu-id="1a4fb-255">A ToDoListAPI projekt telepítése az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="1a4fb-255">Deploy the ToDoListAPI project to Azure</span></span>
1. <span data-ttu-id="1a4fb-256">A **Megoldáskezelőben**, kattintson a jobb gombbal a ToDoListAPI projektet, és kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-256">In **Solution Explorer**, right-click the ToDoListAPI project, and then click **Publish**.</span></span>
2. <span data-ttu-id="1a4fb-257">Kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-257">Click **Publish**.</span></span>
   
    <span data-ttu-id="1a4fb-258">A Visual Studio telepíti a projektet, és az API-alkalmazás alap URL-cím egy böngészőablakban megnyitja.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-258">Visual Studio deploys the project and opens a browser to the API app's base URL.</span></span>
3. <span data-ttu-id="1a4fb-259">Zárja be a böngészőt.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-259">Close the browser.</span></span>

### <a name="test-the-application"></a><span data-ttu-id="1a4fb-260">Az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="1a4fb-260">Test the application</span></span>
1. <span data-ttu-id="1a4fb-261">Nyissa meg a webes alkalmazás URL-címét **HTTPS-kapcsolaton keresztül, nem HTTP**.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-261">Go to the URL of the web app, **using HTTPS, not HTTP**.</span></span>
2. <span data-ttu-id="1a4fb-262">Kattintson a **To Do List** fülre.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-262">Click the **To Do List** tab.</span></span>
   
    <span data-ttu-id="1a4fb-263">Jelentkezzen be kéri.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-263">You are prompted to log in.</span></span>
3. <span data-ttu-id="1a4fb-264">Jelentkezzen be az AAD-bérlőben egy olyan felhasználó hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-264">Log in with the credentials of a user in your AAD tenant.</span></span>
4. <span data-ttu-id="1a4fb-265">A **To Do List** lap jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-265">The **To Do List** page appears.</span></span>
   
   ![Tennivalók listája lap](./media/app-service-api-dotnet-user-principal-auth/webappindex.png)
   
   <span data-ttu-id="1a4fb-267">Nincs tennivaló jelennek meg, mert eddig összes voltak tulajdonosa "*".</span><span class="sxs-lookup"><span data-stu-id="1a4fb-267">No to-do items are displayed because until now they have all been for owner "*".</span></span> <span data-ttu-id="1a4fb-268">A középső réteg most kér a bejelentkezett felhasználó elemeket, és nincs még létrejött.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-268">Now the middle tier is requesting items for the logged-on user, and none have been created yet.</span></span>
5. <span data-ttu-id="1a4fb-269">Vegyen fel új tennivaló, hogy az alkalmazás működésének ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-269">Add new to-do items to verify that the application is working.</span></span>
6. <span data-ttu-id="1a4fb-270">Egy másik böngészőablakban, nyissa meg a Swagger felhasználói felület URL-címe a ToDoListDataAPI API-alkalmazást, és kattintson a **ToDoList > Get**.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-270">In another browser window, go to the Swagger UI URL for the ToDoListDataAPI API app and click **ToDoList > Get**.</span></span> <span data-ttu-id="1a4fb-271">Írjon be csillag karaktert a `owner` paraméter, és kattintson **próbálja ki**.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-271">Enter an asterisk for the `owner` parameter, and then click **Try it out**.</span></span>
   
   <span data-ttu-id="1a4fb-272">A válasz azt mutatja, hogy új teendőelemet a tényleges az Owner tulajdonság az Azure AD felhasználói Azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-272">The response shows that the new to-do items have the actual Azure AD user ID in the Owner property.</span></span>
   
   ![JSON-NÁ válaszul tulajdonos azonosítója](./media/app-service-api-dotnet-user-principal-auth/todolistapiauth.png)

## <a name="building-the-projects-from-scratch"></a><span data-ttu-id="1a4fb-274">Teljesen új projektek felépítése</span><span class="sxs-lookup"><span data-stu-id="1a4fb-274">Building the projects from scratch</span></span>
<span data-ttu-id="1a4fb-275">A két Web API-projektek használatával létrehozott a **Azure API App** projektek sablon és az alapértelmezett értékek vezérlő lecserélését egy ToDoList tartományvezérlőre.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-275">The two Web API projects were created by using the **Azure API App** project template and replacing the default Values controller with a ToDoList controller.</span></span> 

<span data-ttu-id="1a4fb-276">Hozzon létre egy AngularJS egyoldalas alkalmazást a Web API 2 háttérből kapcsolatos információkért lásd: [beavatkozás nélküli a labor: egy egyetlen oldal alkalmazás (SPA) az ASP.NET Web API és Angular.js Build](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs).</span><span class="sxs-lookup"><span data-stu-id="1a4fb-276">For information about how to  create an AngularJS single-page application with a Web API 2 back end, see  [Hands On Lab: Build a Single Page Application (SPA) with ASP.NET Web API and Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs).</span></span> <span data-ttu-id="1a4fb-277">Az Azure AD hitelesítési kód adásával kapcsolatos információkért lásd a következőket:</span><span class="sxs-lookup"><span data-stu-id="1a4fb-277">For information about how to add Azure AD authentication code, see the following resources:</span></span>

* <span data-ttu-id="1a4fb-278">[Az Azure AD-alkalmazások biztonságossá tétele az AngularJS egyetlen lapon](../active-directory/active-directory-devquickstarts-angular.md).</span><span class="sxs-lookup"><span data-stu-id="1a4fb-278">[Securing AngularJS Single Page Apps with Azure AD](../active-directory/active-directory-devquickstarts-angular.md).</span></span>
* [<span data-ttu-id="1a4fb-279">A V1-es ADAL JS bemutatása</span><span class="sxs-lookup"><span data-stu-id="1a4fb-279">Introducing ADAL JS v1</span></span>](http://www.cloudidentity.com/blog/2015/02/19/introducing-adal-js-v1/)

## <a name="troubleshooting"></a><span data-ttu-id="1a4fb-280">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="1a4fb-280">Troubleshooting</span></span>
[!INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* <span data-ttu-id="1a4fb-281">Győződjön meg arról, hogy azt ne tévessze össze ToDoListAPI (középső réteg) és a ToDoListDataAPI (adatrétegbeli).</span><span class="sxs-lookup"><span data-stu-id="1a4fb-281">Make sure that you don't confuse ToDoListAPI (middle tier) and ToDoListDataAPI (data tier).</span></span> <span data-ttu-id="1a4fb-282">Például győződjön meg arról, hogy a középső rétegbeli API-alkalmazást, nem az adatréteghez tartozó hitelesítési felvette.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-282">For example, verify that you added authentication to the middle tier API app, not the data tier.</span></span> 
* <span data-ttu-id="1a4fb-283">Győződjön meg arról, hogy az AngularJS forráskód hivatkozik-e a középső réteg API alkalmazás URL-CÍMÉT (ToDoListAPI, ToDoListDataAPI nem) és a megfelelő Azure AD ügyfél-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-283">Make sure that the AngularJS source code references the middle tier API app URL (ToDoListAPI, not ToDoListDataAPI)and the correct Azure AD client ID.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="1a4fb-284">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1a4fb-284">Next steps</span></span>
<span data-ttu-id="1a4fb-285">Ebben az oktatóanyagban megismerte az API-alkalmazás az App Service authentication használatával, és hogyan hívhatja meg az API-alkalmazás az ADAL-JS kódtárat használatával.</span><span class="sxs-lookup"><span data-stu-id="1a4fb-285">In this tutorial you learned how to use App Service authentication for an API app and how to call the API app by using the ADAL JS library.</span></span> <span data-ttu-id="1a4fb-286">A következő oktatóanyag megtudhatja, hogyan [biztonságos hozzáférés az API-alkalmazás a szolgáltatások közötti forgatókönyvek](app-service-api-dotnet-service-principal-auth.md).</span><span class="sxs-lookup"><span data-stu-id="1a4fb-286">In the next tutorial you'll learn how to [secure access to your API app for service-to-service scenarios](app-service-api-dotnet-service-principal-auth.md).</span></span>

