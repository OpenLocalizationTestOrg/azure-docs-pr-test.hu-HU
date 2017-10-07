---
title: "az Azure AD hitelesítési tooaccess Azure Media Services API a .NET aaaUse |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan toouse Azure Active Directory (Azure AD) hitelesítési tooaccess Azure Media Services (AMS) API-t .NET."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: 2f750e460d9e476ad92e96adeac6500cb692cd77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-ad-authentication-tooaccess-azure-media-services-api-with-net"></a><span data-ttu-id="9ad44-103">Az Azure AD hitelesítési tooaccess Azure Media Services API használata a .NET</span><span class="sxs-lookup"><span data-stu-id="9ad44-103">Use Azure AD authentication tooaccess Azure Media Services API with .NET</span></span>

<span data-ttu-id="9ad44-104">Windowsazure.mediaservices 4.0.0.4 verziótól kezdődően Azure Media Services Azure Active Directory (Azure AD) alapuló hitelesítést támogatja.</span><span class="sxs-lookup"><span data-stu-id="9ad44-104">Starting with windowsazure.mediaservices 4.0.0.4, Azure Media Services supports authentication based on Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="9ad44-105">Ez a témakör bemutatja, hogyan toouse az Azure AD hitelesítési tooaccess Azure Media Services API a Microsoft .NET.</span><span class="sxs-lookup"><span data-stu-id="9ad44-105">This topic shows you how toouse Azure AD  authentication tooaccess Azure Media Services API with Microsoft .NET.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9ad44-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9ad44-106">Prerequisites</span></span>

