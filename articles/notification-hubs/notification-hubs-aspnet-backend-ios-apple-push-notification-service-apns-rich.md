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
# <a name="azure-notification-hubs-rich-push"></a>Az Azure Notification Hubs gazdag leküldéses
## <a name="overview"></a>Áttekintés
Egy alkalmazás rendelés tooengage felhasználók azonnali gazdag tartalmával, túl egyszerű szöveges toopush lehet szükség. Ezek az értesítések felhasználói kommunikációt, valamint a tartalom URL-címek, hangok, képek/kuponok és több például lépteti elő. Ez az oktatóanyag épít, hello [felhasználók értesítése](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) témakör, és bemutatja a toosend a leküldéses értesítések, amelyek hasznos adat található (például kép).

Ebben az oktatóanyagban található kompatibilis iOS 7 és a 8.

  ![][IOS1]

Magas szinten:

1. hello háttéralkalmazás:
   * Tárolók hello gazdag forgalma (ebben az esetben kép) az adatbázis/helyi háttértárolóként hello
   * Elküldi az gazdag értesítési toohello eszköz azonosítója
2. Az alkalmazás hello eszközön:
   * Névjegyek hello hello gazdag hasznos kap hello azonosítójú kérő háttér
   * Felhasználók értesítéseket küld hello eszközön, adatok beolvasása befejeződött, és azonnal, amikor a felhasználók további toolearn koppintson a hello hasznos jeleníti meg

## <a name="webapi-project"></a>WebAPI projekt
1. A Visual Studióban nyissa meg a hello **AppBackend** hello létrehozott projekt [felhasználók értesítése](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) oktatóanyag.
2. Lemezkép kívánja toonotify rendelkező felhasználók, majd tegye beszerzése egy **img** projektkönyvtárban mappa.
3. Kattintson a **minden fájl megjelenítése** a hello a Megoldáskezelőben, majd a jobb gombbal a hello mappa túl**közé tartozik a projekt**.
4. Kijelölt hello lemezképpel, módosíthatja a Build művelet tulajdonságai ablakban túl**beágyazott erőforrás**.
   
    ![][IOS2]
5. A **Notifications.cs**, adja hozzá hello következő using utasítást:
   
        using System.Reflection;
