---
title: "aaaNotification hubok Megtörje hírek oktatóanyag – iOS"
description: "Megtudhatja, hogyan toouse Azure Service Bus Notification Hubs toosend megtörje hírek értesítések tooiOS eszközök."
services: notification-hubs
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
ms.assetid: 6ead4169-deff-4947-858c-8c6cf03cc3b2
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 763b80b5ffed238b351d95bd3d6a96cb914f53cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-notification-hubs-toosend-breaking-news"></a><span data-ttu-id="464dc-103">Használja a Notification Hubs toosend legfrissebb hírek</span><span class="sxs-lookup"><span data-stu-id="464dc-103">Use Notification Hubs toosend breaking news</span></span>
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a><span data-ttu-id="464dc-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="464dc-104">Overview</span></span>
<span data-ttu-id="464dc-105">Ez a témakör bemutatja, hogyan toouse Azure Notification Hubs toobroadcast breaking news értesítések tooan iOS-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="464dc-105">This topic shows you how toouse Azure Notification Hubs toobroadcast breaking news notifications tooan iOS app.</span></span> <span data-ttu-id="464dc-106">Amikor végzett, akkor tudja tooregister számára megtörje hírek kategóriák érdekli, és ezen kategóriák csak leküldéses értesítések fogadásához.</span><span class="sxs-lookup"><span data-stu-id="464dc-106">When complete, you will be able tooregister for breaking news categories you are interested in, and receive only push notifications for those categories.</span></span> <span data-ttu-id="464dc-107">Ebben a forgatókönyvben számos alkalmazás általános felépítését, amelyben értesítések vannak küldött toobe toogroups, amely rendelkezik deklarálva érdeklődési rajtuk, például az RSS-olvasóval, az alkalmazások zene ventilátorok stb.</span><span class="sxs-lookup"><span data-stu-id="464dc-107">This scenario is a common pattern for many apps where notifications have toobe sent toogroups of users that have previously declared interest in them, e.g. RSS reader, apps for music fans, etc.</span></span>

