---
title: "Notification Hubs – legfrissebb hírek oktatóanyag - Android"
description: "Útmutató: Azure Service Bus Notification Hubs használatával legfrissebb híreket tartalmazó értesítések küldése Android-eszközök."
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
ms.openlocfilehash: 76ec01c874fceedab7d76b2ef58e4b45b5489f58
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-notification-hubs-to-send-breaking-news"></a><span data-ttu-id="5bfe7-103">A legfrissebb hírek elküldése a Notification Hubs használatával</span><span class="sxs-lookup"><span data-stu-id="5bfe7-103">Use Notification Hubs to send breaking news</span></span>
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a><span data-ttu-id="5bfe7-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="5bfe7-104">Overview</span></span>
<span data-ttu-id="5bfe7-105">Ez a témakör bemutatja, hogyan szórási legfrissebb híreket tartalmazó értesítések Android-alkalmazás az Azure Notification Hubs használatával.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-105">This topic shows you how to use Azure Notification Hubs to broadcast breaking news notifications to an Android app.</span></span> <span data-ttu-id="5bfe7-106">Amikor végzett, akkor fog számára megtörje érdekli hírek kategóriák regisztrálni, és ezen kategóriák csak leküldéses értesítések fogadásához.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-106">When complete, you will be able to register for breaking news categories you are interested in, and receive only push notifications for those categories.</span></span> <span data-ttu-id="5bfe7-107">Ebben a forgatókönyvben számos alkalmazás általános felépítését, ahol értesítések kell őket, például az RSS-olvasóval, az alkalmazások zene ventilátorok stb érdeklődik elemnek már deklarálva felhasználói csoportokat kell küldeni.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-107">This scenario is a common pattern for many apps where notifications have to be sent to groups of users that have previously declared interest in them, e.g. RSS reader, apps for music fans, etc.</span></span>

