1. <span data-ttu-id="50991-101">Nyissa meg a hello Android SDK Manager hello eszköztár Android Studio hello ikonjára kattintva vagy kattintva **eszközök** -> **Android** -> **SDK Manager**hello menüben.</span><span class="sxs-lookup"><span data-stu-id="50991-101">Open hello Android SDK Manager by clicking hello icon on hello toolbar of Android Studio or by clicking **Tools** -> **Android** -> **SDK Manager** on hello menu.</span></span> <span data-ttu-id="50991-102">Keresse meg a hello cél verzióját hello használt Android SDK célverzióját, nyissa meg kattintva **csomag részletek megjelenítése**, és válassza a **Google API-k**, ha még nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="50991-102">Locate hello target version of hello Android SDK that is used in your project , open it by clicking **Show Package Details**, and choose **Google APIs**, if it is not already installed.</span></span>
2. <span data-ttu-id="50991-103">Kattintson a hello **SDK Tools** fülre. Ha még nem telepítette a Google Play szolgáltatást, kattintson a **Google Play Services** (Google Play szolgáltatások) elemre, ahogy az ábra mutatja.</span><span class="sxs-lookup"><span data-stu-id="50991-103">Click hello **SDK Tools** tab. If you haven't already installed Google Play Service, click **Google Play Services** as shown below.</span></span> <span data-ttu-id="50991-104">Kattintson a **alkalmaz** tooinstall.</span><span class="sxs-lookup"><span data-stu-id="50991-104">Then click **Apply** tooinstall.</span></span> 
   
    <span data-ttu-id="50991-105">Megjegyzés: hello SDK elérési út egy későbbi lépésben használatra.</span><span class="sxs-lookup"><span data-stu-id="50991-105">Note hello SDK path, for use in a later step.</span></span> 
   
    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. <span data-ttu-id="50991-106">Nyissa meg hello **build.gradle** hello app fájl.</span><span class="sxs-lookup"><span data-stu-id="50991-106">Open hello **build.gradle** file in hello app directory.</span></span>
   
    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-add-google-play-dependency.png)
4. <span data-ttu-id="50991-107">Adja hozzá ezt a sort a *dependencies* (függőségek) sor alatt:</span><span class="sxs-lookup"><span data-stu-id="50991-107">Add this line under *dependencies*:</span></span> 
   
           compile 'com.google.android.gms:play-services-gcm:9.2.0'
5. <span data-ttu-id="50991-108">Kattintson a hello **projekt szinkronizálása a Gradle-fájlokkal** hello eszköztáron ikonra.</span><span class="sxs-lookup"><span data-stu-id="50991-108">Click hello **Sync Project with Gradle Files** icon in hello tool bar.</span></span>
6. <span data-ttu-id="50991-109">Nyissa meg **AndroidManifest.xml** , és adja hozzá a címke toohello *alkalmazás* címke.</span><span class="sxs-lookup"><span data-stu-id="50991-109">Open **AndroidManifest.xml** and add this tag toohello *application* tag.</span></span>
   
        <meta-data android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version" />

