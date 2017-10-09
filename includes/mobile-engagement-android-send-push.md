
### <a name="update-manifest-file-tooenable-notifications"></a>Frissítse a jegyzékfájlt tooenable értesítések
Hello alkalmazáson belüli alábbi üzenetküldési erőforrásokat hello között a Manifest.xml fájlba másolni `<application>` és `</application>` címkék.

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

### <a name="specify-an-icon-for-notifications"></a>Az értesítések ikonjának megadása
XML-részletet a manifest.XML fájlba hello között a következő Beillesztés hello `<application>` és `</application>` címkék.

        <meta-data android:name="engagement:reach:notification:icon" android:value="engagement_close"/>

Ez határozza meg a hello mind a rendszer és az alkalmazásbeli értesítésekben megjelenő ikont. A használata alkalmazásbeli értesítések esetén nem, rendszerértesítések esetén viszont kötelező. Az Android rendszer elutasítja az érvénytelen ikonnal rendelkező rendszerértesítéseket.

Győződjön meg arról, hogy létezik-e ikont használ egy hello **rajzolható** mappák (például ``engagement_close.png``). A **mipmap** mappa nem támogatott.

> [!NOTE]
> Ne használjon hello **indítója** ikonra. Más felbontással rendelkezik, és általában megtalálható hello mipmap-mappákban nem támogatottak.
> 
> 

Valós alkalmazások esetén használjon olyan ikont, amely az [Android tervezési útmutatója](http://developer.android.com/design/patterns/notifications.html) szerint használható értesítésekhez.

> [!TIP]
> toobe meg arról, hogy toouse helyes ikonméretet, vessen egy pillantást [ezekben a példákban](https://www.google.com/design/icons).
> Görgessen lefelé toohello **értesítési** szakaszt, kattintson egy ikonra, és kattintson a `PNGS` hello rajzolható ikonkészlet toodownload. Láthatja, hogy melyik rajzolható mappa és melyik megoldás toouse hello ikon egyes verzióihoz.
> 
> 

### <a name="enable-your-app-tooreceive-gcm-push-notifications"></a>Az alkalmazás tooreceive GCM leküldéses értesítések engedélyezése
1. Illessze be a következő hello hello között a Manifest.xml fájlba `<application>` és `</application>` hello cseréje után címkék **Küldőazonosító** a Firebase projekt konzolból beszerzett. hello \n azért így, ezért győződjön meg arról, hogy befejezi a hello projektszám.
   
        <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
2. Illessze be az alábbi hello kódot hello között a Manifest.xml fájlba `<application>` és `</application>` címkék. Cserélje le a csomag neve hello <Your package name>.
   
        <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
        android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
            </intent-filter>
        </receiver>
   
        <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<Your package name>" />
            </intent-filter>
        </receiver>
3. Adja hozzá a hello utolsó engedélykészletüket előtt hello kiemelt `<application>` címke. Cserélje le `<Your package name>` által az alkalmazás hello csomag tényleges neve.
   
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="<Your package name>.permission.C2D_MESSAGE" />
        <permission android:name="<Your package name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

