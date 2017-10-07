---
title: "aaaGet Baidu segítségével Azure Notification Hubs használatába |} Microsoft Docs"
description: "Ebben az oktatóanyagban elsajátíthatja, hogyan toouse Azure Notification Hubs toopush értesítések tooAndroid eszközök Baidu segítségével."
services: notification-hubs
documentationcenter: android
author: ysxu
manager: erikre
editor: 
ms.assetid: 23bde1ea-f978-43b2-9eeb-bfd7b9edc4c1
ms.service: notification-hubs
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: mobile-baidu
ms.workload: mobile
ms.date: 08/19/2016
ms.author: yuaxu
ms.openlocfilehash: 2767fdd3bb04674e7a531634237cc05cd8c21cb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-notification-hubs-using-baidu"></a>Ismerkedés a Notification Hubs Baiduval való használatával
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Áttekintés
Felhőalapú baidu egy kínai felhőszolgáltatás toosend leküldéses értesítések toomobile eszközöket használhatja. Ez a szolgáltatás akkor hasznos, kínai, ahol különböző alkalmazás-áruházak és leküldési hello jelenléte miatt tooAndroid összetett leküldéses értesítések kézbesítéséhez szolgáltatásokat, továbbá Android-eszközök, amelyek nincsenek általában csatlakoztatott tooGCM (Google toohello rendelkezésre állása Cloud Messaging).

## <a name="prerequisites"></a>Előfeltételek
Az oktatóanyaghoz a következőkre lesz szükség:

* Android SDK (feltételezzük, hogy Eclipse használ), amelyre tölthető le: hello <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android webhelyéről</a>
* [Mobile Services Android SDK]
* [Baidu Push Android SDK]

> [!NOTE]
> toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F).
> 
> 

## <a name="create-a-baidu-account"></a>Baidu-fiók létrehozása
toouse Baidu, rendelkeznie kell a Baidu-fiók. Ha már rendelkezik egy, jelentkezzen be toohello [Baidu portálra] toohello következő lépés kihagyása. Ellenkező esetben tekintse meg az utasításoknak hogyan hello toocreate Baidu-fiók.  

1. Nyissa meg toohello [Baidu portálra] hello kattintson**登录**(**bejelentkezési**) hivatkozásra. Kattintson a**立即注册**toostart hello fiók regisztrációs folyamat során.
   
   ![][1]
2. Írja be a szükséges hello adatait – telefonszám/e-mail-cím, jelszó és Ellenőrzőkód – kattintson **előfizetési**.
   
   ![][2]
3. Kapnak az e-mailek toohello e-mail címet kell megadni a egy hivatkozás tooactivate a Baidu-fiók.
   
   ![][3]
4. Jelentkezzen be tooyour e-mail fiók, nyissa meg a hello Baidu aktiválási levelét, majd kattintson hello aktiválási hivatkozásra tooactivate a Baidu-fiók.
   
   ![][4]

Ha elvégezte a Baidu-fiók aktiválása, jelentkezzen be toohello [Baidu portálra].

## <a name="register-as-a-baidu-developer"></a>Regisztráció Baidu-fejlesztőként
1. Ha már bejelentkezett toohello [Baidu portálra], kattintson a**更多 >>** (**további**).
   
      ![][5]
2. Görgessen le a hello**站长与开发者服务 (gazdáját és fejlesztői szolgáltatások)** szakaszt, és kattintson**百度开放云平台**(**Baidu nyissa meg a felhő platform**).
   
      ![][6]
3. Hello következő lapon kattintson a**开发者服务**(**fejlesztői szolgáltatások**) hello jobb felső sarokban.
   
      ![][7]
4. Hello következő lapon kattintson a**注册开发者**(**regisztrált fejlesztők**) hello jobb felső sarokban hello menüjéből.
   
      ![][8]
