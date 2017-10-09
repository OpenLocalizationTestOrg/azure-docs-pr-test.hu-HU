---
title: "aaaGet Android alkalmazások Azure Mobile Engagement használatába"
description: "Megtudhatja, hogyan toouse Azure Mobile Engagement az elemzések és leküldéses értesítések Android-alkalmazások."
services: mobile-engagement
documentationcenter: android
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3c286c6d-cfef-4e3e-9b2c-715429fe82db
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: hero-article
ms.date: 08/10/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: e8c92607691104750cdf1c4f7639a041d8a7bcd5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-android-apps"></a>Ismerkedés az Azure Mobile Engagement Android-alkalmazásokkal való használatával
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Ez a témakör bemutatja, hogyan toouse Azure Mobile Engagement toounderstand az alkalmazás használatának és toosend a leküldéses értesítések toosegmented felhasználók Android-alkalmazásba.
Ez az oktatóanyag bemutatja, hogyan hello használó egyszerű küldési forgatókönyvet a Mobile Engagement. Az oktatóanyagban létrehoz egy üres Android-alkalmazást, amely alapszintű adatokat gyűjt, és leküldéses értesítéseket fogad a Google Cloud Messaging (GCM) használatával.

## <a name="prerequisites"></a>Előfeltételek
Az oktatóanyag elvégzéséhez szükség hello [Android Developer Tools](https://developer.android.com/sdk/index.html), amely magában foglalja a hello Android Studio integrált fejlesztőkörnyezetet és hello legújabb Android platformot.

Azt is szükséges hello [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).

> [!IMPORTANT]
> toocomplete ebben az oktatóanyagban egy aktív Azure-fiókra van szüksége. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).
>
>

## <a name="set-up-mobile-engagement-for-your-android-app"></a>A Mobile Engagement beállítása az Android-alkalmazáshoz
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="connect-your-app-toohello-mobile-engagement-backend"></a>Csatlakozás az alkalmazás toohello Mobile Engagement háttérrendszeréhez
Ez az oktatóanyag egy "alapszintű integrációt" mutat, amely minimális hello beállítása szükséges toocollect adatokat, és leküldéses értesítés küldéséhez. Létrehozhat egy alapszintű alkalmazást az Android Studio toodemonstrate hello integráció.

hello teljes integrációs dokumentáció itt található a hello [Mobile Engagement Android SDK-integráció](mobile-engagement-android-sdk-overview.md).

### <a name="create-an-android-project"></a>Android-projekt létrehozása
1. Start **Android Studio**, és hello előugró ablakban válassza **új Android Studio projekt indítása**.

    ![][1]
2. Adjon meg egy alkalmazásnevet és egy vállalati tartományt. Jegyezze fel a megadott információkat, mert később még szüksége lesz rájuk. Kattintson a **Tovább** gombra.

    ![][2]
3. Válassza ki a hello kívánt méretet és API-szintet, és kattintson a **következő**.

   > [!NOTE]
   > A Mobile Engagement legalább 10-es API-szintet igényel (Android 2.3.3).
   >
   >

    ![][3]
4. Válassza ki **üres tevékenység** itt, amely egyetlen képernyője hello alkalmazáshoz, majd kattintson a **következő**.

    ![][4]
5. Végül hagyja az hello alapértelmezett beállításokat, és kattintson **Befejezés**.

    ![][5]

Az Android Studio létrehozza hello bemutatóalkalmazást, amelybe integrálni a Mobile Engagement.

### <a name="include-hello-sdk-library-in-your-project"></a>A projekt tartalmazza a hello SDK-könyvtár
1. Töltse le a hello [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).
2. Bontsa ki a hello archív fájl tooa mappába a számítógépen.
3. Ez az SDK aktuális verziójához hello hello .jar könyvtárat határozza meg és toohello vágólapra másolásához.

      ![][6]
4. Keresse meg a toohello **projekt** szakasz (1) és illessze be a hello .jar hello függvénytárak mappába (2).

      ![][7]
5. tooload hello könyvtárban, a szinkronizálási hello projekt.

      ![][8]

### <a name="connect-your-app-toomobile-engagement-backend-with-hello-connection-string"></a>Csatlakozás a tooMobile Engagement háttéralkalmazás hello kapcsolati karakterlánc
1. Másolás hello alábbi kód a hello tevékenység létrehozása (elvégzendő csak egyetlen helyen az alkalmazásba, általában hello fő tevékenységnél). Ezen PéldaAlkalmazás esetén nyisson meg az src MainActivity hello -> main -> java mappában, és adja hozzá a következő hello:

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);
2. Hárítsa el a hello hivatkozások Alt + Enter megnyomásával, vagy a következő importálási utasításokat hello hozzáadása:

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;
3. Lépjen vissza toohello klasszikus Azure portálra az alkalmazás **Kapcsolatinformáció** lapjáról, és másolja hello **kapcsolati karakterlánc**.

      ![][9]
