<!--author=SharS last changed: 02/22/2016-->

### <a name="tooconfigure-and-register-hello-device"></a><span data-ttu-id="6aabb-101">hello eszköz tooconfigure és regisztrálása</span><span class="sxs-lookup"><span data-stu-id="6aabb-101">tooconfigure and register hello device</span></span>
1. <span data-ttu-id="6aabb-102">Hozzáférés hello Windows PowerShell felületet a StorSimple eszköz soros konzoljához a.</span><span class="sxs-lookup"><span data-stu-id="6aabb-102">Access hello Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="6aabb-103">Lásd: [PuTTY használata tooconnect toohello eszköz soros konzoljához](../articles/storsimple/storsimple-deployment-walkthrough-gov-u2.md#use-putty-to-connect-to-the-device-serial-console) utasításokat.</span><span class="sxs-lookup"><span data-stu-id="6aabb-103">See [Use PuTTY tooconnect toohello device serial console](../articles/storsimple/storsimple-deployment-walkthrough-gov-u2.md#use-putty-to-connect-to-the-device-serial-console) for instructions.</span></span> <span data-ttu-id="6aabb-104">**Lehet, hogy toofollow hello eljárás pontosan vagy nem fog tudni tooaccess hello konzol.**</span><span class="sxs-lookup"><span data-stu-id="6aabb-104">**Be sure toofollow hello procedure exactly or you will not be able tooaccess hello console.**</span></span>
2. <span data-ttu-id="6aabb-105">A megnyitott munkamenetben hello nyomja le az adjon meg egy idő tooget egy parancssori ablakot.</span><span class="sxs-lookup"><span data-stu-id="6aabb-105">In hello session that opens up, press Enter one time tooget a command prompt.</span></span>
3. <span data-ttu-id="6aabb-106">Rákérdezéses toochoose hello nyelvi, hogy milyen tooset az eszközhöz lesz.</span><span class="sxs-lookup"><span data-stu-id="6aabb-106">You will be prompted toochoose hello language that you would like tooset for your device.</span></span> <span data-ttu-id="6aabb-107">Adja meg a hello nyelv, és nyomja le az ENTER billentyűt.</span><span class="sxs-lookup"><span data-stu-id="6aabb-107">Specify hello language, and then press Enter.</span></span>
   
    ![StorSimple-eszköz konfigurálása és regisztrálása, 1](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice1-gov-include.png)
4. <span data-ttu-id="6aabb-109">Hello soros konzol menüben megjelenő válassza a 1. lehetőség toolog a teljes hozzáféréssel.</span><span class="sxs-lookup"><span data-stu-id="6aabb-109">In hello serial console menu that is presented, choose option 1 toolog on with full access.</span></span>
   
    ![StorSimple-eszköz regisztrálása, 2](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice2-gov-include.png)
5. <span data-ttu-id="6aabb-111">Hajtsa végre a következő lépéseket tooconfigure hello minimálisan szükséges hálózati beállításokat az eszközhöz hello.</span><span class="sxs-lookup"><span data-stu-id="6aabb-111">Perform hello following steps tooconfigure hello minimum required network settings for your device.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="6aabb-112">Ezeket a konfigurációs lépéseket kell toobe hello hello eszköz aktív vezérlőjén végre.</span><span class="sxs-lookup"><span data-stu-id="6aabb-112">These configuration steps need toobe performed on hello active controller of hello device.</span></span> <span data-ttu-id="6aabb-113">hello soros konzol menü hello vezérlő állapotát a szalagcím üdvözlőüzenetére jelzi.</span><span class="sxs-lookup"><span data-stu-id="6aabb-113">hello serial console menu indicates hello controller state in hello banner message.</span></span> <span data-ttu-id="6aabb-114">Ha vannak nem toohello aktív vezérlő, válassza le, és csatlakoztassa a toohello aktív vezérlő.</span><span class="sxs-lookup"><span data-stu-id="6aabb-114">If you are not connect toohello active controller, disconnect and then connect toohello active controller.</span></span>
   > 
   > 
   
   1. <span data-ttu-id="6aabb-115">Hello parancssorba írja be a jelszavát.</span><span class="sxs-lookup"><span data-stu-id="6aabb-115">At hello command prompt, type your password.</span></span> <span data-ttu-id="6aabb-116">hello eszköz alapértelmezett jelszava: **jelszó1**.</span><span class="sxs-lookup"><span data-stu-id="6aabb-116">hello default device password is **Password1**.</span></span>
   2. <span data-ttu-id="6aabb-117">Írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="6aabb-117">Type hello following command:</span></span>
      
        `Invoke-HcsSetupWizard`
   3. <span data-ttu-id="6aabb-118">A telepítő varázsló jelenik meg toohelp hello hello eszköz hálózati beállításainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6aabb-118">A setup wizard will appear toohelp you configure hello network settings for hello device.</span></span> <span data-ttu-id="6aabb-119">Adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="6aabb-119">Supply hello following information:</span></span>
      
      * <span data-ttu-id="6aabb-120">DATA 0 hálózati adapterén IP-címe</span><span class="sxs-lookup"><span data-stu-id="6aabb-120">IP address for DATA 0 network interface</span></span>
      * <span data-ttu-id="6aabb-121">Alhálózati maszk</span><span class="sxs-lookup"><span data-stu-id="6aabb-121">Subnet mask</span></span>
      * <span data-ttu-id="6aabb-122">Átjáró</span><span class="sxs-lookup"><span data-stu-id="6aabb-122">Gateway</span></span>
      * <span data-ttu-id="6aabb-123">Az elsődleges DNS-kiszolgáló IP-címe</span><span class="sxs-lookup"><span data-stu-id="6aabb-123">IP address for Primary DNS server</span></span>
      * <span data-ttu-id="6aabb-124">Az elsődleges NTP-kiszolgáló IP-címe</span><span class="sxs-lookup"><span data-stu-id="6aabb-124">IP address for Primary NTP server</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="6aabb-125">Előfordulhat, hogy toowait hello alhálózati maszk és a DNS-beállítások toobe alkalmazása néhány percig.</span><span class="sxs-lookup"><span data-stu-id="6aabb-125">You may have toowait for a few minutes for hello subnet mask and DNS settings toobe applied.</span></span>
      > 
      > 
   4. <span data-ttu-id="6aabb-126">Igény szerint állítsa be a proxy-webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="6aabb-126">Optionally, configure your web proxy server.</span></span>
      
      > [!IMPORTANT]
      > <span data-ttu-id="6aabb-127">Bár a webproxy konfigurálása nem kötelező, vegye figyelembe, hogy ha olyan webproxyt használ, csak konfigurálhatja azt itt.</span><span class="sxs-lookup"><span data-stu-id="6aabb-127">Although web proxy configuration is optional, be aware that if you use a web proxy, you can only configure it here.</span></span> <span data-ttu-id="6aabb-128">További információ: túl[az eszköz webalkalmazás-proxy konfigurálása](../articles/storsimple/storsimple-configure-web-proxy.md).</span><span class="sxs-lookup"><span data-stu-id="6aabb-128">For more information, go too[Configure web proxy for your device](../articles/storsimple/storsimple-configure-web-proxy.md).</span></span>
      > 
      > 
6. <span data-ttu-id="6aabb-129">Ctrl + C billentyűkombinációval tooexit hello beállítása varázsló.</span><span class="sxs-lookup"><span data-stu-id="6aabb-129">Press Ctrl + C tooexit hello setup wizard.</span></span>
7. <span data-ttu-id="6aabb-130">Hello frissítések telepítése az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="6aabb-130">Install hello updates as follows:</span></span>
   
   1. <span data-ttu-id="6aabb-131">Használja a következő parancsmag tooset IP-címeket mindkét hello tartományvezérlőn hello:</span><span class="sxs-lookup"><span data-stu-id="6aabb-131">Use hello following cmdlet tooset IPs on both hello controllers:</span></span>
      
      `Set-HcsNetInterface -InterfaceAlias Data0 -Controller0IPv4Address <Controller0 IP> -Controller1IPv4Address <Controller1 IP>`
   2. <span data-ttu-id="6aabb-132">Hello parancssorban futtassa `Get-HcsUpdateAvailability`.</span><span class="sxs-lookup"><span data-stu-id="6aabb-132">At hello command prompt, run `Get-HcsUpdateAvailability`.</span></span> <span data-ttu-id="6aabb-133">Meg kell értesítés frissítések érhetők el.</span><span class="sxs-lookup"><span data-stu-id="6aabb-133">You should be notified that updates are available.</span></span>
   3. <span data-ttu-id="6aabb-134">Futtassa az `Start-HcsUpdate` parancsot.</span><span class="sxs-lookup"><span data-stu-id="6aabb-134">Run `Start-HcsUpdate`.</span></span> <span data-ttu-id="6aabb-135">Ez a parancs futtatható bármely csomópontján.</span><span class="sxs-lookup"><span data-stu-id="6aabb-135">You can run this command on any node.</span></span> <span data-ttu-id="6aabb-136">Hello első tartományvezérlő alkalmazandó frissítések hello vezérlő hajt végre feladatátvételt és majd a alkalmazandó frissítések hello hello más vezérlő.</span><span class="sxs-lookup"><span data-stu-id="6aabb-136">Updates will be applied on hello first controller, hello controller will fail over, and then hello updates will be applied on hello other controller.</span></span>
      
      <span data-ttu-id="6aabb-137">Kísérheti hello hello frissítés futtatásával `Get-HcsUpdateStatus`.</span><span class="sxs-lookup"><span data-stu-id="6aabb-137">You can monitor hello progress of hello update by running `Get-HcsUpdateStatus`.</span></span>    
      
      <span data-ttu-id="6aabb-138">hello következő minta kimenet látható hello frissítés folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="6aabb-138">hello following sample output shows hello update in progress.</span></span>
      
      ````
      Controller0>Get-HcsUpdateStatus
      RunInprogress       : True
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   :
      ````
      
      <span data-ttu-id="6aabb-139">a következő minta kimenet hello azt jelzi, hogy adott hello frissítése befejeződött.</span><span class="sxs-lookup"><span data-stu-id="6aabb-139">hello following sample output indicates that hello update is finished.</span></span>
      
      ```
      Controller1>Get-HcsUpdateStatus
      
      RunInprogress       : False
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   :
      ```
      
      <span data-ttu-id="6aabb-140">Tooapply összes hello frissítéseket, beleértve a Windows-frissítések hello too11 órát igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="6aabb-140">It may take up too11 hours tooapply all hello updates, including hello Windows Updates.</span></span>
8. <span data-ttu-id="6aabb-141">Futtassa a következő parancsmag toopoint hello eszköz toohello a Microsoft Azure Government portálon (mert alapértelmezés szerint mutat toohello nyilvános a klasszikus Azure portálon) hello.</span><span class="sxs-lookup"><span data-stu-id="6aabb-141">Run hello following cmdlet toopoint hello device toohello Microsoft Azure Government portal (because it points toohello public Azure classic portal by default).</span></span> <span data-ttu-id="6aabb-142">Ez mindkét tartományvezérlők újraindul.</span><span class="sxs-lookup"><span data-stu-id="6aabb-142">This will restart both controllers.</span></span> <span data-ttu-id="6aabb-143">Azt javasoljuk, hogy használja-e két PuTTY munkamenet toosimultaneously csatlakozás tooboth vezérlőket, hogy a tartományvezérlők újraindításakor láthatóvá.</span><span class="sxs-lookup"><span data-stu-id="6aabb-143">We recommend that you use two PuTTY sessions toosimultaneously connect tooboth controllers so that you can see when each controller is restarted.</span></span>
   
    `Set-CloudPlatform -AzureGovt_US`
   
   <span data-ttu-id="6aabb-144">Egy megerősítő üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6aabb-144">You will see a confirmation message.</span></span> <span data-ttu-id="6aabb-145">Fogadja el az alapértelmezett hello (**Y**).</span><span class="sxs-lookup"><span data-stu-id="6aabb-145">Accept hello default (**Y**).</span></span>
9. <span data-ttu-id="6aabb-146">Futtassa a következő parancsmag tooresume telepítő hello:</span><span class="sxs-lookup"><span data-stu-id="6aabb-146">Run hello following cmdlet tooresume setup:</span></span>
   
    `Invoke-HcsSetupWizard`
   
    ![A telepítővarázsló folytatása](./media/storsimple-configure-and-register-device-gov-u2/HCS_ResumeSetup-gov-include.png)
   
   <span data-ttu-id="6aabb-148">Amikor a telepítő folytatja, akkor hello varázsló hello Update 2 verziója lesz.</span><span class="sxs-lookup"><span data-stu-id="6aabb-148">When you resume setup, hello wizard will be hello Update 2 version.</span></span>
10. <span data-ttu-id="6aabb-149">Fogadja el a hello hálózati beállításokat.</span><span class="sxs-lookup"><span data-stu-id="6aabb-149">Accept hello network settings.</span></span> <span data-ttu-id="6aabb-150">Egy érvényesítési üzenet jelenik meg, ha elfogadja, hogy egyes beállítások.</span><span class="sxs-lookup"><span data-stu-id="6aabb-150">You will see a validation message after you accept each setting.</span></span>
11. <span data-ttu-id="6aabb-151">Biztonsági okokból hello eszköz rendszergazdai jelszava hello első munkamenet végeztével lejár, és szüksége lesz a toochange informatikai most.</span><span class="sxs-lookup"><span data-stu-id="6aabb-151">For security reasons, hello device administrator password expires after hello first session, and you will need toochange it now.</span></span> <span data-ttu-id="6aabb-152">Amikor a rendszer erre kéri, adjon meg az eszközhöz egy rendszergazdai jelszót.</span><span class="sxs-lookup"><span data-stu-id="6aabb-152">When prompted, provide a device administrator password.</span></span> <span data-ttu-id="6aabb-153">Az eszköz rendszergazdai jelszavának 8–15 karakter hosszúságúnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6aabb-153">A valid device administrator password must be between 8 and 15 characters.</span></span> <span data-ttu-id="6aabb-154">hello jelszó közül hármat tartalmaznia kell következő hello:, kisbetű, nagybetű, numerikus és speciális karaktereket.</span><span class="sxs-lookup"><span data-stu-id="6aabb-154">hello password must contain three of hello following: lowercase, uppercase, numeric, and special characters.</span></span>
    
    <br/>![StorSimple-eszköz regisztrálása, 5](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice5_gov-include.png)
12. <span data-ttu-id="6aabb-156">hello hello telepítővarázsló utolsó lépésként regisztrálja az eszközt a StorSimple Manager szolgáltatás hello.</span><span class="sxs-lookup"><span data-stu-id="6aabb-156">hello final step in hello setup wizard registers your device with hello StorSimple Manager service.</span></span> <span data-ttu-id="6aabb-157">Ezt akkor lesz kell hello beolvasott Szolgáltatásregisztrációs kulcs [2. lépés: hello Szolgáltatásregisztrációs kulcs lekérése](../articles/storsimple/storsimple-deployment-walkthrough-gov-u2.md#step-2-get-the-service-registration-key).</span><span class="sxs-lookup"><span data-stu-id="6aabb-157">For this, you will need hello service registration key that you obtained in [Step 2: Get hello service registration key](../articles/storsimple/storsimple-deployment-walkthrough-gov-u2.md#step-2-get-the-service-registration-key).</span></span> <span data-ttu-id="6aabb-158">Hello regisztrációs kulcsot fogja tartalmazni, miután szükséges toowait 2-3 percet hello eszköz regisztrálva van-e.</span><span class="sxs-lookup"><span data-stu-id="6aabb-158">After you supply hello registration key, you may need toowait for 2-3 minutes before hello device is registered.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="6aabb-159">Minden alkalommal tooexit hello beállítása varázsló, is használja a Ctrl + C.</span><span class="sxs-lookup"><span data-stu-id="6aabb-159">You can press Ctrl + C at any time tooexit hello setup wizard.</span></span> <span data-ttu-id="6aabb-160">Ha a megadott összes hello hálózati beállításokat (a Data 0, alhálózati maszk és átjáró IP-cím), a bejegyzéseket megőrzi a rendszer.</span><span class="sxs-lookup"><span data-stu-id="6aabb-160">If you have entered all hello network settings (IP address for Data 0, Subnet mask, and Gateway), your entries will be retained.</span></span>
    > 
    > 
    
    ![A StorSimple regisztrációs folyamat](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegistrationProgress-gov-include.png)
13. <span data-ttu-id="6aabb-162">Hello eszköz regisztrálása után megjelenik egy szolgáltatásadat-titkosítási kulcs.</span><span class="sxs-lookup"><span data-stu-id="6aabb-162">After hello device is registered, a Service Data Encryption key will appear.</span></span> <span data-ttu-id="6aabb-163">Másolja ki ezt a kulcsot, és mentse egy biztonságos helyre.</span><span class="sxs-lookup"><span data-stu-id="6aabb-163">Copy this key and save it in a safe location.</span></span> <span data-ttu-id="6aabb-164">**Ezt a kulcsot kötelező megadni a hello szolgáltatás regisztrációs kulcs tooregister további eszközöket a StorSimple Manager szolgáltatás hello lesz.**</span><span class="sxs-lookup"><span data-stu-id="6aabb-164">**This key will be required with hello service registration key tooregister additional devices with hello StorSimple Manager service.**</span></span> <span data-ttu-id="6aabb-165">Tekintse meg a túl[StorSimple biztonsági](../articles/storsimple/storsimple-security.md) ezt a kulcsot további információt.</span><span class="sxs-lookup"><span data-stu-id="6aabb-165">Refer too[StorSimple security](../articles/storsimple/storsimple-security.md) for more information about this key.</span></span>
    
    ![StorSimple-eszköz regisztrálása, 7](./media/storsimple-configure-and-register-device-gov-u2/HCS_RegisterYourDevice7_gov-include.png)    
    
    > [!IMPORTANT]
    > <span data-ttu-id="6aabb-167">toocopy hello szöveg hello soros konzol ablakából, egyszerűen jelölje ki a hello szöveget.</span><span class="sxs-lookup"><span data-stu-id="6aabb-167">toocopy hello text from hello serial console window, simply select hello text.</span></span> <span data-ttu-id="6aabb-168">Ezután meg kell tudni toopaste hello vágólapra vagy bármilyen szövegszerkesztővel azt.</span><span class="sxs-lookup"><span data-stu-id="6aabb-168">You should then be able toopaste it in hello clipboard or any text editor.</span></span>
    > 
    > <span data-ttu-id="6aabb-169">Ne használja a Ctrl + C toocopy hello szolgáltatásadat-titkosítási kulcs.</span><span class="sxs-lookup"><span data-stu-id="6aabb-169">DO NOT use Ctrl + C toocopy hello service data encryption key.</span></span> <span data-ttu-id="6aabb-170">A Ctrl + C miatt meg tooexit hello beállítása varázsló.</span><span class="sxs-lookup"><span data-stu-id="6aabb-170">Using Ctrl + C will cause you tooexit hello setup wizard.</span></span> <span data-ttu-id="6aabb-171">Ennek eredményeképpen hello eszköz rendszergazdai jelszava nem változik, és hello eszköz visszaállítja toohello alapértelmezett jelszót.</span><span class="sxs-lookup"><span data-stu-id="6aabb-171">As a result, hello device administrator password will not be changed and hello device will revert toohello default password.</span></span>
    > 
    > 
14. <span data-ttu-id="6aabb-172">Kilépés hello soros konzolon.</span><span class="sxs-lookup"><span data-stu-id="6aabb-172">Exit hello serial console.</span></span>
15. <span data-ttu-id="6aabb-173">Térjen vissza a toohello Azure Government portálon, és végezze el az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="6aabb-173">Return toohello Azure Government Portal, and complete hello following steps:</span></span>
    
    1. <span data-ttu-id="6aabb-174">Kattintson duplán a StorSimple Manager szolgáltatás tooaccess hello **gyors üzembe helyezés** lap.</span><span class="sxs-lookup"><span data-stu-id="6aabb-174">Double-click your StorSimple Manager service tooaccess hello **Quick Start** page.</span></span>
    2. <span data-ttu-id="6aabb-175">Kattintson a **Csatlakoztatott eszközök megtekintése** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="6aabb-175">Click **View connected devices**.</span></span>
    3. <span data-ttu-id="6aabb-176">A hello **eszközök** lapon, győződjön meg arról, hogy hello eszköz sikeresen csatlakozott toohello szolgáltatás hello állapot megkeresésével.</span><span class="sxs-lookup"><span data-stu-id="6aabb-176">On hello **Devices** page, verify that hello device has successfully connected toohello service by looking up hello status.</span></span> <span data-ttu-id="6aabb-177">hello Eszközállapot kell **Online**.</span><span class="sxs-lookup"><span data-stu-id="6aabb-177">hello device status should be **Online**.</span></span>
       
        ![A StorSimple Eszközök lapja](./media/storsimple-configure-and-register-device-gov-u2/HCS_DeviceOnline-gov-include.png)
       
        <span data-ttu-id="6aabb-179">Ha hello eszköz állapota **Offline**, néhány perc alatt az online hello eszköz toocome várja.</span><span class="sxs-lookup"><span data-stu-id="6aabb-179">If hello device status is **Offline**, wait for a couple of minutes for hello device toocome online.</span></span>
       
        <span data-ttu-id="6aabb-180">Ha hello eszköz továbbra is kapcsolat nélküli üzemmódban néhány perc múlva, akkor meg kell, hogy a tűzfallal védett hálózat lett konfigurálva, a toomake [hálózati követelményei a StorSimple eszköz](../articles/storsimple/storsimple-system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="6aabb-180">If hello device is still offline after a few minutes, then you need toomake sure that your firewall network was configured as described in [networking requirements for your StorSimple device](../articles/storsimple/storsimple-system-requirements.md).</span></span>
       
        <span data-ttu-id="6aabb-181">Győződjön meg arról, hogy 9354-es port kimenő kommunikációra nyitva-e, ez használható hello service bus által a StorSimple Manager szolgáltatás-és eszköz közötti kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="6aabb-181">Verify that port 9354 is open for outbound communication as this is used by hello service bus for StorSimple Manager Service-to-device communication.</span></span>

