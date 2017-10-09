

## <a name="generate-hello-certificate-signing-request-file"></a><span data-ttu-id="d1ed0-101">Hello tanúsítvány-aláírási kérelem fájljának létrehozása</span><span class="sxs-lookup"><span data-stu-id="d1ed0-101">Generate hello Certificate Signing Request file</span></span>
<span data-ttu-id="d1ed0-102">hello Apple leküldéses értesítési (APN) szolgáltatás által használt tanúsítványok tooauthenticate a leküldéses értesítések.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-102">hello Apple Push Notification Service (APNS) uses certificates tooauthenticate your push notifications.</span></span> <span data-ttu-id="d1ed0-103">Kövesse az alábbi utasításokat toocreate hello szükséges leküldéses tanúsítvány toosend, és értesítéseket kaphat.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-103">Follow these instructions toocreate hello necessary push certificate toosend and receive notifications.</span></span> <span data-ttu-id="d1ed0-104">További információ ezekről a fogalmakról: hello hivatalos [Apple Push Notification szolgáltatás](http://go.microsoft.com/fwlink/p/?LinkId=272584) dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-104">For more information on these concepts see hello official [Apple Push Notification Service](http://go.microsoft.com/fwlink/p/?LinkId=272584) documentation.</span></span>

<span data-ttu-id="d1ed0-105">Hozza létre hello tanúsítvány-aláírási kérelem (CSR) fájlját, amely egy aláírt leküldéses tanúsítványt az Apple toogenerate használják.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-105">Generate hello Certificate Signing Request (CSR) file, which is used by Apple toogenerate a signed push certificate.</span></span>

1. <span data-ttu-id="d1ed0-106">A Mac gépen futtassa a kulcskarika-elérés eszközt hello.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-106">On your Mac, run hello Keychain Access tool.</span></span> <span data-ttu-id="d1ed0-107">A hello megnyitható **segédprogramok** mappa vagy hello **más** hello launchpadről mappájába.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-107">It can be opened from hello **Utilities** folder or hello **Other** folder on hello launch pad.</span></span>
2. <span data-ttu-id="d1ed0-108">Kattintson a **Kulcskarika-elérés** menüpontra, bontsa ki a **Tanúsítványasszisztens** almenü, majd kattintson a **Tanúsítvány igénylése egy tanúsítványkiadó központból...** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-108">Click **Keychain Access**, expand **Certificate Assistant**, then click **Request a Certificate from a Certificate Authority...**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)
3. <span data-ttu-id="d1ed0-109">Válassza ki a **felhasználó E-mail címét** és **köznapi név** , győződjön meg arról, hogy **toodisk mentett** van kiválasztva, és kattintson **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-109">Select your **User Email Address** and **Common Name** , make sure that **Saved toodisk** is selected, and then click **Continue**.</span></span> <span data-ttu-id="d1ed0-110">Hagyja hello **CA E-mail-cím** mező üres, mert nincs rá szükség.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-110">Leave hello **CA Email Address** field blank as it is not required.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-csr-info.png)
4. <span data-ttu-id="d1ed0-111">Írja be a hello tanúsítvány-aláírási kérelem (CSR) fájl nevét **Mentés másként**, válassza ki azt a hello helyet **ahol**, majd kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-111">Type a name for hello Certificate Signing Request (CSR) file in **Save As**, select hello location in **Where**, then click **Save**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-save-csr.png)
   
      <span data-ttu-id="d1ed0-112">Ez menti hello CSR-fájlt a kijelölt hello location; hello alapértelmezett helye a hello asztali.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-112">This saves hello CSR file in hello selected location; hello default location is in hello Desktop.</span></span> <span data-ttu-id="d1ed0-113">Ne feledje, hogy a fájl választott hello helyet.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-113">Remember hello location chosen for this file.</span></span>

<span data-ttu-id="d1ed0-114">A következő, regisztrálja az alkalmazást az Apple rendszerében, engedélyezi leküldéses értesítéseket, és töltse fel az exportált CSR toocreate leküldéses tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-114">Next, you will register your app with Apple, enable push notifications, and upload this exported CSR toocreate a push certificate.</span></span>