<span data-ttu-id="464dc-108">Szórási forgatókönyvek engedélyezve vannak, beleértve a következőket egy vagy több *címkék* hello értesítési központ regisztráció létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="464dc-108">Broadcast scenarios are enabled by including one or more *tags* when creating a registration in hello notification hub.</span></span> <span data-ttu-id="464dc-109">Ha az értesítések küldése tooa címke hello címke regisztrált minden eszköz hello értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="464dc-109">When notifications are sent tooa tag, all devices that have registered for hello tag will receive hello notification.</span></span> <span data-ttu-id="464dc-110">Mivel a címkékkel egyszerűen csak karakterláncok, nincs kiépítve előzetes toobe.</span><span class="sxs-lookup"><span data-stu-id="464dc-110">Because tags are simply strings, they do not have toobe provisioned in advance.</span></span> <span data-ttu-id="464dc-111">Címkékkel kapcsolatos további információkért tekintse meg túl[Notification Hubs útválasztási és címke kifejezések](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="464dc-111">For more information about tags, refer too[Notification Hubs Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="464dc-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="464dc-112">Prerequisites</span></span>
<span data-ttu-id="464dc-113">Ebben a témakörben megfogalmazott célra épül létrehozott hello app [Ismerkedés a Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="464dc-113">This topic builds on hello app you created in [Get started with Notification Hubs][get-started].</span></span> <span data-ttu-id="464dc-114">Az oktatóanyag elindítása előtt már végrehajtotta [Ismerkedés a Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="464dc-114">Before starting this tutorial, you must have already completed [Get started with Notification Hubs][get-started].</span></span>

## <a name="add-category-selection-toohello-app"></a><span data-ttu-id="464dc-115">Kategória kiválasztása toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="464dc-115">Add category selection toohello app</span></span>
<span data-ttu-id="464dc-116">első lépés hello tooadd hello felhasználói felületi elemek tooyour meglévő storyboard, amelyek lehetővé teszik a hello felhasználói tooselect kategóriák tooregister.</span><span class="sxs-lookup"><span data-stu-id="464dc-116">hello first step is tooadd hello UI elements tooyour existing storyboard that enable hello user tooselect categories tooregister.</span></span> <span data-ttu-id="464dc-117">felhasználó által kijelölt hello kategóriák hello eszközön tárolja.</span><span class="sxs-lookup"><span data-stu-id="464dc-117">hello categories selected by a user are stored on hello device.</span></span> <span data-ttu-id="464dc-118">Hello alkalmazás indításakor a eszközregisztráció címkeként az értesítési központ kijelölt hello kategóriákhoz jön létre.</span><span class="sxs-lookup"><span data-stu-id="464dc-118">When hello app starts, a device registration is created in your notification hub with hello selected categories as tags.</span></span>

1. <span data-ttu-id="464dc-119">Adja hozzá a MainStoryboard_iPhone.storyboard hello összetevők hello objektum könyvtárból a következő:</span><span class="sxs-lookup"><span data-stu-id="464dc-119">In your MainStoryboard_iPhone.storyboard add hello following components from hello object library:</span></span>
   
   * <span data-ttu-id="464dc-120">A címkék "Megtörje hírek" szóra,</span><span class="sxs-lookup"><span data-stu-id="464dc-120">A label with "Breaking News" text,</span></span>
   * <span data-ttu-id="464dc-121">Címkék kategória szöveget a "World", "Politika", "Vállalati", "Technológia", "Tudományos", "Sport",</span><span class="sxs-lookup"><span data-stu-id="464dc-121">Labels with category texts "World", "Politics", "Business", "Technology", "Science", "Sports",</span></span>
   * <span data-ttu-id="464dc-122">Egy kategóriát, egy hat kapcsolók beállítása minden kapcsoló **állapot** toobe **ki** alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="464dc-122">Six switches, one per category, set each switch **State** toobe **Off** by default.</span></span>
   * <span data-ttu-id="464dc-123">Egy gomb "Előfizetés" címkével</span><span class="sxs-lookup"><span data-stu-id="464dc-123">One button labeled "Subscribe"</span></span>
     
     <span data-ttu-id="464dc-124">A storyboard fájlt a következőképpen kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="464dc-124">Your storyboard should look as follows:</span></span>
     
     ![][3]
2. <span data-ttu-id="464dc-125">Hello Segéd-szerkesztőben kimeneteket összes hello kapcsolók létrehozása, és hívja meg őket "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"</span><span class="sxs-lookup"><span data-stu-id="464dc-125">In hello assistant editor, create outlets for all hello switches and call them "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"</span></span>
3. <span data-ttu-id="464dc-126">Az "előfizetés" gombra a művelet létrehozása.</span><span class="sxs-lookup"><span data-stu-id="464dc-126">Create an Action for your button called "subscribe".</span></span> <span data-ttu-id="464dc-127">A ViewController.h hello következőket kell tartalmaznia:</span><span class="sxs-lookup"><span data-stu-id="464dc-127">Your ViewController.h should contain hello following:</span></span>
   
        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;
   
        - (IBAction)subscribe:(id)sender;
4. <span data-ttu-id="464dc-128">Hozzon létre egy új **Cocoa Touch osztály** nevű `Notifications`.</span><span class="sxs-lookup"><span data-stu-id="464dc-128">Create a new **Cocoa Touch Class** called `Notifications`.</span></span> <span data-ttu-id="464dc-129">Másolja a következő kód hello fájl Notifications.h hello felület szakaszában hello:</span><span class="sxs-lookup"><span data-stu-id="464dc-129">Copy hello following code in hello interface section of hello file Notifications.h:</span></span>
   
        @property NSData* deviceToken;
   
        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;
   
        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;
   
        - (NSSet*)retrieveCategories;
   
        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;
5. <span data-ttu-id="464dc-130">Adja hozzá a következő importálási irányelv tooNotifications.m hello:</span><span class="sxs-lookup"><span data-stu-id="464dc-130">Add hello following import directive tooNotifications.m:</span></span>
   
        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
6. <span data-ttu-id="464dc-131">A következő kód hello implementation szakaszban hello fájl Notifications.m hello másolja.</span><span class="sxs-lookup"><span data-stu-id="464dc-131">Copy hello following code in hello implementation section of hello file Notifications.m.</span></span>
   
        SBNotificationHub* hub;
   
        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName{
   
            hub = [[SBNotificationHub alloc] initWithConnectionString:listenConnectionString
                                        notificationHubPath:hubName];
   
            return self;
        }
   
        - (void)storeCategoriesAndSubscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
   
            [self subscribeWithCategories:categories completion:completion];
        }

        - (NSSet*)retrieveCategories {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            NSArray* categories = [defaults stringArrayForKey:@"BreakingNewsCategories"];

            if (!categories) return [[NSSet alloc] init];
            return [[NSSet alloc] initWithArray:categories];
        }


        - (void)subscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion
        {
           //[hub registerNativeWithDeviceToken:self.deviceToken tags:categories completion: completion];

            NSString* templateBodyAPNS = @"{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"simpleAPNSTemplate" 
                jsonBodyTemplate:templateBodyAPNS expiryTemplate:@"0" tags:categories completion:completion];
        }



    <span data-ttu-id="464dc-132">Ez az osztály toostore helyi tárhelyet használ, és az eszköz fogadó hírek hello kategóriák lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="464dc-132">This class uses local storage toostore and retrieve hello categories of news that this device will receive.</span></span> <span data-ttu-id="464dc-133">Tartalmaz egy metódus tooregister ezen kategóriák segítségével az is, egy [sablon](notification-hubs-templates-cross-platform-push-messages.md) regisztrációs.</span><span class="sxs-lookup"><span data-stu-id="464dc-133">Also, it contains a method tooregister for these categories using a [Template](notification-hubs-templates-cross-platform-push-messages.md) registration.</span></span>

1. <span data-ttu-id="464dc-134">Hello AppDelegate.h fájlban adja hozzá az importálási utasítást Notifications.h és hello értesítések osztály egy példányának tulajdonság hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="464dc-134">In hello AppDelegate.h file, add an import statement for Notifications.h and add a property for an instance of hello Notifications class:</span></span>
   
        #import "Notifications.h"
   
        @property (nonatomic) Notifications* notifications;
2. <span data-ttu-id="464dc-135">A hello **didFinishLaunchingWithOptions** metódus AppDelegate.m, a hello kód tooinitialize hello értesítések példány hozzáadása hello metódus hello elején.</span><span class="sxs-lookup"><span data-stu-id="464dc-135">In hello **didFinishLaunchingWithOptions** method in AppDelegate.m, add hello code tooinitialize hello notifications instance at hello beginning of hello method.</span></span>  
   
    <span data-ttu-id="464dc-136">`HUBNAME`és `HUBLISTENACCESS` (hubinfo.h-ban meghatározott) már rendelkezik hello `<hub name>` és `<connection string with listen access>` helyőrzők helyett az értesítési központ nevét és hello kapcsolati karakterláncot *DefaultListenSharedAccessSignature*korábban beszerzett</span><span class="sxs-lookup"><span data-stu-id="464dc-136">`HUBNAME` and `HUBLISTENACCESS` (defined in hubinfo.h) should already have hello `<hub name>` and `<connection string with listen access>` placeholders replaced with your notification hub name and hello connection string for *DefaultListenSharedAccessSignature* that you obtained earlier</span></span>
   
        self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];
   
   > [!NOTE]
   > <span data-ttu-id="464dc-137">Ügyfél alkalmazáshoz elosztott hitelesítő adatok nem általában biztonságos, mert csak kell terjeszteni figyelési hozzáférési kulcs hello ügyfél alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="464dc-137">Because credentials that are distributed with a client app are not generally secure, you should only distribute hello key for listen access with your client app.</span></span> <span data-ttu-id="464dc-138">Figyeljen hozzáférés lehetővé teszi, hogy az alkalmazás tooregister az értesítéseket, de a meglévő regisztrációját nem lehet módosítani, és értesítések nem küldhető el.</span><span class="sxs-lookup"><span data-stu-id="464dc-138">Listen access enables your app tooregister for notifications, but existing registrations cannot be modified and notifications cannot be sent.</span></span> <span data-ttu-id="464dc-139">hello teljes körű hozzáférési kulcsot használnak a következő biztonságos háttérszolgáltatás értesítések küldését, és meglévő regisztrációk módosítása.</span><span class="sxs-lookup"><span data-stu-id="464dc-139">hello full access key is used in a secured backend service for sending notifications and changing existing registrations.</span></span>
   > 
   > 
3. <span data-ttu-id="464dc-140">A hello **didRegisterForRemoteNotificationsWithDeviceToken** metódus AppDelegate.m, a hello kód hello metódusban cserélje le a következő kód toopass hello token toohello értesítések eszközosztályt hello.</span><span class="sxs-lookup"><span data-stu-id="464dc-140">In hello **didRegisterForRemoteNotificationsWithDeviceToken** method in AppDelegate.m, replace hello code in hello method with hello following code toopass hello device token toohello notifications class.</span></span> <span data-ttu-id="464dc-141">hello értesítések osztály fogja elvégezni az értesítések küldése a hello kategóriák regisztrálásakor hello.</span><span class="sxs-lookup"><span data-stu-id="464dc-141">hello notifications class will perform hello registering for notifications with hello categories.</span></span> <span data-ttu-id="464dc-142">Hello felhasználói kategória beállításokat módosítja, ha hívása hello `subscribeWithCategories` metódus a válasz toohello **előfizetés** gomb tooupdate őket.</span><span class="sxs-lookup"><span data-stu-id="464dc-142">If hello user changes category selections, we call hello `subscribeWithCategories` method in response toohello **subscribe** button tooupdate them.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="464dc-143">Hello eszköz jogkivonatát hello Apple Push Notification (APN) szolgáltatás által hozzárendelt is alkalommal bármikor, mert regisztrálnia kell az értesítésekhez gyakran tooavoid értesítés sikertelen.</span><span class="sxs-lookup"><span data-stu-id="464dc-143">Because hello device token assigned by hello Apple Push Notification Service (APNS) can chance at any time, you should register for notifications frequently tooavoid notification failures.</span></span> <span data-ttu-id="464dc-144">Ebben a példában regisztrál az értesítési hello alkalmazás minden indításakor.</span><span class="sxs-lookup"><span data-stu-id="464dc-144">This example registers for notification every time that hello app starts.</span></span> <span data-ttu-id="464dc-145">Gyakran futtatott alkalmazások esetén naponta csak egyszer, valószínűleg kihagyhatja regisztrációs toopreserve sávszélesség Ha kevesebb mint egy nappal hello előző regisztráció óta eltelt.</span><span class="sxs-lookup"><span data-stu-id="464dc-145">For apps that are run frequently, more than once a day, you can probably skip registration toopreserve bandwidth if less than a day has passed since hello previous registration.</span></span>
   > 
   > 
   
        self.notifications.deviceToken = deviceToken;
   
        // Retrieves hello categories from local storage and requests a registration for these categories
        // each time hello app starts and performs a registration.
   
        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

    <span data-ttu-id="464dc-146">Vegye figyelembe, hogy ezen a ponton kell nincs más kód a hello **didRegisterForRemoteNotificationsWithDeviceToken** metódust.</span><span class="sxs-lookup"><span data-stu-id="464dc-146">Note that at this point there should be no other code in hello **didRegisterForRemoteNotificationsWithDeviceToken** method.</span></span>

1. <span data-ttu-id="464dc-147">hello következőkkel már jelen kell lenniük AppDelegate.m hello befejezését [Ismerkedés a Notification Hubs] [ get-started] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="464dc-147">hello following methods should already be present in AppDelegate.m from completing hello [Get started with Notification Hubs][get-started] tutorial.</span></span>  <span data-ttu-id="464dc-148">Ha nem, adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="464dc-148">If not, add them.</span></span>
   
    <span data-ttu-id="464dc-149">-(void) MessageBox:(NSString *) cím üzenet:(NSString *) ÜzenetSzövege {</span><span class="sxs-lookup"><span data-stu-id="464dc-149">-(void)MessageBox:(NSString *)title message:(NSString *)messageText  {</span></span>
   
        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
            cancelButtonTitle:@"OK" otherButtonTitles: nil];
        [alert show];
    <span data-ttu-id="464dc-150">}</span><span class="sxs-lookup"><span data-stu-id="464dc-150">}</span></span>
   
   * <span data-ttu-id="464dc-151">(a "void") alkalmazás:(UIApplication *) alkalmazás didReceiveRemoteNotification: (NSDictionary *) userInfo {NSLog (@"% @", userInfo);   [a saját MessageBox:@"Notification" üzenetet: [valueForKey:@"alert [userInfo objectForKey:@"aps"]"]]; }</span><span class="sxs-lookup"><span data-stu-id="464dc-151">(void)application:(UIApplication *)application didReceiveRemoteNotification:   (NSDictionary *)userInfo {   NSLog(@"%@", userInfo);   [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]]; }</span></span>
   
   <span data-ttu-id="464dc-152">Ez a metódus kezeli a bejelentések hello alkalmazás futtatásakor egy egyszerű megjelenítésével **UIAlert**.</span><span class="sxs-lookup"><span data-stu-id="464dc-152">This method handles notifications received when hello app is running by displaying a simple **UIAlert**.</span></span>