- <span data-ttu-id="9ad44-107">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="9ad44-107">An Azure account.</span></span> <span data-ttu-id="9ad44-108">További részletek: [Ingyenes Azure-próbafiók](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9ad44-108">For details, see [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
- <span data-ttu-id="9ad44-109">Egy Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="9ad44-109">A Media Services account.</span></span> <span data-ttu-id="9ad44-110">További információkért lásd: [hello Azure-portál használatával Azure Media Services-fiók létrehozása](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="9ad44-110">For more information, see [Create an Azure Media Services account using hello Azure portal](media-services-portal-create-account.md).</span></span>
- <span data-ttu-id="9ad44-111">hello legújabb [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) csomag.</span><span class="sxs-lookup"><span data-stu-id="9ad44-111">hello latest [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) package.</span></span>
- <span data-ttu-id="9ad44-112">Hello témakör ismeretét [eléréséhez Azure Media Services API-t az AAD-hitelesítés áttekintése](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="9ad44-112">Familiarity with hello topic [Accessing Azure Media Services API with AAD authentication overview](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

<span data-ttu-id="9ad44-113">Ha az Azure AD-alapú hitelesítés az Azure Media Services használata esetén az alábbi két módszer egyikével hitelesítheti:</span><span class="sxs-lookup"><span data-stu-id="9ad44-113">When you're using Azure AD authentication with Azure Media Services, you can authenticate in one of two ways:</span></span>

- <span data-ttu-id="9ad44-114">**Felhasználó hitelesítése** hitelesíti a személy, aki az Azure Media Services erőforrások hello app toointeract használ.</span><span class="sxs-lookup"><span data-stu-id="9ad44-114">**User authentication** authenticates a person who is using hello app toointeract with Azure Media Services resources.</span></span> <span data-ttu-id="9ad44-115">hello interaktív alkalmazás először kell hello felhasználók hitelesítő adatainak kéréséhez.</span><span class="sxs-lookup"><span data-stu-id="9ad44-115">hello interactive application should first prompt hello user for credentials.</span></span> <span data-ttu-id="9ad44-116">Példa: a felügyeleti konzol alkalmazást, amely jogosult felhasználók toomonitor kódolási feladatok által használt vagy live streaming.</span><span class="sxs-lookup"><span data-stu-id="9ad44-116">An example is a management console app that's used by authorized users toomonitor encoding jobs or live streaming.</span></span> 
- <span data-ttu-id="9ad44-117">**Szolgáltatás egyszerű hitelesítési** szolgáltatás hitelesíti.</span><span class="sxs-lookup"><span data-stu-id="9ad44-117">**Service principal authentication** authenticates a service.</span></span> <span data-ttu-id="9ad44-118">Alkalmazásokat, amelyek általában arra használják ezt a hitelesítési módszert démon szolgáltatások, a középső rétegbeli szolgáltatások vagy az ütemezett feladatok, például webalkalmazások, függvény alkalmazások, a logic apps, API-k vagy mikroszolgáltatások futó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="9ad44-118">Applications that commonly use this authentication method are apps that run daemon services, middle-tier services, or scheduled jobs, such as web apps, function apps, logic apps, APIs, or microservices.</span></span>

>[!IMPORTANT]
><span data-ttu-id="9ad44-119">Az Azure Media Services jelenleg olyan Azure Access Control Service hitelesítési modellt.</span><span class="sxs-lookup"><span data-stu-id="9ad44-119">Azure Media Service currently supports an Azure Access Control Service  authentication model.</span></span> <span data-ttu-id="9ad44-120">Azonban a hozzáférés-vezérlés engedélyezési érintetlen toobe 2018. június 1. az elavult.</span><span class="sxs-lookup"><span data-stu-id="9ad44-120">However, Access Control authorization is going toobe deprecated on June 1, 2018.</span></span> <span data-ttu-id="9ad44-121">Azt javasoljuk, hogy az áttelepített tooan Azure Active Directory hitelesítési modell lehető legrövidebb időn belül.</span><span class="sxs-lookup"><span data-stu-id="9ad44-121">We recommend that you migrate tooan Azure Active Directory authentication model as soon as possible.</span></span>

## <a name="get-an-azure-ad-access-token"></a><span data-ttu-id="9ad44-122">Az Azure AD hozzáférési jogkivonat beszerzése</span><span class="sxs-lookup"><span data-stu-id="9ad44-122">Get an Azure AD access token</span></span>

<span data-ttu-id="9ad44-123">tooconnect toohello Azure Media Services API az Azure AD-alapú hitelesítés, az Azure AD hozzáférési tokent toorequest kell hello ügyfél alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9ad44-123">tooconnect toohello Azure Media Services API with Azure AD authentication, hello client app needs toorequest an Azure AD access token.</span></span> <span data-ttu-id="9ad44-124">Hello Media Services .NET ügyfél SDK, hogyan az Azure AD hozzáférési tokent tooacquire burkolt, és a hello meg egyszerűsített hello adatait számos használatakor [AzureAdTokenProvider](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenProvider.cs) és [AzureAdTokenCredentials ](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenCredentials.cs) osztályok.</span><span class="sxs-lookup"><span data-stu-id="9ad44-124">When you use hello Media Services .NET client SDK, many of hello details about how tooacquire an Azure AD access token are wrapped and simplified for you in hello [AzureAdTokenProvider](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenProvider.cs) and [AzureAdTokenCredentials](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenCredentials.cs) classes.</span></span> 

<span data-ttu-id="9ad44-125">Például nincs szükség a tooprovide hello Azure AD-szolgáltatóként, a Media Services erőforrás URI vagy a natív Azure AD alkalmazás részletei.</span><span class="sxs-lookup"><span data-stu-id="9ad44-125">For example, you don't need tooprovide hello Azure AD authority, Media Services resource URI, or native Azure AD application details.</span></span> <span data-ttu-id="9ad44-126">Ezek a jól ismert hello Azure AD hozzáférési jogkivonat-szolgáltató osztály által már beállított értékeket.</span><span class="sxs-lookup"><span data-stu-id="9ad44-126">These are well-known values that are already configured by hello Azure AD access token provider class.</span></span> 

<span data-ttu-id="9ad44-127">Ha nem használ az Azure Media Service .NET SDK-t, azt javasoljuk, hogy használja-e hello [Azure AD Authentication Library](../active-directory/develop/active-directory-authentication-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="9ad44-127">If you are not using Azure Media Service .NET SDK, we recommend that you use hello [Azure AD Authentication Library](../active-directory/develop/active-directory-authentication-libraries.md).</span></span> <span data-ttu-id="9ad44-128">telepíteni kell, amely az Azure AD Authentication Library toouse hello paraméterek értékeit tooget lásd [hello Azure portál tooaccess az Azure AD hitelesítési beállítások](media-services-portal-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="9ad44-128">tooget values for hello parameters that you need toouse with Azure AD Authentication Library, see [Use hello Azure portal tooaccess Azure AD authentication settings](media-services-portal-get-started-with-aad.md).</span></span>

<span data-ttu-id="9ad44-129">Akkor is, hogy az alapértelmezett implementációja hello hello hello beállítás **AzureAdTokenProvider** a saját példánnyal.</span><span class="sxs-lookup"><span data-stu-id="9ad44-129">You also have hello option of replacing hello default implementation of hello **AzureAdTokenProvider** with your own implementation.</span></span>

## <a name="install-and-configure-azure-media-services-net-sdk"></a><span data-ttu-id="9ad44-130">Telepítse és konfigurálja az Azure Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="9ad44-130">Install and configure Azure Media Services .NET SDK</span></span>

>[!NOTE] 
><span data-ttu-id="9ad44-131">a Media Services .NET SDK hello toouse az Azure AD hitelesítési, kell toohave hello legújabb [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) csomag.</span><span class="sxs-lookup"><span data-stu-id="9ad44-131">toouse Azure AD authentication with hello Media Services .NET SDK, you need toohave hello latest [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) package.</span></span> <span data-ttu-id="9ad44-132">Továbbá adja hozzá egy hivatkozást toohello **Microsoft.IdentityModel.Clients.ActiveDirectory** szerelvény.</span><span class="sxs-lookup"><span data-stu-id="9ad44-132">Also, add a reference toohello **Microsoft.IdentityModel.Clients.ActiveDirectory** assembly.</span></span> <span data-ttu-id="9ad44-133">Ha egy meglévő alkalmazást használ, a következők hello **Microsoft.WindowsAzure.MediaServices.Client.Common.Authentication.dll** szerelvény.</span><span class="sxs-lookup"><span data-stu-id="9ad44-133">If you are using an existing app, include hello **Microsoft.WindowsAzure.MediaServices.Client.Common.Authentication.dll** assembly.</span></span> 

1. <span data-ttu-id="9ad44-134">Hozzon létre egy új C# konzolalkalmazást a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9ad44-134">Create a new C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="9ad44-135">Használjon hello [windowsazure.mediaservices](https://www.nuget.org/packages/windowsazure.mediaservices) NuGet csomag tooinstall **Azure Media Services .NET SDK**.</span><span class="sxs-lookup"><span data-stu-id="9ad44-135">Use hello [windowsazure.mediaservices](https://www.nuget.org/packages/windowsazure.mediaservices) NuGet package tooinstall **Azure Media Services .NET SDK**.</span></span> 

    <span data-ttu-id="9ad44-136">tooadd hivatkozások használatával NuGet, érvénybe hello a következő lépéseket: a **Megoldáskezelőben**, kattintson a jobb gombbal a hello projekt nevét, majd válassza ki **kezelése NuGet-csomagok**.</span><span class="sxs-lookup"><span data-stu-id="9ad44-136">tooadd references by using NuGet, take hello following steps: in **Solution Explorer**, right-click hello project name, and then select **Manage NuGet packages**.</span></span> <span data-ttu-id="9ad44-137">Ezután keresse meg **windowsazure.mediaservices** válassza **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="9ad44-137">Then, search for **windowsazure.mediaservices** and select **Install**.</span></span>
    
    <span data-ttu-id="9ad44-138">– vagy –</span><span class="sxs-lookup"><span data-stu-id="9ad44-138">-or-</span></span>

    <span data-ttu-id="9ad44-139">Futtatási hello parancs a következő **Csomagkezelő konzol** a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="9ad44-139">Run hello following command in **Package Manager Console** in Visual Studio.</span></span>

        Install-Package windowsazure.mediaservices -Version 4.0.0.4

3. <span data-ttu-id="9ad44-140">Adja hozzá **használatával** tooyour forráskódot.</span><span class="sxs-lookup"><span data-stu-id="9ad44-140">Add **using** tooyour source code.</span></span>

        using Microsoft.WindowsAzure.MediaServices.Client; 

## <a name="use-user-authentication"></a><span data-ttu-id="9ad44-141">Hitelesítés használatára</span><span class="sxs-lookup"><span data-stu-id="9ad44-141">Use user authentication</span></span>

<span data-ttu-id="9ad44-142">tooconnect toohello Azure Media Service API hello felhasználói hitelesítési lehetőséggel, hello ügyfélalkalmazás kell toorequest használatával az Azure AD-tokent hello a következő paramétereket:</span><span class="sxs-lookup"><span data-stu-id="9ad44-142">tooconnect toohello Azure Media Service API with hello user authentication option, hello client app needs toorequest an Azure AD token by using hello following parameters:</span></span>  

- <span data-ttu-id="9ad44-143">Azure AD-bérlő végpont.</span><span class="sxs-lookup"><span data-stu-id="9ad44-143">Azure AD tenant endpoint.</span></span> <span data-ttu-id="9ad44-144">hello bérlői adatait lekérhetők hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="9ad44-144">hello tenant information can be retrieved from hello Azure portal.</span></span> <span data-ttu-id="9ad44-145">Hello bejelentkezett felhasználó hello jobb felső sarkában található mutasson.</span><span class="sxs-lookup"><span data-stu-id="9ad44-145">Hover over hello signed-in user in hello upper-right corner.</span></span>
- <span data-ttu-id="9ad44-146">A Media Services erőforrás URI azonosítója.</span><span class="sxs-lookup"><span data-stu-id="9ad44-146">Media Services resource URI.</span></span>
- <span data-ttu-id="9ad44-147">A Media Services (natív) alkalmazás ügyfél-azonosító.</span><span class="sxs-lookup"><span data-stu-id="9ad44-147">Media Services (native) application client ID.</span></span> 
- <span data-ttu-id="9ad44-148">Media Services (natív) alkalmazás átirányítási URI-címe.</span><span class="sxs-lookup"><span data-stu-id="9ad44-148">Media Services (native) application redirect URI.</span></span> 

<span data-ttu-id="9ad44-149">Ezek a paraméterek értékeit hello található **AzureEnvironments.AzureCloudEnvironment**.</span><span class="sxs-lookup"><span data-stu-id="9ad44-149">hello values for these parameters can be found in **AzureEnvironments.AzureCloudEnvironment**.</span></span> <span data-ttu-id="9ad44-150">Hello **AzureEnvironments.AzureCloudEnvironment** állandó van a .NET SDK tooget hello segítő hello jobb környezetiváltozó-beállításainak egy nyilvános Azure-adatközpont.</span><span class="sxs-lookup"><span data-stu-id="9ad44-150">hello **AzureEnvironments.AzureCloudEnvironment** constant is a helper in hello .NET SDK tooget hello right environment variable settings for a public Azure Data Center.</span></span> 

<span data-ttu-id="9ad44-151">Előre megadott környezeti beállításokat tartalmaz hello nyilvános adatközpontokban csak a Media Services eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="9ad44-151">It contains pre-defined environment settings for accessing Media Services in hello public data centers only.</span></span> <span data-ttu-id="9ad44-152">A állami vagy kormányzati felhőalapú területeket, használhat **AzureChinaCloudEnvironment**, **AzureUsGovernmentEnvrionment**, vagy **AzureGermanCloudEnvironment** kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="9ad44-152">For sovereign or government cloud regions, you can use **AzureChinaCloudEnvironment**, **AzureUsGovernmentEnvrionment**, or **AzureGermanCloudEnvironment** respectively.</span></span>

<span data-ttu-id="9ad44-153">a következő példakód hello létrehoz egy tokent:</span><span class="sxs-lookup"><span data-stu-id="9ad44-153">hello following code example creates a token:</span></span>
    
    var tokenCredentials = new AzureAdTokenCredentials("microsoft.onmicrosoft.com", AzureEnvironments.AzureCloudEnvironment);
    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
  
<span data-ttu-id="9ad44-154">toostart programozási Media Services ellen, toocreate kell egy **CloudMediaContext** hello kiszolgálói környezetbe jelölő példány.</span><span class="sxs-lookup"><span data-stu-id="9ad44-154">toostart programming against Media Services, you need toocreate a **CloudMediaContext** instance that represents hello server context.</span></span> <span data-ttu-id="9ad44-155">Hello **CloudMediaContext** magában foglalja többek között a feladatok, eszközök, fájlok, hozzáférési házirendek és keresők hivatkozások tooimportant gyűjtemények.</span><span class="sxs-lookup"><span data-stu-id="9ad44-155">hello **CloudMediaContext** includes references tooimportant collections including jobs, assets, files, access policies, and locators.</span></span> 

<span data-ttu-id="9ad44-156">Szükség toopass hello **erőforrás URI azonosítója a Media Services-REST** toohello **CloudMediaContext** konstruktor.</span><span class="sxs-lookup"><span data-stu-id="9ad44-156">You also need toopass hello **resource URI for Media REST Services** toohello **CloudMediaContext** constructor.</span></span> <span data-ttu-id="9ad44-157">tooget hello erőforrás URI REST médiaszolgáltatások, jelentkezzen be Azure-portálon toohello, válassza ki az Azure Media Services-fiókhoz, válasszon **API-hozzáférés**, majd válassza ki **tooAzure Media Services felhasználói csatlakozás hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="9ad44-157">tooget hello resource URI for Media REST Services, sign in toohello Azure portal, select your Azure Media Services account, select **API access**, and then select **Connect tooAzure Media Services with user authentication**.</span></span> 

<span data-ttu-id="9ad44-158">hello alábbi példakód létrehoz egy **CloudMediaContext** példány:</span><span class="sxs-lookup"><span data-stu-id="9ad44-158">hello following code example creates a **CloudMediaContext** instance:</span></span>

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);

<span data-ttu-id="9ad44-159">hello a következő példa bemutatja, hogyan toocreate hello Azure AD-jogkivonat és hello környezetben:</span><span class="sxs-lookup"><span data-stu-id="9ad44-159">hello following example shows how toocreate hello Azure AD token and hello context:</span></span>

    namespace AADAuthSample
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Specify your Azure AD tenant domain, for example "microsoft.onmicrosoft.com".
                var tokenCredentials = new AzureAdTokenCredentials("{YOUR AAD TENANT DOMAIN HERE}", AzureEnvironments.AzureCloudEnvironment);
    
                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
    
                // Specify your REST API endpoint, for example "https://accountname.restv2.westcentralus.media.azure.net/API".
                CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
                var assets = context.Assets;
                foreach (var a in assets)
                {
                    Console.WriteLine(a.Name);
                }
            }
    
        }
    }

>[!NOTE]
><span data-ttu-id="9ad44-160">Ha egy kivételt, amely szerint "hello távoli kiszolgáló hibát adott vissza: (401) nem engedélyezett," hello lásd [hozzáférés-vezérlés](media-services-use-aad-auth-to-access-ams-api.md#access-control) Azure Media Services API elérése az Azure AD hitelesítési – Áttekintés szakasza.</span><span class="sxs-lookup"><span data-stu-id="9ad44-160">If you get an exception that says "hello remote server returned an error: (401) Unauthorized," see hello [Access control](media-services-use-aad-auth-to-access-ams-api.md#access-control) section of Accessing Azure Media Services API with Azure AD authentication overview.</span></span>

## <a name="use-service-principal-authentication"></a><span data-ttu-id="9ad44-161">Szolgáltatás egyszerű hitelesítés használata</span><span class="sxs-lookup"><span data-stu-id="9ad44-161">Use service principal authentication</span></span>
    
<span data-ttu-id="9ad44-162">tooconnect toohello Azure Media Services API hello szolgáltatás egyszerű beállítás, a középső rétegbeli (webes API-t vagy webalkalmazás) kell toorequests az Azure AD-tokent a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="9ad44-162">tooconnect toohello Azure Media Services API with hello service principal option, your middle-tier app (web API or web application) needs toorequests an Azure AD token with hello following parameters:</span></span>  

- <span data-ttu-id="9ad44-163">Azure AD-bérlő végpont.</span><span class="sxs-lookup"><span data-stu-id="9ad44-163">Azure AD tenant endpoint.</span></span> <span data-ttu-id="9ad44-164">hello bérlői adatait lekérhetők hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="9ad44-164">hello tenant information can be retrieved from hello Azure portal.</span></span> <span data-ttu-id="9ad44-165">Hello bejelentkezett felhasználó hello jobb felső sarkában található mutasson.</span><span class="sxs-lookup"><span data-stu-id="9ad44-165">Hover over hello signed-in user in hello upper-right corner.</span></span>
- <span data-ttu-id="9ad44-166">A Media Services erőforrás URI azonosítója.</span><span class="sxs-lookup"><span data-stu-id="9ad44-166">Media Services resource URI.</span></span>
- <span data-ttu-id="9ad44-167">Az Azure AD alkalmazás értékek: hello **ügyfél-azonosító** és **ügyfélkulcs**.</span><span class="sxs-lookup"><span data-stu-id="9ad44-167">Azure AD application values: hello **Client ID** and **Client secret**.</span></span>

<span data-ttu-id="9ad44-168">hello értékeinek hello **ügyfél-azonosító** és **ügyfélkulcs** paraméterek hello Azure-portálon található.</span><span class="sxs-lookup"><span data-stu-id="9ad44-168">hello values for hello **Client ID** and **Client secret** parameters can be found in hello Azure portal.</span></span> <span data-ttu-id="9ad44-169">További információkért lásd: [Ismerkedés az Azure AD használatával hello Azure-portálon](media-services-portal-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="9ad44-169">For more information, see [Getting started with Azure AD authentication using hello Azure portal](media-services-portal-get-started-with-aad.md).</span></span>

<span data-ttu-id="9ad44-170">hello alábbi példakód létrehoz egy tokent hello segítségével **AzureAdTokenCredentials** konstruktort **AzureAdClientSymmetricKey** paramétert:</span><span class="sxs-lookup"><span data-stu-id="9ad44-170">hello following code example creates a token by using hello **AzureAdTokenCredentials** constructor that takes **AzureAdClientSymmetricKey** as a parameter:</span></span> 
    
    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientSymmetricKey("{YOUR CLIENT ID HERE}", "{YOUR CLIENT SECRET}"), 
                                AzureEnvironments.AzureCloudEnvironment);

    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

<span data-ttu-id="9ad44-171">Azt is megadhatja, hello **AzureAdTokenCredentials** konstruktort **AzureAdClientCertificate** paraméterként.</span><span class="sxs-lookup"><span data-stu-id="9ad44-171">You can also specify hello **AzureAdTokenCredentials** constructor that takes **AzureAdClientCertificate** as a parameter.</span></span> 

<span data-ttu-id="9ad44-172">Kapcsolatos útmutatásért toocreate és a tanúsítvány konfigurálása az Azure AD használatával, lásd: űrlapon [tooAzure AD a tanúsítványok – kézi konfigurálás lépéseit démon alkalmazások hitelesítése](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md).</span><span class="sxs-lookup"><span data-stu-id="9ad44-172">For instructions about how toocreate and configure a certificate in a form that can be used by Azure AD, see [Authenticating tooAzure AD in daemon apps with certificates - manual configuration steps](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md).</span></span>

    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientCertificate("{YOUR CLIENT ID HERE}", "{YOUR CLIENT CERTIFICATE THUMBPRINT}"), 
                                AzureEnvironments.AzureCloudEnvironment);

<span data-ttu-id="9ad44-173">toostart programozási Media Services ellen, toocreate kell egy **CloudMediaContext** hello kiszolgálói környezetbe jelölő példány.</span><span class="sxs-lookup"><span data-stu-id="9ad44-173">toostart programming against Media Services, you need toocreate a **CloudMediaContext** instance that represents hello server context.</span></span> <span data-ttu-id="9ad44-174">Szükség toopass hello **erőforrás URI azonosítója a Media Services-REST** toohello **CloudMediaContext** konstruktor.</span><span class="sxs-lookup"><span data-stu-id="9ad44-174">You also need toopass hello **resource URI for Media REST Services** toohello **CloudMediaContext** constructor.</span></span> <span data-ttu-id="9ad44-175">Hello kaphat **erőforrás URI azonosítója a Media Services-REST** hello Azure-portálon is közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="9ad44-175">You can get hello **resource URI for Media REST Services** value from hello Azure portal as well.</span></span>

<span data-ttu-id="9ad44-176">hello alábbi példakód létrehoz egy **CloudMediaContext** példány:</span><span class="sxs-lookup"><span data-stu-id="9ad44-176">hello following code example creates a **CloudMediaContext** instance:</span></span>

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
<span data-ttu-id="9ad44-177">hello a következő példa bemutatja, hogyan toocreate hello Azure AD-jogkivonat és hello környezetben:</span><span class="sxs-lookup"><span data-stu-id="9ad44-177">hello following example shows how toocreate hello Azure AD token and hello context:</span></span>

    namespace AADAuthSample
    {
    
        class Program
        {
            static void Main(string[] args)
            {
                var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                            new AzureAdClientSymmetricKey("{YOUR CLIENT ID HERE}", "{YOUR CLIENT SECRET}"), 
                                            AzureEnvironments.AzureCloudEnvironment);
            
                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
    
                // Specify your REST API endpoint, for example "https://accountname.restv2.westcentralus.media.azure.net/API".      
                CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
                var assets = context.Assets;
                foreach (var a in assets)
                {
                    Console.WriteLine(a.Name);
                }
    
                Console.ReadLine();
            }
    
        }
    }

## <a name="next-steps"></a><span data-ttu-id="9ad44-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9ad44-178">Next steps</span></span>

<span data-ttu-id="9ad44-179">Ismerkedés a [fájlok feltöltése a tooyour fiók](media-services-dotnet-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="9ad44-179">Get started with [uploading files tooyour account](media-services-dotnet-upload-files.md).</span></span>
