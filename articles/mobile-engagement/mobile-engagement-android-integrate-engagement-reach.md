---
title: "Mobile Engagement Android SDK-integráció aaaAzure"
description: "Legújabb frissítések és az Azure Mobile Engagement Android SDK eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 9ec3fab3-35ec-458e-bf41-6cdd69e3fa44
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 06/27/2016
ms.author: piyushjo
ms.openlocfilehash: 4ab6143771bdc0758a548abb529d6bde98fc0e4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-engagement-reach-on-android"></a>Hogyan tooIntegrate Engagement Reach Android rendszeren
> [!IMPORTANT]
> Hogyan tooIntegrate Engagement az Android-dokumentum Ez az útmutató követése előtt hello ismertetett hello integrációs eljárást kell követnie.
> 
> 

## <a name="standard-integration"></a>Standard integráció

Másolja a projekt SDK hello Reach erőforrás fájlokat:

* Hello fájlok másolása hello `res/layout` mappa küldi hello SDK be hello `res/layout` az alkalmazás mappájában.
* Hello fájlok másolása hello `res/drawable` mappa küldi hello SDK be hello `res/drawable` az alkalmazás mappájában.

Szerkessze a `AndroidManifest.xml` fájlt:

* Adja hozzá a következő szakasz hello (közötti hello `<application>` és `</application>` címkék):
  
          <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
              <category android:name="android.intent.category.DEFAULT" />
              <data android:mimeType="text/plain" />
            </intent-filter>
          </activity>
          <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
              <category android:name="android.intent.category.DEFAULT" />
              <data android:mimeType="text/html" />
            </intent-filter>
          </activity>
          <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
              <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
          </activity>
          <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
            <intent-filter>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
              <category android:name="android.intent.category.DEFAULT"/>
            </intent-filter>
          </activity>
          <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
            <intent-filter>
              <action android:name="android.intent.action.BOOT_COMPLETED"/>
              <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
              <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
            </intent-filter>
          </receiver>
          <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
            <intent-filter>
              <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
            </intent-filter>
          </receiver>
* Az engedély tooreplay rendszerértesítéseket, amely nem rendszerindításkor van szüksége (más módon lemezen megmarad, de nem fognak többé megjelenni valóban telepítette, tooinclude ez).
  
          <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
* Adjon meg egy ikon másolásával és szerkesztése a következő szakasz hello (az alkalmazás és a rendszer megfelelően) értesítésekhez használatos (közötti hello `<application>` és `</application>` címkék):
  
          <meta-data android:name="engagement:reach:notification:icon" android:value="<name_of_icon_WITHOUT_file_extension_and_WITHOUT_'@drawable/'>" />

> [!IMPORTANT]
> Ez a szakasz **kötelező** Ha a kíván használni a rendszer értesítéseket Reach-kampányokat létrehozásakor. Android megakadályozza, hogy a rendszer értesítéseket ikonok nélkül látható. Ezért ez a szakasz kihagyása esetén a végfelhasználók számára nem lesz képes tooreceive őket.
> 
> 

