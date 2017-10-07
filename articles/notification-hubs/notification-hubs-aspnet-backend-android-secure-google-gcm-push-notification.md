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
# <a name="sending-secure-push-notifications-with-azure-notification-hubs"></a>Biztonságos leküldéses értesítések küldése az Azure Notification hubs használatával
> [!div class="op_single_selector"]
> * [Windows Universal](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a>Áttekintés
> [!IMPORTANT]
> toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).
> 
> 

Leküldéses értesítési támogatása a Microsoft Azure lehetővé teszi tooaccess egy könnyen használható, többplatformos, kibővített leküldéses üzenet infrastruktúrában, ami jelentősen egyszerűbb a leküldéses értesítések az ügyfél és a vállalati alkalmazások hello végrehajtása mobil platformokon.

Tooregulatory vagy biztonsági korlátozások miatt néha egy alkalmazás érdemes tooinclude valamit a hello szabványos leküldéses értesítési infrastruktúrát keresztül nem továbbítható hello értesítést. Ez az oktatóanyag leírja, hogyan tooachieve hello úgy, hogy a bizalmas adatokat hello Android ügyféleszköz- és hello háttéralkalmazás közötti biztonságos és hitelesített kapcsolaton keresztül küld ugyanazt a felhasználói élményt.

Magas szinten hello folyamat a következőképpen történik:

1. hello app háttér:
   * Háttér-adatbázisban tárolja biztonságos hasznos.
   * Küldi hello Azonosítóját az értesítési toohello Android-eszköz (nem biztonságos információk küldése).
2. hello alkalmazást hello eszközön, hello értesítés fogadása közben:
   * hello Android-eszköz kapcsolatot létesít a hello háttér-kérelmező hello biztonságos hasznos.
   * hello hasznos mutatja az hello app értesítésként hello eszközön.

Fontos, hogy megelőző folyamata hello (és az oktatóanyag) feltételezzük, hogy hello eszköz toonote tárol egy hitelesítési jogkivonatot helyi tárhelyre – hello felhasználó bejelentkezése után. Ez biztosítja, hogy teljesen zökkenőmentes élményt, mivel hello eszköz hello értesítési biztonságos hasznos a token használatával kérheti le. Ha az alkalmazás nem tárolja a hitelesítési tokenek hello eszköz, vagy ezeket a jogkivonatokat is járhatott, hello eszközalkalmazás hello leküldéses értesítés fogadásakor megjelenjen-e arra kéri a hello felhasználói toolaunch hello alkalmazás általános értesítést. hello app majd hello felhasználó hitelesíti, és hello értesítési tartalom jeleníti meg.

Ez az oktatóanyag bemutatja, hogyan biztonságos toosend leküldéses értesítések. -Buildekről nyújtanak a hello [felhasználók értesítése](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) oktatóanyag, ezért el kell végeznie hello lépéseket, hogy az oktatóanyagban először Ha még nem tette meg.

> [!NOTE]
> Ez az oktatóanyag feltételezi, hogy létrehozta és leírtak szerint konfigurálta az értesítési központ [Ismerkedés a Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-hello-android-project"></a>Android-projekt hello módosítása
Most, hogy az alkalmazás háttér-toosend csak hello módosított *azonosító* leküldéses értesítés, hogy toochange az Android-alkalmazás toohandle, értesítések és a visszahívás a háttér-tooretrieve hello megjelenített üzenet toobe biztonságos.
tooachieve ezen cél esetében van toomake meg arról, hogy az Android-alkalmazás tudja, hogy hogyan a háttér-hello leküldéses értesítés fogadásakor a saját magát tooauthenticate.

Most módosítja, a Microsoft hello *bejelentkezési* folyamata a rendelés toosave hello hitelesítési fejléc értéke hello megosztott az alkalmazás beállításait. Hasonló mechanizmusok használt toostore lehet bármely hitelesítési jogkivonat (pl. OAuth jogkivonatok), amely az alkalmazás hello lesz toouse anélkül, hogy a felhasználói hitelesítő adatokat.

1. Az Android-alkalmazás projektben adja hozzá a következő állandókat hello hello tetején hello **MainActivity** osztály:
   
        public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
        public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";
2. Még tart a hello **MainActivity** osztály, a frissítés hello `getAuthorizationHeader()` metódus toocontain hello a következő kódot:
   
        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);
   
            SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();
   
            return basicAuthHeader;
        }
3. Adja hozzá a következő hello `import` hello hello tetején utasítások **MainActivity** fájlt:
   
        import android.content.SharedPreferences;

Most azt fogja módosítani hello kezelő is nevezett hello értesítés fogadásakor.

1. A hello **MyHandler** osztály módosítása hello `OnReceive()` metódus toocontain:
   
        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String secureMessageId = bundle.getString("secureId");
            retrieveNotification(secureMessageId);
        }
2. Majd adja hozzá a hello `retrieveNotification()` metódus hello helyőrző cseréje `{back-end endpoint}` hello háttér-végponthoz, a háttér-másikban kapott:
   
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

Ez a metódus meghívja az alkalmazás háttér-tooretrieve hello értesítés hello tárolt hello hitelesítő adatok használatával megosztott beállítások, és megjeleníti azt a normál értesítésként tartalom. hello értesítési jelek toohello alkalmazás felhasználói akárcsak bármely más leküldéses értesítést.

Ne feledje, hogy hiányzó hitelesítési fejléc tulajdonság vagy elutasíthatják hello háttér-érdemes toohandle hello esetekben. Ezekben az esetekben hello adott kezelésének legtöbbször a cél felhasználói élmény függ. Egy elem toodisplay hello tooauthenticate tooretrieve hello tényleges Felhasználóértesítés általános kérése az értesítést.

## <a name="run-hello-application"></a>Hello alkalmazás futtatása
toorun hello alkalmazás, a következő hello:

1. Győződjön meg arról, hogy **AppBackend** a rendszer az Azure-ban. Ha a Visual Studio használatával, futtassa a hello **AppBackend** webes API-alkalmazás. Az ASP.NET-weblap jelenik meg.
2. Az eclipse-ben egy fizikai Android eszköz vagy hello emulátor hello alkalmazást futtatnak.
3. A hello Android-alkalmazás felhasználói felületén, írja be a felhasználónevet és jelszót. Ezek karakterlánc lehet, de kell hello ugyanazt az értéket.
4. Hello Android-alkalmazás felhasználói felületén, kattintson **jelentkezzen be**. Kattintson a **leküldéses küldése**.

