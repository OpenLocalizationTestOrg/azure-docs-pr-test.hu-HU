---
title: "Azure Notification Hubs – felhasználók értesítése iOS rendszerhez .NET-háttérrendszerrel"
description: "Megtudhatja, hogyan küldhetők leküldéses értesítések az Azure-ban a felhasználók számára. Kódminták Objective-C és a háttérkiszolgáló .NET API-t."
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: 1f7d1410-ef93-4c4b-813b-f075eed20082
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 0fa7a886e1ecb0a90b6aebc1dbf9ef0c6ce1acf1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-notification-hubs-notify-users-for-ios-with-net-backend"></a><span data-ttu-id="744d8-104">Azure Notification Hubs – felhasználók értesítése iOS rendszerhez .NET-háttérrendszerrel</span><span class="sxs-lookup"><span data-stu-id="744d8-104">Azure Notification Hubs Notify Users for iOS with .NET backend</span></span>
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a><span data-ttu-id="744d8-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="744d8-105">Overview</span></span>
<span data-ttu-id="744d8-106">Leküldéses értesítési támogatás az Azure-ban lehetővé teszi egy könnyen kezelhető, multiplatform és kibővített leküldéses infrastruktúrában, ami jelentősen egyszerűsíti a leküldéses értesítések mobil platformokhoz fogyasztói, valamint a vállalati alkalmazások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="744d8-106">Push notification support in Azure enables you to access an easy-to-use, multiplatform, and scaled-out push infrastructure, which greatly simplifies the implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span> <span data-ttu-id="744d8-107">Az oktatóanyag bemutatja, hogyan küldhetők az Azure Notification Hubs használatával leküldéses értesítések adott alkalmazásfelhasználónak, meghatározott eszközre.</span><span class="sxs-lookup"><span data-stu-id="744d8-107">This tutorial shows you how to use Azure Notification Hubs to send push notifications to a specific app user on a specific device.</span></span> <span data-ttu-id="744d8-108">ASP.NET WebAPI háttérrendszerből használt hitelesíti az ügyfeleket, és értesítéseket, ahogy az az útmutató témakör [az alkalmazás háttérrendszeréből regisztrálása](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).</span><span class="sxs-lookup"><span data-stu-id="744d8-108">An ASP.NET WebAPI backend is used to authenticate clients and to generate notifications, as shown in the guidance topic [Registering from your app backend](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).</span></span>

