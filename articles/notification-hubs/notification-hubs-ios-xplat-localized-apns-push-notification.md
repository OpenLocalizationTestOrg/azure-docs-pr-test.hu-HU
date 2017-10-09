---
title: "aaaNotification hubok honosított Megtörje hírek oktatóanyag iOS-hez"
description: "Ismerje meg, hogyan toouse Azure Service Bus Notification Hubs toosend honosított legfrissebb híreket tartalmazó értesítések (iOS)."
services: notification-hubs
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
ms.assetid: 484914b5-e081-4a05-a84a-798bbd89d428
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 9fe88c0440e93b72d349574160ddcd85a7ba0be0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-notification-hubs-toosend-localized-breaking-news-tooios-devices"></a><span data-ttu-id="b312a-103">Használja a Notification Hubs honosított toosend breaking news tooiOS eszközök</span><span class="sxs-lookup"><span data-stu-id="b312a-103">Use Notification Hubs toosend localized breaking news tooiOS devices</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b312a-104">Windows áruház C#</span><span class="sxs-lookup"><span data-stu-id="b312a-104">Windows Store C#</span></span>](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [<span data-ttu-id="b312a-105">iOS</span><span class="sxs-lookup"><span data-stu-id="b312a-105">iOS</span></span>](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="b312a-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b312a-106">Overview</span></span>
<span data-ttu-id="b312a-107">Ez a témakör bemutatja, hogyan toouse hello [sablonok](notification-hubs-templates-cross-platform-push-messages.md) az Azure Notification Hubs toobroadcast nyelv és az eszköz honosított katalóguselemekről híreket tartalmazó értesítések megtörje szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="b312a-107">This topic shows you how toouse hello [templates](notification-hubs-templates-cross-platform-push-messages.md) feature of Azure Notification Hubs toobroadcast breaking news notifications that have been localized by language and device.</span></span> <span data-ttu-id="b312a-108">Ez az oktatóanyag a kiindulási pont hello iOS-alkalmazás létrehozása [legfrissebb hírek Notification Hubs használata toosend].</span><span class="sxs-lookup"><span data-stu-id="b312a-108">In this tutorial you start with hello iOS app created in [Use Notification Hubs toosend breaking news].</span></span> <span data-ttu-id="b312a-109">Amikor végzett, képes tooregister érdekli kategóriákban fogja, adjon meg egy nyelvi mely tooreceive hello értesítések, és csak a kiválasztott hello kategóriák leküldéses értesítések fogadásához az adott nyelveken.</span><span class="sxs-lookup"><span data-stu-id="b312a-109">When complete, you will be able tooregister for categories you are interested in, specify a language in which tooreceive hello notifications, and receive only push notifications for hello selected categories in that language.</span></span>

<span data-ttu-id="b312a-110">Nincsenek két részből toothis forgatókönyv:</span><span class="sxs-lookup"><span data-stu-id="b312a-110">There are two parts toothis scenario:</span></span>

* <span data-ttu-id="b312a-111">iOS-alkalmazás lehetővé teszi, hogy ügyfél eszközök toospecify egy nyelvet, és toosubscribe toodifferent megtörje hírek kategóriák;</span><span class="sxs-lookup"><span data-stu-id="b312a-111">iOS app allows client devices toospecify a language, and toosubscribe toodifferent breaking news categories;</span></span>
* <span data-ttu-id="b312a-112">hello háttér-közzéteszi hello értesítéseket, hello **címke** és **sablon** az Azure Notification Hubs feautres.</span><span class="sxs-lookup"><span data-stu-id="b312a-112">hello back-end broadcasts hello notifications, using hello **tag** and **template** feautres of Azure Notification Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b312a-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b312a-113">Prerequisites</span></span>
<span data-ttu-id="b312a-114">Már végrehajtotta hello [legfrissebb hírek Notification Hubs használata toosend] oktatóanyag és hello kód érhető el, mert ez az oktatóanyag közvetlenül épít, hogy a kód.</span><span class="sxs-lookup"><span data-stu-id="b312a-114">You must have already completed hello [Use Notification Hubs toosend breaking news] tutorial and have hello code available, because this tutorial builds directly upon that code.</span></span>