<span data-ttu-id="5bfe7-108">Szórási forgatókönyvek engedélyezve vannak, beleértve a következőket egy vagy több *címkék* regisztráció létrehozásakor az értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-108">Broadcast scenarios are enabled by including one or more *tags* when creating a registration in the notification hub.</span></span> <span data-ttu-id="5bfe7-109">Amikor a rendszer értesítéseket küld egy címkét, akkor a címke regisztrált minden eszköz a értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-109">When notifications are sent to a tag, all devices that have registered for the tag will receive the notification.</span></span> <span data-ttu-id="5bfe7-110">Mivel a címkékkel egyszerűen csak karakterláncok, nem rendelkeznek előre kell építeni.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-110">Because tags are simply strings, they do not have to be provisioned in advance.</span></span> <span data-ttu-id="5bfe7-111">Címkékkel kapcsolatos további információkért tekintse meg [Notification Hubs útválasztási és címke kifejezések](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="5bfe7-111">For more information about tags, refer to [Notification Hubs Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5bfe7-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5bfe7-112">Prerequisites</span></span>
<span data-ttu-id="5bfe7-113">Ez a témakör a létrehozott alkalmazás épül [Ismerkedés a Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="5bfe7-113">This topic builds on the app you created in [Get started with Notification Hubs][get-started].</span></span> <span data-ttu-id="5bfe7-114">Az oktatóanyag elindítása előtt már végrehajtotta [Ismerkedés a Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="5bfe7-114">Before starting this tutorial, you must have already completed [Get started with Notification Hubs][get-started].</span></span>

## <a name="add-category-selection-to-the-app"></a><span data-ttu-id="5bfe7-115">Kategória kiválasztása hozzáadása az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="5bfe7-115">Add category selection to the app</span></span>
<span data-ttu-id="5bfe7-116">Az első lépés a felhasználói felületi elemek hozzáadása a meglévő fő tevékenységeket, amelyek lehetővé teszik a felhasználó számára a kategóriák regisztrálni kívánt.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-116">The first step is to add the UI elements to your existing main activity that enable the user to select categories to register.</span></span> <span data-ttu-id="5bfe7-117">A felhasználó által kiválasztott kategóriák tárolódnak az eszközön.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-117">The categories selected by a user are stored on the device.</span></span> <span data-ttu-id="5bfe7-118">Az alkalmazás indításakor a eszközregisztráció címkeként az értesítési központ, a kiválasztott kategóriákra jön létre.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-118">When the app starts, a device registration is created in your notification hub with the selected categories as tags.</span></span>

1. <span data-ttu-id="5bfe7-119">Nyissa meg a res/layout/activity_main.xml fájlt, és helyettesítse be a tartalmat a következő:</span><span class="sxs-lookup"><span data-stu-id="5bfe7-119">Open your res/layout/activity_main.xml file, and substitute the content with the following:</span></span>
   
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
2. <span data-ttu-id="5bfe7-120">Nyissa meg a res/values/strings.xml fájlt, és adja hozzá a következő sorokat:</span><span class="sxs-lookup"><span data-stu-id="5bfe7-120">Open your res/values/strings.xml file and add the following lines:</span></span>
   
        <string name="button_subscribe">Subscribe</string>
        <string name="label_world">World</string>
        <string name="label_politics">Politics</string>
        <string name="label_business">Business</string>
        <string name="label_technology">Technology</string>
        <string name="label_science">Science</string>
        <string name="label_sports">Sports</string>
   
    <span data-ttu-id="5bfe7-121">A main_activity.xml grafikus elrendezés most példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="5bfe7-121">Your main_activity.xml graphical layout should now look like this:</span></span>
   
    ![][A1]
3. <span data-ttu-id="5bfe7-122">Most hozzon létre egy osztályt **értesítések** azonos csomagban található a **MainActivity** osztály.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-122">Now create a class **Notifications** in the same package as your **MainActivity** class.</span></span>
   
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
                            Log.e("MainActivity", "Failed to register - " + e.getMessage());
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
   
    <span data-ttu-id="5bfe7-123">Ez az osztály a helyi tároló, amely az eszköz rendelkezik fogadásához hírek kategóriáinak tárolására használja.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-123">This class uses the local storage to store the categories of news that this device has to receive.</span></span> <span data-ttu-id="5bfe7-124">Ezen kategóriák regisztrálásához módszerek is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-124">It also contains methods to register for these categories.</span></span>
4. <span data-ttu-id="5bfe7-125">Az a **MainActivity** osztály, távolítsa el a privát mezőinek **NotificationHub** és **GoogleCloudMessaging**, és a mező felvétele **értesítések**:</span><span class="sxs-lookup"><span data-stu-id="5bfe7-125">In your **MainActivity** class remove your private fields for **NotificationHub** and **GoogleCloudMessaging**, and add a field for **Notifications**:</span></span>
   
        // private GoogleCloudMessaging gcm;
        // private NotificationHub hub;
        private Notifications notifications;
5. <span data-ttu-id="5bfe7-126">Ezt követően a a **onCreate** módszer, távolítsa el a inicializálása a **hub** mező és a **registerWithNotificationHubs** metódust.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-126">Then, in the **onCreate** method, remove the initialization of the **hub** field and the **registerWithNotificationHubs** method.</span></span> <span data-ttu-id="5bfe7-127">Majd adja hozzá a következő sort, amelyben a példányának inicializálása a **értesítések** osztály.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-127">Then add the following lines which initialize an instance of the **Notifications** class.</span></span> 

        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            MyHandler.mainActivity = this;

            NotificationsManager.handleNotifications(this, SENDER_ID,
                    MyHandler.class);

            notifications = new Notifications(this, SENDER_ID, HubName, HubListenConnectionString);

            notifications.subscribeToCategories(notifications.retrieveCategories());
        }

    <span data-ttu-id="5bfe7-128">`HubName`és `HubListenConnectionString` kell már lehet beállítani a `<hub name>` és `<connection string with listen access>` helyőrzőket az értesítési központ nevére és a kapcsolati karakterláncot *DefaultListenSharedAccessSignature* korábban beszerzett.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-128">`HubName` and `HubListenConnectionString` should already be set with the `<hub name>` and `<connection string with listen access>` placeholders with your notification hub name and the connection string for *DefaultListenSharedAccessSignature* that you obtained earlier.</span></span>

    > [AZURE.NOTE] <span data-ttu-id="5bfe7-129">Eszközzel együtt egy ügyfélalkalmazás hitelesítő adatok nem általában biztonságos, mert csak kell terjeszteni a figyelési hozzáférési kulcs ügyfél alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-129">Because credentials that are distributed with a client app are not generally secure, you should only distribute the key for listen access with your client app.</span></span> <span data-ttu-id="5bfe7-130">Figyeljen hozzáférés lehetővé teszi, hogy az alkalmazás regisztrálásához értesítések, de a meglévő regisztrációk nem módosítható, és értesítések nem küldhető el.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-130">Listen access enables your app to register for notifications, but existing registrations cannot be modified and notifications cannot be sent.</span></span> <span data-ttu-id="5bfe7-131">A teljes körű hozzáférési kulcs értesítések küldését, és meglévő regisztrációk módosítása védett háttérszolgáltatás használatban.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-131">The full access key is used in a secured backend service for sending notifications and changing existing registrations.</span></span>


1. <span data-ttu-id="5bfe7-132">Ezt követően adja hozzá az alábbi importálásokat és `subscribe` metódust az előfizetés gombra kattintson esemény:</span><span class="sxs-lookup"><span data-stu-id="5bfe7-132">Then, add the following imports and `subscribe` method to handle the subscribe button click event:</span></span>
   
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
   
    <span data-ttu-id="5bfe7-133">Ezzel a módszerrel hoz létre a kategóriák és a használja listáját a **értesítések** osztályra, hogy a lista a helyi tároló tárolja, és regisztrálja a megfelelő címkéket az értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-133">This method creates a list of categories and uses the **Notifications** class to store the list in the local storage and register the corresponding tags with your notification hub.</span></span> <span data-ttu-id="5bfe7-134">Kategóriák módosításakor a regisztrációs újra létrejön az új kategóriák.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-134">When categories are changed, the registration is recreated with the new categories.</span></span>

<span data-ttu-id="5bfe7-135">Az alkalmazás már kategóriák készlete tárolja az eszköz helyi tárolóhoz, és regisztrálhatja az értesítési központban, amikor a felhasználó módosítja a kiválasztott kategóriák.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-135">Your app is now able to store a set of categories in local storage on the device and register with the notification hub whenever the user changes the selection of categories.</span></span>

## <a name="register-for-notifications"></a><span data-ttu-id="5bfe7-136">Az értesítések regisztrálása</span><span class="sxs-lookup"><span data-stu-id="5bfe7-136">Register for notifications</span></span>
<span data-ttu-id="5bfe7-137">Ezeket a lépéseket az értesítési központ indításakor a helyi tárolóban tárolt kategóriák segítségével regisztrálja.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-137">These steps register with the notification hub on startup using the categories that have been stored in local storage.</span></span>

> [!NOTE]
> <span data-ttu-id="5bfe7-138">A hozzárendelt által Google Cloud Messaging (GCM) registrationId bármikor módosíthatja, mert az értesítések a notification hibák elkerülése érdekében gyakran kell regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-138">Because the registrationId assigned by Google Cloud Messaging (GCM) can change at any time, you should register for notifications frequently to avoid notification failures.</span></span> <span data-ttu-id="5bfe7-139">Ebben a példában regisztrál az értesítési minden alkalommal, az alkalmazás indítása.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-139">This example registers for notification every time that the app starts.</span></span> <span data-ttu-id="5bfe7-140">Gyakran futtatott alkalmazások esetén naponta csak egyszer, valószínűleg kihagyhatja regisztrációt, hogy a sávszélesség megőrzése, ha az előző regisztráció óta eltelt egy napnál.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-140">For apps that are run frequently, more than once a day, you can probably skip registration to preserve bandwidth if less than a day has passed since the previous registration.</span></span>
> 
> 

1. <span data-ttu-id="5bfe7-141">Adja hozzá a következő kódot végén a **onCreate** metódust a **MainActivity** osztály:</span><span class="sxs-lookup"><span data-stu-id="5bfe7-141">Add the following code at the end of the **onCreate** method in the **MainActivity** class:</span></span>
   
        notifications.subscribeToCategories(notifications.retrieveCategories());
   
    <span data-ttu-id="5bfe7-142">Ez biztosítja, hogy az alkalmazás minden indításakor azt a kategóriák átmásolja a helyi tároló, és kéri a regisztrálás ezen kategóriák.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-142">This makes sure that every time the app starts it retrieves the categories from local storage and requests a registeration for these categories.</span></span> 
2. <span data-ttu-id="5bfe7-143">Ezután frissítse a `onStart()` metódusában a `MainActivity` osztály az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="5bfe7-143">Then update the `onStart()` method of the `MainActivity` class as follows:</span></span>
   
    <span data-ttu-id="5bfe7-144">@Overridevédett "void" onStart() {</span><span class="sxs-lookup"><span data-stu-id="5bfe7-144">@Override  protected void onStart() {</span></span>
   
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
    <span data-ttu-id="5bfe7-145">}</span><span class="sxs-lookup"><span data-stu-id="5bfe7-145">}</span></span>
   
    <span data-ttu-id="5bfe7-146">Ekkor frissül, a fő tevékenységnél a korábban mentett kategóriák állapota alapján.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-146">This updates the main activity based on the status of previously saved categories.</span></span>

<span data-ttu-id="5bfe7-147">Az alkalmazás most már befejeződött, és képes tárolni a kategóriák az eszköz helyi tárolójára való regisztrálásához az értesítési központnak a felhasználói kategóriák kiválasztott megváltozásakor használt.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-147">The app is now complete and can store a set of categories in the device local storage used to register with the notification hub whenever the user changes the selection of categories.</span></span> <span data-ttu-id="5bfe7-148">A következő azt határozza meg, amely ennek az alkalmazásnak kategória értesítéseket küldhet háttérkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-148">Next, we will define a backend that can send category notifications to this app.</span></span>

## <a name="sending-tagged-notifications"></a><span data-ttu-id="5bfe7-149">Címkézett értesítések küldése</span><span class="sxs-lookup"><span data-stu-id="5bfe7-149">Sending tagged notifications</span></span>
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-the-app-and-generate-notifications"></a><span data-ttu-id="5bfe7-150">Futtassa az alkalmazást, és értesítések</span><span class="sxs-lookup"><span data-stu-id="5bfe7-150">Run the app and generate notifications</span></span>
1. <span data-ttu-id="5bfe7-151">Az Android Studióban az alkalmazás elkészítésére, és indítsa el a egy eszközt vagy emulátort.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-151">In Android Studio, build the app and start it on a device or emulator.</span></span>
   
    <span data-ttu-id="5bfe7-152">Vegye figyelembe, hogy az alkalmazás felhasználói felület számos váltógombok, amely lehetővé teszi, hogy válassza ki a előfizetni.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-152">Note that the app UI provides a set of toggles that lets you choose the categories to subscribe to.</span></span>
2. <span data-ttu-id="5bfe7-153">Egy vagy több kategóriák váltógombok engedélyezése, majd kattintson az **előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-153">Enable one or more categories toggles, then click **Subscribe**.</span></span>
   
    <span data-ttu-id="5bfe7-154">Az app alakítja át a kiválasztott kategóriákra címkék, és egy új eszköz regisztrálása a kijelölt címkék kéri le az értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-154">The app converts the selected categories into tags and requests a new device registration for the selected tags from the notification hub.</span></span> <span data-ttu-id="5bfe7-155">A regisztrált kategóriák tér vissza, és egy bejelentési értesítés jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-155">The registered categories are returned and displayed in a toast notification.</span></span>
3. <span data-ttu-id="5bfe7-156">Egy új értesítés küldése a .NET-Konzolalkalmazás futtatásával.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-156">Send a new notification by running the .NET Console app.</span></span>  <span data-ttu-id="5bfe7-157">Másik lehetőségként az értesítési központ hibakeresési lapján címkézett sablon értesítéseket küldhet a [klasszikus Azure portál].</span><span class="sxs-lookup"><span data-stu-id="5bfe7-157">Alternatively, you can send tagged template notifications using the debug tab of your notification hub in the [Azure Classic Portal].</span></span>
   
    <span data-ttu-id="5bfe7-158">A kiválasztott kategóriákra értesítések bejelentési értesítések jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-158">Notifications for the selected categories appear as toast notifications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5bfe7-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5bfe7-159">Next steps</span></span>
<span data-ttu-id="5bfe7-160">Az oktatóanyag azt megtudta, hogyan kell közvetíteni legfrissebb hírek kategória szerint.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-160">In this tutorial we learned how to broadcast breaking news by category.</span></span> <span data-ttu-id="5bfe7-161">Vegye figyelembe az alábbi oktatóanyagok számára más speciális Notification Hubs-forgatókönyvek közül befejezése:</span><span class="sxs-lookup"><span data-stu-id="5bfe7-161">Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:</span></span>

* <span data-ttu-id="5bfe7-162">[Honosított legfrissebb hírek szórási a Notification Hubs használatával]</span><span class="sxs-lookup"><span data-stu-id="5bfe7-162">[Use Notification Hubs to broadcast localized breaking news]</span></span>
  
    <span data-ttu-id="5bfe7-163">Megtudhatja, hogyan bontsa ki a legfrissebb hírek app küldő honosított értesítések engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="5bfe7-163">Learn how to expand the breaking news app to enable sending localized notifications.</span></span>

<!-- Images. -->
[A1]: ./media/notification-hubs-aspnet-backend-android-breaking-news/android-breaking-news1.PNG

<!-- URLs.-->
[get-started]: notification-hubs-android-push-notification-google-gcm-get-started.md
<span data-ttu-id="5bfe7-164">[Honosított legfrissebb hírek szórási a Notification Hubs használatával]: /manage/services/notification-hubs/breaking-news-localized-dotnet/</span><span class="sxs-lookup"><span data-stu-id="5bfe7-164">[Use Notification Hubs to broadcast localized breaking news]: /manage/services/notification-hubs/breaking-news-localized-dotnet/</span></span>
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
<span data-ttu-id="5bfe7-165">[klasszikus Azure portál]: https://manage.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="5bfe7-165">[Azure Classic Portal]: https://manage.windowsazure.com</span></span>
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
