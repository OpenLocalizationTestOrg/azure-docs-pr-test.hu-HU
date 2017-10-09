### <a name="grant-access-tooyour-push-certificate-toomobile-engagement"></a><span data-ttu-id="546a0-101">Adjon hozzáférést tooyour leküldéses tanúsítványhoz tooMobile Engagement</span><span class="sxs-lookup"><span data-stu-id="546a0-101">Grant access tooyour Push Certificate tooMobile Engagement</span></span>
<span data-ttu-id="546a0-102">tooallow a Mobile Engagement toosend leküldéses értesítések küldése az Ön nevében, meg kell azt elérni tooyour tanúsítvány toogrant.</span><span class="sxs-lookup"><span data-stu-id="546a0-102">tooallow Mobile Engagement toosend Push Notifications on your behalf, you need toogrant it access tooyour certificate.</span></span> <span data-ttu-id="546a0-103">Ehhez írja be a tanúsítvány hello a Mobile Engagement portált és konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="546a0-103">This is done by configuring and entering your certificate into hello Mobile Engagement portal.</span></span> <span data-ttu-id="546a0-104">Győződjön meg róla, hogy az [Apple dokumentációjában](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6) leírtak alapján szerezte be .p12-tanúsítványát.</span><span class="sxs-lookup"><span data-stu-id="546a0-104">Make sure you obtain your .p12 certificate as explained in [Apple's documentation](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)</span></span>

1. <span data-ttu-id="546a0-105">Keresse meg a tooyour Mobile Engagement portálon.</span><span class="sxs-lookup"><span data-stu-id="546a0-105">Navigate tooyour Mobile Engagement portal.</span></span> <span data-ttu-id="546a0-106">Győződjön meg arról is a megfelelő hello, és kattintson a hello **Engage** hello alsó gombra:</span><span class="sxs-lookup"><span data-stu-id="546a0-106">Ensure you're in hello correct and then click on hello **Engage** button at hello bottom:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/engage-button.png)
2. <span data-ttu-id="546a0-107">Kattintson a hello **beállítások** lapra az Engagement portálon.</span><span class="sxs-lookup"><span data-stu-id="546a0-107">Click on hello **Settings** page in your Engagement Portal.</span></span> <span data-ttu-id="546a0-108">Kattintson a hello **natív leküldés** szakasz tooupload a p12-tanúsítvány:</span><span class="sxs-lookup"><span data-stu-id="546a0-108">From there click on hello **Native Push** section tooupload your p12 certificate:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/engagement-portal.png)
3. <span data-ttu-id="546a0-109">Válassza ki a p12-tanúsítványát, töltse fel, majd írja be a jelszavát:</span><span class="sxs-lookup"><span data-stu-id="546a0-109">Select your p12, upload it and type your password:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/native-push-settings.png)

## <span data-ttu-id="546a0-110"><a id="send"></a>Egy értesítési tooyour app küldése</span><span class="sxs-lookup"><span data-stu-id="546a0-110"><a id="send"></a>Send a notification tooyour app</span></span>
<span data-ttu-id="546a0-111">Most létrehozunk egy egyszerű leküldéses értesítési kampányt, amely elküld egy leküldéses tooour alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="546a0-111">We will now create a simple Push Notification campaign that will send a push tooour app:</span></span>

1. <span data-ttu-id="546a0-112">Keresse meg a toohello **elérni** a Mobile Engagement portálra a lap.</span><span class="sxs-lookup"><span data-stu-id="546a0-112">Navigate toohello **Reach** tab in your Mobile Engagement portal.</span></span>
2. <span data-ttu-id="546a0-113">Kattintson a **új hirdetmény** toocreate a leküldéses kampány</span><span class="sxs-lookup"><span data-stu-id="546a0-113">Click **New Announcement** toocreate your push campaign</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/new-announcement.png)
3. <span data-ttu-id="546a0-114">A telepítő a kampány első mezőinek hello:</span><span class="sxs-lookup"><span data-stu-id="546a0-114">Setup hello first fields of your campaign:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/campaign-first-params.png)
   
   * <span data-ttu-id="546a0-115">A **Name** (Név) mezőben adja meg a kampány nevét.</span><span class="sxs-lookup"><span data-stu-id="546a0-115">Provide a **Name** for your campaign</span></span> 
   * <span data-ttu-id="546a0-116">Jelölje be hello **kézbesítési időhöz** , **csak az alkalmazáson kívül**: Ez az hello egyszerű Apple leküldéses értesítési típus, amely egy kevés szöveget.</span><span class="sxs-lookup"><span data-stu-id="546a0-116">Select hello **Delivery time** as **Out of app only**: this is hello simple Apple push notification type that features some text.</span></span>
   * <span data-ttu-id="546a0-117">Hello értesítés szövegéhez, írja be az első hello **cím** amely lesz hello hello leküldéses első sorában.</span><span class="sxs-lookup"><span data-stu-id="546a0-117">In hello notification text, type first hello **Title** which will be hello first line in hello push.</span></span>
   * <span data-ttu-id="546a0-118">Írja be a **üzenet** amely lesz hello második sor</span><span class="sxs-lookup"><span data-stu-id="546a0-118">Then type your **Message** which will be hello second line</span></span>
4. <span data-ttu-id="546a0-119">Görgessen lefelé, és a hello tartalomszakasz válassza **csak értesítés**</span><span class="sxs-lookup"><span data-stu-id="546a0-119">Scroll down, and in hello content section select **Notification only**</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/campaign-content.png)
5. <span data-ttu-id="546a0-120">Elkészült a beállítással hello lehető legegyszerűbb kampányt.</span><span class="sxs-lookup"><span data-stu-id="546a0-120">You're done setting hello most basic campaign.</span></span> <span data-ttu-id="546a0-121">Görgessen lefelé, és kattintson a **létrehozása** toosave gombra a leküldéses értesítési kampány.</span><span class="sxs-lookup"><span data-stu-id="546a0-121">Now scroll down and click on **Create** button toosave your push notification campaign.</span></span> 
6. <span data-ttu-id="546a0-122">Végezetül kattintson a **aktiválás** toosend leküldéses értesítést.</span><span class="sxs-lookup"><span data-stu-id="546a0-122">Finally - click on **Activate** toosend push notification.</span></span> 
   
    ![](./media/mobile-engagement-ios-send-push/campaign-activate.png)
7. <span data-ttu-id="546a0-123">Akkor lehet értesítés hello iOS-eszközön a hello következő hello értesítési központjában:</span><span class="sxs-lookup"><span data-stu-id="546a0-123">You will be able receive hello notification on your iOS device in hello notification center like hello following:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/iphone-notification.png)
8. <span data-ttu-id="546a0-124">Ha egy az iOS-eszközzel párosított Apple Watch majd jelenik meg hello értesítési meg az Apple Watch:</span><span class="sxs-lookup"><span data-stu-id="546a0-124">If you have an Apple Watch paired with this iOS device then you will see hello notification on your Apple Watch:</span></span>
   
    ![](./media/mobile-engagement-ios-send-push/apple-watch.png)