> [!NOTE]
> <span data-ttu-id="744d8-109">Ez az oktatóanyag feltételezi, hogy létrehozta és leírtak szerint konfigurálta az értesítési központ [Ismerkedés a Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="744d8-109">This tutorial assumes that you have created and configured your notification hub as described in [Getting Started with Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).</span></span> <span data-ttu-id="744d8-110">Ez az oktatóanyag egyben a előfeltételeit a [(iOS) biztonságos leküldéses](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="744d8-110">This tutorial is also the prerequisite to the [Secure Push (iOS)](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md) tutorial.</span></span>
> <span data-ttu-id="744d8-111">Ha szeretné használni a háttérszolgáltatás Mobile Apps, tekintse meg a [Mobile Apps első lépések leküldéses](../app-service-mobile/app-service-mobile-ios-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="744d8-111">If you want to use Mobile Apps as your backend service, see the [Mobile Apps Get Started with Push](../app-service-mobile/app-service-mobile-ios-get-started-push.md).</span></span>
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="modify-your-ios-app"></a><span data-ttu-id="744d8-112">Az iOS-alkalmazás módosítása</span><span class="sxs-lookup"><span data-stu-id="744d8-112">Modify your iOS app</span></span>
1. <span data-ttu-id="744d8-113">Nyissa meg a létrehozott alkalmazás egylapos megtekintése a [Ismerkedés a Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="744d8-113">Open the Single Page view app you created in the [Getting Started with Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) tutorial.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="744d8-114">Ebben a szakaszban feltételezzük, hogy a projekt egy üres szervezetnév van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="744d8-114">In this section we assume that your project is configured with an empty organization name.</span></span> <span data-ttu-id="744d8-115">Ha nem, akkor be kell illesztenie a szervezet neve, az összes osztály nevét.</span><span class="sxs-lookup"><span data-stu-id="744d8-115">If not, you will need to prepend your organization name to all class names.</span></span>
   > 
   > 
2. <span data-ttu-id="744d8-116">A Main.storyboard adja hozzá az összetevőket az objektumtárból alatt a képernyőfelvételen látható módon.</span><span class="sxs-lookup"><span data-stu-id="744d8-116">In your Main.storyboard add the components shown in the screenshot below from the object library.</span></span>
   
    ![][1]
   
   * <span data-ttu-id="744d8-117">**Felhasználónév**: a helyőrző szöveg A UITextField *adjon meg felhasználónevet*, azonnal eredmények címkével, és a bal és jobb margó korlátozott a küldés alatt, és a küldési eredmények címke alatt.</span><span class="sxs-lookup"><span data-stu-id="744d8-117">**Username**: A UITextField with placeholder text, *Enter Username*, immediately beneath the send results label and constrained to the left and right margins and beneath the send results label.</span></span>
   * <span data-ttu-id="744d8-118">**Jelszó**: a helyőrző szöveg A UITextField *jelszó megadása*, azonnal a felhasználónév alatt szöveges mezőt, és korlátozott, a bal és jobb margó és a felhasználónév szövegmezőt alatt.</span><span class="sxs-lookup"><span data-stu-id="744d8-118">**Password**: A UITextField with placeholder text, *Enter Password*, immediately beneath the username text field and constrained to the left and right margins and beneath the username text field.</span></span> <span data-ttu-id="744d8-119">Ellenőrizze a **szövegbeviteli biztonságos** attribútum megtekintő lehetőséget a *vissza kulcs*.</span><span class="sxs-lookup"><span data-stu-id="744d8-119">Check the **Secure Text Entry** option in the Attribute Inspector, under *Return Key*.</span></span>
   * <span data-ttu-id="744d8-120">**Jelentkezzen be**: A UIButton feliratú azonnal a jelszó szövegmező alatt, és törölje a jelet a **engedélyezve** attribútumok megtekintő lehetőséget a *vezérlő – tartalom*</span><span class="sxs-lookup"><span data-stu-id="744d8-120">**Log in**: A UIButton labeled immediately beneath the password text field and uncheck the **Enabled** option in the Attributes Inspector, under *Control-Content*</span></span>
   * <span data-ttu-id="744d8-121">**WNS**: címke és kapcsoló ahhoz, hogy a Windows értesítési szolgáltatás értesítést küld, ha a központi telepítés le van.</span><span class="sxs-lookup"><span data-stu-id="744d8-121">**WNS**: Label and switch to enable sending the notification Windows Notification Service if it has been setup on the hub.</span></span> <span data-ttu-id="744d8-122">Tekintse meg a [Windows bevezetés](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="744d8-122">See the [Windows Getting Started](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) tutorial.</span></span>
   * <span data-ttu-id="744d8-123">**GCM**: címke és a kapcsoló az értesítés küldését a Google Cloud Messaging, ha a központi telepítés lett.</span><span class="sxs-lookup"><span data-stu-id="744d8-123">**GCM**: Label and switch to enable sending the notification to Google Cloud Messaging if it has been setup on the hub.</span></span> <span data-ttu-id="744d8-124">Lásd: [Android bevezetés](notification-hubs-android-push-notification-google-gcm-get-started.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="744d8-124">See [Android Getting Started](notification-hubs-android-push-notification-google-gcm-get-started.md) tutorial.</span></span>
   * <span data-ttu-id="744d8-125">**APNS**: címke és kapcsoló engedélyezése az Apple Platform Notification szolgáltatással küldött.</span><span class="sxs-lookup"><span data-stu-id="744d8-125">**APNS**: Label and switch to enable sending the notification to the Apple Platform Notification Service.</span></span>
   * <span data-ttu-id="744d8-126">**Recipent felhasználónév**: a helyőrző szöveg A UITextField *címzett felhasználónév címke*, azonnal a GCM alatt címkével, és a bal és jobb margó és a GCM-címke alatt korlátozott.</span><span class="sxs-lookup"><span data-stu-id="744d8-126">**Recipent Username**:A UITextField with placeholder text, *Recipient username tag*, immediately beneath the GCM label and constrained to the left and right margins and beneath the GCM label.</span></span>

    <span data-ttu-id="744d8-127">Néhány összetevőt hozzáadott a [Ismerkedés a Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="744d8-127">Some components were added in the [Getting Started with Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) tutorial.</span></span>

1. <span data-ttu-id="744d8-128">**CTRL** ViewController.h húzza a összetevői a nézetben, és adja hozzá ezek új értékesítési lehetőségek.</span><span class="sxs-lookup"><span data-stu-id="744d8-128">**Ctrl** drag from the components in the view to ViewController.h and add these new outlets.</span></span>
   
        @property (weak, nonatomic) IBOutlet UITextField *UsernameField;
        @property (weak, nonatomic) IBOutlet UITextField *PasswordField;
        @property (weak, nonatomic) IBOutlet UITextField *RecipientField;
        @property (weak, nonatomic) IBOutlet UITextField *NotificationField;
   
        // Used to enable the buttons on the UI
        @property (weak, nonatomic) IBOutlet UIButton *LogInButton;
        @property (weak, nonatomic) IBOutlet UIButton *SendNotificationButton;
   
        // Used to enabled sending notifications across platforms
        @property (weak, nonatomic) IBOutlet UISwitch *WNSSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *GCMSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *APNSSwitch;
   
        - (IBAction)LogInAction:(id)sender;
2. <span data-ttu-id="744d8-129">A ViewController.h, adja hozzá a következő `#define` alatt az importálási utasítást.</span><span class="sxs-lookup"><span data-stu-id="744d8-129">In ViewController.h, add the following `#define` just below your import statements.</span></span> <span data-ttu-id="744d8-130">Helyettesítő a *< írja be a háttérkiszolgáló végpont\>*  helyőrző a célként megadott URL-címet, a háttéralkalmazás az előző szakaszban központi telepítéséhez használt.</span><span class="sxs-lookup"><span data-stu-id="744d8-130">Substitute the *<Enter Your Backend Endpoint\>* placeholder with the Destination URL you used to deploy your app backend in the previous section.</span></span> <span data-ttu-id="744d8-131">Például *http://you_backend.azurewebsites.net*.</span><span class="sxs-lookup"><span data-stu-id="744d8-131">For example, *http://you_backend.azurewebsites.net*.</span></span>
   
        #define BACKEND_ENDPOINT @"<Enter Your Backend Endpoint>"
3. <span data-ttu-id="744d8-132">A projekt, hozzon létre egy új **Cocoa Touch osztály** nevű **RegisterClient** való az ASP.NET-háttér létrehozott.</span><span class="sxs-lookup"><span data-stu-id="744d8-132">In your project, create a new **Cocoa Touch class** named **RegisterClient** to interface with the ASP.NET back-end you created.</span></span> <span data-ttu-id="744d8-133">Hozzon létre a osztályt való öröklődés `NSObject`.</span><span class="sxs-lookup"><span data-stu-id="744d8-133">Create the class inheriting from `NSObject`.</span></span> <span data-ttu-id="744d8-134">Majd adja hozzá a következő kódot a RegisterClient.h a.</span><span class="sxs-lookup"><span data-stu-id="744d8-134">Then add the following code in the RegisterClient.h.</span></span>
   
        @interface RegisterClient : NSObject
   
        @property (strong, nonatomic) NSString* authenticationHeader;
   
        -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
            andCompletion:(void(^)(NSError*))completion;
   
        -(instancetype) initWithEndpoint:(NSString*)Endpoint;
   
        @end
4. <span data-ttu-id="744d8-135">A RegisterClient.m frissítés a `@interface` szakasz:</span><span class="sxs-lookup"><span data-stu-id="744d8-135">In the RegisterClient.m update the `@interface` section:</span></span>
   
        @interface RegisterClient ()
   
        @property (strong, nonatomic) NSURLSession* session;
        @property (strong, nonatomic) NSURLSession* endpoint;
   
        -(void) tryToRegisterWithDeviceToken:(NSData*)token tags:(NSSet*)tags retry:(BOOL)retry
                    andCompletion:(void(^)(NSError*))completion;
        -(void) retrieveOrRequestRegistrationIdWithDeviceToken:(NSString*)token
                    completion:(void(^)(NSString*, NSError*))completion;
        -(void) upsertRegistrationWithRegistrationId:(NSString*)registrationId deviceToken:(NSString*)token
                    tags:(NSSet*)tags andCompletion:(void(^)(NSURLResponse*, NSError*))completion;
   
        @end
5. <span data-ttu-id="744d8-136">Cserélje le a `@implementation` című szakasza a következő kóddal RegisterClient.m.</span><span class="sxs-lookup"><span data-stu-id="744d8-136">Replace the `@implementation` section in the RegisterClient.m with the following code.</span></span>

        @implementation RegisterClient

        // Globals used by RegisterClient
        NSString *const RegistrationIdLocalStorageKey = @"RegistrationId";

        -(instancetype) initWithEndpoint:(NSString*)Endpoint
        {
            self = [super init];
            if (self) {
                NSURLSessionConfiguration* config = [NSURLSessionConfiguration defaultSessionConfiguration];
                _session = [NSURLSession sessionWithConfiguration:config delegate:nil delegateQueue:nil];
                _endpoint = Endpoint;
            }
            return self;
        }

        -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
                    andCompletion:(void(^)(NSError*))completion
        {
            [self tryToRegisterWithDeviceToken:token tags:tags retry:YES andCompletion:completion];
        }

        -(void) tryToRegisterWithDeviceToken:(NSData*)token tags:(NSSet*)tags retry:(BOOL)retry
                    andCompletion:(void(^)(NSError*))completion
        {
            NSSet* tagsSet = tags?tags:[[NSSet alloc] init];

            NSString *deviceTokenString = [[token description]
                stringByTrimmingCharactersInSet:[NSCharacterSet characterSetWithCharactersInString:@"<>"]];
            deviceTokenString = [[deviceTokenString stringByReplacingOccurrencesOfString:@" " withString:@""]
                                    uppercaseString];

            [self retrieveOrRequestRegistrationIdWithDeviceToken: deviceTokenString
                completion:^(NSString* registrationId, NSError *error) {
                NSLog(@"regId: %@", registrationId);
                if (error) {
                    completion(error);
                    return;
                }

                [self upsertRegistrationWithRegistrationId:registrationId deviceToken:deviceTokenString
                    tags:tagsSet andCompletion:^(NSURLResponse * response, NSError *error) {
                    if (error) {
                        completion(error);
                        return;
                    }

                    NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                    if (httpResponse.statusCode == 200) {
                        completion(nil);
                    } else if (httpResponse.statusCode == 410 && retry) {
                        [self tryToRegisterWithDeviceToken:token tags:tags retry:NO andCompletion:completion];
                    } else {
                        NSLog(@"Registration error with response status: %ld", (long)httpResponse.statusCode);

                        completion([NSError errorWithDomain:@"Registration" code:httpResponse.statusCode
                                    userInfo:nil]);
                    }

                }];
            }];
        }

        -(void) upsertRegistrationWithRegistrationId:(NSString*)registrationId deviceToken:(NSData*)token
                    tags:(NSSet*)tags andCompletion:(void(^)(NSURLResponse*, NSError*))completion
        {
            NSDictionary* deviceRegistration = @{@"Platform" : @"apns", @"Handle": token,
                                                    @"Tags": [tags allObjects]};
            NSData* jsonData = [NSJSONSerialization dataWithJSONObject:deviceRegistration
                                options:NSJSONWritingPrettyPrinted error:nil];

            NSLog(@"JSON registration: %@", [[NSString alloc] initWithData:jsonData
                                                encoding:NSUTF8StringEncoding]);

            NSString* endpoint = [NSString stringWithFormat:@"%@/api/register/%@", _endpoint,
                                    registrationId];
            NSURL* requestURL = [NSURL URLWithString:endpoint];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"PUT"];
            [request setHTTPBody:jsonData];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",
                                                    self.authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];
            [request setValue:@"application/json" forHTTPHeaderField:@"Content-Type"];

            NSURLSessionDataTask* dataTask = [self.session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                if (!error)
                {
                    completion(response, error);
                }
                else
                {
                    NSLog(@"Error request: %@", error);
                    completion(nil, error);
                }
            }];
            [dataTask resume];
        }

        -(void) retrieveOrRequestRegistrationIdWithDeviceToken:(NSString*)token
                    completion:(void(^)(NSString*, NSError*))completion
        {
            NSString* registrationId = [[NSUserDefaults standardUserDefaults]
                                        objectForKey:RegistrationIdLocalStorageKey];

            if (registrationId)
            {
                completion(registrationId, nil);
                return;
            }

            // request new one & save
            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/api/register?handle=%@",
                                    _endpoint, token]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"POST"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",
                                                    self.authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [self.session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200)
                {
                    NSString* registrationId = [[NSString alloc] initWithData:data
                        encoding:NSUTF8StringEncoding];

                    // remove quotes
                    registrationId = [registrationId substringWithRange:NSMakeRange(1,
                                        [registrationId length]-2)];

                    [[NSUserDefaults standardUserDefaults] setObject:registrationId
                        forKey:RegistrationIdLocalStorageKey];
                    [[NSUserDefaults standardUserDefaults] synchronize];

                    completion(registrationId, nil);
                }
                else
                {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(nil, error);
                    else {
                        completion(nil, [NSError errorWithDomain:@"Registration" code:httpResponse.statusCode
                                    userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }

        @end

    <span data-ttu-id="744d8-137">A fenti kódot valósítja meg az útmutató a cikkben ismertetett logika [az alkalmazás háttérrendszeréből regisztrálása](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend) a háttéralkalmazás NSURLSession REST elvégzéséhez használt hívások, és helyileg tárolja a registrationId NSUserDefaults adott vissza által az értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="744d8-137">The code above implements the logic explained in the guidance article [Registering from your app backend](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend) using NSURLSession to perform REST calls to your app backend, and NSUserDefaults to locally store the registrationId returned by the notification hub.</span></span>

    <span data-ttu-id="744d8-138">Vegye figyelembe, hogy ez az osztály van szükség a tulajdonsága **authorizationHeader** kell beállítani a megfelelő működéshez.</span><span class="sxs-lookup"><span data-stu-id="744d8-138">Note that this class requires its property **authorizationHeader** to be set in order to work properly.</span></span> <span data-ttu-id="744d8-139">A tulajdonság értéke a **ViewController** osztály a bejelentkezés után.</span><span class="sxs-lookup"><span data-stu-id="744d8-139">This property is set by the **ViewController** class after the log in.</span></span>

1. <span data-ttu-id="744d8-140">A ViewController.h, adjon hozzá egy `#import` RegisterClient.h utasítást.</span><span class="sxs-lookup"><span data-stu-id="744d8-140">In ViewController.h, add a `#import` statement for RegisterClient.h.</span></span> <span data-ttu-id="744d8-141">Ezután adjon hozzá egy; az az eszköz jogkivonatát, és hivatkozás egy `RegisterClient` a példányt a `@interface` szakasz:</span><span class="sxs-lookup"><span data-stu-id="744d8-141">Then add a declaration for the device token and reference to a `RegisterClient` instance in the `@interface` section:</span></span>
   
        #import "RegisterClient.h"
   
        @property (strong, nonatomic) NSData* deviceToken;
        @property (strong, nonatomic) RegisterClient* registerClient;
2. <span data-ttu-id="744d8-142">ViewController.m, adja hozzá a belső metódust nyilatkozatot a `@interface` szakasz:</span><span class="sxs-lookup"><span data-stu-id="744d8-142">In ViewController.m, add a private method declaration in the `@interface` section:</span></span>
   
        @interface ViewController () <UITextFieldDelegate, NSURLConnectionDataDelegate, NSXMLParserDelegate>
   
        // create the Authorization header to perform Basic authentication with your app back-end
        -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                        AndPassword:(NSString*)password;
   
        @end

> [!NOTE]
> <span data-ttu-id="744d8-143">Az alábbi kódrészletben nincs a biztonságos hitelesítési séma, helyettesítse be a végrehajtása a **createAndSetAuthenticationHeaderWithUsername:AndPassword:** , az adott hitelesítési mechanizmus, amely létrehoz egy hitelesítési jogkivonatot a register ügyfél osztály, például az OAuth, az Active Directory által.</span><span class="sxs-lookup"><span data-stu-id="744d8-143">The following snippet is not a secure authentication scheme, you should substitute the implementation of the **createAndSetAuthenticationHeaderWithUsername:AndPassword:** with your specific authentication mechanism that generates an authentication token to be consumed by the register client class, e.g. OAuth, Active Directory.</span></span>
> 
> 

1. <span data-ttu-id="744d8-144">Ezt a a `@implementation` ViewController.m szakasza adja hozzá az alábbi kód, amely hozzáadja a beállítás a eszköz token és hitelesítési fejléc végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="744d8-144">Then in the `@implementation` section of ViewController.m add the following code which adds the implementation for setting the device token and authentication header.</span></span>
   
        -(void) setDeviceToken: (NSData*) deviceToken
        {
            _deviceToken = deviceToken;
            self.LogInButton.enabled = YES;
        }
   
        -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                        AndPassword:(NSString*)password;
        {
            NSString* headerValue = [NSString stringWithFormat:@"%@:%@", username, password];
   
            NSData* encodedData = [[headerValue dataUsingEncoding:NSUTF8StringEncoding] base64EncodedDataWithOptions:NSDataBase64EncodingEndLineWithCarriageReturn];
   
            self.registerClient.authenticationHeader = [[NSString alloc] initWithData:encodedData
                                                        encoding:NSUTF8StringEncoding];
        }
   
        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }
   
    <span data-ttu-id="744d8-145">Vegye figyelembe, hogyan az eszköz jogkivonatát beállítás lehetővé teszi, hogy a Bejelentkezés gombra.</span><span class="sxs-lookup"><span data-stu-id="744d8-145">Note how setting the device token enables the log in button.</span></span> <span data-ttu-id="744d8-146">Ennek oka az, a bejelentkezési művelet részeként a view vezérlő regisztrálja, a leküldéses értesítések az a háttéralkalmazás.</span><span class="sxs-lookup"><span data-stu-id="744d8-146">This is becasue as a part of the login action, the view controller registers for push notifications with the app backend.</span></span> <span data-ttu-id="744d8-147">Emiatt nem szeretnénk bejelentkezési művelet elérhetők lesznek, amíg az eszköz jogkivonatát megfelelően be van állítva.</span><span class="sxs-lookup"><span data-stu-id="744d8-147">Hence, we do not want Log In action to be accessible till the device token has been properly set up.</span></span> <span data-ttu-id="744d8-148">Mindaddig, amíg a korábbi történik, az utóbbi előtt használata is leválasztja a leküldéses regisztráció a bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="744d8-148">You can decouple the log in from the push registration as long as the former happens before the latter.</span></span>
2. <span data-ttu-id="744d8-149">A ViewController.m, az alábbi kódtöredékek segítségével valósítja meg a műveletet a a **bejelentkezés** gombra és a értesítést küldeni az ASP.NET-háttérrendszerből használatával metódust.</span><span class="sxs-lookup"><span data-stu-id="744d8-149">In ViewController.m, use the following snippets to implement the action method for your **Log In** button and a method to send the notification message using the ASP.NET backend.</span></span>
   
       - <span data-ttu-id="744d8-150">(IBAction) LogInAction: (id) küldő {/ / hitelesítési fejléc létrehozásáról és a nyilvántartás ügyfél NSString * felhasználónév = saját maga. UsernameField.text;   NSString * jelszó = saját maga. PasswordField.text;</span><span class="sxs-lookup"><span data-stu-id="744d8-150">(IBAction)LogInAction:(id)sender {   // create authentication header and set it in register client   NSString* username = self.UsernameField.text;   NSString* password = self.PasswordField.text;</span></span>
   
           <span data-ttu-id="744d8-151">[az önkiszolgáló createAndSetAuthenticationHeaderWithUsername:username AndPassword:password];</span><span class="sxs-lookup"><span data-stu-id="744d8-151">[self createAndSetAuthenticationHeaderWithUsername:username AndPassword:password];</span></span>
   
           <span data-ttu-id="744d8-152">__weak ViewController * selfie = önkiszolgáló;   [self.registerClient registerWithDeviceToken:self.deviceToken címkék: üres andCompletion:^(NSError* error) {Ha (! hiba) {dispatch_async(dispatch_get_main_queue(), ^ {selfie. SendNotificationButton.enabled = YES;               [self MessageBox:@"Success" message:@"Registered sikeresen!"];});}}];}</span><span class="sxs-lookup"><span data-stu-id="744d8-152">__weak ViewController* selfie = self;   [self.registerClient registerWithDeviceToken:self.deviceToken tags:nil       andCompletion:^(NSError* error) {       if (!error) {           dispatch_async(dispatch_get_main_queue(),           ^{               selfie.SendNotificationButton.enabled = YES;               [self MessageBox:@"Success" message:@"Registered successfully!"];           });       }   }]; }</span></span>

        <span data-ttu-id="744d8-153">-SendNotificationASPNETBackend (a "void"): (NSString*) pns UsernameTag: (NSString*) usernameTag üzenetet: (NSString*) üzenet {NSURLSession* munkamenet = [NSURLSession sessionWithConfiguration: [ NSURLSessionConfiguration defaultSessionConfiguration] delegált: üres delegateQueue:nil];</span><span class="sxs-lookup"><span data-stu-id="744d8-153">- (void)SendNotificationASPNETBackend:(NSString*)pns UsernameTag:(NSString*)usernameTag            Message:(NSString*)message {    NSURLSession* session = [NSURLSession        sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration] delegate:nil        delegateQueue:nil];</span></span>

            <span data-ttu-id="744d8-154">A pns-sel és a username címke a többi URL-címet paraméterekként átadása a háttérrendszer által igényelt NSURL * requestURL ASP.NET = [által igényelt NSURL URLWithString: [NSString stringWithFormat:@"%@/api/notifications? pns = % @& to_tag = % @", BACKEND_ENDPOINT, pns, usernameTag]];</span><span class="sxs-lookup"><span data-stu-id="744d8-154">// Pass the pns and username tag as parameters with the REST URL to the ASP.NET backend    NSURL* requestURL = [NSURL URLWithString:[NSString        stringWithFormat:@"%@/api/notifications?pns=%@&to_tag=%@", BACKEND_ENDPOINT, pns,        usernameTag]];</span></span>

            <span data-ttu-id="744d8-155">NSMutableURLRequest * kérelem = [NSMutableURLRequest requestWithURL:requestURL];    [lekérő setHTTPMethod:@"POST"];</span><span class="sxs-lookup"><span data-stu-id="744d8-155">NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];    [request setHTTPMethod:@"POST"];</span></span>

            <span data-ttu-id="744d8-156">A utánzatait authenticationheader beszerezni a register-ügyfél NSString * authorizationHeaderValue = [NSString stringWithFormat:@"Basic % @", self.registerClient.authenticationHeader];    [lekérő setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];</span><span class="sxs-lookup"><span data-stu-id="744d8-156">// Get the mock authenticationheader from the register client    NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",        self.registerClient.authenticationHeader];    [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];</span></span>

            <span data-ttu-id="744d8-157">Az értesítési üzenet törzse hozzáadása [setValue:@"application/json;charset=utf-8 kérelem" forHTTPHeaderField:@"Content-Type"];    [setHTTPBody kérése: [üzenet dataUsingEncoding:NSUTF8StringEncoding]];</span><span class="sxs-lookup"><span data-stu-id="744d8-157">//Add the notification message body    [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];    [request setHTTPBody:[message dataUsingEncoding:NSUTF8StringEncoding]];</span></span>

            <span data-ttu-id="744d8-158">Hajtsa végre a send notification REST API-t az ASP.NET háttér NSURLSessionDataTask * dataTask = [munkamenet dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError  *Hiba történt) {NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) választ;        Ha (hiba || httpResponse.statusCode! = 200) {NSString* állapot = [NSString stringWithFormat:@"Error állapota a(z) % @: % d\nError: %@\n", pns, httpResponse.statusCode, hiba];            dispatch_async(dispatch_get_main_queue(), ^ {/ szöveg hozzáfűzése, mert minden 3 PNS-hívások is előfordulhat, hogy rendelkezik információval nézet [self.sendResults setText:[self.sendResults.text stringByAppendingString:status]];            });            NSLog(status);        }</span><span class="sxs-lookup"><span data-stu-id="744d8-158">// Execute the send notification REST API on the ASP.NET Backend    NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request        completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)    {        NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;        if (error || httpResponse.statusCode != 200)        {            NSString* status = [NSString stringWithFormat:@"Error Status for %@: %d\nError: %@\n",                                pns, httpResponse.statusCode, error];            dispatch_async(dispatch_get_main_queue(),            ^{                // Append text because all 3 PNS calls may also have information to view                [self.sendResults setText:[self.sendResults.text stringByAppendingString:status]];            });            NSLog(status);        }</span></span>

                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            <span data-ttu-id="744d8-159">}];    [dataTask folytatása]; }</span><span class="sxs-lookup"><span data-stu-id="744d8-159">}];    [dataTask resume]; }</span></span>


