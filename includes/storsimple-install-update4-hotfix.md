<!--author=alkohli last changed: 02/10/17-->

#### <a name="toodownload-hotfixes"></a><span data-ttu-id="3ba65-101">toodownload gyorsjavítások</span><span class="sxs-lookup"><span data-stu-id="3ba65-101">toodownload hotfixes</span></span>

<span data-ttu-id="3ba65-102">Hajtsa végre a következő lépéseket toodownload hello szoftverfrissítést a Microsoft Update katalógus hello hello.</span><span class="sxs-lookup"><span data-stu-id="3ba65-102">Perform hello following steps toodownload hello software update from hello Microsoft Update Catalog.</span></span>

1. <span data-ttu-id="3ba65-103">Indítsa el az Internet Explorert, és keresse meg a túl[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="3ba65-103">Start Internet Explorer and navigate too[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>
2. <span data-ttu-id="3ba65-104">Ha az első alkalommal használja a Microsoft Update katalógus hello ezen a számítógépen, kattintson a **telepítése** amikor felszólító tooinstall hello bővítmény a Microsoft Update katalógusból.</span><span class="sxs-lookup"><span data-stu-id="3ba65-104">If this is your first time using hello Microsoft Update Catalog on this computer, click **Install** when prompted tooinstall hello Microsoft Update Catalog add-on.</span></span>

    ![Katalógus telepítése](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)

3. <span data-ttu-id="3ba65-106">A Microsoft Update katalógus hello hello a keresési mezőbe, írja be a hello Tudásbázis (KB) száma hello gyorsjavítás kívánt toodownload, például **4011839**, és kattintson a **keresési**.</span><span class="sxs-lookup"><span data-stu-id="3ba65-106">In hello search box of hello Microsoft Update Catalog, enter hello Knowledge Base (KB) number of hello hotfix you want toodownload, for example **4011839**, and then click **Search**.</span></span>
   
    <span data-ttu-id="3ba65-107">hello gyorsjavítás lista megjelenik, például **összegző szoftverfrissítési csomagot frissítés 4.0 a StorSimple 8000 Series**.</span><span class="sxs-lookup"><span data-stu-id="3ba65-107">hello hotfix listing appears, for example, **Cumulative Software Bundle Update 4.0 for StorSimple 8000 Series**.</span></span>
   
    ![Keresés a katalógusban](./media/storsimple-install-update2-hotfix/HCS_SearchCatalog1-include.png)

4. <span data-ttu-id="3ba65-109">Kattintson a **Letöltés** gombra.</span><span class="sxs-lookup"><span data-stu-id="3ba65-109">Click **Download**.</span></span> <span data-ttu-id="3ba65-110">Adja meg vagy **Tallózás** tooa helyi helyének hello tooappear tölti le.</span><span class="sxs-lookup"><span data-stu-id="3ba65-110">Specify or **Browse** tooa local location where you want hello downloads tooappear.</span></span> <span data-ttu-id="3ba65-111">Kattintson a hello fájlok toodownload toohello megadott helyre és mappára.</span><span class="sxs-lookup"><span data-stu-id="3ba65-111">Click hello files toodownload toohello specified location and folder.</span></span> <span data-ttu-id="3ba65-112">hello mappába másolt tooa hálózati megosztásra, amely elérhető hello eszköz is lehet.</span><span class="sxs-lookup"><span data-stu-id="3ba65-112">hello folder can also be copied tooa network share that is reachable from hello device.</span></span>
5. <span data-ttu-id="3ba65-113">Keressen további gyorsjavítások a fenti hello táblázatban felsorolt (**4011841**), és a letöltési hello megfelelő fájlok toohello meghatározott mappákról, a tábla megelőző hello.</span><span class="sxs-lookup"><span data-stu-id="3ba65-113">Search for any additional hotfixes listed in hello table above (**4011841**), and download hello corresponding files toohello specific folders as listed in hello preceding table.</span></span>

> [!NOTE]
> <span data-ttu-id="3ba65-114">hello gyorsjavítások mindkét hello társ vezérlő érkező üzenetek ezt a hibát a tartományvezérlők toodetect elérhetőknek kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="3ba65-114">hello hotfixes must be accessible from both controllers toodetect any potential error messages from hello peer controller.</span></span>
>
> <span data-ttu-id="3ba65-115">hello gyorsjavítások 3 külön mappákban kell másolni.</span><span class="sxs-lookup"><span data-stu-id="3ba65-115">hello hotfixes must be copied in 3 separate folders.</span></span> <span data-ttu-id="3ba65-116">Például hello eszköz szoftver/Cis/MDS ügynök frissítése a másolhatók _FirstOrderUpdate_ mappa összes hello más nem zavaró frissítések sikerült átmásolni a hello _SecondOrderUpdate_ mappa, és karbantartási mód frissítések átmásolja a _ThirdOrderUpdate_ mappa.</span><span class="sxs-lookup"><span data-stu-id="3ba65-116">For example, hello device software/Cis/MDS agent update can be copied in _FirstOrderUpdate_ folder, all hello other non-disruptive updates could be copied in hello _SecondOrderUpdate_ folder, and maintenance mode updates copied in _ThirdOrderUpdate_ folder.</span></span>

#### <a name="tooinstall-and-verify-regular-mode-hotfixes"></a><span data-ttu-id="3ba65-117">tooinstall és normál mód gyorsjavítások ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="3ba65-117">tooinstall and verify regular mode hotfixes</span></span>

<span data-ttu-id="3ba65-118">Hajtsa végre a következő lépéseket tooinstall hello, és ellenőrizze a normál módú gyorsjavítások.</span><span class="sxs-lookup"><span data-stu-id="3ba65-118">Perform hello following steps tooinstall and verify regular-mode hotfixes.</span></span> <span data-ttu-id="3ba65-119">Ha már telepítette hello segítségével a klasszikus Azure portálon, hagyja ki azokat, amelyek túl[telepítse, és ellenőrizze a karbantartási mód gyorsjavítások](#to-install-and-verify-maintenance-mode-hotfixes).</span><span class="sxs-lookup"><span data-stu-id="3ba65-119">If you already installed them using hello Azure classic portal, skip ahead too[install and verify maintenance mode hotfixes](#to-install-and-verify-maintenance-mode-hotfixes).</span></span>

1. <span data-ttu-id="3ba65-120">tooinstall hello gyorsjavítások, hozzáférési hello Windows PowerShell felületet a StorSimple eszköz soros konzoljához a.</span><span class="sxs-lookup"><span data-stu-id="3ba65-120">tooinstall hello hotfixes, access hello Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="3ba65-121">Hajtsa végre a hello részletes utasításait [PuTTy használata tooconnect toohello soros konzol](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="3ba65-121">Follow hello detailed instructions in [Use PuTTy tooconnect toohello serial console](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span> <span data-ttu-id="3ba65-122">Hello parancssorban nyomja le az **Enter**.</span><span class="sxs-lookup"><span data-stu-id="3ba65-122">At hello command prompt, press **Enter**.</span></span>
2. <span data-ttu-id="3ba65-123">Válassza ki **1. lehetőség** toolog teljes hozzáféréssel rendelkező toohello eszközön.</span><span class="sxs-lookup"><span data-stu-id="3ba65-123">Select **Option 1** toolog on toohello device with full access.</span></span> <span data-ttu-id="3ba65-124">Azt javasoljuk, hogy hello gyorsjavítás hello passzív tartományvezérlőre telepíti először.</span><span class="sxs-lookup"><span data-stu-id="3ba65-124">We recommend that you install hello hotfix on hello passive controller first.</span></span>
3. <span data-ttu-id="3ba65-125">tooinstall hello gyorsjavítás, parancssorba hello típusa:</span><span class="sxs-lookup"><span data-stu-id="3ba65-125">tooinstall hello hotfix, at hello command prompt, type:</span></span>
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="3ba65-126">DNS helyett IP használata a megosztás elérési útja, a fenti parancs hello.</span><span class="sxs-lookup"><span data-stu-id="3ba65-126">Use IP rather than DNS in share path in hello above command.</span></span> <span data-ttu-id="3ba65-127">hello credential paraméter csak akkor, ha egy hitelesített megosztás elérésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="3ba65-127">hello credential parameter is used only if you are accessing an authenticated share.</span></span>
   
    <span data-ttu-id="3ba65-128">Azt javasoljuk, hogy használja-e hello credential paraméter tooaccess megosztások.</span><span class="sxs-lookup"><span data-stu-id="3ba65-128">We recommend that you use hello credential parameter tooaccess shares.</span></span> <span data-ttu-id="3ba65-129">Még akkor is, megosztások, megnyitott túl "mindenki" jellemzően a toounauthenticated felhasználók nem nyitható meg.</span><span class="sxs-lookup"><span data-stu-id="3ba65-129">Even shares that are open too“everyone” are typically not open toounauthenticated users.</span></span>
   
    <span data-ttu-id="3ba65-130">Adja meg a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="3ba65-130">Supply hello password when prompted.</span></span>
   
    <span data-ttu-id="3ba65-131">Az alábbiakban látható egy minta kimenet hello első rendelés frissítések telepítése.</span><span class="sxs-lookup"><span data-stu-id="3ba65-131">A sample output for installing hello first order updates is shown below.</span></span> <span data-ttu-id="3ba65-132">Hello első rendelés frissítés toopoint toohello adott fájl szükséges.</span><span class="sxs-lookup"><span data-stu-id="3ba65-132">For hello first order update, you need toopoint toohello specific file.</span></span>
   
        ````
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \FirstOrderUpdate\HcsSoftwareUpdate.exe -Credential contoso\John
   
        Confirm
   
        This operation starts hello hotfix installation and could reboot one or
        both of hello controllers. If hello device is serving I/Os, these will not
        be disrupted. Are you sure you want toocontinue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y
   
        ````
4. <span data-ttu-id="3ba65-133">Típus **Y** amikor felszólító tooconfirm hello gyorsjavítás telepítése.</span><span class="sxs-lookup"><span data-stu-id="3ba65-133">Type **Y** when prompted tooconfirm hello hotfix installation.</span></span>
5. <span data-ttu-id="3ba65-134">Figyelje az hello frissítés hello `Get-HcsUpdateStatus` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="3ba65-134">Monitor hello update by using hello `Get-HcsUpdateStatus` cmdlet.</span></span> <span data-ttu-id="3ba65-135">hello frissítés először befejezi az hello passzív történik.</span><span class="sxs-lookup"><span data-stu-id="3ba65-135">hello update will first complete on hello passive controller.</span></span> <span data-ttu-id="3ba65-136">Miután hello passzív vezérlő frissül, a feladatátvétel, és hello frissítés majd lesznek érvényben lévő hello más vezérlő.</span><span class="sxs-lookup"><span data-stu-id="3ba65-136">Once hello passive controller is updated, there will be a failover and hello update will then get applied on hello other controller.</span></span> <span data-ttu-id="3ba65-137">hello frissítés befejeződött, mindkét hello tartományvezérlők frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="3ba65-137">hello update is complete when both hello controllers are updated.</span></span>
   
    <span data-ttu-id="3ba65-138">hello következő minta kimenet látható hello frissítés folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="3ba65-138">hello following sample output shows hello update in progress.</span></span> <span data-ttu-id="3ba65-139">Hello `RunInprogress` lesz `True` amikor hello frissítése folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="3ba65-139">hello `RunInprogress` will be `True` when hello update is in progress.</span></span>

    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 02/03/2017 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     <span data-ttu-id="3ba65-140">a következő minta kimenet hello azt jelzi, hogy adott hello frissítése befejeződött.</span><span class="sxs-lookup"><span data-stu-id="3ba65-140">hello following sample output indicates that hello update is finished.</span></span> <span data-ttu-id="3ba65-141">Hello `RunInProgress` lesz `False` amikor hello frissítése befejeződött.</span><span class="sxs-lookup"><span data-stu-id="3ba65-141">hello `RunInProgress` will be `False` when hello update has completed.</span></span>
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 02/03/2017 9:15:55 AM
    LastUpdateTimestamp : 02/03/2017 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE]
    > <span data-ttu-id="3ba65-142">Alkalmanként hello parancsmag jelentések `False` amikor hello frissítése még folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="3ba65-142">Occasionally, hello cmdlet reports `False` when hello update is still in progress.</span></span> <span data-ttu-id="3ba65-143">amely gyorsjavítás hello tooensure befejeződött, várjon néhány percet, futtassa újra a parancsot, és győződjön meg arról, hogy hello `RunInProgress` van `False`.</span><span class="sxs-lookup"><span data-stu-id="3ba65-143">tooensure that hello hotfix is complete, wait for a few minutes, rerun this command and verify that hello `RunInProgress` is `False`.</span></span> <span data-ttu-id="3ba65-144">Ha igen, hello gyorsjavítás befejeződött.</span><span class="sxs-lookup"><span data-stu-id="3ba65-144">If it is, then hello hotfix has completed.</span></span>

6. <span data-ttu-id="3ba65-145">Hello szoftverfrissítés befejeződése után ellenőrizze a hello rendszer szoftververziók.</span><span class="sxs-lookup"><span data-stu-id="3ba65-145">After hello software update is complete, verify hello system software versions.</span></span> <span data-ttu-id="3ba65-146">Típus:</span><span class="sxs-lookup"><span data-stu-id="3ba65-146">Type:</span></span>
   
    `Get-HcsSystem`
   
    <span data-ttu-id="3ba65-147">A következő verziók hello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="3ba65-147">You should see hello following versions:</span></span>
   
   * `FriendlySoftwareVersion: StorSimple 8000 Series Update 4.0`
   *  `HcsSoftwareVersion: 6.3.9600.17820`
   
    <span data-ttu-id="3ba65-148">Hello verziószáma nem változik hello frissítés telepítését követően, azt jelzi, hello gyorsjavítás tooapply sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="3ba65-148">If hello version number does not change after applying hello update, it indicates that hello hotfix has failed tooapply.</span></span> <span data-ttu-id="3ba65-149">Ha ezt látja, további segítségért forduljon a [Microsoft támogatási szolgálatához](../articles/storsimple/storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="3ba65-149">Should you see this, please contact [Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) for further assistance.</span></span>
     
    > [!IMPORTANT]
    > <span data-ttu-id="3ba65-150">Újra kell indítania a hello aktív vezérlő keresztül hello `Restart-HcsController` parancsmag alkalmazása előtt a következő frissítés hello.</span><span class="sxs-lookup"><span data-stu-id="3ba65-150">You must restart hello active controller via hello `Restart-HcsController` cmdlet before applying hello next update.</span></span>
     
7. <span data-ttu-id="3ba65-151">Ismételje meg a 3-5 tooinstall hello Cis/MDS letöltött ügynök tooyour _FirstOrderUpdate_ mappa.</span><span class="sxs-lookup"><span data-stu-id="3ba65-151">Repeat steps 3-5 tooinstall hello Cis/MDS agent downloaded tooyour _FirstOrderUpdate_ folder.</span></span> 
8. <span data-ttu-id="3ba65-152">Ismételje meg a 3-5. lépéseket tooinstall hello második rendelés frissítések.</span><span class="sxs-lookup"><span data-stu-id="3ba65-152">Repeat steps 3-5 tooinstall hello second order updates.</span></span> <span data-ttu-id="3ba65-153">**Második rendelés frissítések több frissítést is telepíthető hello csak futtatásával `Start-HcsHotfix cmdlet` és mutató toohello tartalmazó mappát második rendelés frissítések. hello parancsmag végrehajtja a rendelkezésre álló hello frissítések hello mappában.**</span><span class="sxs-lookup"><span data-stu-id="3ba65-153">**For second order updates, multiple updates can be installed by just running hello `Start-HcsHotfix cmdlet` and pointing toohello folder where second order updates are located. hello cmdlet will execute all hello updates available in hello folder.**</span></span> <span data-ttu-id="3ba65-154">Ha egy frissítés már telepítve van, hello frissítés logika, amely észleli, és, hogy a frissítés nem alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="3ba65-154">If an update is already installed, hello update logic will detect that and not apply that update.</span></span> 

<span data-ttu-id="3ba65-155">Miután minden hello gyorsjavítás telepítve van, a hello `Get-HcsSystem` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="3ba65-155">After all hello hotfixes are installed, use hello `Get-HcsSystem` cmdlet.</span></span> <span data-ttu-id="3ba65-156">hello verziók le kell:</span><span class="sxs-lookup"><span data-stu-id="3ba65-156">hello versions should be:</span></span>

   * `CisAgentVersion:  1.0.9441.0`
   * `MdsAgentVersion: 35.2.2.0`
   * `Lsisas2Version: 2.0.78.00`


#### <a name="tooinstall-and-verify-maintenance-mode-hotfixes"></a><span data-ttu-id="3ba65-157">tooinstall és karbantartási mód gyorsjavítások ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="3ba65-157">tooinstall and verify maintenance mode hotfixes</span></span>
<span data-ttu-id="3ba65-158">Használja a KB4011837 tooinstall lemez belső vezérlőprogram-frissítésekre.</span><span class="sxs-lookup"><span data-stu-id="3ba65-158">Use KB4011837 tooinstall disk firmware updates.</span></span> <span data-ttu-id="3ba65-159">Ezek zavaró frissítések, és körülbelül 30 percet toocomplete igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="3ba65-159">These are disruptive updates and take around 30 minutes toocomplete.</span></span> <span data-ttu-id="3ba65-160">Szerint is választhat tooinstall ezeket a tervezett karbantartások az összekötő toohello eszköz soros konzoljához.</span><span class="sxs-lookup"><span data-stu-id="3ba65-160">You can choose tooinstall these in a planned maintenance window by connecting toohello device serial console.</span></span>

<span data-ttu-id="3ba65-161">Vegye figyelembe, ha a lemez belső vezérlőprogram már naprakész, akkor nem kell tooinstall ezeket a frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="3ba65-161">Note that if your disk firmware is already up-to-date, you won't need tooinstall these updates.</span></span> <span data-ttu-id="3ba65-162">Futtassa a hello `Get-HcsUpdateAvailability` hello eszköz soros konzoljához toocheck, ha frissítések érhetők el, és hogy hello parancsmag frissíti biztosan zavaró (karbantartás mód) vagy nem zavaró (normál mód) frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="3ba65-162">Run hello `Get-HcsUpdateAvailability` cmdlet from hello device serial console toocheck if updates are available and whether hello updates are disruptive (maintenance mode) or non-disruptive (regular mode) updates.</span></span>

<span data-ttu-id="3ba65-163">tooinstall hello lemez belső vezérlőprogram-frissítésekre, kövesse az alábbi hello utasításokat.</span><span class="sxs-lookup"><span data-stu-id="3ba65-163">tooinstall hello disk firmware updates, follow hello instructions below.</span></span>

1. <span data-ttu-id="3ba65-164">Állítsa hello eszköz hello karbantartási módba.</span><span class="sxs-lookup"><span data-stu-id="3ba65-164">Place hello device in hello maintenance mode.</span></span> <span data-ttu-id="3ba65-165">**Vegye figyelembe, hogy ne használjon Windows PowerShell távoli eljáráshívás tooa eszköz karbantartási módba kapcsolódáskor. Ehelyett futtassa ezt a parancsmagot hello eszköz tartományvezérlőn hello eszköz soros konzolon keresztül csatlakozáskor.**</span><span class="sxs-lookup"><span data-stu-id="3ba65-165">**Note that you should not use Windows PowerShell remoting when connecting tooa device in maintenance mode. Instead run this cmdlet on hello device controller when connected through hello device serial console.**</span></span> <span data-ttu-id="3ba65-166">Típus:</span><span class="sxs-lookup"><span data-stu-id="3ba65-166">Type:</span></span>
   
    `Enter-HcsMaintenanceMode`
   
    <span data-ttu-id="3ba65-167">Az alábbiakban egy példa látható a kimenetre.</span><span class="sxs-lookup"><span data-stu-id="3ba65-167">A sample output is shown below.</span></span>
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from hello Microsoft Azure StorSimple Manager service. Entering maintenance mode will end hello current session and reboot both controllers, which takes a few minutes toocomplete. Are you sure you want tooenter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8600
        Name: Update4-8600-mystorsimple
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected tooController0 - Passive
        ---------------------------------------------------------------
   
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    <span data-ttu-id="3ba65-168">Mindkét hello tartományvezérlők majd indítsa újra a számítógépet karbantartási módba.</span><span class="sxs-lookup"><span data-stu-id="3ba65-168">Both hello controllers then restart into maintenance mode.</span></span>
2. <span data-ttu-id="3ba65-169">tooinstall hello lemez belső vezérlőprogram frissítése, típus:</span><span class="sxs-lookup"><span data-stu-id="3ba65-169">tooinstall hello disk firmware update, type:</span></span>
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="3ba65-170">Az alábbiakban egy példa látható a kimenetre.</span><span class="sxs-lookup"><span data-stu-id="3ba65-170">A sample output is shown below.</span></span>
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\ThirdOrderUpdates\ -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After hello hotfix is installed on this controller, install it on hello peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of hello controllers. By installing new updates you agree to, and accept any additional terms associated with, hello new functionality listed in hello release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want toocontinue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes toocomplete.
3. <span data-ttu-id="3ba65-171">A figyelő hello telepítési folyamat használatával `Get-HcsUpdateStatus` parancsot.</span><span class="sxs-lookup"><span data-stu-id="3ba65-171">Monitor hello install progress using `Get-HcsUpdateStatus` command.</span></span> <span data-ttu-id="3ba65-172">hello frissítés befejeződött amikor hello `RunInProgress` túl változik`False`.</span><span class="sxs-lookup"><span data-stu-id="3ba65-172">hello update is complete when hello `RunInProgress` changes too`False`.</span></span>
4. <span data-ttu-id="3ba65-173">Hello telepítés befejezése után újraindítja a hello vezérlő melyik hello karbantartási mód gyorsjavítás telepítve lett.</span><span class="sxs-lookup"><span data-stu-id="3ba65-173">After hello installation is complete, hello controller on which hello maintenance mode hotfix was installed restarts.</span></span> <span data-ttu-id="3ba65-174">Jelentkezzen be 1. lehetőség teljes hozzáféréssel, és hello lemez belső vezérlőprogram verziójának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="3ba65-174">Log in as option 1 with full access and verify hello disk firmware version.</span></span> <span data-ttu-id="3ba65-175">Típus:</span><span class="sxs-lookup"><span data-stu-id="3ba65-175">Type:</span></span>
   
   `Get-HcsFirmwareVersion`
   
   <span data-ttu-id="3ba65-176">hello várt, belsővezérlőprogram-verziók a következők:</span><span class="sxs-lookup"><span data-stu-id="3ba65-176">hello expected disk firmware versions are:</span></span>
   
   `XMGJ, XGEG, KZ50, F6C2, VR08, N002, 0106`
   
   <span data-ttu-id="3ba65-177">Az alábbiakban egy példa látható a kimenetre.</span><span class="sxs-lookup"><span data-stu-id="3ba65-177">A sample output is shown below.</span></span>
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8600
       Name: Update4-8600-mystorsimple
       Software Version: 6.3.9600.17820
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected tooController1
       ---------------------------------------------------------------
   
       Controller1>Get-HcsFirmwareVersion
   
       Controller0 : TalladegaFirmware
           ActiveBIOS:0.45.0010
              BackupBIOS:0.45.0006
              MainCPLD:17.0.000b
              ActiveBMCRoot:2.0.001F
              BackupBMCRoot:2.0.001F
              BMCBoot:2.0.0002
              LsiFirmware:20.00.04.00
              LsiBios:07.37.00.00
              Battery1Firmware:06.2C
              Battery2Firmware:06.2C
              DomFirmware:X231600
              CanisterFirmware:3.5.0.56
              CanisterBootloader:5.03
              CanisterConfigCRC:0x9134777A
              CanisterVPDStructure:0x06
              CanisterGEMCPLD:0x19
              CanisterVPDCRC:0x142F7DC2
              MidplaneVPDStructure:0x0C
              MidplaneVPDCRC:0xA6BD4F64
              MidplaneCPLD:0x10
              PCM1Firmware:1.00|1.05
              PCM1VPDStructure:0x05
              PCM1VPDCRC:0x41BEF99C
              PCM2Firmware:1.00|1.05
              PCM2VPDStructure:0x05
              PCM2VPDCRC:0x41BEF99C

           EbodFirmware
              CanisterFirmware:3.5.0.56
              CanisterBootloader:5.03
              CanisterConfigCRC:0xB23150F8
              CanisterVPDStructure:0x06
              CanisterGEMCPLD:0x14
              CanisterVPDCRC:0xBAA55828
              MidplaneVPDStructure:0x0C
              MidplaneVPDCRC:0xA6BD4F64
              MidplaneCPLD:0x10
              PCM1Firmware:3.11
              PCM1VPDStructure:0x03
              PCM1VPDCRC:0x6B58AD13
              PCM2Firmware:3.11
              PCM2VPDStructure:0x03
              PCM2VPDCRC:0x6B58AD13

           DisksFirmware
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
   
    <span data-ttu-id="3ba65-178">Futtassa a hello `Get-HcsFirmwareVersion` hello második vezérlő tooverify hello szoftverének verziójával, a parancs frissítve lett.</span><span class="sxs-lookup"><span data-stu-id="3ba65-178">Run hello `Get-HcsFirmwareVersion` command on hello second controller tooverify that hello software version has been updated.</span></span> <span data-ttu-id="3ba65-179">Lépjen ki a karbantartási mód hello majd.</span><span class="sxs-lookup"><span data-stu-id="3ba65-179">You can then exit hello maintenance mode.</span></span> <span data-ttu-id="3ba65-180">toodo úgy, írja be a következő parancsot minden eszköz vezérlő hello:</span><span class="sxs-lookup"><span data-stu-id="3ba65-180">toodo so, type hello following command for each device controller:</span></span>
   
   `Exit-HcsMaintenanceMode`

5. <span data-ttu-id="3ba65-181">Hello, tartományvezérlői újraindítása a kilép karbantartási módból.</span><span class="sxs-lookup"><span data-stu-id="3ba65-181">hello controllers restart when you exit maintenance mode.</span></span> <span data-ttu-id="3ba65-182">Után hello lemez belső vezérlőprogram frissítése sikeresen telepítve van, és hello eszköz már kilépett a karbantartási mód visszatérési toohello a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="3ba65-182">After hello disk firmware updates are successfully applied and hello device has exited maintenance mode, return toohello Azure classic portal.</span></span> <span data-ttu-id="3ba65-183">Vegye figyelembe, hogy hello portal előfordulhat, hogy ne jelenjen meg, hogy telepítette-e hello karbantartási mód frissítéseit 24 óra.</span><span class="sxs-lookup"><span data-stu-id="3ba65-183">Note that hello portal might not show that you installed hello maintenance mode updates for 24 hours.</span></span>

