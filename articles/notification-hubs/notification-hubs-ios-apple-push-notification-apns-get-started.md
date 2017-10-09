---
title: "aaaSending leküldéses értesítések tooiOS az Azure Notification hubs használatával |} Microsoft Docs"
description: "Ebben az oktatóanyagban elsajátíthatja, hogyan toouse Azure Notification Hubs toosend leküldéses értesítések tooan iOS-alkalmazás."
services: notification-hubs
documentationcenter: ios
keywords: "leküldéses értesítés,leküldéses értesítések,ios leküldéses értesítések"
author: ysxu
manager: erikre
editor: 
ms.assetid: b7fcd916-8db8-41a6-ae88-fc02d57cb914
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: d8bb47fee4c229b3ed2a7a4dbff25a56a7a7d009
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sending-push-notifications-tooios-with-azure-notification-hubs"></a><span data-ttu-id="8a755-104">Az Azure Notification Hubs leküldéses értesítések tooiOS küldése</span><span class="sxs-lookup"><span data-stu-id="8a755-104">Sending push notifications tooiOS with Azure Notification Hubs</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="8a755-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="8a755-105">Overview</span></span>
> [!NOTE]
> <span data-ttu-id="8a755-106">toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="8a755-106">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="8a755-107">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="8a755-107">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="8a755-108">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="8a755-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).</span></span>
> 
> 

<span data-ttu-id="8a755-109">Az oktatóanyag bemutatja, hogyan toouse Azure Notification Hubs toosend leküldéses értesítések tooan iOS-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8a755-109">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooan iOS application.</span></span> <span data-ttu-id="8a755-110">Létre fog hozni egy üres iOS-alkalmazást, amely leküldéses értesítéseket fogad hello segítségével [Apple Push Notification szolgáltatás (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html).</span><span class="sxs-lookup"><span data-stu-id="8a755-110">You'll create a blank iOS app that receives push notifications by using hello [Apple Push Notification service (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html).</span></span> 

<span data-ttu-id="8a755-111">Amikor végzett, képes toouse lesz az értesítési központ toobroadcast leküldéses értesítések tooall hello eszközök az alkalmazást futtató.</span><span class="sxs-lookup"><span data-stu-id="8a755-111">When you're finished, you'll be able toouse your notification hub toobroadcast push notifications tooall hello devices running your app.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="8a755-112">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="8a755-112">Before you begin</span></span>
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