## <a name="register-your-app-for-push-notifications"></a><span data-ttu-id="d1ed0-115">Alkalmazás regisztrálása leküldéses értesítésekhez</span><span class="sxs-lookup"><span data-stu-id="d1ed0-115">Register your app for push notifications</span></span>
<span data-ttu-id="d1ed0-116">toobe képes toosend leküldéses értesítések tooan iOS-alkalmazás, regisztrálnia kell az alkalmazás az Apple, és a leküldéses értesítések regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-116">toobe able toosend push notifications tooan iOS app, you must register your application with Apple and also register for push notifications.</span></span>  

1. <span data-ttu-id="d1ed0-117">Ha már nincs regisztrálva az alkalmazás, keresse meg a toohello <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a> hello Apple Developer Center központban, jelentkezzen be az Apple-Azonosítójával, kattintson **azonosítók**, majd kattintson a **Alkalmazásazonosítók** , végül kattintson a hello  **+**  tooregister egy új alkalmazások aláírásához.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-117">If you have not already registered your app, navigate toohello <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a> at hello Apple Developer Center, log on with your Apple ID, click **Identifiers**, then click **App IDs**, and finally click on hello **+** sign tooregister a new app.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids.png)
      
2. <span data-ttu-id="d1ed0-118">A következő három mezőt az új alkalmazás hello frissítést, majd kattintson **Folytatás**:</span><span class="sxs-lookup"><span data-stu-id="d1ed0-118">Update hello following three fields for your new app and then click **Continue**:</span></span>
   
   * <span data-ttu-id="d1ed0-119">**Név**: Adjon egy leíró nevet az alkalmazás a hello **neve** hello mezőjét **App ID leírás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-119">**Name**: Type a descriptive name for your app in hello **Name** field in hello **App ID Description** section.</span></span>
   * <span data-ttu-id="d1ed0-120">**Azonosító csomagot**: hello alatt **Explicit Alkalmazásazonosító** területen adja meg a **Bundle Identifier** hello formában `<Organization Identifier>.<Product Name>` a hello [alkalmazások terjesztése Az útmutató](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8).</span><span class="sxs-lookup"><span data-stu-id="d1ed0-120">**Bundle Identifier**: Under hello **Explicit App ID** section, enter a **Bundle Identifier** in hello form `<Organization Identifier>.<Product Name>` as mentioned in hello [App Distribution Guide](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8).</span></span> <span data-ttu-id="d1ed0-121">Hello *Organization Identifier* és *Terméknév* kell használnia megfelel-e hello szervezeti azonosító és a termék neve, az XCode projektje létrehozásakor fog használni.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-121">hello *Organization Identifier* and *Product Name* you use must match hello organization identifier and product name you will use when you create your XCode project.</span></span> <span data-ttu-id="d1ed0-122">Az alábbi képernyőképen hello *NotificationHubs* használt szervezetazonosító és *GetStarted* hello terméknév használatos.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-122">In hello screeshot below *NotificationHubs* is used as a organization idenitifier and *GetStarted* is used as hello product name.</span></span> <span data-ttu-id="d1ed0-123">Meggyőződött arról, hogy ez megegyezik, használhatja az XCode projektjében hello értékek lehetővé teszi az Ön toouse hello helyes közzétételi profilt az XCode.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-123">Making sure this matches hello values you will use in your XCode project will allow you toouse hello correct publishing profile with XCode.</span></span> 
   * <span data-ttu-id="d1ed0-124">**Leküldéses értesítések**: ellenőrzés hello **leküldéses értesítések** hello beállítása **alkalmazásszolgáltatások** területen.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-124">**Push Notifications**: Check hello **Push Notifications** option in hello **App Services** section, .</span></span>
     
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-info.png)
     
      <span data-ttu-id="d1ed0-125">Ezzel létrejön az Alkalmazásazonosító, és megkéri Önt tooconfirm hello információkat.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-125">This generates your App ID and requests you tooconfirm hello information.</span></span> <span data-ttu-id="d1ed0-126">Kattintson a **regisztrálása** tooconfirm hello új alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-126">Click **Register** tooconfirm hello new App ID.</span></span>
     
      <span data-ttu-id="d1ed0-127">Gombra kattintva **regisztrálása**, látni fogja a hello **végezze el a regisztrációs** képernyőn, a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-127">Once you click **Register**, you will see hello **Registration complete** screen, as shown below.</span></span> <span data-ttu-id="d1ed0-128">Kattintson a **Done** (Kész) gombra.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-128">Click **Done**.</span></span>
      
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-registration-complete.png)


