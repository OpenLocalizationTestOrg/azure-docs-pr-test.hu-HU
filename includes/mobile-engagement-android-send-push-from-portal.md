### <a name="grant-mobile-engagement-access-tooyour-gcm-api-key"></a><span data-ttu-id="a5a43-101">Engedélyezze a Mobile Engagement hozzáférés tooyour GCM API-kulcs</span><span class="sxs-lookup"><span data-stu-id="a5a43-101">Grant Mobile Engagement access tooyour GCM API Key</span></span>
<span data-ttu-id="a5a43-102">tooallow a Mobile Engagement toosend leküldéses értesítések az Ön nevében, meg kell azt tooyour API-kulcs hozzáférési toogrant.</span><span class="sxs-lookup"><span data-stu-id="a5a43-102">tooallow Mobile Engagement toosend push notifications on your behalf, you need toogrant it access tooyour API Key.</span></span> <span data-ttu-id="a5a43-103">Ez történik, konfigurálásával és a kulcs megadása hello a Mobile Engagement portált.</span><span class="sxs-lookup"><span data-stu-id="a5a43-103">This is done by configuring and entering your key into hello Mobile Engagement portal.</span></span>

1. <span data-ttu-id="a5a43-104">A klasszikus Azure portálon, ellenőrizze a van a hello alkalmazás azt a projekt használ, és kattintson a hello **Engage** hello alsó gombra:</span><span class="sxs-lookup"><span data-stu-id="a5a43-104">From your Azure Classic Portal, ensure you're in hello app we're using for this project, and then click hello **Engage** button at hello bottom:</span></span>
   
    ![](./media/mobile-engagement-android-send-push/engage-button.png)
2. <span data-ttu-id="a5a43-105">Kattintson a hello **beállítások** -> **natív leküldés** szakasz tooenter a GCM-kulcs:</span><span class="sxs-lookup"><span data-stu-id="a5a43-105">Then click hello **Settings** -> **Native Push** section tooenter your GCM Key:</span></span>
   
    ![](./media/mobile-engagement-android-send-push/engagement-portal.png)
3. <span data-ttu-id="a5a43-106">Hello kattintson **szerkesztése** elé ikon **API-kulcs** a hello **GCM-beállítások** szakasz a lent látható módon:</span><span class="sxs-lookup"><span data-stu-id="a5a43-106">Click hello **Edit** icon in front of **API Key** in hello **GCM Settings** section as shown below:</span></span>
   
    ![](./media/mobile-engagement-android-send-push/native-push-settings.png)
4. <span data-ttu-id="a5a43-107">Hello előugró ablakban illessze be a hello előtt beszerzett GCM kiszolgálói kulcsot, és kattintson a **Ok**.</span><span class="sxs-lookup"><span data-stu-id="a5a43-107">In hello pop-up, paste hello GCM Server Key you obtained before and then click **Ok**.</span></span>
   
    ![](./media/mobile-engagement-android-send-push/api-key.png)

## <span data-ttu-id="a5a43-108"><a id="send"></a>Egy értesítési tooyour app küldése</span><span class="sxs-lookup"><span data-stu-id="a5a43-108"><a id="send"></a>Send a notification tooyour app</span></span>
<span data-ttu-id="a5a43-109">Most létrehozunk egy egyszerű leküldéses értesítési kampányt, amely elküld egy leküldéses értesítési tooour alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a5a43-109">We will now create a simple push notification campaign that sends a push notification tooour app.</span></span>

1. <span data-ttu-id="a5a43-110">Keresse meg a toohello **ELÉRNI** a Mobile Engagement portálra a lap.</span><span class="sxs-lookup"><span data-stu-id="a5a43-110">Navigate toohello **REACH** tab in your Mobile Engagement portal.</span></span>
2. <span data-ttu-id="a5a43-111">Kattintson a **új hirdetmény** toocreate a leküldéses értesítési kampány.</span><span class="sxs-lookup"><span data-stu-id="a5a43-111">Click **New announcement** toocreate your push notification campaign.</span></span>
   
    ![](./media/mobile-engagement-android-send-push/new-announcement.png)