<span data-ttu-id="b312a-115">A Visual Studio 2012 vagy újabb nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="b312a-115">Visual Studio 2012 or later is optional.</span></span>

## <a name="template-concepts"></a><span data-ttu-id="b312a-116">Sablon fogalmak</span><span class="sxs-lookup"><span data-stu-id="b312a-116">Template concepts</span></span>
<span data-ttu-id="b312a-117">A [legfrissebb hírek Notification Hubs használata toosend] olyan alkalmazás, amelynek használt parancsfájlkezelő **címkék** toosubscribe toonotifications hírek különböző kategóriákban.</span><span class="sxs-lookup"><span data-stu-id="b312a-117">In [Use Notification Hubs toosend breaking news] you built an app that used **tags** toosubscribe toonotifications for different news categories.</span></span>
<span data-ttu-id="b312a-118">Számos alkalmazás, azonban több piacok célként, és honosítási igényelnek.</span><span class="sxs-lookup"><span data-stu-id="b312a-118">Many apps, however, target multiple markets and require localization.</span></span> <span data-ttu-id="b312a-119">Ez azt jelenti, hogy maguk hello értesítések hello tartalmának rendelkezik honosított toobe kézbesített toohello, javítsa ki az eszközök.</span><span class="sxs-lookup"><span data-stu-id="b312a-119">This means that hello content of hello notifications themselves have toobe localized and delivered toohello correct set of devices.</span></span>
<span data-ttu-id="b312a-120">Ebben a témakörben bemutatjuk a hogyan toouse hello **sablon** tooeasily biztosítanak a Notification Hubs szolgáltatása honosított legfrissebb híreket tartalmazó értesítések.</span><span class="sxs-lookup"><span data-stu-id="b312a-120">In this topic we will show how toouse hello **template** feature of Notification Hubs tooeasily deliver localized breaking news notifications.</span></span>

<span data-ttu-id="b312a-121">Megjegyzés: egyirányú toosend honosított értesítések toocreate minden címke több verziója van.</span><span class="sxs-lookup"><span data-stu-id="b312a-121">Note: one way toosend localized notifications is toocreate multiple versions of each tag.</span></span> <span data-ttu-id="b312a-122">Például toosupport angol, francia és Mandarin, lenne szükséges három különböző címkék híreket: "world_en", "world_fr" és "world_ch".</span><span class="sxs-lookup"><span data-stu-id="b312a-122">For instance, toosupport English, French, and Mandarin, we would need three different tags for world news: "world_en", "world_fr", and "world_ch".</span></span> <span data-ttu-id="b312a-123">A Microsoft majd kellene toosend hello world hírek tooeach címkéket honosított verzióját.</span><span class="sxs-lookup"><span data-stu-id="b312a-123">We would then have toosend a localized version of hello world news tooeach of these tags.</span></span> <span data-ttu-id="b312a-124">Ez a témakör használjuk, a címkéket a sablonok tooavoid hello elterjedése és hello követelmény több üzenetet küldeni.</span><span class="sxs-lookup"><span data-stu-id="b312a-124">In this topic we use templates tooavoid hello proliferation of tags and hello requirement of sending multiple messages.</span></span>

<span data-ttu-id="b312a-125">Magas szinten, a sablonok olyan módon toospecify hogyan egy adott eszközhöz egy értesítést kell kapnia.</span><span class="sxs-lookup"><span data-stu-id="b312a-125">At a high level, templates are a way toospecify how a specific device should receive a notification.</span></span> <span data-ttu-id="b312a-126">hello sablon hello pontos az adattartalom formátuma hello által küldött üzenet az alkalmazás háttér-részét képező tooproperties alapján határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b312a-126">hello template specifies hello exact payload format by referring tooproperties that are part of hello message sent by your app back-end.</span></span> <span data-ttu-id="b312a-127">Ebben az esetben az összes támogatott nyelvek tartalmazó területibeállítás-független üzenetet küldünk:</span><span class="sxs-lookup"><span data-stu-id="b312a-127">In our case, we will send a locale-agnostic message containing all supported languages:</span></span>

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