<span data-ttu-id="8a755-113">az oktatóanyag kódjának befejeződött hello található [a Githubon](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted).</span><span class="sxs-lookup"><span data-stu-id="8a755-113">hello completed code for this tutorial can be found [on GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="8a755-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8a755-114">Prerequisites</span></span>
<span data-ttu-id="8a755-115">Ez az oktatóanyag hello következő szükséges:</span><span class="sxs-lookup"><span data-stu-id="8a755-115">This tutorial requires hello following:</span></span>

* <span data-ttu-id="8a755-116">[Mobile Services iOS SDK 1.2.4-es]</span><span class="sxs-lookup"><span data-stu-id="8a755-116">[Mobile Services iOS SDK version 1.2.4]</span></span>
* <span data-ttu-id="8a755-117">Az [Xcode] legújabb verziója</span><span class="sxs-lookup"><span data-stu-id="8a755-117">Latest version of [Xcode]</span></span>
* <span data-ttu-id="8a755-118">Az iOS 8-cal (vagy újabb verzióval) kompatibilis eszköz</span><span class="sxs-lookup"><span data-stu-id="8a755-118">An iOS 8 (or later version)-capable device</span></span>
* <span data-ttu-id="8a755-119">Tagság az [Apple fejlesztői programjában](https://developer.apple.com/programs/).</span><span class="sxs-lookup"><span data-stu-id="8a755-119">[Apple Developer Program](https://developer.apple.com/programs/) membership.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="8a755-120">A leküldéses értesítések konfigurációs követelményei miatt kell telepítenie és hello iOS-szimulátor helyett egy fizikai iOS-eszközön (iPhone vagy iPad) leküldéses értesítések teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="8a755-120">Because of configuration requirements for push notifications, you must deploy and test push notifications on a physical iOS device (iPhone or iPad) instead of hello iOS Simulator.</span></span>
  > 
  > 

<span data-ttu-id="8a755-121">Ennek az oktatóanyagnak az elvégzése előfeltétel minden további, iOS-alkalmazásokkal kapcsolatos Notification Hubs-oktatóanyag elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="8a755-121">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for iOS apps.</span></span>

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub-for-ios-push-notifications"></a><span data-ttu-id="8a755-122">Az értesítési központ konfigurálása iOS leküldéses értesítésekhez</span><span class="sxs-lookup"><span data-stu-id="8a755-122">Configure your Notification Hub for iOS push notifications</span></span>
<span data-ttu-id="8a755-123">Ez a szakasz végigvezeti egy új értesítési központ létrehozása és konfigurálása a hitelesítés az apns-sel hello segítségével **.p12** létrehozott leküldéses tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="8a755-123">This section walks you through creating a new notification hub and configuring authentication with APNS using hello **.p12** push certificate that you created.</span></span> <span data-ttu-id="8a755-124">Ha azt szeretné, hogy toouse egy már létrehozott értesítési központot, kihagyhatja toostep 5.</span><span class="sxs-lookup"><span data-stu-id="8a755-124">If you want toouse a notification hub that you have already created, you can skip toostep 5.</span></span>

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">

<li>

<p><span data-ttu-id="8a755-125">Hello kattintson <b>értesítési szolgáltatások</b> hello gombjára <b>beállítások</b> panelen, majd válassza ki <b>Apple (APNS)</b>.</span><span class="sxs-lookup"><span data-stu-id="8a755-125">Click hello <b>Notification Services</b> button in hello <b>Settings</b> blade, then select <b>Apple (APNS)</b>.</span></span> <span data-ttu-id="8a755-126">Kattintson a <b>tanúsítvány feltöltése</b> és select hello <b>.p12</b> korábban exportált fájl.</span><span class="sxs-lookup"><span data-stu-id="8a755-126">Click on <b>Upload Certificate</b> and select hello <b>.p12</b> file that you exported earlier.</span></span> <span data-ttu-id="8a755-127">Győződjön meg arról, hogy hello helyes jelszót is meg.</span><span class="sxs-lookup"><span data-stu-id="8a755-127">Make sure you also specify hello correct password.</span></span></p>

<p><span data-ttu-id="8a755-128">Győződjön meg arról, hogy tooselect <b>védőfal</b> módot, mivel ez a fejlesztés.</span><span class="sxs-lookup"><span data-stu-id="8a755-128">Make sure tooselect <b>Sandbox</b> mode since this is for development.</span></span> <span data-ttu-id="8a755-129">Csak a hello használata <b>éles</b> Ha azt szeretné, hogy toosend leküldéses értesítések toousers akik megvásárolták az alkalmazást hello áruházból.</span><span class="sxs-lookup"><span data-stu-id="8a755-129">Only use hello <b>Production</b> if you want toosend push notifications toousers who purchased your app from hello store.</span></span></p>
</li>
</ol>
<span data-ttu-id="8a755-130">&emsp;&emsp;&emsp;&emsp;![APN szolgáltatás konfigurálása az Azure-portálon](./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png)</span><span class="sxs-lookup"><span data-stu-id="8a755-130">&emsp;&emsp;&emsp;&emsp;![Configure APNS in Azure Portal](./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png)</span></span>

&emsp;&emsp;&emsp;&emsp;![APNS-tanúsítvány konfigurálása az Azure Portalon](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)

<span data-ttu-id="8a755-132">Az értesítési központ most már konfigurált toowork az apns-sel, és rendelkezik hello kapcsolati karakterláncok tooregister az alkalmazást, és leküldéses értesítések küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="8a755-132">Your notification hub is now configured toowork with APNS, and you have hello connection strings tooregister your app and send push notifications.</span></span>

## <a name="connect-your-ios-app-toonotification-hubs"></a><span data-ttu-id="8a755-133">Csatlakoztassa az iOS app tooNotification hubok</span><span class="sxs-lookup"><span data-stu-id="8a755-133">Connect your iOS app tooNotification Hubs</span></span>
1. <span data-ttu-id="8a755-134">Az Xcode-ban hozzon létre egy új iOS-projektet, és válassza ki a hello **egyetlen nézetben alkalmazás** sablont.</span><span class="sxs-lookup"><span data-stu-id="8a755-134">In Xcode, create a new iOS project and select hello **Single View Application** template.</span></span>
   
    ![Xcode – Egynézetes alkalmazás][8]
    
2. <span data-ttu-id="8a755-136">Az új projekt hello-beállítások megadásakor, ellenőrizze, hogy toouse hello azonos **Terméknév** és **Organization Identifier** használhatók, ha az Apple Developer hello korábban beállítva hello alkalmazásköteg-azonosító portál.</span><span class="sxs-lookup"><span data-stu-id="8a755-136">When setting hello options for your new project, make sure toouse hello same **Product Name** and **Organization Identifier** that you used when you previously set hello bundle ID on hello Apple Developer portal.</span></span>
   
    ![Xcode – projektbeállítások][11]
    
3. <span data-ttu-id="8a755-138">A **célok**, kattintson a projektnévre, majd a hello **Build Settings** lapra, és bontsa ki a **Code aláíró identitásának**, majd a **Debug**, állítsa be a kódaláíró identitását.</span><span class="sxs-lookup"><span data-stu-id="8a755-138">Under **Targets**, click your project name, click hello **Build Settings** tab and expand **Code Signing Identity**, and then under **Debug**, set your code-signing identity.</span></span> <span data-ttu-id="8a755-139">Váltás **szintek** a **alapvető** túl**összes**, és állítsa be **Provisioning Profile** toohello létesítési profilt, amelyet korábban hozott létre .</span><span class="sxs-lookup"><span data-stu-id="8a755-139">Toggle **Levels** from **Basic** too**All**, and set **Provisioning Profile** toohello provisioning profile that you created previously.</span></span>
   
    <span data-ttu-id="8a755-140">Ha nem látja az új létesítési profilt, amely az xcode-ban létrehozott hello, frissítse az aláíró identitása hello-profilok.</span><span class="sxs-lookup"><span data-stu-id="8a755-140">If you don't see hello new provisioning profile that you created in Xcode, try refreshing hello profiles for your signing identity.</span></span> <span data-ttu-id="8a755-141">Kattintson a **Xcode** hello menüsávjában kattintson **beállítások**, hello kattintson **fiók** lapra, majd hello **részleteinek megtekintése** gombra, majd a aláíró identitása, és kattintson a jobb alsó sarok hello hello frissítés gombra.</span><span class="sxs-lookup"><span data-stu-id="8a755-141">Click **Xcode** on hello menu bar, click **Preferences**, click hello **Account** tab, click hello **View Details** button, click your signing identity, and then click hello refresh button in hello bottom-right corner.</span></span>
   
    ![Xcode – Létesítési profil][9]
4. <span data-ttu-id="8a755-143">Töltse le a hello [Mobile Services iOS SDK 1.2.4-es] és hello fájlt csomagolja ki.</span><span class="sxs-lookup"><span data-stu-id="8a755-143">Download hello [Mobile Services iOS SDK version 1.2.4] and unzip hello file.</span></span> <span data-ttu-id="8a755-144">Az xcode-ban, kattintson jobb gombbal a projektre, majd kattintson a hello **fájlok hozzáadása a** beállítás tooadd hello **WindowsAzureMessaging.framework** mappa tooyour Xcode projekt.</span><span class="sxs-lookup"><span data-stu-id="8a755-144">In Xcode, right-click your project and click hello **Add Files to** option tooadd hello **WindowsAzureMessaging.framework** folder tooyour Xcode project.</span></span> <span data-ttu-id="8a755-145">Válassza a **Copy items if needed** (Elemek másolása, ha szükséges) lehetőséget, majd kattintson az **Add** (Hozzáadás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8a755-145">Select **Copy items if needed**, and then click **Add**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="8a755-146">hello notification hubs SDK jelenleg nem támogatja a bitcode az Xcode 7.</span><span class="sxs-lookup"><span data-stu-id="8a755-146">hello notification hubs SDK does not currently support bitcode on Xcode 7.</span></span>  <span data-ttu-id="8a755-147">Meg kell adni **Bitcode engedélyezése** túl**nem** a hello **Build beállítások** a projekthez.</span><span class="sxs-lookup"><span data-stu-id="8a755-147">You must set **Enable Bitcode** too**No** in hello **Build Options** for your project.</span></span>
   > 
   > 
   
    ![Az Azure SDK kicsomagolása][10]
5. <span data-ttu-id="8a755-149">Adja hozzá egy új fejléc fájl tooyour nevű projekt `HubInfo.h`.</span><span class="sxs-lookup"><span data-stu-id="8a755-149">Add a new header file tooyour project named `HubInfo.h`.</span></span> <span data-ttu-id="8a755-150">Ez a fájl tárolja majd hello állandókat az értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="8a755-150">This file will hold hello constants for your notification hub.</span></span>  <span data-ttu-id="8a755-151">Adja hozzá a következő definíciókat hello, és cserélje le a hello szövegkonstans helyőrzőit a *központnév* és hello *DefaultListenSharedAccessSignature* korábban feljegyzett.</span><span class="sxs-lookup"><span data-stu-id="8a755-151">Add hello following definitions and replace hello string literal placeholders with your *hub name* and hello *DefaultListenSharedAccessSignature* that you noted earlier.</span></span>
   
        #ifndef HubInfo_h
        #define HubInfo_h
   
            #define HUBNAME @"<Enter hello name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
   
        #endif /* HubInfo_h */
6. <span data-ttu-id="8a755-152">Nyissa meg a `AppDelegate.h` fájlt adja hozzá a következő importálási irányelveket hello:</span><span class="sxs-lookup"><span data-stu-id="8a755-152">Open your `AppDelegate.h` file add hello following import directives:</span></span>
   
         #import <WindowsAzureMessaging/WindowsAzureMessaging.h> 
         #import "HubInfo.h"
7. <span data-ttu-id="8a755-153">Az a `AppDelegate.m file`, adja hozzá a következő kódot a hello hello `didFinishLaunchingWithOptions` metódus az iOS verziójától függően.</span><span class="sxs-lookup"><span data-stu-id="8a755-153">In your `AppDelegate.m file`, add hello following code in hello `didFinishLaunchingWithOptions` method based on your version of iOS.</span></span> <span data-ttu-id="8a755-154">Ez a kód regisztrálja az eszközleíróját az APNs szolgáltatással:</span><span class="sxs-lookup"><span data-stu-id="8a755-154">This code registers your device handle with APNs:</span></span>
   
    <span data-ttu-id="8a755-155">iOS 8 esetén:</span><span class="sxs-lookup"><span data-stu-id="8a755-155">For iOS 8:</span></span>
   
         UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];
   
        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];
   
    <span data-ttu-id="8a755-156">Az iOS korábbi verziói-too8:</span><span class="sxs-lookup"><span data-stu-id="8a755-156">For iOS versions prior too8:</span></span>
   
         [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
8. <span data-ttu-id="8a755-157">A hello ugyanazt a fájlt, adja hozzá a következő módszerek hello.</span><span class="sxs-lookup"><span data-stu-id="8a755-157">In hello same file, add hello following methods.</span></span> <span data-ttu-id="8a755-158">Ez a kód a hubinfo.h-ban megadott hello kapcsolati információk használatával toohello értesítési központ csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="8a755-158">This code connects toohello notification hub using hello connection information you specified in HubInfo.h.</span></span> <span data-ttu-id="8a755-159">Ezután megadja hello eszköz token toohello értesítési központot, így hello értesítési képes értesítéseket küldeni:</span><span class="sxs-lookup"><span data-stu-id="8a755-159">It then gives hello device token toohello notification hub so that hello notification hub can send notifications:</span></span>
   
        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];
   
            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
                if (error != nil) {
                    NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                    [self MessageBox:@"Registration Status" message:@"Registered"];
                }
            }];
        }
   
        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }
