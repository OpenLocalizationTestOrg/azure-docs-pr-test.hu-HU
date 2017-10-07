---
title: "aaaSending biztonságos leküldéses értesítések küldése az Azure Notification hubs használatával"
description: "Tudnivalók a biztonságos toosend a leküldéses értesítések tooan Android-alkalmazás az Azure-ból. Kódminták Java és a C#."
documentationcenter: android
keywords: "leküldéses értesítési, leküldéses értesítések leküldéses üzenetek, android leküldéses értesítések"
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: daf3de1c-f6a9-43c4-8165-a76bfaa70893
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: d07943c4691ed07acb987086228ef565e6281d57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sending-secure-push-notifications-with-azure-notification-hubs"></a><span data-ttu-id="e709c-105">Biztonságos leküldéses értesítések küldése az Azure Notification hubs használatával</span><span class="sxs-lookup"><span data-stu-id="e709c-105">Sending Secure Push Notifications with Azure Notification Hubs</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e709c-106">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="e709c-106">Windows Universal</span></span>](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [<span data-ttu-id="e709c-107">iOS</span><span class="sxs-lookup"><span data-stu-id="e709c-107">iOS</span></span>](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [<span data-ttu-id="e709c-108">Android</span><span class="sxs-lookup"><span data-stu-id="e709c-108">Android</span></span>](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="e709c-109">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="e709c-109">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="e709c-110">toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="e709c-110">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="e709c-111">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="e709c-111">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="e709c-112">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="e709c-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).</span></span>
> 
> 

<span data-ttu-id="e709c-113">Leküldéses értesítési támogatása a Microsoft Azure lehetővé teszi tooaccess egy könnyen használható, többplatformos, kibővített leküldéses üzenet infrastruktúrában, ami jelentősen egyszerűbb a leküldéses értesítések az ügyfél és a vállalati alkalmazások hello végrehajtása mobil platformokon.</span><span class="sxs-lookup"><span data-stu-id="e709c-113">Push notification support in Microsoft Azure enables you tooaccess an easy-to-use, multiplatform, scaled-out push message infrastructure, which greatly simplifies hello implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span>

<span data-ttu-id="e709c-114">Tooregulatory vagy biztonsági korlátozások miatt néha egy alkalmazás érdemes tooinclude valamit a hello szabványos leküldéses értesítési infrastruktúrát keresztül nem továbbítható hello értesítést.</span><span class="sxs-lookup"><span data-stu-id="e709c-114">Due tooregulatory or security constraints, sometimes an application might want tooinclude something in hello notification that cannot be transmitted through hello standard push notification infrastructure.</span></span> <span data-ttu-id="e709c-115">Ez az oktatóanyag leírja, hogyan tooachieve hello úgy, hogy a bizalmas adatokat hello Android ügyféleszköz- és hello háttéralkalmazás közötti biztonságos és hitelesített kapcsolaton keresztül küld ugyanazt a felhasználói élményt.</span><span class="sxs-lookup"><span data-stu-id="e709c-115">This tutorial describes how tooachieve hello same experience by sending sensitive information through a secure, authenticated connection between hello client Android device and hello app backend.</span></span>

<span data-ttu-id="e709c-116">Magas szinten hello folyamat a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="e709c-116">At a high level, hello flow is as follows:</span></span>

1. <span data-ttu-id="e709c-117">hello app háttér:</span><span class="sxs-lookup"><span data-stu-id="e709c-117">hello app back-end:</span></span>
   * <span data-ttu-id="e709c-118">Háttér-adatbázisban tárolja biztonságos hasznos.</span><span class="sxs-lookup"><span data-stu-id="e709c-118">Stores secure payload in back-end database.</span></span>
   * <span data-ttu-id="e709c-119">Küldi hello Azonosítóját az értesítési toohello Android-eszköz (nem biztonságos információk küldése).</span><span class="sxs-lookup"><span data-stu-id="e709c-119">Sends hello ID of this notification toohello Android device (no secure information is sent).</span></span>
2. <span data-ttu-id="e709c-120">hello alkalmazást hello eszközön, hello értesítés fogadása közben:</span><span class="sxs-lookup"><span data-stu-id="e709c-120">hello app on hello device, when receiving hello notification:</span></span>
   * <span data-ttu-id="e709c-121">hello Android-eszköz kapcsolatot létesít a hello háttér-kérelmező hello biztonságos hasznos.</span><span class="sxs-lookup"><span data-stu-id="e709c-121">hello Android device contacts hello back-end requesting hello secure payload.</span></span>
   * <span data-ttu-id="e709c-122">hello hasznos mutatja az hello app értesítésként hello eszközön.</span><span class="sxs-lookup"><span data-stu-id="e709c-122">hello app can show hello payload as a notification on hello device.</span></span>

<span data-ttu-id="e709c-123">Fontos, hogy megelőző folyamata hello (és az oktatóanyag) feltételezzük, hogy hello eszköz toonote tárol egy hitelesítési jogkivonatot helyi tárhelyre – hello felhasználó bejelentkezése után.</span><span class="sxs-lookup"><span data-stu-id="e709c-123">It is important toonote that in hello preceding flow (and in this tutorial), we assume that hello device stores an authentication token in local storage, after hello user logs in.</span></span> <span data-ttu-id="e709c-124">Ez biztosítja, hogy teljesen zökkenőmentes élményt, mivel hello eszköz hello értesítési biztonságos hasznos a token használatával kérheti le.</span><span class="sxs-lookup"><span data-stu-id="e709c-124">This guarantees a completely seamless experience, as hello device can retrieve hello notification’s secure payload using this token.</span></span> <span data-ttu-id="e709c-125">Ha az alkalmazás nem tárolja a hitelesítési tokenek hello eszköz, vagy ezeket a jogkivonatokat is járhatott, hello eszközalkalmazás hello leküldéses értesítés fogadásakor megjelenjen-e arra kéri a hello felhasználói toolaunch hello alkalmazás általános értesítést.</span><span class="sxs-lookup"><span data-stu-id="e709c-125">If your application does not store authentication tokens on hello device, or if these tokens can be expired, hello device app, upon receiving hello push notification should display a generic notification prompting hello user toolaunch hello app.</span></span> <span data-ttu-id="e709c-126">hello app majd hello felhasználó hitelesíti, és hello értesítési tartalom jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="e709c-126">hello app then authenticates hello user and shows hello notification payload.</span></span>

<span data-ttu-id="e709c-127">Ez az oktatóanyag bemutatja, hogyan biztonságos toosend leküldéses értesítések.</span><span class="sxs-lookup"><span data-stu-id="e709c-127">This tutorial shows how toosend secure push notifications.</span></span> <span data-ttu-id="e709c-128">-Buildekről nyújtanak a hello [felhasználók értesítése](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) oktatóanyag, ezért el kell végeznie hello lépéseket, hogy az oktatóanyagban először Ha még nem tette meg.</span><span class="sxs-lookup"><span data-stu-id="e709c-128">It builds on hello [Notify Users](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) tutorial, so you should complete hello steps in that tutorial first if you haven't already.</span></span>

> [!NOTE]
> <span data-ttu-id="e709c-129">Ez az oktatóanyag feltételezi, hogy létrehozta és leírtak szerint konfigurálta az értesítési központ [Ismerkedés a Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e709c-129">This tutorial assumes that you have created and configured your notification hub as described in [Getting Started with Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).</span></span>
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-hello-android-project"></a><span data-ttu-id="e709c-130">Android-projekt hello módosítása</span><span class="sxs-lookup"><span data-stu-id="e709c-130">Modify hello Android project</span></span>
<span data-ttu-id="e709c-131">Most, hogy az alkalmazás háttér-toosend csak hello módosított *azonosító* leküldéses értesítés, hogy toochange az Android-alkalmazás toohandle, értesítések és a visszahívás a háttér-tooretrieve hello megjelenített üzenet toobe biztonságos.</span><span class="sxs-lookup"><span data-stu-id="e709c-131">Now that you modified your app back-end toosend just hello *id* of a push notification, you have toochange your Android app toohandle that notification and call back your back-end tooretrieve hello secure message toobe displayed.</span></span>
<span data-ttu-id="e709c-132">tooachieve ezen cél esetében van toomake meg arról, hogy az Android-alkalmazás tudja, hogy hogyan a háttér-hello leküldéses értesítés fogadásakor a saját magát tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="e709c-132">tooachieve this goal, you have toomake sure that your Android app knows how tooauthenticate itself with your back-end when it receives hello push notifications.</span></span>

<span data-ttu-id="e709c-133">Most módosítja, a Microsoft hello *bejelentkezési* folyamata a rendelés toosave hello hitelesítési fejléc értéke hello megosztott az alkalmazás beállításait.</span><span class="sxs-lookup"><span data-stu-id="e709c-133">We will now modify hello *login* flow in order toosave hello authentication header value in hello shared preferences of your app.</span></span> <span data-ttu-id="e709c-134">Hasonló mechanizmusok használt toostore lehet bármely hitelesítési jogkivonat (pl. OAuth jogkivonatok), amely az alkalmazás hello lesz toouse anélkül, hogy a felhasználói hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="e709c-134">Analogous mechanisms can be used toostore any authentication token (e.g. OAuth tokens) that hello app will have toouse without requiring user credentials.</span></span>

1. <span data-ttu-id="e709c-135">Az Android-alkalmazás projektben adja hozzá a következő állandókat hello hello tetején hello **MainActivity** osztály:</span><span class="sxs-lookup"><span data-stu-id="e709c-135">In your Android app project, add hello following constants at hello top of hello **MainActivity** class:</span></span>
   
        public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
        public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";
2. <span data-ttu-id="e709c-136">Még tart a hello **MainActivity** osztály, a frissítés hello `getAuthorizationHeader()` metódus toocontain hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="e709c-136">Still in hello **MainActivity** class, update hello `getAuthorizationHeader()` method toocontain hello following code:</span></span>
   
        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);
   
            SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();
   
            return basicAuthHeader;
        }
3. <span data-ttu-id="e709c-137">Adja hozzá a következő hello `import` hello hello tetején utasítások **MainActivity** fájlt:</span><span class="sxs-lookup"><span data-stu-id="e709c-137">Add hello following `import` statements at hello top of hello **MainActivity** file:</span></span>
   
        import android.content.SharedPreferences;

<span data-ttu-id="e709c-138">Most azt fogja módosítani hello kezelő is nevezett hello értesítés fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="e709c-138">Now we will change hello handler that is called when hello notification is received.</span></span>

1. <span data-ttu-id="e709c-139">A hello **MyHandler** osztály módosítása hello `OnReceive()` metódus toocontain:</span><span class="sxs-lookup"><span data-stu-id="e709c-139">In hello **MyHandler** class change hello `OnReceive()` method toocontain:</span></span>
   
        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String secureMessageId = bundle.getString("secureId");
            retrieveNotification(secureMessageId);
        }
2. <span data-ttu-id="e709c-140">Majd adja hozzá a hello `retrieveNotification()` metódus hello helyőrző cseréje `{back-end endpoint}` hello háttér-végponthoz, a háttér-másikban kapott:</span><span class="sxs-lookup"><span data-stu-id="e709c-140">Then add hello `retrieveNotification()` method, replacing hello placeholder `{back-end endpoint}` with hello back-end endpoint obtained while deploying your back-end:</span></span>
   
        private void retrieveNotification(final String secureMessageId) {
            SharedPreferences sp = ctx.getSharedPreferences(MainActivity.NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            final String authorizationHeader = sp.getString(MainActivity.AUTHORIZATION_HEADER_PROPERTY, null);
   
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        HttpUriRequest request = new HttpGet("{back-end endpoint}/api/notifications/"+secureMessageId);
                        request.addHeader("Authorization", "Basic "+authorizationHeader);
                        HttpResponse response = new DefaultHttpClient().execute(request);
                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            Log.e("MainActivity", "Error retrieving secure notification" + response.getStatusLine().getStatusCode());
                            throw new RuntimeException("Error retrieving secure notification");
                        }
                        String secureNotificationJSON = EntityUtils.toString(response.getEntity());
                        JSONObject secureNotification = new JSONObject(secureNotificationJSON);
                        sendNotification(secureNotification.getString("Payload"));
                    } catch (Exception e) {
                        Log.e("MainActivity", "Failed tooretrieve secure notification - " + e.getMessage());
                        return e;
                    }
                    return null;
                }
            }.execute(null, null, null);
        }