* Rendszerértesítéseket használatával nagy vonalakban tekinti kampányok hoz létre, ha szüksége van-e a következő engedélyek tooadd hello (után hello `</application>` címke) Ha hiányoznak:
  
          <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
          <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
  
  * Az Android m-alapú, és ha az alkalmazás célozza 23 vagy újabb, Android API-szintet ``WRITE_EXTERNAL_STORAGE`` engedély a felhasználó jóváhagyást igényel. Kérjük, olvassa el [ebben a szakaszban](mobile-engagement-android-integrate-engagement.md#android-m-permissions).
* A rendszer értesítések azt is megadhatja a hello érhetik el kampány hello eszköz kell gyűrű és/vagy a mozog. Az toowork, rendelkezik arra megtörténjen a deklarálása a következő engedély hello toomake (után hello `</application>` címke):
  
          <uses-permission android:name="android.permission.VIBRATE" />
  
  Ez az engedély nélkül Android megakadályozza, hogy a rendszer értesítéseket megjelenítésének Ha bejelölte hello ring vagy hello mozog hello Reach-kampány manager beállítást.

## <a name="native-push"></a>Natív leküldéssel
Most, hogy konfigurálta a Reach-modul, tooconfigure natív leküldéses toobe képes tooreceive hello kampányok hello eszközön kell.

Az Android két szolgáltatás támogatott:

* Google Play-eszközök: használata [Google Cloud Messaging] által következő hello [hogyan tooIntegrate az Engagement GCM útmutató](mobile-engagement-android-gcm-integrate.md) útmutató.
* Amazon eszközök: használata [Amazon Device Messaging] által következő hello [hogyan tooIntegrate az Engagement ADM útmutató](mobile-engagement-android-adm-integrate.md) útmutató.

Ha azt szeretné, hogy tootarget Amazon és a Google Play eszközök, a lehetséges toohave fejlesztési 1 AndroidManifest.xml/APK tartalmát. De tooAmazon terjesztésekor, azok előfordulhat, hogy elutasítja az alkalmazás, ha találnak GCM-kódot.

Több APKs ebben az esetben kell használnia.

**Az alkalmazás most már készen áll a tooreceive és megjelenítési elérni kampányok van!**

## <a name="how-toohandle-data-push"></a>Toohandle adatok leküldés
### <a name="integration"></a>Integráció
Ha az alkalmazás toobe képes tooreceive Reach adatok leküldéses értesítések, toocreate alosztályának kell `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` és hello vonatkozó hivatkozást `AndroidManifest.xml` fájl (közötti hello `<application>` és/vagy `</application>` címkék):

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

Akkor lehet felülbírálni hello `onDataPushStringReceived` és `onDataPushBase64Received` visszahívások. Például:

            public class MyDataPushReceiver extends EngagementReachDataPushReceiver
            {
              @Override
              protected Boolean onDataPushStringReceived(Context context, String category, String body)
              {
                Log.d("tmp", "String data push message received: " + body);
                return true;
              }

              @Override
              protected Boolean onDataPushBase64Received(Context context, String category, byte[] decodedBody, String encodedBody)
              {
                Log.d("tmp", "Base64 data push message received: " + encodedBody);
                // Do something useful with decodedBody like updating an image view
                return true;
              }
            }

### <a name="category"></a>Kategória
hello kategória paraméter nem kötelező, amikor adatokat leküldéses kampány létrehozása, és lehetővé teszi, hogy toofilter adatok leküldéses értesítések. Ez akkor hasznos, ha több szórási fogadók adatleküldések, különböző típusú kezelése, vagy ha azt szeretné, hogy toopush különböző a `Base64` adatok és a kívánt tooidentify típusuk őket elemzése előtt.

### <a name="callbacks-return-parameter"></a>A visszahívások visszatérési paraméter
Az alábbiakban néhány irányelvek tooproperly leíró hello paraméternek `onDataPushStringReceived` és `onDataPushBase64Received`:

* Visszaadja-e a szórásos receiver `null` hello visszahívási, ha egy adatok toohandle leküldés nem ismeri a. Hello kategória toodetermine kell használnia, hogy a szórásos receiver hello adatleküldés kezelje-e.
* Hello szórásos receiver valamelyikét kell visszaadnia `true` a hello visszahívási, ha elfogadja a hello adatleküldés.
* Hello szórásos receiver valamelyikét kell visszaadnia `false` a hello visszahívási Ha hello adatleküldés felismeri, de bármilyen oknál fogva figyelmen kívül hagyja. Például vissza `false` érvénytelen hello fogadott adatok esetén.
* Ha egy szórási fogadó beolvasása `true` során egy másik egyikét adja vissza `false` hello ugyanazt az adatleküldés, hello viselkedés van definiálva, akkor soha ne tegye, hogy.

hello visszatérési típusa csak hello Reach statisztika szolgál:

* `Replied`értéke akkor növekszik, ha az egyik hello szórási fogadók adott vissza, vagy `true` vagy `false`.
* `Actioned`értéke akkor nő, csak akkor, ha egy hello szórási visszaadott fogadók `true`.

## <a name="how-toocustomize-campaigns"></a>Hogyan toocustomize kampányok
toocustomize kampányok, módosíthatja a Reach SDK hello hello elrendezése.

Kell hello elrendezések használt összes hello azonosítónak tároljuk és hello nézettípusokat hello azonosítót, különösen a lemezkép és szöveges nézeteket használó róluk. Egyes nézetek csak használt toohide vagy szemléltet, ezért a típus módosítható. Tekintse meg a hello forráskódját, ha azt tervezi, toochange hello típusú nézet a megadott hello elrendezés.

### <a name="notifications"></a>Értesítések
Az értesítések két típusa van: az elrendezés fájlokat használó rendszer és az alkalmazáson belüli értesítések.

#### <a name="system-notifications"></a>Rendszer-értesítések
toouse hello kell toocustomize rendszerértesítéseket **kategóriák**. Túl is ugorhat[kategóriák](#categories).

#### <a name="in-app-notifications"></a>Alkalmazáson belüli értesítések
Alapértelmezés szerint egy alkalmazásbeli értesítés egy nézet, amellyel dinamikusan hozzáadott toohello aktuális tevékenység felhasználói felület Köszönjük toohello Android módszer `addContentView()`. Ez a lehetőség egy értesítési területre. Értesítési átfedések nincsenek rendkívül gyors integrációs, mert nem igényelnek, toomodify bármely elrendezés az alkalmazásban.

az értesítési átfedések toomodify hello megjelenését, egyszerűen módosíthatja hello fájlt `engagement_notification_area.xml` tooyour kell.

> [!NOTE]
> hello fájl `engagement_notification_overlay.xml` hello tartalmazó használt toocreate egy értesítési területre hello fájl tartalmaz `engagement_notification_area.xml`. Testre is szabhatja az toosuit az igényeinek, (például hello értesítési területen belül hello átfedő elhelyezéséhez esetében).
> 
> 

##### <a name="include-notification-layout-as-part-of-an-activity-layout"></a>Egy tevékenység elrendezés részeként értesítési elrendezés tartalmazza
Átfedések ideális az egy gyors integrációt, de lehet kényelmetlen vagy mellékhatásokkal bizonyos esetekben. hello átfedő rendszer testre szabható tevékenység szinten, így könnyen tooprevent hatásai különleges tevékenységekhez.

Dönthet úgy is tooinclude az értesítési elrendezés a a meglévő elrendezés Köszönjük toohello Android **tartalmaznak** utasítást. hello az alábbiakban látható egy példa egy módosított `ListActivity` elrendezést tartalmazó csak egy `ListView`.

**Mielőtt Engagement-integráció:**

            <?xml version="1.0" encoding="utf-8"?>
            <ListView
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@android:id/list"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent" />

**Miután Engagement-integráció:**

            <?xml version="1.0" encoding="utf-8"?>
            <LinearLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <ListView
                android:id="@android:id/list"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:layout_weight="1" />

              <include layout="@layout/engagement_notification_area" />

            </LinearLayout>

Ebben a példában hozzáadott egy szülőtároló mert hello eredeti elrendezés egy listanézeti hello felső szintű eleme. Is hozzáadtunk `android:layout_weight="1"` toobe képes tooadd egy nézet egy listanézeti alatt konfigurált `android:layout_height="fill_parent"`.

hello Engagement Reach SDK automatikusan észleli, hogy hello értesítési elrendezés ezt a tevékenységet tartalmazza, és nem adja hozzá egy átmeneti területre, ehhez a tevékenységhez.

> [!TIP]
> Az alkalmazás egy ListActivity használja, ha egy látható Reach területre elkerülheti reakciójú tooclicked elemek hello listanézetben többé. Ez az egy ismert probléma. toowork a probléma megoldásához javasoljuk, hogy tooembed hello értesítési elrendezés saját lista tevékenység elrendezésben, például a hello korábbi minta.
> 
> 

##### <a name="disabling-application-notification-per-activity"></a>/ Tevékenység alkalmazás értesítés letiltása
Ha nem szeretné, hogy hello átfedő toobe adnak tooyour tevékenység, és ha nincs hello értesítési elrendezés saját elrendezésben, letilthatja a hello felirat a tevékenységnek hello `AndroidManifest.xml` hozzáadásával egy `meta-data` szakasz hasonlóan a hello következő Példa:

            <activity android:name="SplashScreenActivity">
              <meta-data android:name="engagement:notification:overlay" android:value="false"/>
            </activity>

#### <a name="categories"></a>Kategóriák
Megadott elrendezések hello módosításakor az értesítések hello megjelenésének módosítása. Kategóriák engedélyezése toodefine, különböző megcélzott értesítések (valószínűleg viselkedések) keresi. A kategória a Reach-kampány létrehozásakor adható meg. Ne feledje, hogy kategóriák is segítségével testre szabható mutató hirdetmények és szavazások, a dokumentum későbbi szakaszában ismertetett.

az értesítések kategória leíró tooregister, kell tooadd hívás hello alkalmazás inicializálásakor.

> [!IMPORTANT]
> Olvassa el a hello figyelmeztetést jelenít meg hello android: folyamat attribútum \<android-sdk-engagement-folyamat\> a hello hogyan tooIntegrate Engagement Android témakör a folytatás előtt.
> 
> 

hello alábbi példa azt feltételezi, hogy Ön hello előző figyelmeztetés vonatkozik, és alosztályának `EngagementApplication`:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), "myCategory");
              }
            }

