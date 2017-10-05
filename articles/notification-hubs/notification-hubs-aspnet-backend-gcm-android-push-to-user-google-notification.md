---
title: "Az Azure Notification Hubs – felhasználók értesítése .NET-háttérrendszerrel Android rendszerhez"
description: "Megtudhatja, hogyan küldhetők leküldéses értesítések az Azure-ban a felhasználók számára. Androidhoz készült Java nyelven írt mintakódok"
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
ms.openlocfilehash: 418a4b638dfaa3fee33a7a7242433699205c79f7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-notification-hubs-notify-users-for-android-with-net-backend"></a><span data-ttu-id="43380-104">Az Azure Notification Hubs – felhasználók értesítése .NET-háttérrendszerrel Android rendszerhez</span><span class="sxs-lookup"><span data-stu-id="43380-104">Azure Notification Hubs Notify Users for Android with .NET backend</span></span>
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a><span data-ttu-id="43380-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="43380-105">Overview</span></span>
<span data-ttu-id="43380-106">Leküldéses értesítési támogatás az Azure-ban lehetővé teszi egy könnyen kezelhető, multiplatform és kibővített leküldéses infrastruktúrában, ami jelentősen egyszerűsíti a leküldéses értesítések mobil platformokhoz fogyasztói, valamint a vállalati alkalmazások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="43380-106">Push notification support in Azure enables you to access an easy-to-use, multiplatform, and scaled-out push infrastructure, which greatly simplifies the implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span> <span data-ttu-id="43380-107">Az oktatóanyag bemutatja, hogyan küldhetők az Azure Notification Hubs használatával leküldéses értesítések adott alkalmazásfelhasználónak, meghatározott eszközre.</span><span class="sxs-lookup"><span data-stu-id="43380-107">This tutorial shows you how to use Azure Notification Hubs to send push notifications to a specific app user on a specific device.</span></span> <span data-ttu-id="43380-108">ASP.NET WebAPI háttérrendszerből használt hitelesíti az ügyfeleket, és értesítéseket, ahogy az az útmutató témakör [az alkalmazás háttérrendszeréből regisztrálása](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).</span><span class="sxs-lookup"><span data-stu-id="43380-108">An ASP.NET WebAPI backend is used to authenticate clients and to generate notifications, as shown in the guidance topic [Registering from your app backend](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).</span></span> <span data-ttu-id="43380-109">Ez az oktatóanyag épít, a létrehozott értesítési központot a [Ismerkedés a Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="43380-109">This tutorial builds on the notification hub that you created in the [Getting Started with Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md) tutorial.</span></span>

> [!NOTE]
> <span data-ttu-id="43380-110">Ez az oktatóanyag feltételezi, hogy létrehozta és leírtak szerint konfigurálta az értesítési központ [Ismerkedés a Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="43380-110">This tutorial assumes that you have created and configured your notification hub as described in [Getting Started with Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).</span></span>
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="create-the-android-project"></a><span data-ttu-id="43380-111">Az Android-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="43380-111">Create the Android Project</span></span>
<span data-ttu-id="43380-112">A következő lépés, ha az Android-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="43380-112">The next step is to create the Android application.</span></span>

1. <span data-ttu-id="43380-113">Kövesse a [Ismerkedés a Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md) oktatóanyag létrehozásához, és állítsa be alkalmazását annak leküldéses értesítéseket a gcm-től.</span><span class="sxs-lookup"><span data-stu-id="43380-113">Follow the [Getting Started with Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md) tutorial to create and configure your app to receive push notifications from GCM.</span></span>
2. <span data-ttu-id="43380-114">Nyissa meg a **res/layout/activity_main.xml** fájlt, cserélje le a következő tartalom definíciókkal.</span><span class="sxs-lookup"><span data-stu-id="43380-114">Open your **res/layout/activity_main.xml** file, replace the with the following content definitions.</span></span>
   
    <span data-ttu-id="43380-115">Ez biztosítja, hogy új EditText vezérlők felhasználóként van bejelentkezve.</span><span class="sxs-lookup"><span data-stu-id="43380-115">This adds new EditText controls for logging in as a user.</span></span> <span data-ttu-id="43380-116">Is mező a rendszer ad hozzá egy felhasználónév tag, értesítést küldünk része lesz:</span><span class="sxs-lookup"><span data-stu-id="43380-116">Also a field is added for a username tag that will be part of notifications you send:</span></span>
   
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
3. <span data-ttu-id="43380-117">Nyissa meg a **res/values/strings.xml** fájlt, és cserélje le a `send_button` definícióját a következő sorokat, amely definiálja újra a karakterláncot a `send_button` , és adja hozzá a más vezérlők karakterláncok:</span><span class="sxs-lookup"><span data-stu-id="43380-117">Open your **res/values/strings.xml** file and replace the `send_button` definition with the following lines that redefine the string for the `send_button` and add strings for the other controls:</span></span>
   
        <string name="usernameHint">Username</string>
        <string name="passwordHint">Password</string>
        <string name="loginButton">1. Log in</string>
        <string name="send_button">2. Send Notification</string>
        <string name="notification_message_tag_hint">
            Recipient username tag
        </string>
   
    <span data-ttu-id="43380-118">A main_activity.xml grafikus elrendezés most példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="43380-118">Your main_activity.xml graphical layout should now look like this:</span></span>
   
    ![][A1]
4. <span data-ttu-id="43380-119">Hozzon létre egy új osztályt **RegisterClient** azonos csomagban található a `MainActivity` osztály.</span><span class="sxs-lookup"><span data-stu-id="43380-119">Create a new class named **RegisterClient** in the same package as your `MainActivity` class.</span></span> <span data-ttu-id="43380-120">Az alábbi kódot használja az új osztály fájl.</span><span class="sxs-lookup"><span data-stu-id="43380-120">Use the code below for the new class file.</span></span>
   
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
   
    <span data-ttu-id="43380-121">Ez az összetevő valósítja meg a REST-hívások lépjen kapcsolatba a háttéralkalmazás regisztrálhat a leküldéses értesítések szükséges.</span><span class="sxs-lookup"><span data-stu-id="43380-121">This component implements the REST calls required to contact the app backend, in order to register for push notifications.</span></span> <span data-ttu-id="43380-122">Helyileg tárolja a *registrationIds* hozta létre az értesítési központnak a [az alkalmazás háttérrendszeréből regisztrálása](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).</span><span class="sxs-lookup"><span data-stu-id="43380-122">It also locally stores the *registrationIds* created by the Notification Hub as detailed in [Registering from your app backend](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).</span></span> <span data-ttu-id="43380-123">Vegye figyelembe, hogy használja-e egy helyi storage-ban tárolt kattintva engedélyezési jogkivonatot a **jelentkezzen be** gombra.</span><span class="sxs-lookup"><span data-stu-id="43380-123">Note that it uses an authorization token stored in local storage when you click the **Log in** button.</span></span>
5. <span data-ttu-id="43380-124">Az a `MainActivity` osztály távolítsa el, vagy a privát mezője megjegyzéssé `NotificationHub`, és a mező felvétele a `RegisterClient` osztály és az ASP.NET-háttérrendszerből végpont karakterláncnak.</span><span class="sxs-lookup"><span data-stu-id="43380-124">In your `MainActivity` class remove or comment out your private field for `NotificationHub`, and add a field for the `RegisterClient` class and a string for your ASP.NET backend's endpoint.</span></span> <span data-ttu-id="43380-125">Ügyeljen arra, hogy a csere `<Enter Your Backend Endpoint>` az a korábban beszerzett a tényleges háttér-végpontot.</span><span class="sxs-lookup"><span data-stu-id="43380-125">Be sure to replace `<Enter Your Backend Endpoint>` with the your actual backend endpoint obtained previously.</span></span> <span data-ttu-id="43380-126">Például: `http://mybackend.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="43380-126">For example, `http://mybackend.azurewebsites.net`.</span></span>

        //private NotificationHub hub;
        private RegisterClient registerClient;
        private static final String BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";


1. <span data-ttu-id="43380-127">Az a `MainActivity` osztály a a `onCreate` metódus, távolítsa el vagy megjegyzéssé inicializálása a `hub` mező, és a hívást a `registerWithNotificationHubs` metódus.</span><span class="sxs-lookup"><span data-stu-id="43380-127">In your `MainActivity` class, in the `onCreate` method, remove or comment out the initialization of the `hub` field and the call to the `registerWithNotificationHubs` method.</span></span> <span data-ttu-id="43380-128">Majd adja hozzá a kódot példányának inicializálása a `RegisterClient` osztály.</span><span class="sxs-lookup"><span data-stu-id="43380-128">Then add code to initialize an instance of the `RegisterClient` class.</span></span> <span data-ttu-id="43380-129">A metódus a következő sorokat kell tartalmaznia:</span><span class="sxs-lookup"><span data-stu-id="43380-129">The method should contain the following lines:</span></span>
   
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
2. <span data-ttu-id="43380-130">Az a `MainActivity` osztály, törlése vagy a teljes megjegyzéssé `registerWithNotificationHubs` metódust.</span><span class="sxs-lookup"><span data-stu-id="43380-130">In your `MainActivity` class, delete or comment out the entire `registerWithNotificationHubs` method.</span></span> <span data-ttu-id="43380-131">Ez az oktatóanyag nem használható.</span><span class="sxs-lookup"><span data-stu-id="43380-131">It will not be used in this tutorial.</span></span>
3. <span data-ttu-id="43380-132">Adja hozzá a következő `import` utasítást, hogy a **MainActivity.java** fájlt.</span><span class="sxs-lookup"><span data-stu-id="43380-132">Add the following `import` statements to your **MainActivity.java** file.</span></span>
   
        import android.widget.Button;
        import java.io.UnsupportedEncodingException;
        import android.content.Context;
        import java.util.HashSet;
        import android.widget.Toast;
        import org.apache.http.client.ClientProtocolException;
        import java.io.IOException;
        import org.apache.http.HttpStatus;
4. <span data-ttu-id="43380-133">Adja hozzá az alábbi módszerek kezelni a **jelentkezzen be** gombra kattintson esemény és leküldéses értesítések küldése.</span><span class="sxs-lookup"><span data-stu-id="43380-133">Then, add the following methods to handle the **Log in** button click event and sending push notifications.</span></span>
   
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
                        DialogNotify("MainActivity - Failed to register", e.getMessage());
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
         * This method calls the ASP.NET WebAPI backend to send the notification message
         * to the platform notification service based on the pns parameter.
         *
         * @param pns     The platform notification service to send the notification message to. Must
         *                be one of the following ("wns", "gcm", "apns").
         * @param userTag The tag for the user who will receive the notification message. This string
         *                must not contain spaces or special characters.
         * @param message The notification message string. This string must include the double quotes
         *                to be used as JSON content.
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
                        DialogNotify("MainActivity - Failed to send " + pns + " notification ", e.getMessage());
                        return e;
                    }
   
                    return null;
                }
            }.execute(null, null, null);
        }

    <span data-ttu-id="43380-134">A `login` kezelő-a **jelentkezzen be** gomb létrehoz egy egyszerű hitelesítés lexikális elem: a bemeneti felhasználónevét és jelszavát (vegye figyelembe, hogy a hitelesítési sémát használja bármely jogkivonat jelképez) használatával, majd használja `RegisterClient` számára a háttér hívja a regisztrációhoz.</span><span class="sxs-lookup"><span data-stu-id="43380-134">The `login` handler for the **Log in** button generates a basic authentication token using on the input username and password (note that this represents any token your authentication scheme uses), then it uses `RegisterClient` to call the backend for registration.</span></span>

    <span data-ttu-id="43380-135">A `sendPush` metódus meghívja a háttérkiszolgálón való biztonságos értesítést a felhasználót a felhasználói kód alapján.</span><span class="sxs-lookup"><span data-stu-id="43380-135">The `sendPush` method calls the backend to trigger a secure notification to the user based on the user tag.</span></span> <span data-ttu-id="43380-136">A platform notification szolgáltatáshoz `sendPush` célok függ a `pns` az átadott karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="43380-136">The platform notification service that `sendPush` targets depends on the `pns` string passed in.</span></span>

