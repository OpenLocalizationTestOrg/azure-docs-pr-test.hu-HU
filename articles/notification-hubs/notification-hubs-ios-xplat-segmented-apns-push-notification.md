---
title: "A Notification Hubs Breaking News oktatóanyag – iOS"
description: "Útmutató: Azure Service Bus Notification Hubs használatával legfrissebb híreket tartalmazó értesítések küldése iOS-eszközök."
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
ms.openlocfilehash: dc47250db6fb3a2853dae24e02bda236154d93fb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-notification-hubs-to-send-breaking-news"></a><span data-ttu-id="4c69e-103">A legfrissebb hírek elküldése a Notification Hubs használatával</span><span class="sxs-lookup"><span data-stu-id="4c69e-103">Use Notification Hubs to send breaking news</span></span>
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a><span data-ttu-id="4c69e-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="4c69e-104">Overview</span></span>
<span data-ttu-id="4c69e-105">Ez a témakör bemutatja, hogyan szórási legfrissebb híreket tartalmazó értesítések iOS-alkalmazásokhoz Azure Notification Hubs használatával.</span><span class="sxs-lookup"><span data-stu-id="4c69e-105">This topic shows you how to use Azure Notification Hubs to broadcast breaking news notifications to an iOS app.</span></span> <span data-ttu-id="4c69e-106">Amikor végzett, akkor fog számára megtörje érdekli hírek kategóriák regisztrálni, és ezen kategóriák csak leküldéses értesítések fogadásához.</span><span class="sxs-lookup"><span data-stu-id="4c69e-106">When complete, you will be able to register for breaking news categories you are interested in, and receive only push notifications for those categories.</span></span> <span data-ttu-id="4c69e-107">Ebben a forgatókönyvben számos alkalmazás általános felépítését, ahol értesítések kell őket, például az RSS-olvasóval, az alkalmazások zene ventilátorok stb érdeklődik elemnek már deklarálva felhasználói csoportokat kell küldeni.</span><span class="sxs-lookup"><span data-stu-id="4c69e-107">This scenario is a common pattern for many apps where notifications have to be sent to groups of users that have previously declared interest in them, e.g. RSS reader, apps for music fans, etc.</span></span>

