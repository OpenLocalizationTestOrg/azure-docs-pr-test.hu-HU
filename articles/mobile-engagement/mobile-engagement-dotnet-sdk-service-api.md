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
# <a name="using-net-sdk-tooaccess-azure-mobile-engagement-service-apis"></a>.NET SDK tooaccess Azure Mobile Engagement Service API-k használatával
Az Azure Mobile Engagement tesz elérhetővé, az Ön toomanage eszközök API-k, Reach/leküldéses kampányokra stb toointeract ezen API-khoz, azt a adjon meg egy [Swagger-fájl](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) használható az eszközök számára a kívánt toogenerate SDK-k nyelv. Azt javasoljuk, hello [AutoRest](https://github.com/Azure/AutoRest) eszköz toogenerate az SDK a Swagger-fájl.

> [!NOTE]
> hello Azure Mobile Engagement szolgáltatás március 2018 rendszerből, és jelenleg csak a rendelkezésre álló tooexisting ügyfelek. További információkért lásd: [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

A .NET SDK létrehoztunk egy hasonló módon, amely lehetővé teszi a toointeract ezen API-khoz a C# burkoló használatával, és nem kell toodo hello hitelesítési jogkivonat egyeztetése révén, és frissítse magát.  

Ez a minta végighalad lépéseket toofollow toouse hello .NET SDK hello készlete:

1. Az API-k használatával hello Azure Active Directory leírtak szerint az első lépésként kell toosetup hello hitelesítési [Itt](mobile-engagement-api-authentication.md#authentication). Ezeket a lépéseket hello végén, rendelkeznie kell egy érvényes **SubscriptionId**, **TenantId**, **ApplicationId** és **titkos**. 
2. Egy bejelentési kampány létrehozásához hello forgatókönyvvel .NET SDK hello használata egyszerű Windows konzol app toodemonstrate használjuk. Ezért nyissa meg a Visual Studio, és hozzon létre egy **Konzolalkalmazás**.   
3. Ezután kell-e toodownload hello elérhető .NET SDK **Microsoft Azure Engagement könyvtár** hello Nuget-dokumentumtárban [Itt](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).
   A Visual Studio hello Nuget telepíti, ha szüksége van-e, hogy rendelkezik-e megjelölve hello jelölőnégyzet tooensure **Include prerelease** hello csomag keresése közben:
   
    ![][1]
4. A hello `Program.cs` fájlt, adja hozzá a következő névterek hello:
   
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.Engagement;
        using Microsoft.Azure.Management.Engagement.Models;
5. Ezután a következő konstans, amely a hitelesítéshez használjuk, és hello Mobile Engagement-alkalmazáshoz hello közlemény kampány létrehozásához való interakció toodefine hello lesz szüksége:
   
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
6. Adja meg a hello `EngagementManagementClient` változót, amely használjuk toocall hello Mobile Engagement SDK módszerek:
   
        static EngagementManagementClient engagementClient; 
7. Adja hozzá a következő tooyour hello `Main` módszert:
   
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
8. Adja meg a következő metódus, amely gondoskodik hello inicializálása hello `EngagementManagementClient` először hitelesíti, és majd maga hello Mobile Engagement-alkalmazáshoz tervezi toocreate hello közlemény kampány társítása:
   
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
   > Vegye figyelembe, hogy kell-e toouse hello **erőforrás alkalmazásnév** hello Azure felügyeleti portálján hello AppName paraméter meghatározottak szerint. 
   > 
   > 
9. Végül adja meg a korábban inicializálták hello EngagementClient toocreate egy egyszerű használatával kezeli hello CreateCampaign metódust **bármikor** & **csak értesítési** kampány a cím és üzenet: 
   
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
10. Futtatási hello Konzolalkalmazás, és hello következő megjelenik a sikeres létrehozást hello kampány:
    
    **"159" Kampányazonosítóval sikeresen létrejött, és a "vázlat" állapotban van**

<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png