2. <span data-ttu-id="464dc-153">A ViewController.m, adjon hozzá egy importálási utasítást, a következő kódot a hello AppDelegate.h, és másolja hello XCode által létrehozott **előfizetés** metódust.</span><span class="sxs-lookup"><span data-stu-id="464dc-153">In ViewController.m, add a import statement for AppDelegate.h and copy hello following code into hello XCode-generated **subscribe** method.</span></span> <span data-ttu-id="464dc-154">Ez a kód frissíti a hello értesítési regisztrációs toouse hello új kategória címkék hello felhasználó által választott hello felhasználói felületen.</span><span class="sxs-lookup"><span data-stu-id="464dc-154">This code will update hello notification registration toouse hello new category tags hello user has chosen in hello user interface.</span></span>
   
       ```
       #import "Notifications.h"
       ```
   
       NSMutableArray* categories = [[NSMutableArray alloc] init];
   
       if (self.WorldSwitch.isOn) [categories addObject:@"World"];
       if (self.PoliticsSwitch.isOn) [categories addObject:@"Politics"];
       if (self.BusinessSwitch.isOn) [categories addObject:@"Business"];
       if (self.TechnologySwitch.isOn) [categories addObject:@"Technology"];
       if (self.ScienceSwitch.isOn) [categories addObject:@"Science"];
       if (self.SportsSwitch.isOn) [categories addObject:@"Sports"];
   
       Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];
   
       [notifications storeCategoriesAndSubscribeWithCategories:categories completion: ^(NSError* error) {
           if (!error) {
               [(AppDelegate*)[[UIApplication sharedApplication]delegate] MessageBox:@"Notification" message:@"Subscribed!"];
           } else {
               NSLog(@"Error subscribing: %@", error);
           }
       }];
   
   <span data-ttu-id="464dc-155">Ezzel a módszerrel hoz létre egy **NSMutableArray** a kategóriák és a hello **értesítések** osztály toostore hello listájában hello helyi tároló, és regisztrál hello megfelelő címkéket az értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="464dc-155">This method creates an **NSMutableArray** of categories and uses hello **Notifications** class toostore hello list in hello local storage and registers hello corresponding tags with your notification hub.</span></span> <span data-ttu-id="464dc-156">Kategóriák megváltozásakor hello regisztrációs hello új kategóriák újra létrejön.</span><span class="sxs-lookup"><span data-stu-id="464dc-156">When categories are changed, hello registration is recreated with hello new categories.</span></span>
