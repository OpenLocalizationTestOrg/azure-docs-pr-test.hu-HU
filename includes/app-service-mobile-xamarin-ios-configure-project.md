#### <a name="configure-hello-ios-project-in-xamarin-studio"></a><span data-ttu-id="8601e-101">A Xamarin Studio hello iOS-projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8601e-101">Configure hello iOS project in Xamarin Studio</span></span>
1. <span data-ttu-id="8601e-102">Xamarin.Studio, nyissa meg **Info.plist**, és a frissítés hello **Bundle Identifier** hello a csomagot az új alkalmazást. a korábban létrehozott azonosító</span><span class="sxs-lookup"><span data-stu-id="8601e-102">In Xamarin.Studio, open **Info.plist**, and update hello **Bundle Identifier** with hello bundle ID that you created earlier with your new app ID.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)
2. <span data-ttu-id="8601e-103">Görgessen lefelé, túl**Háttérmódok**.</span><span class="sxs-lookup"><span data-stu-id="8601e-103">Scroll down too**Background Modes**.</span></span> <span data-ttu-id="8601e-104">Jelölje be hello **Háttérmódok engedélyezése** mezőbe, majd hello **távoli értesítések** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="8601e-104">Select hello **Enable Background Modes** box and hello **Remote notifications** box.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)
3. <span data-ttu-id="8601e-105">Kattintson duplán a projektre a hello megoldás Panel tooopen **Projektbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="8601e-105">Double-click your project in hello Solution Panel tooopen **Project Options**.</span></span>
4. <span data-ttu-id="8601e-106">A **Build**, válassza ki **iOS alkalmazáscsomag aláírása**, és válassza ki a megfelelő identitás hello és létesítési profil imént beállított ebben a projektben.</span><span class="sxs-lookup"><span data-stu-id="8601e-106">Under **Build**, choose **iOS Bundle Signing**, and select hello corresponding identity and provisioning profile you just set up for this project.</span></span>

   ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

   <span data-ttu-id="8601e-107">Ez biztosítja, hogy adott hello project kódaláíró hello új profilt használja.</span><span class="sxs-lookup"><span data-stu-id="8601e-107">This ensures that hello project uses hello new profile for code signing.</span></span> <span data-ttu-id="8601e-108">Hello hivatalos Xamarin eszköz-üzembehelyezési dokumentáció, lásd: [Xamarin eszköz kiépítése].</span><span class="sxs-lookup"><span data-stu-id="8601e-108">For hello official Xamarin device provisioning documentation, see [Xamarin Device Provisioning].</span></span>

#### <a name="configure-hello-ios-project-in-visual-studio"></a><span data-ttu-id="8601e-109">A Visual Studio hello iOS-projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8601e-109">Configure hello iOS project in Visual Studio</span></span>
1. <span data-ttu-id="8601e-110">A Visual Studióban, kattintson a jobb gombbal a hello projektet, és kattintson **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="8601e-110">In Visual Studio, right-click hello project, and then click **Properties**.</span></span>
2. <span data-ttu-id="8601e-111">Hello tulajdonságai lapon kattintson a hello **iOS alkalmazás** fülre, majd a frissítés hello **azonosító** korábban létrehozott hello azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="8601e-111">In hello properties pages, click hello **iOS Application** tab, and update hello **Identifier** with hello ID that you created earlier.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)
3. <span data-ttu-id="8601e-112">A hello **iOS alkalmazáscsomag aláírása** lapra, jelölje be hello megfelelő identitás létesítési profil és Ön imént beállított fel ebben a projektben.</span><span class="sxs-lookup"><span data-stu-id="8601e-112">In hello **iOS Bundle Signing** tab, select hello corresponding identity and provisioning profile you just set up for this project.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    <span data-ttu-id="8601e-113">Ez biztosítja, hogy adott hello project kódaláíró hello új profilt használja.</span><span class="sxs-lookup"><span data-stu-id="8601e-113">This ensures that hello project uses hello new profile for code signing.</span></span> <span data-ttu-id="8601e-114">Hello hivatalos Xamarin eszköz-üzembehelyezési dokumentáció, lásd: [Xamarin eszköz kiépítése].</span><span class="sxs-lookup"><span data-stu-id="8601e-114">For hello official Xamarin device provisioning documentation, see [Xamarin Device Provisioning].</span></span>
4. <span data-ttu-id="8601e-115">Kattintson duplán az Info.plist tooopen és majd engedélyezése **RemoteNotifications** alatt **Háttérmódok**.</span><span class="sxs-lookup"><span data-stu-id="8601e-115">Double-click Info.plist tooopen it, and then enable **RemoteNotifications** under **Background Modes**.</span></span>

[Xamarin eszköz kiépítése]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/
