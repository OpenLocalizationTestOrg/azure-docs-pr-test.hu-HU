---
title: "aaaRegister hello aktuális felhasználó a leküldéses értesítések webes API használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan toorequest leküldéses értesítés regisztrálása az Azure Notification Hubs iOS-alkalmazást a regisztrálás ASP.NET Web API végrehajtásakor."
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
ms.openlocfilehash: f859feb436093e703d7e1db38354dd356fff8efe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="register-hello-current-user-for-push-notifications-by-using-aspnet"></a><span data-ttu-id="96d5a-103">ASP.NET segítségével hello aktuális felhasználó a leküldéses értesítések regisztrálása</span><span class="sxs-lookup"><span data-stu-id="96d5a-103">Register hello current user for push notifications by using ASP.NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="96d5a-104">iOS</span><span class="sxs-lookup"><span data-stu-id="96d5a-104">iOS</span></span>](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="96d5a-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="96d5a-105">Overview</span></span>
<span data-ttu-id="96d5a-106">Ez a témakör bemutatja, toorequest a leküldéses értesítés regisztrálása az Azure Notification Hubs regisztrációs ASP.NET Web API végrehajtásakor.</span><span class="sxs-lookup"><span data-stu-id="96d5a-106">This topic shows you how toorequest push notification registration with Azure Notification Hubs when registration is performed by ASP.NET Web API.</span></span> <span data-ttu-id="96d5a-107">Ez a témakör bővíti hello oktatóanyag [értesítse a felhasználókat a Notification hubs használatával].</span><span class="sxs-lookup"><span data-stu-id="96d5a-107">This topic extends hello tutorial [Notify users with Notification Hubs].</span></span> <span data-ttu-id="96d5a-108">Már végrehajtotta a oktatóanyag toocreate hitelesített hello mobilszolgáltatásban hello szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="96d5a-108">You must have already completed hello required steps in that tutorial toocreate hello authenticated mobile service.</span></span> <span data-ttu-id="96d5a-109">További információt a hello felhasználók forgatókönyv értesíti, lásd: [értesítse a felhasználókat a Notification hubs használatával].</span><span class="sxs-lookup"><span data-stu-id="96d5a-109">For more information on hello notify users scenario, see [Notify users with Notification Hubs].</span></span>

## <a name="update-your-app"></a><span data-ttu-id="96d5a-110">Az alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="96d5a-110">Update your app</span></span>
1. <span data-ttu-id="96d5a-111">Adja hozzá a MainStoryboard_iPhone.storyboard hello összetevők hello objektum könyvtárból a következő:</span><span class="sxs-lookup"><span data-stu-id="96d5a-111">In your MainStoryboard_iPhone.storyboard, add hello following components from hello object library:</span></span>
   
   * <span data-ttu-id="96d5a-112">**Címke**: "Push Notification Hubs szolgáltatással tooUser"</span><span class="sxs-lookup"><span data-stu-id="96d5a-112">**Label**: "Push tooUser with Notification Hubs"</span></span>
   * <span data-ttu-id="96d5a-113">**Címke**: "Végrehajtott"</span><span class="sxs-lookup"><span data-stu-id="96d5a-113">**Label**: "InstallationId"</span></span>
   * <span data-ttu-id="96d5a-114">**Címke**: "User"</span><span class="sxs-lookup"><span data-stu-id="96d5a-114">**Label**: "User"</span></span>
   * <span data-ttu-id="96d5a-115">**Szövegmező**: "User"</span><span class="sxs-lookup"><span data-stu-id="96d5a-115">**Text Field**: "User"</span></span>
   * <span data-ttu-id="96d5a-116">**Címke**: "Password"</span><span class="sxs-lookup"><span data-stu-id="96d5a-116">**Label**: "Password"</span></span>
   * <span data-ttu-id="96d5a-117">**Szövegmező**: "Password"</span><span class="sxs-lookup"><span data-stu-id="96d5a-117">**Text Field**: "Password"</span></span>
   * <span data-ttu-id="96d5a-118">**Gomb**: "Bejelentkezés"</span><span class="sxs-lookup"><span data-stu-id="96d5a-118">**Button**: "Login"</span></span>
     
     <span data-ttu-id="96d5a-119">Ezen a ponton a storyboard következőhöz hasonló hello:</span><span class="sxs-lookup"><span data-stu-id="96d5a-119">At this point, your storyboard looks like hello following:</span></span>
     
      ![][0]
2. <span data-ttu-id="96d5a-120">Hello Segéd-szerkesztőben hozzon létre az összes kapcsolt hello vezérlők kimeneteket, keresheti őket, hello szövegmezők kapcsolattartásnak hello nézetvezérlő (delegált), és hozzon létre egy **művelet** a hello **bejelentkezési** gombra.</span><span class="sxs-lookup"><span data-stu-id="96d5a-120">In hello assistant editor, create outlets for all hello switched controls and call them, connect hello text fields with hello View Controller (delegate), and create an **Action** for hello **login** button.</span></span>
   
       ![][1]
   
       Your BreakingNewsViewController.h file should now contain hello following code:
   
        @property (weak, nonatomic) IBOutlet UILabel *installationId;
        @property (weak, nonatomic) IBOutlet UITextField *User;
        @property (weak, nonatomic) IBOutlet UITextField *Password;
   
        - (IBAction)login:(id)sender;
3. <span data-ttu-id="96d5a-121">Hozzon létre egy osztályt **deviceinfo információja**, és a következő kód másolása hello hello fájl DeviceInfo.h hello felület szakaszba:</span><span class="sxs-lookup"><span data-stu-id="96d5a-121">Create a class named **DeviceInfo**, and copy hello following code into hello interface section of hello file DeviceInfo.h:</span></span>
   
        @property (readonly, nonatomic) NSString* installationId;
        @property (nonatomic) NSData* deviceToken;
4. <span data-ttu-id="96d5a-122">Másolja a következő kód hello implementation szakaszban hello DeviceInfo.m fájl hello:</span><span class="sxs-lookup"><span data-stu-id="96d5a-122">Copy hello following code in hello implementation section of hello DeviceInfo.m file:</span></span>
   
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
   
                    //store hello install ID so we don't generate a new one next time
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
5. <span data-ttu-id="96d5a-123">PushToUserAppDelegate.h adja hozzá a következő tulajdonság egypéldányos hello:</span><span class="sxs-lookup"><span data-stu-id="96d5a-123">In PushToUserAppDelegate.h, add hello following property singleton:</span></span>
   
        @property (strong, nonatomic) DeviceInfo* deviceInfo;
6. <span data-ttu-id="96d5a-124">A hello **didFinishLaunchingWithOptions** metódus a PushToUserAppDelegate.m, adja hozzá a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="96d5a-124">In hello **didFinishLaunchingWithOptions** method in PushToUserAppDelegate.m, add hello following code:</span></span>
   
        self.deviceInfo = [[DeviceInfo alloc] init];
   
        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
   
    <span data-ttu-id="96d5a-125">első sor hello inicializálja hello **deviceinfo információja** egypéldányos.</span><span class="sxs-lookup"><span data-stu-id="96d5a-125">hello first line initializes hello **DeviceInfo** singleton.</span></span> <span data-ttu-id="96d5a-126">hello második sor indítása hello regisztráció a leküldéses értesítések, amelyek már jelen van az hello már befejeződött [Ismerkedés a Notification Hubs] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="96d5a-126">hello second line starts hello registration for push notifications, which is already present is you have already completed hello [Get Started with Notification Hubs] tutorial.</span></span>
7. <span data-ttu-id="96d5a-127">A PushToUserAppDelegate.m, valósítja meg hello **didRegisterForRemoteNotificationsWithDeviceToken** a AppDelegate a, és adja hozzá a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="96d5a-127">In PushToUserAppDelegate.m, implement hello method **didRegisterForRemoteNotificationsWithDeviceToken** in your AppDelegate and add hello following code:</span></span>
   
        self.deviceInfo.deviceToken = deviceToken;
   
    <span data-ttu-id="96d5a-128">Ez a beállítás hello eszköz jogkivonatát hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="96d5a-128">This sets hello device token for hello request.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="96d5a-129">Ezen a ponton nem lehetnek többi kód az ezzel a módszerrel.</span><span class="sxs-lookup"><span data-stu-id="96d5a-129">At this point, there should not be any other code in this method.</span></span> <span data-ttu-id="96d5a-130">Ha már rendelkezik egy hívás toohello **registerNativeWithDeviceToken** hello befejezésekor hozzáadott metódus [Ismerkedés a Notification Hubs](/manage/services/notification-hubs/get-started-notification-hubs-ios/) kell Megjegyzés kimenő, vagy távolítsa el, amely útmutató hívja meg.</span><span class="sxs-lookup"><span data-stu-id="96d5a-130">If you already have a call toohello **registerNativeWithDeviceToken** method that was added when you completed hello [Get Started with Notification Hubs](/manage/services/notification-hubs/get-started-notification-hubs-ios/) tutorial, you must comment-out or remove that call.</span></span>
   > 
   > 
8. <span data-ttu-id="96d5a-131">Hello PushToUserAppDelegate.m fájlban adja hozzá a következő leíró metódus hello:</span><span class="sxs-lookup"><span data-stu-id="96d5a-131">In hello PushToUserAppDelegate.m file, add hello following handler method:</span></span>
   
   * <span data-ttu-id="96d5a-132">(a "void") alkalmazás:(UIApplication *) alkalmazás didReceiveRemoteNotification:(NSDictionary *) userInfo {NSLog (@"% @", userInfo);   UIAlertView * riasztás = [[UIAlertView foglalási] initWithTitle:@"Notification" üzenetet: [userInfo objectForKey:@"inAppMessage"] delegált: üres cancelButtonTitle: @"OK" otherButtonTitles:nil, üres];   [riasztás megjelenítése]; }</span><span class="sxs-lookup"><span data-stu-id="96d5a-132">(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {   NSLog(@"%@", userInfo);   UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:                         [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:                         @"OK" otherButtonTitles:nil, nil];   [alert show]; }</span></span>
   
   <span data-ttu-id="96d5a-133">Ezt a módszert a felhasználói felület hello megjelenít egy riasztást, amikor az alkalmazás futtatása értesítéseket fogad.</span><span class="sxs-lookup"><span data-stu-id="96d5a-133">This method displays an alert in hello UI when your app receives notifications while it is running.</span></span>
9. <span data-ttu-id="96d5a-134">Nyissa meg a végrehajtása a következő hello hello PushToUserViewController.m fájlt, és visszatérési hello billentyűzet:</span><span class="sxs-lookup"><span data-stu-id="96d5a-134">Open hello PushToUserViewController.m file, and return hello keyboard in hello following implementation:</span></span>
   
        - (BOOL)textFieldShouldReturn:(UITextField *)theTextField {
            if (theTextField == self.User || theTextField == self.Password) {
                [theTextField resignFirstResponder];
            }
            return YES;
        }
10. <span data-ttu-id="96d5a-135">A hello **viewDidLoad** hello PushToUserViewController.m fájlban metódus inicializálása hello végrehajtott címke az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="96d5a-135">In hello **viewDidLoad** method in hello PushToUserViewController.m file, initialize hello installationId label as follows:</span></span>
    
         DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
         Self.installationId.text = deviceInfo.installationId;
11. <span data-ttu-id="96d5a-136">Adja hozzá a következő felületen a PushToUserViewController.m tulajdonságai hello:</span><span class="sxs-lookup"><span data-stu-id="96d5a-136">Add hello following properties in interface in PushToUserViewController.m:</span></span>
    
        @property (readonly) NSOperationQueue* downloadQueue;
        - (NSString*)base64forData:(NSData*)theData;
12. <span data-ttu-id="96d5a-137">Adja hozzá a következő megvalósítási hello:</span><span class="sxs-lookup"><span data-stu-id="96d5a-137">Then, add hello following implementation:</span></span>
    
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
13. <span data-ttu-id="96d5a-138">Másolás hello következő hello kódot **bejelentkezési** eseménykezelő metódus hozta létre xcode-ban:</span><span class="sxs-lookup"><span data-stu-id="96d5a-138">Copy hello following code into hello **login** handler method created by XCode:</span></span>
    
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
    
    <span data-ttu-id="96d5a-139">Ez a módszer egy telepítési Azonosítót és a csatorna lekérdezi a leküldéses értesítések, és elküldi azt, és hello eszköztípus, toohello hitelesítése egy regisztrációs létrehozó a Notification Hubs módszer a webes API.</span><span class="sxs-lookup"><span data-stu-id="96d5a-139">This method gets both an installation ID and channel for push notifications and sends it, along with hello device type, toohello authenticated Web API method that creates a registration in Notification Hubs.</span></span> <span data-ttu-id="96d5a-140">A Web API lett definiálva [értesítse a felhasználókat a Notification hubs használatával].</span><span class="sxs-lookup"><span data-stu-id="96d5a-140">This Web API was defined in [Notify users with Notification Hubs].</span></span>

<span data-ttu-id="96d5a-141">Most, hogy hello ügyfélalkalmazás frissítették, térjen vissza a toohello [értesítse a felhasználókat a Notification hubs használatával] és frissítési hello mobilszolgáltatás toosend értesítések a Notification Hubs használatával.</span><span class="sxs-lookup"><span data-stu-id="96d5a-141">Now that hello client app has been updated, return toohello [Notify users with Notification Hubs] and update hello mobile service toosend notifications by using Notification Hubs.</span></span>

<!-- Anchors. -->

<!-- Images. -->
[0]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios1.png
[1]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios2.png

<!-- URLs. -->
[értesítse a felhasználókat a Notification hubs használatával]: /manage/services/notification-hubs/notify-users-aspnet

[Ismerkedés a Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-ios
