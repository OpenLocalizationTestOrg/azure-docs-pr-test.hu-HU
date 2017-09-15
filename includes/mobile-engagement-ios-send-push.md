### <a name="grant-access-to-your-push-certificate-to-mobile-engagement"></a><span data-ttu-id="cf128-101">A leküldéses tanúsítványhoz való hozzáférés biztosítása a Mobile Engagement számára</span><span class="sxs-lookup"><span data-stu-id="cf128-101">Grant access to your Push Certificate to Mobile Engagement</span></span>
<span data-ttu-id="cf128-102">Ha engedélyezni szeretné, hogy a Mobile Engagement leküldéses értesítéseket küldjön az Ön nevében, hozzáférést kell biztosítania számára a tanúsítványhoz.</span><span class="sxs-lookup"><span data-stu-id="cf128-102">To allow Mobile Engagement to send Push Notifications on your behalf, you need to grant it access to your certificate.</span></span> <span data-ttu-id="cf128-103">Ezt a tanúsítvány konfigurálásával, valamint annak a Mobile Engagement portálon történő megadásával teheti meg.</span><span class="sxs-lookup"><span data-stu-id="cf128-103">This is done by configuring and entering your certificate into the Mobile Engagement portal.</span></span> <span data-ttu-id="cf128-104">Győződjön meg róla, hogy az [Apple dokumentációjában](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6) leírtak alapján szerezte be .p12-tanúsítványát.</span><span class="sxs-lookup"><span data-stu-id="cf128-104">Make sure you obtain your .p12 certificate as explained in [Apple's documentation](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)</span></span>

1. <span data-ttu-id="cf128-105">Lépjen a Mobile Engagement portálra.</span><span class="sxs-lookup"><span data-stu-id="cf128-105">Navigate to your Mobile Engagement portal.</span></span> <span data-ttu-id="cf128-106">Győződjön meg arról, hogy a megfelelő helyen van, majd kattintson a lap alján található **Engage** (Aktiválás) gombra:</span><span class="sxs-lookup"><span data-stu-id="cf128-106">Ensure you're in the correct and then click on the **Engage** button at the bottom:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/engage-button.png)
2. <span data-ttu-id="cf128-107">Kattintson a **Settings** (Beállítások) lapra az Engagement portálon.</span><span class="sxs-lookup"><span data-stu-id="cf128-107">Click on the **Settings** page in your Engagement Portal.</span></span> <span data-ttu-id="cf128-108">Itt kattintson a **Native Push** (Natív leküldés) szakaszra a p12-tanúsítvány feltöltéséhez:</span><span class="sxs-lookup"><span data-stu-id="cf128-108">From there click on the **Native Push** section to upload your p12 certificate:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/engagement-portal.png)
3. <span data-ttu-id="cf128-109">Válassza ki a p12-tanúsítványát, töltse fel, majd írja be a jelszavát:</span><span class="sxs-lookup"><span data-stu-id="cf128-109">Select your p12, upload it and type your password:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/native-push-settings.png)

## <span data-ttu-id="cf128-110"><a id="send"></a>Értesítés küldése az alkalmazásnak</span><span class="sxs-lookup"><span data-stu-id="cf128-110"><a id="send"></a>Send a notification to your app</span></span>
<span data-ttu-id="cf128-111">Most létrehozunk egy egyszerű leküldéses értesítési kampányt, amely elküld egy leküldéses értesítést az alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="cf128-111">We will now create a simple Push Notification campaign that will send a push to our app:</span></span>

1. <span data-ttu-id="cf128-112">Lépjen a Mobile Engagement portál **Reach** (Elérés) lapjára.</span><span class="sxs-lookup"><span data-stu-id="cf128-112">Navigate to the **Reach** tab in your Mobile Engagement portal.</span></span>
2. <span data-ttu-id="cf128-113">Kattintson a **New Announcement** (Új értesítés) elemre a leküldéses kampány létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="cf128-113">Click **New Announcement** to create your push campaign</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/new-announcement.png)
3. <span data-ttu-id="cf128-114">Adja meg a kampány első mezőinek értékét:</span><span class="sxs-lookup"><span data-stu-id="cf128-114">Setup the first fields of your campaign:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/campaign-first-params.png)
   
   * <span data-ttu-id="cf128-115">A **Name** (Név) mezőben adja meg a kampány nevét.</span><span class="sxs-lookup"><span data-stu-id="cf128-115">Provide a **Name** for your campaign</span></span> 
   * <span data-ttu-id="cf128-116">A **Delivey time** (Kézbesítés időpontja) beállítást állítsa **Out of app only** (Csak alkalmazáson kívül) értékre. Ez egy egyszerű Apple leküldéses értesítési típus, amely egy kevés szöveget tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="cf128-116">Select the **Delivery time** as **Out of app only**: this is the simple Apple push notification type that features some text.</span></span>
   * <span data-ttu-id="cf128-117">Az értesítés szövegéhez először adja meg a címet (**Title**), amely a leküldéses üzenetben az elsős sorban jelenik majd meg.</span><span class="sxs-lookup"><span data-stu-id="cf128-117">In the notification text, type first the **Title** which will be the first line in the push.</span></span>
   * <span data-ttu-id="cf128-118">Ezután írja be az üzenetét (**Message**), amely a második sorban jelenik majd meg.</span><span class="sxs-lookup"><span data-stu-id="cf128-118">Then type your **Message** which will be the second line</span></span>
4. <span data-ttu-id="cf128-119">Görgessen lefelé, és a tartalom szakaszban válassza a **Notification only** (Csak értesítés) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="cf128-119">Scroll down, and in the content section select **Notification only**</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/campaign-content.png)
5. <span data-ttu-id="cf128-120">Ezzel beállította a lehető legegyszerűbb kampányt.</span><span class="sxs-lookup"><span data-stu-id="cf128-120">You're done setting the most basic campaign.</span></span> <span data-ttu-id="cf128-121">Görgessen lefelé, és kattintson a **Create** (Létrehozás) gombra a leküldéses értesítési kampány mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="cf128-121">Now scroll down and click on **Create** button to save your push notification campaign.</span></span> 
6. <span data-ttu-id="cf128-122">Végezetül kattintson az **Activate** (Aktiválás) gombra a leküldéses értesítés kiküldéséhez.</span><span class="sxs-lookup"><span data-stu-id="cf128-122">Finally - click on **Activate** to send push notification.</span></span> 
   
    ![](./media/mobile-engagement-ios-send-push/campaign-activate.png)
7. <span data-ttu-id="cf128-123">Az értesítést az alábbi formában fogadhatja iOS-eszközének értesítési központjában:</span><span class="sxs-lookup"><span data-stu-id="cf128-123">You will be able receive the notification on your iOS device in the notification center like the following:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/iphone-notification.png)
8. <span data-ttu-id="cf128-124">Ha rendelkezik egy, az iOS-eszközzel párosított Apple Watchcsal, akkor az értesítés az óráján jelenik majd meg.</span><span class="sxs-lookup"><span data-stu-id="cf128-124">If you have an Apple Watch paired with this iOS device then you will see the notification on your Apple Watch:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/apple-watch.png)