1. <span data-ttu-id="744d8-160">A művelet frissíti a **értesítés küldése** gombot használja az ASP.NET-háttérrendszerből és bármely, a kapcsoló által engedélyezett PNS küldeni.</span><span class="sxs-lookup"><span data-stu-id="744d8-160">Update the action for the **Send Notification** button to use the ASP.NET backend and send to any PNS enabled by a switch.</span></span>

        - (IBAction)SendNotificationMessage:(id)sender
        {
            //[self SendNotificationRESTAPI];
            [self SendToEnabledPlatforms];
        }


        -(void)SendToEnabledPlatforms
        {
            NSString* json = [NSString stringWithFormat:@"\"%@\"",self.notificationMessage.text];

            [self.sendResults setText:@""];

            if ([self.WNSSwitch isOn])
                [self SendNotificationASPNETBackend:@"wns" UsernameTag:self.RecipientField.text Message:json];

            if ([self.GCMSwitch isOn])
                [self SendNotificationASPNETBackend:@"gcm" UsernameTag:self.RecipientField.text Message:json];

            if ([self.APNSSwitch isOn])
                [self SendNotificationASPNETBackend:@"apns" UsernameTag:self.RecipientField.text Message:json];
        }



1. <span data-ttu-id="744d8-161">A függvény **ViewDidLoad**, adja hozzá a következő példányt létrehozni a RegisterClient példány és állítsa be a szövegmezők delegáltja.</span><span class="sxs-lookup"><span data-stu-id="744d8-161">In function **ViewDidLoad**, add the following to instantiate the RegisterClient instance and set the delegate for your text fields.</span></span>
   
       self.UsernameField.delegate = self;
       self.PasswordField.delegate = self;
       self.RecipientField.delegate = self;
       self.registerClient = [[RegisterClient alloc] initWithEndpoint:BACKEND_ENDPOINT];
