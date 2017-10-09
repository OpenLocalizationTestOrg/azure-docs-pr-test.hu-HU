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
# <a name="azure-notification-hubs-notify-users-for-android-with-net-backend"></a>Az Azure Notification Hubs – felhasználók értesítése .NET-háttérrendszerrel Android rendszerhez
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a>Áttekintés
Leküldéses értesítési támogatás az Azure-ban lehetővé teszi tooaccess egy könnyen kezelhető, multiplatform és kibővített leküldéses infrastruktúrában, ami jelentősen egyszerűbb a leküldéses értesítések a mobile fogyasztói, valamint a vállalati alkalmazások hello végrehajtása platformok. Ez az oktatóanyag toouse Azure Notification Hubs toosend a leküldéses értesítések tooa adott alkalmazás felhasználói az adott eszközön. ASP.NET WebAPI háttérrendszerből használt tooauthenticate ügyfelek és toogenerate értesítéseket, ahogy hello útmutatást a témakör az [az alkalmazás háttérrendszeréből regisztrálása](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend). Ez az oktatóanyag épít, hello hello létrehozott értesítési központot [Ismerkedés a Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md) oktatóanyag.

> [!NOTE]
> Ez az oktatóanyag feltételezi, hogy létrehozta és leírtak szerint konfigurálta az értesítési központ [Ismerkedés a Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="create-hello-android-project"></a>Hello Android-projekt létrehozása
következő lépés hello toocreate hello Android-alkalmazás.

1. Hajtsa végre a hello [Ismerkedés a Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md) oktatóanyag toocreate, és konfigurálja az alkalmazás tooreceive leküldéses értesítéseket a gcm-től.
2. Nyissa meg a **res/layout/activity_main.xml** fájlt, hello cserélje le a következő tartalom definíciók hello.
   
    Ez biztosítja, hogy új EditText vezérlők felhasználóként van bejelentkezve. Is mező a rendszer ad hozzá egy felhasználónév tag, értesítést küldünk része lesz:
   
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
3. Nyissa meg a **res/values/strings.xml** fájlt, és cserélje le a hello `send_button` hello következőre definition sorok hello az újradefiniálás hello karakterláncokat `send_button` és hello karakterláncok más vezérlők hozzáadása:
   
        <string name="usernameHint">Username</string>
        <string name="passwordHint">Password</string>
        <string name="loginButton">1. Log in</string>
        <string name="send_button">2. Send Notification</string>
        <string name="notification_message_tag_hint">
            Recipient username tag
        </string>
   
    A main_activity.xml grafikus elrendezés most példához hasonló:
   
    ![][A1]
4. Hozzon létre egy új osztályt **RegisterClient** a hello azonos csomag a `MainActivity` osztály. Használja az alábbi hello kódot hello új osztály fájl.
   
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
   
    Ez az összetevő hello REST hívások szükséges toocontact hello háttéralkalmazás, a rendelés tooregister a leküldéses értesítések valósítja meg. Helyileg tárolja a hello *registrationIds* hello által létrehozott értesítési központot, ahogy az az [az alkalmazás háttérrendszeréből regisztrálása](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend). Vegye figyelembe, hogy használja-e egy engedélyezési jogkivonatot hello kattintva helyi storage-ban tárolt **jelentkezzen be** gombra.
5. Az a `MainActivity` osztály távolítsa el, vagy a privát mezője megjegyzéssé `NotificationHub`, és hello mező felvétele `RegisterClient` osztály és az ASP.NET-háttérrendszerből végpont karakterláncnak. Lehet, hogy tooreplace `<Enter Your Backend Endpoint>` a hello a tényleges háttér-végpontot szerezte be korábban. Például: `http://mybackend.azurewebsites.net`.

        //private NotificationHub hub;
        private RegisterClient registerClient;
        private static final String BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";


1. Az a `MainActivity` osztály hello `onCreate` módszer, távolítsa el vagy hello hello inicializálása megjegyzéssé `hub` mező és hello hívás toohello `registerWithNotificationHubs` metódus. Majd adja hozzá a kódot tooinitialize hello példányának `RegisterClient` osztály. hello metódus hello a következő sorokat kell tartalmaznia:
   
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
2. Az a `MainActivity` osztály, törölje vagy teljes hello megjegyzéssé `registerWithNotificationHubs` metódust. Ez az oktatóanyag nem használható.
3. Adja hozzá a következő hello `import` utasítások tooyour **MainActivity.java** fájlt.
   
        import android.widget.Button;
        import java.io.UnsupportedEncodingException;
        import android.content.Context;
        import java.util.HashSet;
        import android.widget.Toast;
        import org.apache.http.client.ClientProtocolException;
        import java.io.IOException;
        import org.apache.http.HttpStatus;
4. Adja hozzá a következő módszerek toohandle hello hello **jelentkezzen be** gombra kattintson esemény és leküldéses értesítések küldése.
   
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

    Hello `login` hello kezelője **jelentkezzen be** gomb létrehoz egy alapszintű hitelesítési jogkivonat használatával hello bemeneti felhasználónevét és jelszavát (vegye figyelembe, hogy a hitelesítési sémát használja bármely jogkivonat jelképez), majd használja `RegisterClient`toocall hello háttér regisztrációjához.

    Hello `sendPush` metódushívások hello háttér tootrigger egy biztonságos értesítési toohello felhasználó hello felhasználói kód alapján. hello platform notification szolgáltatáshoz `sendPush` célok függ hello `pns` az átadott karakterlánc.

1. Az a `MainActivity` osztály, a frissítés hello `sendNotificationButtonOnClick` metódus toocall hello `sendPush` jelölni platform értesítési szolgáltatások hello felhasználóval módszert az alábbiak szerint.
   
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

## <a name="run-hello-application"></a>Hello alkalmazás futtatása
1. Egy eszköz vagy az Android Studio segítségével emulátor hello alkalmazás futtatható.
2. A hello Android-alkalmazás írja be a felhasználónevet és jelszót. Mindkét kell hello azonos karakterláncértéket, és nem a szóközt vagy speciális karaktereket kell tartalmaznia.
3. Hello Android-alkalmazás, kattintson **jelentkezzen be**. Várjon, amíg egy bejelentési üzenet **naplózva a és a regisztrált**. Ez lehetővé teszi a hello **értesítés küldése** gombra.
   
    ![][A2]
4. Kattintson a hello váltása gombok tooenable esetében minden platform hello app futott, és a felhasználó regisztrált.
5. Adja meg a hello felhasználónevet hello értesítési üzenetet fog kapni. Az értesítések hello céleszközökön meg regisztrálni kell, hogy a felhasználó.
6. Adjon meg egy üzenetet hello felhasználói tooreceive egy leküldéses értesítési üzenetet.
7. Kattintson a **értesítésküldés**.  Minden eszköz, amely rendelkezik egy regisztrációs hello megfelelő felhasználónév címkével ellátott hello leküldéses értesítést fog kapni.

[A1]: ./media/notification-hubs-aspnet-backend-android-notify-users/android-notify-users.png
[A2]: ./media/notification-hubs-aspnet-backend-android-notify-users/android-notify-users-enter-password.png