5. Adja meg a nevét, a leírást és a telefonszámot, amelyre meg kívánja kapni az ellenőrző SMS-t, majd kattintson az **送验证码** (**Ellenőrzőkód elküldése**) elemre. A nemzetközi telefonszám tooenclose hello országhívószám zárójelbe kell. Egyesült államokbeli telefonszámok esetén például a következőképpen: **(1)1234567890**.
   
      ![][9]
6. Ezután a rendszer elküld egy szöveges üzenetet t, ahogy az alábbi példa hello:
   
      ![][10]
7. Hello ellenőrzési számot írja a üdvözlőüzenetére**验证码**(**megerősítő kód**).
8. Végezetül regisztrálást hello fejlesztői hello Baidu szerződés elfogadását jelző, majd**提交**(**Submit**). Hello oldalon, a regisztráció sikeres befejezését követően jelenik meg:
   
      ![][11]

## <a name="create-a-baidu-cloud-push-project"></a>Felhőalapú Baidu-értesítési projekt létrehozása
Felhőalapú Baidu-értesítési projekt létrehozásakor megkapja az alkalmazásazonosítóját, az API-kulcsot és a titkos kulcsot.

1. Ha már bejelentkezett toohello [Baidu portálra], kattintson a**更多 >>** (**további**).
   
      ![][5]
2. Görgessen le a hello**站长与开发者服务**(**gazdáját és fejlesztői szolgáltatások**) szakaszt, és kattintson**百度开放云平台**(**Baidu nyissa meg a felhő platform**).
   
      ![][6]
3. Hello következő lapon kattintson a**开发者服务**(**fejlesztői szolgáltatások**) hello jobb felső sarokban.
   
      ![][7]
4. Hello következő lapon kattintson a**云推送**(**felhő leküldéses**) a hello**云服务**(**Felhőszolgáltatások**) szakaszban.
   
      ![][12]
5. Ha egy regisztrált fejlesztők, tekintse meg a**管理控制台**(**felügyeleti konzol**): hello felső menüjében. Kattintson a **开发者服务管理** (**Fejlesztői szolgáltatások kezelése**) parancsra.
   
      ![][13]
6. Hello következő lapon kattintson a**创建工程**(**projekt létrehozása**).
   
      ![][14]
7. Adjon meg egy alkalmazásnevet, majd kattintson a **创建** (**Létrehozás**) parancsra.
   
      ![][15]
8. A felhőalapú Baidu-értesítési projekt sikeres létrehozása után megjelenő oldalon megtalálja az **alkalmazásazonosítót**, az **API-kulcsot** és a **titkos kulcsot**. Jegyezze fel a hello API-kulcsot és titkos kulcsot, mert később fogjuk használni.
   
      ![][16]
9. Leküldéses értesítések hello projekt konfigurálásához kattintva**云推送**(**felhő leküldéses**) hello bal oldali ablaktáblán.
   
      ![][31]
10. Hello következő lapon kattintson a hello**推送设置**(**beállítások leküldéses**) gombra.
    
    ![][32]  
11. A hello konfiguráció lapon vegye fel a hello csomag neve, amely az Android projekt a hello fog használni**应用包名**(**alkalmazáscsomag**) mezőben, majd kattintson a**保存设置**() **Mentése**).  
    
    ![][33]

Megjelenik a hello**保存成功!** (**Sikeresen mentve!**) üzenet.

## <a name="configure-your-notification-hub"></a>Az értesítési központ konfigurálása
1. Jelentkezzen be toohello [klasszikus Azure portál], és kattintson a **+ új** üdvözlő képernyőt hello alján.
2. Kattintson az **App Services**, a **Service Bus**, a **Notification Hub**, végül pedig a **Gyors létrehozás** elemre.
3. Adjon meg egy nevet a **értesítési központ**, jelölje be hello **régió** és hello **Namespace** ahol az értesítési központ jön létre, és kattintson  **Hozzon létre egy új értesítési központ**.  
   
      ![][17]
4. Kattintson a hello névtér, amelyben létrehozta az értesítési központot, majd **Notification Hubs** hello tetején.
   
      ![][18]
