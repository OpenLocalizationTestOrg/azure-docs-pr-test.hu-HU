---
title: "Notification Hubs gazdag leküldéses aaaAzure"
description: "Ismerje meg, toosend gazdag a leküldéses értesítések tooan iOS-alkalmazás az Azure-ból. Kódminták Objective-C és C#."
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
ms.openlocfilehash: 5432d8bf47777371bea3521a0c0176ade75fbd9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs-rich-push"></a><span data-ttu-id="84eed-104">Az Azure Notification Hubs gazdag leküldéses</span><span class="sxs-lookup"><span data-stu-id="84eed-104">Azure Notification Hubs Rich Push</span></span>
## <a name="overview"></a><span data-ttu-id="84eed-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="84eed-105">Overview</span></span>
<span data-ttu-id="84eed-106">Egy alkalmazás rendelés tooengage felhasználók azonnali gazdag tartalmával, túl egyszerű szöveges toopush lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="84eed-106">In order tooengage users with instant rich contents, an application might want toopush beyond plain text.</span></span> <span data-ttu-id="84eed-107">Ezek az értesítések felhasználói kommunikációt, valamint a tartalom URL-címek, hangok, képek/kuponok és több például lépteti elő.</span><span class="sxs-lookup"><span data-stu-id="84eed-107">These notifications promote user interactions and  present content such as urls, sounds, images/coupons, and more.</span></span> <span data-ttu-id="84eed-108">Ez az oktatóanyag épít, hello [felhasználók értesítése](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) témakör, és bemutatja a toosend a leküldéses értesítések, amelyek hasznos adat található (például kép).</span><span class="sxs-lookup"><span data-stu-id="84eed-108">This tutorial builds on hello [Notify Users](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) topic, and shows how toosend push notifications that incorporate payloads (for example, image).</span></span>

<span data-ttu-id="84eed-109">Ebben az oktatóanyagban található kompatibilis iOS 7 és a 8.</span><span class="sxs-lookup"><span data-stu-id="84eed-109">This tutorial is compatible with iOS 7 & 8.</span></span>

  ![][IOS1]

<span data-ttu-id="84eed-110">Magas szinten:</span><span class="sxs-lookup"><span data-stu-id="84eed-110">At a high level:</span></span>

1. <span data-ttu-id="84eed-111">hello háttéralkalmazás:</span><span class="sxs-lookup"><span data-stu-id="84eed-111">hello app backend:</span></span>
   * <span data-ttu-id="84eed-112">Tárolók hello gazdag forgalma (ebben az esetben kép) az adatbázis/helyi háttértárolóként hello</span><span class="sxs-lookup"><span data-stu-id="84eed-112">Stores hello rich payload (in this case, image) in hello backend database/local storage</span></span>
   * <span data-ttu-id="84eed-113">Elküldi az gazdag értesítési toohello eszköz azonosítója</span><span class="sxs-lookup"><span data-stu-id="84eed-113">Sends ID of this rich notification toohello device</span></span>
2. <span data-ttu-id="84eed-114">Az alkalmazás hello eszközön:</span><span class="sxs-lookup"><span data-stu-id="84eed-114">App on hello device:</span></span>
   * <span data-ttu-id="84eed-115">Névjegyek hello hello gazdag hasznos kap hello azonosítójú kérő háttér</span><span class="sxs-lookup"><span data-stu-id="84eed-115">Contacts hello backend requesting hello rich payload with hello ID it receives</span></span>
   * <span data-ttu-id="84eed-116">Felhasználók értesítéseket küld hello eszközön, adatok beolvasása befejeződött, és azonnal, amikor a felhasználók további toolearn koppintson a hello hasznos jeleníti meg</span><span class="sxs-lookup"><span data-stu-id="84eed-116">Sends users notifications on hello device when data retrieval is complete, and shows hello payload immediately when users tap toolearn more</span></span>

