### <a name="grant-mobile-engagement-access-to-your-gcm-api-key"></a><span data-ttu-id="daf73-101">A GCM API-kulcshoz való hozzáférés biztosítása a Mobile Engagement számára</span><span class="sxs-lookup"><span data-stu-id="daf73-101">Grant Mobile Engagement access to your GCM API Key</span></span>
<span data-ttu-id="daf73-102">Ha engedélyezni szeretné, hogy a Mobile Engagement leküldéses értesítéseket küldjön az Ön nevében, hozzáférést kell biztosítania számára az API-kulcshoz.</span><span class="sxs-lookup"><span data-stu-id="daf73-102">To allow Mobile Engagement to send push notifications on your behalf, you need to grant it access to your API Key.</span></span> <span data-ttu-id="daf73-103">Ezt a kulcs konfigurálásával, valamint annak a Mobile Engagement portálon történő megadásával teheti meg.</span><span class="sxs-lookup"><span data-stu-id="daf73-103">This is done by configuring and entering your key into the Mobile Engagement portal.</span></span>

1. <span data-ttu-id="daf73-104">A klasszikus Azure portálon győződjön meg arról, hogy be van lépve a projekthez használt alkalmazásba, majd kattintson a lap alján található **Engage** (Aktiválás) gombra:</span><span class="sxs-lookup"><span data-stu-id="daf73-104">From your Azure Classic Portal, ensure you're in the app we're using for this project, and then click the **Engage** button at the bottom:</span></span>
   
    ![](./media/mobile-engagement-android-send-push/engage-button.png)
2. <span data-ttu-id="daf73-105">Ezután kattintson a **Beállítások** -> **Natív leküldés** elemre a GCM-kulcs megadásához:</span><span class="sxs-lookup"><span data-stu-id="daf73-105">Then click the **Settings** -> **Native Push** section to enter your GCM Key:</span></span>
   
    ![](./media/mobile-engagement-android-send-push/engagement-portal.png)
3. <span data-ttu-id="daf73-106">Kattintson az **API-kulcs** melletti **Szerkesztés** ikonra a **GCM-beállítások** szakaszban, ahogy az ábra is mutatja:</span><span class="sxs-lookup"><span data-stu-id="daf73-106">Click the **Edit** icon in front of **API Key** in the **GCM Settings** section as shown below:</span></span>
   
    ![](./media/mobile-engagement-android-send-push/native-push-settings.png)
4. <span data-ttu-id="daf73-107">A felugró ablakban illessze be az előzőleg beszerzett GCM-kiszolgálókulcsot, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="daf73-107">In the pop-up, paste the GCM Server Key you obtained before and then click **Ok**.</span></span>
   
    ![](./media/mobile-engagement-android-send-push/api-key.png)

## <span data-ttu-id="daf73-108"><a id="send"></a>Értesítés küldése az alkalmazásnak</span><span class="sxs-lookup"><span data-stu-id="daf73-108"><a id="send"></a>Send a notification to your app</span></span>
<span data-ttu-id="daf73-109">Ezután létrehozunk egy egyszerű leküldéses értesítési kampányt, amely elküld egy leküldéses értesítést az alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="daf73-109">We will now create a simple push notification campaign that sends a push notification to our app.</span></span>

1. <span data-ttu-id="daf73-110">Lépjen a Mobile Engagement portál **REACH** (ELÉRÉS) lapjára.</span><span class="sxs-lookup"><span data-stu-id="daf73-110">Navigate to the **REACH** tab in your Mobile Engagement portal.</span></span>
2. <span data-ttu-id="daf73-111">Kattintson a **New announcement** (Új értesítés) elemre a leküldéses értesítési kampány létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="daf73-111">Click **New announcement** to create your push notification campaign.</span></span>
   
    ![](./media/mobile-engagement-android-send-push/new-announcement.png)
3. <span data-ttu-id="daf73-112">Állítsa be a kampány első mezőjét a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="daf73-112">Set up the first field of your campaign through the following steps:</span></span>
   
    ![](./media/mobile-engagement-android-send-push/campaign-first-params.png)
   
    <span data-ttu-id="daf73-113">a.</span><span class="sxs-lookup"><span data-stu-id="daf73-113">a.</span></span> <span data-ttu-id="daf73-114">Nevezze el a kampányt.</span><span class="sxs-lookup"><span data-stu-id="daf73-114">Name your campaign.</span></span>
   
    <span data-ttu-id="daf73-115">b.</span><span class="sxs-lookup"><span data-stu-id="daf73-115">b.</span></span> <span data-ttu-id="daf73-116">A Kézbesítési típust (**Delivery type**) állítsa *System notification -> Simple* (Rendszerértesítés -> Egyszerű) lehetőségre. Ez egy egyszerű Android leküldéses értesítési típus, amely egy címet és egy rövid szöveges sort tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="daf73-116">Select the **Delivery type** as *System notification -> Simple*: This is the simple Android push notification type that features a title and a small line of text.</span></span>
   
    <span data-ttu-id="daf73-117">c.</span><span class="sxs-lookup"><span data-stu-id="daf73-117">c.</span></span> <span data-ttu-id="daf73-118">A **Delivey time** (Kézbesítés időpontja) beállítást állítsa *Any time* (Bármikor) értékre. Így az alkalmazás akkor is megkapja az értesítéseket, ha nincs elindítva.</span><span class="sxs-lookup"><span data-stu-id="daf73-118">Select **Delivery time** as *Any time* to allow the app to receive a notification whether the app is started or not.</span></span>
   
    <span data-ttu-id="daf73-119">d.</span><span class="sxs-lookup"><span data-stu-id="daf73-119">d.</span></span> <span data-ttu-id="daf73-120">Az értesítés szövegéhez adja meg a címet (**Title**), amely a leküldéses üzenetben félkövérrel szedve jelenik majd meg.</span><span class="sxs-lookup"><span data-stu-id="daf73-120">In the notification text type the **Title** which will be in bold in the push.</span></span>
   
    <span data-ttu-id="daf73-121">e.</span><span class="sxs-lookup"><span data-stu-id="daf73-121">e.</span></span> <span data-ttu-id="daf73-122">Ezután írja be az üzenetét a **Message** (Üzenet) mezőbe</span><span class="sxs-lookup"><span data-stu-id="daf73-122">Then type your **Message**</span></span>
4. <span data-ttu-id="daf73-123">Görgessen lefelé, és a **Content** (Tartalom) szakaszban válassza a **Notification only** (Csak értesítés) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="daf73-123">Scroll down, and in the **Content** section, select **Notification only**.</span></span>
   
    ![](./media/mobile-engagement-android-send-push/campaign-content.png)
5. <span data-ttu-id="daf73-124">Ezzel beállította a lehető legegyszerűbb kampányt.</span><span class="sxs-lookup"><span data-stu-id="daf73-124">You're done setting the most basic campaign possible.</span></span> <span data-ttu-id="daf73-125">Görgessen újra le, és kattintson a **Create** (Létrehozás) gombra a kampány mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="daf73-125">Now scroll down again and click the **Create** button to save your campaign.</span></span>
6. <span data-ttu-id="daf73-126">Utolsó lépés: Kattintson az **Activate** (Aktiválás) lehetőségre a kampány aktiválásához és a leküldéses értesítések küldésének megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="daf73-126">Last step: click **Activate** to activate your campaign to send push notifications.</span></span>
   
    ![](./media/mobile-engagement-android-send-push/campaign-activate.png)

