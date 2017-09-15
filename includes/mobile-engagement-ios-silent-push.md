> [!IMPORTANT]
> <span data-ttu-id="ca817-101">Ahhoz, hogy leküldéses értesítéseket fogadjon a Mobile Engagement programtól, engedélyeznie kell a `Silent Remote Notifications` elemet az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ca817-101">To receive Push Notifications from Mobile Engagement, you need to enable `Silent Remote Notifications` in your application.</span></span> <span data-ttu-id="ca817-102">Az Info.plist fájlban hozzá kell adnia a távértesítési értéket az UIBackgroundModes tömbhöz.</span><span class="sxs-lookup"><span data-stu-id="ca817-102">You need to add the remote-notification value to the UIBackgroundModes array in your Info.plist file.</span></span>
> 
> 

1. <span data-ttu-id="ca817-103">Nyissa meg az `info.plist` fájlt a projektben</span><span class="sxs-lookup"><span data-stu-id="ca817-103">Open `info.plist` file in the project</span></span>
2. <span data-ttu-id="ca817-104">Kattintson a jobb egérgombbal a lista legfelső elemére (`Information Property List`), és adjon hozzá egy új sort</span><span class="sxs-lookup"><span data-stu-id="ca817-104">Right click on the top item in the list (`Information Property List`) and add a new row</span></span>
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push1.png)
3. <span data-ttu-id="ca817-105">Adja meg a következőt az új sorban `Required background modes`</span><span class="sxs-lookup"><span data-stu-id="ca817-105">In the new row enter `Required background modes`</span></span>
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push2.png)
4. <span data-ttu-id="ca817-106">Kattintson a balra mutató nyílra a sor kibontásához</span><span class="sxs-lookup"><span data-stu-id="ca817-106">Click on the left arrow to expand the row</span></span>
5. <span data-ttu-id="ca817-107">Adja a következő értéket a 0. elemhez `App downloads content in response to push notifications`</span><span class="sxs-lookup"><span data-stu-id="ca817-107">Add the following value to the item 0 `App downloads content in response to push notifications`</span></span>
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push3.png)
6. <span data-ttu-id="ca817-108">A módosítás elvégzése után az info.plist XML-fájlnak a következő kulcsot és értéket kell tartalmaznia:</span><span class="sxs-lookup"><span data-stu-id="ca817-108">Once you make the change, the info.plist XML should contain the following key and value:</span></span>
   
        <key>UIBackgroundModes</key>
        <array>
        <string>remote-notification</string>
        </array>
7. <span data-ttu-id="ca817-109">**Xcode 7+** és **iOS 9+** használata esetén:</span><span class="sxs-lookup"><span data-stu-id="ca817-109">If you are using **Xcode 7+** and **iOS 9+**:</span></span>
   
   * <span data-ttu-id="ca817-110">Engedélyezze a **Push Notifications** (leküldéses értesítések) beállítást a Targets (Célok) > Your Target Name (Saját cél neve) > Capabilities (Képességek) menüben.</span><span class="sxs-lookup"><span data-stu-id="ca817-110">Enable **Push Notifications** in Targets > Your Target Name > Capabilities.</span></span>

