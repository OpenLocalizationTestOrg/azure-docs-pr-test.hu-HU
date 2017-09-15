

## <a name="generate-the-certificate-signing-request-file"></a><span data-ttu-id="67b46-101">A tanúsítvány-aláírási kérelem fájljának létrehozása</span><span class="sxs-lookup"><span data-stu-id="67b46-101">Generate the Certificate Signing Request file</span></span>
<span data-ttu-id="67b46-102">Az Apple Push Notification szolgáltatás (APNS) tanúsítványokat használ a leküldéses értesítések hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="67b46-102">The Apple Push Notification Service (APNS) uses certificates to authenticate your push notifications.</span></span> <span data-ttu-id="67b46-103">Kövesse ezeket az utasításokat az értesítések küldéséhez és fogadásához szükséges leküldéses tanúsítvány létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="67b46-103">Follow these instructions to create the necessary push certificate to send and receive notifications.</span></span> <span data-ttu-id="67b46-104">További információért lásd az [Apple Push Notification Service](http://go.microsoft.com/fwlink/p/?LinkId=272584) (Apple Push Notification szolgáltatás) hivatalos dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="67b46-104">For more information on these concepts see the official [Apple Push Notification Service](http://go.microsoft.com/fwlink/p/?LinkId=272584) documentation.</span></span>

<span data-ttu-id="67b46-105">Hozza létre a tanúsítvány-aláírási kérelem (CSR) fájlját, mellyel az Apple létrehoz egy aláírt leküldéses tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="67b46-105">Generate the Certificate Signing Request (CSR) file, which is used by Apple to generate a signed push certificate.</span></span>

1. <span data-ttu-id="67b46-106">Futtassa a Kulcskarika-elérés eszközt Mac számítógépén.</span><span class="sxs-lookup"><span data-stu-id="67b46-106">On your Mac, run the Keychain Access tool.</span></span> <span data-ttu-id="67b46-107">Ez a **Segédprogramok** vagy az **Egyéb** mappából nyitható meg a Launchpadről.</span><span class="sxs-lookup"><span data-stu-id="67b46-107">It can be opened from the **Utilities** folder or the **Other** folder on the launch pad.</span></span>
2. <span data-ttu-id="67b46-108">Kattintson a **Kulcskarika-elérés** menüpontra, bontsa ki a **Tanúsítványasszisztens** almenü, majd kattintson a **Tanúsítvány igénylése egy tanúsítványkiadó központból...** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="67b46-108">Click **Keychain Access**, expand **Certificate Assistant**, then click **Request a Certificate from a Certificate Authority...**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)
3. <span data-ttu-id="67b46-109">Töltse ki a **Felhasználói e-mail-cím** és az **Általános név** mezőket, ügyeljen arra, hogy a **Mentve lemezre** lehetőség legyen kiválasztva, majd kattintson a **Folytatás** gombra.</span><span class="sxs-lookup"><span data-stu-id="67b46-109">Select your **User Email Address** and **Common Name** , make sure that **Saved to disk** is selected, and then click **Continue**.</span></span> <span data-ttu-id="67b46-110">Hagyja üresen a **CA e-mail-cím** mezőt, ennek kitöltése nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="67b46-110">Leave the **CA Email Address** field blank as it is not required.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-csr-info.png)
4. <span data-ttu-id="67b46-111">Adjon meg egy nevet a tanúsítvány-aláírási kérelem (CSR) fájljához a **Mentés mint** mezőben, válassza ki a helyet a **Hely** mezőben, majd kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="67b46-111">Type a name for the Certificate Signing Request (CSR) file in **Save As**, select the location in **Where**, then click **Save**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-save-csr.png)
   
      <span data-ttu-id="67b46-112">Ez menti a CSR-fájlt a megadott helyre; az alapértelmezett hely az Íróasztal.</span><span class="sxs-lookup"><span data-stu-id="67b46-112">This saves the CSR file in the selected location; the default location is in the Desktop.</span></span> <span data-ttu-id="67b46-113">Jegyezze meg a fájlhoz választott helyet.</span><span class="sxs-lookup"><span data-stu-id="67b46-113">Remember the location chosen for this file.</span></span>

