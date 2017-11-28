---
title: "Az Azure Mobile Engagement felhasználói felület - a Reach hogyan"
description: "Felhasználói felület és Azure Mobile Engagement áttekintése"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 30af87e6-c816-4cce-8609-6cbd3e83de14
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 33a0a9d0c399cb7f0a791c4c16dde2e2d62364ca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-get-started-using-and-managing-pushes-to-reach-out-to-your-end-users"></a><span data-ttu-id="12fdd-103">Első lépések, használata és kezelése a végfelhasználók számára elérhetők a leküldéses értesítések</span><span class="sxs-lookup"><span data-stu-id="12fdd-103">How to get started using and managing pushes to reach out to your end users</span></span>
<span data-ttu-id="12fdd-104">Ha az SDK az alkalmazás teljesen integrálva van, is használatának megkezdésében a leküldéses értesítések küldéséhez a felhasználók számára az alkalmazás a felhasználói felület Reach szakasza.</span><span class="sxs-lookup"><span data-stu-id="12fdd-104">Once the SDK is fully integrated into your app, you can get started using the the Reach section of the UI to Push notifications to the users of your app.</span></span>  

## <a name="do-your-first-push-notification-campaign"></a><span data-ttu-id="12fdd-105">Hajtsa végre az első leküldéses Értesítéses kampány</span><span class="sxs-lookup"><span data-stu-id="12fdd-105">Do Your First Push Notification Campaign</span></span>
* <span data-ttu-id="12fdd-106">Győződjön meg arról, hogy a Reach integrálva van az alkalmazás az SDK-val.</span><span class="sxs-lookup"><span data-stu-id="12fdd-106">Confirm that your Reach is integrated into your app with the SDK.</span></span> 
* <span data-ttu-id="12fdd-107">Az alkalmazás kiválasztása</span><span class="sxs-lookup"><span data-stu-id="12fdd-107">Select your application</span></span>

![First1][1]

* <span data-ttu-id="12fdd-109">Nyissa meg az "Reach" szakaszban, és kattintson a "közlemény"</span><span class="sxs-lookup"><span data-stu-id="12fdd-109">Go to the "Reach" Section and Click "New announcement"</span></span>

![First2][2]

* <span data-ttu-id="12fdd-111">Hozzon létre egy új kampányt, és adjon neki nevet</span><span class="sxs-lookup"><span data-stu-id="12fdd-111">Create a new campaign and name it</span></span>
  
![First3][3]

* <span data-ttu-id="12fdd-113">Válassza ki, hogyan az értesítés kézbesítési, mint alkalmazásbeli csak</span><span class="sxs-lookup"><span data-stu-id="12fdd-113">Select how the notification should be delivered, as In-app only</span></span>

![First4][4]

* <span data-ttu-id="12fdd-115">A leküldéses üzenet létrehozása</span><span class="sxs-lookup"><span data-stu-id="12fdd-115">Create the message you want to push</span></span>

![First5][5]

* <span data-ttu-id="12fdd-117">A cím írhat az értesítésre kattinthat (nem kötelező).</span><span class="sxs-lookup"><span data-stu-id="12fdd-117">You may write a title on the notification (Optional).</span></span>
* <span data-ttu-id="12fdd-118">Írás a leküldéses üzenet tartalma.</span><span class="sxs-lookup"><span data-stu-id="12fdd-118">Write push message content.</span></span>
* <span data-ttu-id="12fdd-119">Lemezkép feltöltése</span><span class="sxs-lookup"><span data-stu-id="12fdd-119">You can upload an image.</span></span> <span data-ttu-id="12fdd-120">Vegye figyelembe, hogy a fájl mérete nem haladhatja meg 32768 bájt.</span><span class="sxs-lookup"><span data-stu-id="12fdd-120">Be aware that the size of the file cannot exceed 32,768 bytes.</span></span>
* <span data-ttu-id="12fdd-121">Akkor is választhatók ki további beállításokat, de ez az oktatóanyag a fókusz, az azt láthatja, hogy később.</span><span class="sxs-lookup"><span data-stu-id="12fdd-121">You also have the ability to select further options, but for the focus of this tutorial, we will see that later.</span></span>
* <span data-ttu-id="12fdd-122">Jelölje ki a tartalomtípus csak értesítés</span><span class="sxs-lookup"><span data-stu-id="12fdd-122">Select the content type as Notification only</span></span>

![First6][6]

* <span data-ttu-id="12fdd-124">A leküldéses kampány létrehozásához, és akkor jelenik meg a kampány listáján.</span><span class="sxs-lookup"><span data-stu-id="12fdd-124">Create your push campaign and it will appear in your campaign list.</span></span>