1. <span data-ttu-id="d1ed0-129">A hello fejlesztői központban az Alkalmazásazonosítók alatt keresse meg az imént létrehozott, és kattintson az azt tartalmazó sorra hello alkalmazás azonosítója.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-129">In hello Developer Center, under App IDs, locate hello app ID that you just created, and click on its row.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids2.png)
   
      <span data-ttu-id="d1ed0-130">Kattintson a hello Alkalmazásazonosító hello alkalmazás adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-130">Clicking on hello app ID will display hello app details.</span></span> <span data-ttu-id="d1ed0-131">Kattintson a hello **szerkesztése** hello alsó gombra.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-131">Click hello **Edit** button at hello bottom.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-edit-appid.png)
      
2. <span data-ttu-id="d1ed0-132">Görgessen toohello hello képernyő aljára, és kattintson a hello **tanúsítvány létrehozása...**  hello szakaszban gomb **Development Push SSL-tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-132">Scroll toohello bottom of hello screen, and click hello **Create Certificate...** button under hello section **Development Push SSL Certificate**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)
   
      <span data-ttu-id="d1ed0-133">Ez megjeleníti a hello "IOS tanúsítvány hozzáadása" segéd.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-133">This displays hello "Add iOS Certificate" assistant.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="d1ed0-134">Ez az oktatóprogram fejlesztési tanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-134">This tutorial uses a development certificate.</span></span> <span data-ttu-id="d1ed0-135">hello ugyanaz a folyamat használatos a termelési tanúsítvány regisztrálásához.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-135">hello same process is used when registering a production certificate.</span></span> <span data-ttu-id="d1ed0-136">Ne feledje, hogy használja-e hello azonos tanúsítványtípus értesítések küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-136">Just make sure that you use hello same certificate type when sending notifications.</span></span>
   > 
   > 
3. <span data-ttu-id="d1ed0-137">Kattintson a **Choose File**, keresse meg a toohello helyet, ahová mentette hello első feladatban létrehozott hello CSR-fájlt, majd kattintson a **Generate**.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-137">Click **Choose File**, browse toohello location where you saved hello CSR file that you created in hello first task, then click **Generate**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)
4. <span data-ttu-id="d1ed0-138">Hello tanúsítvány hello portál által a létrehozása után kattintson a hello **letöltése** gombra, és kattintson a **végzett**.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-138">After hello certificate is created by hello portal, click hello **Download** button, and click **Done**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)
   
      <span data-ttu-id="d1ed0-139">Ez letölti a hello tanúsítványt, és a Letöltések mappába tooyour számítógép menti.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-139">This downloads hello certificate and saves it tooyour computer in your Downloads folder.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)
   
   > [!NOTE]
   > <span data-ttu-id="d1ed0-140">Alapértelmezés szerint hello letöltött fejlesztési tanúsítványfájl neve fájl **aps_development.cer**.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-140">By default, hello downloaded file a development certificate is named **aps_development.cer**.</span></span>
   > 
   > 
5. <span data-ttu-id="d1ed0-141">Kattintson duplán a letöltött leküldéses tanúsítvány hello **aps_development.cer**.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-141">Double-click hello downloaded push certificate **aps_development.cer**.</span></span>
   
      <span data-ttu-id="d1ed0-142">Ez telepíti hello új tanúsítványt hello Kulcsláncba, az alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="d1ed0-142">This installs hello new certificate in hello Keychain, as shown below:</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)
   
   > [!NOTE]
   > <span data-ttu-id="d1ed0-143">hello a tanúsítványban található név lehet különböző, de rendelkezni fog az **Apple Development iOS Push Services:**.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-143">hello name in your certificate might be different, but it will be prefixed with **Apple Development iOS Push Services:**.</span></span>
   > 
   > 
