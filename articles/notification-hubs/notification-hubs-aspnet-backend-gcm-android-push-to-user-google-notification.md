---
title: "Notification Hubs – felhasználók értesítése Android aaaAzure .NET-háttérrendszerrel"
description: "Ismerje meg, toosend a leküldéses értesítések toousers az Azure-ban. Androidhoz készült Java nyelven írt mintakódok"
documentationcenter: android
services: notification-hubs
author: ysxu
manager: erikre
editor: 
ms.assetid: ae0e17a8-9d2b-496e-afd2-baa151370c25
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: b042d2e6fb7f7c861c378526a8a0d59ab75beef9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs-notify-users-for-android-with-net-backend"></a><span data-ttu-id="27f24-104">Az Azure Notification Hubs – felhasználók értesítése .NET-háttérrendszerrel Android rendszerhez</span><span class="sxs-lookup"><span data-stu-id="27f24-104">Azure Notification Hubs Notify Users for Android with .NET backend</span></span>
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a><span data-ttu-id="27f24-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="27f24-105">Overview</span></span>
<span data-ttu-id="27f24-106">Leküldéses értesítési támogatás az Azure-ban lehetővé teszi tooaccess egy könnyen kezelhető, multiplatform és kibővített leküldéses infrastruktúrában, ami jelentősen egyszerűbb a leküldéses értesítések a mobile fogyasztói, valamint a vállalati alkalmazások hello végrehajtása platformok.</span><span class="sxs-lookup"><span data-stu-id="27f24-106">Push notification support in Azure enables you tooaccess an easy-to-use, multiplatform, and scaled-out push infrastructure, which greatly simplifies hello implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span> <span data-ttu-id="27f24-107">Ez az oktatóanyag toouse Azure Notification Hubs toosend a leküldéses értesítések tooa adott alkalmazás felhasználói az adott eszközön.</span><span class="sxs-lookup"><span data-stu-id="27f24-107">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooa specific app user on a specific device.</span></span> <span data-ttu-id="27f24-108">ASP.NET WebAPI háttérrendszerből használt tooauthenticate ügyfelek és toogenerate értesítéseket, ahogy hello útmutatást a témakör az [az alkalmazás háttérrendszeréből regisztrálása](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).</span><span class="sxs-lookup"><span data-stu-id="27f24-108">An ASP.NET WebAPI backend is used tooauthenticate clients and toogenerate notifications, as shown in hello guidance topic [Registering from your app backend](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).</span></span> <span data-ttu-id="27f24-109">Ez az oktatóanyag épít, hello hello létrehozott értesítési központot [Ismerkedés a Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="27f24-109">This tutorial builds on hello notification hub that you created in hello [Getting Started with Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md) tutorial.</span></span>

> [!NOTE]
> <span data-ttu-id="27f24-110">Ez az oktatóanyag feltételezi, hogy létrehozta és leírtak szerint konfigurálta az értesítési központ [Ismerkedés a Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="27f24-110">This tutorial assumes that you have created and configured your notification hub as described in [Getting Started with Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).</span></span>
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="create-hello-android-project"></a><span data-ttu-id="27f24-111">Hello Android-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="27f24-111">Create hello Android Project</span></span>
<span data-ttu-id="27f24-112">következő lépés hello toocreate hello Android-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="27f24-112">hello next step is toocreate hello Android application.</span></span>

1. <span data-ttu-id="27f24-113">Hajtsa végre a hello [Ismerkedés a Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md) oktatóanyag toocreate, és konfigurálja az alkalmazás tooreceive leküldéses értesítéseket a gcm-től.</span><span class="sxs-lookup"><span data-stu-id="27f24-113">Follow hello [Getting Started with Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md) tutorial toocreate and configure your app tooreceive push notifications from GCM.</span></span>
2. <span data-ttu-id="27f24-114">Nyissa meg a **res/layout/activity_main.xml** fájlt, hello cserélje le a következő tartalom definíciók hello.</span><span class="sxs-lookup"><span data-stu-id="27f24-114">Open your **res/layout/activity_main.xml** file, replace hello with hello following content definitions.</span></span>
   
    <span data-ttu-id="27f24-115">Ez biztosítja, hogy új EditText vezérlők felhasználóként van bejelentkezve.</span><span class="sxs-lookup"><span data-stu-id="27f24-115">This adds new EditText controls for logging in as a user.</span></span> <span data-ttu-id="27f24-116">Is mező a rendszer ad hozzá egy felhasználónév tag, értesítést küldünk része lesz:</span><span class="sxs-lookup"><span data-stu-id="27f24-116">Also a field is added for a username tag that will be part of notifications you send:</span></span>
   
        <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
            android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            android:paddingBottom="@dimen/activity_vertical_margin" tools:context=".MainActivity">
   
        <EditText
            android:id="@+id/usernameText"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:ems="10"
            android:hint="@string/usernameHint"
            android:layout_above="@+id/passwordText"
            android:layout_alignParentEnd="true" />
        <EditText
            android:id="@+id/passwordText"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:ems="10"
            android:hint="@string/passwordHint"
            android:inputType="textPassword"
            android:layout_above="@+id/buttonLogin"
            android:layout_alignParentEnd="true" />
        <Button
            android:id="@+id/buttonLogin"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/loginButton"
            android:onClick="login"
            android:layout_above="@+id/toggleButtonGCM"
            android:layout_centerHorizontal="true"
            android:layout_marginBottom="24dp" />
        <ToggleButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textOn="WNS on"
            android:textOff="WNS off"
            android:id="@+id/toggleButtonWNS"
            android:layout_toLeftOf="@id/toggleButtonGCM"
            android:layout_centerVertical="true" />
        <ToggleButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textOn="GCM on"
            android:textOff="GCM off"
            android:id="@+id/toggleButtonGCM"
            android:checked="true"
            android:layout_centerHorizontal="true"
            android:layout_centerVertical="true" />
        <ToggleButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textOn="APNS on"
            android:textOff="APNS off"
            android:id="@+id/toggleButtonAPNS"
            android:layout_toRightOf="@id/toggleButtonGCM"
            android:layout_centerVertical="true" />
        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/editTextNotificationMessageTag"
            android:layout_below="@id/toggleButtonGCM"
            android:layout_centerHorizontal="true"
            android:hint="@string/notification_message_tag_hint" />
        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/editTextNotificationMessage"
            android:layout_below="@+id/editTextNotificationMessageTag"
            android:layout_centerHorizontal="true"
            android:hint="@string/notification_message_hint" />
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/send_button"
            android:id="@+id/sendbutton"
            android:onClick="sendNotificationButtonOnClick"
            android:layout_below="@+id/editTextNotificationMessage"
            android:layout_centerHorizontal="true" />
        </RelativeLayout>
3. <span data-ttu-id="27f24-117">Nyissa meg a **res/values/strings.xml** fájlt, és cserélje le a hello `send_button` hello következőre definition sorok hello az újradefiniálás hello karakterláncokat `send_button` és hello karakterláncok más vezérlők hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="27f24-117">Open your **res/values/strings.xml** file and replace hello `send_button` definition with hello following lines that redefine hello string for hello `send_button` and add strings for hello other controls:</span></span>
   
        <string name="usernameHint">Username</string>
        <string name="passwordHint">Password</string>
        <string name="loginButton">1. Log in</string>
        <string name="send_button">2. Send Notification</string>
        <string name="notification_message_tag_hint">
            Recipient username tag
        </string>
   
    <span data-ttu-id="27f24-118">A main_activity.xml grafikus elrendezés most példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="27f24-118">Your main_activity.xml graphical layout should now look like this:</span></span>
   
    ![][A1]
4. <span data-ttu-id="27f24-119">Hozzon létre egy új osztályt **RegisterClient** a hello azonos csomag a `MainActivity` osztály.</span><span class="sxs-lookup"><span data-stu-id="27f24-119">Create a new class named **RegisterClient** in hello same package as your `MainActivity` class.</span></span> <span data-ttu-id="27f24-120">Használja az alábbi hello kódot hello új osztály fájl.</span><span class="sxs-lookup"><span data-stu-id="27f24-120">Use hello code below for hello new class file.</span></span>
   
        import java.io.IOException;
        import java.io.UnsupportedEncodingException;
        import java.util.Set;
   
        import org.apache.http.HttpResponse;
        import org.apache.http.HttpStatus;
        import org.apache.http.client.ClientProtocolException;
        import org.apache.http.client.HttpClient;
        import org.apache.http.client.methods.HttpPost;
        import org.apache.http.client.methods.HttpPut;
        import org.apache.http.client.methods.HttpUriRequest;
        import org.apache.http.entity.StringEntity;
        import org.apache.http.impl.client.DefaultHttpClient;
        import org.apache.http.util.EntityUtils;
        import org.json.JSONArray;
        import org.json.JSONException;
        import org.json.JSONObject;
   
        import android.content.Context;
        import android.content.SharedPreferences;
        import android.util.Log;
   
        public class RegisterClient {
            private static final String PREFS_NAME = "ANHSettings";
            private static final String REGID_SETTING_NAME = "ANHRegistrationId";
            private String Backend_Endpoint;
            SharedPreferences settings;
            protected HttpClient httpClient;
            private String authorizationHeader;
   
            public RegisterClient(Context context, String backendEnpoint) {
                super();
                this.settings = context.getSharedPreferences(PREFS_NAME, 0);
                httpClient =  new DefaultHttpClient();
                Backend_Endpoint = backendEnpoint + "/api/register";
            }
   
            public String getAuthorizationHeader() {
                return authorizationHeader;
            }
   
            public void setAuthorizationHeader(String authorizationHeader) {
                this.authorizationHeader = authorizationHeader;
            }
   
            public void register(String handle, Set<String> tags) throws ClientProtocolException, IOException, JSONException {
                String registrationId = retrieveRegistrationIdOrRequestNewOne(handle);
   
                JSONObject deviceInfo = new JSONObject();
                deviceInfo.put("Platform", "gcm");
                deviceInfo.put("Handle", handle);
                deviceInfo.put("Tags", new JSONArray(tags));
   
                int statusCode = upsertRegistration(registrationId, deviceInfo);
   
                if (statusCode == HttpStatus.SC_OK) {
                    return;
                } else if (statusCode == HttpStatus.SC_GONE){
                    settings.edit().remove(REGID_SETTING_NAME).commit();
                    registrationId = retrieveRegistrationIdOrRequestNewOne(handle);
                    statusCode = upsertRegistration(registrationId, deviceInfo);
                    if (statusCode != HttpStatus.SC_OK) {
                        Log.e("RegisterClient", "Error upserting registration: " + statusCode);
                        throw new RuntimeException("Error upserting registration");
                    }
                } else {
                    Log.e("RegisterClient", "Error upserting registration: " + statusCode);
                    throw new RuntimeException("Error upserting registration");
                }
            }
   
            private int upsertRegistration(String registrationId, JSONObject deviceInfo)
                    throws UnsupportedEncodingException, IOException,
                    ClientProtocolException {
                HttpPut request = new HttpPut(Backend_Endpoint+"/"+registrationId);
                request.setEntity(new StringEntity(deviceInfo.toString()));
                request.addHeader("Authorization", "Basic "+authorizationHeader);
                request.addHeader("Content-Type", "application/json");
                HttpResponse response = httpClient.execute(request);
                int statusCode = response.getStatusLine().getStatusCode();
                return statusCode;
            }
   
            private String retrieveRegistrationIdOrRequestNewOne(String handle) throws ClientProtocolException, IOException {
                if (settings.contains(REGID_SETTING_NAME))
                    return settings.getString(REGID_SETTING_NAME, null);
   
                HttpUriRequest request = new HttpPost(Backend_Endpoint+"?handle="+handle);
                request.addHeader("Authorization", "Basic "+authorizationHeader);
                HttpResponse response = httpClient.execute(request);
                if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                    Log.e("RegisterClient", "Error creating registrationId: " + response.getStatusLine().getStatusCode());
                    throw new RuntimeException("Error creating Notification Hubs registrationId");
                }
                String registrationId = EntityUtils.toString(response.getEntity());
                registrationId = registrationId.substring(1, registrationId.length()-1);
   
                settings.edit().putString(REGID_SETTING_NAME, registrationId).commit();
   
                return registrationId;
            }
        }
   
    <span data-ttu-id="27f24-121">Ez az összetevő hello REST hívások szükséges toocontact hello háttéralkalmazás, a rendelés tooregister a leküldéses értesítések valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="27f24-121">This component implements hello REST calls required toocontact hello app backend, in order tooregister for push notifications.</span></span> <span data-ttu-id="27f24-122">Helyileg tárolja a hello *registrationIds* hello által létrehozott értesítési központot, ahogy az az [az alkalmazás háttérrendszeréből regisztrálása](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).</span><span class="sxs-lookup"><span data-stu-id="27f24-122">It also locally stores hello *registrationIds* created by hello Notification Hub as detailed in [Registering from your app backend](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).</span></span> <span data-ttu-id="27f24-123">Vegye figyelembe, hogy használja-e egy engedélyezési jogkivonatot hello kattintva helyi storage-ban tárolt **jelentkezzen be** gombra.</span><span class="sxs-lookup"><span data-stu-id="27f24-123">Note that it uses an authorization token stored in local storage when you click hello **Log in** button.</span></span>
5. <span data-ttu-id="27f24-124">Az a `MainActivity` osztály távolítsa el, vagy a privát mezője megjegyzéssé `NotificationHub`, és hello mező felvétele `RegisterClient` osztály és az ASP.NET-háttérrendszerből végpont karakterláncnak.</span><span class="sxs-lookup"><span data-stu-id="27f24-124">In your `MainActivity` class remove or comment out your private field for `NotificationHub`, and add a field for hello `RegisterClient` class and a string for your ASP.NET backend's endpoint.</span></span> <span data-ttu-id="27f24-125">Lehet, hogy tooreplace `<Enter Your Backend Endpoint>` a hello a tényleges háttér-végpontot szerezte be korábban.</span><span class="sxs-lookup"><span data-stu-id="27f24-125">Be sure tooreplace `<Enter Your Backend Endpoint>` with hello your actual backend endpoint obtained previously.</span></span> <span data-ttu-id="27f24-126">Például: `http://mybackend.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="27f24-126">For example, `http://mybackend.azurewebsites.net`.</span></span>

        //private NotificationHub hub;
        private RegisterClient registerClient;
        private static final String BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";


1. <span data-ttu-id="27f24-127">Az a `MainActivity` osztály hello `onCreate` módszer, távolítsa el vagy hello hello inicializálása megjegyzéssé `hub` mező és hello hívás toohello `registerWithNotificationHubs` metódus.</span><span class="sxs-lookup"><span data-stu-id="27f24-127">In your `MainActivity` class, in hello `onCreate` method, remove or comment out hello initialization of hello `hub` field and hello call toohello `registerWithNotificationHubs` method.</span></span> <span data-ttu-id="27f24-128">Majd adja hozzá a kódot tooinitialize hello példányának `RegisterClient` osztály.</span><span class="sxs-lookup"><span data-stu-id="27f24-128">Then add code tooinitialize an instance of hello `RegisterClient` class.</span></span> <span data-ttu-id="27f24-129">hello metódus hello a következő sorokat kell tartalmaznia:</span><span class="sxs-lookup"><span data-stu-id="27f24-129">hello method should contain hello following lines:</span></span>
   
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
   
            MyHandler.mainActivity = this;
            NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);
            gcm = GoogleCloudMessaging.getInstance(this);
   
            //hub = new NotificationHub(HubName, HubListenConnectionString, this);
            //registerWithNotificationHubs();
   
            registerClient = new RegisterClient(this, BACKEND_ENDPOINT);
   
            setContentView(R.layout.activity_main);
        }