3. <span data-ttu-id="464dc-157">ViewController.m, adja hozzá a következő kódot a hello hello **viewDidLoad** metódus tooset hello felhasználói felülete a korábban mentett hello kategóriák alapján.</span><span class="sxs-lookup"><span data-stu-id="464dc-157">In ViewController.m, add hello following code in hello **viewDidLoad** method tooset hello user interface based on hello previously saved categories.</span></span>

        // This updates hello UI on startup based on hello status of previously saved categories.

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;



<span data-ttu-id="464dc-158">hello app most tárolhat kategóriák készlete hello eszköz használt helyi tárhely tooregister hello értesítési központban amikor hello alkalmazás indítása.</span><span class="sxs-lookup"><span data-stu-id="464dc-158">hello app can now store a set of categories in hello device local storage used tooregister with hello notification hub whenever hello app starts.</span></span>  <span data-ttu-id="464dc-159">hello felhasználói kategóriák futásidőben hello beállítás módosítható, és kattintson hello **előfizetés** metódus tooupdate hello regisztrációs hello eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="464dc-159">hello user can change hello selection of categories at runtime and click hello **subscribe** method tooupdate hello registration for hello device.</span></span> <span data-ttu-id="464dc-160">A következő később frissíteni hello app toosend hello híreket tartalmazó értesítések megtörje közvetlenül a hello alkalmazás funkcióit.</span><span class="sxs-lookup"><span data-stu-id="464dc-160">Next, you will update hello app toosend hello breaking news notifications directly in hello app itself.</span></span>