9. <span data-ttu-id="8a755-160">A hello ugyanazt a fájlt, adja hozzá a következő metódus toodisplay hello egy **UIAlert** Ha hello értesítés érkezik, amíg aktív hello alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="8a755-160">In hello same file, add hello following method toodisplay a **UIAlert** if hello notification is received while hello app is active:</span></span>

        - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

1. <span data-ttu-id="8a755-161">Hozza létre és hello alkalmazást futtatnak az eszköz tooverify, hogy vannak-e hibák.</span><span class="sxs-lookup"><span data-stu-id="8a755-161">Build and run hello app on your device tooverify that there are no failures.</span></span>

## <a name="send-test-push-notifications"></a><span data-ttu-id="8a755-162">Teszt leküldéses értesítések küldése</span><span class="sxs-lookup"><span data-stu-id="8a755-162">Send test push notifications</span></span>
<span data-ttu-id="8a755-163">Értesítések fogadásának az alkalmazásban való leküldéses értesítések küldése a hello tesztelheti [Azure Portal] keresztül hello **hibaelhárítás** hello hub panel szakasz (hello használata *Küldéstesztelése* lehetőséget).</span><span class="sxs-lookup"><span data-stu-id="8a755-163">You can test receiving notifications in your app by sending push notifications in hello [Azure Portal] via hello **Troubleshooting** section in hello hub blade (use hello *Test Send* option).</span></span>