<span data-ttu-id="4c69e-108">Szórási forgatókönyvek engedélyezve vannak, beleértve a következőket egy vagy több *címkék* regisztráció létrehozásakor az értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="4c69e-108">Broadcast scenarios are enabled by including one or more *tags* when creating a registration in the notification hub.</span></span> <span data-ttu-id="4c69e-109">Amikor a rendszer értesítéseket küld egy címkét, akkor a címke regisztrált minden eszköz a értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="4c69e-109">When notifications are sent to a tag, all devices that have registered for the tag will receive the notification.</span></span> <span data-ttu-id="4c69e-110">Mivel a címkékkel egyszerűen csak karakterláncok, nem rendelkeznek előre kell építeni.</span><span class="sxs-lookup"><span data-stu-id="4c69e-110">Because tags are simply strings, they do not have to be provisioned in advance.</span></span> <span data-ttu-id="4c69e-111">Címkékkel kapcsolatos további információkért tekintse meg [Notification Hubs útválasztási és címke kifejezések](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="4c69e-111">For more information about tags, refer to [Notification Hubs Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4c69e-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4c69e-112">Prerequisites</span></span>
<span data-ttu-id="4c69e-113">Ez a témakör a létrehozott alkalmazás épül [Ismerkedés a Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="4c69e-113">This topic builds on the app you created in [Get started with Notification Hubs][get-started].</span></span> <span data-ttu-id="4c69e-114">Az oktatóanyag elindítása előtt már végrehajtotta [Ismerkedés a Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="4c69e-114">Before starting this tutorial, you must have already completed [Get started with Notification Hubs][get-started].</span></span>

## <a name="add-category-selection-to-the-app"></a><span data-ttu-id="4c69e-115">Kategória kiválasztása hozzáadása az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="4c69e-115">Add category selection to the app</span></span>
<span data-ttu-id="4c69e-116">Az első lépés a felhasználói felületi elemek hozzáadása a meglévő storyboard, amelyek lehetővé teszik a felhasználó számára a kategóriák regisztrálni kívánt.</span><span class="sxs-lookup"><span data-stu-id="4c69e-116">The first step is to add the UI elements to your existing storyboard that enable the user to select categories to register.</span></span> <span data-ttu-id="4c69e-117">A felhasználó által kiválasztott kategóriák tárolódnak az eszközön.</span><span class="sxs-lookup"><span data-stu-id="4c69e-117">The categories selected by a user are stored on the device.</span></span> <span data-ttu-id="4c69e-118">Az alkalmazás indításakor a eszközregisztráció címkeként az értesítési központ, a kiválasztott kategóriákra jön létre.</span><span class="sxs-lookup"><span data-stu-id="4c69e-118">When the app starts, a device registration is created in your notification hub with the selected categories as tags.</span></span>

1. <span data-ttu-id="4c69e-119">A MainStoryboard_iPhone.storyboard adja hozzá a következő összetevőket az objektumtárból:</span><span class="sxs-lookup"><span data-stu-id="4c69e-119">In your MainStoryboard_iPhone.storyboard add the following components from the object library:</span></span>
   
   * <span data-ttu-id="4c69e-120">A címkék "Megtörje hírek" szóra,</span><span class="sxs-lookup"><span data-stu-id="4c69e-120">A label with "Breaking News" text,</span></span>
   * <span data-ttu-id="4c69e-121">Címkék kategória szöveget a "World", "Politika", "Vállalati", "Technológia", "Tudományos", "Sport",</span><span class="sxs-lookup"><span data-stu-id="4c69e-121">Labels with category texts "World", "Politics", "Business", "Technology", "Science", "Sports",</span></span>
   * <span data-ttu-id="4c69e-122">Egy kategóriát, egy hat kapcsolók beállítása minden kapcsoló **állapot** kell **ki** alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="4c69e-122">Six switches, one per category, set each switch **State** to be **Off** by default.</span></span>
   * <span data-ttu-id="4c69e-123">Egy gomb "Előfizetés" címkével</span><span class="sxs-lookup"><span data-stu-id="4c69e-123">One button labeled "Subscribe"</span></span>
     
     <span data-ttu-id="4c69e-124">A storyboard fájlt a következőképpen kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="4c69e-124">Your storyboard should look as follows:</span></span>
     
     ![][3]
2. <span data-ttu-id="4c69e-125">A Segéd-szerkesztőben kimeneteket a kapcsolók létrehozása, és hívja meg őket "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"</span><span class="sxs-lookup"><span data-stu-id="4c69e-125">In the assistant editor, create outlets for all the switches and call them "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"</span></span>
3. <span data-ttu-id="4c69e-126">Az "előfizetés" gombra a művelet létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4c69e-126">Create an Action for your button called "subscribe".</span></span> <span data-ttu-id="4c69e-127">A ViewController.h tartalmazniuk kell a következőket:</span><span class="sxs-lookup"><span data-stu-id="4c69e-127">Your ViewController.h should contain the following:</span></span>
   
        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;
   
        - (IBAction)subscribe:(id)sender;
4. <span data-ttu-id="4c69e-128">Hozzon létre egy új **Cocoa Touch osztály** nevű `Notifications`.</span><span class="sxs-lookup"><span data-stu-id="4c69e-128">Create a new **Cocoa Touch Class** called `Notifications`.</span></span> <span data-ttu-id="4c69e-129">Másolja az alábbi kódot a fájl Notifications.h felület részében:</span><span class="sxs-lookup"><span data-stu-id="4c69e-129">Copy the following code in the interface section of the file Notifications.h:</span></span>
   
        @property NSData* deviceToken;
   
        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;
   
        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;
   
        - (NSSet*)retrieveCategories;
   
        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;
5. <span data-ttu-id="4c69e-130">Adja hozzá a következő importálási irányelv Notifications.m:</span><span class="sxs-lookup"><span data-stu-id="4c69e-130">Add the following import directive to Notifications.m:</span></span>
   
        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
6. <span data-ttu-id="4c69e-131">Másolja az alábbi kódot a fájl Notifications.m az implementation szakaszban.</span><span class="sxs-lookup"><span data-stu-id="4c69e-131">Copy the following code in the implementation section of the file Notifications.m.</span></span>
   
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



    <span data-ttu-id="4c69e-132">Ez az osztály tárolásához és lekéréséhez híreket az eszköz fogadó kategóriáinak helyi tárolást használ.</span><span class="sxs-lookup"><span data-stu-id="4c69e-132">This class uses local storage to store and retrieve the categories of news that this device will receive.</span></span> <span data-ttu-id="4c69e-133">Ezen kategóriák használatával regisztrálni egy metódust tartalmaz is, egy [sablon](notification-hubs-templates-cross-platform-push-messages.md) regisztrációs.</span><span class="sxs-lookup"><span data-stu-id="4c69e-133">Also, it contains a method to register for these categories using a [Template](notification-hubs-templates-cross-platform-push-messages.md) registration.</span></span>

1. <span data-ttu-id="4c69e-134">A AppDelegate.h fájlban adja hozzá az importálási utasítást Notifications.h, és az értesítések osztály egy példányának tulajdonság hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="4c69e-134">In the AppDelegate.h file, add an import statement for Notifications.h and add a property for an instance of the Notifications class:</span></span>
   
        #import "Notifications.h"
   
        @property (nonatomic) Notifications* notifications;
2. <span data-ttu-id="4c69e-135">Az a **didFinishLaunchingWithOptions** metódus a AppDelegate.m, adja hozzá a kódot, a módszer az elején értesítések példány inicializálása.</span><span class="sxs-lookup"><span data-stu-id="4c69e-135">In the **didFinishLaunchingWithOptions** method in AppDelegate.m, add the code to initialize the notifications instance at the beginning of the method.</span></span>  
   
    <span data-ttu-id="4c69e-136">`HUBNAME`és `HUBLISTENACCESS` (hubinfo.h-ban meghatározott) már rendelkezik a `<hub name>` és `<connection string with listen access>` helyőrzők helyett az értesítési központ nevére és a kapcsolati karakterláncot *DefaultListenSharedAccessSignature* korábban beszerzett</span><span class="sxs-lookup"><span data-stu-id="4c69e-136">`HUBNAME` and `HUBLISTENACCESS` (defined in hubinfo.h) should already have the `<hub name>` and `<connection string with listen access>` placeholders replaced with your notification hub name and the connection string for *DefaultListenSharedAccessSignature* that you obtained earlier</span></span>
   
        self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];
   
   > [!NOTE]
   > <span data-ttu-id="4c69e-137">Eszközzel együtt egy ügyfélalkalmazás hitelesítő adatok nem általában biztonságos, mert csak kell terjeszteni a figyelési hozzáférési kulcs ügyfél alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="4c69e-137">Because credentials that are distributed with a client app are not generally secure, you should only distribute the key for listen access with your client app.</span></span> <span data-ttu-id="4c69e-138">Figyeljen hozzáférés lehetővé teszi, hogy az alkalmazás regisztrálásához értesítések, de a meglévő regisztrációk nem módosítható, és értesítések nem küldhető el.</span><span class="sxs-lookup"><span data-stu-id="4c69e-138">Listen access enables your app to register for notifications, but existing registrations cannot be modified and notifications cannot be sent.</span></span> <span data-ttu-id="4c69e-139">A teljes körű hozzáférési kulcs értesítések küldését, és meglévő regisztrációk módosítása védett háttérszolgáltatás használatban.</span><span class="sxs-lookup"><span data-stu-id="4c69e-139">The full access key is used in a secured backend service for sending notifications and changing existing registrations.</span></span>
   > 
   > 
3. <span data-ttu-id="4c69e-140">Az a **didRegisterForRemoteNotificationsWithDeviceToken** metódus a AppDelegate.m, cserélje le a kód metódus a következő kódot az eszköz jogkivonatát átadása az értesítések osztály.</span><span class="sxs-lookup"><span data-stu-id="4c69e-140">In the **didRegisterForRemoteNotificationsWithDeviceToken** method in AppDelegate.m, replace the code in the method with the following code to pass the device token to the notifications class.</span></span> <span data-ttu-id="4c69e-141">Az értesítések osztály fogja elvégezni az értesítések küldése a kategóriák regisztrálásakor.</span><span class="sxs-lookup"><span data-stu-id="4c69e-141">The notifications class will perform the registering for notifications with the categories.</span></span> <span data-ttu-id="4c69e-142">A felhasználó megváltoztatja a kategória-beállításokat, ha hívása a `subscribeWithCategories` válaszul módszer a **előfizetés** gombra kattintva frissítse azokat.</span><span class="sxs-lookup"><span data-stu-id="4c69e-142">If the user changes category selections, we call the `subscribeWithCategories` method in response to the **subscribe** button to update them.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="4c69e-143">Az eszköz jogkivonatát által az Apple Push Notification (APN) szolgáltatás hozzárendelt is alkalommal bármikor, mert az értesítések a notification hibák elkerülése érdekében gyakran kell regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="4c69e-143">Because the device token assigned by the Apple Push Notification Service (APNS) can chance at any time, you should register for notifications frequently to avoid notification failures.</span></span> <span data-ttu-id="4c69e-144">Ebben a példában regisztrál az értesítési minden alkalommal, az alkalmazás indítása.</span><span class="sxs-lookup"><span data-stu-id="4c69e-144">This example registers for notification every time that the app starts.</span></span> <span data-ttu-id="4c69e-145">Gyakran futtatott alkalmazások esetén naponta csak egyszer, valószínűleg kihagyhatja regisztrációt, hogy a sávszélesség megőrzése, ha az előző regisztráció óta eltelt egy napnál.</span><span class="sxs-lookup"><span data-stu-id="4c69e-145">For apps that are run frequently, more than once a day, you can probably skip registration to preserve bandwidth if less than a day has passed since the previous registration.</span></span>
   > 
   > 
   
        self.notifications.deviceToken = deviceToken;
   
        // Retrieves the categories from local storage and requests a registration for these categories
        // each time the app starts and performs a registration.
   
        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

    <span data-ttu-id="4c69e-146">Vegye figyelembe, hogy ezen a ponton kell nincs más kód a **didRegisterForRemoteNotificationsWithDeviceToken** metódust.</span><span class="sxs-lookup"><span data-stu-id="4c69e-146">Note that at this point there should be no other code in the **didRegisterForRemoteNotificationsWithDeviceToken** method.</span></span>

1. <span data-ttu-id="4c69e-147">Az alábbi módszerek már befejezését AppDelegate.m jelen kell lenniük a [Ismerkedés a Notification Hubs] [ get-started] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="4c69e-147">The following methods should already be present in AppDelegate.m from completing the [Get started with Notification Hubs][get-started] tutorial.</span></span>  <span data-ttu-id="4c69e-148">Ha nem, adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="4c69e-148">If not, add them.</span></span>
   
    <span data-ttu-id="4c69e-149">-(void) MessageBox:(NSString *) cím üzenet:(NSString *) ÜzenetSzövege {</span><span class="sxs-lookup"><span data-stu-id="4c69e-149">-(void)MessageBox:(NSString *)title message:(NSString *)messageText  {</span></span>
   
        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
            cancelButtonTitle:@"OK" otherButtonTitles: nil];
        [alert show];
    <span data-ttu-id="4c69e-150">}</span><span class="sxs-lookup"><span data-stu-id="4c69e-150">}</span></span>
   
   * <span data-ttu-id="4c69e-151">(a "void") alkalmazás:(UIApplication *) alkalmazás didReceiveRemoteNotification: (NSDictionary *) userInfo {NSLog (@"% @", userInfo);   [a saját MessageBox:@"Notification" üzenetet: [valueForKey:@"alert [userInfo objectForKey:@"aps"]"]]; }</span><span class="sxs-lookup"><span data-stu-id="4c69e-151">(void)application:(UIApplication *)application didReceiveRemoteNotification:   (NSDictionary *)userInfo {   NSLog(@"%@", userInfo);   [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]]; }</span></span>
   
   <span data-ttu-id="4c69e-152">Ez a metódus kezeli az alkalmazás futtatásakor egy egyszerű megjelenítésével fogadott értesítések **UIAlert**.</span><span class="sxs-lookup"><span data-stu-id="4c69e-152">This method handles notifications received when the app is running by displaying a simple **UIAlert**.</span></span>
2. <span data-ttu-id="4c69e-153">A ViewController.m, adjon hozzá egy importálási utasítást a AppDelegate.h, és másolja a következő kódot az XCode által létrehozott **előfizetés** metódust.</span><span class="sxs-lookup"><span data-stu-id="4c69e-153">In ViewController.m, add a import statement for AppDelegate.h and copy the following code into the XCode-generated **subscribe** method.</span></span> <span data-ttu-id="4c69e-154">Ez a kód frissíti az új, a felhasználó által választott, a felhasználói felületen kategória címkék használata az értesítési regisztráció.</span><span class="sxs-lookup"><span data-stu-id="4c69e-154">This code will update the notification registration to use the new category tags the user has chosen in the user interface.</span></span>
   
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
   
   <span data-ttu-id="4c69e-155">Ezzel a módszerrel hoz létre egy **NSMutableArray** kategóriák és használja a **értesítések** osztályra, hogy a lista tárolása a helyi tárolóhoz, és regisztrál az értesítési központban a megfelelő címkéket.</span><span class="sxs-lookup"><span data-stu-id="4c69e-155">This method creates an **NSMutableArray** of categories and uses the **Notifications** class to store the list in the local storage and registers the corresponding tags with your notification hub.</span></span> <span data-ttu-id="4c69e-156">Kategóriák módosításakor a regisztrációs újra létrejön az új kategóriák.</span><span class="sxs-lookup"><span data-stu-id="4c69e-156">When categories are changed, the registration is recreated with the new categories.</span></span>
3. <span data-ttu-id="4c69e-157">ViewController.m, adja hozzá a következő kódot a **viewDidLoad** beállítása a felhasználói felület módszer a korábban mentett kategóriák alapján.</span><span class="sxs-lookup"><span data-stu-id="4c69e-157">In ViewController.m, add the following code in the **viewDidLoad** method to set the user interface based on the previously saved categories.</span></span>

        // This updates the UI on startup based on the status of previously saved categories.

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;



<span data-ttu-id="4c69e-158">Az alkalmazás most tárolhat kategóriák készlete eszköz helyi tárolójára regisztrálhatók az értesítési központban, amikor az alkalmazás indítása.</span><span class="sxs-lookup"><span data-stu-id="4c69e-158">The app can now store a set of categories in the device local storage used to register with the notification hub whenever the app starts.</span></span>  <span data-ttu-id="4c69e-159">A felhasználó módosíthatja a kiválasztott kategóriák futásidejű, és kattintson a **előfizetés** módszer az eszköz regisztrációjának frissítését.</span><span class="sxs-lookup"><span data-stu-id="4c69e-159">The user can change the selection of categories at runtime and click the **subscribe** method to update the registration for the device.</span></span> <span data-ttu-id="4c69e-160">A következő később frissíteni az alkalmazásnak, hogy a legfrissebb híreket tartalmazó értesítések küldése közvetlenül az alkalmazás funkcióit.</span><span class="sxs-lookup"><span data-stu-id="4c69e-160">Next, you will update the app to send the breaking news notifications directly in the app itself.</span></span>

## <a name="optional-sending-tagged-notifications"></a><span data-ttu-id="4c69e-161">(választható) Címkézett értesítések küldése</span><span class="sxs-lookup"><span data-stu-id="4c69e-161">(optional) Sending tagged notifications</span></span>
<span data-ttu-id="4c69e-162">Ha nem fér a Visual Studio, ugorjon a következő részre, és értesítések küldése az alkalmazás funkcióit.</span><span class="sxs-lookup"><span data-stu-id="4c69e-162">If you don't have access to Visual Studio, you can skip to the next section and send notifications from the app itself.</span></span> <span data-ttu-id="4c69e-163">A megfelelő sablon értesítést is küldhet a [klasszikus Azure portál] az értesítési központ hibakeresési lapján.</span><span class="sxs-lookup"><span data-stu-id="4c69e-163">You can also send the proper template notification from the [Azure Classic Portal] using the debug tab for your notification hub.</span></span> 

[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="optional-send-notifications-from-the-device"></a><span data-ttu-id="4c69e-164">(választható) Értesítések küldése az eszköz</span><span class="sxs-lookup"><span data-stu-id="4c69e-164">(optional) Send notifications from the device</span></span>
<span data-ttu-id="4c69e-165">Általában akkor kell értesítéseket egy háttér-szolgáltatás, de az alkalmazásból közvetlenül is elküldheti a legfrissebb híreket tartalmazó értesítések.</span><span class="sxs-lookup"><span data-stu-id="4c69e-165">Normally notifications would be sent by a backend service but, you can send breaking news notifications directly from the app.</span></span> <span data-ttu-id="4c69e-166">Ehhez a Microsoft frissíti a `SendNotificationRESTAPI` , hogy a megadott metódust a [Ismerkedés a Notification Hubs] [ get-started] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="4c69e-166">To do this we will update the `SendNotificationRESTAPI` method that we defined in the [Get started with Notification Hubs][get-started] tutorial.</span></span>

1. <span data-ttu-id="4c69e-167">Frissítés a ViewController.m a `SendNotificationRESTAPI` módszert az követi, hogy a kategória címke egy paramétert fogad és küld a megfelelő [sablon](notification-hubs-templates-cross-platform-push-messages.md) értesítést.</span><span class="sxs-lookup"><span data-stu-id="4c69e-167">In ViewController.m update the `SendNotificationRESTAPI` method as follows so that it accepts a parameter for the category tag and sends the proper [template](notification-hubs-templates-cross-platform-push-messages.md) notification.</span></span>
   
        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];
   
            NSString *json;
   
            // Construct the messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];
   
            // Generated the token to be used in the authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];
   
            //Create the request to add the template notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
   
            // Add the category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];
   
            // Template notification
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\"}",
                    categoryTag, self.notificationMessage.text];
   
            // Signify template notification format
            [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];
   
            // JSON Content-Type
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];
   
            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];
   
            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
   
            // Send the REST request
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
2. <span data-ttu-id="4c69e-168">Frissítés a ViewController.m a **értesítés küldése** művelet az alábbi kódban látható módon.</span><span class="sxs-lookup"><span data-stu-id="4c69e-168">In ViewController.m update the **Send Notification** action as shown in the code that follows.</span></span> <span data-ttu-id="4c69e-169">Hogy azt az egyes címkék használatával külön-külön értesítések küldéséhez, és több platform küldeni.</span><span class="sxs-lookup"><span data-stu-id="4c69e-169">So that it will send the notifications using each tag individually and send to multiple platforms.</span></span>

        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";

            NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                    @"Technology", @"Science", @"Sports", nil];

            // Lets send the message as breaking news for each category to WNS, GCM, and APNS
            // using a template.
            for(NSString* category in categories)
            {
                [self SendNotificationRESTAPI:category];
            }
        }