<span data-ttu-id="e709c-141">Ez a metódus meghívja az alkalmazás háttér-tooretrieve hello értesítés hello tárolt hello hitelesítő adatok használatával megosztott beállítások, és megjeleníti azt a normál értesítésként tartalom.</span><span class="sxs-lookup"><span data-stu-id="e709c-141">This method calls your app back-end tooretrieve hello notification content using hello credentials stored in hello shared preferences and displays it as a normal notification.</span></span> <span data-ttu-id="e709c-142">hello értesítési jelek toohello alkalmazás felhasználói akárcsak bármely más leküldéses értesítést.</span><span class="sxs-lookup"><span data-stu-id="e709c-142">hello notification looks toohello app user exactly like any other push notification.</span></span>

<span data-ttu-id="e709c-143">Ne feledje, hogy hiányzó hitelesítési fejléc tulajdonság vagy elutasíthatják hello háttér-érdemes toohandle hello esetekben.</span><span class="sxs-lookup"><span data-stu-id="e709c-143">Note that it is preferable toohandle hello cases of missing authentication header property or rejection by hello back-end.</span></span> <span data-ttu-id="e709c-144">Ezekben az esetekben hello adott kezelésének legtöbbször a cél felhasználói élmény függ.</span><span class="sxs-lookup"><span data-stu-id="e709c-144">hello specific handling of these cases depend mostly on your target user experience.</span></span> <span data-ttu-id="e709c-145">Egy elem toodisplay hello tooauthenticate tooretrieve hello tényleges Felhasználóértesítés általános kérése az értesítést.</span><span class="sxs-lookup"><span data-stu-id="e709c-145">One option is toodisplay a notification with a generic prompt for hello user tooauthenticate tooretrieve hello actual notification.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="e709c-146">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="e709c-146">Run hello Application</span></span>
<span data-ttu-id="e709c-147">toorun hello alkalmazás, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="e709c-147">toorun hello application, do hello following:</span></span>