![Azure portál – Küldés tesztelése][30]

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-from-hello-app"></a><span data-ttu-id="8a755-165">(Választható) Leküldéses értesítések küldéséhez hello alkalmazásból</span><span class="sxs-lookup"><span data-stu-id="8a755-165">(Optional) Send push notifications from hello app</span></span>
> [!IMPORTANT]
> <span data-ttu-id="8a755-166">Ebben a példában a hello ügyfélalkalmazás értesítések küldésére szolgáló jelleggel valósul meg.</span><span class="sxs-lookup"><span data-stu-id="8a755-166">This example of sending notifications from hello client app is provided for learning purposes only.</span></span> <span data-ttu-id="8a755-167">Mivel ezt a beállítást hello `DefaultFullSharedAccessSignature` toobe hello ügyfélalkalmazás megtalálható, azt mutatja meg az értesítési központ toohello kockázata, hogy a felhasználó szerezhetnek hozzáférés nem engedélyezett toosend értesítések tooyour ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="8a755-167">Since this will require hello `DefaultFullSharedAccessSignature` toobe present on hello client app, it exposes your notification hub toohello risk that a user may gain access toosend unauthorized notifications tooyour clients.</span></span>
> 
> 

<span data-ttu-id="8a755-168">Ha azt szeretné, hogy toosend leküldéses értesítéseket az alkalmazáson belül, ez a témakör példa bemutatja, hogyan toodo a használatával hello REST-felületen.</span><span class="sxs-lookup"><span data-stu-id="8a755-168">If you want toosend push notifications from within an app, this section provides an example of how toodo this using hello REST interface.</span></span>

1. <span data-ttu-id="8a755-169">Az Xcode-ban nyissa meg a `Main.storyboard` , és adja hozzá a felhasználói felületi összetevőket hello objektum könyvtár tooallow hello felhasználói toosend leküldéses értesítések hello alkalmazásban a következő hello:</span><span class="sxs-lookup"><span data-stu-id="8a755-169">In Xcode, open `Main.storyboard` and add hello following UI components from hello object library tooallow hello user toosend push notifications in hello app:</span></span>
   
   * <span data-ttu-id="8a755-170">Egy címke címkeszöveg nélkül.</span><span class="sxs-lookup"><span data-stu-id="8a755-170">A label with no label text.</span></span> <span data-ttu-id="8a755-171">Az értesítések küldése használt tooreport hibák lesz.</span><span class="sxs-lookup"><span data-stu-id="8a755-171">It will be used tooreport errors in sending notifications.</span></span> <span data-ttu-id="8a755-172">Hello **sorok** tulajdonságot kell beállítani, túl**0** , hogy automatikusan átméreteződik korlátozott toohello jobb és bal oldali margókhoz és hello hello nézet tetejéhez.</span><span class="sxs-lookup"><span data-stu-id="8a755-172">hello **Lines** property should be set too**0** so that it will automatically size constrained toohello right and left margins and hello top of hello view.</span></span>
   * <span data-ttu-id="8a755-173">A szövegmező **helyőrző** szöveg túl beállítása**értesítési üzenet beírása**.</span><span class="sxs-lookup"><span data-stu-id="8a755-173">A text field with **Placeholder** text set too**Enter Notification Message**.</span></span> <span data-ttu-id="8a755-174">A hozzáférési hello mezőt pont hello címke alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="8a755-174">Constrain hello field just below hello label as shown below.</span></span> <span data-ttu-id="8a755-175">Állítsa be a hello nézetvezérlő hello kilépő delegált.</span><span class="sxs-lookup"><span data-stu-id="8a755-175">Set hello View Controller as hello outlet delegate.</span></span>
   * <span data-ttu-id="8a755-176">Egy című gomb pont **értesítés küldése** hello szövegmező alá és vízszintesen középre hello korlátozott.</span><span class="sxs-lookup"><span data-stu-id="8a755-176">A button titled **Send Notification** constrained just below hello text field and in hello horizontal center.</span></span>
     
     <span data-ttu-id="8a755-177">hello nézet a következőképpen kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="8a755-177">hello view should look as follows:</span></span>
     
     ![Xcode-tervező][32]
