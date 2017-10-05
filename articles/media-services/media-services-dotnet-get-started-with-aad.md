---
title: "Azure Media Services API a .NET eléréséhez használja az Azure AD hitelesítési |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan Azure Media Services (AMS) API-t .NET eléréséhez Azure Active Directory (Azure AD-) hitelesítés használatára."
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
ms.openlocfilehash: a9355200a05a3aa1b494b76977d38ddc42bfe179
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-ad-authentication-to-access-azure-media-services-api-with-net"></a><span data-ttu-id="eb92b-103">Azure Media Services API a .NET eléréséhez használja az Azure AD-alapú hitelesítés</span><span class="sxs-lookup"><span data-stu-id="eb92b-103">Use Azure AD authentication to access Azure Media Services API with .NET</span></span>

<span data-ttu-id="eb92b-104">Windowsazure.mediaservices 4.0.0.4 verziótól kezdődően Azure Media Services Azure Active Directory (Azure AD) alapuló hitelesítést támogatja.</span><span class="sxs-lookup"><span data-stu-id="eb92b-104">Starting with windowsazure.mediaservices 4.0.0.4, Azure Media Services supports authentication based on Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="eb92b-105">Ez a témakör bemutatja, hogyan használják az Azure AD-alapú hitelesítés Azure Media Services API a Microsoft .NET eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="eb92b-105">This topic shows you how to use Azure AD  authentication to access Azure Media Services API with Microsoft .NET.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eb92b-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="eb92b-106">Prerequisites</span></span>

