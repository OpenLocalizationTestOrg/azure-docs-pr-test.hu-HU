---
title: "Az Azure Notification Hubs gazdag leküldéses"
description: "Megtudhatja, hogyan gazdag leküldéses értesítések küldéséhez iOS-alkalmazásokhoz az Azure-ból. Kódminták Objective-C és C#."
documentationcenter: ios
services: notification-hubs
author: ysxu
manager: erikre
editor: 
ms.assetid: 590304df-c0a4-46c5-8ef5-6a6486bb3340
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 394efdc2dfaff0666bc23d8a448b0a00d414da99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-notification-hubs-rich-push"></a><span data-ttu-id="bfaaa-104">Az Azure Notification Hubs gazdag leküldéses</span><span class="sxs-lookup"><span data-stu-id="bfaaa-104">Azure Notification Hubs Rich Push</span></span>
## <a name="overview"></a><span data-ttu-id="bfaaa-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="bfaaa-105">Overview</span></span>
<span data-ttu-id="bfaaa-106">Azonnali gazdag tartalma a felhasználókat szólítsa meg, hogy egy alkalmazás előfordulhat, hogy szeretné leküldeni túl egyszerű szöveg.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-106">In order to engage users with instant rich contents, an application might want to push beyond plain text.</span></span> <span data-ttu-id="bfaaa-107">Ezek az értesítések felhasználói kommunikációt, valamint a tartalom URL-címek, hangok, képek/kuponok és több például lépteti elő.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-107">These notifications promote user interactions and  present content such as urls, sounds, images/coupons, and more.</span></span> <span data-ttu-id="bfaaa-108">Ez az oktatóanyag épít, a [felhasználók értesítése](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) témakör, és bemutatja, hogyan küldhetők leküldéses értesítések, amelyek hasznos adat található (például kép).</span><span class="sxs-lookup"><span data-stu-id="bfaaa-108">This tutorial builds on the [Notify Users](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) topic, and shows how to send push notifications that incorporate payloads (for example, image).</span></span>

<span data-ttu-id="bfaaa-109">Ebben az oktatóanyagban található kompatibilis iOS 7 és a 8.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-109">This tutorial is compatible with iOS 7 & 8.</span></span>

  ![][IOS1]

<span data-ttu-id="bfaaa-110">Magas szinten:</span><span class="sxs-lookup"><span data-stu-id="bfaaa-110">At a high level:</span></span>

1. <span data-ttu-id="bfaaa-111">A háttéralkalmazás:</span><span class="sxs-lookup"><span data-stu-id="bfaaa-111">The app backend:</span></span>
   * <span data-ttu-id="bfaaa-112">A gazdag forgalma tárolja (ebben az esetben kép) a háttérbeli adatbázis/helyi tárolóban</span><span class="sxs-lookup"><span data-stu-id="bfaaa-112">Stores the rich payload (in this case, image) in the backend database/local storage</span></span>
   * <span data-ttu-id="bfaaa-113">Elküldi az eszköznek gazdag értesítés azonosítója</span><span class="sxs-lookup"><span data-stu-id="bfaaa-113">Sends ID of this rich notification to the device</span></span>
2. <span data-ttu-id="bfaaa-114">Az alkalmazás az eszközön:</span><span class="sxs-lookup"><span data-stu-id="bfaaa-114">App on the device:</span></span>
   * <span data-ttu-id="bfaaa-115">Kapcsolatba lép a háttérkiszolgálón a gazdag forgalma kap azonosítójú kérő</span><span class="sxs-lookup"><span data-stu-id="bfaaa-115">Contacts the backend requesting the rich payload with the ID it receives</span></span>
   * <span data-ttu-id="bfaaa-116">Felhasználók értesítéseket küld az eszközre, adatok beolvasása befejeződött, és bemutatja a tartalom azonnal, amikor a felhasználók további koppintson</span><span class="sxs-lookup"><span data-stu-id="bfaaa-116">Sends users notifications on the device when data retrieval is complete, and shows the payload immediately when users tap to learn more</span></span>

