---
title: "Az aktuális felhasználó a leküldéses értesítések regisztrálása webes API használatával |} Microsoft Docs"
description: "Útmutató az iOS-alkalmazásoknak az Azure Notification Hubs a leküldéses értesítési regisztrációban kérése regisztrálás ASP.NET Web API hajtja végre."
services: notification-hubs
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
ms.assetid: 4e3772cf-20db-4b9f-bb74-886adfaaa65d
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: fd56bb2dd627b31f00363851a4e76484aa382988
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="register-the-current-user-for-push-notifications-by-using-aspnet"></a><span data-ttu-id="00259-103">Az aktuális felhasználó a leküldéses értesítések regisztrálása ASP.NET használatával</span><span class="sxs-lookup"><span data-stu-id="00259-103">Register the current user for push notifications by using ASP.NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="00259-104">iOS</span><span class="sxs-lookup"><span data-stu-id="00259-104">iOS</span></span>](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="00259-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="00259-105">Overview</span></span>
<span data-ttu-id="00259-106">Ez a témakör bemutatja, hogyan kérjen az Azure Notification Hubs leküldéses értesítési regisztrációban, ha regisztrációs ASP.NET Web API hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="00259-106">This topic shows you how to request push notification registration with Azure Notification Hubs when registration is performed by ASP.NET Web API.</span></span> <span data-ttu-id="00259-107">Ez a témakör kibővíti az oktatóanyag [értesítse a felhasználókat a Notification hubs használatával].</span><span class="sxs-lookup"><span data-stu-id="00259-107">This topic extends the tutorial [Notify users with Notification Hubs].</span></span> <span data-ttu-id="00259-108">Már végrehajtotta a szükséges lépéseket, hogy az oktatóanyagban a hitelesített mobilszolgáltatás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="00259-108">You must have already completed the required steps in that tutorial to create the authenticated mobile service.</span></span> <span data-ttu-id="00259-109">Az értesítendő felhasználók forgatókönyvön további információkért lásd: [értesítse a felhasználókat a Notification hubs használatával].</span><span class="sxs-lookup"><span data-stu-id="00259-109">For more information on the notify users scenario, see [Notify users with Notification Hubs].</span></span>

## <a name="update-your-app"></a><span data-ttu-id="00259-110">Az alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="00259-110">Update your app</span></span>
1. <span data-ttu-id="00259-111">A MainStoryboard_iPhone.storyboard adja hozzá a következő összetevőket az objektumtárból:</span><span class="sxs-lookup"><span data-stu-id="00259-111">In your MainStoryboard_iPhone.storyboard, add the following components from the object library:</span></span>
   
   * <span data-ttu-id="00259-112">**Címke**: "A Notification Hubs felhasználói leküldése"</span><span class="sxs-lookup"><span data-stu-id="00259-112">**Label**: "Push to User with Notification Hubs"</span></span>
   * <span data-ttu-id="00259-113">**Címke**: "Végrehajtott"</span><span class="sxs-lookup"><span data-stu-id="00259-113">**Label**: "InstallationId"</span></span>
   * <span data-ttu-id="00259-114">**Címke**: "User"</span><span class="sxs-lookup"><span data-stu-id="00259-114">**Label**: "User"</span></span>
   * <span data-ttu-id="00259-115">**Szövegmező**: "User"</span><span class="sxs-lookup"><span data-stu-id="00259-115">**Text Field**: "User"</span></span>
   * <span data-ttu-id="00259-116">**Címke**: "Password"</span><span class="sxs-lookup"><span data-stu-id="00259-116">**Label**: "Password"</span></span>
   * <span data-ttu-id="00259-117">**Szövegmező**: "Password"</span><span class="sxs-lookup"><span data-stu-id="00259-117">**Text Field**: "Password"</span></span>
   * <span data-ttu-id="00259-118">**Gomb**: "Bejelentkezés"</span><span class="sxs-lookup"><span data-stu-id="00259-118">**Button**: "Login"</span></span>
     
     <span data-ttu-id="00259-119">Ezen a ponton a storyboard néz ki a következő:</span><span class="sxs-lookup"><span data-stu-id="00259-119">At this point, your storyboard looks like the following:</span></span>
     
      ![][0]
2. <span data-ttu-id="00259-120">Segéd-szerkesztőben kimeneteket a kapcsolt vezérlők létrehozása, keresheti őket, az mezők csatlakozzon a nézetvezérlő (delegált), és hozzon létre egy **művelet** a a **bejelentkezési** gombra.</span><span class="sxs-lookup"><span data-stu-id="00259-120">In the assistant editor, create outlets for all the switched controls and call them, connect the text fields with the View Controller (delegate), and create an **Action** for the **login** button.</span></span>
   
       ![][1]
   
       Your BreakingNewsViewController.h file should now contain the following code:
   
        @property (weak, nonatomic) IBOutlet UILabel *installationId;
        @property (weak, nonatomic) IBOutlet UITextField *User;
        @property (weak, nonatomic) IBOutlet UITextField *Password;
   
        - (IBAction)login:(id)sender;
3. <span data-ttu-id="00259-121">Hozzon létre egy osztályt **deviceinfo információja**, és másolja az alábbi kódot a fájl DeviceInfo.h felület szakaszába:</span><span class="sxs-lookup"><span data-stu-id="00259-121">Create a class named **DeviceInfo**, and copy the following code into the interface section of the file DeviceInfo.h:</span></span>
   
        @property (readonly, nonatomic) NSString* installationId;
        @property (nonatomic) NSData* deviceToken;
4. <span data-ttu-id="00259-122">Az implementációs szakaszban DeviceInfo.m fájl másolja az alábbi kódot:</span><span class="sxs-lookup"><span data-stu-id="00259-122">Copy the following code in the implementation section of the DeviceInfo.m file:</span></span>
   
            @synthesize installationId = _installationId;
   
            - (id)init {
                if (!(self = [super init]))
                    return nil;
   
                NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
                _installationId = [defaults stringForKey:@"PushToUserInstallationId"];
                if(!_installationId) {
                    CFUUIDRef newUUID = CFUUIDCreate(kCFAllocatorDefault);
                    _installationId = (__bridge_transfer NSString *)CFUUIDCreateString(kCFAllocatorDefault, newUUID);
                    CFRelease(newUUID);
   
                    //store the install ID so we don't generate a new one next time
                    [defaults setObject:_installationId forKey:@"PushToUserInstallationId"];
                    [defaults synchronize];
                }
   
                return self;
            }
   
            - (NSString*)getDeviceTokenInHex {
                const unsigned *tokenBytes = [[self deviceToken] bytes];
                NSString *hexToken = [NSString stringWithFormat:@"%08X%08X%08X%08X%08X%08X%08X%08X",
                                      ntohl(tokenBytes[0]), ntohl(tokenBytes[1]), ntohl(tokenBytes[2]),
                                      ntohl(tokenBytes[3]), ntohl(tokenBytes[4]), ntohl(tokenBytes[5]),
                                      ntohl(tokenBytes[6]), ntohl(tokenBytes[7])];
                return hexToken;
            }
5. <span data-ttu-id="00259-123">PushToUserAppDelegate.h adja hozzá a következő tulajdonság egypéldányos:</span><span class="sxs-lookup"><span data-stu-id="00259-123">In PushToUserAppDelegate.h, add the following property singleton:</span></span>
   
        @property (strong, nonatomic) DeviceInfo* deviceInfo;
6. <span data-ttu-id="00259-124">Az a **didFinishLaunchingWithOptions** PushToUserAppDelegate.m, metódusban adja hozzá a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="00259-124">In the **didFinishLaunchingWithOptions** method in PushToUserAppDelegate.m, add the following code:</span></span>
   
        self.deviceInfo = [[DeviceInfo alloc] init];
   
        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
   
    <span data-ttu-id="00259-125">Az első sor inicializálja a **deviceinfo információja** egypéldányos.</span><span class="sxs-lookup"><span data-stu-id="00259-125">The first line initializes the **DeviceInfo** singleton.</span></span> <span data-ttu-id="00259-126">A második sor elindul a regisztráció a leküldéses értesítések, amelyek már létezik rendszer már végrehajtotta a [Ismerkedés a Notification Hubs] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="00259-126">The second line starts the registration for push notifications, which is already present is you have already completed the [Get Started with Notification Hubs] tutorial.</span></span>
7. <span data-ttu-id="00259-127">A PushToUserAppDelegate.m, valósítja meg a **didRegisterForRemoteNotificationsWithDeviceToken** a AppDelegate a, és adja hozzá a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="00259-127">In PushToUserAppDelegate.m, implement the method **didRegisterForRemoteNotificationsWithDeviceToken** in your AppDelegate and add the following code:</span></span>
   
        self.deviceInfo.deviceToken = deviceToken;
   
    <span data-ttu-id="00259-128">Ez a beállítás az eszköz jogkivonatát a kérelemhez.</span><span class="sxs-lookup"><span data-stu-id="00259-128">This sets the device token for the request.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="00259-129">Ezen a ponton nem lehetnek többi kód az ezzel a módszerrel.</span><span class="sxs-lookup"><span data-stu-id="00259-129">At this point, there should not be any other code in this method.</span></span> <span data-ttu-id="00259-130">Ha már rendelkezik egy hívás a **registerNativeWithDeviceToken** metódus befejezése felvett a [Ismerkedés a Notification Hubs](/manage/services/notification-hubs/get-started-notification-hubs-ios/) oktatóanyag, kell Megjegyzés kimenő, vagy távolítsa el az adott hívás.</span><span class="sxs-lookup"><span data-stu-id="00259-130">If you already have a call to the **registerNativeWithDeviceToken** method that was added when you completed the [Get Started with Notification Hubs](/manage/services/notification-hubs/get-started-notification-hubs-ios/) tutorial, you must comment-out or remove that call.</span></span>
   > 
   > 
8. <span data-ttu-id="00259-131">A PushToUserAppDelegate.m fájlban adja hozzá a következő kezelő metódust:</span><span class="sxs-lookup"><span data-stu-id="00259-131">In the PushToUserAppDelegate.m file, add the following handler method:</span></span>
   
   * <span data-ttu-id="00259-132">(a "void") alkalmazás:(UIApplication *) alkalmazás didReceiveRemoteNotification:(NSDictionary *) userInfo {NSLog (@"% @", userInfo);   UIAlertView * riasztás = [[UIAlertView foglalási] initWithTitle:@"Notification" üzenetet: [userInfo objectForKey:@"inAppMessage"] delegált: üres cancelButtonTitle: @"OK" otherButtonTitles:nil, üres];   [riasztás megjelenítése]; }</span><span class="sxs-lookup"><span data-stu-id="00259-132">(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {   NSLog(@"%@", userInfo);   UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:                         [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:                         @"OK" otherButtonTitles:nil, nil];   [alert show]; }</span></span>
   
   <span data-ttu-id="00259-133">Ez a módszer a felhasználói felületen megjelenít egy riasztást, amikor az alkalmazás futtatása értesítéseket fogad.</span><span class="sxs-lookup"><span data-stu-id="00259-133">This method displays an alert in the UI when your app receives notifications while it is running.</span></span>
9. <span data-ttu-id="00259-134">Nyissa meg a PushToUserViewController.m fájlt, és térjen vissza a billentyűzet a következő megvalósításában:</span><span class="sxs-lookup"><span data-stu-id="00259-134">Open the PushToUserViewController.m file, and return the keyboard in the following implementation:</span></span>
   
        - (BOOL)textFieldShouldReturn:(UITextField *)theTextField {
            if (theTextField == self.User || theTextField == self.Password) {
                [theTextField resignFirstResponder];
            }
            return YES;
        }
10. <span data-ttu-id="00259-135">Az a **viewDidLoad** PushToUserViewController.m fájlban metódus inicializálni a végrehajtott címke az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="00259-135">In the **viewDidLoad** method in the PushToUserViewController.m file, initialize the installationId label as follows:</span></span>
    
         DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
         Self.installationId.text = deviceInfo.installationId;
11. <span data-ttu-id="00259-136">Adja hozzá a következő tulajdonságok PushToUserViewController.m felülettel:</span><span class="sxs-lookup"><span data-stu-id="00259-136">Add the following properties in interface in PushToUserViewController.m:</span></span>
    
        @property (readonly) NSOperationQueue* downloadQueue;
        - (NSString*)base64forData:(NSData*)theData;
12. <span data-ttu-id="00259-137">Adja hozzá a következő végrehajtására:</span><span class="sxs-lookup"><span data-stu-id="00259-137">Then, add the following implementation:</span></span>
    
            - (NSOperationQueue *)downloadQueue {
                if (!_downloadQueue) {
                    _downloadQueue = [[NSOperationQueue alloc] init];
                    _downloadQueue.name = @"Download Queue";
                    _downloadQueue.maxConcurrentOperationCount = 1;
                }
                return _downloadQueue;
            }
    
            // base64 encoding
            - (NSString*)base64forData:(NSData*)theData
            {
                const uint8_t* input = (const uint8_t*)[theData bytes];
                NSInteger length = [theData length];
    
                static char table[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";
    
                NSMutableData* data = [NSMutableData dataWithLength:((length + 2) / 3) * 4];
                uint8_t* output = (uint8_t*)data.mutableBytes;
    
                NSInteger i;
                for (i=0; i < length; i += 3) {
                    NSInteger value = 0;
                    NSInteger j;
                    for (j = i; j < (i + 3); j++) {
                        value <<= 8;
    
                        if (j < length) {
                            value |= (0xFF & input[j]);
                        }
                    }
    
                    NSInteger theIndex = (i / 3) * 4;
                    output[theIndex + 0] =                    table[(value >> 18) & 0x3F];
                    output[theIndex + 1] =                    table[(value >> 12) & 0x3F];
                    output[theIndex + 2] = (i + 1) < length ? table[(value >> 6)  & 0x3F] : '=';
                    output[theIndex + 3] = (i + 2) < length ? table[(value >> 0)  & 0x3F] : '=';
                }
    
                return [[NSString alloc] initWithData:data encoding:NSASCIIStringEncoding];
            }
13. <span data-ttu-id="00259-138">Másolja az alábbi kódot a **bejelentkezési** eseménykezelő metódus hozta létre xcode-ban:</span><span class="sxs-lookup"><span data-stu-id="00259-138">Copy the following code into the **login** handler method created by XCode:</span></span>
    
            DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
    
            // build JSON
            NSString* json = [NSString stringWithFormat:@"{\"platform\":\"ios\", \"instId\":\"%@\", \"deviceToken\":\"%@\"}", deviceInfo.installationId, [deviceInfo getDeviceTokenInHex]];
    
            // build auth string
            NSString* authString = [NSString stringWithFormat:@"%@:%@", self.User.text, self.Password.text];
    
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"http://nhnotifyuser.azurewebsites.net/api/register"]];
            [request setHTTPMethod:@"POST"];
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
            [request addValue:[@([json lengthOfBytesUsingEncoding:NSUTF8StringEncoding]) description] forHTTPHeaderField:@"Content-Length"];
            [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
            [request addValue:[NSString stringWithFormat:@"Basic %@",[self base64forData:[authString dataUsingEncoding:NSUTF8StringEncoding]]] forHTTPHeaderField:@"Authorization"];
    
            // connect with POST
            [NSURLConnection sendAsynchronousRequest:request queue:[self downloadQueue] completionHandler:^(NSURLResponse* response, NSData* data, NSError* error) {
                // add UIAlert depending on response.
                if (error != nil) {
                    NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                    if ([httpResponse statusCode] == 200) {
                        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Back-end registration" message:@"Registration successful" delegate:nil cancelButtonTitle: @"OK" otherButtonTitles:nil, nil];
                        [alert show];
                    } else {
                        NSLog(@"status: %ld", (long)[httpResponse statusCode]);
                    }
                } else {
                    NSLog(@"error: %@", error);
                }
            }];
    
    <span data-ttu-id="00259-139">Ez a módszer egy telepítési Azonosítót és a csatorna lekérdezi a leküldéses értesítések, és elküldi, az eszköz típusa, valamint a hitelesített webes API-módszer, amely a Notification Hubs egy regisztrációs hoz.</span><span class="sxs-lookup"><span data-stu-id="00259-139">This method gets both an installation ID and channel for push notifications and sends it, along with the device type, to the authenticated Web API method that creates a registration in Notification Hubs.</span></span> <span data-ttu-id="00259-140">A Web API lett definiálva [értesítse a felhasználókat a Notification hubs használatával].</span><span class="sxs-lookup"><span data-stu-id="00259-140">This Web API was defined in [Notify users with Notification Hubs].</span></span>

<span data-ttu-id="00259-141">Most, hogy az ügyfélalkalmazás frissítették, térjen vissza a [értesítse a felhasználókat a Notification hubs használatával] , és frissítse a mobilszolgáltatáshoz az értesítések küldése a Notification Hubs használatával.</span><span class="sxs-lookup"><span data-stu-id="00259-141">Now that the client app has been updated, return to the [Notify users with Notification Hubs] and update the mobile service to send notifications by using Notification Hubs.</span></span>

<!-- Anchors. -->

<!-- Images. -->
[0]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios1.png
[1]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios2.png

<!-- URLs. -->
<span data-ttu-id="00259-142">[értesítse a felhasználókat a Notification hubs használatával]: /manage/services/notification-hubs/notify-users-aspnet</span><span class="sxs-lookup"><span data-stu-id="00259-142">[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users-aspnet</span></span>

<span data-ttu-id="00259-143">[Ismerkedés a Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-ios</span><span class="sxs-lookup"><span data-stu-id="00259-143">[Get Started with Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-ios</span></span>
