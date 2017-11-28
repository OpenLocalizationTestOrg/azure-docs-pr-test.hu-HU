
### <a name="update-manifest-file-to-enable-notifications"></a><span data-ttu-id="c1b26-101">A jegyzékfájl frissítése az értesítések engedélyezéséhez</span><span class="sxs-lookup"><span data-stu-id="c1b26-101">Update manifest file to enable notifications</span></span>
<span data-ttu-id="c1b26-102">Másolja át az alkalmazáson belüli alábbi üzenetküldési erőforrásokat a Manifest.xml fájlba, az `<application>` és `</application>` címkék közé.</span><span class="sxs-lookup"><span data-stu-id="c1b26-102">Copy the in-app messaging resources below into your Manifest.xml between the `<application>` and `</application>` tags.</span></span>

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

### <a name="specify-an-icon-for-notifications"></a><span data-ttu-id="c1b26-103">Az értesítések ikonjának megadása</span><span class="sxs-lookup"><span data-stu-id="c1b26-103">Specify an icon for notifications</span></span>
<span data-ttu-id="c1b26-104">Illessze be a következő XML-részletet a Manifest.xml fájlba, az `<application>` és `</application>` címkék közé.</span><span class="sxs-lookup"><span data-stu-id="c1b26-104">Paste the following XML snippet in your Manifest.xml between the `<application>` and `</application>` tags.</span></span>

        <meta-data android:name="engagement:reach:notification:icon" android:value="engagement_close"/>

<span data-ttu-id="c1b26-105">Ez meghatározza a rendszerben, valamint az alkalmazásbeli értesítésekben megjelenő ikont.</span><span class="sxs-lookup"><span data-stu-id="c1b26-105">This defines the icon that is displayed both in system and in-app notifications.</span></span> <span data-ttu-id="c1b26-106">A használata alkalmazásbeli értesítések esetén nem, rendszerértesítések esetén viszont kötelező.</span><span class="sxs-lookup"><span data-stu-id="c1b26-106">It is optional for in-app notifications however mandatory for system notifications.</span></span> <span data-ttu-id="c1b26-107">Az Android rendszer elutasítja az érvénytelen ikonnal rendelkező rendszerértesítéseket.</span><span class="sxs-lookup"><span data-stu-id="c1b26-107">Android will rejects system notifications with invalid icons.</span></span>

<span data-ttu-id="c1b26-108">Győződjön meg arról, hogy olyan ikont használ, amely megtalálható a **drawable** (rajzolható) mappák egyikében (pl. ``engagement_close.png``).</span><span class="sxs-lookup"><span data-stu-id="c1b26-108">Make sure you are using an icon that exists in one of the **drawable** folders (like ``engagement_close.png``).</span></span> <span data-ttu-id="c1b26-109">A **mipmap** mappa nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="c1b26-109">**mipmap** folder isn't supported.</span></span>

> [!NOTE]
> <span data-ttu-id="c1b26-110">Ne használja az **indító**ikont.</span><span class="sxs-lookup"><span data-stu-id="c1b26-110">You should not use the **launcher** icon.</span></span> <span data-ttu-id="c1b26-111">Az indítóikon más felbontással rendelkezik, és általában a nem támogatott mipmap-mappákban található.</span><span class="sxs-lookup"><span data-stu-id="c1b26-111">It has a different resolution and is usually in the mipmap folders, which we don't support.</span></span>
> 
> 

<span data-ttu-id="c1b26-112">Valós alkalmazások esetén használjon olyan ikont, amely az [Android tervezési útmutatója](http://developer.android.com/design/patterns/notifications.html) szerint használható értesítésekhez.</span><span class="sxs-lookup"><span data-stu-id="c1b26-112">For real apps, you can use an icon that is suitable for notifications per [Android design guidelines](http://developer.android.com/design/patterns/notifications.html).</span></span>

> [!TIP]
> <span data-ttu-id="c1b26-113">Az [alábbi példák](https://www.google.com/design/icons) megtekintésével meggyőződhet arról, hogy a helyes ikonméretet használja-e.</span><span class="sxs-lookup"><span data-stu-id="c1b26-113">To be sure to use correct icon resolutions, you can look at [these examples](https://www.google.com/design/icons).</span></span>
> <span data-ttu-id="c1b26-114">Görgessen le a **Notification** (Értesítés) szakaszhoz, kattintson egy ikonra, majd kattintson a `PNGS` gombra a rajzolható ikonkészlet letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="c1b26-114">Scroll down to the **Notification** section, click an icon, and then click `PNGS` to download the icon drawable set.</span></span> <span data-ttu-id="c1b26-115">Itt láthatja, hogy melyik rajzolható mappa és melyik méret használható az ikon egyes verzióihoz.</span><span class="sxs-lookup"><span data-stu-id="c1b26-115">You can see what drawable folders with which resolution to use for each version of the icon.</span></span>
> 
> 

### <a name="enable-your-app-to-receive-gcm-push-notifications"></a><span data-ttu-id="c1b26-116">GCM leküldéses értesítések fogadásának engedélyezése az alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="c1b26-116">Enable your app to receive GCM push notifications</span></span>
1. <span data-ttu-id="c1b26-117">Illessze be a következőt a Manifest.xml fájlba, az `<application>` és `</application>` címkék közé, miután kicserélte a Firebase-konzolból beszerzett **Feladóazonosítót**.</span><span class="sxs-lookup"><span data-stu-id="c1b26-117">Paste the following into your Manifest.xml between the `<application>` and `</application>` tags after replacing the **Sender ID** obtained from your Firebase project console.</span></span> <span data-ttu-id="c1b26-118">Az \n rész szándékosan került bele, ezért győződjön meg arról, hogy odaírja a projektszám végére.</span><span class="sxs-lookup"><span data-stu-id="c1b26-118">The \n is intentional so make sure that you end the project number with it.</span></span>
   
        <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
2. <span data-ttu-id="c1b26-119">Illessze be az alábbi kódot a Manifest.xml fájlba, az `<application>` és `</application>` címkék közé.</span><span class="sxs-lookup"><span data-stu-id="c1b26-119">Paste the code below into your Manifest.xml between the `<application>` and `</application>` tags.</span></span> <span data-ttu-id="c1b26-120">Cserélje ki az alábbi csomagnevet: <Your package name>.</span><span class="sxs-lookup"><span data-stu-id="c1b26-120">Replace the package name <Your package name>.</span></span>
   
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
3. <span data-ttu-id="c1b26-121">Adja hozzá a kiemelt utolsó engedélykészletet az `<application>` címke elé.</span><span class="sxs-lookup"><span data-stu-id="c1b26-121">Add the last set of permissions that are highlighted before the `<application>` tag.</span></span> <span data-ttu-id="c1b26-122">Cserélje ki a `<Your package name>` részt az alkalmazás tényleges csomagnevére.</span><span class="sxs-lookup"><span data-stu-id="c1b26-122">Replace `<Your package name>` by the actual package name of your application.</span></span>
   
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="<Your package name>.permission.C2D_MESSAGE" />
        <permission android:name="<Your package name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