![First7][7]

## <a name="test-your-push-notification-campaign"></a><span data-ttu-id="12fdd-126">A leküldéses értesítési kampány tesztelése</span><span class="sxs-lookup"><span data-stu-id="12fdd-126">Test Your Push Notification Campaign</span></span>
![Test1][8]

* <span data-ttu-id="12fdd-128">Regisztrálja az eszközt.</span><span class="sxs-lookup"><span data-stu-id="12fdd-128">Register your device.</span></span>
* <span data-ttu-id="12fdd-129">Kattintson az eszköz kíván leküldeni a jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="12fdd-129">Click on the checkbox of the device you want to push.</span></span>
* <span data-ttu-id="12fdd-130">Kattintson a "Test" gombra a leküldéses küldeni az eszközön.</span><span class="sxs-lookup"><span data-stu-id="12fdd-130">Click on the "Test" button to send the push to the device.</span></span>

![Teszt2][9]

* <span data-ttu-id="12fdd-132">Aktiválja a kampányt.</span><span class="sxs-lookup"><span data-stu-id="12fdd-132">Activate the campaign</span></span>

![Teszt3][10]

* <span data-ttu-id="12fdd-134">Most, hogy létrehozta a kampány csak azt az értesítés a felhasználóknak leküldött aktiválni kell.</span><span class="sxs-lookup"><span data-stu-id="12fdd-134">Now that you have created your campaign you just need to activate it for the notification to be pushed to your users.</span></span>

## <a name="send-personalized-pushes"></a><span data-ttu-id="12fdd-135">Személyre szabott leküldéses értesítések küldése</span><span class="sxs-lookup"><span data-stu-id="12fdd-135">Send Personalized Pushes</span></span>
* <span data-ttu-id="12fdd-136">Ez a példa egy leküldéses, ha egy egyéni visszatérítési kódja bekerültek a leküldéses értesítési hoz létre.</span><span class="sxs-lookup"><span data-stu-id="12fdd-136">This example creates a push where a custom rebate code is entered into the push notification.</span></span>

![Personalize1][11]

<span data-ttu-id="12fdd-138">Személyre szabás működésével úgy lecserélésével a mutató által egy app-info címke a akkor kell győződjön meg arról, hogy a felhasználó rendelkezik-e a megfelelő app-info első.</span><span class="sxs-lookup"><span data-stu-id="12fdd-138">Personalization works by replacing a marker by from an app info tag so, you'll have to make sure the user has the proper app-info defined first.</span></span> <span data-ttu-id="12fdd-139">Ebben a példában a megcélzott felhasználók rendelkeznek egy meghatározott rebate_code nevű alkalmazás info címke.</span><span class="sxs-lookup"><span data-stu-id="12fdd-139">In this example the targeted users will have an app info tag named rebate_code defined.</span></span>
<span data-ttu-id="12fdd-140">Ahogy fent látni a leküldéses értesítési tartalom a jelölő ${rebate_code}, amely tájékoztatja arról, hogy azt a tényleges tartalom a app-info címke által cserélni kívánt magában foglalja.</span><span class="sxs-lookup"><span data-stu-id="12fdd-140">As you see above the push notification content includes the marker ${rebate_code} which will indicate that it is to be replaced by the actual content of the app info tag.</span></span>

> [!WARNING]
> <span data-ttu-id="12fdd-141">Ha a felhasználó nincs definiálva a app-info címke, a felhasználó nem kap a leküldéses.</span><span class="sxs-lookup"><span data-stu-id="12fdd-141">If the app info tag is not defined for the user, the user will not receive the push.</span></span>

* <span data-ttu-id="12fdd-142">eredménye</span><span class="sxs-lookup"><span data-stu-id="12fdd-142">Result</span></span>

![Personalize2][12]

### <a name="you-can-further-personalize-the-text-your-notification"></a><span data-ttu-id="12fdd-144">További személyre szabhatja a szöveget az értesítés</span><span class="sxs-lookup"><span data-stu-id="12fdd-144">You can further personalize the text your notification</span></span>
![Personalize3][13]

* <span data-ttu-id="12fdd-146">Az értesítés címe</span><span class="sxs-lookup"><span data-stu-id="12fdd-146">Including the title of the notification,</span></span>
* <span data-ttu-id="12fdd-147">És az üzenet tartalma.</span><span class="sxs-lookup"><span data-stu-id="12fdd-147">And the content of the message.</span></span>
* <span data-ttu-id="12fdd-148">Válassza ki a közlemény (szöveges vagy webes nézetben)</span><span class="sxs-lookup"><span data-stu-id="12fdd-148">Choose the type of announcement (Text view or Web view)</span></span>