4. Illessze be hello `setConnectionString` paraméter hello teljes karakterlánc a következő kód hello megjelenő mag cseréje:

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### <a name="add-permissions-and-a-service-declaration"></a>Engedélyek és szolgáltatásdeklaráció hozzáadása
1. Adja hozzá ezek engedélyek toohello a projekt manifest.XML fájljához, azonnal előtt vagy után hello `<application>` címke:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
2. toodeclare hello ügynökszolgáltatás, adja hozzá a kódot közötti hello `<application>` és `</application>` címkék:

        <service
             android:name="com.microsoft.azure.engagement.service.EngagementService"
             android:exported="false"
             android:label="<Your application name>"
             android:process=":Engagement"/>
3. Hello a beillesztett kódban, cserélje le a `"<Your application name>"` hello címke, amely megjelenik hello **beállítások** menü, ahol láthatja hello eszközön futó szolgáltatásokat. A label például hozzáadhat hello "Szolgáltatás" szót.

### <a name="send-a-screen-toomobile-engagement"></a>A képernyő tooMobile Engagement küldése
adatküldés toostart, és győződjön meg arról, hogy hello felhasználók aktívak, el kell küldenie a Mobile Engagement háttérrendszeréhez legalább egy képernyőt (tevékenységet) toohello.

Nyissa meg túl**MainActivity.java** , és adja hozzá a következő tooreplace hello alaposztály hello **MainActivity** túl**EngagementActivity**:

    public class MainActivity extends EngagementActivity {

> [!NOTE]
> Ha az alaposztály nem *tevékenység*, tekintse át [speciális Android-jelentéskészítéssel](mobile-engagement-android-advanced-reporting.md) arról, hogyan tooinherit a más osztályoktól.
>
>

A következő ezen egyszerű forgatókönyv esetén a sor hello megjegyzéssé:

    // setSupportActionBar(toolbar);

Ha azt szeretné, hogy tookeep hello `ActionBar` az alkalmazásban, lásd: [speciális Android-jelentéskészítéssel](mobile-engagement-android-advanced-reporting.md).

## <a name="connect-app-with-real-time-monitoring"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="enable-push-notifications-and-in-app-messaging"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése
A Mobile Engagement a kampányok során lehetővé teszi a felhasználókkal folytatott interakciót és a felhasználók ELÉRÉSÉT leküldéses értesítésekkel és alkalmazáson belüli üzenetekkel. Ez a modul REACH neve hello a Mobile Engagement portálon.
a következő szakasz hello állít be az alkalmazás tooreceive őket.

### <a name="copy-sdk-resources-in-your-project"></a>SDK-erőforrások másolása a projektben
1. Keresse meg a visszafelé tooyour SDK letöltési tartalomhoz, és másolja hello **res** mappát.

    ![][10]
2. Lépjen vissza tooAndroid Studio eszközt, jelölje be hello **fő** könyvtárát a projektfájlok, majd illessze be azt tooadd hello erőforrások tooyour projekt.

    ![][11]

[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[!INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## <a name="next-steps"></a>Következő lépések
Nyissa meg túl[Android SDK](mobile-engagement-android-sdk-overview.md) tooget részletes hello SDK-integráció ismerete.

<!-- Images. -->
[1]: ./media/mobile-engagement-android-get-started/android-studio-new-project.png
[2]: ./media/mobile-engagement-android-get-started/android-studio-project-props.png
[3]: ./media/mobile-engagement-android-get-started/android-studio-project-props2.png
[4]: ./media/mobile-engagement-android-get-started/android-studio-add-activity.png
[5]: ./media/mobile-engagement-android-get-started/android-studio-activity-name.png
[6]: ./media/mobile-engagement-android-get-started/sdk-content.png
[7]: ./media/mobile-engagement-android-get-started/paste-jar.png
[8]: ./media/mobile-engagement-android-get-started/sync-project.png
[9]: ./media/mobile-engagement-android-get-started/app-connection-info-page.png
[10]: ./media/mobile-engagement-android-get-started/copy-resources.png
[11]: ./media/mobile-engagement-android-get-started/paste-resources.png
