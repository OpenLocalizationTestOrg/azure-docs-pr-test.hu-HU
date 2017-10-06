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
# <a name="use-notification-hubs-toosend-breaking-news"></a>Használja a Notification Hubs toosend legfrissebb hírek
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Áttekintés
Ez a témakör bemutatja, hogyan toouse Azure Notification Hubs toobroadcast breaking news értesítések tooan Android-alkalmazás. Amikor végzett, akkor tudja tooregister számára megtörje hírek kategóriák érdekli, és ezen kategóriák csak leküldéses értesítések fogadásához. Ebben a forgatókönyvben számos alkalmazás általános felépítését, amelyben értesítések vannak küldött toobe toogroups, amely rendelkezik deklarálva érdeklődési rajtuk, például az RSS-olvasóval, az alkalmazások zene ventilátorok stb.

Szórási forgatókönyvek engedélyezve vannak, beleértve a következőket egy vagy több *címkék* hello értesítési központ regisztráció létrehozásakor. Ha az értesítések küldése tooa címke hello címke regisztrált minden eszköz hello értesítést kap. Mivel a címkékkel egyszerűen csak karakterláncok, nincs kiépítve előzetes toobe. Címkékkel kapcsolatos további információkért tekintse meg túl[Notification Hubs útválasztási és címke kifejezések](notification-hubs-tags-segment-push-message.md).

## <a name="prerequisites"></a>Előfeltételek
Ebben a témakörben megfogalmazott célra épül létrehozott hello app [Ismerkedés a Notification Hubs][get-started]. Az oktatóanyag elindítása előtt már végrehajtotta [Ismerkedés a Notification Hubs][get-started].

## <a name="add-category-selection-toohello-app"></a>Kategória kiválasztása toohello alkalmazás hozzáadása
első lépés hello tooadd hello felhasználói felületi elemek tooyour meglévő fő tevékenységet, amelyek lehetővé teszik a hello felhasználói tooselect kategóriák tooregister. felhasználó által kijelölt hello kategóriák hello eszközön tárolja. Hello alkalmazás indításakor a eszközregisztráció címkeként az értesítési központ kijelölt hello kategóriákhoz jön létre.

1. Nyissa meg a res/layout/activity_main.xml fájlt, és helyettesítő hello következőre hello tartalom:
   
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
2. Nyissa meg a res/values/strings.xml fájlt, és adja hozzá az alábbi hello:
   
        <string name="button_subscribe">Subscribe</string>
        <string name="label_world">World</string>
        <string name="label_politics">Politics</string>
        <string name="label_business">Business</string>
        <string name="label_technology">Technology</string>
        <string name="label_science">Science</string>
        <string name="label_sports">Sports</string>
   
    A main_activity.xml grafikus elrendezés most példához hasonló:
   
    ![][A1]
3. Most hozzon létre egy osztályt **értesítések** a hello azonos csomag a **MainActivity** osztály.
   
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
   
    Ez az osztály hello helyi tároló toostore hello kategóriáinak híreket, hogy az eszköz rendelkezik-e tooreceive használja. Az ezen kategóriák módszerek tooregister is tartalmaz.
4. Az a **MainActivity** osztály, távolítsa el a privát mezőinek **NotificationHub** és **GoogleCloudMessaging**, és a mező felvétele **értesítések**:
   
        // private GoogleCloudMessaging gcm;
        // private NotificationHub hub;
        private Notifications notifications;
5. Ezt követően a hello **onCreate** módszer, eltávolítás hello inicializálása hello **hub** mező és hello **registerWithNotificationHubs** metódust. Majd adja hozzá a sort, amelyben a hello példányának inicializálása a következő hello **értesítések** osztály. 

        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            MyHandler.mainActivity = this;

            NotificationsManager.handleNotifications(this, SENDER_ID,
                    MyHandler.class);

            notifications = new Notifications(this, SENDER_ID, HubName, HubListenConnectionString);

            notifications.subscribeToCategories(notifications.retrieveCategories());
        }

    `HubName`és `HubListenConnectionString` már kell beállítani a hello `<hub name>` és `<connection string with listen access>` helyőrzőket az értesítési központ nevét és hello kapcsolati karakterláncot *DefaultListenSharedAccessSignature* beszerzett korábban.

    > [AZURE.NOTE] Ügyfél alkalmazáshoz elosztott hitelesítő adatok nem általában biztonságos, mert csak kell terjeszteni figyelési hozzáférési kulcs hello ügyfél alkalmazása. Figyeljen hozzáférés lehetővé teszi, hogy az alkalmazás tooregister az értesítéseket, de a meglévő regisztrációját nem lehet módosítani, és értesítések nem küldhető el. hello teljes körű hozzáférési kulcsot használnak a következő biztonságos háttérszolgáltatás értesítések küldését, és meglévő regisztrációk módosítása.