2. <span data-ttu-id="744d8-162">Most már a **AppDelegate.m**, távolítsa el a módszer a tartalom **alkalmazás: didRegisterForPushNotificationWithDeviceToken:** , és cserélje le a következő meggyőződni arról, hogy a view vezérlő a legújabb eszköz jogkivonatát APNs lekért tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="744d8-162">Now in **AppDelegate.m**, remove all the content of the method **application:didRegisterForPushNotificationWithDeviceToken:** and replace it with the following to make sure that the view controller contains the latest device token retrieved from APNs:</span></span>
   
       // Add import to the top of the file
       #import "ViewController.h"
   
       - (void)application:(UIApplication *)application
                   didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
       {
           ViewController* rvc = (ViewController*) self.window.rootViewController;
           rvc.deviceToken = deviceToken;
       }
3. <span data-ttu-id="744d8-163">Végül a **AppDelegate.m**, győződjön meg arról, hogy a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="744d8-163">Finally in **AppDelegate.m**, make sure you have the following method:</span></span>
   
       - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
           NSLog(@"%@", userInfo);
           [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
       }

## <a name="test-the-application"></a><span data-ttu-id="744d8-164">Az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="744d8-164">Test the Application</span></span>
1. <span data-ttu-id="744d8-165">Az xcode-ban az alkalmazás futtatása egy fizikai iOS-eszközön (leküldéses értesítések nem fog működni a szimulátor).</span><span class="sxs-lookup"><span data-stu-id="744d8-165">In XCode, run the app on a physical iOS device (push notifications will not work in the simulator).</span></span>
2. <span data-ttu-id="744d8-166">Az iOS-alkalmazás felhasználói felületén adja meg egy felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="744d8-166">In the iOS app UI, enter a username and password.</span></span> <span data-ttu-id="744d8-167">Bármilyen karakterlánc is lehetnek, de mindkét kell ugyanaz a karakterláncértéke.</span><span class="sxs-lookup"><span data-stu-id="744d8-167">These can be any string, but they must both be the same string value.</span></span> <span data-ttu-id="744d8-168">Kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="744d8-168">Then click **Log In**.</span></span>
   
    ![][2]
