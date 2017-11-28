---
title: "aaaNotification hubok Megtörje hírek oktatóanyag – Android"
description: "Megtudhatja, hogyan toouse Azure Service Bus Notification Hubs toosend megtörje hírek értesítések tooAndroid eszközök."
services: notification-hubs
documentationcenter: android
author: ysxu
manager: erikre
editor: 
ms.assetid: 3c23cb80-9d35-4dde-b26d-a7bfd4cb8f81
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: e6eb41bec95c67d7dc059f560194966d04400494
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-notification-hubs-toosend-breaking-news"></a><span data-ttu-id="57ce1-103">Használja a Notification Hubs toosend legfrissebb hírek</span><span class="sxs-lookup"><span data-stu-id="57ce1-103">Use Notification Hubs toosend breaking news</span></span>
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a><span data-ttu-id="57ce1-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="57ce1-104">Overview</span></span>
<span data-ttu-id="57ce1-105">Ez a témakör bemutatja, hogyan toouse Azure Notification Hubs toobroadcast breaking news értesítések tooan Android-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="57ce1-105">This topic shows you how toouse Azure Notification Hubs toobroadcast breaking news notifications tooan Android app.</span></span> <span data-ttu-id="57ce1-106">Amikor végzett, akkor tudja tooregister számára megtörje hírek kategóriák érdekli, és ezen kategóriák csak leküldéses értesítések fogadásához.</span><span class="sxs-lookup"><span data-stu-id="57ce1-106">When complete, you will be able tooregister for breaking news categories you are interested in, and receive only push notifications for those categories.</span></span> <span data-ttu-id="57ce1-107">Ebben a forgatókönyvben számos alkalmazás általános felépítését, amelyben értesítések vannak küldött toobe toogroups, amely rendelkezik deklarálva érdeklődési rajtuk, például az RSS-olvasóval, az alkalmazások zene ventilátorok stb.</span><span class="sxs-lookup"><span data-stu-id="57ce1-107">This scenario is a common pattern for many apps where notifications have toobe sent toogroups of users that have previously declared interest in them, e.g. RSS reader, apps for music fans, etc.</span></span>