1. Adja hozzá a következő hello importálja és `subscribe` metódus toohandle hello előfizetés gomb eseményt:
   
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
   
    Ezzel a módszerrel hoz létre a kategóriák és a hello listáját **értesítések** osztály toostore hello lista hello helyi tárolóban, és regisztrálja a hello megfelelő címkéket az értesítési központban. Kategóriák megváltozásakor hello regisztrációs hello új kategóriák újra létrejön.

Az alkalmazás most már tudja toostore kategóriák készlete hello eszközön helyi tárolóban és regisztrálása az értesítési központban hello hello felhasználót módosítások hello kategóriák kiválasztását.

## <a name="register-for-notifications"></a>Az értesítések regisztrálása
Ezeket a lépéseket hello értesítési központ használatával a helyi tárolóban tárolt hello kategóriák indításkor regisztrálja.

> [!NOTE]
> Hello registrationId által Google Cloud Messaging (GCM) hozzárendelt bármikor módosíthatja, mert regisztrálnia kell az értesítésekhez gyakran tooavoid értesítés sikertelen. Ebben a példában regisztrál az értesítési hello alkalmazás minden indításakor. Gyakran futtatott alkalmazások esetén naponta csak egyszer, valószínűleg kihagyhatja regisztrációs toopreserve sávszélesség Ha kevesebb mint egy nappal hello előző regisztráció óta eltelt.
> 
> 

1. Adja hozzá a következő kód hello hello végén hello **onCreate** metódus a hello **MainActivity** osztály:
   
        notifications.subscribeToCategories(notifications.retrieveCategories());
   
    Ez biztosítja, hogy hello alkalmazás minden indításakor azt hello kategóriák átmásolja a helyi tárolóhoz, és kéri a regisztrálás ezen kategóriák. 
2. Ezután frissítse a hello `onStart()` hello metódusában `MainActivity` osztály az alábbiak szerint:
   
    @Overridevédett "void" onStart() {
   
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
    }
   
    Ekkor frissül hello fő tevékenységnél a korábban mentett kategóriák hello állapota alapján.

hello alkalmazás kész, és képes tárolni a kategóriák hello eszköz használt helyi tárhely tooregister hello értesítési központban az amikor hello felhasználót módosítások hello kategóriák kiválasztása. A következő kategória értesítések toothis app küldő háttérkiszolgálón meghatározzák azt.

## <a name="sending-tagged-notifications"></a>Címkézett értesítések küldése
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-hello-app-and-generate-notifications"></a>Hello alkalmazás futtatását, és értesítések
1. Az Android Studióban hello alkalmazás elkészítésére, és indítsa el a egy eszközt vagy emulátort.
   
    Megjegyzés: a felhasználói felület számos hello alkalmazást, amely váltja kiválaszthatja hello kategóriák toosubscribe számára.
2. Egy vagy több kategóriák váltógombok engedélyezése, majd kattintson az **előfizetés**.
   
    hello app hello kiválasztott kategóriák alakítja címkék, és egy új eszközök regisztrációja kijelölt hello címkék hello értesítési központ érkező kérelmeket. hello regisztrált kategóriák adott vissza, és egy bejelentési értesítés jelenik meg.
3. Egy új értesítés küldése hello .NET Konzolalkalmazás futtatásával.  Másik lehetőségként az értesítési központ hibakeresési lapján hello használatát hello címkézett sablon értesítéseket küldhet [klasszikus Azure portál].
   
    Értesítések a kiválasztott hello kategóriák bejelentési értesítések jelennek meg.

## <a name="next-steps"></a>Következő lépések
Ez az oktatóanyag azt megtanulta, hogyan toobroadcast legfrissebb hírek kategória szerint. Vegye figyelembe, hogy befejezése hello oktatóprogramot kínál, amelyek más speciális Notification Hubs forgatókönyvek jelölje ki a következő egyikét:

* [Használjon honosított toobroadcast Notification Hubs – legfrissebb hírek]
  
    Ismerje meg, hogyan megtörje hírek app tooenable küldése tooexpand hello honosított értesítések.

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