Hello `MyNotifier` objektum hello kategória értesítéskezelő hello végrehajtását. Hello, vagy egy megvalósított `EngagementNotifier` illesztőfelület vagy hello alapértelmezett végrehajtásának sub osztály: `EngagementDefaultNotifier`.

Vegye figyelembe, hogy ugyanazon bejelentő hello kezelni tud a különböző kategóriákba, ilyen regisztrálhatja:

            reachAgent.registerNotifier(new MyNotifier(this), "myCategory", "myAnotherCategory");

tooreplace hello alapértelmezett kategória implementációja regisztrálhatja a megvalósítás, például a következő példa hello:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), Intent.CATEGORY_DEFAULT); // "android.intent.category.DEFAULT"
              }
            }

a kezelő használt hello aktuális kategória felülbírálhatja a legtöbb módszer paraméterként átadott `EngagementDefaultNotifier`.

Átadva akár egy `String` paraméter vagy közvetetten egy `EngagementReachContent` objektum, amely rendelkezik egy `getCategory()` metódus.

Módosíthatja a legtöbb hello értesítési létrehozási folyamata módszerek az újradefiniálás `EngagementDefaultNotifier`, amely speciális testreszabás érzi, hogy az ingyenes tootake hello műszaki dokumentáció és hello forráskód meg.