2. <span data-ttu-id="27f24-130">Az a `MainActivity` osztály, törölje vagy teljes hello megjegyzéssé `registerWithNotificationHubs` metódust.</span><span class="sxs-lookup"><span data-stu-id="27f24-130">In your `MainActivity` class, delete or comment out hello entire `registerWithNotificationHubs` method.</span></span> <span data-ttu-id="27f24-131">Ez az oktatóanyag nem használható.</span><span class="sxs-lookup"><span data-stu-id="27f24-131">It will not be used in this tutorial.</span></span>
3. <span data-ttu-id="27f24-132">Adja hozzá a következő hello `import` utasítások tooyour **MainActivity.java** fájlt.</span><span class="sxs-lookup"><span data-stu-id="27f24-132">Add hello following `import` statements tooyour **MainActivity.java** file.</span></span>
   
        import android.widget.Button;
        import java.io.UnsupportedEncodingException;
        import android.content.Context;
        import java.util.HashSet;
        import android.widget.Toast;
        import org.apache.http.client.ClientProtocolException;
        import java.io.IOException;
        import org.apache.http.HttpStatus;
4. <span data-ttu-id="27f24-133">Adja hozzá a következő módszerek toohandle hello hello **jelentkezzen be** gombra kattintson esemény és leküldéses értesítések küldése.</span><span class="sxs-lookup"><span data-stu-id="27f24-133">Then, add hello following methods toohandle hello **Log in** button click event and sending push notifications.</span></span>
   
        @Override
        protected void onStart() {
            super.onStart();
            Button sendPush = (Button) findViewById(R.id.sendbutton);
            sendPush.setEnabled(false);
        }
   
        public void login(View view) throws UnsupportedEncodingException {
            this.registerClient.setAuthorizationHeader(getAuthorizationHeader());
   
            final Context context = this;
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        String regid = gcm.register(SENDER_ID);
                        registerClient.register(regid, new HashSet<String>());
                    } catch (Exception e) {
                        DialogNotify("MainActivity - Failed tooregister", e.getMessage());
                        return e;
                    }
                    return null;
                }
   
                protected void onPostExecute(Object result) {
                    Button sendPush = (Button) findViewById(R.id.sendbutton);
                    sendPush.setEnabled(true);
                    Toast.makeText(context, "Logged in and registered.",
                            Toast.LENGTH_LONG).show();
                }
            }.execute(null, null, null);
        }
   
        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);
            return basicAuthHeader;
        }
   
        /**
         * This method calls hello ASP.NET WebAPI backend toosend hello notification message
         * toohello platform notification service based on hello pns parameter.
         *
         * @param pns     hello platform notification service toosend hello notification message to. Must
         *                be one of hello following ("wns", "gcm", "apns").
         * @param userTag hello tag for hello user who will receive hello notification message. This string
         *                must not contain spaces or special characters.
         * @param message hello notification message string. This string must include hello double quotes
         *                toobe used as JSON content.
         */
        public void sendPush(final String pns, final String userTag, final String message)
                throws ClientProtocolException, IOException {
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
   
                        String uri = BACKEND_ENDPOINT + "/api/notifications";
                        uri += "?pns=" + pns;
                        uri += "&to_tag=" + userTag;
   
                        HttpPost request = new HttpPost(uri);
                        request.addHeader("Authorization", "Basic "+ getAuthorizationHeader());
                        request.setEntity(new StringEntity(message));
                        request.addHeader("Content-Type", "application/json");
   
                        HttpResponse response = new DefaultHttpClient().execute(request);
   
                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            DialogNotify("MainActivity - Error sending " + pns + " notification",
                                response.getStatusLine().toString());
                            throw new RuntimeException("Error sending notification");
                        }
                    } catch (Exception e) {
                        DialogNotify("MainActivity - Failed toosend " + pns + " notification ", e.getMessage());
                        return e;
                    }
   
                    return null;
                }
            }.execute(null, null, null);
        }

    <span data-ttu-id="27f24-134">Hello `login` hello kezelője **jelentkezzen be** gomb létrehoz egy alapszintű hitelesítési jogkivonat használatával hello bemeneti felhasználónevét és jelszavát (vegye figyelembe, hogy a hitelesítési sémát használja bármely jogkivonat jelképez), majd használja `RegisterClient`toocall hello háttér regisztrációjához.</span><span class="sxs-lookup"><span data-stu-id="27f24-134">hello `login` handler for hello **Log in** button generates a basic authentication token using on hello input username and password (note that this represents any token your authentication scheme uses), then it uses `RegisterClient` toocall hello backend for registration.</span></span>

    <span data-ttu-id="27f24-135">Hello `sendPush` metódushívások hello háttér tootrigger egy biztonságos értesítési toohello felhasználó hello felhasználói kód alapján.</span><span class="sxs-lookup"><span data-stu-id="27f24-135">hello `sendPush` method calls hello backend tootrigger a secure notification toohello user based on hello user tag.</span></span> <span data-ttu-id="27f24-136">hello platform notification szolgáltatáshoz `sendPush` célok függ hello `pns` az átadott karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="27f24-136">hello platform notification service that `sendPush` targets depends on hello `pns` string passed in.</span></span>