## <a name="optional-sending-tagged-notifications"></a><span data-ttu-id="464dc-161">(választható) Címkézett értesítések küldése</span><span class="sxs-lookup"><span data-stu-id="464dc-161">(optional) Sending tagged notifications</span></span>
<span data-ttu-id="464dc-162">Ha még nem rendelkezik hozzáféréssel tooVisual Studio, toohello következő szakaszt kihagyhatja, és értesítések küldése hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="464dc-162">If you don't have access tooVisual Studio, you can skip toohello next section and send notifications from hello app itself.</span></span> <span data-ttu-id="464dc-163">Hello megfelelő sablon értesítési is elküldheti a hello [klasszikus Azure portál] az értesítési központ hibakeresési lapján hello használatával.</span><span class="sxs-lookup"><span data-stu-id="464dc-163">You can also send hello proper template notification from hello [Azure Classic Portal] using hello debug tab for your notification hub.</span></span> 

[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="optional-send-notifications-from-hello-device"></a><span data-ttu-id="464dc-164">(választható) Értesítések küldése hello eszköz</span><span class="sxs-lookup"><span data-stu-id="464dc-164">(optional) Send notifications from hello device</span></span>
<span data-ttu-id="464dc-165">Általában akkor kell értesítéseket egy háttér-szolgáltatás, de hello alkalmazásból közvetlenül is elküldheti a legfrissebb híreket tartalmazó értesítések.</span><span class="sxs-lookup"><span data-stu-id="464dc-165">Normally notifications would be sent by a backend service but, you can send breaking news notifications directly from hello app.</span></span> <span data-ttu-id="464dc-166">toodo ez frissítjük a hello `SendNotificationRESTAPI` módszerrel, amely a hello meghatározott [Ismerkedés a Notification Hubs] [ get-started] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="464dc-166">toodo this we will update hello `SendNotificationRESTAPI` method that we defined in hello [Get started with Notification Hubs][get-started] tutorial.</span></span>

1. <span data-ttu-id="464dc-167">A frissítés hello ViewController.m `SendNotificationRESTAPI` módszert az követi, hogy egy hello kategória címke paramétert fogad és küld a megfelelő hello [sablon](notification-hubs-templates-cross-platform-push-messages.md) értesítést.</span><span class="sxs-lookup"><span data-stu-id="464dc-167">In ViewController.m update hello `SendNotificationRESTAPI` method as follows so that it accepts a parameter for hello category tag and sends hello proper [template](notification-hubs-templates-cross-platform-push-messages.md) notification.</span></span>
   
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
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\"}",
                    categoryTag, self.notificationMessage.text];
   
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
2. <span data-ttu-id="464dc-168">A frissítés hello ViewController.m **értesítés küldése** művelet a következő hello kódban látható módon.</span><span class="sxs-lookup"><span data-stu-id="464dc-168">In ViewController.m update hello **Send Notification** action as shown in hello code that follows.</span></span> <span data-ttu-id="464dc-169">Hogy az egyes címkék használatával külön-külön hello értesítések küldéséhez, és toomultiple platformok küldése.</span><span class="sxs-lookup"><span data-stu-id="464dc-169">So that it will send hello notifications using each tag individually and send toomultiple platforms.</span></span>

        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";

            NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                    @"Technology", @"Science", @"Sports", nil];

            // Lets send hello message as breaking news for each category tooWNS, GCM, and APNS
            // using a template.
            for(NSString* category in categories)
            {
                [self SendNotificationRESTAPI:category];
            }
        }