3. <span data-ttu-id="a5a43-112">A következő lépéseket hello kampány első mezőjét hello beállítása:</span><span class="sxs-lookup"><span data-stu-id="a5a43-112">Set up hello first field of your campaign through hello following steps:</span></span>
   
    ![](./media/mobile-engagement-android-send-push/campaign-first-params.png)
   
    <span data-ttu-id="a5a43-113">a.</span><span class="sxs-lookup"><span data-stu-id="a5a43-113">a.</span></span> <span data-ttu-id="a5a43-114">Nevezze el a kampányt.</span><span class="sxs-lookup"><span data-stu-id="a5a43-114">Name your campaign.</span></span>
   
    <span data-ttu-id="a5a43-115">b.</span><span class="sxs-lookup"><span data-stu-id="a5a43-115">b.</span></span> <span data-ttu-id="a5a43-116">Jelölje be hello **kézbesítési típust** , *Rendszerértesítés -> egyszerű*: Ez az hello egyszerű Android leküldéses értesítési típus, amely egy címet és egy rövid szöveges sort.</span><span class="sxs-lookup"><span data-stu-id="a5a43-116">Select hello **Delivery type** as *System notification -> Simple*: This is hello simple Android push notification type that features a title and a small line of text.</span></span>
   
    <span data-ttu-id="a5a43-117">c.</span><span class="sxs-lookup"><span data-stu-id="a5a43-117">c.</span></span> <span data-ttu-id="a5a43-118">Válassza ki **kézbesítési időhöz** , *bármikor* tooallow hello app tooreceive értesítést, hogy hello alkalmazás elindult-e.</span><span class="sxs-lookup"><span data-stu-id="a5a43-118">Select **Delivery time** as *Any time* tooallow hello app tooreceive a notification whether hello app is started or not.</span></span>
   
    <span data-ttu-id="a5a43-119">d.</span><span class="sxs-lookup"><span data-stu-id="a5a43-119">d.</span></span> <span data-ttu-id="a5a43-120">A hello értesítési szöveg típusú hello **cím** amely szerepelni fog a hello leküldéses üzenetben félkövérrel.</span><span class="sxs-lookup"><span data-stu-id="a5a43-120">In hello notification text type hello **Title** which will be in bold in hello push.</span></span>
   
    <span data-ttu-id="a5a43-121">e.</span><span class="sxs-lookup"><span data-stu-id="a5a43-121">e.</span></span> <span data-ttu-id="a5a43-122">Ezután írja be az üzenetét a **Message** (Üzenet) mezőbe</span><span class="sxs-lookup"><span data-stu-id="a5a43-122">Then type your **Message**</span></span>
4. <span data-ttu-id="a5a43-123">Görgessen lefelé, és a hello **tartalom** szakaszban jelölje be **csak értesítés**.</span><span class="sxs-lookup"><span data-stu-id="a5a43-123">Scroll down, and in hello **Content** section, select **Notification only**.</span></span>
   
    ![](./media/mobile-engagement-android-send-push/campaign-content.png)
5. <span data-ttu-id="a5a43-124">Elkészült a beállítással hello lehető legegyszerűbb kampányt lehetséges.</span><span class="sxs-lookup"><span data-stu-id="a5a43-124">You're done setting hello most basic campaign possible.</span></span> <span data-ttu-id="a5a43-125">Görgessen lefelé, és kattintson a hello **létrehozása** toosave gombra a kampány.</span><span class="sxs-lookup"><span data-stu-id="a5a43-125">Now scroll down again and click hello **Create** button toosave your campaign.</span></span>
6. <span data-ttu-id="a5a43-126">Utolsó lépés: kattintson a **aktiválás** tooactivate a kampány toosend leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="a5a43-126">Last step: click **Activate** tooactivate your campaign toosend push notifications.</span></span>
   
    ![](./media/mobile-engagement-android-send-push/campaign-activate.png)