![Personalize4][14]

### <a name="the-body-of-an-announcement-may-also-be-personalized-with"></a><span data-ttu-id="12fdd-150">A szervezet bejelentés előfordulhat, hogy is az általunk:</span><span class="sxs-lookup"><span data-stu-id="12fdd-150">The body of an announcement may also be personalized with:</span></span>
* <span data-ttu-id="12fdd-151">A műveleti URL-cím kell szeretné testre szabni a kezdőlapja</span><span class="sxs-lookup"><span data-stu-id="12fdd-151">The action URL, should you want to customize the landing page</span></span>
* <span data-ttu-id="12fdd-152">A cím</span><span class="sxs-lookup"><span data-stu-id="12fdd-152">The title,</span></span>
* <span data-ttu-id="12fdd-153">Az üzenet törzsét.</span><span class="sxs-lookup"><span data-stu-id="12fdd-153">The body of the message.</span></span>

## <a name="differentiate-your-push-notification-in-or-out-of-app"></a><span data-ttu-id="12fdd-154">A leküldéses értesítési megkülönböztetéséhez (a vagy alkalmazásból)</span><span class="sxs-lookup"><span data-stu-id="12fdd-154">Differentiate Your Push Notification (in or out of app)</span></span>
* <span data-ttu-id="12fdd-155">Válassza ki az értesítési fog leküldéses, válassza ki az alkalmazását, nyissa meg a "Elérni" részt, vagy a leküldéses kampány létrehozásához, és nyissa meg az "Értesítések" szakaszhoz.</span><span class="sxs-lookup"><span data-stu-id="12fdd-155">Choose the type of notification you will push, select your application, go to the "Reach" section, select or create a push campaign and go to the "Notification" section.</span></span>
* <span data-ttu-id="12fdd-156">Kattintson a "szállítási mód" szeretné.</span><span class="sxs-lookup"><span data-stu-id="12fdd-156">Click on the "delivery mode" you want.</span></span>
* <span data-ttu-id="12fdd-157">Kattintson a "Korlátozása tevékenységek" jelölőnégyzetet, ha azt szeretné, hogy az értesítést az adott tevékenységei (képernyői) történik.</span><span class="sxs-lookup"><span data-stu-id="12fdd-157">Click on the "Restrict Activities" checkbox when you want the notification occurs on specific activities (screens).</span></span>

![Differentiate1][15]

### <a name="out-of-app-only-delivery-mode"></a><span data-ttu-id="12fdd-159">"Csak az alkalmazáson kívül" szállítási mód</span><span class="sxs-lookup"><span data-stu-id="12fdd-159">"Out of App Only" delivery mode</span></span>
![Differentiate2][16]

<span data-ttu-id="12fdd-161">"Csak az alkalmazáson kívül" kézbesítési módot biztosít a leküldéses értesítést, ha az alkalmazáshoz be van zárva.</span><span class="sxs-lookup"><span data-stu-id="12fdd-161">"Out of App Only" delivery mode provides push notification when the application is closed.</span></span> <span data-ttu-id="12fdd-162">Ez az a szabványos leküldéses értesítést.</span><span class="sxs-lookup"><span data-stu-id="12fdd-162">This is the standard push notification.</span></span>
<span data-ttu-id="12fdd-163">Kiválasztásakor "csak az alkalmazáson kívül" kell már megadta a platform, amely az alkalmazás (APNS vagy GCM) van építve a tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="12fdd-163">When you select "out of app only" ,you must have already provided the certificates from the platform that your application is building on (APNS or GCM).</span></span>