<span data-ttu-id="67b46-114">Ezután regisztrálni fogja az alkalmazását az Apple rendszerében, engedélyezi a leküldéses értesítéseket, és feltölti ezt az exportált CSR-t a leküldéses tanúsítvány létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="67b46-114">Next, you will register your app with Apple, enable push notifications, and upload this exported CSR to create a push certificate.</span></span>

## <a name="register-your-app-for-push-notifications"></a><span data-ttu-id="67b46-115">Alkalmazás regisztrálása leküldéses értesítésekhez</span><span class="sxs-lookup"><span data-stu-id="67b46-115">Register your app for push notifications</span></span>
<span data-ttu-id="67b46-116">Ahhoz, hogy leküldéses értesítéseket küldhessen az iOS-alkalmazásoknak, először regisztrálnia kell az alkalmazását az Apple rendszerében, és regisztrálnia kell a leküldéses értesítésekhez is.</span><span class="sxs-lookup"><span data-stu-id="67b46-116">To be able to send push notifications to an iOS app, you must register your application with Apple and also register for push notifications.</span></span>  

1. <span data-ttu-id="67b46-117">Ha már regisztrálta az alkalmazását, lépjen az <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a> (iOS üzembe helyezési portál) felületére az Apple Developer Center központban, jelentkezzen be Apple-azonosítójával, kattintson az **Identifiers** (Azonosítók), majd az **App IDs** (Alkalmazásazonosítók) lehetőségre, végül kattintson a **+** jelre az új alkalmazás regisztrálásához.</span><span class="sxs-lookup"><span data-stu-id="67b46-117">If you have not already registered your app, navigate to the <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a> at the Apple Developer Center, log on with your Apple ID, click **Identifiers**, then click **App IDs**, and finally click on the **+** sign to register a new app.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids.png)
      