5. Létrehozott, és kattintson a kijelölés hello értesítési központ **konfigurálása** hello felső menüjében.
   
      ![][19]
6. Görgessen lefelé toohello **baidu-értesítési beállításainak** szakaszt, és adja meg a hello API és beszerzett hello Baidu-konzolon korábban a Baidu felhőalapú leküldéses értesítési projektet a titkos kulcsot. Kattintson a **Save** (Mentés) gombra.
   
      ![][20]
7. Kattintson a hello **irányítópult** hello felső hello értesítési központ fülre, majd **kapcsolati karakterlánc megtekintése**.
   
      ![][21]
8. Jegyezze fel a hello **DefaultListenSharedAccessSignature** és **DefaultFullSharedAccessSignature** a hello **kapcsolati adatok elérése** ablak.
   
    ![][22]

## <a name="connect-your-app-toohello-notification-hub"></a>Csatlakozás az alkalmazás toohello értesítési központ
1. Az Eclipse ADT-ben hozzon létre egy új Android-projektet (**File** (Fájl)  > **New** (Új)  > **Android Application Project** (Android-alkalmazásprojekt)).
   
    ![][23]
2. Adjon meg egy **alkalmazásnév** , és győződjön meg arról, hogy hello **minimálisan szükséges SDK** verzió beállítása túl**API 16: Android 4.1**.
   
    ![][24]
3. Kattintson a **következő** , és folytassa a következő hello varázsló amíg hello **tevékenység létrehozása** ablak jelenik meg. Győződjön meg arról, hogy **üres tevékenység** kiválasztva, és végül válassza **Befejezés** toocreate egy új Android-alkalmazás.
   
    ![][25]
4. Győződjön meg arról, hogy hello **Project Build Target** megfelelően van beállítva.
   
    ![][26]
