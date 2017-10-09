
### <a name="update-manifest-file-tooenable-notifications"></a><span data-ttu-id="4fe8a-101">Frissítse a jegyzékfájlt tooenable értesítések</span><span class="sxs-lookup"><span data-stu-id="4fe8a-101">Update manifest file tooenable notifications</span></span>
<span data-ttu-id="4fe8a-102">Hello alkalmazáson belüli alábbi üzenetküldési erőforrásokat hello között a Manifest.xml fájlba másolni `<application>` és `</application>` címkék.</span><span class="sxs-lookup"><span data-stu-id="4fe8a-102">Copy hello in-app messaging resources below into your Manifest.xml between hello `<application>` and `</application>` tags.</span></span>

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

### <a name="specify-an-icon-for-notifications"></a><span data-ttu-id="4fe8a-103">Az értesítések ikonjának megadása</span><span class="sxs-lookup"><span data-stu-id="4fe8a-103">Specify an icon for notifications</span></span>
<span data-ttu-id="4fe8a-104">XML-részletet a manifest.XML fájlba hello között a következő Beillesztés hello `<application>` és `</application>` címkék.</span><span class="sxs-lookup"><span data-stu-id="4fe8a-104">Paste hello following XML snippet in your Manifest.xml between hello `<application>` and `</application>` tags.</span></span>

        <meta-data android:name="engagement:reach:notification:icon" android:value="engagement_close"/>

<span data-ttu-id="4fe8a-105">Ez határozza meg a hello mind a rendszer és az alkalmazásbeli értesítésekben megjelenő ikont.</span><span class="sxs-lookup"><span data-stu-id="4fe8a-105">This defines hello icon that is displayed both in system and in-app notifications.</span></span> <span data-ttu-id="4fe8a-106">A használata alkalmazásbeli értesítések esetén nem, rendszerértesítések esetén viszont kötelező.</span><span class="sxs-lookup"><span data-stu-id="4fe8a-106">It is optional for in-app notifications however mandatory for system notifications.</span></span> <span data-ttu-id="4fe8a-107">Az Android rendszer elutasítja az érvénytelen ikonnal rendelkező rendszerértesítéseket.</span><span class="sxs-lookup"><span data-stu-id="4fe8a-107">Android will rejects system notifications with invalid icons.</span></span>

<span data-ttu-id="4fe8a-108">Győződjön meg arról, hogy létezik-e ikont használ egy hello **rajzolható** mappák (például ``engagement_close.png``).</span><span class="sxs-lookup"><span data-stu-id="4fe8a-108">Make sure you are using an icon that exists in one of hello **drawable** folders (like ``engagement_close.png``).</span></span> <span data-ttu-id="4fe8a-109">A **mipmap** mappa nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="4fe8a-109">**mipmap** folder isn't supported.</span></span>

> [!NOTE]
> <span data-ttu-id="4fe8a-110">Ne használjon hello **indítója** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4fe8a-110">You should not use hello **launcher** icon.</span></span> <span data-ttu-id="4fe8a-111">Más felbontással rendelkezik, és általában megtalálható hello mipmap-mappákban nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="4fe8a-111">It has a different resolution and is usually in hello mipmap folders, which we don't support.</span></span>
> 
> 

<span data-ttu-id="4fe8a-112">Valós alkalmazások esetén használjon olyan ikont, amely az [Android tervezési útmutatója](http://developer.android.com/design/patterns/notifications.html) szerint használható értesítésekhez.</span><span class="sxs-lookup"><span data-stu-id="4fe8a-112">For real apps, you can use an icon that is suitable for notifications per [Android design guidelines](http://developer.android.com/design/patterns/notifications.html).</span></span>

> [!TIP]
> <span data-ttu-id="4fe8a-113">toobe meg arról, hogy toouse helyes ikonméretet, vessen egy pillantást [ezekben a példákban](https://www.google.com/design/icons).</span><span class="sxs-lookup"><span data-stu-id="4fe8a-113">toobe sure toouse correct icon resolutions, you can look at [these examples](https://www.google.com/design/icons).</span></span>
> <span data-ttu-id="4fe8a-114">Görgessen lefelé toohello **értesítési** szakaszt, kattintson egy ikonra, és kattintson a `PNGS` hello rajzolható ikonkészlet toodownload.</span><span class="sxs-lookup"><span data-stu-id="4fe8a-114">Scroll down toohello **Notification** section, click an icon, and then click `PNGS` toodownload hello icon drawable set.</span></span> <span data-ttu-id="4fe8a-115">Láthatja, hogy melyik rajzolható mappa és melyik megoldás toouse hello ikon egyes verzióihoz.</span><span class="sxs-lookup"><span data-stu-id="4fe8a-115">You can see what drawable folders with which resolution toouse for each version of hello icon.</span></span>
> 
> 

### <a name="enable-your-app-tooreceive-gcm-push-notifications"></a><span data-ttu-id="4fe8a-116">Az alkalmazás tooreceive GCM leküldéses értesítések engedélyezése</span><span class="sxs-lookup"><span data-stu-id="4fe8a-116">Enable your app tooreceive GCM push notifications</span></span>
1. <span data-ttu-id="4fe8a-117">Illessze be a következő hello hello között a Manifest.xml fájlba `<application>` és `</application>` hello cseréje után címkék **Küldőazonosító** a Firebase projekt konzolból beszerzett.</span><span class="sxs-lookup"><span data-stu-id="4fe8a-117">Paste hello following into your Manifest.xml between hello `<application>` and `</application>` tags after replacing hello **Sender ID** obtained from your Firebase project console.</span></span> <span data-ttu-id="4fe8a-118">hello \n azért így, ezért győződjön meg arról, hogy befejezi a hello projektszám.</span><span class="sxs-lookup"><span data-stu-id="4fe8a-118">hello \n is intentional so make sure that you end hello project number with it.</span></span>
   
        <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
2. <span data-ttu-id="4fe8a-119">Illessze be az alábbi hello kódot hello között a Manifest.xml fájlba `<application>` és `</application>` címkék.</span><span class="sxs-lookup"><span data-stu-id="4fe8a-119">Paste hello code below into your Manifest.xml between hello `<application>` and `</application>` tags.</span></span> <span data-ttu-id="4fe8a-120">Cserélje le a csomag neve hello <Your package name>.</span><span class="sxs-lookup"><span data-stu-id="4fe8a-120">Replace hello package name <Your package name>.</span></span>
   
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
3. <span data-ttu-id="4fe8a-121">Adja hozzá a hello utolsó engedélykészletüket előtt hello kiemelt `<application>` címke.</span><span class="sxs-lookup"><span data-stu-id="4fe8a-121">Add hello last set of permissions that are highlighted before hello `<application>` tag.</span></span> <span data-ttu-id="4fe8a-122">Cserélje le `<Your package name>` által az alkalmazás hello csomag tényleges neve.</span><span class="sxs-lookup"><span data-stu-id="4fe8a-122">Replace `<Your package name>` by hello actual package name of your application.</span></span>
   
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="<Your package name>.permission.C2D_MESSAGE" />
        <permission android:name="<Your package name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