- <span data-ttu-id="eb92b-107">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="eb92b-107">An Azure account.</span></span> <span data-ttu-id="eb92b-108">További részletek: [Ingyenes Azure-próbafiók](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="eb92b-108">For details, see [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
- <span data-ttu-id="eb92b-109">Egy Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="eb92b-109">A Media Services account.</span></span> <span data-ttu-id="eb92b-110">További információkért lásd: [hozzon létre egy Azure Media Services-fiók az Azure portál használatával](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="eb92b-110">For more information, see [Create an Azure Media Services account using the Azure portal](media-services-portal-create-account.md).</span></span>
- <span data-ttu-id="eb92b-111">A legújabb [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) csomag.</span><span class="sxs-lookup"><span data-stu-id="eb92b-111">The latest [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) package.</span></span>
- <span data-ttu-id="eb92b-112">A témakör ismeretét [eléréséhez Azure Media Services API-t az AAD-hitelesítés áttekintése](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="eb92b-112">Familiarity with the topic [Accessing Azure Media Services API with AAD authentication overview](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

<span data-ttu-id="eb92b-113">Ha az Azure AD-alapú hitelesítés az Azure Media Services használata esetén az alábbi két módszer egyikével hitelesítheti:</span><span class="sxs-lookup"><span data-stu-id="eb92b-113">When you're using Azure AD authentication with Azure Media Services, you can authenticate in one of two ways:</span></span>

- <span data-ttu-id="eb92b-114">**Felhasználó hitelesítése** hitelesíti a személy, aki használja az alkalmazás interakciót Azure Media Services-erőforrásokkal.</span><span class="sxs-lookup"><span data-stu-id="eb92b-114">**User authentication** authenticates a person who is using the app to interact with Azure Media Services resources.</span></span> <span data-ttu-id="eb92b-115">Az interaktív alkalmazás először kell kérni a felhasználót a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="eb92b-115">The interactive application should first prompt the user for credentials.</span></span> <span data-ttu-id="eb92b-116">Példa: egy kódolási feladatok figyelése, vagy live streaming jogosult felhasználók által használt felügyeleti konzol alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="eb92b-116">An example is a management console app that's used by authorized users to monitor encoding jobs or live streaming.</span></span> 
- <span data-ttu-id="eb92b-117">**Szolgáltatás egyszerű hitelesítési** szolgáltatás hitelesíti.</span><span class="sxs-lookup"><span data-stu-id="eb92b-117">**Service principal authentication** authenticates a service.</span></span> <span data-ttu-id="eb92b-118">Alkalmazásokat, amelyek általában arra használják ezt a hitelesítési módszert démon szolgáltatások, a középső rétegbeli szolgáltatások vagy az ütemezett feladatok, például webalkalmazások, függvény alkalmazások, a logic apps, API-k vagy mikroszolgáltatások futó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="eb92b-118">Applications that commonly use this authentication method are apps that run daemon services, middle-tier services, or scheduled jobs, such as web apps, function apps, logic apps, APIs, or microservices.</span></span>

>[!IMPORTANT]
><span data-ttu-id="eb92b-119">Az Azure Media Services jelenleg olyan Azure Access Control Service hitelesítési modellt.</span><span class="sxs-lookup"><span data-stu-id="eb92b-119">Azure Media Service currently supports an Azure Access Control Service  authentication model.</span></span> <span data-ttu-id="eb92b-120">Azonban a hozzáférés-vezérlés engedélyezési érintetlen 2018. június 1. a elavulttá válik.</span><span class="sxs-lookup"><span data-stu-id="eb92b-120">However, Access Control authorization is going to be deprecated on June 1, 2018.</span></span> <span data-ttu-id="eb92b-121">Azt javasoljuk, hogy telepítse át az Azure Active Directory hitelesítési modell lehető legrövidebb időn belül.</span><span class="sxs-lookup"><span data-stu-id="eb92b-121">We recommend that you migrate to an Azure Active Directory authentication model as soon as possible.</span></span>

## <a name="get-an-azure-ad-access-token"></a><span data-ttu-id="eb92b-122">Az Azure AD hozzáférési jogkivonat beszerzése</span><span class="sxs-lookup"><span data-stu-id="eb92b-122">Get an Azure AD access token</span></span>

<span data-ttu-id="eb92b-123">Szeretne csatlakozni az Azure Media Services API-t az Azure AD-alapú hitelesítés, az ügyfélalkalmazás kell egy Azure AD hozzáférési jogkivonatot kérni.</span><span class="sxs-lookup"><span data-stu-id="eb92b-123">To connect to the Azure Media Services API with Azure AD authentication, the client app needs to request an Azure AD access token.</span></span> <span data-ttu-id="eb92b-124">A Media Services .NET SDK ügyfél használata esetén az Azure AD hozzáférési tokent beszerzésére vonatkozó részleteket számos becsomagolt, és meg az egyszerűsített a [AzureAdTokenProvider](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenProvider.cs) és [AzureAdTokenCredentials](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenCredentials.cs)osztályok.</span><span class="sxs-lookup"><span data-stu-id="eb92b-124">When you use the Media Services .NET client SDK, many of the details about how to acquire an Azure AD access token are wrapped and simplified for you in the [AzureAdTokenProvider](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenProvider.cs) and [AzureAdTokenCredentials](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenCredentials.cs) classes.</span></span> 

<span data-ttu-id="eb92b-125">Például, hogy nem kell adja meg az Azure AD-szolgáltatóként, a Media Services erőforrás URI vagy a natív Azure AD alkalmazás részletei.</span><span class="sxs-lookup"><span data-stu-id="eb92b-125">For example, you don't need to provide the Azure AD authority, Media Services resource URI, or native Azure AD application details.</span></span> <span data-ttu-id="eb92b-126">Ezek a jól ismert értékek, amelyek már az Azure AD hozzáférési jogkivonat-szolgáltató osztály.</span><span class="sxs-lookup"><span data-stu-id="eb92b-126">These are well-known values that are already configured by the Azure AD access token provider class.</span></span> 

<span data-ttu-id="eb92b-127">Ha az Azure Media Service .NET SDK-t nem használ, azt javasoljuk, hogy használja a [Azure AD Authentication Library](../active-directory/develop/active-directory-authentication-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="eb92b-127">If you are not using Azure Media Service .NET SDK, we recommend that you use the [Azure AD Authentication Library](../active-directory/develop/active-directory-authentication-libraries.md).</span></span> <span data-ttu-id="eb92b-128">Ahhoz, hogy az értékeket a paraméterek, amelyekre szüksége van az Azure AD Authentication Library használandó, lásd: [az Azure AD hitelesítési beállítások eléréséhez használja az Azure-portálon](media-services-portal-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="eb92b-128">To get values for the parameters that you need to use with Azure AD Authentication Library, see [Use the Azure portal to access Azure AD authentication settings](media-services-portal-get-started-with-aad.md).</span></span>

<span data-ttu-id="eb92b-129">Lehetősége is van, hogy az alapértelmezett végrehajtása a **AzureAdTokenProvider** a saját példánnyal.</span><span class="sxs-lookup"><span data-stu-id="eb92b-129">You also have the option of replacing the default implementation of the **AzureAdTokenProvider** with your own implementation.</span></span>

## <a name="install-and-configure-azure-media-services-net-sdk"></a><span data-ttu-id="eb92b-130">Telepítse és konfigurálja az Azure Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="eb92b-130">Install and configure Azure Media Services .NET SDK</span></span>

>[!NOTE] 
><span data-ttu-id="eb92b-131">A Media Services .NET SDK-val az Azure AD-alapú hitelesítés használatához meg kell rendelkeznie a legújabb [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) csomag.</span><span class="sxs-lookup"><span data-stu-id="eb92b-131">To use Azure AD authentication with the Media Services .NET SDK, you need to have the latest [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) package.</span></span> <span data-ttu-id="eb92b-132">Továbbá adja hozzá egy hivatkozást a **Microsoft.IdentityModel.Clients.ActiveDirectory** szerelvény.</span><span class="sxs-lookup"><span data-stu-id="eb92b-132">Also, add a reference to the **Microsoft.IdentityModel.Clients.ActiveDirectory** assembly.</span></span> <span data-ttu-id="eb92b-133">Ha egy meglévő alkalmazást használ, a **Microsoft.WindowsAzure.MediaServices.Client.Common.Authentication.dll** szerelvény.</span><span class="sxs-lookup"><span data-stu-id="eb92b-133">If you are using an existing app, include the **Microsoft.WindowsAzure.MediaServices.Client.Common.Authentication.dll** assembly.</span></span> 

1. <span data-ttu-id="eb92b-134">Hozzon létre egy új C# konzolalkalmazást a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eb92b-134">Create a new C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="eb92b-135">Használja a [windowsazure.mediaservices](https://www.nuget.org/packages/windowsazure.mediaservices) NuGet-csomag telepítése **Azure Media Services .NET SDK**.</span><span class="sxs-lookup"><span data-stu-id="eb92b-135">Use the [windowsazure.mediaservices](https://www.nuget.org/packages/windowsazure.mediaservices) NuGet package to install **Azure Media Services .NET SDK**.</span></span> 

    <span data-ttu-id="eb92b-136">NuGet segítségével hivatkozások hozzáadásához tegye a következőket: a **Megoldáskezelőben**, kattintson a jobb gombbal a projekt nevére, majd válassza ki **kezelése NuGet-csomagok**.</span><span class="sxs-lookup"><span data-stu-id="eb92b-136">To add references by using NuGet, take the following steps: in **Solution Explorer**, right-click the project name, and then select **Manage NuGet packages**.</span></span> <span data-ttu-id="eb92b-137">Ezután keresse meg **windowsazure.mediaservices** válassza **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="eb92b-137">Then, search for **windowsazure.mediaservices** and select **Install**.</span></span>
    
    <span data-ttu-id="eb92b-138">– vagy –</span><span class="sxs-lookup"><span data-stu-id="eb92b-138">-or-</span></span>

    <span data-ttu-id="eb92b-139">A következő parancsot **Csomagkezelő konzol** a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="eb92b-139">Run the following command in **Package Manager Console** in Visual Studio.</span></span>

        Install-Package windowsazure.mediaservices -Version 4.0.0.4

3. <span data-ttu-id="eb92b-140">Adja hozzá **használatával** számára a forráskódot.</span><span class="sxs-lookup"><span data-stu-id="eb92b-140">Add **using** to your source code.</span></span>

        using Microsoft.WindowsAzure.MediaServices.Client; 

## <a name="use-user-authentication"></a><span data-ttu-id="eb92b-141">Hitelesítés használatára</span><span class="sxs-lookup"><span data-stu-id="eb92b-141">Use user authentication</span></span>

<span data-ttu-id="eb92b-142">Szeretne csatlakozni az Azure Media Service API a felhasználói hitelesítést választja, az ügyfélalkalmazás van szüksége az Azure AD-tokent kérése a következő paraméterek használatával:</span><span class="sxs-lookup"><span data-stu-id="eb92b-142">To connect to the Azure Media Service API with the user authentication option, the client app needs to request an Azure AD token by using the following parameters:</span></span>  

- <span data-ttu-id="eb92b-143">Azure AD-bérlő végpont.</span><span class="sxs-lookup"><span data-stu-id="eb92b-143">Azure AD tenant endpoint.</span></span> <span data-ttu-id="eb92b-144">A bérlői adatait az Azure portálról lehet beolvasni.</span><span class="sxs-lookup"><span data-stu-id="eb92b-144">The tenant information can be retrieved from the Azure portal.</span></span> <span data-ttu-id="eb92b-145">A jobb felső sarkában a bejelentkezett felhasználó mutasson.</span><span class="sxs-lookup"><span data-stu-id="eb92b-145">Hover over the signed-in user in the upper-right corner.</span></span>
- <span data-ttu-id="eb92b-146">A Media Services erőforrás URI azonosítója.</span><span class="sxs-lookup"><span data-stu-id="eb92b-146">Media Services resource URI.</span></span>
- <span data-ttu-id="eb92b-147">A Media Services (natív) alkalmazás ügyfél-azonosító.</span><span class="sxs-lookup"><span data-stu-id="eb92b-147">Media Services (native) application client ID.</span></span> 
- <span data-ttu-id="eb92b-148">Media Services (natív) alkalmazás átirányítási URI-címe.</span><span class="sxs-lookup"><span data-stu-id="eb92b-148">Media Services (native) application redirect URI.</span></span> 

<span data-ttu-id="eb92b-149">Ezek a paraméterek értékeit található **AzureEnvironments.AzureCloudEnvironment**.</span><span class="sxs-lookup"><span data-stu-id="eb92b-149">The values for these parameters can be found in **AzureEnvironments.AzureCloudEnvironment**.</span></span> <span data-ttu-id="eb92b-150">A **AzureEnvironments.AzureCloudEnvironment** állandó van a .NET SDK segíthetnek a megfelelő környezet egy nyilvános Azure-adatközpont környezetiváltozó-beállításainak segítő.</span><span class="sxs-lookup"><span data-stu-id="eb92b-150">The **AzureEnvironments.AzureCloudEnvironment** constant is a helper in the .NET SDK to get the right environment variable settings for a public Azure Data Center.</span></span> 

<span data-ttu-id="eb92b-151">Csak a nyilvános adatközpontokban Media Services eléréséhez előre megadott környezeti beállításokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="eb92b-151">It contains pre-defined environment settings for accessing Media Services in the public data centers only.</span></span> <span data-ttu-id="eb92b-152">A állami vagy kormányzati felhőalapú területeket, használhat **AzureChinaCloudEnvironment**, **AzureUsGovernmentEnvrionment**, vagy **AzureGermanCloudEnvironment** kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="eb92b-152">For sovereign or government cloud regions, you can use **AzureChinaCloudEnvironment**, **AzureUsGovernmentEnvrionment**, or **AzureGermanCloudEnvironment** respectively.</span></span>

<span data-ttu-id="eb92b-153">Az alábbi példakód létrehoz egy tokent:</span><span class="sxs-lookup"><span data-stu-id="eb92b-153">The following code example creates a token:</span></span>
    
    var tokenCredentials = new AzureAdTokenCredentials("microsoft.onmicrosoft.com", AzureEnvironments.AzureCloudEnvironment);
    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
  
<span data-ttu-id="eb92b-154">Media Services elleni programozási elindításához kell létrehoznia egy **CloudMediaContext** példányt jelenti. a kiszolgáló a környezetben.</span><span class="sxs-lookup"><span data-stu-id="eb92b-154">To start programming against Media Services, you need to create a **CloudMediaContext** instance that represents the server context.</span></span> <span data-ttu-id="eb92b-155">A **CloudMediaContext** többek között a feladatok, eszközök, fájlok, hozzáférési házirendek és keresők fontos gyűjtemények mutató hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="eb92b-155">The **CloudMediaContext** includes references to important collections including jobs, assets, files, access policies, and locators.</span></span> 

<span data-ttu-id="eb92b-156">Is kell átadni a **erőforrás URI azonosítója a Media Services-REST** számára a **CloudMediaContext** konstruktor.</span><span class="sxs-lookup"><span data-stu-id="eb92b-156">You also need to pass the **resource URI for Media REST Services** to the **CloudMediaContext** constructor.</span></span> <span data-ttu-id="eb92b-157">Válassza ki ahhoz, hogy az erőforrás URI azonosítója, a többi médiaszolgáltatások, jelentkezzen be az Azure-portálon, válassza ki az Azure Media Services-fiók, **API-hozzáférés**, majd válassza ki **csatlakozás az Azure Media Services és a felhasználói hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="eb92b-157">To get the resource URI for Media REST Services, sign in to the Azure portal, select your Azure Media Services account, select **API access**, and then select **Connect to Azure Media Services with user authentication**.</span></span> 

<span data-ttu-id="eb92b-158">Az alábbi példakód létrehoz egy **CloudMediaContext** példány:</span><span class="sxs-lookup"><span data-stu-id="eb92b-158">The following code example creates a **CloudMediaContext** instance:</span></span>

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);

<span data-ttu-id="eb92b-159">A következő példa bemutatja, hogyan hozzon létre az Azure AD és a környezetben:</span><span class="sxs-lookup"><span data-stu-id="eb92b-159">The following example shows how to create the Azure AD token and the context:</span></span>

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
><span data-ttu-id="eb92b-160">Ha egy kivételt, amely szerint "a távoli kiszolgáló hibát adott vissza: (401) nem engedélyezett," tekintse meg a [hozzáférés-vezérlés](media-services-use-aad-auth-to-access-ams-api.md#access-control) Azure Media Services API elérése az Azure AD hitelesítési – Áttekintés szakasza.</span><span class="sxs-lookup"><span data-stu-id="eb92b-160">If you get an exception that says "The remote server returned an error: (401) Unauthorized," see the [Access control](media-services-use-aad-auth-to-access-ams-api.md#access-control) section of Accessing Azure Media Services API with Azure AD authentication overview.</span></span>

## <a name="use-service-principal-authentication"></a><span data-ttu-id="eb92b-161">Szolgáltatás egyszerű hitelesítés használata</span><span class="sxs-lookup"><span data-stu-id="eb92b-161">Use service principal authentication</span></span>
    
<span data-ttu-id="eb92b-162">Csatlakozás az Azure Media Services API a szolgáltatás egyszerű a beállítást, a középső rétegbeli alkalmazás (webes API-t vagy a webes alkalmazás) kell kérések az Azure AD-tokent a következő paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="eb92b-162">To connect to the Azure Media Services API with the service principal option, your middle-tier app (web API or web application) needs to requests an Azure AD token with the following parameters:</span></span>  

- <span data-ttu-id="eb92b-163">Azure AD-bérlő végpont.</span><span class="sxs-lookup"><span data-stu-id="eb92b-163">Azure AD tenant endpoint.</span></span> <span data-ttu-id="eb92b-164">A bérlői adatait az Azure portálról lehet beolvasni.</span><span class="sxs-lookup"><span data-stu-id="eb92b-164">The tenant information can be retrieved from the Azure portal.</span></span> <span data-ttu-id="eb92b-165">A jobb felső sarkában a bejelentkezett felhasználó mutasson.</span><span class="sxs-lookup"><span data-stu-id="eb92b-165">Hover over the signed-in user in the upper-right corner.</span></span>
- <span data-ttu-id="eb92b-166">A Media Services erőforrás URI azonosítója.</span><span class="sxs-lookup"><span data-stu-id="eb92b-166">Media Services resource URI.</span></span>
- <span data-ttu-id="eb92b-167">Az Azure AD alkalmazás értékeket: a **ügyfél-azonosító** és **ügyfélkulcs**.</span><span class="sxs-lookup"><span data-stu-id="eb92b-167">Azure AD application values: the **Client ID** and **Client secret**.</span></span>

<span data-ttu-id="eb92b-168">Az értékek a **ügyfél-azonosító** és **ügyfélkulcs** paraméterek az Azure portálon található.</span><span class="sxs-lookup"><span data-stu-id="eb92b-168">The values for the **Client ID** and **Client secret** parameters can be found in the Azure portal.</span></span> <span data-ttu-id="eb92b-169">További információkért lásd: [Ismerkedés az Azure AD-alapú hitelesítés az Azure portál használatával](media-services-portal-get-started-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="eb92b-169">For more information, see [Getting started with Azure AD authentication using the Azure portal](media-services-portal-get-started-with-aad.md).</span></span>

<span data-ttu-id="eb92b-170">Az alábbi példakód egy token használatával hozza létre a **AzureAdTokenCredentials** konstruktort **AzureAdClientSymmetricKey** paramétert:</span><span class="sxs-lookup"><span data-stu-id="eb92b-170">The following code example creates a token by using the **AzureAdTokenCredentials** constructor that takes **AzureAdClientSymmetricKey** as a parameter:</span></span> 
    
    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientSymmetricKey("{YOUR CLIENT ID HERE}", "{YOUR CLIENT SECRET}"), 
                                AzureEnvironments.AzureCloudEnvironment);

    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

<span data-ttu-id="eb92b-171">Azt is megadhatja a **AzureAdTokenCredentials** konstruktort **AzureAdClientCertificate** paraméterként.</span><span class="sxs-lookup"><span data-stu-id="eb92b-171">You can also specify the **AzureAdTokenCredentials** constructor that takes **AzureAdClientCertificate** as a parameter.</span></span> 

<span data-ttu-id="eb92b-172">Hozzon létre és tanúsítvány konfigurálása az Azure AD által felhasználható formában utasításokért lásd: [hitelesítéséhez az Azure AD-démon alkalmazásokban tanúsítványokkal - kézi konfigurálás lépéseit](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md).</span><span class="sxs-lookup"><span data-stu-id="eb92b-172">For instructions about how to create and configure a certificate in a form that can be used by Azure AD, see [Authenticating to Azure AD in daemon apps with certificates - manual configuration steps](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md).</span></span>

    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientCertificate("{YOUR CLIENT ID HERE}", "{YOUR CLIENT CERTIFICATE THUMBPRINT}"), 
                                AzureEnvironments.AzureCloudEnvironment);

<span data-ttu-id="eb92b-173">Media Services elleni programozási elindításához kell létrehoznia egy **CloudMediaContext** példányt jelenti. a kiszolgáló a környezetben.</span><span class="sxs-lookup"><span data-stu-id="eb92b-173">To start programming against Media Services, you need to create a **CloudMediaContext** instance that represents the server context.</span></span> <span data-ttu-id="eb92b-174">Is kell átadni a **erőforrás URI azonosítója a Media Services-REST** számára a **CloudMediaContext** konstruktor.</span><span class="sxs-lookup"><span data-stu-id="eb92b-174">You also need to pass the **resource URI for Media REST Services** to the **CloudMediaContext** constructor.</span></span> <span data-ttu-id="eb92b-175">Beszerezheti a **erőforrás URI azonosítója a Media Services-REST** érték is az Azure portálról.</span><span class="sxs-lookup"><span data-stu-id="eb92b-175">You can get the **resource URI for Media REST Services** value from the Azure portal as well.</span></span>

<span data-ttu-id="eb92b-176">Az alábbi példakód létrehoz egy **CloudMediaContext** példány:</span><span class="sxs-lookup"><span data-stu-id="eb92b-176">The following code example creates a **CloudMediaContext** instance:</span></span>

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
<span data-ttu-id="eb92b-177">A következő példa bemutatja, hogyan hozzon létre az Azure AD és a környezetben:</span><span class="sxs-lookup"><span data-stu-id="eb92b-177">The following example shows how to create the Azure AD token and the context:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="eb92b-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="eb92b-178">Next steps</span></span>

<span data-ttu-id="eb92b-179">Ismerkedés a [fájlok feltöltése a fiókjához](media-services-dotnet-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="eb92b-179">Get started with [uploading files to your account](media-services-dotnet-upload-files.md).</span></span>