<span data-ttu-id="b312a-128">Ezután azt fogja győződjön meg arról, hogy az eszközök regisztrálása a sablont, amely hivatkozik a toohello megfelelő tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b312a-128">Then we will ensure that devices register with a template that refers toohello correct property.</span></span> <span data-ttu-id="b312a-129">Például egy iOS-alkalmazást, amely a francia hírek tooregister szeretne regisztrálni fogja hello következő:</span><span class="sxs-lookup"><span data-stu-id="b312a-129">For instance,  an iOS app that wants tooregister for French news will register hello following:</span></span>

    {
        aps:{
            alert: "$(News_French)"
        }
    }

<span data-ttu-id="b312a-130">Sablonok egyik újdonsága nagyon hatékony többet is megtudhat arról a a [sablonok](notification-hubs-templates-cross-platform-push-messages.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b312a-130">Templates are a very powerful feature you can learn more about in our [Templates](notification-hubs-templates-cross-platform-push-messages.md) article.</span></span>

## <a name="hello-app-user-interface"></a><span data-ttu-id="b312a-131">hello alkalmazás felhasználói felülete</span><span class="sxs-lookup"><span data-stu-id="b312a-131">hello app user interface</span></span>
<span data-ttu-id="b312a-132">Most módosítja, a Microsoft hello Megtörje hírek app hello a témakör a [legfrissebb hírek Notification Hubs használata toosend] toosend honosított legfrissebb hírek sablonok használatával.</span><span class="sxs-lookup"><span data-stu-id="b312a-132">We will now modify hello Breaking News app that you created in hello topic [Use Notification Hubs toosend breaking news] toosend localized breaking news using templates.</span></span>

<span data-ttu-id="b312a-133">A MainStoryboard_iPhone.storyboard hello három nyelv, amely támogatjuk szegmentált vezérlőt hozzá: angol, francia és Mandarin.</span><span class="sxs-lookup"><span data-stu-id="b312a-133">In your MainStoryboard_iPhone.storyboard, add a Segmented Control with hello three languages which we will support: English, French, and Mandarin.</span></span>

![][13]

<span data-ttu-id="b312a-134">Győződjön meg arról, hogy tooadd egy IBOutlet a ViewController.h az alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="b312a-134">Then make sure tooadd an IBOutlet in your ViewController.h as shown below:</span></span>

![][14]

## <a name="building-hello-ios-app"></a><span data-ttu-id="b312a-135">Épület hello iOS-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="b312a-135">Building hello iOS app</span></span>
1. <span data-ttu-id="b312a-136">A Notification.h hozzá hello *retrieveLocale* módszer módosításához hello tárolóban, és az előfizetés módszerek alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="b312a-136">In your Notification.h add hello *retrieveLocale* method, and modify hello store and subscribe methods as shown below:</span></span>
   
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;
   
        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;
   
        - (NSSet*) retrieveCategories;
   
        - (int) retrieveLocale;
   
    <span data-ttu-id="b312a-137">Módosítsa a Notification.m hello *storeCategoriesAndSubscribe* metódus hello területi beállítással rendelkezik, és tárolja őket hello felhasználók alapértelmezett beállításait:</span><span class="sxs-lookup"><span data-stu-id="b312a-137">In your Notification.m, modify hello *storeCategoriesAndSubscribe* method, by adding hello locale parameter and storing it in hello user defaults:</span></span>
   
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];
   
            [self subscribeWithLocale: locale categories:categories completion:completion];
        }
   
    <span data-ttu-id="b312a-138">Módosítsa a hello *előfizetés* metódus tooinclude hello területi beállítás:</span><span class="sxs-lookup"><span data-stu-id="b312a-138">Then modify hello *subscribe* method tooinclude hello locale:</span></span>
   
        - (void) subscribeWithLocale: (int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion{
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<connection string>" notificationHubPath:@"<hub name>"];
   
            NSString* localeString;
            switch (locale) {
                case 0:
                    localeString = @"English";
                    break;
                case 1:
                    localeString = @"French";
                    break;
                case 2:
                    localeString = @"Mandarin";
                    break;
            }
   
            NSString* template = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"$(News_%@)\"},\"inAppMessage\":\"$(News_%@)\"}", localeString, localeString];
   
            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"localizednewsTemplate" jsonBodyTemplate:template expiryTemplate:@"0" tags:categories completion:completion];
        }
   
    <span data-ttu-id="b312a-139">Vegye figyelembe, hogyan most használjuk hello metódus *registerTemplateWithDeviceToken*, ahelyett, hogy *registerNativeWithDeviceToken*.</span><span class="sxs-lookup"><span data-stu-id="b312a-139">Note how we are now using hello method *registerTemplateWithDeviceToken*, instead of *registerNativeWithDeviceToken*.</span></span> <span data-ttu-id="b312a-140">Ha a sablon regisztráljuk tudunk tooprovide hello json-sablon, és hello sablon nevét (az alkalmazásnak érdemes tooregister különböző sablonok).</span><span class="sxs-lookup"><span data-stu-id="b312a-140">When we register for a template we have tooprovide hello json template and also a name for hello template (as our app might want tooregister different templates).</span></span> <span data-ttu-id="b312a-141">Győződjön meg arról, hogy tooregister címkéket, mint a kategóriák, az adott hírek szeretnénk toomake meg arról, hogy tooreceive hello notifciations.</span><span class="sxs-lookup"><span data-stu-id="b312a-141">Make sure tooregister your categories as tags, as we want toomake sure tooreceive hello notifciations for those news.</span></span>
   
    <span data-ttu-id="b312a-142">Adja hozzá a metódus tooretrieve hello területi hello felhasználó alapértelmezett beállításai közül:</span><span class="sxs-lookup"><span data-stu-id="b312a-142">Add a method tooretrieve hello locale from hello user default settings:</span></span>
   
        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
   
            int locale = [defaults integerForKey:@"BreakingNewsLocale"];
   
            return locale < 0?0:locale;
        }
