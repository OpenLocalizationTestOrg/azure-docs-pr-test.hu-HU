> [!IMPORTANT]
> <span data-ttu-id="da2ba-101">Leküldéses értesítések a Mobile Engagement tooreceive, kell tooenable `Silent Remote Notifications` az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="da2ba-101">tooreceive Push Notifications from Mobile Engagement, you need tooenable `Silent Remote Notifications` in your application.</span></span> <span data-ttu-id="da2ba-102">Az Info.plist fájlban kell tooadd hello távértesítési értéket toohello UIBackgroundModes tömbhöz.</span><span class="sxs-lookup"><span data-stu-id="da2ba-102">You need tooadd hello remote-notification value toohello UIBackgroundModes array in your Info.plist file.</span></span>
> 
> 

1. <span data-ttu-id="da2ba-103">Nyissa meg `info.plist` hello projektben fájl</span><span class="sxs-lookup"><span data-stu-id="da2ba-103">Open `info.plist` file in hello project</span></span>
2. <span data-ttu-id="da2ba-104">Kattintson jobb gombbal a hello hello lista legfelső elemére (`Information Property List`) és egy új sort ad hozzá</span><span class="sxs-lookup"><span data-stu-id="da2ba-104">Right click on hello top item in hello list (`Information Property List`) and add a new row</span></span>
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push1.png)
3. <span data-ttu-id="da2ba-105">Hello új sorban adjon meg`Required background modes`</span><span class="sxs-lookup"><span data-stu-id="da2ba-105">In hello new row enter `Required background modes`</span></span>
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push2.png)
4. <span data-ttu-id="da2ba-106">Kattintson a hello balra mutató nyílra tooexpand hello sor</span><span class="sxs-lookup"><span data-stu-id="da2ba-106">Click on hello left arrow tooexpand hello row</span></span>
5. <span data-ttu-id="da2ba-107">A következő érték toohello elem 0 hello hozzáadása`App downloads content in response toopush notifications`</span><span class="sxs-lookup"><span data-stu-id="da2ba-107">Add hello following value toohello item 0 `App downloads content in response toopush notifications`</span></span>
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push3.png)
6. <span data-ttu-id="da2ba-108">Hello módosítása után, hello info.plist XML tartalmaznia kell a következő hello kulcs, és értéket:</span><span class="sxs-lookup"><span data-stu-id="da2ba-108">Once you make hello change, hello info.plist XML should contain hello following key and value:</span></span>
   
        <key>UIBackgroundModes</key>
        <array>
        <string>remote-notification</string>
        </array>
7. <span data-ttu-id="da2ba-109">**Xcode 7+** és **iOS 9+** használata esetén:</span><span class="sxs-lookup"><span data-stu-id="da2ba-109">If you are using **Xcode 7+** and **iOS 9+**:</span></span>
   
   * <span data-ttu-id="da2ba-110">Engedélyezze a **Push Notifications** (leküldéses értesítések) beállítást a Targets (Célok) > Your Target Name (Saját cél neve) > Capabilities (Képességek) menüben.</span><span class="sxs-lookup"><span data-stu-id="da2ba-110">Enable **Push Notifications** in Targets > Your Target Name > Capabilities.</span></span>