2. <span data-ttu-id="8a755-179">[Adja hozzá kimeneteket](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) a hello címke és a szöveges nézethez kapcsolt, és frissítse a `interface` definition toosupport `UITextFieldDelegate` és `NSXMLParserDelegate`.</span><span class="sxs-lookup"><span data-stu-id="8a755-179">[Add outlets](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) for hello label and text field connected your view, and update your `interface` definition toosupport `UITextFieldDelegate` and `NSXMLParserDelegate`.</span></span> <span data-ttu-id="8a755-180">Adja hozzá a hello három tulajdonságdeklarációt alatt toohelp támogatási hello REST API hívása és hello-válasz elemzése.</span><span class="sxs-lookup"><span data-stu-id="8a755-180">Add hello three property declarations shown below toohelp support calling hello REST API and parsing hello response.</span></span>
   
    <span data-ttu-id="8a755-181">A ViewController.h fájlnak az alábbihoz kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="8a755-181">Your ViewController.h file should look as follows:</span></span>
   
        #import <UIKit/UIKit.h>
   
        @interface ViewController : UIViewController <UITextFieldDelegate, NSXMLParserDelegate>
        {
            NSXMLParser *xmlParser;
        }
   
        // Make sure these outlets are connected tooyour UI by ctrl+dragging
        @property (weak, nonatomic) IBOutlet UITextField *notificationMessage;
        @property (weak, nonatomic) IBOutlet UILabel *sendResults;
   
        @property (copy, nonatomic) NSString *statusResult;
        @property (copy, nonatomic) NSString *currentElement;
   
        @end
3. <span data-ttu-id="8a755-182">Nyissa meg `HubInfo.h` , és adja hozzá a következő állandókat, amelyek az értesítésközpontról tooyour küldéséhez használható hello.</span><span class="sxs-lookup"><span data-stu-id="8a755-182">Open `HubInfo.h` and add hello following constants which will be used for sending notifications tooyour hub.</span></span> <span data-ttu-id="8a755-183">Hello helyőrző karakterláncot cserélje le a tényleges *DefaultFullSharedAccessSignature* kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="8a755-183">Replace hello placeholder string literal with your actual *DefaultFullSharedAccessSignature* connection string.</span></span>
   
        #define API_VERSION @"?api-version=2015-01"
        #define HUBFULLACCESS @"<Enter Your DefaultFullSharedAccess Connection string>"
4. <span data-ttu-id="8a755-184">Adja hozzá a következő hello `#import` utasítások tooyour `ViewController.h` fájlt.</span><span class="sxs-lookup"><span data-stu-id="8a755-184">Add hello following `#import` statements tooyour `ViewController.h` file.</span></span>
   
        #import <CommonCrypto/CommonHMAC.h>
        #import "HubInfo.h"