5. Töltse le hello notification-hubs-0.4.jar fájlt hello **fájlok** hello lapján [Notification-Hubs-Android-SDK a Files](https://bintray.com/microsoftazuremobile/SDK/Notification-Hubs-Android-SDK/0.4). Adja hozzá a hello fájl toohello **függvénytárak** mappa az Eclipse-projekt, és a frissítési hello *függvénytárak* mappát.
6. Töltse le és csomagolja ki a hello [Baidu Push Android SDK], nyissa meg hello **függvénytárak** mappát, majd a Másolás hello **pushservice-x.y.z** fájl- és hello jar **armeabi**  &  **mips** hello mappák **függvénytárak** az Android-alkalmazás mappájában.
7. Nyissa meg hello **AndroidManifest.xml** fájl az Android projektre, majd adja hozzá a hello Baidu SDK által igényelt hello engedélyeket.
   
        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.WRITE_SETTINGS" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
        <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />
8. Adja hozzá a hello **android: name** tulajdonság tooyour **alkalmazás** elemében **AndroidManifest.xml**, ahol *com.example.baidutest* (a a példában **com.example.BaiduTest**). Győződjön meg arról, hogy ez a projektnév megegyezik-e a hello egy hello Baidu-konzolon konfigurált.
   
        <application android:name="yourprojectname.DemoApplication"
9. Adja hozzá a következő konfigurációs hello alkalmazás elemen belül hello után hello **. MainActivity** tevékenységelem cseréje *com.example.baidutest* (például **com.example.BaiduTest**):
   
        <receiver android:name="yourprojectname.MyPushMessageReceiver">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
                <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
                <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
            </intent-filter>
        </receiver>
   
        <receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
            </intent-filter>
        </receiver>
   
        <receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.METHOD" />
                <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_REMOVED"/>
                <data android:scheme="package" />
            </intent-filter>
        </receiver>
   
        <service
            android:name="com.baidu.android.pushservice.PushService"
            android:exported="true"
            android:process=":bdservice_v1"  >
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
            </intent-filter>
        </service>
10. Adja hozzá egy új osztályt **ConfigurationSettings.java** toohello projekt.
    
     ![][28]
    
     ![][29]
11. Adja hozzá a következő kód tooit hello:
    
        public class ConfigurationSettings {
                public static String API_KEY = "...";
                public static String NotificationHubName = "...";
                public static String NotificationHubConnectionString = "...";
            }
    
    Állítsa be hello **API_KEY** lekért hello Baidu felhőalapú projektet korábban, a **NotificationHubName** hello klasszikus Azure portálról származó értesítésiközpont-nevet a és  **NotificationHubConnectionString** és a klasszikus Azure portál hello a defaultlistensharedaccesssignature értéket.
12. Adja hozzá egy új osztályt **DemoApplication.java**, és adja hozzá a következő kód tooit hello:
    
        import com.baidu.frontia.FrontiaApplication;
    
        public class DemoApplication extends FrontiaApplication {
            @Override
            public void onCreate() {
                super.onCreate();
            }
        }
13. Adja hozzá egy másik új osztályt **MyPushMessageReceiver.java**, és adja hozzá a következő kód tooit hello. Leírók hello hello Baidu leküldési kiszolgálóról kapott leküldéses értesítések hello osztály.
    
        import java.util.List;
        import android.content.Context;
        import android.os.AsyncTask;
        import android.util.Log;
        import com.baidu.frontia.api.FrontiaPushMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub;
    
        public class MyPushMessageReceiver extends FrontiaPushMessageReceiver {
            /** TAG tooLog */
            public static NotificationHub hub = null;
            public static String mChannelId, mUserId;
            public static final String TAG = MyPushMessageReceiver.class
                    .getSimpleName();
    
            @Override
            public void onBind(Context context, int errorCode, String appid,
                    String userId, String channelId, String requestId) {
                String responseString = "onBind errorCode=" + errorCode + " appid="
                        + appid + " userId=" + userId + " channelId=" + channelId
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
                mChannelId = channelId;
                mUserId = userId;
    
                try {
                    if (hub == null) {
                        hub = new NotificationHub(
                                ConfigurationSettings.NotificationHubName,
                                ConfigurationSettings.NotificationHubConnectionString,
                                context);
                        Log.i(TAG, "Notification hub initialized");
                    }
                } catch (Exception e) {
                   Log.e(TAG, e.getMessage());
                }
    
                registerWithNotificationHubs();
            }
    
            private void registerWithNotificationHubs() {
               new AsyncTask<Void, Void, Void>() {
                  @Override
                  protected Void doInBackground(Void... params) {
                     try {
                         hub.registerBaidu(mUserId, mChannelId);
                         Log.i(TAG, "Registered with Notification Hub - '"
                                 + ConfigurationSettings.NotificationHubName + "'"
                                 + " with UserId - '"
                                 + mUserId + "' and Channel Id - '"
                                 + mChannelId + "'");
                     } catch (Exception e) {
                         Log.e(TAG, e.getMessage());
                     }
                     return null;
                 }
               }.execute(null, null, null);
            }
    
            @Override
            public void onSetTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onSetTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }
    
            @Override
            public void onDelTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onDelTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }
    
            @Override
            public void onListTags(Context context, int errorCode, List<String> tags,
                    String requestId) {
                String responseString = "onListTags errorCode=" + errorCode + " tags="
                        + tags;
                Log.d(TAG, responseString);
            }
    
            @Override
            public void onUnbind(Context context, int errorCode, String requestId) {
                String responseString = "onUnbind errorCode=" + errorCode
                        + " requestId = " + requestId;
                Log.d(TAG, responseString);
            }
    
            @Override
            public void onNotificationClicked(Context context, String title,
                    String description, String customContentString) {
                String notifyString = "title=\"" + title + "\" description=\""
                        + description + "\" customContent=" + customContentString;
                Log.d(TAG, notifyString);
            }
    
            @Override
            public void onMessage(Context context, String message,
                    String customContentString) {
                String messageString = "message=\"" + message + "\" customContentString=" + customContentString;
                Log.d(TAG, messageString);
            }
        }