### <a name="see-also"></a><span data-ttu-id="12fdd-164">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="12fdd-164">See also</span></span>
* <span data-ttu-id="12fdd-165">[Apple Push Notification szolgáltatás – tanúsítványok](http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9), [Google Cloud Messaging – tanúsítvány](http://developer.android.com/google/gcm/index.html)</span><span class="sxs-lookup"><span data-stu-id="12fdd-165">[Apple Push Notification Service – Certificates](http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9), [Google Cloud Messaging – Certificate](http://developer.android.com/google/gcm/index.html)</span></span> 

### <a name="in-app-only-delivery-mode"></a><span data-ttu-id="12fdd-166">"az alkalmazáson belüli csak" szállítási mód</span><span class="sxs-lookup"><span data-stu-id="12fdd-166">"in-App Only" delivery mode</span></span>
![Differentiate3][17]

<span data-ttu-id="12fdd-168">"Alkalmazásbeli csak" szállítási mód leküldéses értesítéseket biztosít, az alkalmazás futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="12fdd-168">"In-App Only" delivery mode provides push notification when the application is running.</span></span>
<span data-ttu-id="12fdd-169">Erre az értesítésre nem kell végighaladnia az APNS-GCM rendszer.</span><span class="sxs-lookup"><span data-stu-id="12fdd-169">For this notification, you do not need to go through the APNS and GCM system.</span></span>
<span data-ttu-id="12fdd-170">Az alkalmazáson belüli kézbesítési rendszer használhatja, ha a végfelhasználók eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="12fdd-170">You can use the in-app delivery system to reach your end-users.</span></span>
<span data-ttu-id="12fdd-171">Teljes körű testre szabhatja az értesítés, és döntse el, ahol tevékenység (képernyő) az értesítés jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="12fdd-171">You can fully customize the notification and decide in which activity (screen) the notification will appear.</span></span>

### <a name="anytime-delivery-mode"></a><span data-ttu-id="12fdd-172">"Bármikor" szállítási mód</span><span class="sxs-lookup"><span data-stu-id="12fdd-172">"Anytime" delivery mode</span></span>
<span data-ttu-id="12fdd-173">Kiválaszthatja az "Bármikor" szállítási módban, biztosítja, hogy elérni a végfelhasználó használni tudja, hogy az alkalmazás fut-e.</span><span class="sxs-lookup"><span data-stu-id="12fdd-173">You can choose an "Anytime" delivery mode, ensures you to reach your end-user whether the application is running or not.</span></span>
<span data-ttu-id="12fdd-174">"Bármikor" kiválasztásakor kell már megadta a tanúsítványokat a platform, amely az alkalmazás van építve a (APNS vagy GCM).</span><span class="sxs-lookup"><span data-stu-id="12fdd-174">When you select "Anytime" , you must have already provided the certificates from the platform that your application is building upon (APNS or GCM).</span></span> 

## <a name="schedule-a-push-campaign"></a><span data-ttu-id="12fdd-175">A leküldéses kampány ütemezése</span><span class="sxs-lookup"><span data-stu-id="12fdd-175">Schedule a Push Campaign</span></span>
### <a name="plan-to-start-a-campaign"></a><span data-ttu-id="12fdd-176">Tervezze meg a kampány indítására</span><span class="sxs-lookup"><span data-stu-id="12fdd-176">Plan to Start a campaign</span></span>
![Shedule1][18]

<span data-ttu-id="12fdd-178">A 21 a március, és rendelkezik egy értesítést, hogy tervezett a a 22. a március éjfélkor.</span><span class="sxs-lookup"><span data-stu-id="12fdd-178">It is the 21st of March and you have an announcement to make and planed for the 22nd of March at midnight.</span></span> <span data-ttu-id="12fdd-179">Nem kell egy leküldéses ehhez a felület elé maradnak!</span><span class="sxs-lookup"><span data-stu-id="12fdd-179">You don’t have to stay in front of the interface to do a push!</span></span> <span data-ttu-id="12fdd-180">Megtervezheti, hogy előzetesen a pontos percet a rendszer értesítést küld.</span><span class="sxs-lookup"><span data-stu-id="12fdd-180">You can plan in advance the exact minute notifications will be sent.</span></span>

* <span data-ttu-id="12fdd-181">UN-ellenőrzése a "None" jelölőnégyzetet, és válassza ki a kezdési idő</span><span class="sxs-lookup"><span data-stu-id="12fdd-181">Un-check the "None" checkbox and select a start time</span></span> 
* <span data-ttu-id="12fdd-182">Válassza ki a dátum és időpont elindítja a leküldéses kampány.</span><span class="sxs-lookup"><span data-stu-id="12fdd-182">Choose the date and the time you want to start the push campaign.</span></span>

### <a name="plan-to-end-a-campaign"></a><span data-ttu-id="12fdd-183">Tervezze meg a kampány befejezéséhez</span><span class="sxs-lookup"><span data-stu-id="12fdd-183">Plan to end a campaign</span></span>
![Shedule2][19]

<span data-ttu-id="12fdd-185">Azt szeretné, hogy a kampány du. 3:00 meg a 25 a március leállítására, de Ön tudja róla, hogy akkor van rá.</span><span class="sxs-lookup"><span data-stu-id="12fdd-185">You want your campaign to stop on the 25th of March at 3.00 pm but you know you won't be there to do it.</span></span>
<span data-ttu-id="12fdd-186">A felület leküldéses elé marad nincs!</span><span class="sxs-lookup"><span data-stu-id="12fdd-186">You don’t have to stay in front of the interface to push!</span></span> <span data-ttu-id="12fdd-187">Megtervezheti, hogy előzetesen a pontos percet a kampány befejeződik.</span><span class="sxs-lookup"><span data-stu-id="12fdd-187">You can plan in advance the exact minute your campaign will stop.</span></span>

* <span data-ttu-id="12fdd-188">Kattintson a "None" a jelölőnégyzet jelölését, vagy válasszon egy befejezési időpontja</span><span class="sxs-lookup"><span data-stu-id="12fdd-188">Click on the "None" checkbox or select a end time</span></span>
* <span data-ttu-id="12fdd-189">Válassza ki a dátum és időpont befejezi a leküldéses kampány.</span><span class="sxs-lookup"><span data-stu-id="12fdd-189">Choose the date and the time you want to finish the push campaign.</span></span>

### <a name="end-a-campaign-manually"></a><span data-ttu-id="12fdd-190">A kampány manuálisan végén</span><span class="sxs-lookup"><span data-stu-id="12fdd-190">End a campaign manually</span></span>
![Shedule3][20]

<span data-ttu-id="12fdd-192">Alapértelmezés szerint a "None"-jelölőnégyzeteket.</span><span class="sxs-lookup"><span data-stu-id="12fdd-192">By default, the "None" check-boxes are selected.</span></span>
<span data-ttu-id="12fdd-193">Ez azt jelenti, hogy a kampány megkezdődik, amint aktiválja a reach szakaszban, és amikor a reach szakasz leáll.</span><span class="sxs-lookup"><span data-stu-id="12fdd-193">This means that the campaign will start as soon as you activate it in the reach section and will end when you will stop it on the reach section.</span></span>

> [!NOTE]
> <span data-ttu-id="12fdd-194">A záró dátum nélküli létrehozott kampányok a leküldéses helyileg tárolja az eszközön, és szerepel, hogy a következő megnyitásakor az alkalmazás akkor is, ha a kampány manuálisan véget ér.</span><span class="sxs-lookup"><span data-stu-id="12fdd-194">Campaigns created without an end date store the push locally on the device and show it the next time the app is opened even if the campaign is manually ended.</span></span>

## <a name="enhance-a-push-notification-with-a-text-view"></a><span data-ttu-id="12fdd-195">A szöveges nézet a leküldéses értesítés javítása</span><span class="sxs-lookup"><span data-stu-id="12fdd-195">Enhance a Push Notification with a Text View</span></span>
### <a name="what-is-a-text-view"></a><span data-ttu-id="12fdd-196">Mi az a szöveg nézetet?</span><span class="sxs-lookup"><span data-stu-id="12fdd-196">What is a Text View?</span></span>
![TextView1][21]

<span data-ttu-id="12fdd-198">A szöveges nézet olyan szöveges tartalommal előugró ablak.</span><span class="sxs-lookup"><span data-stu-id="12fdd-198">A text view is a pop-up with text content.</span></span> <span data-ttu-id="12fdd-199">Az előugró ablak akkor jelenik meg, miután a végfelhasználó a leküldéses értesítésre kattintott.</span><span class="sxs-lookup"><span data-stu-id="12fdd-199">This pop-up appears after the end-user has clicked on the push notification.</span></span>
<span data-ttu-id="12fdd-200">A szöveges nézet lehetővé teszi a végfelhasználó használni tudja a további tartalmat.</span><span class="sxs-lookup"><span data-stu-id="12fdd-200">A text view allows you to present more content to your end-user.</span></span> <span data-ttu-id="12fdd-201">Ez egyben a lehetőség van a művelet, például az alkalmazás-Áruházbeli, a weblap megnyitásakor egy földrajzi helyét keresés indítása e-mail, küldés átirányítása egy lap Ugrás hívása stb...</span><span class="sxs-lookup"><span data-stu-id="12fdd-201">This is also the opportunity to present a call to action such as jumping to a page of your app, redirecting to a Store, opening a web page, sending an e-mail, starting a geo-localized search, etc...</span></span>

### <a name="example-text-view"></a><span data-ttu-id="12fdd-202">Példa: Szöveges nézet</span><span class="sxs-lookup"><span data-stu-id="12fdd-202">Example: Text View</span></span>
* <span data-ttu-id="12fdd-203">A leküldéses értesítési kampány létrehozásához a "Reach" szakaszban, és nevezze el a kampány</span><span class="sxs-lookup"><span data-stu-id="12fdd-203">Create your Push notification campaign in the "Reach" section and give your campaign a name</span></span>

![TextView2][22]

* <span data-ttu-id="12fdd-205">Az üzenet, amely megjelenik majd írni az értesítésre kattinthat.</span><span class="sxs-lookup"><span data-stu-id="12fdd-205">Write the message that will appear on the notification.</span></span>
* <span data-ttu-id="12fdd-206">Válassza ki a "text" tartalomtípusa bejelentés</span><span class="sxs-lookup"><span data-stu-id="12fdd-206">Select the Announcement Content Type of “text”</span></span>

![TextView3][23]

> [!NOTE]
> <span data-ttu-id="12fdd-208">Amikor a szöveges nézet, azt mindig tartalmaz egy értesítési először.</span><span class="sxs-lookup"><span data-stu-id="12fdd-208">When you push a text view, it always comes with a notification first.</span></span> 

* <span data-ttu-id="12fdd-209">Adja meg a szöveget (kiválasztását követően a szöveg közlemény tartalmát, a szakasz jelenik meg, lehetővé téve adja meg a megjeleníteni kívánt szöveg.)</span><span class="sxs-lookup"><span data-stu-id="12fdd-209">Define the text (After having selected the text announcement content, the sub-section will appear, allowing you to define the text you want to be displayed.)</span></span>

![TextView4][24]

* <span data-ttu-id="12fdd-211">A cím az üzenet tetején megjelenő írni.</span><span class="sxs-lookup"><span data-stu-id="12fdd-211">Write the title that will appear at the top of the message.</span></span>
* <span data-ttu-id="12fdd-212">A szöveges nézet fő tartalmának írása.</span><span class="sxs-lookup"><span data-stu-id="12fdd-212">Write the main content of the text view.</span></span>
* <span data-ttu-id="12fdd-213">Írjon a tartalmat, amely megjelenik majd az akciógombra kattinthat (Akciógomb lehetővé teszi, hogy az alkalmazás megfelelően egy adott művelet, például az alkalmazás, az alkalmazás-áruházra vagy bármilyen típusú források megadhat egy lap megnyitása).</span><span class="sxs-lookup"><span data-stu-id="12fdd-213">Write the content that will appear on the action button (an action button enables the application to make a specific action such as opening a page of the application, redirecting to an App store or any kind of sources you can provide).</span></span>
* <span data-ttu-id="12fdd-214">Írás a tartalmat, amely megjelenik majd a Kilépés gombra (a Kilépés gombra kattintva, a szöveges nézet eltűnik.)</span><span class="sxs-lookup"><span data-stu-id="12fdd-214">Write the content that will appear on the exit button (by clicking on the exit button, the text view will disappear.)</span></span>
* <span data-ttu-id="12fdd-215">A leküldéses értesítési kampány létrehozásához, és akkor jelenik meg a kampány listában.</span><span class="sxs-lookup"><span data-stu-id="12fdd-215">Create your push notification campaign and it will appear on the campaign list.</span></span>

![TextView5][25]

* <span data-ttu-id="12fdd-217">Aktiválja a leküldéses értesítési kampány történő küldéséhez a szöveges nézet a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="12fdd-217">Activate your push notification campaign to send the text view to your users.</span></span>

![TextView6][26]

* <span data-ttu-id="12fdd-219">eredménye</span><span class="sxs-lookup"><span data-stu-id="12fdd-219">Result</span></span>

![TextView7][27]

* <span data-ttu-id="12fdd-221">A felhasználó kap a értesítési, majd kattintson rá.</span><span class="sxs-lookup"><span data-stu-id="12fdd-221">The user receives the notification and click on it.</span></span>
* <span data-ttu-id="12fdd-222">A szöveges nézet jelenik meg egy előugró ablak, a felhasználó használhatja őket.</span><span class="sxs-lookup"><span data-stu-id="12fdd-222">The text view appears as a pop-up allowing the user to interact with it.</span></span>

## <a name="enhance-a-push-notification-with-a-web-view"></a><span data-ttu-id="12fdd-223">A webes nézet a leküldéses értesítés javítása</span><span class="sxs-lookup"><span data-stu-id="12fdd-223">Enhance a Push Notification with a Web View</span></span>
### <a name="what-is-a-web-view"></a><span data-ttu-id="12fdd-224">Mi az a webes nézet?</span><span class="sxs-lookup"><span data-stu-id="12fdd-224">What is a Web View?</span></span>
![WebView1][28]

<span data-ttu-id="12fdd-226">A webes nézet egy előugró ablak webtartalmakkal együtt.</span><span class="sxs-lookup"><span data-stu-id="12fdd-226">A web view is a pop-up with web content.</span></span> <span data-ttu-id="12fdd-227">Ez az előugró ablak akkor jelenik meg, ha a végfelhasználó a leküldéses értesítésre kattintott.</span><span class="sxs-lookup"><span data-stu-id="12fdd-227">This pop-up appears when the end-user has clicked on the push notification.</span></span>
<span data-ttu-id="12fdd-228">A webes nézet több interakciót tesz lehetővé a végfelhasználóval.</span><span class="sxs-lookup"><span data-stu-id="12fdd-228">A web view allows you to have more interaction with the end-user.</span></span>
<span data-ttu-id="12fdd-229">Ez az App Store áruházból, például az átirányítási teendő bevezetését lehetőséget is egy weblap megnyitásakor, egy e-mailek küldéséhez, a földrajzi helyét keresés indítása stb...</span><span class="sxs-lookup"><span data-stu-id="12fdd-229">This is also the opportunity to present a call to action such as redirection to App Store, opening a web page, sending an e-mail, starting a geo-localized search, etc...</span></span>

### <a name="example-web-view"></a><span data-ttu-id="12fdd-230">Példa: Webes nézet</span><span class="sxs-lookup"><span data-stu-id="12fdd-230">Example: Web View</span></span>
* <span data-ttu-id="12fdd-231">A leküldéses kampány létrehozásához a "Reach" szakaszban, és nevezze el a kampányt.</span><span class="sxs-lookup"><span data-stu-id="12fdd-231">Create your Push campaign in the "Reach" section and give your campaign a name.</span></span>

![WebView2][29]

* <span data-ttu-id="12fdd-233">Az üzenet, amely megjelenik majd írni az értesítésre kattinthat.</span><span class="sxs-lookup"><span data-stu-id="12fdd-233">Write the message that will appear on the notification.</span></span>
* <span data-ttu-id="12fdd-234">Jelölje ki a közlemény tartalomtípus "web"</span><span class="sxs-lookup"><span data-stu-id="12fdd-234">Select the Announcement Content Type as “web”</span></span>

![WebView3][30]

### <a name="about-announcement-types"></a><span data-ttu-id="12fdd-236">Kapcsolatos bejelentés típusok:</span><span class="sxs-lookup"><span data-stu-id="12fdd-236">About Announcement types:</span></span>
* <span data-ttu-id="12fdd-237">Csak értesítés: egy egyszerű standard szintű értesítési.</span><span class="sxs-lookup"><span data-stu-id="12fdd-237">Notification only: It is a simple standard notification.</span></span> <span data-ttu-id="12fdd-238">Azt jelenti, hogy ha a felhasználó kattint, további nézet nélkül jelenik meg, de csak a hozzá tartozó műveletet hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="12fdd-238">Meaning that if a user clicks on it, no additional view will appear, but only the action associated to it will occur.</span></span>
* <span data-ttu-id="12fdd-239">Szöveg közlemény: egy értesítés, amely kapcsolatba lép a felhasználó számára a szöveges nézet, tekintse meg a legyen.</span><span class="sxs-lookup"><span data-stu-id="12fdd-239">Text announcement: It is a notification that engages the user to have a look at a text view.</span></span>
* <span data-ttu-id="12fdd-240">Webes hirdetmény: egy értesítés, amely kapcsolatba lép a felhasználó számára, tekintse meg a következő a webes nézet.</span><span class="sxs-lookup"><span data-stu-id="12fdd-240">Web announcement: It is a notification that engages the user to have a look at a web view.</span></span>
  <span data-ttu-id="12fdd-241">Válassza ki a "Web közlemény" tartalmat.</span><span class="sxs-lookup"><span data-stu-id="12fdd-241">Select the "Web announcement" content.</span></span>

> [!NOTE]
> <span data-ttu-id="12fdd-242">Amikor a webes nézet, azt mindig tartalmaz egy értesítési először.</span><span class="sxs-lookup"><span data-stu-id="12fdd-242">When you push a web view, it always comes with a notification first.</span></span>

* <span data-ttu-id="12fdd-243">Adja meg a webes tartalom (kiválasztását követően a webes hirdetmény tartalmat, a szakasz jelenik meg, lehetővé téve adja meg a webes nézet tartalom megjeleníteni kívánt.)</span><span class="sxs-lookup"><span data-stu-id="12fdd-243">Define the web content (After having selected the web announcement content, the subsection will appear, allowing you to define the web view content you want to be displayed.)</span></span>

![WebView4][31]

* <span data-ttu-id="12fdd-245">A cím az üzenet (nem kötelező) tetején megjelenő írni.</span><span class="sxs-lookup"><span data-stu-id="12fdd-245">Write the title that will appear at the top of the message (optional).</span></span>
* <span data-ttu-id="12fdd-246">Ide írhatja a HTML kódot.</span><span class="sxs-lookup"><span data-stu-id="12fdd-246">Write your HTML code here.</span></span>
* <span data-ttu-id="12fdd-247">Kattintson a Szerkesztés módban gombra kattintva edition váltson, és megjelenését például a forrás.</span><span class="sxs-lookup"><span data-stu-id="12fdd-247">Click on the source editing mode button to switch edition and see how it looks like.</span></span>
* <span data-ttu-id="12fdd-248">Írjon a tartalmat, amely megjelenik majd az akciógombra kattinthat (Akciógomb lehetővé teszi, hogy az alkalmazás megfelelően egy adott művelet, például az alkalmazás-Áruházbeli vagy bármilyen típusú adatforrások biztosíthat egy lap megnyitása).</span><span class="sxs-lookup"><span data-stu-id="12fdd-248">Write the content that will appear on the action button (an action button enables the application to make a specific action such as opening a page of the application, redirecting to a Store or any kind of sources you can provide).</span></span>
* <span data-ttu-id="12fdd-249">Írás a tartalmat, amely megjelenik majd a Kilépés gombra (a Kilépés gombra kattintva, a webes nézet eltűnik).</span><span class="sxs-lookup"><span data-stu-id="12fdd-249">Write the content that will appear on the exit button (by clicking on the exit button, the web view will disappear).</span></span>
* <span data-ttu-id="12fdd-250">eredménye</span><span class="sxs-lookup"><span data-stu-id="12fdd-250">Result</span></span>

![WebView5][32]

* <span data-ttu-id="12fdd-252">A felhasználó az értesítést fogadó, és kattintson rá.</span><span class="sxs-lookup"><span data-stu-id="12fdd-252">The user receive the notification and click on it.</span></span>
* <span data-ttu-id="12fdd-253">A szöveges nézet jelenik meg egy előugró ablak, a felhasználó használhatja őket.</span><span class="sxs-lookup"><span data-stu-id="12fdd-253">The text view appears as a pop-up allowing the user to interact with it.</span></span>

<!--Image references-->
[1]: ./media/mobile-engagement-how-tos/First1.png
[2]: ./media/mobile-engagement-how-tos/First2.png
[3]: ./media/mobile-engagement-how-tos/First3.png
[4]: ./media/mobile-engagement-how-tos/First4.png
[5]: ./media/mobile-engagement-how-tos/First5.png
[6]: ./media/mobile-engagement-how-tos/First6.png
[7]: ./media/mobile-engagement-how-tos/First7.png
[8]: ./media/mobile-engagement-how-tos/Test1.png
[9]: ./media/mobile-engagement-how-tos/Test2.png
[10]: ./media/mobile-engagement-how-tos/Test3.png
[11]: ./media/mobile-engagement-how-tos/Personalize1.png
[12]: ./media/mobile-engagement-how-tos/Personalize2.png
[13]: ./media/mobile-engagement-how-tos/Personalize3.png
[14]: ./media/mobile-engagement-how-tos/Personalize4.png
[15]: ./media/mobile-engagement-how-tos/Differentiate1.png
[16]: ./media/mobile-engagement-how-tos/Differentiate2.png
[17]: ./media/mobile-engagement-how-tos/Differentiate3.png
[18]: ./media/mobile-engagement-how-tos/Schedule1.png
[19]: ./media/mobile-engagement-how-tos/Schedule2.png
[20]: ./media/mobile-engagement-how-tos/Schedule3.png
[21]: ./media/mobile-engagement-how-tos/TextView1.png
[22]: ./media/mobile-engagement-how-tos/TextView2.png
[23]: ./media/mobile-engagement-how-tos/TextView3.png
[24]: ./media/mobile-engagement-how-tos/TextView4.png
[25]: ./media/mobile-engagement-how-tos/TextView5.png
[26]: ./media/mobile-engagement-how-tos/TextView6.png
[27]: ./media/mobile-engagement-how-tos/TextView7.png
[28]: ./media/mobile-engagement-how-tos/WebView1.png
[29]: ./media/mobile-engagement-how-tos/WebView2.png
[30]: ./media/mobile-engagement-how-tos/WebView3.png
[31]: ./media/mobile-engagement-how-tos/WebView4.png
[32]: ./media/mobile-engagement-how-tos/WebView5.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md