1. <span data-ttu-id="43380-137">Az a `MainActivity` osztály, frissítse a `sendNotificationButtonOnClick` metódust kell meghívni a `sendPush` metódust használ a felhasználó kiválasztott platform értesítési szolgáltatások az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="43380-137">In your `MainActivity` class, update the `sendNotificationButtonOnClick` method to call the `sendPush` method with the user's selected platform notification services as follows.</span></span>
   
       /**
        * Send Notification button click handler. This method sends the push notification
        * message to each platform selected.
        *
        * @param v The view
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

## <a name="run-the-application"></a><span data-ttu-id="43380-138">Futtassa az alkalmazást</span><span class="sxs-lookup"><span data-stu-id="43380-138">Run the Application</span></span>
1. <span data-ttu-id="43380-139">Futtassa az alkalmazást egy eszközön vagy az emulátor Android Studio segítségével.</span><span class="sxs-lookup"><span data-stu-id="43380-139">Run the application on a device or an emulator using Android Studio.</span></span>
2. <span data-ttu-id="43380-140">Az Android-alkalmazás adja meg egy felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="43380-140">In the Android app, enter a username and password.</span></span> <span data-ttu-id="43380-141">Mindkét legyen ugyanaz a karakterláncértéke, és azok nem tartalmazhat szóközt vagy speciális karaktereket.</span><span class="sxs-lookup"><span data-stu-id="43380-141">They must both be the same string value and they must not contain spaces or special characters.</span></span>
3. <span data-ttu-id="43380-142">Kattintson az Android-alkalmazás **jelentkezzen be**.</span><span class="sxs-lookup"><span data-stu-id="43380-142">In the Android app, click **Log in**.</span></span> <span data-ttu-id="43380-143">Várjon, amíg egy bejelentési üzenet **naplózva a és a regisztrált**.</span><span class="sxs-lookup"><span data-stu-id="43380-143">Wait for a toast message that states **Logged in and registered**.</span></span> <span data-ttu-id="43380-144">Ez lehetővé teszi a **értesítés küldése** gombra.</span><span class="sxs-lookup"><span data-stu-id="43380-144">This will enable the **Send Notification** button.</span></span>
   
    ![][A2]
4. <span data-ttu-id="43380-145">Kattintson a váltógomb gombok esetében minden platform engedélyezése az alkalmazás futtatása, és a felhasználó regisztrált.</span><span class="sxs-lookup"><span data-stu-id="43380-145">Click the toggle buttons to enable all platforms where you have ran the app and registered a user.</span></span>
5. <span data-ttu-id="43380-146">Adja meg a felhasználó nevét, akik szintén megkapják az értesítési üzenet.</span><span class="sxs-lookup"><span data-stu-id="43380-146">Enter the user's name that will receive the notification message.</span></span> <span data-ttu-id="43380-147">Az értesítések a céleszközökön meg regisztrálni kell, hogy a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="43380-147">That user must be registered for notifications on the target devices.</span></span>
6. <span data-ttu-id="43380-148">Adja meg a felhasználó számára a leküldéses értesítési üzenet fogadása az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="43380-148">Enter a message for the user to receive as a push notification message.</span></span>
7. <span data-ttu-id="43380-149">Kattintson a **értesítésküldés**.</span><span class="sxs-lookup"><span data-stu-id="43380-149">Click **Send Notification**.</span></span>  <span data-ttu-id="43380-150">Minden eszköz, amely rendelkezik a megfelelő felhasználónév címkével regisztráció a leküldéses értesítést fog kapni.</span><span class="sxs-lookup"><span data-stu-id="43380-150">Each device that has a registration with the matching username tag will receive the push notification.</span></span>

[A1]: ./media/notification-hubs-aspnet-backend-android-notify-users/android-notify-users.png
[A2]: ./media/notification-hubs-aspnet-backend-android-notify-users/android-notify-users-enter-password.png