1. <span data-ttu-id="27f24-137">Az a `MainActivity` osztály, a frissítés hello `sendNotificationButtonOnClick` metódus toocall hello `sendPush` jelölni platform értesítési szolgáltatások hello felhasználóval módszert az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="27f24-137">In your `MainActivity` class, update hello `sendNotificationButtonOnClick` method toocall hello `sendPush` method with hello user's selected platform notification services as follows.</span></span>
   
       /**
        * Send Notification button click handler. This method sends hello push notification
        * message tooeach platform selected.
        *
        * @param v hello view
        */
       public void sendNotificationButtonOnClick(View v)
               throws ClientProtocolException, IOException {
   
           String nhMessageTag = ((EditText) findViewById(R.id.editTextNotificationMessageTag))
                   .getText().toString();
           String nhMessage = ((EditText) findViewById(R.id.editTextNotificationMessage))
                   .getText().toString();
   
           // JSON String
           nhMessage = "\"" + nhMessage + "\"";
   
           if (((ToggleButton)findViewById(R.id.toggleButtonWNS)).isChecked())
           {
               sendPush("wns", nhMessageTag, nhMessage);
           }
           if (((ToggleButton)findViewById(R.id.toggleButtonGCM)).isChecked())
           {
               sendPush("gcm", nhMessageTag, nhMessage);
           }
           if (((ToggleButton)findViewById(R.id.toggleButtonAPNS)).isChecked())
           {
               sendPush("apns", nhMessageTag, nhMessage);
           }
       }