1. <span data-ttu-id="464dc-170">A projekt újraépítéséhez, és győződjön meg arról, hogy nincs összeállítási hiba.</span><span class="sxs-lookup"><span data-stu-id="464dc-170">Rebuild your project and make sure you have no build errors.</span></span>

## <a name="run-hello-app-and-generate-notifications"></a><span data-ttu-id="464dc-171">Hello alkalmazás futtatását, és értesítések</span><span class="sxs-lookup"><span data-stu-id="464dc-171">Run hello app and generate notifications</span></span>
1. <span data-ttu-id="464dc-172">Nyomja le az hello futtassa gomb toobuild hello projektet, és indítsa el hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="464dc-172">Press hello Run button toobuild hello project and start hello app.</span></span> <span data-ttu-id="464dc-173">Jelölje ki az egyes breaking news beállítások toosubscribe tooand, majd nyomja le az ENTER hello **előfizetés** gombra.</span><span class="sxs-lookup"><span data-stu-id="464dc-173">Select some breaking news options toosubscribe tooand then press hello **Subscribe** button.</span></span> <span data-ttu-id="464dc-174">Már előfizetett értesítések hello utaló üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="464dc-174">You should see a dialog indicating hello notifications have been subscribed to.</span></span>
   
    ![][1]
   
    <span data-ttu-id="464dc-175">Ha úgy dönt, **előfizetés**, app alakítja hello kiválasztott kategóriák hello címkék be, és egy új eszközök regisztrációja kijelölt hello címkék hello értesítési központ érkező kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="464dc-175">When you choose **Subscribe**, hello app converts hello selected categories into tags and requests a new device registration for hello selected tags from hello notification hub.</span></span>