## <a name="webapi-project"></a><span data-ttu-id="84eed-117">WebAPI projekt</span><span class="sxs-lookup"><span data-stu-id="84eed-117">WebAPI Project</span></span>
1. <span data-ttu-id="84eed-118">A Visual Studióban nyissa meg a hello **AppBackend** hello létrehozott projekt [felhasználók értesítése](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="84eed-118">In Visual Studio, open hello **AppBackend** project that you created in hello [Notify Users](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tutorial.</span></span>
2. <span data-ttu-id="84eed-119">Lemezkép kívánja toonotify rendelkező felhasználók, majd tegye beszerzése egy **img** projektkönyvtárban mappa.</span><span class="sxs-lookup"><span data-stu-id="84eed-119">Obtain an image you would like toonotify users with, and put it in an **img** folder in your project directory.</span></span>
3. <span data-ttu-id="84eed-120">Kattintson a **minden fájl megjelenítése** a hello a Megoldáskezelőben, majd a jobb gombbal a hello mappa túl**közé tartozik a projekt**.</span><span class="sxs-lookup"><span data-stu-id="84eed-120">Click **Show All Files** in hello Solution Explorer, and right-click hello folder too**Include In Project**.</span></span>
4. <span data-ttu-id="84eed-121">Kijelölt hello lemezképpel, módosíthatja a Build művelet tulajdonságai ablakban túl**beágyazott erőforrás**.</span><span class="sxs-lookup"><span data-stu-id="84eed-121">With hello image selected, change its Build Action in Properties window too**Embedded Resource**.</span></span>
   
    ![][IOS2]
5. <span data-ttu-id="84eed-122">A **Notifications.cs**, adja hozzá hello következő using utasítást:</span><span class="sxs-lookup"><span data-stu-id="84eed-122">In **Notifications.cs**, add hello following using statement:</span></span>
   
        using System.Reflection;
6. <span data-ttu-id="84eed-123">Teljes frissítés hello **értesítések** a következő kód hello osztályt.</span><span class="sxs-lookup"><span data-stu-id="84eed-123">Update hello whole **Notifications** class with hello following code.</span></span> <span data-ttu-id="84eed-124">Lehet, hogy tooreplace hello helyőrzőt a notification hub hitelesítő adatait, és a kép fájlnevét.</span><span class="sxs-lookup"><span data-stu-id="84eed-124">Be sure tooreplace hello placeholders with your notification hub credentials and image file name.</span></span>
   
        public class Notification {
            public int Id { get; set; }
            // Initial notification message toodisplay toousers
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
                // Placeholders: replace with hello connection string (with full access) for your notification hub and hello hub name from hello Azure Classics Portal
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
   > <span data-ttu-id="84eed-125">(választható) Tekintse meg a túl[hogyan tooembed és a hozzáférési erőforrások Visual C# használatával](http://support.microsoft.com/kb/319292) további információt a tooadd, és szerezze be a projekt erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="84eed-125">(optional) Refer too[How tooembed and access resources by using Visual C#](http://support.microsoft.com/kb/319292) for more information on how tooadd and obtain project resources.</span></span>
   > 
   > 
7. <span data-ttu-id="84eed-126">A **NotificationsController.cs**, újradefiniálása **NotificationsController** az alábbi kódtöredékek hello.</span><span class="sxs-lookup"><span data-stu-id="84eed-126">In **NotificationsController.cs**, redefine **NotificationsController**  with hello following snippets.</span></span> <span data-ttu-id="84eed-127">Ez az első csendes gazdag értesítés azonosító toodevice küld, és lehetővé teszi, hogy a kép lekérése az ügyféloldali:</span><span class="sxs-lookup"><span data-stu-id="84eed-127">This sends an initial silent rich notification id toodevice and allows client-side retrieval of image:</span></span>
   
        // Return http response with image binary
        public HttpResponseMessage Get(int id) {
            var stream = Notifications.Instance.ReadImage(id);
   
            var result = new HttpResponseMessage(HttpStatusCode.OK);
            result.Content = new StreamContent(stream);
            // Switch in your image extension for "png"
            result.Content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/{png}");
   
            return result;
        }
   
        // Create rich notification and send initial silent notification (containing id) tooclient
        public async Task<HttpResponseMessage> Post() {
            // Replace hello placeholder with image file name
            var richNotificationInTheBackend = Notifications.Instance.CreateNotification("Check this image out!", "img",  "{logo.png}");
   
            var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;
   
            // Silent notification with content available
            var aboutUser = "{\"aps\": {\"content-available\": 1, \"sound\":\"\"}, \"richId\": \"" + richNotificationInTheBackend.Id.ToString() + "\",  \"richMessage\": \"" + richNotificationInTheBackend.Message + "\", \"richType\": \"" + richNotificationInTheBackend.RichType + "\"}";
   
            // Send notification tooapns
            await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(aboutUser, usernameTag);
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
8. <span data-ttu-id="84eed-128">Most fogjuk újra üzembe helyezni az alkalmazás tooan Azure-webhely rendelés toomake azt minden eszköz érhető el.</span><span class="sxs-lookup"><span data-stu-id="84eed-128">Now we will re-deploy this app tooan Azure Website in order toomake it accessible from all devices.</span></span> <span data-ttu-id="84eed-129">Kattintson a jobb gombbal a hello **AppBackend** projektre, és válassza ki **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="84eed-129">Right-click on hello **AppBackend** project and select **Publish**.</span></span>
9. <span data-ttu-id="84eed-130">A közzétételi célként válassza az Azure webhelyén.</span><span class="sxs-lookup"><span data-stu-id="84eed-130">Select Azure Website as your publish target.</span></span> <span data-ttu-id="84eed-131">Jelentkezzen be az Azure-fiókjával, és válassza ki a meglévő vagy új webhely létrehozása, és jegyezze fel a hello **URL-címre** hello tulajdonság **kapcsolat** fülre. Toothis URL-CÍMÉRE hivatkozik a *háttér végpont* ebben az oktatóanyagban később.</span><span class="sxs-lookup"><span data-stu-id="84eed-131">Log in with your Azure account and select an existing or new Website, and make a note of hello **destination URL** property in hello **Connection** tab. We will refer toothis URL as your *backend endpoint* later in this tutorial.</span></span> <span data-ttu-id="84eed-132">Kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="84eed-132">Click **Publish**.</span></span>

## <a name="modify-hello-ios-project"></a><span data-ttu-id="84eed-133">IOS-projekt hello módosítása</span><span class="sxs-lookup"><span data-stu-id="84eed-133">Modify hello iOS project</span></span>
<span data-ttu-id="84eed-134">Most, hogy az alkalmazás háttér toosend csak hello módosította *azonosító* egy értesítés, módosítja az iOS app toohandle azonosító és gazdag üdvözlőüzenetére beolvasni a háttérrendszerből.</span><span class="sxs-lookup"><span data-stu-id="84eed-134">Now that you have modified your app backend toosend just hello *id* of a notification, you will change your iOS app toohandle that id and retrieve hello rich message from your backend.</span></span>

1. <span data-ttu-id="84eed-135">Nyissa meg az iOS-projekthez, és engedélyezze a távoli értesítéseket fog tooyour fő cél a hello **célok** szakasz.</span><span class="sxs-lookup"><span data-stu-id="84eed-135">Open your iOS project, and enable remote notifications by going tooyour main app target in hello **Targets** section.</span></span>
2. <span data-ttu-id="84eed-136">Kattintson a **képességek**, kapcsolja be a **Háttérmódok**, és ellenőrizze a hello **távoli értesítések** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="84eed-136">Click on **Capabilities**, turn on **Background Modes**, and check hello **Remote Notifications** checkbox.</span></span>
   
    ![][IOS3]
3. <span data-ttu-id="84eed-137">Nyissa meg túl**Main.storyboard**, és győződjön meg arról, hogy a nézetvezérlő (beruházások tooas Home nézetvezérlő ebben az oktatóanyagban) a [a felhasználó értesítése](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="84eed-137">Go too**Main.storyboard**, and make sure you have a View Controller (refered tooas Home View Controller in this tutorial) from [Notify User](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tutorial.</span></span>
4. <span data-ttu-id="84eed-138">Adja hozzá a **navigációs vezérlő** tooyour történethez és a Ctrl + húzás tooHome nézetvezérlő toomake azt hello **nézet a gyökérkönyvtár** navigációs.</span><span class="sxs-lookup"><span data-stu-id="84eed-138">Add a **Navigation Controller** tooyour storyboard, and control-drag tooHome View Controller toomake it hello **root view** of navigation.</span></span> <span data-ttu-id="84eed-139">Győződjön meg arról, hogy hello **kezdeti View-Controller van** attribútumokban inspector hello navigációs vezérlő csak az van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="84eed-139">Make sure hello **Is Initial View Controller** in Attributes inspector is selected for hello Navigation Controller only.</span></span>
5. <span data-ttu-id="84eed-140">Adja hozzá a **nézetvezérlő** toostoryboard, és adja hozzá egy **kép**.</span><span class="sxs-lookup"><span data-stu-id="84eed-140">Add a **View Controller** toostoryboard and add an **Image View**.</span></span> <span data-ttu-id="84eed-141">Ez az, hogy a felhasználók által látható, miután választ toolearn több hello notifiication kattintva hello oldal.</span><span class="sxs-lookup"><span data-stu-id="84eed-141">This is hello page users will see once they choose toolearn more by clicking on hello notifiication.</span></span> <span data-ttu-id="84eed-142">A storyboard fájlt a következőképpen kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="84eed-142">Your storyboard should look as follows:</span></span>
   
    ![][IOS4]
6. <span data-ttu-id="84eed-143">Kattintson a hello **Home nézetvezérlő** storyboard fájlt, és győződjön meg arról, hogy rendelkezik **homeViewController** , a **egyéni osztály** és **Storyboard azonosító**hello identitás inspector alatt.</span><span class="sxs-lookup"><span data-stu-id="84eed-143">Click on hello **Home View Controller** in storyboard, and make sure it has **homeViewController** as its **Custom Class** and **Storyboard ID** under hello Identity inspector.</span></span>
7. <span data-ttu-id="84eed-144">Azonos kép View vezérlő hello **imageViewController**.</span><span class="sxs-lookup"><span data-stu-id="84eed-144">Do hello same for Image View Controller as **imageViewController**.</span></span>
8. <span data-ttu-id="84eed-145">Ezután hozzon létre egy új nézet vezérlőosztály című **imageViewController** toohandle hello újonnan létrehozott felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="84eed-145">Then, create a new View Controller class titled **imageViewController** toohandle hello UI you just created.</span></span>
9. <span data-ttu-id="84eed-146">A **imageViewController.h**, adja hozzá a következő toohello vezérlő felület deklarációja hello.</span><span class="sxs-lookup"><span data-stu-id="84eed-146">In **imageViewController.h**, add hello following toohello controller's interface declarations.</span></span> <span data-ttu-id="84eed-147">Győződjön meg arról, hogy toocontrol-közben húzza hello storyboard kép nézet toothese tulajdonságok toolink hello két:</span><span class="sxs-lookup"><span data-stu-id="84eed-147">Make sure toocontrol-drag from hello storyboard image view toothese properties toolink hello two:</span></span>
   
        @property (weak, nonatomic) IBOutlet UIImageView *myImage;
        @property (strong) UIImage* imagePayload;
10. <span data-ttu-id="84eed-148">A **imageViewController.m**, adja hozzá a hello következő hello végén lévő **viewDidload**:</span><span class="sxs-lookup"><span data-stu-id="84eed-148">In **imageViewController.m**, add hello following at hello end of **viewDidload**:</span></span>
    
        // Display hello UI Image in UI Image View
        [self.myImage setImage:self.imagePayload];
11. <span data-ttu-id="84eed-149">A **AppDelegate.m**, importálás hello kép vezérlő létrehozott:</span><span class="sxs-lookup"><span data-stu-id="84eed-149">In **AppDelegate.m**, import hello image controller you created:</span></span>
    
        #import "imageViewController.h"
12. <span data-ttu-id="84eed-150">Vegyen fel egy felület szakaszt tartalmazó hello deklaráció a következő:</span><span class="sxs-lookup"><span data-stu-id="84eed-150">Add an interface section with hello following declaration:</span></span>
    
        @interface AppDelegate ()
    
        @property UIImage* imagePayload;
        @property NSDictionary* userInfo;
        @property BOOL iOS8;
    
        // Obtain content from backend with notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;
    
        // Redirect tooImage View Controller after notification interaction
        - (void)redirectToImageViewWithImage: (UIImage *)img;
    
        @end
13. <span data-ttu-id="84eed-151">A **AppDelegate**, győződjön meg arról, hogy az alkalmazás regisztrálása a beavatkozás nélküli értesítéseket **alkalmazás: didFinishLaunchingWithOptions**:</span><span class="sxs-lookup"><span data-stu-id="84eed-151">In **AppDelegate**, make sure your app registers for silent notifications in **application: didFinishLaunchingWithOptions**:</span></span>
    
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

1. <span data-ttu-id="84eed-152">A következő megvalósítását hello parancs **alkalmazás: didRegisterForRemoteNotificationsWithDeviceToken** tootake hello storyboard figyelembe módosítja felhasználói felületén:</span><span class="sxs-lookup"><span data-stu-id="84eed-152">Subsitute in hello following implementation for **application:didRegisterForRemoteNotificationsWithDeviceToken** tootake hello storyboard UI changes into account:</span></span>
   
       // Access navigation controller which is at hello root of window
       UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
       // Get home view controller from stack on navigation controller
       homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
       hvc.deviceToken = deviceToken;
2. <span data-ttu-id="84eed-153">Adja hozzá a következő módszerek túl hello**AppDelegate.m** tooretrieve hello a végpont lemezképét, és helyi értesítés küldéséhez lekérés befejezésekor.</span><span class="sxs-lookup"><span data-stu-id="84eed-153">Then, add hello following methods too**AppDelegate.m** tooretrieve hello image from your endpoint and send a local notification when retrieval is complete.</span></span> <span data-ttu-id="84eed-154">Győződjön meg arról, hogy toosubstitute hello helyőrző `{backend endpoint}` a háttér-végponthoz:</span><span class="sxs-lookup"><span data-stu-id="84eed-154">Make sure toosubstitute hello placeholder `{backend endpoint}` with your backend endpoint:</span></span>
   
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
                   // From NSData tooUIImage
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
   
                       // "5" is arbitrary here toogive you enough time tooquit out of hello app and receive push notifications
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
           // Add "else if" here toohandle more types of rich content such as url, sound files, etc.
       }
3. <span data-ttu-id="84eed-155">Hello helyi értesítési fent kezelni hello kép nézetvezérlő a mentést megnyitásával **AppDelegate.m** a következő módszerek hello:</span><span class="sxs-lookup"><span data-stu-id="84eed-155">Handle hello local notification above by opening up hello image view controller in **AppDelegate.m** with hello following methods:</span></span>
   
       // Helper: redirect users tooimage view controller
       - (void)redirectToImageViewWithImage: (UIImage *)img {
           UINavigationController *navigationController = (UINavigationController*) self.window.rootViewController;
           UIStoryboard *mainStoryboard = [UIStoryboard storyboardWithName:@"Main"
                                                                    bundle: nil];
           imageViewController *imgViewController = [mainStoryboard instantiateViewControllerWithIdentifier: @"imageViewController"];
           // Pass data/image tooimage view controller
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
           // Add "else if" here toohandle more buttons
       }
   
       // Handle notification setting actions in iOS8
       - (void)application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {
           // Handle richPush related buttons
           if ([identifier isEqualToString:@"richPushMore"]) {
               [self redirectToImageViewWithImage:self.imagePayload];
           }
           completionHandler();
       }

## <a name="run-hello-application"></a><span data-ttu-id="84eed-156">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="84eed-156">Run hello Application</span></span>
1. <span data-ttu-id="84eed-157">Az XCode-ban hello alkalmazás futtatása egy fizikai iOS-eszközön (leküldéses értesítések hello szimulátor fog működni).</span><span class="sxs-lookup"><span data-stu-id="84eed-157">In XCode, run hello app on a physical iOS device (push notifications will not work in hello simulator).</span></span>
2. <span data-ttu-id="84eed-158">A hello iOS-alkalmazás felhasználói felületén, írja be a felhasználónevet és jelszót hello ugyanazt a hitelesítési értékét, és kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="84eed-158">In hello iOS app UI, enter a username and password of hello same value for authentication and click **Log In**.</span></span>
3. <span data-ttu-id="84eed-159">Kattintson a **leküldéses küldése** és alkalmazáson belüli riasztást kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="84eed-159">Click **Send push** and you should see an in-app alert.</span></span> <span data-ttu-id="84eed-160">Ha rákattint az **további**, tooinclude lehetőséget választotta a háttéralkalmazás hozható toohello lemezképet fog.</span><span class="sxs-lookup"><span data-stu-id="84eed-160">If you click on **More**, you will be brought toohello image you chose tooinclude in your app backend.</span></span>
4. <span data-ttu-id="84eed-161">Is **leküldéses küldése** azonnal nyomja le az ENTER hello otthoni gombra az eszköz.</span><span class="sxs-lookup"><span data-stu-id="84eed-161">You can also click **Send push** and immediately press hello home button of your device.</span></span> <span data-ttu-id="84eed-162">Néhány perc múlva egy leküldéses értesítést fog kapni.</span><span class="sxs-lookup"><span data-stu-id="84eed-162">In a few moments, you will receive a push notification.</span></span> <span data-ttu-id="84eed-163">Ha rajta koppintson vagy kattintson a több, jut tooyour alkalmazást, és hello gazdag kép tartalmához.</span><span class="sxs-lookup"><span data-stu-id="84eed-163">If you tap on it or click More, you will be brought tooyour app and hello rich image content.</span></span>

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