2. <span data-ttu-id="67b46-118">Frissítse a következő három mezőt az új alkalmazásában, és kattintson a **Continue** (Folytatás) gombra:</span><span class="sxs-lookup"><span data-stu-id="67b46-118">Update the following three fields for your new app and then click **Continue**:</span></span>
   
   * <span data-ttu-id="67b46-119">**Name** (Név): Adjon meg egy leíró nevet az alkalmazáshoz a **Name** (Név) mezőben az **App ID Description** (Alkalmazásazonosító leírása) szakaszban.</span><span class="sxs-lookup"><span data-stu-id="67b46-119">**Name**: Type a descriptive name for your app in the **Name** field in the **App ID Description** section.</span></span>
   * <span data-ttu-id="67b46-120">**Bundle Identifier** (Csomagazonosító): Az **Explicit App ID** (Explicit alkalmazásazonosító) szakasz alatt adjon meg egy csomagazonosítót (**Bundle Identifier**) abban a formában, `<Organization Identifier>.<Product Name>` ahogy azt az [App Distribution Guide](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8) (Alkalmazásterjesztési útmutató) írja.</span><span class="sxs-lookup"><span data-stu-id="67b46-120">**Bundle Identifier**: Under the **Explicit App ID** section, enter a **Bundle Identifier** in the form `<Organization Identifier>.<Product Name>` as mentioned in the [App Distribution Guide](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8).</span></span> <span data-ttu-id="67b46-121">A használt szervezetazonosítónak (*Organization Identifier*) és terméknévnek (*Product Name*) meg kell egyeznie azzal a szervezetazonosítóval és terméknévvel, melyeket az Xcode projektje létrehozásakor fog használni.</span><span class="sxs-lookup"><span data-stu-id="67b46-121">The *Organization Identifier* and *Product Name* you use must match the organization identifier and product name you will use when you create your XCode project.</span></span> <span data-ttu-id="67b46-122">Az alábbi képernyőképen a *NotificationHubs* a használt szervezetazonosító, a terméknév pedig a *GetStarted*.</span><span class="sxs-lookup"><span data-stu-id="67b46-122">In the screeshot below *NotificationHubs* is used as a organization idenitifier and *GetStarted* is used as the product name.</span></span> <span data-ttu-id="67b46-123">Ha ügyel arra, hogy ezek az értékek megegyezzenek azokkal, amelyeket az XCode projektjében fog használni, a helyes közzétételi profilt fogja tudni használni az XCode környezetben.</span><span class="sxs-lookup"><span data-stu-id="67b46-123">Making sure this matches the values you will use in your XCode project will allow you to use the correct publishing profile with XCode.</span></span> 
   * <span data-ttu-id="67b46-124">**Push Notifications** (Leküldéses értesítések): Jelölje be a **Push Notifications** (Leküldéses értesítések) beállítást az **App Services** (Alkalmazásszolgáltatások) szakaszban.</span><span class="sxs-lookup"><span data-stu-id="67b46-124">**Push Notifications**: Check the **Push Notifications** option in the **App Services** section, .</span></span>
     
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-info.png)
     
      <span data-ttu-id="67b46-125">Ekkor létrejön az alkalmazásazonosító, és a rendszer megkéri Önt az információk megerősítésére.</span><span class="sxs-lookup"><span data-stu-id="67b46-125">This generates your App ID and requests you to confirm the information.</span></span> <span data-ttu-id="67b46-126">Kattintson a **Register** (Regisztráció) gombra az új alkalmazásazonosító megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="67b46-126">Click **Register** to confirm the new App ID.</span></span>
     
      <span data-ttu-id="67b46-127">Miután a **Register** (Regisztráció) gombra kattintott, megjelenik az alábbiakban is látható **Registration complete** (Regisztráció kész) képernyő.</span><span class="sxs-lookup"><span data-stu-id="67b46-127">Once you click **Register**, you will see the **Registration complete** screen, as shown below.</span></span> <span data-ttu-id="67b46-128">Kattintson a **Done** (Kész) gombra.</span><span class="sxs-lookup"><span data-stu-id="67b46-128">Click **Done**.</span></span>
      
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-registration-complete.png)


1. <span data-ttu-id="67b46-129">Keresse meg az előbb létrehozott alkalmazásazonosítót a fejlesztői központban az alkalmazásazonosítók alatt, és kattintson az azt tartalmazó sorra.</span><span class="sxs-lookup"><span data-stu-id="67b46-129">In the Developer Center, under App IDs, locate the app ID that you just created, and click on its row.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids2.png)
   
      <span data-ttu-id="67b46-130">Ha az alkalmazásazonosítóra kattint, megjelennek az alkalmazás részletei.</span><span class="sxs-lookup"><span data-stu-id="67b46-130">Clicking on the app ID will display the app details.</span></span> <span data-ttu-id="67b46-131">Kattintson az alul található **Edit** (Szerkesztés) gombra.</span><span class="sxs-lookup"><span data-stu-id="67b46-131">Click the **Edit** button at the bottom.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-edit-appid.png)
      
2. <span data-ttu-id="67b46-132">Görgessen a képernyő aljára, és kattintson a **Create Certificate...** (Tanúsítvány létrehozása...) gombra a **Development Push SSL Certificate** (Leküldéses fejlesztési SSL-tanúsítvány) szakasz alatt.</span><span class="sxs-lookup"><span data-stu-id="67b46-132">Scroll to the bottom of the screen, and click the **Create Certificate...** button under the section **Development Push SSL Certificate**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)
   
      <span data-ttu-id="67b46-133">Ekkor megjelenik az „Add iOS Certificate” (iOS tanúsítvány hozzáadása) segédprogram.</span><span class="sxs-lookup"><span data-stu-id="67b46-133">This displays the "Add iOS Certificate" assistant.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="67b46-134">Ez az oktatóprogram fejlesztési tanúsítványt használ.</span><span class="sxs-lookup"><span data-stu-id="67b46-134">This tutorial uses a development certificate.</span></span> <span data-ttu-id="67b46-135">Ugyanez a folyamat használatos a termelési tanúsítvány regisztrálásához is.</span><span class="sxs-lookup"><span data-stu-id="67b46-135">The same process is used when registering a production certificate.</span></span> <span data-ttu-id="67b46-136">Csak arra ügyeljen, hogy ugyanazt a tanúsítványtípust használja az értesítések küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="67b46-136">Just make sure that you use the same certificate type when sending notifications.</span></span>
   > 
   > 