## <a name="run-hello-application"></a><span data-ttu-id="27f24-138">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="27f24-138">Run hello Application</span></span>
1. <span data-ttu-id="27f24-139">Egy eszköz vagy az Android Studio segítségével emulátor hello alkalmazás futtatható.</span><span class="sxs-lookup"><span data-stu-id="27f24-139">Run hello application on a device or an emulator using Android Studio.</span></span>
2. <span data-ttu-id="27f24-140">A hello Android-alkalmazás írja be a felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="27f24-140">In hello Android app, enter a username and password.</span></span> <span data-ttu-id="27f24-141">Mindkét kell hello azonos karakterláncértéket, és nem a szóközt vagy speciális karaktereket kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="27f24-141">They must both be hello same string value and they must not contain spaces or special characters.</span></span>
3. <span data-ttu-id="27f24-142">Hello Android-alkalmazás, kattintson **jelentkezzen be**.</span><span class="sxs-lookup"><span data-stu-id="27f24-142">In hello Android app, click **Log in**.</span></span> <span data-ttu-id="27f24-143">Várjon, amíg egy bejelentési üzenet **naplózva a és a regisztrált**.</span><span class="sxs-lookup"><span data-stu-id="27f24-143">Wait for a toast message that states **Logged in and registered**.</span></span> <span data-ttu-id="27f24-144">Ez lehetővé teszi a hello **értesítés küldése** gombra.</span><span class="sxs-lookup"><span data-stu-id="27f24-144">This will enable hello **Send Notification** button.</span></span>
   
    ![][A2]