5. <span data-ttu-id="8a755-185">A `ViewController.m` adja hozzá a következő kód toohello felhasználói felület megvalósításához hello.</span><span class="sxs-lookup"><span data-stu-id="8a755-185">In `ViewController.m` add hello following code toohello interface implementation.</span></span> <span data-ttu-id="8a755-186">Ez a kód fogja elemezni a *DefaultFullSharedAccessSignature* kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="8a755-186">This code will parse your *DefaultFullSharedAccessSignature* connection string.</span></span> <span data-ttu-id="8a755-187">A hello [REST API-referenciában](http://msdn.microsoft.com/library/azure/dn495627.aspx), ezzel az elemzett információval lesz használt toogenerate egy SaS-jogkivonat hello **engedélyezési** kérelemfejlécet.</span><span class="sxs-lookup"><span data-stu-id="8a755-187">As mentioned in hello [REST API reference](http://msdn.microsoft.com/library/azure/dn495627.aspx), this parsed information will be used toogenerate a SaS token for hello **Authorization** request header.</span></span>
   
        NSString *HubEndpoint;
        NSString *HubSasKeyName;
        NSString *HubSasKeyValue;
   
        -(void)ParseConnectionString
        {
            NSArray *parts = [HUBFULLACCESS componentsSeparatedByString:@";"];
            NSString *part;
   
            if ([parts count] != 3)
            {
                NSException* parseException = [NSException exceptionWithName:@"ConnectionStringParseException"
                    reason:@"Invalid full shared access connection string" userInfo:nil];
   
                @throw parseException;
            }
   
            for (part in parts)
            {
                if ([part hasPrefix:@"Endpoint"])
                {
                    HubEndpoint = [NSString stringWithFormat:@"https%@",[part substringFromIndex:11]];
                }
                else if ([part hasPrefix:@"SharedAccessKeyName"])
                {
                    HubSasKeyName = [part substringFromIndex:20];
                }
                else if ([part hasPrefix:@"SharedAccessKey"])
                {
                    HubSasKeyValue = [part substringFromIndex:16];
                }
            }
        }
6. <span data-ttu-id="8a755-188">A `ViewController.m`, frissítés hello `viewDidLoad` metódus tooparse hello kapcsolati karakterlánc hello nézet betöltésekor.</span><span class="sxs-lookup"><span data-stu-id="8a755-188">In `ViewController.m`, update hello `viewDidLoad` method tooparse hello connection string when hello view loads.</span></span> <span data-ttu-id="8a755-189">Az alábbi hello segédprogram-metódusokat is hozzáadhat toohello felhasználói felület megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="8a755-189">Also add hello utility methods, shown below, toohello interface implementation.</span></span>  

        - (void)viewDidLoad
        {
            [super viewDidLoad];
            [self ParseConnectionString];
            [_notificationMessage setDelegate:self];
        }

        -(NSString *)CF_URLEncodedString:(NSString *)inputString
        {
           return (__bridge NSString *)CFURLCreateStringByAddingPercentEscapes(NULL, (CFStringRef)inputString,
                NULL, (CFStringRef)@"!*'();:@&=+$,/?%#[]", kCFStringEncodingUTF8);
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }





1. <span data-ttu-id="8a755-190">A `ViewController.m`, adja hozzá a következő kód toohello illesztőfelület megvalósítása toogenerate hello SaS-hitelesítő jogkivonatának hello a biztosított hello **engedélyezési** fejléc, ahogyan az hello [REST API-n Hivatkozás](http://msdn.microsoft.com/library/azure/dn495627.aspx).</span><span class="sxs-lookup"><span data-stu-id="8a755-190">In `ViewController.m`, add hello following code toohello interface implementation toogenerate hello SaS authorization token that will be provided in hello **Authorization** header, as mentioned in hello [REST API Reference](http://msdn.microsoft.com/library/azure/dn495627.aspx).</span></span>
   
        -(NSString*) generateSasToken:(NSString*)uri
        {
            NSString *targetUri;
            NSString* utf8LowercasedUri = NULL;
            NSString *signature = NULL;
            NSString *token = NULL;
   
            @try
            {
                // Add expiration
                uri = [uri lowercaseString];
                utf8LowercasedUri = [self CF_URLEncodedString:uri];
                targetUri = [utf8LowercasedUri lowercaseString];
                NSTimeInterval expiresOnDate = [[NSDate date] timeIntervalSince1970];
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60;
                UInt64 expires = trunc(expiresOnDate);
                NSString* toSign = [NSString stringWithFormat:@"%@\n%qu", targetUri, expires];
   
                // Get an hmac_sha1 Mac instance and initialize with hello signing key
                const char *cKey  = [HubSasKeyValue cStringUsingEncoding:NSUTF8StringEncoding];
                const char *cData = [toSign cStringUsingEncoding:NSUTF8StringEncoding];
                unsigned char cHMAC[CC_SHA256_DIGEST_LENGTH];
                CCHmac(kCCHmacAlgSHA256, cKey, strlen(cKey), cData, strlen(cData), cHMAC);
                NSData *rawHmac = [[NSData alloc] initWithBytes:cHMAC length:sizeof(cHMAC)];
                signature = [self CF_URLEncodedString:[rawHmac base64EncodedStringWithOptions:0]];
   
                // Construct authorization token string
                token = [NSString stringWithFormat:@"SharedAccessSignature sig=%@&se=%qu&skn=%@&sr=%@",
                    signature, expires, HubSasKeyName, targetUri];
            }
            @catch (NSException *exception)
            {
                [self MessageBox:@"Exception Generating SaS Token" message:[exception reason]];
            }
            @finally
            {
                if (utf8LowercasedUri != NULL)
                    CFRelease((CFStringRef)utf8LowercasedUri);
                if (signature != NULL)
                CFRelease((CFStringRef)signature);
            }
   
            return token;
        }
2. <span data-ttu-id="8a755-191">CTRL + a húzás hello **értesítés küldése** túl gomb`ViewController.m` tooadd nevű művelet **SendNotificationMessage** a hello **Touch Down** esemény.</span><span class="sxs-lookup"><span data-stu-id="8a755-191">Ctrl+drag from hello **Send Notification** button too`ViewController.m` tooadd an action named **SendNotificationMessage** for hello **Touch Down** event.</span></span> <span data-ttu-id="8a755-192">Frissítse a metódust a következő kód toosend hello értesítést hello REST API használatával hello.</span><span class="sxs-lookup"><span data-stu-id="8a755-192">Update method with hello following code toosend hello notification using hello REST API.</span></span>
   
        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";
            [self SendNotificationRESTAPI];
        }
   
        - (void)SendNotificationRESTAPI
        {
            NSURLSession* session = [NSURLSession
                             sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                             delegate:nil delegateQueue:nil];
   
            // Apple Notification format of hello notification message
            NSString *json = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"%@\"}}",
                                self.notificationMessage.text];
   
            // Construct hello message's REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                                HUBNAME, API_VERSION]];
   
            // Generate hello token toobe used in hello authorization header
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];
   
            //Create hello request tooadd hello APNs notification message toohello hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];
   
            // Signify Apple notification format
            [request setValue:@"apple" forHTTPHeaderField:@"ServiceBusNotification-Format"];
   
            //Authenticate hello notification message POST request with hello SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];
   
            //Add hello notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
   
            // Send hello REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (error || (httpResponse.statusCode != 200 && httpResponse.statusCode != 201))
                {
                    NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                }
                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                       [xmlParser parse];
                }
            }];
            [dataTask resume];
        }