3. <span data-ttu-id="67b46-137">Kattintson a **Choose File** (Fájl kiválasztása) lehetőségre, keresse meg a helyet, ahová mentette az első feladatban létrehozott CSR-fájlt, majd kattintson a **Generate** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="67b46-137">Click **Choose File**, browse to the location where you saved the CSR file that you created in the first task, then click **Generate**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)
4. <span data-ttu-id="67b46-138">Miután a portál létrehozza a tanúsítványt, kattintson a **Download** (Letöltés) gombra, majd a **Done** (Kész) gombra.</span><span class="sxs-lookup"><span data-stu-id="67b46-138">After the certificate is created by the portal, click the **Download** button, and click **Done**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)
   
      <span data-ttu-id="67b46-139">Ez letölti a tanúsítványt, és menti azt a számítógépére, a Letöltések mappába.</span><span class="sxs-lookup"><span data-stu-id="67b46-139">This downloads the certificate and saves it to your computer in your Downloads folder.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)
   
   > [!NOTE]
   > <span data-ttu-id="67b46-140">Alapértelmezés szerint a letöltött fejlesztési tanúsítványfájl neve **aps_development.cer**.</span><span class="sxs-lookup"><span data-stu-id="67b46-140">By default, the downloaded file a development certificate is named **aps_development.cer**.</span></span>
   > 
   > 
5. <span data-ttu-id="67b46-141">Kattintson duplán a letöltött **aps_development.cer** leküldéses tanúsítványra.</span><span class="sxs-lookup"><span data-stu-id="67b46-141">Double-click the downloaded push certificate **aps_development.cer**.</span></span>
   
      <span data-ttu-id="67b46-142">Ez telepíti az új tanúsítványt a kulcsláncba, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="67b46-142">This installs the new certificate in the Keychain, as shown below:</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)
   
   > [!NOTE]
   > <span data-ttu-id="67b46-143">A tanúsítványban található név lehet különböző, de rendelkezni fog az **Apple Development iOS Push Services:** (Apple fejlesztési iOS leküldéses szolgáltatások:) előtagjával.</span><span class="sxs-lookup"><span data-stu-id="67b46-143">The name in your certificate might be different, but it will be prefixed with **Apple Development iOS Push Services:**.</span></span>
   > 
   > 
6. <span data-ttu-id="67b46-144">A kulcslánc-hozzáférési oldalon kattintson a jobb egérgombbal az új leküldéses tanúsítványra, melyet a **Certificates** (Tanúsítványok) kategóriában létrehozott.</span><span class="sxs-lookup"><span data-stu-id="67b46-144">In Keychain Access, right-click the new push certificate that you created in the **Certificates** category.</span></span> <span data-ttu-id="67b46-145">Kattintson az **Export** (Exportálás) elemre, válassza a **.p12** formátumot, majd kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="67b46-145">Click **Export**, name the file, select the **.p12** format, and then click **Save**.</span></span>
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)
   
    <span data-ttu-id="67b46-146">Jegyezze fel az exportált .p12 tanúsítvány nevét és helyét.</span><span class="sxs-lookup"><span data-stu-id="67b46-146">Make a note of the file name and location of the exported .p12 certificate.</span></span> <span data-ttu-id="67b46-147">Ezek szükségesek lesznek az APNS-sel való hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="67b46-147">It will be used to enable authentication with APNS.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="67b46-148">Ez az oktatóprogram egy QuickStart.p12 fájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="67b46-148">This tutorial creates a QuickStart.p12 file.</span></span> <span data-ttu-id="67b46-149">Az Ön fájljának neve és helye eltérhet ettől.</span><span class="sxs-lookup"><span data-stu-id="67b46-149">Your file name and location might be different.</span></span>
   > 
   > 