## <a name="webapi-project"></a><span data-ttu-id="bfaaa-117">WebAPI projekt</span><span class="sxs-lookup"><span data-stu-id="bfaaa-117">WebAPI Project</span></span>
1. <span data-ttu-id="bfaaa-118">A Visual Studióban nyissa meg a **AppBackend** a projekt a [felhasználók értesítése](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-118">In Visual Studio, open the **AppBackend** project that you created in the [Notify Users](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tutorial.</span></span>
2. <span data-ttu-id="bfaaa-119">Szerezzen be szeretne értesíteni a felhasználókat, és elhelyezi lemezkép egy **img** projektkönyvtárban mappa.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-119">Obtain an image you would like to notify users with, and put it in an **img** folder in your project directory.</span></span>
3. <span data-ttu-id="bfaaa-120">Kattintson a **minden fájl megjelenítése** a Megoldáskezelőben, és kattintson a jobb gombbal a mappa **közé tartozik a projekt**.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-120">Click **Show All Files** in the Solution Explorer, and right-click the folder to **Include In Project**.</span></span>
4. <span data-ttu-id="bfaaa-121">A kiválasztott lemezképpel, módosítsa a Tulajdonságok ablak Build műveletét **beágyazott erőforrás**.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-121">With the image selected, change its Build Action in Properties window to **Embedded Resource**.</span></span>
   
    ![][IOS2]
5. <span data-ttu-id="bfaaa-122">A **Notifications.cs**, adja hozzá a következő using utasítást:</span><span class="sxs-lookup"><span data-stu-id="bfaaa-122">In **Notifications.cs**, add the following using statement:</span></span>
   
        using System.Reflection;
6. <span data-ttu-id="bfaaa-123">Frissítse a teljes **értesítések** osztály a következő kóddal.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-123">Update the whole **Notifications** class with the following code.</span></span> <span data-ttu-id="bfaaa-124">Győződjön meg arról, hogy cserélje le a helyőrzőket az értesítési központ hitelesítő adatokat és a kép fájlnevét.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-124">Be sure to replace the placeholders with your notification hub credentials and image file name.</span></span>
   
        public class Notification {
            public int Id { get; set; }
            // Initial notification message to display to users
            public string Message { get; set; }
            // Type of rich payload (developer-defined)
            public string RichType { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }
   
        public class Notifications {
            public static Notifications Instance = new Notifications();
   
            private List<Notification> notifications = new List<Notification>();
   
            public NotificationHubClient Hub { get; set; }
   
            private Notifications() {
                // Placeholders: replace with the connection string (with full access) for your notification hub and the hub name from the Azure Classics Portal
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",  "{hub name}");
            }
   
            public Notification CreateNotification(string message, string richType, string payload) {
                var notification = new Notification() {
                    Id = notifications.Count,
                    Message = message,
                    RichType = richType,
                    Payload = payload,
                    Read = false
                };
   
                notifications.Add(notification);
   
                return notification;
            }
   
            public Stream ReadImage(int id) {
                var assembly = Assembly.GetExecutingAssembly();
                // Placeholder: image file name (for example, logo.png).
                return assembly.GetManifestResourceStream("AppBackend.img.{logo.png}");
            }
        }
   
   > [!NOTE]
   > <span data-ttu-id="bfaaa-125">(választható) Tekintse meg [beágyazása és Visual C# használatával erőforrások elérését](http://support.microsoft.com/kb/319292) hozzáadása és projekt szerezzen be további tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-125">(optional) Refer to [How to embed and access resources by using Visual C#](http://support.microsoft.com/kb/319292) for more information on how to add and obtain project resources.</span></span>
   > 
   > 
7. <span data-ttu-id="bfaaa-126">A **NotificationsController.cs**, újradefiniálása **NotificationsController** az alábbi kódtöredékek.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-126">In **NotificationsController.cs**, redefine **NotificationsController**  with the following snippets.</span></span> <span data-ttu-id="bfaaa-127">Ez csendes gazdag értesítése azonosítót küld eszköz, és lehetővé teszi, hogy a kép lekérése az ügyféloldali:</span><span class="sxs-lookup"><span data-stu-id="bfaaa-127">This sends an initial silent rich notification id to device and allows client-side retrieval of image:</span></span>
   
        // Return http response with image binary
        public HttpResponseMessage Get(int id) {
            var stream = Notifications.Instance.ReadImage(id);
   
            var result = new HttpResponseMessage(HttpStatusCode.OK);
            result.Content = new StreamContent(stream);
            // Switch in your image extension for "png"
            result.Content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/{png}");
   
            return result;
        }
   
        // Create rich notification and send initial silent notification (containing id) to client
        public async Task<HttpResponseMessage> Post() {
            // Replace the placeholder with image file name
            var richNotificationInTheBackend = Notifications.Instance.CreateNotification("Check this image out!", "img",  "{logo.png}");
   
            var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;
   
            // Silent notification with content available
            var aboutUser = "{\"aps\": {\"content-available\": 1, \"sound\":\"\"}, \"richId\": \"" + richNotificationInTheBackend.Id.ToString() + "\",  \"richMessage\": \"" + richNotificationInTheBackend.Message + "\", \"richType\": \"" + richNotificationInTheBackend.RichType + "\"}";
   
            // Send notification to apns
            await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(aboutUser, usernameTag);
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
8. <span data-ttu-id="bfaaa-128">Most azt újra telepíti ezt a webalkalmazást az Azure-webhely annak érdekében, hogy minden eszköz érhető el.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-128">Now we will re-deploy this app to an Azure Website in order to make it accessible from all devices.</span></span> <span data-ttu-id="bfaaa-129">Kattintson jobb gombbal az **AppBackend** projektre, és válassza a **Publish** (Közzététel) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-129">Right-click on the **AppBackend** project and select **Publish**.</span></span>
9. <span data-ttu-id="bfaaa-130">A közzétételi célként válassza az Azure webhelyén.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-130">Select Azure Website as your publish target.</span></span> <span data-ttu-id="bfaaa-131">Jelentkezzen be az Azure-fiókjával, és válassza ki a meglévő vagy új webhely létrehozása, és jegyezze fel a a **URL-címre** tulajdonságot a **kapcsolat** fülre.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-131">Log in with your Azure account and select an existing or new Website, and make a note of the **destination URL** property in the **Connection** tab.</span></span> <span data-ttu-id="bfaaa-132">Az oktatóanyag további részében erre az URL-címre fogunk hivatkozni a *háttérrendszer végpontjaként*.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-132">We will refer to this URL as your *backend endpoint* later in this tutorial.</span></span> <span data-ttu-id="bfaaa-133">Kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-133">Click **Publish**.</span></span>

## <a name="modify-the-ios-project"></a><span data-ttu-id="bfaaa-134">Az iOS-projektre módosítása</span><span class="sxs-lookup"><span data-stu-id="bfaaa-134">Modify the iOS project</span></span>
<span data-ttu-id="bfaaa-135">Most, hogy módosította a háttéralkalmazás küldése csak a *azonosító* egy értesítés, az iOS-alkalmazás azonosító, és a gazdag üzenet olvasható be a háttérrendszerből változik.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-135">Now that you have modified your app backend to send just the *id* of a notification, you will change your iOS app to handle that id and retrieve the rich message from your backend.</span></span>

1. <span data-ttu-id="bfaaa-136">Nyissa meg az iOS-projekthez, és nyissa meg a fő cél a távoli értesítések engedélyezése a **célok** szakasz.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-136">Open your iOS project, and enable remote notifications by going to your main app target in the **Targets** section.</span></span>
2. <span data-ttu-id="bfaaa-137">Kattintson a **képességek**, kapcsolja be a **Háttérmódok**, és ellenőrizze a **távoli értesítések** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-137">Click on **Capabilities**, turn on **Background Modes**, and check the **Remote Notifications** checkbox.</span></span>
   
    ![][IOS3]
3. <span data-ttu-id="bfaaa-138">Nyissa meg a **Main.storyboard**, és győződjön meg arról, hogy a nézetvezérlő (beruházások kezdőlap nézetvezérlő ebben az oktatóanyagban) a [a felhasználó értesítése](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-138">Go to **Main.storyboard**, and make sure you have a View Controller (refered to as Home View Controller in this tutorial) from [Notify User](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tutorial.</span></span>
4. <span data-ttu-id="bfaaa-139">Adja hozzá a **navigációs vezérlő** a storyboard fájlt, és a vezérlő húzás kezdőlap nézetvezérlő abba, hogy a **nézet a gyökérkönyvtár** navigációs.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-139">Add a **Navigation Controller** to your storyboard, and control-drag to Home View Controller to make it the **root view** of navigation.</span></span> <span data-ttu-id="bfaaa-140">Győződjön meg arról, hogy a **kezdeti View-Controller van** attribútumokban inspector csak az navigációs tartományvezérlő van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-140">Make sure the **Is Initial View Controller** in Attributes inspector is selected for the Navigation Controller only.</span></span>
5. <span data-ttu-id="bfaaa-141">Adja hozzá a **nézetvezérlő** történethez, adja hozzá egy **kép**.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-141">Add a **View Controller** to storyboard and add an **Image View**.</span></span> <span data-ttu-id="bfaaa-142">Ez egy, a felhasználók által látható, amennyiben az általuk választott a notifiication kattintva további lapot.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-142">This is the page users will see once they choose to learn more by clicking on the notifiication.</span></span> <span data-ttu-id="bfaaa-143">A storyboard fájlt a következőképpen kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="bfaaa-143">Your storyboard should look as follows:</span></span>
   
    ![][IOS4]
6. <span data-ttu-id="bfaaa-144">Kattintson a a **Home nézetvezérlő** storyboard fájlt, és győződjön meg arról, hogy rendelkezik **homeViewController** , a **egyéni osztály** és **Storyboard azonosító**az identitás-inspector alatt.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-144">Click on the **Home View Controller** in storyboard, and make sure it has **homeViewController** as its **Custom Class** and **Storyboard ID** under the Identity inspector.</span></span>
7. <span data-ttu-id="bfaaa-145">Tegye meg ugyanezt a kép View vezérlő **imageViewController**.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-145">Do the same for Image View Controller as **imageViewController**.</span></span>
8. <span data-ttu-id="bfaaa-146">Ezután hozzon létre egy új nézet vezérlőosztály című **imageViewController** kezelje az újonnan létrehozott felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-146">Then, create a new View Controller class titled **imageViewController** to handle the UI you just created.</span></span>
9. <span data-ttu-id="bfaaa-147">A **imageViewController.h**, adja hozzá a következőket a vezérlő illesztőfelület deklarációjában.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-147">In **imageViewController.h**, add the following to the controller's interface declarations.</span></span> <span data-ttu-id="bfaaa-148">Ügyeljen arra, hogy a vezérlő-húzza a storyboard kép nézetben ezeket a tulajdonságokat, a kettő:</span><span class="sxs-lookup"><span data-stu-id="bfaaa-148">Make sure to control-drag from the storyboard image view to these properties to link the two:</span></span>
   
        @property (weak, nonatomic) IBOutlet UIImageView *myImage;
        @property (strong) UIImage* imagePayload;
10. <span data-ttu-id="bfaaa-149">A **imageViewController.m**, adja hozzá a következő végén **viewDidload**:</span><span class="sxs-lookup"><span data-stu-id="bfaaa-149">In **imageViewController.m**, add the following at the end of **viewDidload**:</span></span>
    
        // Display the UI Image in UI Image View
        [self.myImage setImage:self.imagePayload];
11. <span data-ttu-id="bfaaa-150">A **AppDelegate.m**, a létrehozott lemezképet vezérlő importálása:</span><span class="sxs-lookup"><span data-stu-id="bfaaa-150">In **AppDelegate.m**, import the image controller you created:</span></span>
    
        #import "imageViewController.h"
12. <span data-ttu-id="bfaaa-151">Egy kapcsolat a szakasz a következő nyilatkozattal hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="bfaaa-151">Add an interface section with the following declaration:</span></span>
    
        @interface AppDelegate ()
    
        @property UIImage* imagePayload;
        @property NSDictionary* userInfo;
        @property BOOL iOS8;
    
        // Obtain content from backend with notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;
    
        // Redirect to Image View Controller after notification interaction
        - (void)redirectToImageViewWithImage: (UIImage *)img;
    
        @end
13. <span data-ttu-id="bfaaa-152">A **AppDelegate**, győződjön meg arról, hogy az alkalmazás regisztrálása a beavatkozás nélküli értesítéseket **alkalmazás: didFinishLaunchingWithOptions**:</span><span class="sxs-lookup"><span data-stu-id="bfaaa-152">In **AppDelegate**, make sure your app registers for silent notifications in **application: didFinishLaunchingWithOptions**:</span></span>
    
        // Software version
        self.iOS8 = [[UIApplication sharedApplication] respondsToSelector:@selector(registerUserNotificationSettings:)] && [[UIApplication sharedApplication] respondsToSelector:@selector(registerForRemoteNotifications)];
    
        // Register for remote notifications for iOS8 and previous versions
        if (self.iOS8) {
            NSLog(@"This device is running with iOS8.");
    
            // Action
            UIMutableUserNotificationAction *richPushAction = [[UIMutableUserNotificationAction alloc] init];
            richPushAction.identifier = @"richPushMore";
            richPushAction.activationMode = UIUserNotificationActivationModeForeground;
            richPushAction.authenticationRequired = NO;
            richPushAction.title = @"More";
    
            // Notification category
            UIMutableUserNotificationCategory* richPushCategory = [[UIMutableUserNotificationCategory alloc] init];
            richPushCategory.identifier = @"richPush";
            [richPushCategory setActions:@[richPushAction] forContext:UIUserNotificationActionContextDefault];
    
            // Notification categories
            NSSet* richPushCategories = [NSSet setWithObjects:richPushCategory, nil];

            UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                    UIUserNotificationTypeAlert |
                                                    UIUserNotificationTypeBadge
                                                                                     categories:richPushCategories];

            [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
            [[UIApplication sharedApplication] registerForRemoteNotifications];

        }
        else {
            // Previous iOS versions
            NSLog(@"This device is running with iOS7 or earlier versions.");

            [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
        }

        return YES;

1. <span data-ttu-id="bfaaa-153">A következő végrehajtása parancs **alkalmazás: didRegisterForRemoteNotificationsWithDeviceToken** a felhasználói felület módosítja figyelembe storyboard érvénybe:</span><span class="sxs-lookup"><span data-stu-id="bfaaa-153">Subsitute in the following implementation for **application:didRegisterForRemoteNotificationsWithDeviceToken** to take the storyboard UI changes into account:</span></span>
   
       // Access navigation controller which is at the root of window
       UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
       // Get home view controller from stack on navigation controller
       homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
       hvc.deviceToken = deviceToken;
2. <span data-ttu-id="bfaaa-154">Adja hozzá a következő módszerekkel **AppDelegate.m** a kép beolvasni végpontot, és helyi értesítést küld, ha lekérés teljes.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-154">Then, add the following methods to **AppDelegate.m** to retrieve the image from your endpoint and send a local notification when retrieval is complete.</span></span> <span data-ttu-id="bfaaa-155">Ügyeljen arra, hogy helyettesítse a helyőrző `{backend endpoint}` a háttér-végponthoz:</span><span class="sxs-lookup"><span data-stu-id="bfaaa-155">Make sure to substitute the placeholder `{backend endpoint}` with your backend endpoint:</span></span>
   
       NSString *const GetNotificationEndpoint = @"{backend endpoint}/api/notifications";
   
       // Helper: retrieve notification content from backend with rich notification id
       - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion {
           UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
           homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
           NSString* authenticationHeader = hvc.registerClient.authenticationHeader;
           // Check if authenticated
           if (!authenticationHeader) return;
   
           NSURLSession* session = [NSURLSession
                                    sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                    delegate:nil
                                    delegateQueue:nil];
   
           NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, richId]];
           NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
           [request setHTTPMethod:@"GET"];
           NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
           [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];
   
           NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
   
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
               if (!error && httpResponse.statusCode == 200) {
                   // From NSData to UIImage
                   self.imagePayload = [UIImage imageWithData:data];
   
                   completion(nil);
               }
               else {
                   NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                   if (error)
                       completion(error);
                   else {
                       completion([NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                   }
               }
           }];
           [dataTask resume];
       }
   
       // Handle silent push notifications when id is sent from backend
       - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler {
           self.userInfo = userInfo;
           int richId = [[self.userInfo objectForKey:@"richId"] intValue];
           NSString* richType = [self.userInfo objectForKey:@"richType"];
   
           // Retrieve image data
           if ([richType isEqualToString:@"img"]) {  
               [self retrieveRichImageWithId:richId completion:^(NSError* error) {
                   if (!error){
                       // Send local notification
                       UILocalNotification* localNotification = [[UILocalNotification alloc] init];
   
                       // "5" is arbitrary here to give you enough time to quit out of the app and receive push notifications
                       localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:5];
                       localNotification.userInfo = self.userInfo;
                       localNotification.alertBody = [self.userInfo objectForKey:@"richMessage"];
                       localNotification.timeZone = [NSTimeZone defaultTimeZone];
   
                       // iOS8 categories
                       if (self.iOS8) {
                           localNotification.category = @"richPush";
                       }
   
                       [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];
   
                       handler(UIBackgroundFetchResultNewData);
                   }
                   else{
                       handler(UIBackgroundFetchResultFailed);
                   }
               }];
           }
           // Add "else if" here to handle more types of rich content such as url, sound files, etc.
       }
3. <span data-ttu-id="bfaaa-156">Nyissa meg a kép nézetvezérlő a mentést fent helyi értesítési kezelni **AppDelegate.m** együtt a következőkkel:</span><span class="sxs-lookup"><span data-stu-id="bfaaa-156">Handle the local notification above by opening up the image view controller in **AppDelegate.m** with the following methods:</span></span>
   
       // Helper: redirect users to image view controller
       - (void)redirectToImageViewWithImage: (UIImage *)img {
           UINavigationController *navigationController = (UINavigationController*) self.window.rootViewController;
           UIStoryboard *mainStoryboard = [UIStoryboard storyboardWithName:@"Main"
                                                                    bundle: nil];
           imageViewController *imgViewController = [mainStoryboard instantiateViewControllerWithIdentifier: @"imageViewController"];
           // Pass data/image to image view controller
           imgViewController.imagePayload = img;
   
           // Redirect
           [navigationController pushViewController:imgViewController animated:YES];
       }
   
       // Handle local notification sent above in didReceiveRemoteNotification
       - (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification {
           if (application.applicationState == UIApplicationStateActive) {
               // Show in-app alert with an extra "more" button
               UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:notification.alertBody delegate:self cancelButtonTitle:@"OK" otherButtonTitles:@"More", nil];
   
               [alert show];
           }
           // App becomes active from user's tap on notification
           else {
               [self redirectToImageViewWithImage:self.imagePayload];
           }
       }
   
       // Handle buttons in in-app alerts and redirect with data/image
       - (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex {
           // Handle "more" button
           if (buttonIndex == 1)
           {
               [self redirectToImageViewWithImage:self.imagePayload];
           }
           // Add "else if" here to handle more buttons
       }
   
       // Handle notification setting actions in iOS8
       - (void)application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {
           // Handle richPush related buttons
           if ([identifier isEqualToString:@"richPushMore"]) {
               [self redirectToImageViewWithImage:self.imagePayload];
           }
           completionHandler();
       }

## <a name="run-the-application"></a><span data-ttu-id="bfaaa-157">Futtassa az alkalmazást</span><span class="sxs-lookup"><span data-stu-id="bfaaa-157">Run the Application</span></span>
1. <span data-ttu-id="bfaaa-158">Az xcode-ban az alkalmazás futtatása egy fizikai iOS-eszközön (leküldéses értesítések nem fog működni a szimulátor).</span><span class="sxs-lookup"><span data-stu-id="bfaaa-158">In XCode, run the app on a physical iOS device (push notifications will not work in the simulator).</span></span>
2. <span data-ttu-id="bfaaa-159">Az iOS-alkalmazás felhasználói felületén, adja meg egy felhasználónevet és jelszót ugyanazt az értéket a hitelesítéshez, és kattintson **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-159">In the iOS app UI, enter a username and password of the same value for authentication and click **Log In**.</span></span>
3. <span data-ttu-id="bfaaa-160">Kattintson a **leküldéses küldése** és alkalmazáson belüli riasztást kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-160">Click **Send push** and you should see an in-app alert.</span></span> <span data-ttu-id="bfaaa-161">Ha rákattint az **további**, a kép úgy döntött, hogy szerepeljen a háttéralkalmazás kerül.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-161">If you click on **More**, you will be brought to the image you chose to include in your app backend.</span></span>
4. <span data-ttu-id="bfaaa-162">Is **leküldéses küldése** azonnal kattintson az eszköz az otthoni gombra.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-162">You can also click **Send push** and immediately press the home button of your device.</span></span> <span data-ttu-id="bfaaa-163">Néhány perc múlva egy leküldéses értesítést fog kapni.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-163">In a few moments, you will receive a push notification.</span></span> <span data-ttu-id="bfaaa-164">Ha rajta koppintson vagy kattintson a több, akkor az alkalmazás és a gazdag kép tartalmához kerül.</span><span class="sxs-lookup"><span data-stu-id="bfaaa-164">If you tap on it or click More, you will be brought to your app and the rich image content.</span></span>

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