##### <a name="in-app-notifications"></a>Alkalmazáson belüli értesítések
Ha csak toouse alternatív elrendezés az egy adott konkrét kategóriával, mint például a következő hello Megvalósíthat ez:

            public class MyNotifier extends EngagementDefaultNotifier
            {
              public MyNotifier(Context context)
              {
                super(context);
              }

              @Override
              protected int getOverlayLayoutId(String category)
              {
                return R.layout.my_notification_overlay;
              }


              @Override
              public Integer getOverlayViewId(String category)
              {
                return R.id.my_notification_overlay;
              }

              @Override
              public Integer getInAppAreaId(String category)
              {
                return R.id.my_notification_area;
              }
            }

**Példa `my_notification_overlay.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <RelativeLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/my_notification_overlay"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <include layout="@layout/my_notification_area" />

            </RelativeLayout>

Ahogy látja, hello átfedő nézet azonosító hello szabványos egy másik. Fontos, hogy minden elrendezés átfedések egy egyedi azonosítót használjon.

**Példa `my_notification_area.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <merge
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <RelativeLayout
                android:id="@+id/my_notification_area"
                android:layout_width="fill_parent"
                android:layout_height="64dp"
                android:layout_alignParentTop="true"
                android:background="#B000">

                <LinearLayout
                  android:orientation="horizontal"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent"
                  android:gravity="center_vertical">

                  <ImageView
                    android:id="@+id/engagement_notification_icon"
                    android:layout_width="48dp"
                    android:layout_height="48dp" />

                  <LinearLayout
                    android:id="@+id/engagement_notification_text"
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent"
                    android:layout_weight="1"
                    android:gravity="center_vertical">

                    <TextView
                      android:id="@+id/engagement_notification_title"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:singleLine="true"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Medium" />

                    <TextView
                      android:id="@+id/engagement_notification_message"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:maxLines="2"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Small" />

                  </LinearLayout>

                  <ImageView
                    android:id="@+id/engagement_notification_image"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:adjustViewBounds="true" />

                  <ImageButton
                    android:id="@+id/engagement_notification_close_area"
                    android:visibility="invisible"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:src="@android:drawable/btn_dialog"
                    android:background="#0F00" />

                </LinearLayout>

                <ImageButton
                  android:id="@+id/engagement_notification_close"
                  android:layout_width="wrap_content"
                  android:layout_height="fill_parent"
                  android:layout_alignParentRight="true"
                  android:src="@android:drawable/btn_dialog"
                  android:background="#0F00" />

              </RelativeLayout>

            </merge>

