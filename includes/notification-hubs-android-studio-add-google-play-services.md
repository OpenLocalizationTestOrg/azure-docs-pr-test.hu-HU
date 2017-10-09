1. Nyissa meg a hello Android SDK Manager hello eszköztár Android Studio hello ikonjára kattintva vagy kattintva **eszközök** -> **Android** -> **SDK Manager**hello menüben. Keresse meg a hello cél verzióját hello használt Android SDK célverzióját, nyissa meg kattintva **csomag részletek megjelenítése**, és válassza a **Google API-k**, ha még nincs telepítve.
2. Kattintson a hello **SDK Tools** fülre. Ha még nem telepítette a Google Play szolgáltatást, kattintson a **Google Play Services** (Google Play szolgáltatások) elemre, ahogy az ábra mutatja. Kattintson a **alkalmaz** tooinstall. 
   
    Megjegyzés: hello SDK elérési út egy későbbi lépésben használatra. 
   
    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. Nyissa meg hello **build.gradle** hello app fájl.
   
    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-add-google-play-dependency.png)
4. Adja hozzá ezt a sort a *dependencies* (függőségek) sor alatt: 
   
           compile 'com.google.android.gms:play-services-gcm:9.2.0'
5. Kattintson a hello **projekt szinkronizálása a Gradle-fájlokkal** hello eszköztáron ikonra.
6. Nyissa meg **AndroidManifest.xml** , és adja hozzá a címke toohello *alkalmazás* címke.
   
        <meta-data android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version" />

