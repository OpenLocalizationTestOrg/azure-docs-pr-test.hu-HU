---
title: aaaUsing .NET SDK tooaccess Azure Mobile Engagement Service API-k
description: Ismerteti, hogyan toouse hello a Mobile Engagement .NET SDK tooaccess Azure Mobile Engagement Service API-k
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: c07728aa-43f2-4238-8b4a-c9eddf9d838b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 812be6f507b29f7b2de6454e06face8082c2d161
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-net-sdk-tooaccess-azure-mobile-engagement-service-apis"></a><span data-ttu-id="e605c-103">.NET SDK tooaccess Azure Mobile Engagement Service API-k használatával</span><span class="sxs-lookup"><span data-stu-id="e605c-103">Using .NET SDK tooaccess Azure Mobile Engagement Service APIs</span></span>
<span data-ttu-id="e605c-104">Az Azure Mobile Engagement tesz elérhetővé, az Ön toomanage eszközök API-k, Reach/leküldéses kampányokra stb toointeract ezen API-khoz, azt a adjon meg egy [Swagger-fájl](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) használható az eszközök számára a kívánt toogenerate SDK-k nyelv.</span><span class="sxs-lookup"><span data-stu-id="e605c-104">Azure Mobile Engagement exposes a set of APIs for you toomanage Devices, Reach/Push campaigns etc. toointeract with these APIs, we also provide you a [Swagger file](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) that you can use with tools toogenerate SDKs for your preferred language.</span></span> <span data-ttu-id="e605c-105">Azt javasoljuk, hello [AutoRest](https://github.com/Azure/AutoRest) eszköz toogenerate az SDK a Swagger-fájl.</span><span class="sxs-lookup"><span data-stu-id="e605c-105">We recommend using hello [AutoRest](https://github.com/Azure/AutoRest) tool toogenerate your SDK from our Swagger file.</span></span>

> [!NOTE]
> <span data-ttu-id="e605c-106">hello Azure Mobile Engagement szolgáltatás március 2018 rendszerből, és jelenleg csak a rendelkezésre álló tooexisting ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="e605c-106">hello Azure Mobile Engagement service will be retired March 2018 and is currently only available tooexisting customers.</span></span> <span data-ttu-id="e605c-107">További információkért lásd: [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="e605c-107">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

<span data-ttu-id="e605c-108">A .NET SDK létrehoztunk egy hasonló módon, amely lehetővé teszi a toointeract ezen API-khoz a C# burkoló használatával, és nem kell toodo hello hitelesítési jogkivonat egyeztetése révén, és frissítse magát.</span><span class="sxs-lookup"><span data-stu-id="e605c-108">We have created a .NET SDK in a similar manner which allows you toointeract with these APIs using a C# wrapper and you don't have toodo hello authentication token negotiation and refresh yourself.</span></span>  

<span data-ttu-id="e605c-109">Ez a minta végighalad lépéseket toofollow toouse hello .NET SDK hello készlete:</span><span class="sxs-lookup"><span data-stu-id="e605c-109">This sample goes through hello set of steps toofollow toouse hello .NET SDK:</span></span>

1. <span data-ttu-id="e605c-110">Az API-k használatával hello Azure Active Directory leírtak szerint az első lépésként kell toosetup hello hitelesítési [Itt](mobile-engagement-api-authentication.md#authentication).</span><span class="sxs-lookup"><span data-stu-id="e605c-110">First of all, you need toosetup hello authentication for your APIs using hello Azure Active Directory as described [here](mobile-engagement-api-authentication.md#authentication).</span></span> <span data-ttu-id="e605c-111">Ezeket a lépéseket hello végén, rendelkeznie kell egy érvényes **SubscriptionId**, **TenantId**, **ApplicationId** és **titkos**.</span><span class="sxs-lookup"><span data-stu-id="e605c-111">At hello end of these steps, you should have a valid **SubscriptionId**, **TenantId**, **ApplicationId** and **Secret**.</span></span> 
2. <span data-ttu-id="e605c-112">Egy bejelentési kampány létrehozásához hello forgatókönyvvel .NET SDK hello használata egyszerű Windows konzol app toodemonstrate használjuk.</span><span class="sxs-lookup"><span data-stu-id="e605c-112">We will use a simple Windows Console app toodemonstrate working with hello .NET SDK with hello scenario of creating an Announcement campaign.</span></span> <span data-ttu-id="e605c-113">Ezért nyissa meg a Visual Studio, és hozzon létre egy **Konzolalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e605c-113">So open up Visual Studio and create a **Console Application**.</span></span>   
3. <span data-ttu-id="e605c-114">Ezután kell-e toodownload hello elérhető .NET SDK **Microsoft Azure Engagement könyvtár** hello Nuget-dokumentumtárban [Itt](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).</span><span class="sxs-lookup"><span data-stu-id="e605c-114">Next you need toodownload hello .NET SDK which is available as **Microsoft Azure Engagement Management Library** in hello Nuget gallery [here](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).</span></span>
   <span data-ttu-id="e605c-115">A Visual Studio hello Nuget telepíti, ha szüksége van-e, hogy rendelkezik-e megjelölve hello jelölőnégyzet tooensure **Include prerelease** hello csomag keresése közben:</span><span class="sxs-lookup"><span data-stu-id="e605c-115">If you are installing hello Nuget from Visual Studio, you need tooensure that you have check marked hello **Include prerelease** option while searching for hello package:</span></span>
   
    ![][1]
4. <span data-ttu-id="e605c-116">A hello `Program.cs` fájlt, adja hozzá a következő névterek hello:</span><span class="sxs-lookup"><span data-stu-id="e605c-116">In hello `Program.cs` file, add hello following namespaces:</span></span>
   
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.Engagement;
        using Microsoft.Azure.Management.Engagement.Models;
5. <span data-ttu-id="e605c-117">Ezután a következő konstans, amely a hitelesítéshez használjuk, és hello Mobile Engagement-alkalmazáshoz hello közlemény kampány létrehozásához való interakció toodefine hello lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="e605c-117">Next you need toodefine hello following constants that we will use for authentication and interacting with hello Mobile Engagement App in which you are creating hello Announcement campaign:</span></span>
   
        // For authentication
        const string TENANT_ID = "<Your TenantId>";
        const string CLIENT_ID = "<Your Application Id>";
        const string CLIENT_SECRET = "<Your Secret>";
        const string SUBSCRIPTION_ID = "<Your Subscription Id>";
   
        // This is hello Azure Resource group concept for grouping together resources 
        //  see here: https://azure.microsoft.com/en-us/documentation/articles/resource-group-portal/
        const string RESOURCE_GROUP = "";
   
        // For Mobile Engagement operations
        // App Collection Name 
        const string APP_COLLECTION_NAME = "";
        // Application Resource Name - make sure you are using hello one as specified in hello Azure portal (NOT hello App Name)
        const string APP_RESOURCE_NAME = "";
6. <span data-ttu-id="e605c-118">Adja meg a hello `EngagementManagementClient` változót, amely használjuk toocall hello Mobile Engagement SDK módszerek:</span><span class="sxs-lookup"><span data-stu-id="e605c-118">Define hello `EngagementManagementClient` variable which we will use toocall hello Mobile Engagement SDK methods:</span></span>
   
        static EngagementManagementClient engagementClient; 
7. <span data-ttu-id="e605c-119">Adja hozzá a következő tooyour hello `Main` módszert:</span><span class="sxs-lookup"><span data-stu-id="e605c-119">Add hello following tooyour `Main` method:</span></span>
   
        try
            {
                // Initialize hello Engagement SDK toocall out APIs. 
                InitEngagementClient().Wait();
   
                // Create a Reach campaign
                CreateCampaign().Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.InnerException.Message);
                throw ex;
            }
8. <span data-ttu-id="e605c-120">Adja meg a következő metódus, amely gondoskodik hello inicializálása hello `EngagementManagementClient` először hitelesíti, és majd maga hello Mobile Engagement-alkalmazáshoz tervezi toocreate hello közlemény kampány társítása:</span><span class="sxs-lookup"><span data-stu-id="e605c-120">Define hello following method which takes care of initializing hello `EngagementManagementClient` by first authenticating and then associating itself with hello Mobile Engagement App in which you plan toocreate hello Announcement campaign:</span></span>
   
        private static async Task InitEngagementClient()
        {
            var credentials = await ApplicationTokenProvider.LoginSilentAsync(TENANT_ID, CLIENT_ID, CLIENT_SECRET);
            engagementClient = new EngagementManagementClient(credentials) { SubscriptionId = SUBSCRIPTION_ID };
   
            // This is hello Azure concept of ResourceGroup
            engagementClient.ResourceGroupName = RESOURCE_GROUP;
   
            // This is your Mobile Engagement App Collection & App Resource Name in which you create hello campaign
            engagementClient.AppCollection = APP_COLLECTION_NAME;
            engagementClient.AppName = APP_RESOURCE_NAME;
        }
   
   > [!IMPORTANT]
   > <span data-ttu-id="e605c-121">Vegye figyelembe, hogy kell-e toouse hello **erőforrás alkalmazásnév** hello Azure felügyeleti portálján hello AppName paraméter meghatározottak szerint.</span><span class="sxs-lookup"><span data-stu-id="e605c-121">Note that you need toouse hello **App Resource Name** as defined in hello Azure management portal for hello AppName parameter.</span></span> 
   > 
   > 
9. <span data-ttu-id="e605c-122">Végül adja meg a korábban inicializálták hello EngagementClient toocreate egy egyszerű használatával kezeli hello CreateCampaign metódust **bármikor** & **csak értesítési** kampány a cím és üzenet:</span><span class="sxs-lookup"><span data-stu-id="e605c-122">Lastly, define hello CreateCampaign method which will take care of using hello previously initialized EngagementClient toocreate a simple **AnyTime** & **Notification-only** campaign with a title and message:</span></span> 
   
        private async static Task CreateCampaign()
        {
            //  Refer toohello Announcement Campaign format from here - 
            //      https://msdn.microsoft.com/en-us/library/azure/mt683751.aspx
            // Make sure you are passing all hello non-optional parameters
            Campaign parameters = new Campaign(
                name:"WelcomeCampaign",
                notificationTitle: "Welcome", 
                notificationMessage: "Thank you for installing hello app!",
                type:"only_notif",
                deliveryTime:"any"
                );
   
            // Refer toohello Campaign Kinds from here - https://msdn.microsoft.com/en-us/library/azure/mt683742.aspx
            CampaignStateResult result = 
                await engagementClient.Campaigns.CreateAsync(CampaignKinds.Announcements, parameters);
            Console.WriteLine("Campaign Id '{0}' was created successfully and it is in '{1}' state", result.Id, result.State);
        }
10. <span data-ttu-id="e605c-123">Futtatási hello Konzolalkalmazás, és hello következő megjelenik a sikeres létrehozást hello kampány:</span><span class="sxs-lookup"><span data-stu-id="e605c-123">Run hello console app and you will see hello following on successful creation of hello campaign:</span></span>
    
    <span data-ttu-id="e605c-124">**"159" Kampányazonosítóval sikeresen létrejött, és a "vázlat" állapotban van**</span><span class="sxs-lookup"><span data-stu-id="e605c-124">**Campaign Id '159' was created successfully and it is in 'draft' state**</span></span>

<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png