2. <span data-ttu-id="b312a-143">Most, hogy az értesítések osztály módosított azt kell arról, hogy a ViewController teszi toomake hello használata új UISegmentControl.</span><span class="sxs-lookup"><span data-stu-id="b312a-143">Now that we modified our Notifications class, we have toomake sure that our ViewController makes use of hello new UISegmentControl.</span></span> <span data-ttu-id="b312a-144">Adja hozzá a következő sort a hello hello *viewDidLoad* metódus toomake meg arról, hogy tooshow hello területi beállítás a kiválasztott:</span><span class="sxs-lookup"><span data-stu-id="b312a-144">Add hello following line in hello *viewDidLoad* method toomake sure tooshow hello locale that is currently selected:</span></span>
   
        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];
   
    <span data-ttu-id="b312a-145">Ezt követően a a *előfizetés* módszer, módosítsa a hívás toohello *storeCategoriesAndSubscribe* toohello következő:</span><span class="sxs-lookup"><span data-stu-id="b312a-145">Then, in your *subscribe* method, change your call toohello *storeCategoriesAndSubscribe* toohello following:</span></span>
   
        [notifications storeCategoriesAndSubscribeWithLocale: self.Locale.selectedSegmentIndex categories:[NSSet setWithArray:categories] completion: ^(NSError* error) {
            if (!error) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                      @"Subscribed!" delegate:nil cancelButtonTitle:
                                      @"OK" otherButtonTitles:nil, nil];
                [alert show];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];
3. <span data-ttu-id="b312a-146">Végül ott tooupdate hello *didRegisterForRemoteNotificationsWithDeviceToken* a AppDelegate.m metódust, hogy helyesen, is frissítheti a regisztrációt, az alkalmazás indításakor.</span><span class="sxs-lookup"><span data-stu-id="b312a-146">Finally, you have tooupdate hello *didRegisterForRemoteNotificationsWithDeviceToken* method in your AppDelegate.m, so that you can correctly refresh your registration when your app starts.</span></span> <span data-ttu-id="b312a-147">Módosítsa a hívás toohello *előfizetés* hello következőre értesítések módszert:</span><span class="sxs-lookup"><span data-stu-id="b312a-147">Change your call toohello *subscribe* method of notifications with hello following:</span></span>
   
        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