Ahogy látja, hello értesítési terület nézet azonosító hello szabványos egy másik. Fontos, hogy minden elrendezés egy egyedi azonosítót használ, az értesítési terület.

Egyszerű példa a kategória lehetővé teszi az alkalmazás (vagy alkalmazásbeli) értesítések, üdvözlő képernyőt hello tetején jelenik meg. A Microsoft hello hello értesítési területen maga használt szabványos azonosítókat nem változott.

Ha azt szeretné, hogy tooredefine hello toochange `EngagementDefaultNotifier.prepareInAppArea` metódust. Ajánlott toolook hello műszaki dokumentáció és annak forráskódját hello `EngagementNotifier` és `EngagementDefaultNotifier` Ha azt szeretné, a szint speciális.

##### <a name="system-notifications"></a>Rendszer-értesítések
Kiterjesztésével `EngagementDefaultNotifier`, ha szeretné felülbírálni az `onNotificationPrepared` tooalter hello értesítési hello alapértelmezett megvalósítás előkészített.

Példa:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content)
              throws RuntimeException
            {
              if ("ongoing".equals(content.getCategory()))
                notification.flags |= Notification.FLAG_ONGOING_EVENT;
              return true;
            }

Ez a példa jeleníti meg a folyamatban lévő esemény hello "folyamatban" kategóriát használatakor tartalom rendszerértesítőként teszi.

Ha azt szeretné, hogy toobuild hello `Notification` objektum teljesen új, lépjen vissza `false` toohello metódus és hívás `notify` saját kezűleg a hello `NotificationManager`. Ebben az esetben fontos, hogy maradjon egy `contentIntent`, egy `deleteIntent` és által használt értesítési azonosítójában hello `EngagementReachReceiver`.

Íme egy végrehajtás megfelelő példát:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content) throws RuntimeException
            {
              /* Required fields */
              NotificationCompat.Builder builder = new NotificationCompat.Builder(mContext)
                .setSmallIcon(notification.icon)              // icon is mandatory
                .setContentIntent(notification.contentIntent) // keep content intent
                .setDeleteIntent(notification.deleteIntent);  // keep delete intent

              /* Your customization */
              // builder.set...

              /* Dismiss option can be managed only after build */
              Notification myNotification = builder.build();
              if (!content.isNotificationCloseable())
                myNotification.flags |= Notification.FLAG_NO_CLEAR;

              /* Notify here instead of super class */
              NotificationManager manager = (NotificationManager) mContext.getSystemService(Context.NOTIFICATION_SERVICE);
              manager.notify(getNotificationId(content), myNotification); // notice hello call tooget hello right identifier

              /* Return false, we notify ourselves */
              return false;
            }

##### <a name="notification-only-announcements"></a>Csak értesítést tartalmazó hirdetmények
hello hello vezetése kattintson a csak közlemény testre szabható felülbírálásával értesítést `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` előkészített toomodify hello `Intent`. Ezzel a módszerrel teszi tootune hello jelzők könnyen.

Például tooadd hello `SINGLE_TOP` jelző:

            @Override
            protected Intent onNotifAnnouncementIntentPrepared(EngagementNotifAnnouncement notifAnnouncement,
              Intent intent)
            {
              intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
              return intent;
            }

A hagyományos Engagement felhasználók számára vegye figyelembe, hogy rendszerüzenetek nélkül művelet URL-CÍMÉT most elindítja hello alkalmazást ha háttérben, így ez a metódus meghívható a bejelentés nélkül műveleti URL-cím. Vegye figyelembe, hogy hello leképezés testreszabásához.