14. Nyissa meg **MainActivity.java**, és adja hozzá a következő toohello hello **onCreate** módszert:
    
            PushManager.startWork(getApplicationContext(),
                    PushConstants.LOGIN_TYPE_API_KEY, ConfigurationSettings.API_KEY);
15. Nyissa meg a következő importálási utasításokat hello felső hello:
    
            import com.baidu.android.pushservice.PushConstants;
            import com.baidu.android.pushservice.PushManager;

## <a name="send-notifications-tooyour-app"></a>Értesítések tooyour app küldése
Gyorsan tesztelheti értesítések fogadásának az alkalmazásban való értesítések a hello [Azure-portálon](https://portal.azure.com/) hello segítségével **küldése** hello értesítési központot, ahogy az a következő képernyő hello gombra:

![](./media/notification-hubs-baidu-get-started/notification-hub-test-send-baidu.png)

A leküldéses értesítések küldése általában olyan háttérszolgáltatásokon keresztül történik egy kompatibilis kódtár használatával, mint a Mobile Services vagy az ASP.NET. Ha a szalagtár nem érhető el a háttér-, hello REST API-t használhatja közvetlenül a toosend értesítési üzeneteket.

Az oktatóanyag azt legyen egyszerű és csak az ügyfélalkalmazás tesztelését küldött értesítésekkel hello .NET SDK használatával egy háttér-szolgáltatás helyett egy konzolalkalmazás értesítési központjának bemutatása. Azt javasoljuk, hogy hello [Notification Hubs használata toopush értesítések toousers](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) oktatóanyagot hello következő lépés az értesítéseknek ASP.NET-háttérrendszerből történő küldéséhez. A következő módszerekkel hello azonban az értesítések küldésével használhatók:

* **REST-felület**: bármely háttér platformon hello segítségével támogathatja az értesítéseket [REST-felület](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).
* **A Microsoft Azure Notification Hubs .NET SDK**: hello Nuget-Csomagkezelőt a Visual Studio, a futtasson [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).
* **NODE.js**: [hogyan toouse Notification Hubs Node.js](notification-hubs-nodejs-push-notification-tutorial.md).
* **Mobile Apps**: A példa bemutatja, hogyan toosend értesítések a Notification Hubs szolgáltatással integrált Azure App Service Mobile Apps-háttérrendszerből lásd [Hozzáadás leküldéses értesítések tooyour mobilalkalmazás](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).
* **Java / PHP**: hogyan toosend értesítések használatával hello REST API-k példát lásd: "hogyan toouse Notification Hubs Java/php-ből" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

## <a name="optional-send-notifications-from-a-net-console-app"></a>(Nem kötelező) Értesítések küldése .NET-konzolalkalmazásból.
Ebben a szakaszban az értesítések .NET-konzolalkalmazásból történő küldését mutatjuk be.

1. Hozzon létre egy új Visual C#-konzolalkalmazást:
   
    ![][30]
2. Hello Package Manager Console ablakban, állítson be hello **alapértelmezett projekt** tooyour új projekt konzolról, és majd hello konzolablakban hajtható végre a következő parancs hello:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Ezeket az utasításokat ad a hivatkozás toohello Azure Notification Hubs SDK használatával hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-csomag</a>.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
3. Megnyitás hello fájl **Program.cs** és adja hozzá hello következő using utasítást:
   
        using Microsoft.Azure.NotificationHubs;
4. Az a `Program` osztályban adja hozzá a következő metódus hello és cserélje le *DefaultFullSharedAccessSignatureSASConnectionString* és *NotificationHubName* hello értékekkel rendelkezik.
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("DefaultFullSharedAccessSignatureSASConnectionString", "NotificationHubName");
            string message = "{\"title\":\"((Notification title))\",\"description\":\"Hello from Azure\"}";
            var result = await hub.SendBaiduNativeNotificationAsync(message);
        }