## <a name="create-a-provisioning-profile-for-the-app"></a><span data-ttu-id="67b46-150">Üzembe helyezési profil létrehozása az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="67b46-150">Create a provisioning profile for the app</span></span>
1. <span data-ttu-id="67b46-151">Térjen vissza az <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a> felületére, válassza a **Provisioning Profiles** (Üzembe helyezési profilok), majd az **All** (Összes) lehetőséget, és kattintson a **+** gombra az új profil létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="67b46-151">Back in the <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a>, select **Provisioning Profiles**, select **All**, and then click the **+** button to create a new profile.</span></span> <span data-ttu-id="67b46-152">Ekkor elindul az **Add iOS Provisiong Profile** (iOS üzembe helyezési profil hozzáadása) varázsló</span><span class="sxs-lookup"><span data-stu-id="67b46-152">This launches the **Add iOS Provisiong Profile** Wizard</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)
2. <span data-ttu-id="67b46-153">Az üzembe helyezési profil típusaként válassza az **iOS App Development** (iOS-alkalmazásfejlesztés) lehetőséget a **Development** (Fejlesztés) alatt, majd kattintson a **Continue** (Folytatás) gombra.</span><span class="sxs-lookup"><span data-stu-id="67b46-153">Select **iOS App Development** under **Development** as the provisiong profile type, and click **Continue**.</span></span> 
3. <span data-ttu-id="67b46-154">Ezután válassza ki az imént létrehozott azonosítót az **App ID** (Alkalmazásazonosító) legördülő listából, és kattintson a **Continue** (Folytatás) gombra</span><span class="sxs-lookup"><span data-stu-id="67b46-154">Next, select the app ID you just created from the **App ID** drop-down list, and click **Continue**</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)
4. <span data-ttu-id="67b46-155">A **Select certificates** (Tanúsítványok kiválasztása) képernyőn válassza ki a kódaláíráshoz használt szokásos fejlesztési tanúsítványt, majd kattintson a **Continue** (Folytatás) gombra.</span><span class="sxs-lookup"><span data-stu-id="67b46-155">In the **Select certificates** screen, select your usual development certificate used for code signing, and click **Continue**.</span></span> <span data-ttu-id="67b46-156">Ez a nem az Ön által imént létrehozott leküldéses tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="67b46-156">This is not the push certificate you just created.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)
5. <span data-ttu-id="67b46-157">Ezután válassza ki a teszteléshez használt eszközöket a **Devices** (Eszközök) mezőben, és kattintson a **Continue** (Folytatás) gombra</span><span class="sxs-lookup"><span data-stu-id="67b46-157">Next, select the **Devices** to use for testing, and click **Continue**</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)
6. <span data-ttu-id="67b46-158">Végül adjon meg egy nevet a profilnak a **Profile Name** (Profilnév) mezőben, és kattintson a **Generate** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="67b46-158">Finally, pick a name for the profile in **Profile Name**, click **Generate**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)
7. <span data-ttu-id="67b46-159">Az új üzembe helyezési profil létrejötte után kattintson rá a letöltéshez, és telepítse az Xcode-fejlesztői gépére.</span><span class="sxs-lookup"><span data-stu-id="67b46-159">When the new provisioning profile is created click to download it and install it on your Xcode development machine.</span></span> <span data-ttu-id="67b46-160">Ezután kattintson a **Done** (Kész) gombra.</span><span class="sxs-lookup"><span data-stu-id="67b46-160">Then click **Done**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-profile-ready.png)