Is megvalósíthatja `EngagementNotifier.executeNotifAnnouncementAction` teljesen.

##### <a name="notification-life-cycle"></a>Értesítési életciklusa
Hello alapértelmezett kategóriát használatakor a hello egyes életciklus metódusok nevezik `EngagementReachInteractiveContent` objektum tooreport statisztikák és a frissítés hello kampány állapota:

* Hello értesítés jelenik meg az alkalmazás vagy hello állapotsor be, amikor hello `displayNotification` módszer neve (amely statisztikáit) által `EngagementReachAgent` Ha `handleNotification` adja vissza `true`.
* Ha hello értesítési eltűnése hello `exitNotification` metódus lehívásra kerül, statisztika jelez, és tovább kampányok most már tudja feldolgozni.
* Ha hello értesítési kattint, `actionNotification` van nevű statisztika jelez, és a kapcsolódó hello leképezés lett elindítva.

Ha a végrehajtásának `EngagementNotifier` Mellőzés hello alapértelmezett viselkedés telepítette, toocall életciklus módszerekhez önállóan. hello a következő példák bemutatják, egyes esetekben, ahol hello alapértelmezett viselkedés elmarad:

* Nem terjeszti ki `EngagementDefaultNotifier`, pl. megvalósította a teljesen új kategória kezelését.
* A rendszer értesítések, üdvözlő overrode `onNotificationPrepared` és módosított `contentIntent` vagy `deleteIntent` a hello `Notification` objektum.
* Az alkalmazásbeli értesítések overrode `prepareInAppArea`, nem lehet rövidebb meg arról, hogy toomap `actionNotification` tooone a U.I vezérlők.

> [!NOTE]
> Ha `handleNotification` jelez kivétel, hello tartalom törlése és `dropContent` nevezik. Ez a statisztika jelez, és tovább kampányok most már tudja feldolgozni.
> 
> 

### <a name="announcements-and-polls"></a>Hirdetmények és szavazások esetén
#### <a name="layouts"></a>Elrendezés
Módosíthatja a hello `engagement_text_announcement.xml`, `engagement_web_announcement.xml` és `engagement_poll.xml` fájlok toocustomize szöveg közlemények, webes mutató hirdetmények és szavazások esetén.

Ezeket a fájlokat két közös elrendezések hello Címterület és hello gomb terület megosztani. hello elrendezését hello cím `engagement_content_title.xml` és felhasználási hello hello háttér városról rajzolható fájl. hello hello akciógombok és kilépési gombok elrendezése van `engagement_button_bar.xml` és felhasználási hello hello háttér városról rajzolható fájl.

A szavazás, hello kérdés elrendezés és a választási lehetőségek dinamikusan vannak növelve többször hello segítségével `engagement_question.xml` elrendezésfájlt hello kérdések és hello `engagement_choice.xml` fájl hello lehetőségekért.

#### <a name="categories"></a>Kategóriák
##### <a name="alternate-layouts"></a>Alternatív elrendezések
Értesítések, például a hello kampány kategória lehet használt toohave alternatív elrendezések a hirdetmények és szavazások esetén.

Például egy kategóriát a szöveg közlemény toocreate, kiterjesztheti `EngagementTextAnnouncementActivity` és hivatkozni rá hello `AndroidManifest.xml` fájlt:

            <activity android:name="com.your_company.MyCustomTextAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>

Vegye figyelembe a hello leképezés hello kategóriát szűrővel toomake hello különbség hello alapértelmezett közlemény tevékenységet.

hello Reach SDK egy adott konkrét kategóriával hello leképezési rendszer tooresolve hello tevékenységre használ, és azt visszavált hello alapértelmezett kategórián Ha hello feloldása nem sikerült.

Akkor el kell tooimplement `MyCustomTextAnnouncementActivity`, ha csak szeretné, hogy toochange hello elrendezés (, de tartsa hello azonos nézet azonosítók), csak rendelkezik toodefine hello osztály, például a következő példa hello:

            public class MyCustomTextAnnouncementActivity extends EngagementTextAnnouncementActivity
            {
              @Override
              protected String getLayoutName()
              {
                return "my_text_announcement";  // tell super class toouse R.layout.my_text_announcement
              }
            }