6. Teljes frissítés hello **értesítések** a következő kód hello osztályt. Lehet, hogy tooreplace hello helyőrzőt a notification hub hitelesítő adatait, és a kép fájlnevét.
   
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
   > (választható) Tekintse meg a túl[hogyan tooembed és a hozzáférési erőforrások Visual C# használatával](http://support.microsoft.com/kb/319292) további információt a tooadd, és szerezze be a projekt erőforrásokat.
   > 
   > 
7. A **NotificationsController.cs**, újradefiniálása **NotificationsController** az alábbi kódtöredékek hello. Ez az első csendes gazdag értesítés azonosító toodevice küld, és lehetővé teszi, hogy a kép lekérése az ügyféloldali:
   
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
8. Most fogjuk újra üzembe helyezni az alkalmazás tooan Azure-webhely rendelés toomake azt minden eszköz érhető el. Kattintson a jobb gombbal a hello **AppBackend** projektre, és válassza ki **közzététel**.
9. A közzétételi célként válassza az Azure webhelyén. Jelentkezzen be az Azure-fiókjával, és válassza ki a meglévő vagy új webhely létrehozása, és jegyezze fel a hello **URL-címre** hello tulajdonság **kapcsolat** fülre. Toothis URL-CÍMÉRE hivatkozik a *háttér végpont* ebben az oktatóanyagban később. Kattintson a **Publish** (Közzététel) gombra.

## <a name="modify-hello-ios-project"></a>IOS-projekt hello módosítása
Most, hogy az alkalmazás háttér toosend csak hello módosította *azonosító* egy értesítés, módosítja az iOS app toohandle azonosító és gazdag üdvözlőüzenetére beolvasni a háttérrendszerből.

1. Nyissa meg az iOS-projekthez, és engedélyezze a távoli értesítéseket fog tooyour fő cél a hello **célok** szakasz.
2. Kattintson a **képességek**, kapcsolja be a **Háttérmódok**, és ellenőrizze a hello **távoli értesítések** jelölőnégyzetet.
   
    ![][IOS3]
3. Nyissa meg túl**Main.storyboard**, és győződjön meg arról, hogy a nézetvezérlő (beruházások tooas Home nézetvezérlő ebben az oktatóanyagban) a [a felhasználó értesítése](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) oktatóanyag.
4. Adja hozzá a **navigációs vezérlő** tooyour történethez és a Ctrl + húzás tooHome nézetvezérlő toomake azt hello **nézet a gyökérkönyvtár** navigációs. Győződjön meg arról, hogy hello **kezdeti View-Controller van** attribútumokban inspector hello navigációs vezérlő csak az van kiválasztva.
5. Adja hozzá a **nézetvezérlő** toostoryboard, és adja hozzá egy **kép**. Ez az, hogy a felhasználók által látható, miután választ toolearn több hello notifiication kattintva hello oldal. A storyboard fájlt a következőképpen kell kinéznie:
   
    ![][IOS4]
6. Kattintson a hello **Home nézetvezérlő** storyboard fájlt, és győződjön meg arról, hogy rendelkezik **homeViewController** , a **egyéni osztály** és **Storyboard azonosító**hello identitás inspector alatt.
7. Azonos kép View vezérlő hello **imageViewController**.
8. Ezután hozzon létre egy új nézet vezérlőosztály című **imageViewController** toohandle hello újonnan létrehozott felhasználói felületén.
9. A **imageViewController.h**, adja hozzá a következő toohello vezérlő felület deklarációja hello. Győződjön meg arról, hogy toocontrol-közben húzza hello storyboard kép nézet toothese tulajdonságok toolink hello két:
   
        @property (weak, nonatomic) IBOutlet UIImageView *myImage;
        @property (strong) UIImage* imagePayload;
10. A **imageViewController.m**, adja hozzá a hello következő hello végén lévő **viewDidload**:
    
        // Display hello UI Image in UI Image View
        [self.myImage setImage:self.imagePayload];
11. A **AppDelegate.m**, importálás hello kép vezérlő létrehozott:
    
        #import "imageViewController.h"
12. Vegyen fel egy felület szakaszt tartalmazó hello deklaráció a következő:
    
        @interface AppDelegate ()
    
        @property UIImage* imagePayload;
        @property NSDictionary* userInfo;
        @property BOOL iOS8;
    
        // Obtain content from backend with notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;
    
        // Redirect tooImage View Controller after notification interaction
        - (void)redirectToImageViewWithImage: (UIImage *)img;
    
        @end
13. A **AppDelegate**, győződjön meg arról, hogy az alkalmazás regisztrálása a beavatkozás nélküli értesítéseket **alkalmazás: didFinishLaunchingWithOptions**:
    
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

1. A következő megvalósítását hello parancs **alkalmazás: didRegisterForRemoteNotificationsWithDeviceToken** tootake hello storyboard figyelembe módosítja felhasználói felületén:
   
       // Access navigation controller which is at hello root of window
       UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
       // Get home view controller from stack on navigation controller
       homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
       hvc.deviceToken = deviceToken;
2. Adja hozzá a következő módszerek túl hello**AppDelegate.m** tooretrieve hello a végpont lemezképét, és helyi értesítés küldéséhez lekérés befejezésekor. Győződjön meg arról, hogy toosubstitute hello helyőrző `{backend endpoint}` a háttér-végponthoz:
   
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
3. Hello helyi értesítési fent kezelni hello kép nézetvezérlő a mentést megnyitásával **AppDelegate.m** a következő módszerek hello:
   
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

## <a name="run-hello-application"></a>Hello alkalmazás futtatása
1. Az XCode-ban hello alkalmazás futtatása egy fizikai iOS-eszközön (leküldéses értesítések hello szimulátor fog működni).
2. A hello iOS-alkalmazás felhasználói felületén, írja be a felhasználónevet és jelszót hello ugyanazt a hitelesítési értékét, és kattintson a **bejelentkezés**.
3. Kattintson a **leküldéses küldése** és alkalmazáson belüli riasztást kell megjelennie. Ha rákattint az **további**, tooinclude lehetőséget választotta a háttéralkalmazás hozható toohello lemezképet fog.
4. Is **leküldéses küldése** azonnal nyomja le az ENTER hello otthoni gombra az eszköz. Néhány perc múlva egy leküldéses értesítést fog kapni. Ha rajta koppintson vagy kattintson a több, jut tooyour alkalmazást, és hello gazdag kép tartalmához.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