1. <span data-ttu-id="4c69e-170">A projekt újraépítéséhez, és győződjön meg arról, hogy nincs összeállítási hiba.</span><span class="sxs-lookup"><span data-stu-id="4c69e-170">Rebuild your project and make sure you have no build errors.</span></span>

## <a name="run-the-app-and-generate-notifications"></a><span data-ttu-id="4c69e-171">Futtassa az alkalmazást, és értesítések</span><span class="sxs-lookup"><span data-stu-id="4c69e-171">Run the app and generate notifications</span></span>
1. <span data-ttu-id="4c69e-172">A Futtatás gombra a projekt felépítéséhez és az alkalmazás indításához.</span><span class="sxs-lookup"><span data-stu-id="4c69e-172">Press the Run button to build the project and start the app.</span></span> <span data-ttu-id="4c69e-173">Adja meg néhány legfrissebb hírek beállításokat előfizetni, és nyomja le az **előfizetés** gombra.</span><span class="sxs-lookup"><span data-stu-id="4c69e-173">Select some breaking news options to subscribe to and then press the **Subscribe** button.</span></span> <span data-ttu-id="4c69e-174">Az értesítések már előfizetett utaló üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="4c69e-174">You should see a dialog indicating the notifications have been subscribed to.</span></span>
   
    ![][1]
   
    <span data-ttu-id="4c69e-175">Ha úgy dönt, **előfizetés**, az app alakítja át a kiválasztott kategóriákra címkék és a kijelölt címke egy új regisztrálásának kéri le az értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="4c69e-175">When you choose **Subscribe**, the app converts the selected categories into tags and requests a new device registration for the selected tags from the notification hub.</span></span>
