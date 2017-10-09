<!--author=alkohli last changed: 01/18/2017-->


#### <a name="tooconfigure-and-register-hello-device"></a><span data-ttu-id="6e57a-101">hello eszköz tooconfigure és regisztrálása</span><span class="sxs-lookup"><span data-stu-id="6e57a-101">tooconfigure and register hello device</span></span>

1. <span data-ttu-id="6e57a-102">Hozzáférés hello Windows PowerShell felületet a StorSimple eszköz soros konzoljához a.</span><span class="sxs-lookup"><span data-stu-id="6e57a-102">Access hello Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="6e57a-103">Lásd: [PuTTY használata tooconnect toohello eszköz soros konzoljához](#use-putty-to-connect-to-the-device-serial-console) utasításokat.</span><span class="sxs-lookup"><span data-stu-id="6e57a-103">See [Use PuTTY tooconnect toohello device serial console](#use-putty-to-connect-to-the-device-serial-console) for instructions.</span></span> <span data-ttu-id="6e57a-104">**Lehet, hogy toofollow hello eljárás pontosan vagy nem fog tudni tooaccess hello konzol.**</span><span class="sxs-lookup"><span data-stu-id="6e57a-104">**Be sure toofollow hello procedure exactly or you will not be able tooaccess hello console.**</span></span>

2. <span data-ttu-id="6e57a-105">Hello megnyitott munkamenetben, nyomja le a **Enter** egy idő tooget egy parancssori ablakot.</span><span class="sxs-lookup"><span data-stu-id="6e57a-105">In hello session that opens up, press **Enter** one time tooget a command prompt.</span></span>

3. <span data-ttu-id="6e57a-106">Rákérdezéses toochoose hello nyelvi, hogy milyen tooset az eszközhöz lesz.</span><span class="sxs-lookup"><span data-stu-id="6e57a-106">You will be prompted toochoose hello language that you would like tooset for your device.</span></span> <span data-ttu-id="6e57a-107">Adja meg a hello nyelv, és nyomja le az **Enter**.</span><span class="sxs-lookup"><span data-stu-id="6e57a-107">Specify hello language, and then press **Enter**.</span></span>

4. <span data-ttu-id="6e57a-108">Hello soros konzol menüben megjelenő, válassza az 1 túl**jelentkezzen be a teljes körű hozzáférési**.</span><span class="sxs-lookup"><span data-stu-id="6e57a-108">In hello serial console menu that is presented, choose option 1 too**log in with full access**.</span></span>
     <span data-ttu-id="6e57a-109">Hajtsa végre a lépéseket 5 – 12 tooconfigure hello minimálisan szükséges hálózati beállításokat az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="6e57a-109">Complete steps 5-12 tooconfigure hello minimum required network settings for your device.</span></span> <span data-ttu-id="6e57a-110">**Ezeket a konfigurációs lépéseket kell toobe hello hello eszköz aktív vezérlőjén végre.**</span><span class="sxs-lookup"><span data-stu-id="6e57a-110">**These configuration steps need toobe performed on hello active controller of hello device.**</span></span> <span data-ttu-id="6e57a-111">hello soros konzol menü hello vezérlő állapotát a szalagcím üdvözlőüzenetére jelzi.</span><span class="sxs-lookup"><span data-stu-id="6e57a-111">hello serial console menu indicates hello controller state in hello banner message.</span></span> <span data-ttu-id="6e57a-112">Ha korábban nem kapcsolódott a toohello aktív vezérlő, válassza le, és csatlakoztassa a toohello aktív vezérlő.</span><span class="sxs-lookup"><span data-stu-id="6e57a-112">If you are not connected toohello active controller, disconnect and then connect toohello active controller.</span></span>

5. <span data-ttu-id="6e57a-113">Hello parancssorba írja be a jelszavát.</span><span class="sxs-lookup"><span data-stu-id="6e57a-113">At hello command prompt, type your password.</span></span> <span data-ttu-id="6e57a-114">hello eszköz alapértelmezett jelszava: **jelszó1**.</span><span class="sxs-lookup"><span data-stu-id="6e57a-114">hello default device password is **Password1**.</span></span>

6. <span data-ttu-id="6e57a-115">A következő parancs típusa hello: `Invoke-HcsSetupWizard`.</span><span class="sxs-lookup"><span data-stu-id="6e57a-115">Type hello following command: `Invoke-HcsSetupWizard`.</span></span>

7. <span data-ttu-id="6e57a-116">A telepítő varázsló jelenik meg toohelp hello hello eszköz hálózati beállításainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6e57a-116">A setup wizard will appear toohelp you configure hello network settings for hello device.</span></span> <span data-ttu-id="6e57a-117">Adja meg a következő információ hello hello:</span><span class="sxs-lookup"><span data-stu-id="6e57a-117">Supply hello hello following information:</span></span>
   
   * <span data-ttu-id="6e57a-118">Hello DATA 0 hálózati adapterén IP-címe</span><span class="sxs-lookup"><span data-stu-id="6e57a-118">IP address for hello DATA 0 network interface</span></span>
   * <span data-ttu-id="6e57a-119">Alhálózati maszk</span><span class="sxs-lookup"><span data-stu-id="6e57a-119">Subnet mask</span></span>
   * <span data-ttu-id="6e57a-120">Átjáró</span><span class="sxs-lookup"><span data-stu-id="6e57a-120">Gateway</span></span>
   * <span data-ttu-id="6e57a-121">Az elsődleges DNS-kiszolgáló IP-címe</span><span class="sxs-lookup"><span data-stu-id="6e57a-121">IP address for Primary DNS server</span></span>

   <span data-ttu-id="6e57a-122">Az alábbiakban egy példa látható a kimenetre.</span><span class="sxs-lookup"><span data-stu-id="6e57a-122">A sample output is presented below.</span></span>

    ```
        ---------------------------------------------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: 8100-SHX0991003G44MT
        Software Version: 6.3.9600.17759
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected tooController0 - Active
        ---------------------------------------------------------------

        Your device needs toobe registered with hello Microsoft Azure StorSimple Manager service. Please run 'Invoke-HcsSetupWizard' tooset up your device.

        Controller0>Invoke-HcsSetupWizard

        Which IP address family would you like tooconfigure on interface Data0?
        [4] IPv4 [6] IPv6 [B] Both (Default is "4"): 4

        Data0 IPv4 address:10.111.111.00
        Data0 IPv4 subnet: 255.255.252.0
        Data0 IPv4 gateway: 10.111.111.11

        IPv4 primary DNS server [10.222.118.154]:10.222.222.111
    ```

    <br>
    <span data-ttu-id="6e57a-123">A minta kimenet megelőző hello láthatja, hogy hello rendszer hello folyamat minden lépése után érvényesíti a hálózati beállításokat.</span><span class="sxs-lookup"><span data-stu-id="6e57a-123">In hello preceding sample output, you can see that hello system is validating network settings after each step in hello process.</span></span>

     > [!NOTE]
     > <span data-ttu-id="6e57a-124">Előfordulhat, hogy toowait hello alhálózati maszk és hello DNS beállítások toobe alkalmazása néhány percig.</span><span class="sxs-lookup"><span data-stu-id="6e57a-124">You may have toowait for a few minutes for hello subnet mask and hello DNS settings toobe applied.</span></span> <span data-ttu-id="6e57a-125">Ha a "Jelölőnégyzet hello hálózati kapcsolat tooData 0" hiba jelenik meg, ellenőrizze az aktív vezérlő hello DATA 0 hálózati adapterén hello fizikai hálózati kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="6e57a-125">If you get a "Check hello network connectivity tooData 0" error message, check hello physical network connection on hello DATA 0 network interface of your active controller.</span></span>

8. <span data-ttu-id="6e57a-126">(Nem kötelező) konfigurálja a webproxy-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="6e57a-126">(Optional) configure your web proxy server.</span></span> <span data-ttu-id="6e57a-127">Bár a webproxy konfigurálása nem kötelező, **vegye figyelembe, hogy ha webproxyt használ, azt csak itt tudja beállítani**.</span><span class="sxs-lookup"><span data-stu-id="6e57a-127">Although web proxy configuration is optional, **be aware that if you use a web proxy, you can only configure it here**.</span></span> <span data-ttu-id="6e57a-128">További információ: túl[az eszköz webalkalmazás-proxy konfigurálása](../articles/storsimple/storsimple-8000-configure-web-proxy.md).</span><span class="sxs-lookup"><span data-stu-id="6e57a-128">For more information, go too[Configure web proxy for your device](../articles/storsimple/storsimple-8000-configure-web-proxy.md).</span></span>
9. <span data-ttu-id="6e57a-129">Állítson be egy elsődleges NTP-kiszolgálót az eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="6e57a-129">Configure a Primary NTP server for your device.</span></span> <span data-ttu-id="6e57a-130">Az NTP-kiszolgálókra azért van szükség, mert az eszköznek szinkronizálnia kell az időt ahhoz, hogy a felhőszolgáltatókkal hitelesítést végezhessen.</span><span class="sxs-lookup"><span data-stu-id="6e57a-130">NTP servers are required, as your device must synchronize time so that it can authenticate with your cloud service providers.</span></span> <span data-ttu-id="6e57a-131">Győződjön meg arról, hogy a hálózat engedélyezi-e a datacenter toohello Internet az NTP-forgalom toopass.</span><span class="sxs-lookup"><span data-stu-id="6e57a-131">Ensure that your network allows NTP traffic toopass from your datacenter toohello Internet.</span></span> <span data-ttu-id="6e57a-132">Ha ez nem lehetséges, akkor adjon meg egy belső NTP-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="6e57a-132">If this is not possible, specify an internal NTP server.</span></span>

    <span data-ttu-id="6e57a-133">Az alábbiakban egy példa látható a kimenetre.</span><span class="sxs-lookup"><span data-stu-id="6e57a-133">A sample output is shown below.</span></span>

    ```
        Would you like tooconfigure a web proxy?
        [Y] Yes [N] No (Default is "N"):N

        Primary NTP server [time.windows.com]:time.windows.com

    ```

10. <span data-ttu-id="6e57a-134">Biztonsági okokból hello eszköz rendszergazdai jelszava hello első munkamenet végeztével lejár, és szüksége lesz a toochange informatikai most.</span><span class="sxs-lookup"><span data-stu-id="6e57a-134">For security reasons, hello device administrator password expires after hello first session, and you will need toochange it now.</span></span> <span data-ttu-id="6e57a-135">Amikor a rendszer erre kéri, adjon meg az eszközhöz egy rendszergazdai jelszót.</span><span class="sxs-lookup"><span data-stu-id="6e57a-135">When prompted, provide a device administrator password.</span></span> <span data-ttu-id="6e57a-136">Az eszköz rendszergazdai jelszavának 8–15 karakter hosszúságúnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6e57a-136">A valid device administrator password must be between 8 and 15 characters.</span></span> <span data-ttu-id="6e57a-137">hello jelszó közül hármat tartalmaznia kell következő hello:, kisbetű, nagybetű, numerikus és speciális karaktereket.</span><span class="sxs-lookup"><span data-stu-id="6e57a-137">hello password must contain three of hello following: lowercase, uppercase, numeric, and special characters.</span></span>

    ```
        hello device administrator password must be between 8 and 15 characters. hello password must contain a combination of uppercase letters, lowercase letters, numbers and special characters.
        Administrator Password:********
        Confirm Administrator Password:********
    ```
11. <span data-ttu-id="6e57a-138">hello hello telepítővarázsló utolsó lépésként regisztrálja az eszközt hello StorSimple Device Manager szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="6e57a-138">hello final step in hello setup wizard registers your device with hello StorSimple Device Manager service.</span></span> <span data-ttu-id="6e57a-139">Ehhez szüksége lesz a 2. lépésben beszerzett hello Szolgáltatásregisztrációs kulcs.</span><span class="sxs-lookup"><span data-stu-id="6e57a-139">For this, you will need hello service registration key that you obtained in step 2.</span></span> <span data-ttu-id="6e57a-140">Hello regisztrációs kulcsot fogja tartalmazni, miután szükséges toowait 2-3 percet hello eszköz regisztrálva van-e.</span><span class="sxs-lookup"><span data-stu-id="6e57a-140">After you supply hello registration key, you may need toowait for 2-3 minutes before hello device is registered.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="6e57a-141">Minden alkalommal tooexit hello beállítása varázsló, is használja a Ctrl + C.</span><span class="sxs-lookup"><span data-stu-id="6e57a-141">You can press Ctrl + C at any time tooexit hello setup wizard.</span></span> <span data-ttu-id="6e57a-142">Ha a megadott összes hello hálózati beállításokat (a Data 0, alhálózati maszk és átjáró IP-cím), a bejegyzéseket megőrzi a rendszer.</span><span class="sxs-lookup"><span data-stu-id="6e57a-142">If you have entered all hello network settings (IP address for Data 0, Subnet mask, and Gateway), your entries will be retained.</span></span>
    
    <span data-ttu-id="6e57a-143">Az alábbiakban egy példa látható a kimenetre.</span><span class="sxs-lookup"><span data-stu-id="6e57a-143">A sample output is shown below.</span></span>

    ```
        hello service registration key is available in hello StorSimple Manager service.
        Enter service registration key:**************************************
        Device registration is in progress. Please wait.

    ```

12. <span data-ttu-id="6e57a-144">Hello eszköz regisztrálása után megjelenik egy szolgáltatásadat-titkosítási kulcs.</span><span class="sxs-lookup"><span data-stu-id="6e57a-144">After hello device is registered, a Service Data Encryption key will appear.</span></span> <span data-ttu-id="6e57a-145">Másolja ki ezt a kulcsot, és mentse egy biztonságos helyre.</span><span class="sxs-lookup"><span data-stu-id="6e57a-145">Copy this key and save it in a safe location.</span></span> <span data-ttu-id="6e57a-146">**Ezt a kulcsot kötelező megadni a hello szolgáltatás regisztrációs kulcs tooregister további eszközök hello StorSimple Device Manager szolgáltatás lesz.**</span><span class="sxs-lookup"><span data-stu-id="6e57a-146">**This key will be required with hello service registration key tooregister additional devices with hello StorSimple Device Manager service.**</span></span> <span data-ttu-id="6e57a-147">Tekintse meg a túl[StorSimple biztonsági](../articles/storsimple/storsimple-security.md) ezt a kulcsot további információt.</span><span class="sxs-lookup"><span data-stu-id="6e57a-147">Refer too[StorSimple security](../articles/storsimple/storsimple-security.md) for more information about this key.</span></span>
    
    ![StorSimple-eszköz regisztrálása, 7](./media/storsimple-8000-configure-and-register-device-u2/step3pssetup1.png)
    
    > [!NOTE]
    > <span data-ttu-id="6e57a-149">toocopy hello szöveg hello soros konzol ablakából, egyszerűen jelölje ki a hello szöveget.</span><span class="sxs-lookup"><span data-stu-id="6e57a-149">toocopy hello text from hello serial console window, simply select hello text.</span></span> <span data-ttu-id="6e57a-150">Ezután meg kell tudni toopaste hello vágólapra vagy bármilyen szövegszerkesztővel azt.</span><span class="sxs-lookup"><span data-stu-id="6e57a-150">You should then be able toopaste it in hello clipboard or any text editor.</span></span> <span data-ttu-id="6e57a-151">Ne használja a Ctrl + C toocopy hello szolgáltatásadat-titkosítási kulcs.</span><span class="sxs-lookup"><span data-stu-id="6e57a-151">DO NOT use Ctrl + C toocopy hello service data encryption key.</span></span> <span data-ttu-id="6e57a-152">A Ctrl + C miatt meg tooexit hello beállítása varázsló.</span><span class="sxs-lookup"><span data-stu-id="6e57a-152">Using Ctrl + C will cause you tooexit hello setup wizard.</span></span> <span data-ttu-id="6e57a-153">Ennek eredményeképpen hello eszköz rendszergazdai jelszava nem változik, és hello eszköz visszaállítja toohello alapértelmezett jelszót.</span><span class="sxs-lookup"><span data-stu-id="6e57a-153">As a result, hello device administrator password will not be changed and hello device will revert toohello default password.</span></span>
    
13. <span data-ttu-id="6e57a-154">Kilépés hello soros konzolon.</span><span class="sxs-lookup"><span data-stu-id="6e57a-154">Exit hello serial console.</span></span>
14. <span data-ttu-id="6e57a-155">Térjen vissza a toohello Azure-portálon, és végezze el az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="6e57a-155">Return toohello Azure portal, and complete hello following steps:</span></span>
    
    1. <span data-ttu-id="6e57a-156">Nyissa meg tooyour StorSimple Device Manager szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="6e57a-156">Go tooyour StorSimple Device Manager service.</span></span>
    2. <span data-ttu-id="6e57a-157">Kattintson az **Eszközök** elemre.</span><span class="sxs-lookup"><span data-stu-id="6e57a-157">Click **Devices**.</span></span>
    3. <span data-ttu-id="6e57a-158">Hello táblázatos eszközök listája ellenőrizze, hogy hello eszköz sikeresen csatlakozott toohello szolgáltatás hello állapot megkeresésével.</span><span class="sxs-lookup"><span data-stu-id="6e57a-158">In hello tabular listing of devices, verify that hello device has successfully connected toohello service by looking up hello status.</span></span> <span data-ttu-id="6e57a-159">hello Eszközállapot kell **tooset kész mentése**.</span><span class="sxs-lookup"><span data-stu-id="6e57a-159">hello device status should be **Ready tooset up**.</span></span>
       
        ![A StorSimple Eszközök lapja](./media/storsimple-8000-configure-and-register-device-u2/step3pssetup2.png)
       
        <span data-ttu-id="6e57a-161">Szükség lehet toowait néhány percig, amíg hello eszköz állapota toochange túl**tooset kész mentése**.</span><span class="sxs-lookup"><span data-stu-id="6e57a-161">You may need toowait for a couple of minutes for hello device status toochange too**Ready tooset up**.</span></span>
       
        <span data-ttu-id="6e57a-162">Ha hello eszköz nem jelenik meg ezen a listán, akkor meg kell, hogy a tűzfallal védett hálózat lett konfigurálva, a toomake [hálózati követelményei a StorSimple eszköz](../articles/storsimple/storsimple-8000-system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="6e57a-162">If hello device does not show up in this list, then you need toomake sure that your firewall network was configured as described in [networking requirements for your StorSimple device](../articles/storsimple/storsimple-8000-system-requirements.md).</span></span> <span data-ttu-id="6e57a-163">Győződjön meg arról, hogy 9354-es port kimenő kommunikációra nyitva-e, ez használható hello service bus által a StorSimple Device Manager szolgáltatás és eszköz közötti kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="6e57a-163">Verify that port 9354 is open for outbound communication as this is used by hello service bus for StorSimple Device Manager service-to-device communication.</span></span>