5. Adja hozzá az alábbi hello a **fő** módszert:
   
         SendNotificationAsync();
         Console.ReadLine();

## <a name="test-your-app"></a>Az alkalmazás tesztelése
Ez az alkalmazás egy valódi telefonon, egyszerűen csatlakoztassa tootest hello phone tooyour számítógép USB-kábelen keresztül. Ez a művelet betölti az alkalmazás feltöltődik a csatlakoztatott hello phone.

tootest hello emulátorral hello Eclipse felső eszköztárán, az alkalmazás kattintson **futtatása**, majd válassza ki az alkalmazást: hello emulátor, terhelés esetén kezdődik, és futtat hello alkalmazás.

hello az alkalmazás lekéri hello "userId" és a "channelId" hello Baidu leküldéses értesítéseket kezelő szolgáltatása, és regisztrálja hello értesítési központban.

tesztértesítés toosend, hello klasszikus Azure portál hibakeresési lapjáról hello is használhatja. Ha a hello .NET konzolalkalmazást a Visual Studio, csak Entert kell hello F5 billentyűt a Visual Studio toorun hello alkalmazásban. hello alkalmazás hello az eszköz vagy az emulátor felső értesítési területén megjelenő értesítést küld.

<!-- Images. -->
[1]: ./media/notification-hubs-baidu-get-started/BaiduRegistration.png
[2]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationInput.png
[3]: ./media/notification-hubs-baidu-get-started/BaiduConfirmation.png
[4]: ./media/notification-hubs-baidu-get-started/BaiduActivationEmail.png
[5]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationMore.png
[6]: ./media/notification-hubs-baidu-get-started/BaiduOpenCloudPlatform.png
[7]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperServices.png
[8]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperRegistration.png
[9]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationInput.png
[10]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationConfirmation.png
[11]: ./media/notification-hubs-baidu-get-started/BaiduDevConfirmationFinal.png
[12]: ./media/notification-hubs-baidu-get-started/BaiduCloudPush.png
[13]: ./media/notification-hubs-baidu-get-started/BaiduDevSvcMgmt.png
[14]: ./media/notification-hubs-baidu-get-started/BaiduCreateProject.png
[15]: ./media/notification-hubs-baidu-get-started/BaiduCreateProjectInput.png
[16]: ./media/notification-hubs-baidu-get-started/BaiduProjectKeys.png
[17]: ./media/notification-hubs-baidu-get-started/AzureNHCreation.png
[18]: ./media/notification-hubs-baidu-get-started/NotificationHubs.png
[19]: ./media/notification-hubs-baidu-get-started/NotificationHubsConfigure.png
[20]: ./media/notification-hubs-baidu-get-started/NotificationHubBaiduConfigure.png
[21]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionStringView.png
[22]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionString.png
[23]: ./media/notification-hubs-baidu-get-started/EclipseNewProject.png
[24]: ./media/notification-hubs-baidu-get-started/EclipseProjectCreation.png
[25]: ./media/notification-hubs-baidu-get-started/EclipseBlankActivity.png
[26]: ./media/notification-hubs-baidu-get-started/EclipseProjectBuildProperty.png
[27]: ./media/notification-hubs-baidu-get-started/EclipseBaiduReferences.png
[28]: ./media/notification-hubs-baidu-get-started/EclipseNewClass.png
[29]: ./media/notification-hubs-baidu-get-started/EclipseConfigSettingsClass.png
[30]: ./media/notification-hubs-baidu-get-started/ConsoleProject.png
[31]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig1.png
[32]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig2.png
[33]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig3.png

<!-- URLs. -->
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Baidu Push Android SDK]: http://developer.baidu.com/wiki/index.php?title=docs/cplat/push/sdk/clientsdk
[klasszikus Azure portál]: https://manage.windowsazure.com/
[Baidu portálra]: http://www.baidu.com/