<span data-ttu-id="57ce1-108">Szórási forgatókönyvek engedélyezve vannak, beleértve a következőket egy vagy több *címkék* hello értesítési központ regisztráció létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="57ce1-108">Broadcast scenarios are enabled by including one or more *tags* when creating a registration in hello notification hub.</span></span> <span data-ttu-id="57ce1-109">Ha az értesítések küldése tooa címke hello címke regisztrált minden eszköz hello értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="57ce1-109">When notifications are sent tooa tag, all devices that have registered for hello tag will receive hello notification.</span></span> <span data-ttu-id="57ce1-110">Mivel a címkékkel egyszerűen csak karakterláncok, nincs kiépítve előzetes toobe.</span><span class="sxs-lookup"><span data-stu-id="57ce1-110">Because tags are simply strings, they do not have toobe provisioned in advance.</span></span> <span data-ttu-id="57ce1-111">Címkékkel kapcsolatos további információkért tekintse meg túl[Notification Hubs útválasztási és címke kifejezések](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="57ce1-111">For more information about tags, refer too[Notification Hubs Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="57ce1-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="57ce1-112">Prerequisites</span></span>
<span data-ttu-id="57ce1-113">Ebben a témakörben megfogalmazott célra épül létrehozott hello app [Ismerkedés a Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="57ce1-113">This topic builds on hello app you created in [Get started with Notification Hubs][get-started].</span></span> <span data-ttu-id="57ce1-114">Az oktatóanyag elindítása előtt már végrehajtotta [Ismerkedés a Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="57ce1-114">Before starting this tutorial, you must have already completed [Get started with Notification Hubs][get-started].</span></span>

## <a name="add-category-selection-toohello-app"></a><span data-ttu-id="57ce1-115">Kategória kiválasztása toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="57ce1-115">Add category selection toohello app</span></span>
<span data-ttu-id="57ce1-116">első lépés hello tooadd hello felhasználói felületi elemek tooyour meglévő fő tevékenységet, amelyek lehetővé teszik a hello felhasználói tooselect kategóriák tooregister.</span><span class="sxs-lookup"><span data-stu-id="57ce1-116">hello first step is tooadd hello UI elements tooyour existing main activity that enable hello user tooselect categories tooregister.</span></span> <span data-ttu-id="57ce1-117">felhasználó által kijelölt hello kategóriák hello eszközön tárolja.</span><span class="sxs-lookup"><span data-stu-id="57ce1-117">hello categories selected by a user are stored on hello device.</span></span> <span data-ttu-id="57ce1-118">Hello alkalmazás indításakor a eszközregisztráció címkeként az értesítési központ kijelölt hello kategóriákhoz jön létre.</span><span class="sxs-lookup"><span data-stu-id="57ce1-118">When hello app starts, a device registration is created in your notification hub with hello selected categories as tags.</span></span>

1. <span data-ttu-id="57ce1-119">Nyissa meg a res/layout/activity_main.xml fájlt, és helyettesítő hello következőre hello tartalom:</span><span class="sxs-lookup"><span data-stu-id="57ce1-119">Open your res/layout/activity_main.xml file, and substitute hello content with hello following:</span></span>
   
        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:paddingBottom="@dimen/activity_vertical_margin"
            android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            tools:context="com.example.breakingnews.MainActivity"
            android:orientation="vertical">
   
                <CheckBox
                    android:id="@+id/worldBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_world" />
                <CheckBox
                    android:id="@+id/politicsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_politics" />
                <CheckBox
                    android:id="@+id/businessBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_business" />
                <CheckBox
                    android:id="@+id/technologyBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_technology" />
                <CheckBox
                    android:id="@+id/scienceBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_science" />
                <CheckBox
                    android:id="@+id/sportsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_sports" />
                <Button
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:onClick="subscribe"
                    android:text="@string/button_subscribe" />
        </LinearLayout>
2. <span data-ttu-id="57ce1-120">Nyissa meg a res/values/strings.xml fájlt, és adja hozzá az alábbi hello:</span><span class="sxs-lookup"><span data-stu-id="57ce1-120">Open your res/values/strings.xml file and add hello following lines:</span></span>
   
        <string name="button_subscribe">Subscribe</string>
        <string name="label_world">World</string>
        <string name="label_politics">Politics</string>
        <string name="label_business">Business</string>
        <string name="label_technology">Technology</string>
        <string name="label_science">Science</string>
        <string name="label_sports">Sports</string>
   
    <span data-ttu-id="57ce1-121">A main_activity.xml grafikus elrendezés most példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="57ce1-121">Your main_activity.xml graphical layout should now look like this:</span></span>
   
    ![][A1]
3. <span data-ttu-id="57ce1-122">Most hozzon létre egy osztályt **értesítések** a hello azonos csomag a **MainActivity** osztály.</span><span class="sxs-lookup"><span data-stu-id="57ce1-122">Now create a class **Notifications** in hello same package as your **MainActivity** class.</span></span>
   
        import java.util.HashSet;
        import java.util.Set;
   
        import android.content.Context;
        import android.content.SharedPreferences;
        import android.os.AsyncTask;
        import android.util.Log;
        import android.widget.Toast;
   
        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.microsoft.windowsazure.messaging.NotificationHub;
   
        public class Notifications {
            private static final String PREFS_NAME = "BreakingNewsCategories";
            private GoogleCloudMessaging gcm;
            private NotificationHub hub;
            private Context context;
            private String senderId;
   
            public Notifications(Context context, String senderId, String hubName, 
                                    String listenConnectionString) {
                this.context = context;
                this.senderId = senderId;
   
                gcm = GoogleCloudMessaging.getInstance(context);
                hub = new NotificationHub(hubName, listenConnectionString, context);
            }
   
            public void storeCategoriesAndSubscribe(Set<String> categories)
            {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                settings.edit().putStringSet("categories", categories).commit();
                subscribeToCategories(categories);
            }
   
            public Set<String> retrieveCategories() {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                return settings.getStringSet("categories", new HashSet<String>());
            }
   
            public void subscribeToCategories(final Set<String> categories) {
                new AsyncTask<Object, Object, Object>() {
                    @Override
                    protected Object doInBackground(Object... params) {
                        try {
                            String regid = gcm.register(senderId);
   
                            String templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";
   
                            hub.registerTemplate(regid,"simpleGCMTemplate", templateBodyGCM, 
                                categories.toArray(new String[categories.size()]));
                        } catch (Exception e) {
                            Log.e("MainActivity", "Failed tooregister - " + e.getMessage());
                            return e;
                        }
                        return null;
                    }
   
                    protected void onPostExecute(Object result) {
                        String message = "Subscribed for categories: "
                                + categories.toString();
                        Toast.makeText(context, message,
                                Toast.LENGTH_LONG).show();
                    }
                }.execute(null, null, null);
            }
   
        }
   
    <span data-ttu-id="57ce1-123">Ez az osztály hello helyi tároló toostore hello kategóriáinak híreket, hogy az eszköz rendelkezik-e tooreceive használja.</span><span class="sxs-lookup"><span data-stu-id="57ce1-123">This class uses hello local storage toostore hello categories of news that this device has tooreceive.</span></span> <span data-ttu-id="57ce1-124">Az ezen kategóriák módszerek tooregister is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="57ce1-124">It also contains methods tooregister for these categories.</span></span>
4. <span data-ttu-id="57ce1-125">Az a **MainActivity** osztály, távolítsa el a privát mezőinek **NotificationHub** és **GoogleCloudMessaging**, és a mező felvétele **értesítések**:</span><span class="sxs-lookup"><span data-stu-id="57ce1-125">In your **MainActivity** class remove your private fields for **NotificationHub** and **GoogleCloudMessaging**, and add a field for **Notifications**:</span></span>
   
        // private GoogleCloudMessaging gcm;
        // private NotificationHub hub;
        private Notifications notifications;
5. <span data-ttu-id="57ce1-126">Ezt követően a hello **onCreate** módszer, eltávolítás hello inicializálása hello **hub** mező és hello **registerWithNotificationHubs** metódust.</span><span class="sxs-lookup"><span data-stu-id="57ce1-126">Then, in hello **onCreate** method, remove hello initialization of hello **hub** field and hello **registerWithNotificationHubs** method.</span></span> <span data-ttu-id="57ce1-127">Majd adja hozzá a sort, amelyben a hello példányának inicializálása a következő hello **értesítések** osztály.</span><span class="sxs-lookup"><span data-stu-id="57ce1-127">Then add hello following lines which initialize an instance of hello **Notifications** class.</span></span> 

        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            MyHandler.mainActivity = this;

            NotificationsManager.handleNotifications(this, SENDER_ID,
                    MyHandler.class);

            notifications = new Notifications(this, SENDER_ID, HubName, HubListenConnectionString);

            notifications.subscribeToCategories(notifications.retrieveCategories());
        }

    <span data-ttu-id="57ce1-128">`HubName`és `HubListenConnectionString` már kell beállítani a hello `<hub name>` és `<connection string with listen access>` helyőrzőket az értesítési központ nevét és hello kapcsolati karakterláncot *DefaultListenSharedAccessSignature* beszerzett korábban.</span><span class="sxs-lookup"><span data-stu-id="57ce1-128">`HubName` and `HubListenConnectionString` should already be set with hello `<hub name>` and `<connection string with listen access>` placeholders with your notification hub name and hello connection string for *DefaultListenSharedAccessSignature* that you obtained earlier.</span></span>

    > [AZURE.NOTE] <span data-ttu-id="57ce1-129">Ügyfél alkalmazáshoz elosztott hitelesítő adatok nem általában biztonságos, mert csak kell terjeszteni figyelési hozzáférési kulcs hello ügyfél alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="57ce1-129">Because credentials that are distributed with a client app are not generally secure, you should only distribute hello key for listen access with your client app.</span></span> <span data-ttu-id="57ce1-130">Figyeljen hozzáférés lehetővé teszi, hogy az alkalmazás tooregister az értesítéseket, de a meglévő regisztrációját nem lehet módosítani, és értesítések nem küldhető el.</span><span class="sxs-lookup"><span data-stu-id="57ce1-130">Listen access enables your app tooregister for notifications, but existing registrations cannot be modified and notifications cannot be sent.</span></span> <span data-ttu-id="57ce1-131">hello teljes körű hozzáférési kulcsot használnak a következő biztonságos háttérszolgáltatás értesítések küldését, és meglévő regisztrációk módosítása.</span><span class="sxs-lookup"><span data-stu-id="57ce1-131">hello full access key is used in a secured backend service for sending notifications and changing existing registrations.</span></span>


1. <span data-ttu-id="57ce1-132">Adja hozzá a következő hello importálja és `subscribe` metódus toohandle hello előfizetés gomb eseményt:</span><span class="sxs-lookup"><span data-stu-id="57ce1-132">Then, add hello following imports and `subscribe` method toohandle hello subscribe button click event:</span></span>
   
        import android.widget.CheckBox;
        import java.util.HashSet;
        import java.util.Set;
   
        public void subscribe(View sender) {
            final Set<String> categories = new HashSet<String>();
   
            CheckBox world = (CheckBox) findViewById(R.id.worldBox);
            if (world.isChecked())
                categories.add("world");
            CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
            if (politics.isChecked())
                categories.add("politics");
            CheckBox business = (CheckBox) findViewById(R.id.businessBox);
            if (business.isChecked())
                categories.add("business");
            CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
            if (technology.isChecked())
                categories.add("technology");
            CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
            if (science.isChecked())
                categories.add("science");
            CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
            if (sports.isChecked())
                categories.add("sports");
   
            notifications.storeCategoriesAndSubscribe(categories);
        }
   
    <span data-ttu-id="57ce1-133">Ezzel a módszerrel hoz létre a kategóriák és a hello listáját **értesítések** osztály toostore hello lista hello helyi tárolóban, és regisztrálja a hello megfelelő címkéket az értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="57ce1-133">This method creates a list of categories and uses hello **Notifications** class toostore hello list in hello local storage and register hello corresponding tags with your notification hub.</span></span> <span data-ttu-id="57ce1-134">Kategóriák megváltozásakor hello regisztrációs hello új kategóriák újra létrejön.</span><span class="sxs-lookup"><span data-stu-id="57ce1-134">When categories are changed, hello registration is recreated with hello new categories.</span></span>

<span data-ttu-id="57ce1-135">Az alkalmazás most már tudja toostore kategóriák készlete hello eszközön helyi tárolóban és regisztrálása az értesítési központban hello hello felhasználót módosítások hello kategóriák kiválasztását.</span><span class="sxs-lookup"><span data-stu-id="57ce1-135">Your app is now able toostore a set of categories in local storage on hello device and register with hello notification hub whenever hello user changes hello selection of categories.</span></span>

## <a name="register-for-notifications"></a><span data-ttu-id="57ce1-136">Az értesítések regisztrálása</span><span class="sxs-lookup"><span data-stu-id="57ce1-136">Register for notifications</span></span>
<span data-ttu-id="57ce1-137">Ezeket a lépéseket hello értesítési központ használatával a helyi tárolóban tárolt hello kategóriák indításkor regisztrálja.</span><span class="sxs-lookup"><span data-stu-id="57ce1-137">These steps register with hello notification hub on startup using hello categories that have been stored in local storage.</span></span>

> [!NOTE]
> <span data-ttu-id="57ce1-138">Hello registrationId által Google Cloud Messaging (GCM) hozzárendelt bármikor módosíthatja, mert regisztrálnia kell az értesítésekhez gyakran tooavoid értesítés sikertelen.</span><span class="sxs-lookup"><span data-stu-id="57ce1-138">Because hello registrationId assigned by Google Cloud Messaging (GCM) can change at any time, you should register for notifications frequently tooavoid notification failures.</span></span> <span data-ttu-id="57ce1-139">Ebben a példában regisztrál az értesítési hello alkalmazás minden indításakor.</span><span class="sxs-lookup"><span data-stu-id="57ce1-139">This example registers for notification every time that hello app starts.</span></span> <span data-ttu-id="57ce1-140">Gyakran futtatott alkalmazások esetén naponta csak egyszer, valószínűleg kihagyhatja regisztrációs toopreserve sávszélesség Ha kevesebb mint egy nappal hello előző regisztráció óta eltelt.</span><span class="sxs-lookup"><span data-stu-id="57ce1-140">For apps that are run frequently, more than once a day, you can probably skip registration toopreserve bandwidth if less than a day has passed since hello previous registration.</span></span>
> 
> 

1. <span data-ttu-id="57ce1-141">Adja hozzá a következő kód hello hello végén hello **onCreate** metódus a hello **MainActivity** osztály:</span><span class="sxs-lookup"><span data-stu-id="57ce1-141">Add hello following code at hello end of hello **onCreate** method in hello **MainActivity** class:</span></span>
   
        notifications.subscribeToCategories(notifications.retrieveCategories());
   
    <span data-ttu-id="57ce1-142">Ez biztosítja, hogy hello alkalmazás minden indításakor azt hello kategóriák átmásolja a helyi tárolóhoz, és kéri a regisztrálás ezen kategóriák.</span><span class="sxs-lookup"><span data-stu-id="57ce1-142">This makes sure that every time hello app starts it retrieves hello categories from local storage and requests a registeration for these categories.</span></span> 
2. <span data-ttu-id="57ce1-143">Ezután frissítse a hello `onStart()` hello metódusában `MainActivity` osztály az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="57ce1-143">Then update hello `onStart()` method of hello `MainActivity` class as follows:</span></span>
   
    <span data-ttu-id="57ce1-144">@Overridevédett "void" onStart() {</span><span class="sxs-lookup"><span data-stu-id="57ce1-144">@Override  protected void onStart() {</span></span>
   
        super.onStart();
        isVisible = true;
   
        Set<String> categories = notifications.retrieveCategories();
   
        CheckBox world = (CheckBox) findViewById(R.id.worldBox);
        world.setChecked(categories.contains("world"));
        CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
        politics.setChecked(categories.contains("politics"));
        CheckBox business = (CheckBox) findViewById(R.id.businessBox);
        business.setChecked(categories.contains("business"));
        CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
        technology.setChecked(categories.contains("technology"));
        CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
        science.setChecked(categories.contains("science"));
        CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
        sports.setChecked(categories.contains("sports"));
    <span data-ttu-id="57ce1-145">}</span><span class="sxs-lookup"><span data-stu-id="57ce1-145">}</span></span>
   
    <span data-ttu-id="57ce1-146">Ekkor frissül hello fő tevékenységnél a korábban mentett kategóriák hello állapota alapján.</span><span class="sxs-lookup"><span data-stu-id="57ce1-146">This updates hello main activity based on hello status of previously saved categories.</span></span>

<span data-ttu-id="57ce1-147">hello alkalmazás kész, és képes tárolni a kategóriák hello eszköz használt helyi tárhely tooregister hello értesítési központban az amikor hello felhasználót módosítások hello kategóriák kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="57ce1-147">hello app is now complete and can store a set of categories in hello device local storage used tooregister with hello notification hub whenever hello user changes hello selection of categories.</span></span> <span data-ttu-id="57ce1-148">A következő kategória értesítések toothis app küldő háttérkiszolgálón meghatározzák azt.</span><span class="sxs-lookup"><span data-stu-id="57ce1-148">Next, we will define a backend that can send category notifications toothis app.</span></span>

## <a name="sending-tagged-notifications"></a><span data-ttu-id="57ce1-149">Címkézett értesítések küldése</span><span class="sxs-lookup"><span data-stu-id="57ce1-149">Sending tagged notifications</span></span>
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-hello-app-and-generate-notifications"></a><span data-ttu-id="57ce1-150">Hello alkalmazás futtatását, és értesítések</span><span class="sxs-lookup"><span data-stu-id="57ce1-150">Run hello app and generate notifications</span></span>
1. <span data-ttu-id="57ce1-151">Az Android Studióban hello alkalmazás elkészítésére, és indítsa el a egy eszközt vagy emulátort.</span><span class="sxs-lookup"><span data-stu-id="57ce1-151">In Android Studio, build hello app and start it on a device or emulator.</span></span>
   
    <span data-ttu-id="57ce1-152">Megjegyzés: a felhasználói felület számos hello alkalmazást, amely váltja kiválaszthatja hello kategóriák toosubscribe számára.</span><span class="sxs-lookup"><span data-stu-id="57ce1-152">Note that hello app UI provides a set of toggles that lets you choose hello categories toosubscribe to.</span></span>
2. <span data-ttu-id="57ce1-153">Egy vagy több kategóriák váltógombok engedélyezése, majd kattintson az **előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="57ce1-153">Enable one or more categories toggles, then click **Subscribe**.</span></span>
   
    <span data-ttu-id="57ce1-154">hello app hello kiválasztott kategóriák alakítja címkék, és egy új eszközök regisztrációja kijelölt hello címkék hello értesítési központ érkező kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="57ce1-154">hello app converts hello selected categories into tags and requests a new device registration for hello selected tags from hello notification hub.</span></span> <span data-ttu-id="57ce1-155">hello regisztrált kategóriák adott vissza, és egy bejelentési értesítés jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="57ce1-155">hello registered categories are returned and displayed in a toast notification.</span></span>
3. <span data-ttu-id="57ce1-156">Egy új értesítés küldése hello .NET Konzolalkalmazás futtatásával.</span><span class="sxs-lookup"><span data-stu-id="57ce1-156">Send a new notification by running hello .NET Console app.</span></span>  <span data-ttu-id="57ce1-157">Másik lehetőségként az értesítési központ hibakeresési lapján hello használatát hello címkézett sablon értesítéseket küldhet [klasszikus Azure portál].</span><span class="sxs-lookup"><span data-stu-id="57ce1-157">Alternatively, you can send tagged template notifications using hello debug tab of your notification hub in hello [Azure Classic Portal].</span></span>
   
    <span data-ttu-id="57ce1-158">Értesítések a kiválasztott hello kategóriák bejelentési értesítések jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="57ce1-158">Notifications for hello selected categories appear as toast notifications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="57ce1-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="57ce1-159">Next steps</span></span>
<span data-ttu-id="57ce1-160">Ez az oktatóanyag azt megtanulta, hogyan toobroadcast legfrissebb hírek kategória szerint.</span><span class="sxs-lookup"><span data-stu-id="57ce1-160">In this tutorial we learned how toobroadcast breaking news by category.</span></span> <span data-ttu-id="57ce1-161">Vegye figyelembe, hogy befejezése hello oktatóprogramot kínál, amelyek más speciális Notification Hubs forgatókönyvek jelölje ki a következő egyikét:</span><span class="sxs-lookup"><span data-stu-id="57ce1-161">Consider completing one of hello following tutorials that highlight other advanced Notification Hubs scenarios:</span></span>

* <span data-ttu-id="57ce1-162">[Használjon honosított toobroadcast Notification Hubs – legfrissebb hírek]</span><span class="sxs-lookup"><span data-stu-id="57ce1-162">[Use Notification Hubs toobroadcast localized breaking news]</span></span>
  
    <span data-ttu-id="57ce1-163">Ismerje meg, hogyan megtörje hírek app tooenable küldése tooexpand hello honosított értesítések.</span><span class="sxs-lookup"><span data-stu-id="57ce1-163">Learn how tooexpand hello breaking news app tooenable sending localized notifications.</span></span>

<!-- Images. -->
[A1]: ./media/notification-hubs-aspnet-backend-android-breaking-news/android-breaking-news1.PNG

<!-- URLs.-->
[get-started]: notification-hubs-android-push-notification-google-gcm-get-started.md
[Használjon honosított toobroadcast Notification Hubs – legfrissebb hírek]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-toofor Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Klasszikus Azure portál]: https://manage.windowsazure.com
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