1. <span data-ttu-id="e709c-148">Győződjön meg arról, hogy **AppBackend** a rendszer az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="e709c-148">Make sure **AppBackend** is deployed in Azure.</span></span> <span data-ttu-id="e709c-149">Ha a Visual Studio használatával, futtassa a hello **AppBackend** webes API-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e709c-149">If using Visual Studio, run hello **AppBackend** Web API application.</span></span> <span data-ttu-id="e709c-150">Az ASP.NET-weblap jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="e709c-150">An ASP.NET web page is displayed.</span></span>
2. <span data-ttu-id="e709c-151">Az eclipse-ben egy fizikai Android eszköz vagy hello emulátor hello alkalmazást futtatnak.</span><span class="sxs-lookup"><span data-stu-id="e709c-151">In Eclipse, run hello app on a physical Android device or hello emulator.</span></span>
3. <span data-ttu-id="e709c-152">A hello Android-alkalmazás felhasználói felületén, írja be a felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="e709c-152">In hello Android app UI, enter a username and password.</span></span> <span data-ttu-id="e709c-153">Ezek karakterlánc lehet, de kell hello ugyanazt az értéket.</span><span class="sxs-lookup"><span data-stu-id="e709c-153">These can be any string, but they must be hello same value.</span></span>
4. <span data-ttu-id="e709c-154">Hello Android-alkalmazás felhasználói felületén, kattintson **jelentkezzen be**.</span><span class="sxs-lookup"><span data-stu-id="e709c-154">In hello Android app UI, click **Log in**.</span></span> <span data-ttu-id="e709c-155">Kattintson a **leküldéses küldése**.</span><span class="sxs-lookup"><span data-stu-id="e709c-155">Then click **Send push**.</span></span>

