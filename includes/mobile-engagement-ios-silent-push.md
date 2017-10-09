> [!IMPORTANT]
> Leküldéses értesítések a Mobile Engagement tooreceive, kell tooenable `Silent Remote Notifications` az alkalmazásban. Az Info.plist fájlban kell tooadd hello távértesítési értéket toohello UIBackgroundModes tömbhöz.
> 
> 

1. Nyissa meg `info.plist` hello projektben fájl
2. Kattintson jobb gombbal a hello hello lista legfelső elemére (`Information Property List`) és egy új sort ad hozzá
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push1.png)
3. Hello új sorban adjon meg`Required background modes`
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push2.png)
4. Kattintson a hello balra mutató nyílra tooexpand hello sor
5. A következő érték toohello elem 0 hello hozzáadása`App downloads content in response toopush notifications`
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push3.png)
6. Hello módosítása után, hello info.plist XML tartalmaznia kell a következő hello kulcs, és értéket:
   
        <key>UIBackgroundModes</key>
        <array>
        <string>remote-notification</string>
        </array>
7. **Xcode 7+** és **iOS 9+** használata esetén:
   
   * Engedélyezze a **Push Notifications** (leküldéses értesítések) beállítást a Targets (Célok) > Your Target Name (Saját cél neve) > Capabilities (Képességek) menüben.

