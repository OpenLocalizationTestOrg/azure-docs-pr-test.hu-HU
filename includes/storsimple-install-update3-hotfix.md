<!--author=alkohli last changed: 09/01/16-->

#### <a name="toodownload-hotfixes"></a><span data-ttu-id="b8789-101">toodownload gyorsjavítások</span><span class="sxs-lookup"><span data-stu-id="b8789-101">toodownload hotfixes</span></span>
<span data-ttu-id="b8789-102">Hajtsa végre a következő lépéseket toodownload hello szoftverfrissítést a Microsoft Update katalógus hello hello.</span><span class="sxs-lookup"><span data-stu-id="b8789-102">Perform hello following steps toodownload hello software update from hello Microsoft Update Catalog.</span></span>

1. <span data-ttu-id="b8789-103">Indítsa el az Internet Explorert, és keresse meg a túl[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="b8789-103">Start Internet Explorer and navigate too[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>
2. <span data-ttu-id="b8789-104">Ha az első alkalommal használja a Microsoft Update katalógus hello ezen a számítógépen, kattintson a **telepítése** amikor felszólító tooinstall hello bővítmény a Microsoft Update katalógusból.</span><span class="sxs-lookup"><span data-stu-id="b8789-104">If this is your first time using hello Microsoft Update Catalog on this computer, click **Install** when prompted tooinstall hello Microsoft Update Catalog add-on.</span></span>
    <span data-ttu-id="b8789-105">![Katalógus telepítése](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)</span><span class="sxs-lookup"><span data-stu-id="b8789-105">![Install catalog](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)</span></span>
3. <span data-ttu-id="b8789-106">A Microsoft Update katalógus hello hello a keresési mezőbe, írja be a hello Tudásbázis (KB) száma hello gyorsjavítás kívánt toodownload, például **3186843**, és kattintson a **keresési**.</span><span class="sxs-lookup"><span data-stu-id="b8789-106">In hello search box of hello Microsoft Update Catalog, enter hello Knowledge Base (KB) number of hello hotfix you want toodownload, for example **3186843**, and then click **Search**.</span></span>
   
    <span data-ttu-id="b8789-107">hello gyorsjavítás lista megjelenik, például **összegző szoftverfrissítési csomagot frissítés 3.0 a StorSimple 8000 Series**.</span><span class="sxs-lookup"><span data-stu-id="b8789-107">hello hotfix listing appears, for example, **Cumulative Software Bundle Update 3.0 for StorSimple 8000 Series**.</span></span>
   
    ![Keresés a katalógusban](./media/storsimple-install-update2-hotfix/HCS_SearchCatalog1-include.png)
4. <span data-ttu-id="b8789-109">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="b8789-109">Click **Add**.</span></span> <span data-ttu-id="b8789-110">hello frissítés hozzá legyen adva toohello kosár.</span><span class="sxs-lookup"><span data-stu-id="b8789-110">hello update is added toohello basket.</span></span>
5. <span data-ttu-id="b8789-111">Keressen további gyorsjavítások a fenti hello táblázatban felsorolt (**3186859**), és adja hozzá az egyes toohello kosár.</span><span class="sxs-lookup"><span data-stu-id="b8789-111">Search for any additional hotfixes listed in hello table above (**3186859**), and add each toohello basket.</span></span>
6. <span data-ttu-id="b8789-112">Kattintson a **Kosár megtekintése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b8789-112">Click **View Basket**.</span></span>
7. <span data-ttu-id="b8789-113">Kattintson a **Letöltés** gombra.</span><span class="sxs-lookup"><span data-stu-id="b8789-113">Click **Download**.</span></span> <span data-ttu-id="b8789-114">Adja meg vagy **Tallózás** tooa helyi helyének hello tooappear tölti le.</span><span class="sxs-lookup"><span data-stu-id="b8789-114">Specify or **Browse** tooa local location where you want hello downloads tooappear.</span></span> <span data-ttu-id="b8789-115">hello frissítések letöltése toohello megadott helyre, és az azonos név hello frissítésként hello alárendelt mappába helyezi.</span><span class="sxs-lookup"><span data-stu-id="b8789-115">hello updates are downloaded toohello specified location and placed in a sub-folder with hello same name as hello update.</span></span> <span data-ttu-id="b8789-116">hello mappába másolt tooa hálózati megosztásra, amely elérhető hello eszköz is lehet.</span><span class="sxs-lookup"><span data-stu-id="b8789-116">hello folder can also be copied tooa network share that is reachable from hello device.</span></span>

> [!NOTE]
> <span data-ttu-id="b8789-117">hello gyorsjavítások mindkét hello társ vezérlő érkező üzenetek ezt a hibát a tartományvezérlők toodetect elérhetőknek kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="b8789-117">hello hotfixes must be accessible from both controllers toodetect any potential error messages from hello peer controller.</span></span>
> 
> 

#### <a name="tooinstall-and-verify-regular-mode-hotfixes"></a><span data-ttu-id="b8789-118">tooinstall és normál mód gyorsjavítások ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="b8789-118">tooinstall and verify regular mode hotfixes</span></span>
<span data-ttu-id="b8789-119">Hajtsa végre a következő lépéseket tooinstall hello, és ellenőrizze a normál módú gyorsjavítások.</span><span class="sxs-lookup"><span data-stu-id="b8789-119">Perform hello following steps tooinstall and verify regular-mode hotfixes.</span></span> <span data-ttu-id="b8789-120">Ha már telepítette a használatával hello Azure portál, hagyja ki azokat, amelyek túl[telepítse, és ellenőrizze a karbantartási mód gyorsjavítások](#to-install-and-verify-maintenance-mode-hotfixes).</span><span class="sxs-lookup"><span data-stu-id="b8789-120">If you already installed them using hello Azure Portal, skip ahead too[install and verify maintenance mode hotfixes](#to-install-and-verify-maintenance-mode-hotfixes).</span></span>

1. <span data-ttu-id="b8789-121">tooinstall hello gyorsjavítások, hozzáférési hello Windows PowerShell felületet a StorSimple eszköz soros konzoljához a.</span><span class="sxs-lookup"><span data-stu-id="b8789-121">tooinstall hello hotfixes, access hello Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="b8789-122">Hajtsa végre a hello részletes utasításait [PuTTy használata tooconnect toohello soros konzol](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="b8789-122">Follow hello detailed instructions in [Use PuTTy tooconnect toohello serial console](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span> <span data-ttu-id="b8789-123">Hello parancssorban nyomja le az **Enter**.</span><span class="sxs-lookup"><span data-stu-id="b8789-123">At hello command prompt, press **Enter**.</span></span>
2. <span data-ttu-id="b8789-124">Válassza ki **1. lehetőség** toolog teljes hozzáféréssel rendelkező toohello eszközön.</span><span class="sxs-lookup"><span data-stu-id="b8789-124">Select **Option 1** toolog on toohello device with full access.</span></span> <span data-ttu-id="b8789-125">Azt javasoljuk, hogy hello gyorsjavítás hello passzív tartományvezérlőre telepíti először.</span><span class="sxs-lookup"><span data-stu-id="b8789-125">We recommend that you install hello hotfix on hello passive controller first.</span></span>
3. <span data-ttu-id="b8789-126">tooinstall hello gyorsjavítás, parancssorba hello típusa:</span><span class="sxs-lookup"><span data-stu-id="b8789-126">tooinstall hello hotfix, at hello command prompt, type:</span></span>
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="b8789-127">DNS helyett IP használata a megosztás elérési útja, a fenti parancs hello.</span><span class="sxs-lookup"><span data-stu-id="b8789-127">Use IP rather than DNS in share path in hello above command.</span></span> <span data-ttu-id="b8789-128">hello credential paraméter csak akkor, ha egy hitelesített megosztás elérésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="b8789-128">hello credential parameter is used only if you are accessing an authenticated share.</span></span>
   
    <span data-ttu-id="b8789-129">Azt javasoljuk, hogy használja-e hello credential paraméter tooaccess megosztások.</span><span class="sxs-lookup"><span data-stu-id="b8789-129">We recommend that you use hello credential parameter tooaccess shares.</span></span> <span data-ttu-id="b8789-130">Még akkor is, megosztások, megnyitott túl "mindenki" jellemzően a toounauthenticated felhasználók nem nyitható meg.</span><span class="sxs-lookup"><span data-stu-id="b8789-130">Even shares that are open too“everyone” are typically not open toounauthenticated users.</span></span>
   
    <span data-ttu-id="b8789-131">Adja meg a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="b8789-131">Supply hello password when prompted.</span></span>
   
    <span data-ttu-id="b8789-132">Az alábbiakban egy példa látható a kimenetre.</span><span class="sxs-lookup"><span data-stu-id="b8789-132">A sample output is shown below.</span></span>
   
        ````
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \hcsmdssoftwareupdate.exe -Credential contoso\John
   
        Confirm
   
        This operation starts hello hotfix installation and could reboot one or
        both of hello controllers. If hello device is serving I/Os, these will not
        be disrupted. Are you sure you want toocontinue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y
   
        ````
4. <span data-ttu-id="b8789-133">Típus **Y** amikor felszólító tooconfirm hello gyorsjavítás telepítése.</span><span class="sxs-lookup"><span data-stu-id="b8789-133">Type **Y** when prompted tooconfirm hello hotfix installation.</span></span>
5. <span data-ttu-id="b8789-134">Figyelje az hello frissítés hello `Get-HcsUpdateStatus` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b8789-134">Monitor hello update by using hello `Get-HcsUpdateStatus` cmdlet.</span></span> <span data-ttu-id="b8789-135">hello frissítés először befejezi az hello passzív történik.</span><span class="sxs-lookup"><span data-stu-id="b8789-135">hello update will first complete on hello passive controller.</span></span> <span data-ttu-id="b8789-136">Miután hello passzív vezérlő frissül, a feladatátvétel, és hello frissítés majd lesznek érvényben lévő hello más vezérlő.</span><span class="sxs-lookup"><span data-stu-id="b8789-136">Once hello passive controller is updated, there will be a failover and hello update will then get applied on hello other controller.</span></span> <span data-ttu-id="b8789-137">hello frissítés befejeződött, mindkét hello tartományvezérlők frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="b8789-137">hello update is complete when both hello controllers are updated.</span></span>
   
    <span data-ttu-id="b8789-138">hello következő minta kimenet látható hello frissítés folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="b8789-138">hello following sample output shows hello update in progress.</span></span> <span data-ttu-id="b8789-139">Hello `RunInprogress` lesz `True` amikor hello frissítése folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="b8789-139">hello `RunInprogress` will be `True` when hello update is in progress.</span></span>

    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 8/29/2016 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     <span data-ttu-id="b8789-140">a következő minta kimenet hello azt jelzi, hogy adott hello frissítése befejeződött.</span><span class="sxs-lookup"><span data-stu-id="b8789-140">hello following sample output indicates that hello update is finished.</span></span> <span data-ttu-id="b8789-141">Hello `RunInProgress` lesz `False` amikor hello frissítése befejeződött.</span><span class="sxs-lookup"><span data-stu-id="b8789-141">hello `RunInProgress` will be `False` when hello update has completed.</span></span>
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 8/30/2016 9:15:55 AM
    LastUpdateTimestamp : 8/30/2016 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE] 
    > <span data-ttu-id="b8789-142">Alkalmanként hello parancsmag jelentések `False` amikor hello frissítése még folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="b8789-142">Occasionally, hello cmdlet reports `False` when hello update is still in progress.</span></span> <span data-ttu-id="b8789-143">amely gyorsjavítás hello tooensure befejeződött, várjon néhány percet, futtassa újra a parancsot, és győződjön meg arról, hogy hello `RunInProgress` van `False`.</span><span class="sxs-lookup"><span data-stu-id="b8789-143">tooensure that hello hotfix is complete, wait for a few minutes, rerun this command and verify that hello `RunInProgress` is `False`.</span></span> <span data-ttu-id="b8789-144">Ha igen, hello gyorsjavítás befejeződött.</span><span class="sxs-lookup"><span data-stu-id="b8789-144">If it is, then hello hotfix has completed.</span></span>

1. <span data-ttu-id="b8789-145">Hello szoftverfrissítés befejeződése után ellenőrizze a hello rendszer szoftververziók.</span><span class="sxs-lookup"><span data-stu-id="b8789-145">After hello software update is complete, verify hello system software versions.</span></span> <span data-ttu-id="b8789-146">Típus:</span><span class="sxs-lookup"><span data-stu-id="b8789-146">Type:</span></span>
   
    `Get-HcsSystem`
   
    <span data-ttu-id="b8789-147">A következő verziók hello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="b8789-147">You should see hello following versions:</span></span>
   
   * `HcsSoftwareVersion: 6.3.9600.17759`
   * `CisAgentVersion:  1.0.9343.0`
   * `MdsAgentVersion: 30.0.4698.16`
     
     <span data-ttu-id="b8789-148">Hello verziószámok hello frissítés telepítését követően ne módosítsa, azt jelzi, hello gyorsjavítás tooapply sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="b8789-148">If hello version numbers do not change after applying hello update, it indicates that hello hotfix has failed tooapply.</span></span> <span data-ttu-id="b8789-149">Ha ezt látja, további segítségért forduljon a [Microsoft támogatási szolgálatához](../articles/storsimple/storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="b8789-149">Should you see this, please contact [Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) for further assistance.</span></span>
     
     > [!IMPORTANT]
     > <span data-ttu-id="b8789-150">Újra kell indítania a hello aktív vezérlő keresztül hello `Restart-HcsController` parancsmag hello fennmaradó frissítések alkalmazása előtt.</span><span class="sxs-lookup"><span data-stu-id="b8789-150">You must restart hello active controller via hello `Restart-HcsController` cmdlet before applying hello remaining updates.</span></span> 
     > 
     > 
2. <span data-ttu-id="b8789-151">Ismételje meg a 3-5 tooinstall hello LSI illesztőprogram és a belső vezérlőprogram gyorsjavítást **KB3186859**.</span><span class="sxs-lookup"><span data-stu-id="b8789-151">Repeat steps 3-5 tooinstall hello LSI driver and firmware hotfix **KB3186859**.</span></span> <span data-ttu-id="b8789-152">Miután hello gyorsjavítás telepítve van, a hello `Get-HcsSystem` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b8789-152">After hello hotfix is installed, use hello `Get-HcsSystem` cmdlet.</span></span> <span data-ttu-id="b8789-153">hello LSI verziószámának kell lennie:</span><span class="sxs-lookup"><span data-stu-id="b8789-153">hello LSI version should be:</span></span>
   
   * `Lsisas2Version: 2.0.78.00`
3. <span data-ttu-id="b8789-154">Ismételje meg a 3-5 tooinstall hello Storport és Spaceport frissítés **KB3121261**.</span><span class="sxs-lookup"><span data-stu-id="b8789-154">Repeat steps 3-5 tooinstall hello Storport and Spaceport update **KB3121261**.</span></span>
4. <span data-ttu-id="b8789-155">2. frissítés vagy korábbi verzió frissítésekor, toodownload is szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="b8789-155">If you are updating from Update 2 or earlier version, you will also need toodownload:</span></span>
   
   * <span data-ttu-id="b8789-156">az iSCSI-javítás KB3146621 használatával</span><span class="sxs-lookup"><span data-stu-id="b8789-156">iSCSI fix using KB3146621</span></span>
   * <span data-ttu-id="b8789-157">WMI-javítás KB3103616 használatával</span><span class="sxs-lookup"><span data-stu-id="b8789-157">WMI fix using KB3103616</span></span>

#### <a name="tooinstall-and-verify-maintenance-mode-hotfixes"></a><span data-ttu-id="b8789-158">tooinstall és karbantartási mód gyorsjavítások ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="b8789-158">tooinstall and verify maintenance mode hotfixes</span></span>
<span data-ttu-id="b8789-159">Használja a KB3121899 tooinstall lemez belső vezérlőprogram-frissítésekre.</span><span class="sxs-lookup"><span data-stu-id="b8789-159">Use KB3121899 tooinstall disk firmware updates.</span></span> <span data-ttu-id="b8789-160">Ezek zavaró frissítések, és körülbelül 30 percet toocomplete igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="b8789-160">These are disruptive updates and take around 30 minutes toocomplete.</span></span> <span data-ttu-id="b8789-161">Szerint is választhat tooinstall ezeket a tervezett karbantartások az összekötő toohello eszköz soros konzoljához.</span><span class="sxs-lookup"><span data-stu-id="b8789-161">You can choose tooinstall these in a planned maintenance window by connecting toohello device serial console.</span></span>

<span data-ttu-id="b8789-162">Vegye figyelembe, ha a lemez belső vezérlőprogram már naprakész, akkor nem kell tooinstall ezeket a frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="b8789-162">Note that if your disk firmware is already up-to-date, you won't need tooinstall these updates.</span></span> <span data-ttu-id="b8789-163">Futtassa a hello `Get-HcsUpdateAvailability` hello eszköz soros konzoljához toocheck, ha frissítések érhetők el, és hogy hello parancsmag frissíti biztosan zavaró (karbantartás mód) vagy nem zavaró (normál mód) frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="b8789-163">Run hello `Get-HcsUpdateAvailability` cmdlet from hello device serial console toocheck if updates are available and whether hello updates are disruptive (maintenance mode) or non-disruptive (regular mode) updates.</span></span>

<span data-ttu-id="b8789-164">tooinstall hello lemez belső vezérlőprogram-frissítésekre, kövesse az alábbi hello utasításokat.</span><span class="sxs-lookup"><span data-stu-id="b8789-164">tooinstall hello disk firmware updates, follow hello instructions below.</span></span>

1. <span data-ttu-id="b8789-165">Állítsa hello eszköz hello karbantartási módba.</span><span class="sxs-lookup"><span data-stu-id="b8789-165">Place hello device in hello Maintenance mode.</span></span> <span data-ttu-id="b8789-166">Vegye figyelembe, hogy ne használjon Windows PowerShell távoli eljáráshívás tooa eszköz karbantartási módba kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="b8789-166">Note that you should not use Windows PowerShell remoting when connecting tooa device in Maintenance mode.</span></span> <span data-ttu-id="b8789-167">Ehelyett futtassa ezt a parancsmagot hello eszköz tartományvezérlőn hello eszköz soros konzolon keresztül csatlakozáskor.</span><span class="sxs-lookup"><span data-stu-id="b8789-167">Instead run this cmdlet on hello device controller when connected through hello device serial console.</span></span> <span data-ttu-id="b8789-168">Típus:</span><span class="sxs-lookup"><span data-stu-id="b8789-168">Type:</span></span>
   
    `Enter-HcsMaintenanceMode`
   
    <span data-ttu-id="b8789-169">Az alábbiakban egy példa látható a kimenetre.</span><span class="sxs-lookup"><span data-stu-id="b8789-169">A sample output is shown below.</span></span>
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from hello Microsoft Azure StorSimple Manager service. Entering maintenance mode will end hello current session and reboot both controllers, which takes a few minutes toocomplete. Are you sure you want tooenter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update2-8100-SHG0997879L76673
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected tooController0 - Passive
        ---------------------------------------------------------------
   
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    <span data-ttu-id="b8789-170">Mindkét hello tartományvezérlők majd indítsa újra a számítógépet karbantartási módba.</span><span class="sxs-lookup"><span data-stu-id="b8789-170">Both hello controllers then restart into Maintenance mode.</span></span>
2. <span data-ttu-id="b8789-171">tooinstall hello lemez belső vezérlőprogram frissítése, típus:</span><span class="sxs-lookup"><span data-stu-id="b8789-171">tooinstall hello disk firmware update, type:</span></span>
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="b8789-172">Az alábbiakban egy példa látható a kimenetre.</span><span class="sxs-lookup"><span data-stu-id="b8789-172">A sample output is shown below.</span></span>
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\DiskFirmwarePackage.exe -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After hello hotfix is installed on this controller, install it on hello peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of hello controllers. By installing new updates you agree to, and accept any additional terms associated with, hello new functionality listed in hello release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want toocontinue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes toocomplete.
3. <span data-ttu-id="b8789-173">A figyelő hello telepítési folyamat használatával `Get-HcsUpdateStatus` parancsot.</span><span class="sxs-lookup"><span data-stu-id="b8789-173">Monitor hello install progress using `Get-HcsUpdateStatus` command.</span></span> <span data-ttu-id="b8789-174">hello frissítés befejeződött amikor hello `RunInProgress` túl változik`False`.</span><span class="sxs-lookup"><span data-stu-id="b8789-174">hello update is complete when hello `RunInProgress` changes too`False`.</span></span>
4. <span data-ttu-id="b8789-175">Hello telepítés befejezése után újraindítja a hello vezérlő melyik hello karbantartási mód gyorsjavítás telepítve lett.</span><span class="sxs-lookup"><span data-stu-id="b8789-175">After hello installation is complete, hello controller on which hello maintenance mode hotfix was installed restarts.</span></span> <span data-ttu-id="b8789-176">Jelentkezzen be 1. lehetőség teljes hozzáféréssel, és hello lemez belső vezérlőprogram verziójának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="b8789-176">Log in as option 1 with full access and verify hello disk firmware version.</span></span> <span data-ttu-id="b8789-177">Típus:</span><span class="sxs-lookup"><span data-stu-id="b8789-177">Type:</span></span>
   
   `Get-HcsFirmwareVersion`
   
   <span data-ttu-id="b8789-178">hello várt, belsővezérlőprogram-verziók a következők:</span><span class="sxs-lookup"><span data-stu-id="b8789-178">hello expected disk firmware versions are:</span></span>
   
   `XMGG, XGEG, KZ50, F6C2, VR08`
   
   <span data-ttu-id="b8789-179">Az alábbiakban egy példa látható a kimenetre.</span><span class="sxs-lookup"><span data-stu-id="b8789-179">A sample output is shown below.</span></span>
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8100
       Name: Update2-8100-SHG0997879L76YD
       Software Version: 6.3.9600.17705
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected tooController1
       ---------------------------------------------------------------
   
       Controller1>Get-HcsFirmwareVersion
   
       Controller0 : TalladegaFirmware
         ActiveBIOS:0.45.0006
         BackupBIOS:0.45.0008
         MainCPLD:17.0.0005
         ActiveBMCRoot:2.0.000E
         BackupBMCRoot:2.0.000E
         BMCBoot:2.0.0001
         LsiFirmware:19.00.00.00
         LsiBios:07.37.00.00
         Battery1Firmware:06.29
         Battery2Firmware:06.29
         DomFirmware:X231600
         CanisterFirmware:3.5.0.32
         CanisterBootloader:5.03
         CanisterConfigCRC:0xD1B030A4
         CanisterVPDStructure:0x06
         CanisterGEMCPLD:0x17
         CanisterVPDCRC:0xEE3504B4
         MidplaneVPDStructure:0x0C
         MidplaneVPDCRC:0xA6BD4F64
         MidplaneCPLD:0x10
         PCM1Firmware:1.00|1.05
         PCM1VPDStructure:0x05
         PCM1VPDCRC:0x41BEF99C
         PCM2Firmware:1.00|1.05
         PCM2VPDStructure:0x05
         PCM2VPDCRC:0x41BEF99C
   
         DisksFirmware
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
   
    <span data-ttu-id="b8789-180">Futtassa a hello `Get-HcsFirmwareVersion` hello második vezérlő tooverify hello szoftverének verziójával, a parancs frissítve lett.</span><span class="sxs-lookup"><span data-stu-id="b8789-180">Run hello `Get-HcsFirmwareVersion` command on hello second controller tooverify that hello software version has been updated.</span></span> <span data-ttu-id="b8789-181">Lépjen ki a karbantartási mód hello majd.</span><span class="sxs-lookup"><span data-stu-id="b8789-181">You can then exit hello maintenance mode.</span></span> <span data-ttu-id="b8789-182">toodo úgy, írja be a következő parancsot minden eszköz vezérlő hello:</span><span class="sxs-lookup"><span data-stu-id="b8789-182">toodo so, type hello following command for each device controller:</span></span>
   
   `Exit-HcsMaintenanceMode`
5. <span data-ttu-id="b8789-183">Hello, tartományvezérlői újraindítása a kilép karbantartási módból.</span><span class="sxs-lookup"><span data-stu-id="b8789-183">hello controllers restart when you exit Maintenance mode.</span></span> <span data-ttu-id="b8789-184">Után hello lemez belső vezérlőprogram frissítése sikeresen telepítve van, és hello eszköz már kilépett a karbantartási mód visszatérési toohello a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="b8789-184">After hello disk firmware updates are successfully applied and hello device has exited maintenance mode, return toohello Azure classic portal.</span></span> <span data-ttu-id="b8789-185">Vegye figyelembe, hogy hello portal előfordulhat, hogy ne jelenjen meg, hogy telepítette-e hello karbantartási mód frissítéseit 24 óra.</span><span class="sxs-lookup"><span data-stu-id="b8789-185">Note that hello portal might not show that you installed hello Maintenance mode updates for 24 hours.</span></span>