6. <span data-ttu-id="d1ed0-144">A kulcslánc-hozzáférési, kattintson a jobb gombbal hello új leküldéses tanúsítványra hello létrehozott **tanúsítványok** kategóriát.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-144">In Keychain Access, right-click hello new push certificate that you created in hello **Certificates** category.</span></span> <span data-ttu-id="d1ed0-145">Kattintson a **exportálása**, név: hello fájlt, jelölje ki a hello **.p12** formázása, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-145">Click **Export**, name hello file, select hello **.p12** format, and then click **Save**.</span></span>
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)
   
    <span data-ttu-id="d1ed0-146">Győződjön meg a Megjegyzés hello fájlnév és hello exportált .p12 tanúsítvány helyét.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-146">Make a note of hello file name and location of hello exported .p12 certificate.</span></span> <span data-ttu-id="d1ed0-147">Az apns-sel használt tooenable hitelesítési lesz.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-147">It will be used tooenable authentication with APNS.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="d1ed0-148">Ez az oktatóprogram egy QuickStart.p12 fájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-148">This tutorial creates a QuickStart.p12 file.</span></span> <span data-ttu-id="d1ed0-149">Az Ön fájljának neve és helye eltérhet ettől.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-149">Your file name and location might be different.</span></span>
   > 
   > 

## <a name="create-a-provisioning-profile-for-hello-app"></a><span data-ttu-id="d1ed0-150">Hozzon létre egy létesítési profilt hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="d1ed0-150">Create a provisioning profile for hello app</span></span>
1. <span data-ttu-id="d1ed0-151">Vissza a hello <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a>, jelölje be **létesítési profilok**, jelölje be **összes**, és kattintson a hello  **+**  gomb toocreate új profil.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-151">Back in hello <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a>, select **Provisioning Profiles**, select **All**, and then click hello **+** button toocreate a new profile.</span></span> <span data-ttu-id="d1ed0-152">Ekkor elindul a hello **iOS Provisiong profil hozzáadása** varázsló</span><span class="sxs-lookup"><span data-stu-id="d1ed0-152">This launches hello **Add iOS Provisiong Profile** Wizard</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)
2. <span data-ttu-id="d1ed0-153">Válassza ki **iOS App Development** alatt **fejlesztési** elemet hello provisiong profil típusát, és kattintson **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-153">Select **iOS App Development** under **Development** as hello provisiong profile type, and click **Continue**.</span></span> 
3. <span data-ttu-id="d1ed0-154">Ezután válassza ki az imént létrehozott hello hello Alkalmazásazonosító **Alkalmazásazonosító** legördülő listából, és kattintson **Folytatás**</span><span class="sxs-lookup"><span data-stu-id="d1ed0-154">Next, select hello app ID you just created from hello **App ID** drop-down list, and click **Continue**</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)
4. <span data-ttu-id="d1ed0-155">A hello **válassza ki a tanúsítványok** képernyőn, válassza ki a kódaláíráshoz használt szokásos fejlesztési tanúsítványt, és kattintson **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-155">In hello **Select certificates** screen, select your usual development certificate used for code signing, and click **Continue**.</span></span> <span data-ttu-id="d1ed0-156">Ez az imént létrehozott hello leküldéses tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-156">This is not hello push certificate you just created.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)
5. <span data-ttu-id="d1ed0-157">Ezután válassza ki a hello **eszközök** toouse tesztelése, és kattintson a **Folytatás**</span><span class="sxs-lookup"><span data-stu-id="d1ed0-157">Next, select hello **Devices** toouse for testing, and click **Continue**</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)
6. <span data-ttu-id="d1ed0-158">Végül adjon meg nevet a hello-profil **profilnév**, kattintson a **Generate**.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-158">Finally, pick a name for hello profile in **Profile Name**, click **Generate**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)
7. <span data-ttu-id="d1ed0-159">Új létesítési profil hello létrehozásakor kattintson toodownload, és telepítse az xcode-ban fejlesztési számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-159">When hello new provisioning profile is created click toodownload it and install it on your Xcode development machine.</span></span> <span data-ttu-id="d1ed0-160">Ezután kattintson a **Done** (Kész) gombra.</span><span class="sxs-lookup"><span data-stu-id="d1ed0-160">Then click **Done**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-profile-ready.png)
