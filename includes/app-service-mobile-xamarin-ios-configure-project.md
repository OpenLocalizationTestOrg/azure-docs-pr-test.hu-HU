#### <a name="configure-the-ios-project-in-xamarin-studio"></a><span data-ttu-id="47c33-101">A Xamarin Studióban az iOS-projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="47c33-101">Configure the iOS project in Xamarin Studio</span></span>
1. <span data-ttu-id="47c33-102">Xamarin.Studio, nyissa meg **Info.plist**, és frissítse a **Bundle Identifier** az új alkalmazást. a korábban létrehozott csomagot-azonosítóval</span><span class="sxs-lookup"><span data-stu-id="47c33-102">In Xamarin.Studio, open **Info.plist**, and update the **Bundle Identifier** with the bundle ID that you created earlier with your new app ID.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)
2. <span data-ttu-id="47c33-103">Görgessen le a **Háttérmódok**.</span><span class="sxs-lookup"><span data-stu-id="47c33-103">Scroll down to **Background Modes**.</span></span> <span data-ttu-id="47c33-104">Válassza ki a **Háttérmódok engedélyezése** bezárásához és a **távoli értesítések** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="47c33-104">Select the **Enable Background Modes** box and the **Remote notifications** box.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)
3. <span data-ttu-id="47c33-105">Kattintson duplán a projektet, nyissa meg a megoldást panelen **Projektbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="47c33-105">Double-click your project in the Solution Panel to open **Project Options**.</span></span>
4. <span data-ttu-id="47c33-106">A **Build**, válassza a **iOS alkalmazáscsomag aláírása**, és válassza ki a megfelelő identitás létesítési profil és akkor imént beállított fel ebben a projektben.</span><span class="sxs-lookup"><span data-stu-id="47c33-106">Under **Build**, choose **iOS Bundle Signing**, and select the corresponding identity and provisioning profile you just set up for this project.</span></span>

   ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

   <span data-ttu-id="47c33-107">Ez biztosítja, hogy használja-e a projektet az új profil tartozó kód aláírása.</span><span class="sxs-lookup"><span data-stu-id="47c33-107">This ensures that the project uses the new profile for code signing.</span></span> <span data-ttu-id="47c33-108">A hivatalos Xamarin eszköz-üzembehelyezési dokumentáció, lásd: [Xamarin eszköz kiépítése].</span><span class="sxs-lookup"><span data-stu-id="47c33-108">For the official Xamarin device provisioning documentation, see [Xamarin Device Provisioning].</span></span>

#### <a name="configure-the-ios-project-in-visual-studio"></a><span data-ttu-id="47c33-109">Az iOS-projektre a Visual Studio konfigurálása</span><span class="sxs-lookup"><span data-stu-id="47c33-109">Configure the iOS project in Visual Studio</span></span>
1. <span data-ttu-id="47c33-110">A Visual Studióban, kattintson jobb gombbal a projektre, és kattintson **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="47c33-110">In Visual Studio, right-click the project, and then click **Properties**.</span></span>
2. <span data-ttu-id="47c33-111">A Tulajdonságok lapon kattintson a **iOS alkalmazás** lapot, és frissítse a **azonosító** a korábban létrehozott Azonosítóval rendelkező.</span><span class="sxs-lookup"><span data-stu-id="47c33-111">In the properties pages, click the **iOS Application** tab, and update the **Identifier** with the ID that you created earlier.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)
3. <span data-ttu-id="47c33-112">Az a **iOS alkalmazáscsomag aláírása** lapon válassza ki a megfelelő identitás és létesítési profil imént beállította ebben a projektben.</span><span class="sxs-lookup"><span data-stu-id="47c33-112">In the **iOS Bundle Signing** tab, select the corresponding identity and provisioning profile you just set up for this project.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    <span data-ttu-id="47c33-113">Ez biztosítja, hogy használja-e a projektet az új profil tartozó kód aláírása.</span><span class="sxs-lookup"><span data-stu-id="47c33-113">This ensures that the project uses the new profile for code signing.</span></span> <span data-ttu-id="47c33-114">A hivatalos Xamarin eszköz-üzembehelyezési dokumentáció, lásd: [Xamarin eszköz kiépítése].</span><span class="sxs-lookup"><span data-stu-id="47c33-114">For the official Xamarin device provisioning documentation, see [Xamarin Device Provisioning].</span></span>
4. <span data-ttu-id="47c33-115">Kattintson duplán az Info.plist-ben nyissa meg azt, és engedélyezze a **RemoteNotifications** alatt **Háttérmódok**.</span><span class="sxs-lookup"><span data-stu-id="47c33-115">Double-click Info.plist to open it, and then enable **RemoteNotifications** under **Background Modes**.</span></span>

<span data-ttu-id="47c33-116">[Xamarin eszköz kiépítése]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/</span><span class="sxs-lookup"><span data-stu-id="47c33-116">[Xamarin Device Provisioning]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/</span></span>