2. <span data-ttu-id="4c69e-176">Adja meg az elküldött, legfrissebb hírek nyomja meg az üzenetet a **értesítés küldése** gombra.</span><span class="sxs-lookup"><span data-stu-id="4c69e-176">Enter a message to be sent as breaking news then press the **Send Notification** button.</span></span> <span data-ttu-id="4c69e-177">Alternatív megoldásként futtassa a .NET-Konzolalkalmazás értesítések létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="4c69e-177">Alternatively, run the .NET console app to generate notifications.</span></span>
   
    ![][2]
3. <span data-ttu-id="4c69e-178">Minden egyes előfizetője a legfrissebb hírek lesz, elküldjük az imént a telefonjára küldött legfrissebb híreket tartalmazó értesítések.</span><span class="sxs-lookup"><span data-stu-id="4c69e-178">Each device subscribed to breaking news will receive the breaking news notifications you just sent.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4c69e-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4c69e-179">Next steps</span></span>
<span data-ttu-id="4c69e-180">Az oktatóanyag azt megtudta, hogyan kell közvetíteni legfrissebb hírek kategória szerint.</span><span class="sxs-lookup"><span data-stu-id="4c69e-180">In this tutorial we learned how to broadcast breaking news by category.</span></span> <span data-ttu-id="4c69e-181">Vegye figyelembe az alábbi oktatóanyagok számára más speciális Notification Hubs-forgatókönyvek közül befejezése:</span><span class="sxs-lookup"><span data-stu-id="4c69e-181">Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:</span></span>

* <span data-ttu-id="4c69e-182">**[Honosított legfrissebb hírek szórási a Notification Hubs használatával]**</span><span class="sxs-lookup"><span data-stu-id="4c69e-182">**[Use Notification Hubs to broadcast localized breaking news]**</span></span>
  
    <span data-ttu-id="4c69e-183">Megtudhatja, hogyan bontsa ki a legfrissebb hírek app küldő honosított értesítések engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="4c69e-183">Learn how to expand the breaking news app to enable sending localized notifications.</span></span>

<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
<span data-ttu-id="4c69e-184">[Honosított legfrissebb hírek szórási a Notification Hubs használatával]: notification-hubs-ios-xplat-localized-apns-push-notification.md</span><span class="sxs-lookup"><span data-stu-id="4c69e-184">[Use Notification Hubs to broadcast localized breaking news]: notification-hubs-ios-xplat-localized-apns-push-notification.md</span></span>
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[get-started]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
<span data-ttu-id="4c69e-185">[klasszikus Azure portál]: https://manage.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="4c69e-185">[Azure Classic Portal]: https://manage.windowsazure.com</span></span>