tooreplace hello alapértelmezett kategória szöveg hirdetmények, egyszerűen cserélje le `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` a megvalósítás.

Webes mutató hirdetmények és szavazások hasonló módon szabhatja testre.

Webes hirdetmények kiterjesztheti `EngagementWebAnnouncementActivity` és a tevékenységeket hello `AndroidManifest.xml` , például az alábbi példa hello:

            <activity android:name="com.your_company.MyCustomWebAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/html" />    <!-- only difference with text announcements in hello intent is hello data mime type -->
              </intent-filter>
            </activity>

A lekérdezések kiterjesztheti `EngagementPollActivity` és a a hello `AndroidManifest.xml` , például az alábbi példa hello:

            <activity android:name="com.your_company.MyCustomPollActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="my_category" />
              </intent-filter>
            </activity>

##### <a name="implementation-from-scratch"></a>Teljesen új végrehajtása
Megvalósíthat kategóriák a közlemény (és a lekérdezési) tevékenységek hello egyik kiterjesztése nélkül `Engagement*Activity` osztályok által biztosított hello Reach SDK. Ez akkor hasznos, például ha azt szeretné, toodefine nem használja, mint szabványos elrendezések hello azonos megtekinti hello elrendezés.

Például a speciális értesítési testreszabáshoz ajánlott: hello forráskódját hello szabványos implementációjától toolook.

Az alábbiakban néhány dolgot tookeep figyelembe vételével: Reach hello tevékenység egy adott biztonsági mentés (megfelelő toohello leképezési szűrő) fog elindulni, plusz egy kiegészítő paraméterrel, amely hello tartalmi azonosító.

tooretrieve hello tartalom objektum mikor hello létrehozása kampány hello webhelyen, megadott hello mezőket tartalmazó teheti meg:

            public class MyCustomTextAnnouncement extends EngagementActivity
            {
              private EngagementAnnouncement mContent;

              @Override
              protected void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);

                /* Get content */
                mContent = EngagementReachAgent.getInstance(this).getContent(getIntent());
                if (mContent == null)
                {
                  /* If problem with content, exit */
                  finish();
                  return;
                }

                setContentView(R.layout.my_text_announcement);

                /* Configure views by querying fields on mContent */
                // ...
              }
            }

A statisztika, jelentse hello tartalom hello jelenik meg `onResume` esemény:

            @Override
            protected void onResume()
            {
             /* Mark hello content displayed */
             mContent.displayContent(this);
             super.onResume();
            }

Ezt követően ne feledje toocall vagy `actionContent(this)` vagy `exitContent(this)` hello tartalom objektum előtt hello tevékenység háttér állapotba kerül.

Ha nem hívható meg vagy `actionContent` vagy `exitContent`, statisztika nem küldhető el (azaz nem elemzés hello kampány), és több ami még fontosabb – hello következő kampányok nem kap értesítést hello alkalmazás folyamatának újraindításáig.

Tájolás vagy egyéb konfigurációs módosítások végrehajtása is hello kód legbonyolultabb toodetermine ellenőrizze, hogy hello tevékenység háttér állapotba kerül, vagy nem, szabványos implementációjától teszi, hogy hello tartalom készként való kilépett hello felhasználó elhagyja hello tevékenység (vagy ha hello nyomja le `HOME` vagy `BACK`) kivéve, ha hello tájolás változik.

Itt része hello érdekes hello végrehajtására:

            @Override
            protected void onUserLeaveHint()
            {
              finish();
            }

            @Override
            protected void onPause()
            {
              if (isFinishing() && mContent != null)
              {
                /*
                 * Exit content on exit, this is has no effect if another process method has already been
                 * called so we don't have toocheck anything here.
                 */
                mContent.exitContent(this);
              }
              super.onPause();
            }

Ahogy látja, ha a hívott `actionContent(this)` hello tevékenység befejeződött, majd `exitContent(this)` anélkül, hogy bármilyen hatása biztonságosan hívható.

[here]:http://developer.android.com/tools/extras/support-library.html#Downloading
[Google Cloud Messaging]:http://developer.android.com/guide/google/gcm/index.html
[Amazon Device Messaging]:https://developer.amazon.com/sdk/adm.html
