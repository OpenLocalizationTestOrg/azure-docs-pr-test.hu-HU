<!--author=SharS last changed: 03/17/2016-->

#### <a name="toodownload-hotfixes"></a><span data-ttu-id="dea7b-101">toodownload gyorsjavítások</span><span class="sxs-lookup"><span data-stu-id="dea7b-101">toodownload hotfixes</span></span>
<span data-ttu-id="dea7b-102">Hajtsa végre a következő lépéseket toodownload hello szoftverfrissítés hello.</span><span class="sxs-lookup"><span data-stu-id="dea7b-102">Perform hello following steps toodownload hello software update.</span></span>

1. <span data-ttu-id="dea7b-103">Indítsa el az Internet Explorert, és keresse meg a túl[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="dea7b-103">Start Internet Explorer and navigate too[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>
2. <span data-ttu-id="dea7b-104">Ha az első alkalommal használja a Microsoft Update katalógus hello ezen a számítógépen, kattintson a **telepítése** amikor felszólító tooinstall hello bővítmény a Microsoft Update katalógusból.</span><span class="sxs-lookup"><span data-stu-id="dea7b-104">If this is your first time using hello Microsoft Update Catalog on this computer, click **Install** when prompted tooinstall hello Microsoft Update Catalog add-on.</span></span>
    <span data-ttu-id="dea7b-105">![Katalógus telepítése](./media/storsimple-install-update-option-1/HCS_InstallCatalog-include.png)</span><span class="sxs-lookup"><span data-stu-id="dea7b-105">![Install catalog](./media/storsimple-install-update-option-1/HCS_InstallCatalog-include.png)</span></span>
3. <span data-ttu-id="dea7b-106">A Microsoft Update katalógus hello hello a keresési mezőbe, írja be a hello Tudásbázis (KB) száma hello gyorsjavítás kívánt toodownload, például **3063418**, és kattintson a **keresési**.</span><span class="sxs-lookup"><span data-stu-id="dea7b-106">In hello search box of hello Microsoft Update Catalog, enter hello Knowledge Base (KB) number of hello hotfix you want toodownload, for example **3063418**, and then click **Search**.</span></span>
4. <span data-ttu-id="dea7b-107">Látni fogja a hello **StorSimple frissítés 1.2-es készülékek frissítés** csomagot.</span><span class="sxs-lookup"><span data-stu-id="dea7b-107">You will see hello **StorSimple Update 1.2 Appliance Update** bundle.</span></span> <span data-ttu-id="dea7b-108">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="dea7b-108">Click **Add**.</span></span> <span data-ttu-id="dea7b-109">hello frissítés toohello kosár lesz hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="dea7b-109">hello update will be added toohello basket.</span></span>
5. <span data-ttu-id="dea7b-110">Keressen további gyorsjavítások a fenti hello táblázatban felsorolt (**3043005** és **3063416**), és adja hozzá az egyes hello kosár.</span><span class="sxs-lookup"><span data-stu-id="dea7b-110">Search for any additional hotfixes listed in hello table above (**3043005** and **3063416**), and add each hello basket.</span></span>
6. <span data-ttu-id="dea7b-111">Kattintson a **Kosár megtekintése** gombra.</span><span class="sxs-lookup"><span data-stu-id="dea7b-111">Click **View Basket**.</span></span>
   
    ![Kosár megtekintése](./media/storsimple-install-update-option-1/HCS_InstallBasket-include.png)
7. <span data-ttu-id="dea7b-113">Kattintson a **Letöltés** gombra.</span><span class="sxs-lookup"><span data-stu-id="dea7b-113">Click **Download**.</span></span> <span data-ttu-id="dea7b-114">Adja meg vagy **Tallózás** tooa helyi helyének hello tooappear tölti le.</span><span class="sxs-lookup"><span data-stu-id="dea7b-114">Specify or **Browse** tooa local location where you want hello downloads tooappear.</span></span> <span data-ttu-id="dea7b-115">hello frissítések letöltése toohello megadott helyre, majd egy almappát azonos nevet hello frissítésként hello helyezi.</span><span class="sxs-lookup"><span data-stu-id="dea7b-115">hello updates are downloaded toohello specified location and placed in a subfolder with hello same name as hello update.</span></span> <span data-ttu-id="dea7b-116">hello mappába másolt tooa hálózati megosztásra, amely elérhető hello eszköz is lehet.</span><span class="sxs-lookup"><span data-stu-id="dea7b-116">hello folder can also be copied tooa network share that is reachable from hello device.</span></span>

> [!NOTE]
> <span data-ttu-id="dea7b-117">hello gyorsjavítások mindkét hello társ vezérlő érkező üzenetek ezt a hibát a tartományvezérlők toodetect elérhetőknek kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="dea7b-117">hello hotfixes must be accessible from both controllers toodetect any potential error messages from hello peer controller.</span></span>
> 
> 

#### <a name="tooinstall-and-verify-regular-mode-hotfixes"></a><span data-ttu-id="dea7b-118">tooinstall és normál mód gyorsjavítások ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="dea7b-118">tooinstall and verify regular mode hotfixes</span></span>
<span data-ttu-id="dea7b-119">Hajtsa végre a következő lépéseket tooinstall hello, és ellenőrizze a hello normál módú gyorsjavítások.</span><span class="sxs-lookup"><span data-stu-id="dea7b-119">Perform hello following steps tooinstall and verify hello regular-mode hotfixes.</span></span> <span data-ttu-id="dea7b-120">Ha már telepítette a használatával hello Azure portál, hagyja ki azokat, amelyek túl[telepítse, és ellenőrizze a karbantartási mód gyorsjavítások](#to-install-and-verify-maintenance-mode-hotfixes).</span><span class="sxs-lookup"><span data-stu-id="dea7b-120">If you already installed them using hello Azure Portal, skip ahead too[install and verify maintenance mode hotfixes](#to-install-and-verify-maintenance-mode-hotfixes).</span></span>

1. <span data-ttu-id="dea7b-121">tooinstall hello szoftverfrissítési, hozzáférési hello Windows PowerShell felületet a StorSimple eszköz soros konzoljához a.</span><span class="sxs-lookup"><span data-stu-id="dea7b-121">tooinstall hello software update, access hello Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="dea7b-122">Hajtsa végre a hello részletes utasításait [PuTTy használata tooconnect toohello soros konzol](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="dea7b-122">Follow hello detailed instructions in [Use PuTTy tooconnect toohello serial console](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span> <span data-ttu-id="dea7b-123">Hello parancssorban nyomja le az **Enter**.</span><span class="sxs-lookup"><span data-stu-id="dea7b-123">At hello command prompt, press **Enter**.</span></span>
2. <span data-ttu-id="dea7b-124">Válassza ki **1. lehetőség** toolog teljes hozzáféréssel rendelkező toohello eszközön.</span><span class="sxs-lookup"><span data-stu-id="dea7b-124">Select **Option 1** toolog on toohello device with full access.</span></span>
3. <span data-ttu-id="dea7b-125">tooinstall hello frissítési csomag, parancssorba hello típusa:</span><span class="sxs-lookup"><span data-stu-id="dea7b-125">tooinstall hello update package, at hello command prompt, type:</span></span>
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="dea7b-126">DNS helyett IP használata a megosztás elérési útja, a fenti parancs hello.</span><span class="sxs-lookup"><span data-stu-id="dea7b-126">Use IP rather than DNS in share path in hello above command.</span></span> <span data-ttu-id="dea7b-127">hello credential paraméter csak akkor, ha egy hitelesített megosztás elérésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="dea7b-127">hello credential parameter is used only if you are accessing an authenticated share.</span></span>
   
    <span data-ttu-id="dea7b-128">Azt javasoljuk, hogy használja-e hello credential paraméter tooaccess megosztások.</span><span class="sxs-lookup"><span data-stu-id="dea7b-128">We recommend that you use hello credential parameter tooaccess shares.</span></span> <span data-ttu-id="dea7b-129">Még akkor is, megosztások, megnyitott túl "mindenki" jellemzően a toounauthenticated felhasználók nem nyitható meg.</span><span class="sxs-lookup"><span data-stu-id="dea7b-129">Even shares that are open too“everyone” are typically not open toounauthenticated users.</span></span>
   
    <span data-ttu-id="dea7b-130">Az alábbiakban egy példa látható a kimenetre.</span><span class="sxs-lookup"><span data-stu-id="dea7b-130">A sample output is shown below.</span></span>
   
    ```
    Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
    \hcsmdssoftwareupdate.exe -Credential contoso\John

    Confirm

    This operation starts hello hotfix installation and could reboot one or
    both of hello controllers. If hello device is serving I/Os, these will not
    be disrupted. Are you sure you want toocontinue?
    [Y] Yes [N] No [?] Help (default is "Y"): Y
    ```

4. <span data-ttu-id="dea7b-131">Típus **Y** amikor felszólító tooconfirm hello gyorsjavítás telepítése.</span><span class="sxs-lookup"><span data-stu-id="dea7b-131">Type **Y** when prompted tooconfirm hello hotfix installation.</span></span>
5. <span data-ttu-id="dea7b-132">Figyelje az hello frissítés hello `Get-HcsUpdateStatus` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="dea7b-132">Monitor hello update by using hello `Get-HcsUpdateStatus` cmdlet.</span></span>
   
    <span data-ttu-id="dea7b-133">hello következő minta kimenet látható hello frissítés folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="dea7b-133">hello following sample output shows hello update in progress.</span></span> <span data-ttu-id="dea7b-134">Hello `RunInprogress` lesz `True` amikor hello frissítése folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="dea7b-134">hello `RunInprogress` will be `True` when hello update is in progress.</span></span>
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp : 9/02/2015 10:36:13 PM
    LastUpdateTimestamp : 9/02/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
     <span data-ttu-id="dea7b-135">a következő minta kimenet hello azt jelzi, hogy adott hello frissítése befejeződött.</span><span class="sxs-lookup"><span data-stu-id="dea7b-135">hello following sample output indicates that hello update is finished.</span></span> <span data-ttu-id="dea7b-136">Hello `RunInProgress` lesz `False` amikor hello frissítése befejeződött.</span><span class="sxs-lookup"><span data-stu-id="dea7b-136">hello `RunInProgress` will be `False` when hello update has completed.</span></span>

    ```
    Controller1>Get-HcsUpdateStatus

    RunInprogress       : False
    LastHotfixTimestamp : 9/02/2015 10:56:13 PM
    LastUpdateTimestamp : 9/02/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
   > [!NOTE]
   > <span data-ttu-id="dea7b-137">Alkalmanként hello parancsmag jelentések `False` amikor hello frissítése még folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="dea7b-137">Occasionally, hello cmdlet reports `False` when hello update is still in progress.</span></span> <span data-ttu-id="dea7b-138">amely gyorsjavítás hello tooensure befejeződött, várjon néhány percet, futtassa újra a parancsot, és győződjön meg arról, hogy hello `RunInProgress` van `False`.</span><span class="sxs-lookup"><span data-stu-id="dea7b-138">tooensure that hello hotfix is complete, wait for a few minutes, rerun this command and verify that hello `RunInProgress` is `False`.</span></span> <span data-ttu-id="dea7b-139">Ha igen, hello gyorsjavítás befejeződött.</span><span class="sxs-lookup"><span data-stu-id="dea7b-139">If it is, then hello hotfix has completed.</span></span>
   > 
   > 
6. <span data-ttu-id="dea7b-140">Hello szoftverfrissítés befejeződése után ellenőrizze a hello rendszer szoftververziók.</span><span class="sxs-lookup"><span data-stu-id="dea7b-140">After hello software update is complete, verify hello system software versions.</span></span> <span data-ttu-id="dea7b-141">Írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="dea7b-141">Type hello following command:</span></span>
   
    `Get-HcsSystem`
   
    <span data-ttu-id="dea7b-142">A következő verziók hello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="dea7b-142">You should see hello following versions:</span></span>
   
   * <span data-ttu-id="dea7b-143">HcsSoftwareVersion: 6.3.9600.17584</span><span class="sxs-lookup"><span data-stu-id="dea7b-143">HcsSoftwareVersion: 6.3.9600.17584</span></span>
   * <span data-ttu-id="dea7b-144">CisAgentVersion: 1.0.9049.0</span><span class="sxs-lookup"><span data-stu-id="dea7b-144">CisAgentVersion: 1.0.9049.0</span></span>
   * <span data-ttu-id="dea7b-145">MdsAgentVersion: 26.0.4696.1433</span><span class="sxs-lookup"><span data-stu-id="dea7b-145">MdsAgentVersion: 26.0.4696.1433</span></span>
     
     <span data-ttu-id="dea7b-146">Hello verziószámok hello frissítés telepítését követően ne módosítsa, azt jelzi, hello gyorsjavítás tooapply sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="dea7b-146">If hello version numbers do not change after applying hello update, it indicates that hello hotfix has failed tooapply.</span></span> <span data-ttu-id="dea7b-147">Ha ezt látja, további segítségért forduljon a [Microsoft támogatási szolgálatához](../articles/storsimple/storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="dea7b-147">Should you see this, please contact [Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) for further assistance.</span></span>
7. <span data-ttu-id="dea7b-148">Ismételje meg a 3-5 tooinstall hello fennmaradó normál módú gyorsjavítás (KB3043005).</span><span class="sxs-lookup"><span data-stu-id="dea7b-148">Repeat steps 3-5 tooinstall hello remaining regular-mode hotfix (KB3043005).</span></span>

#### <a name="tooinstall-and-verify-maintenance-mode-hotfixes"></a><span data-ttu-id="dea7b-149">tooinstall és karbantartási mód gyorsjavítások ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="dea7b-149">tooinstall and verify maintenance mode hotfixes</span></span>
<span data-ttu-id="dea7b-150">Használja a KB3063416 tooinstall lemez belső vezérlőprogram-frissítésekre.</span><span class="sxs-lookup"><span data-stu-id="dea7b-150">Use KB3063416 tooinstall disk firmware updates.</span></span> <span data-ttu-id="dea7b-151">Ezek zavaró frissítések és körül toocomplete 30-45 percig tarthat.</span><span class="sxs-lookup"><span data-stu-id="dea7b-151">These are disruptive updates and take around 30-45 minutes toocomplete.</span></span> <span data-ttu-id="dea7b-152">Szerint is választhat tooinstall ezeket a tervezett karbantartások az összekötő toohello eszköz soros konzoljához.</span><span class="sxs-lookup"><span data-stu-id="dea7b-152">You can choose tooinstall these in a planned maintenance window by connecting toohello device serial console.</span></span>

<span data-ttu-id="dea7b-153">tooinstall hello lemez belső vezérlőprogram-frissítésekre, kövesse az alábbi hello utasításokat.</span><span class="sxs-lookup"><span data-stu-id="dea7b-153">tooinstall hello disk firmware updates, follow hello instructions below.</span></span>

1. <span data-ttu-id="dea7b-154">Hello eszköz helyez karbantartási módba.</span><span class="sxs-lookup"><span data-stu-id="dea7b-154">Place hello device in Maintenance mode.</span></span> <span data-ttu-id="dea7b-155">Vegye figyelembe, hogy ne használjon Windows PowerShell távoli eljáráshívás tooa eszköz karbantartási módba kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="dea7b-155">Note that you should not use Windows PowerShell remoting when connecting tooa device in Maintenance mode.</span></span> <span data-ttu-id="dea7b-156">Ez a parancsmag hello eszköz tartományvezérlőn hello eszköz soros konzolon keresztül csatlakozáskor kell toorun.</span><span class="sxs-lookup"><span data-stu-id="dea7b-156">You will need toorun this cmdlet on hello device controller when connected through hello device serial console.</span></span> <span data-ttu-id="dea7b-157">Típus:</span><span class="sxs-lookup"><span data-stu-id="dea7b-157">Type:</span></span>
   
    `Enter-HcsMaintenanceMode`
   
    <span data-ttu-id="dea7b-158">Az alábbiakban egy példa látható a kimenetre.</span><span class="sxs-lookup"><span data-stu-id="dea7b-158">A sample output is shown below.</span></span>
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from hello Microsoft Azure StorSimple Manager service. Entering maintenance mode will end hello current session and reboot both controllers, which takes a few minutes toocomplete. Are you sure you want tooenter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update1-8100-SHG0997879L76YD
        Software Version: 6.3.9600.17584
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected tooController0 - Passive
        ---------------------------------------------------------------
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    <span data-ttu-id="dea7b-159">Mindkét hello tartományvezérlők majd indítsa újra a számítógépet karbantartási módba.</span><span class="sxs-lookup"><span data-stu-id="dea7b-159">Both hello controllers then restart into Maintenance mode.</span></span>
2. <span data-ttu-id="dea7b-160">tooinstall hello lemez belső vezérlőprogram frissítése, típus:</span><span class="sxs-lookup"><span data-stu-id="dea7b-160">tooinstall hello disk firmware update, type:</span></span>
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="dea7b-161">Az alábbiakban egy példa látható a kimenetre.</span><span class="sxs-lookup"><span data-stu-id="dea7b-161">A sample output is shown below.</span></span>
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\DiskFirmwarePackage.exe -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After hello hotfix is installed on this controller, install it on hello peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of hello controllers. Are you sure you want toocontinue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes toocomplete.
3. <span data-ttu-id="dea7b-162">A figyelő hello telepítési folyamat használatával `Get-HcsUpdateStatus` parancsot.</span><span class="sxs-lookup"><span data-stu-id="dea7b-162">Monitor hello install progress using `Get-HcsUpdateStatus` command.</span></span> <span data-ttu-id="dea7b-163">hello frissítés befejeződött amikor hello `RunInProgress` túl változik`False`.</span><span class="sxs-lookup"><span data-stu-id="dea7b-163">hello update is complete when hello `RunInProgress` changes too`False`.</span></span>
4. <span data-ttu-id="dea7b-164">Hello telepítés befejezése után, amelyre hello karbantartási mód gyorsjavítás telepítve lett hello vezérlő újra kell indítani.</span><span class="sxs-lookup"><span data-stu-id="dea7b-164">After hello installation is complete, hello controller on which hello maintenance mode hotfix was installed will be rebooted.</span></span> <span data-ttu-id="dea7b-165">Jelentkezzen be 1. lehetőség teljes hozzáféréssel, és hello lemez belső vezérlőprogram verziójának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="dea7b-165">Log in as option 1 with full access and verify hello disk firmware version.</span></span> <span data-ttu-id="dea7b-166">Típus:</span><span class="sxs-lookup"><span data-stu-id="dea7b-166">Type:</span></span>
   
   `Get-HcsFirmwareVersion`
   
   <span data-ttu-id="dea7b-167">hello várt, belsővezérlőprogram-verziók a következők:</span><span class="sxs-lookup"><span data-stu-id="dea7b-167">hello expected disk firmware versions are:</span></span>
   
   `XMGG, XGEE, KZ50, F6C2, VR08`
   
   <span data-ttu-id="dea7b-168">Futtassa a hello `Get-HcsFirmwareVersion` hello második vezérlő tooverify hello szoftverének verziójával, a parancs frissítve lett.</span><span class="sxs-lookup"><span data-stu-id="dea7b-168">Run hello `Get-HcsFirmwareVersion` command on hello second controller tooverify that hello software version has been updated.</span></span> <span data-ttu-id="dea7b-169">Lépjen ki a karbantartási mód hello majd.</span><span class="sxs-lookup"><span data-stu-id="dea7b-169">You can then exit hello maintenance mode.</span></span> <span data-ttu-id="dea7b-170">Írja be a következő parancsot minden eszköz vezérlő hello:</span><span class="sxs-lookup"><span data-stu-id="dea7b-170">Type hello following command for each device controller:</span></span>
   
   `Exit-HcsMaintenanceMode`
5. <span data-ttu-id="dea7b-171">Hello, tartományvezérlői újraindítása a kilép karbantartási módból.</span><span class="sxs-lookup"><span data-stu-id="dea7b-171">hello controllers restart when you exit Maintenance mode.</span></span> <span data-ttu-id="dea7b-172">Után hello lemez belső vezérlőprogram frissítése sikeresen telepítve van, és hello eszköz már kilépett a karbantartási mód visszatérési toohello a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="dea7b-172">After hello disk firmware updates are successfully applied and hello device has exited maintenance mode, return toohello Azure classic portal.</span></span> <span data-ttu-id="dea7b-173">Vegye figyelembe, hogy hello portal előfordulhat, hogy ne jelenjen meg, hogy telepítette-e hello karbantartási mód frissítéseit 24 óra.</span><span class="sxs-lookup"><span data-stu-id="dea7b-173">Note that hello portal might not show that you installed hello Maintenance mode updates for 24 hours.</span></span>