3. <span data-ttu-id="8a755-193">A `ViewController.m`, adja hozzá a következő delegált metódust toosupport hello billentyűzet hello szövegmezőt a záró hello.</span><span class="sxs-lookup"><span data-stu-id="8a755-193">In `ViewController.m`, add hello following delegate method toosupport closing hello keyboard for hello text field.</span></span> <span data-ttu-id="8a755-194">CTRL + hello szöveg mező toohello nézetvezérlő ikonra hello felület Tervező tooset hello közben húzza megtekintése a vezérlő hello kilépő delegált aláírásával.</span><span class="sxs-lookup"><span data-stu-id="8a755-194">Ctrl+drag from hello text field toohello View Controller icon in hello interface designer tooset hello view controller as hello outlet delegate.</span></span>
   
        //===[ Implement UITextFieldDelegate methods ]===
   
        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }
4. <span data-ttu-id="8a755-195">A `ViewController.m`, adja hozzá a hello következő delegálása módszerek toosupport elemzési hello válasz használatával `NSXMLParser`.</span><span class="sxs-lookup"><span data-stu-id="8a755-195">In `ViewController.m`, add hello following delegate methods toosupport parsing hello response by using `NSXMLParser`.</span></span>
   
       //===[ Implement NSXMLParserDelegate methods ]===
   
       -(void)parserDidStartDocument:(NSXMLParser *)parser
       {
           self.statusResult = @"";
       }
   
       -(void)parser:(NSXMLParser *)parser didStartElement:(NSString *)elementName
           namespaceURI:(NSString *)namespaceURI qualifiedName:(NSString *)qName
           attributes:(NSDictionary *)attributeDict
       {
           NSString * element = [elementName lowercaseString];
           NSLog(@"*** New element parsed : %@ ***",element);
   
           if ([element isEqualToString:@"code"] | [element isEqualToString:@"detail"])
           {
               self.currentElement = element;
           }
       }
   
       -(void) parser:(NSXMLParser *)parser foundCharacters:(NSString *)parsedString
       {
           self.statusResult = [self.statusResult stringByAppendingString:
               [NSString stringWithFormat:@"%@ : %@\n", self.currentElement, parsedString]];
       }
   
       -(void)parserDidEndDocument:(NSXMLParser *)parser
       {
           // Set hello status label text on hello UI thread
           dispatch_async(dispatch_get_main_queue(),
           ^{
               [self.sendResults setText:self.statusResult];
           });
       }
5. <span data-ttu-id="8a755-196">Hello projekt buildjének elkészítéséhez, és győződjön meg arról, hogy nincsenek-e hibák.</span><span class="sxs-lookup"><span data-stu-id="8a755-196">Build hello project and verify that there are no errors.</span></span>

> [!NOTE]
> <span data-ttu-id="8a755-197">Ha egy összeállítási hibát Xcode7 bitcode-támogatással kapcsolatos, módosítania kell a hello **Build Settings** > **engedélyezése Bitcode (ENABLE_BITCODE)** túl**nem** az xcode-ban.</span><span class="sxs-lookup"><span data-stu-id="8a755-197">If you encounter a build error in Xcode7 about bitcode support, you should change hello **Build Settings** > **Enable Bitcode (ENABLE_BITCODE)** too**NO** in Xcode.</span></span> <span data-ttu-id="8a755-198">hello Notification Hubs SDK jelenleg nem támogatja a bitcode.</span><span class="sxs-lookup"><span data-stu-id="8a755-198">hello Notification Hubs SDK does not currently support bitcode.</span></span> 
> 
> 

<span data-ttu-id="8a755-199">Minden hello értesítés lehetséges hasznos adatot megtalálja az hello Apple [helyi és leküldéses értesítések programozásával foglalkozó útmutatójában].</span><span class="sxs-lookup"><span data-stu-id="8a755-199">You can find all hello possible notification payloads in hello Apple [Local and Push Notification Programming Guide].</span></span>