2. <span data-ttu-id="464dc-176">Adjon meg egy elküldött, legfrissebb hírek nyomja meg az üdvözlő üzenet toobe **értesítés küldése** gombra.</span><span class="sxs-lookup"><span data-stu-id="464dc-176">Enter a message toobe sent as breaking news then press hello **Send Notification** button.</span></span> <span data-ttu-id="464dc-177">Alternatív megoldásként futtassa hello .NET konzol app toogenerate értesítések.</span><span class="sxs-lookup"><span data-stu-id="464dc-177">Alternatively, run hello .NET console app toogenerate notifications.</span></span>
   
    ![][2]
3. <span data-ttu-id="464dc-178">Minden eszköz előfizetett toobreaking hírek hello legfrissebb híreket tartalmazó értesítések imént a telefonjára küldött fog kapni.</span><span class="sxs-lookup"><span data-stu-id="464dc-178">Each device subscribed toobreaking news will receive hello breaking news notifications you just sent.</span></span>

## <a name="next-steps"></a><span data-ttu-id="464dc-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="464dc-179">Next steps</span></span>
<span data-ttu-id="464dc-180">Ez az oktatóanyag azt megtanulta, hogyan toobroadcast legfrissebb hírek kategória szerint.</span><span class="sxs-lookup"><span data-stu-id="464dc-180">In this tutorial we learned how toobroadcast breaking news by category.</span></span> <span data-ttu-id="464dc-181">Vegye figyelembe, hogy befejezése hello oktatóprogramot kínál, amelyek más speciális Notification Hubs forgatókönyvek jelölje ki a következő egyikét:</span><span class="sxs-lookup"><span data-stu-id="464dc-181">Consider completing one of hello following tutorials that highlight other advanced Notification Hubs scenarios:</span></span>

* <span data-ttu-id="464dc-182">**[Használjon honosított toobroadcast Notification Hubs – legfrissebb hírek]**</span><span class="sxs-lookup"><span data-stu-id="464dc-182">**[Use Notification Hubs toobroadcast localized breaking news]**</span></span>
  
    <span data-ttu-id="464dc-183">Ismerje meg, hogyan megtörje hírek app tooenable küldése tooexpand hello honosított értesítések.</span><span class="sxs-lookup"><span data-stu-id="464dc-183">Learn how tooexpand hello breaking news app tooenable sending localized notifications.</span></span>

<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Használjon honosított toobroadcast Notification Hubs – legfrissebb hírek]: notification-hubs-ios-xplat-localized-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-toofor iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[get-started]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
[klasszikus Azure portál]: https://manage.windowsazure.com