4. <span data-ttu-id="27f24-145">Kattintson a hello váltása gombok tooenable esetében minden platform hello app futott, és a felhasználó regisztrált.</span><span class="sxs-lookup"><span data-stu-id="27f24-145">Click hello toggle buttons tooenable all platforms where you have ran hello app and registered a user.</span></span>
5. <span data-ttu-id="27f24-146">Adja meg a hello felhasználónevet hello értesítési üzenetet fog kapni.</span><span class="sxs-lookup"><span data-stu-id="27f24-146">Enter hello user's name that will receive hello notification message.</span></span> <span data-ttu-id="27f24-147">Az értesítések hello céleszközökön meg regisztrálni kell, hogy a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="27f24-147">That user must be registered for notifications on hello target devices.</span></span>
6. <span data-ttu-id="27f24-148">Adjon meg egy üzenetet hello felhasználói tooreceive egy leküldéses értesítési üzenetet.</span><span class="sxs-lookup"><span data-stu-id="27f24-148">Enter a message for hello user tooreceive as a push notification message.</span></span>
7. <span data-ttu-id="27f24-149">Kattintson a **értesítésküldés**.</span><span class="sxs-lookup"><span data-stu-id="27f24-149">Click **Send Notification**.</span></span>  <span data-ttu-id="27f24-150">Minden eszköz, amely rendelkezik egy regisztrációs hello megfelelő felhasználónév címkével ellátott hello leküldéses értesítést fog kapni.</span><span class="sxs-lookup"><span data-stu-id="27f24-150">Each device that has a registration with hello matching username tag will receive hello push notification.</span></span>

[A1]: ./media/notification-hubs-aspnet-backend-android-notify-users/android-notify-users.png
[A2]: ./media/notification-hubs-aspnet-backend-android-notify-users/android-notify-users-enter-password.png