## <a name="optional-send-localized-template-notifications-from-net-console-app"></a><span data-ttu-id="b312a-148">(választható) .NET-Konzolalkalmazás honosított sablonhoz értesítésküldést.</span><span class="sxs-lookup"><span data-stu-id="b312a-148">(optional) Send localized template notifications from .NET console app.</span></span>
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

## <a name="optional-send-localized-template-notifications-from-hello-device"></a><span data-ttu-id="b312a-149">(választható) Honosított sablonhoz értesítésküldést hello eszköz</span><span class="sxs-lookup"><span data-stu-id="b312a-149">(optional) Send localized template notifications from hello device</span></span>
<span data-ttu-id="b312a-150">Ha nem access tooVisual Studio, esetleg toojust teszt honosított hello sablon értesítések küldése közvetlenül hello alkalmazásból hello eszközön.</span><span class="sxs-lookup"><span data-stu-id="b312a-150">If you don't have access tooVisual Studio, or want toojust test sending hello localized template notifications directly from hello app on hello device.</span></span>  <span data-ttu-id="b312a-151">Egyszerű is vegye fel a honosított hello sablon paraméterek toohello `SendNotificationRESTAPI` hello az oktatóanyag előző meghatározott metódus.</span><span class="sxs-lookup"><span data-stu-id="b312a-151">You can simple add hello localized template parameters toohello `SendNotificationRESTAPI` method you defined in hello previous tutorial.</span></span>

        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];

            NSString *json;

            // Construct hello messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];

            // Generated hello token toobe used in hello authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create hello request tooadd hello template notification message toohello hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];

            // Add hello category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];

            // Template notification
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\","
                    \"News_English\":\"Breaking %@ News in English : %@\","
                    \"News_French\":\"Breaking %@ News in French : %@\","
                    \"News_Mandarin\":\"Breaking %@ News in Mandarin : %@\","
                    categoryTag, self.notificationMessage.text,
                    categoryTag, self.notificationMessage.text,  // insert English localized news here
                    categoryTag, self.notificationMessage.text,  // insert French localized news here
                    categoryTag, self.notificationMessage.text]; // insert Mandarin localized news here

            // Signify template notification format
            [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            // JSON Content-Type
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            //Authenticate hello notification message POST request with hello SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add hello notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send hello REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                       completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
               {
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                   if (error || httpResponse.statusCode != 200)
                   {
                       NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                   }
                   if (data != NULL)
                   {
                       //xmlParser = [[NSXMLParser alloc] initWithData:data];
                       //[xmlParser setDelegate:self];
                       //[xmlParser parse];
                   }
               }];

            [dataTask resume];
        }




## <a name="next-steps"></a><span data-ttu-id="b312a-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b312a-152">Next Steps</span></span>
<span data-ttu-id="b312a-153">Sablonok használatával kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="b312a-153">For more information on using templates, see:</span></span>

* <span data-ttu-id="b312a-154">[Értesítse a felhasználókat a Notification hubs használatával: ASP.NET]</span><span class="sxs-lookup"><span data-stu-id="b312a-154">[Notify users with Notification Hubs: ASP.NET]</span></span>
* <span data-ttu-id="b312a-155">[Értesítse a felhasználókat a Notification hubs használatával: Mobile Services]</span><span class="sxs-lookup"><span data-stu-id="b312a-155">[Notify users with Notification Hubs: Mobile Services]</span></span>

<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[legfrissebb hírek Notification Hubs használata toosend]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Értesítse a felhasználókat a Notification hubs használatával: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Értesítse a felhasználókat a Notification hubs használatával: Mobile Services]: /manage/services/notification-hubs/notify-users
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications tooapp users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md

[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-toofor iOS]: http://msdn.microsoft.com/library/jj927168.aspx