## <a name="checking-if-your-app-can-receive-push-notifications"></a><span data-ttu-id="8a755-200">Annak ellenőrzése, hogy az alkalmazás képes-e leküldéses értesítéseket fogadni</span><span class="sxs-lookup"><span data-stu-id="8a755-200">Checking if your app can receive push notifications</span></span>
<span data-ttu-id="8a755-201">tootest leküldéses értesítések IOS rendszerű eszközökön, telepítenie kell a hello app tooa fizikai iOS-eszközön.</span><span class="sxs-lookup"><span data-stu-id="8a755-201">tootest push notifications on iOS, you must deploy hello app tooa physical iOS device.</span></span> <span data-ttu-id="8a755-202">Az Apple leküldéses értesítések hello iOS-szimulátor használatával nem lehet elküldeni.</span><span class="sxs-lookup"><span data-stu-id="8a755-202">You cannot send Apple push notifications by using hello iOS Simulator.</span></span>

1. <span data-ttu-id="8a755-203">Hello alkalmazás futtatását, és győződjön meg arról, hogy a regisztráció sikeres, és nyomja le az **OK**.</span><span class="sxs-lookup"><span data-stu-id="8a755-203">Run hello app and verify that registration succeeds, and then press **OK**.</span></span>
   
    ![iOS-alkalmazás leküldésesértesítés-regisztrációs tesztje][33]
2. <span data-ttu-id="8a755-205">Teszt leküldéses értesítést küldhet a hello [Azure Portal]fent leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="8a755-205">You can send a test push notification from hello [Azure Portal], as described above.</span></span> <span data-ttu-id="8a755-206">Ha hozzáadott kódot hello alkalmazásban leküldéses értesítések küldéséhez, érintse meg hello szöveg mező tooenter egy értesítési üzenetet.</span><span class="sxs-lookup"><span data-stu-id="8a755-206">If you added code for sending push notifications in hello app, touch inside hello text field tooenter a notification message.</span></span> <span data-ttu-id="8a755-207">Nyomja le az hello **küldése** gombra hello billentyűzet vagy hello **értesítés küldése** hello értesítési nézet toosend üdvözlőüzenetére gombjára.</span><span class="sxs-lookup"><span data-stu-id="8a755-207">Then press hello **Send** button on hello keyboard or hello **Send Notification** button in hello view toosend hello notification message.</span></span>
   
    ![iOS-alkalmazás leküldésesértesítés-küldési tesztje][34]
3. <span data-ttu-id="8a755-209">hello leküldéses értesítést küld tooall eszközök regisztrált tooreceive hello értesítéseket hello adott értesítési központból.</span><span class="sxs-lookup"><span data-stu-id="8a755-209">hello push notification is sent tooall devices that are registered tooreceive hello notifications from hello particular Notification Hub.</span></span>
   
    ![iOS-alkalmazás leküldésesértesítés-fogadási tesztje][35]

## <a name="next-steps"></a><span data-ttu-id="8a755-211">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8a755-211">Next steps</span></span>
<span data-ttu-id="8a755-212">Ez az egyszerű példában küldött leküldéses értesítések tooall regisztrált iOS-eszközre.</span><span class="sxs-lookup"><span data-stu-id="8a755-212">In this simple example, you broadcasted push notifications tooall your registered iOS devices.</span></span> <span data-ttu-id="8a755-213">Azt javasoljuk, hogy folytatja-e toohello a tanulás következő lépéseként, [Azure Notification Hubs – felhasználók értesítése iOS rendszerhez .NET-háttérrendszerrel] oktatóanyag, amely végigvezeti a létrehozása olyan háttér toosend leküldéses értesítések címkék használatával.</span><span class="sxs-lookup"><span data-stu-id="8a755-213">We suggest as a next step in your learning that you proceed toohello [Azure Notification Hubs Notify Users for iOS with .NET backend] tutorial, which will walk you through creating a backend toosend push notifications using tags.</span></span> 

<span data-ttu-id="8a755-214">Ha a felhasználókat érdeklődési körök alapján szeretné toosegment, továbbléphet a toohello [legfrissebb hírek Notification Hubs használata toosend] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="8a755-214">If you want toosegment your users by interest groups, you can additionally move on toohello [Use Notification Hubs toosend breaking news] tutorial.</span></span> 

<span data-ttu-id="8a755-215">A Notification Hubs használatával kapcsolatban a [Notification Hubs használatával] foglalkozó témakörben tekinthet meg további általános információt.</span><span class="sxs-lookup"><span data-stu-id="8a755-215">For general information about Notification Hubs, see [Notification Hubs Guidance].</span></span>

<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[34]: ./media/notification-hubs-ios-get-started/notification-hubs-test2.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[Mobile Services iOS SDK 1.2.4-es]: http://aka.ms/kymw2g
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Azure Classic Portal]: https://manage.windowsazure.com/
[Notification Hubs használatával]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Azure Notification Hubs – felhasználók értesítése iOS rendszerhez .NET-háttérrendszerrel]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[legfrissebb hírek Notification Hubs használata toosend]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[helyi és leküldéses értesítések programozásával foglalkozó útmutatójában]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Azure Portal]: https://portal.azure.com