3. <span data-ttu-id="744d8-169">Meg kell jelennie egy előugró ablak regisztráció sikeres ad tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="744d8-169">You should see a pop-up informing you of registration success.</span></span> <span data-ttu-id="744d8-170">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="744d8-170">Click **OK**.</span></span>
   
    ![][3]
4. <span data-ttu-id="744d8-171">Az a **címzett felhasználónév címke* szöveg mezőbe írja be a felhasználó nevét címke használt egy másik eszköz regisztrációját.</span><span class="sxs-lookup"><span data-stu-id="744d8-171">In the **Recipient username tag* text field, enter the user name tag used with the registration from another device.</span></span>
5. <span data-ttu-id="744d8-172">Adjon meg egy értesítési üzenetet, és kattintson a **értesítés küldése**.</span><span class="sxs-lookup"><span data-stu-id="744d8-172">Enter a notification message and click **Send Notification**.</span></span>  <span data-ttu-id="744d8-173">Csak azokat az eszközöket, amelyek a címzett felhasználói név címkével regisztráció a értesítési üzenet.</span><span class="sxs-lookup"><span data-stu-id="744d8-173">Only the devices that have a registration with the recipient user name tag receive the notification message.</span></span>  <span data-ttu-id="744d8-174">Csak azoknak a felhasználóknak továbbítja.</span><span class="sxs-lookup"><span data-stu-id="744d8-174">It is only sent to those users.</span></span>
   
    ![][4]

[1]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-interface.png
[2]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-user-pwd.png
[3]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-registered.png
[4]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-msg.png
