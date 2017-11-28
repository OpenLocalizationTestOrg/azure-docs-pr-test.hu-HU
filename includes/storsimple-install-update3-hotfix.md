<!--author=alkohli last changed: 09/01/16-->

#### <a name="to-download-hotfixes"></a><span data-ttu-id="25c04-101">Gyorsjavítások letöltése</span><span class="sxs-lookup"><span data-stu-id="25c04-101">To download hotfixes</span></span>
<span data-ttu-id="25c04-102">Hajtsa végre a következő lépéseket a szoftverfrissítés a Microsoft Update katalógusból történő letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="25c04-102">Perform the following steps to download the software update from the Microsoft Update Catalog.</span></span>

1. <span data-ttu-id="25c04-103">Indítsa el az Internet Explorert, és keresse fel a [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com) címet.</span><span class="sxs-lookup"><span data-stu-id="25c04-103">Start Internet Explorer and navigate to [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>
2. <span data-ttu-id="25c04-104">Ha most használja először a Microsoft Update katalógust ezen a számítógépen, kattintson a **Telepítés** gombra, amikor a rendszer a Microsoft Update katalógus beépülő moduljának telepítésére kéri.</span><span class="sxs-lookup"><span data-stu-id="25c04-104">If this is your first time using the Microsoft Update Catalog on this computer, click **Install** when prompted to install the Microsoft Update Catalog add-on.</span></span>
    <span data-ttu-id="25c04-105">![Katalógus telepítése](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)</span><span class="sxs-lookup"><span data-stu-id="25c04-105">![Install catalog](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)</span></span>
3. <span data-ttu-id="25c04-106">A keresési mezőbe, a Microsoft Update katalógus, adja meg a Tudásbázis (KB) le szeretné tölteni, például a gyorsjavítás **3186843**, és kattintson a **keresési**.</span><span class="sxs-lookup"><span data-stu-id="25c04-106">In the search box of the Microsoft Update Catalog, enter the Knowledge Base (KB) number of the hotfix you want to download, for example **3186843**, and then click **Search**.</span></span>
   
    <span data-ttu-id="25c04-107">A gyorsjavítás-lista megjelenik, például **összegző szoftverfrissítési csomagot frissítés 3.0 a StorSimple 8000 Series**.</span><span class="sxs-lookup"><span data-stu-id="25c04-107">The hotfix listing appears, for example, **Cumulative Software Bundle Update 3.0 for StorSimple 8000 Series**.</span></span>
   
    ![Keresés a katalógusban](./media/storsimple-install-update2-hotfix/HCS_SearchCatalog1-include.png)
4. <span data-ttu-id="25c04-109">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="25c04-109">Click **Add**.</span></span> <span data-ttu-id="25c04-110">Ezzel a frissítést hozzáadja a kosárhoz.</span><span class="sxs-lookup"><span data-stu-id="25c04-110">The update is added to the basket.</span></span>
5. <span data-ttu-id="25c04-111">A fenti táblázatban szereplő további gyorsjavítások keresése (**3186859**), és adja hozzá a kosárhoz mindegyik.</span><span class="sxs-lookup"><span data-stu-id="25c04-111">Search for any additional hotfixes listed in the table above (**3186859**), and add each to the basket.</span></span>
6. <span data-ttu-id="25c04-112">Kattintson a **Kosár megtekintése** gombra.</span><span class="sxs-lookup"><span data-stu-id="25c04-112">Click **View Basket**.</span></span>
7. <span data-ttu-id="25c04-113">Kattintson a **Letöltés** gombra.</span><span class="sxs-lookup"><span data-stu-id="25c04-113">Click **Download**.</span></span> <span data-ttu-id="25c04-114">Adja meg vagy **tallózással** válassza ki a helyet, ahová a fájlokat le szeretné tölteni.</span><span class="sxs-lookup"><span data-stu-id="25c04-114">Specify or **Browse** to a local location where you want the downloads to appear.</span></span> <span data-ttu-id="25c04-115">A szoftverfrissítések letöltése a megadott helyre és a neve megegyezik a frissítési alárendelt mappába helyezi.</span><span class="sxs-lookup"><span data-stu-id="25c04-115">The updates are downloaded to the specified location and placed in a sub-folder with the same name as the update.</span></span> <span data-ttu-id="25c04-116">A mappa átmásolható egy, az eszközről elérhető hálózati megosztásra is.</span><span class="sxs-lookup"><span data-stu-id="25c04-116">The folder can also be copied to a network share that is reachable from the device.</span></span>

> [!NOTE]
> <span data-ttu-id="25c04-117">A gyorsjavítások mindkét vezérlők annak a partner tartományvezérlőről esetleges hibaüzeneteket észlelésére elérhetőknek kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="25c04-117">The hotfixes must be accessible from both controllers to detect any potential error messages from the peer controller.</span></span>
> 
> 

#### <a name="to-install-and-verify-regular-mode-hotfixes"></a><span data-ttu-id="25c04-118">Normál módú gyorsjavítások telepítése és ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="25c04-118">To install and verify regular mode hotfixes</span></span>
<span data-ttu-id="25c04-119">A normál módú gyorsjavítások telepítéséhez és ellenőrzéséhez hajtsa végre az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="25c04-119">Perform the following steps to install and verify regular-mode hotfixes.</span></span> <span data-ttu-id="25c04-120">Ha már telepítette azokat az Azure portál használatával, ugorjon előre [telepítse, és ellenőrizze a karbantartási mód gyorsjavítások](#to-install-and-verify-maintenance-mode-hotfixes).</span><span class="sxs-lookup"><span data-stu-id="25c04-120">If you already installed them using the Azure Portal, skip ahead to [install and verify maintenance mode hotfixes](#to-install-and-verify-maintenance-mode-hotfixes).</span></span>

1. <span data-ttu-id="25c04-121">A gyorsjavítások telepítéséhez nyissa meg a Windows PowerShell felületét a StorSimple-eszköz soros konzoljában.</span><span class="sxs-lookup"><span data-stu-id="25c04-121">To install the hotfixes, access the Windows PowerShell interface on your StorSimple device serial console.</span></span> <span data-ttu-id="25c04-122">Kövesse [a PuTTY a soros konzolhoz való csatlakozáshoz történő használatát](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console) ismertető részletes útmutatásokat.</span><span class="sxs-lookup"><span data-stu-id="25c04-122">Follow the detailed instructions in [Use PuTTy to connect to the serial console](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span> <span data-ttu-id="25c04-123">A parancssorban nyomja le az **Enter** billentyűt.</span><span class="sxs-lookup"><span data-stu-id="25c04-123">At the command prompt, press **Enter**.</span></span>
2. <span data-ttu-id="25c04-124">Válassza az **1. lehetőséget** a teljes körű hozzáféréssel való bejelentkezéshez az eszközbe.</span><span class="sxs-lookup"><span data-stu-id="25c04-124">Select **Option 1** to log on to the device with full access.</span></span> <span data-ttu-id="25c04-125">Javasoljuk, hogy előbb a passzív vezérlőre telepítse a gyorsjavítást.</span><span class="sxs-lookup"><span data-stu-id="25c04-125">We recommend that you install the hotfix on the passive controller first.</span></span>
3. <span data-ttu-id="25c04-126">A gyorsjavítás telepítéséhez írja be a következőt a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="25c04-126">To install the hotfix, at the command prompt, type:</span></span>
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="25c04-127">A fenti parancsban a megosztás elérési útjában DNS-cím helyett inkább IP-címet adjon meg.</span><span class="sxs-lookup"><span data-stu-id="25c04-127">Use IP rather than DNS in share path in the above command.</span></span> <span data-ttu-id="25c04-128">A hitelesítési paramétert csak akkor kell használni, ha egy hitelesített megosztáshoz kíván hozzáférni.</span><span class="sxs-lookup"><span data-stu-id="25c04-128">The credential parameter is used only if you are accessing an authenticated share.</span></span>
   
    <span data-ttu-id="25c04-129">Javasoljuk, hogy a megosztások elérése során használja a hitelesítési paramétert.</span><span class="sxs-lookup"><span data-stu-id="25c04-129">We recommend that you use the credential parameter to access shares.</span></span> <span data-ttu-id="25c04-130">Még a „mindenki” számára elérhető megosztások sem elérhetőek a nem hitelesített felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="25c04-130">Even shares that are open to “everyone” are typically not open to unauthenticated users.</span></span>
   
    <span data-ttu-id="25c04-131">Amikor a rendszer kéri, adja meg a jelszót.</span><span class="sxs-lookup"><span data-stu-id="25c04-131">Supply the password when prompted.</span></span>
   
    <span data-ttu-id="25c04-132">Az alábbiakban egy példa látható a kimenetre.</span><span class="sxs-lookup"><span data-stu-id="25c04-132">A sample output is shown below.</span></span>
   
        ````
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \hcsmdssoftwareupdate.exe -Credential contoso\John
   
        Confirm
   
        This operation starts the hotfix installation and could reboot one or
        both of the controllers. If the device is serving I/Os, these will not
        be disrupted. Are you sure you want to continue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y
   
        ````
4. <span data-ttu-id="25c04-133">Írja be az **Y** karaktert, amikor a rendszer kéri, hogy erősítse meg a gyorsjavítás telepítését.</span><span class="sxs-lookup"><span data-stu-id="25c04-133">Type **Y** when prompted to confirm the hotfix installation.</span></span>
5. <span data-ttu-id="25c04-134">A frissítést a `Get-HcsUpdateStatus` parancsmag használatával figyelheti.</span><span class="sxs-lookup"><span data-stu-id="25c04-134">Monitor the update by using the `Get-HcsUpdateStatus` cmdlet.</span></span> <span data-ttu-id="25c04-135">A frissítés előbb a passzív vezérlőn lesz végrehajtva.</span><span class="sxs-lookup"><span data-stu-id="25c04-135">The update will first complete on the passive controller.</span></span> <span data-ttu-id="25c04-136">Miután a passzív vezérlő frissítve lett, az azt követő feladatátvétel során a frissítés a másik vezérlőre is alkalmazva lesz.</span><span class="sxs-lookup"><span data-stu-id="25c04-136">Once the passive controller is updated, there will be a failover and the update will then get applied on the other controller.</span></span> <span data-ttu-id="25c04-137">A frissítés akkor fejeződött be, ha mindkét vezérlő frissítve lett.</span><span class="sxs-lookup"><span data-stu-id="25c04-137">The update is complete when both the controllers are updated.</span></span>
   
    <span data-ttu-id="25c04-138">Az alábbi kimeneti példa azt mutatja, hogy a frissítés még folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="25c04-138">The following sample output shows the update in progress.</span></span> <span data-ttu-id="25c04-139">A `RunInprogress` értéke `True`, ha a frissítés folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="25c04-139">The `RunInprogress` will be `True` when the update is in progress.</span></span>

    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 8/29/2016 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     <span data-ttu-id="25c04-140">Az alábbi kimeneti példa azt mutatja, hogy a frissítés befejeződött.</span><span class="sxs-lookup"><span data-stu-id="25c04-140">The following sample output indicates that the update is finished.</span></span> <span data-ttu-id="25c04-141">A `RunInProgress` értéke `False`, ha a frissítés befejeződött.</span><span class="sxs-lookup"><span data-stu-id="25c04-141">The `RunInProgress` will be `False` when the update has completed.</span></span>
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 8/30/2016 9:15:55 AM
    LastUpdateTimestamp : 8/30/2016 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE] 
    > <span data-ttu-id="25c04-142">Alkalmanként a parancsmag `False` értéket jelenít meg, amikor a frissítés még folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="25c04-142">Occasionally, the cmdlet reports `False` when the update is still in progress.</span></span> <span data-ttu-id="25c04-143">A gyorsjavítás befejezéséhez várjon néhány percet, majd futtassa újra a parancsot, és ellenőrizze, hogy a `RunInProgress` értéke `False`.</span><span class="sxs-lookup"><span data-stu-id="25c04-143">To ensure that the hotfix is complete, wait for a few minutes, rerun this command and verify that the `RunInProgress` is `False`.</span></span> <span data-ttu-id="25c04-144">Ha így van, a gyorsjavítás befejeződött.</span><span class="sxs-lookup"><span data-stu-id="25c04-144">If it is, then the hotfix has completed.</span></span>

1. <span data-ttu-id="25c04-145">A szoftverfrissítés befejeztét követően ellenőrizze a rendszerszoftverek verzióját.</span><span class="sxs-lookup"><span data-stu-id="25c04-145">After the software update is complete, verify the system software versions.</span></span> <span data-ttu-id="25c04-146">Típus:</span><span class="sxs-lookup"><span data-stu-id="25c04-146">Type:</span></span>
   
    `Get-HcsSystem`
   
    <span data-ttu-id="25c04-147">A következő verziónak kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="25c04-147">You should see the following versions:</span></span>
   
   * `HcsSoftwareVersion: 6.3.9600.17759`
   * `CisAgentVersion:  1.0.9343.0`
   * `MdsAgentVersion: 30.0.4698.16`
     
     <span data-ttu-id="25c04-148">A frissítés telepítését követően ne módosítsa a verziószámok, azt jelzi, hogy a gyorsjavítás alkalmazása sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="25c04-148">If the version numbers do not change after applying the update, it indicates that the hotfix has failed to apply.</span></span> <span data-ttu-id="25c04-149">Ha ezt látja, további segítségért forduljon a [Microsoft támogatási szolgálatához](../articles/storsimple/storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="25c04-149">Should you see this, please contact [Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) for further assistance.</span></span>
     
     > [!IMPORTANT]
     > <span data-ttu-id="25c04-150">A további frissítések végrehajtása előtt újra kell indítania az aktív vezérlőt a `Restart-HcsController` parancsmaggal.</span><span class="sxs-lookup"><span data-stu-id="25c04-150">You must restart the active controller via the `Restart-HcsController` cmdlet before applying the remaining updates.</span></span> 
     > 
     > 
2. <span data-ttu-id="25c04-151">Ismételje meg a 3-5 LSI illesztőprogram és a belső vezérlőprogram gyorsjavítás telepítése **KB3186859**.</span><span class="sxs-lookup"><span data-stu-id="25c04-151">Repeat steps 3-5 to install the LSI driver and firmware hotfix **KB3186859**.</span></span> <span data-ttu-id="25c04-152">A gyorsjavítás telepítését követően a `Get-HcsSystem` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="25c04-152">After the hotfix is installed, use the `Get-HcsSystem` cmdlet.</span></span> <span data-ttu-id="25c04-153">A LSI verziónak kell lennie:</span><span class="sxs-lookup"><span data-stu-id="25c04-153">The LSI version should be:</span></span>
   
   * `Lsisas2Version: 2.0.78.00`
3. <span data-ttu-id="25c04-154">Ismételje meg a 3-5 a Storport és Spaceport frissítés **KB3121261**.</span><span class="sxs-lookup"><span data-stu-id="25c04-154">Repeat steps 3-5 to install the Storport and Spaceport update **KB3121261**.</span></span>
4. <span data-ttu-id="25c04-155">2. frissítés vagy korábbi verzió frissítésekor, is szüksége lesz töltheti le:</span><span class="sxs-lookup"><span data-stu-id="25c04-155">If you are updating from Update 2 or earlier version, you will also need to download:</span></span>
   
   * <span data-ttu-id="25c04-156">az iSCSI-javítás KB3146621 használatával</span><span class="sxs-lookup"><span data-stu-id="25c04-156">iSCSI fix using KB3146621</span></span>
   * <span data-ttu-id="25c04-157">WMI-javítás KB3103616 használatával</span><span class="sxs-lookup"><span data-stu-id="25c04-157">WMI fix using KB3103616</span></span>

#### <a name="to-install-and-verify-maintenance-mode-hotfixes"></a><span data-ttu-id="25c04-158">Karbantartási módú gyorsjavítások telepítése és ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="25c04-158">To install and verify maintenance mode hotfixes</span></span>
<span data-ttu-id="25c04-159">Belső vezérlőprogram-frissítésekre telepítendő KB3121899 használja.</span><span class="sxs-lookup"><span data-stu-id="25c04-159">Use KB3121899 to install disk firmware updates.</span></span> <span data-ttu-id="25c04-160">Ezek működési zavart okozó frissítések, amelyeknek a végrehajtása körülbelül 30 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="25c04-160">These are disruptive updates and take around 30 minutes to complete.</span></span> <span data-ttu-id="25c04-161">Ha úgy dönt, ezeket egy karbantartási időszakban is telepítheti, ha csatlakozik az eszköz soros konzoljához.</span><span class="sxs-lookup"><span data-stu-id="25c04-161">You can choose to install these in a planned maintenance window by connecting to the device serial console.</span></span>

<span data-ttu-id="25c04-162">Vegye figyelembe, hogy amennyiben a lemezfirmware már naprakész, nem szükséges telepítenie ezeket a frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="25c04-162">Note that if your disk firmware is already up-to-date, you won't need to install these updates.</span></span> <span data-ttu-id="25c04-163">Futtassa a `Get-HcsUpdateAvailability` parancsmagot az eszköz soros konzoljáról, és ellenőrizze, hogy vannak-e elérhető frissítések, és azok zavart okozó (karbantartási módú) vagy zavart nem okozó (normál módú) frissítések-e.</span><span class="sxs-lookup"><span data-stu-id="25c04-163">Run the `Get-HcsUpdateAvailability` cmdlet from the device serial console to check if updates are available and whether the updates are disruptive (maintenance mode) or non-disruptive (regular mode) updates.</span></span>

<span data-ttu-id="25c04-164">A lemezfirmware-frissítések telepítéséhez kövesse az alábbi utasításokat.</span><span class="sxs-lookup"><span data-stu-id="25c04-164">To install the disk firmware updates, follow the instructions below.</span></span>

1. <span data-ttu-id="25c04-165">Helyezze az eszközt a karbantartási módban.</span><span class="sxs-lookup"><span data-stu-id="25c04-165">Place the device in the Maintenance mode.</span></span> <span data-ttu-id="25c04-166">Vegye figyelembe, hogy ne használjon Windows PowerShell távoli eljáráshívás karbantartási módban lévő eszköz történő csatlakozás során.</span><span class="sxs-lookup"><span data-stu-id="25c04-166">Note that you should not use Windows PowerShell remoting when connecting to a device in Maintenance mode.</span></span> <span data-ttu-id="25c04-167">Ehelyett futtassa ezt a parancsmagot a eszköz tartományvezérlőn, amikor az eszköz soros konzolon keresztül csatlakoznak.</span><span class="sxs-lookup"><span data-stu-id="25c04-167">Instead run this cmdlet on the device controller when connected through the device serial console.</span></span> <span data-ttu-id="25c04-168">Típus:</span><span class="sxs-lookup"><span data-stu-id="25c04-168">Type:</span></span>
   
    `Enter-HcsMaintenanceMode`
   
    <span data-ttu-id="25c04-169">Az alábbiakban egy példa látható a kimenetre.</span><span class="sxs-lookup"><span data-stu-id="25c04-169">A sample output is shown below.</span></span>
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update2-8100-SHG0997879L76673
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected to Controller0 - Passive
        ---------------------------------------------------------------
   
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    <span data-ttu-id="25c04-170">Mindkét vezérlőhöz majd indítsa újra a számítógépet karbantartási módba.</span><span class="sxs-lookup"><span data-stu-id="25c04-170">Both the controllers then restart into Maintenance mode.</span></span>
2. <span data-ttu-id="25c04-171">A lemezfirmware-frissítés telepítéséhez írja be a következőt:</span><span class="sxs-lookup"><span data-stu-id="25c04-171">To install the disk firmware update, type:</span></span>
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    <span data-ttu-id="25c04-172">Az alábbiakban egy példa látható a kimenetre.</span><span class="sxs-lookup"><span data-stu-id="25c04-172">A sample output is shown below.</span></span>
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\DiskFirmwarePackage.exe -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After the hotfix is installed on this controller, install it on the peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of the controllers. By installing new updates you agree to, and accept any additional terms associated with, the new functionality listed in the release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want to continue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes to complete.
3. <span data-ttu-id="25c04-173">A telepítési folyamatot a `Get-HcsUpdateStatus` parancs használatával figyelheti.</span><span class="sxs-lookup"><span data-stu-id="25c04-173">Monitor the install progress using `Get-HcsUpdateStatus` command.</span></span> <span data-ttu-id="25c04-174">A frissítés akkor fejeződött be, ha a `RunInProgress` `False` értékre vált.</span><span class="sxs-lookup"><span data-stu-id="25c04-174">The update is complete when the `RunInProgress` changes to `False`.</span></span>
4. <span data-ttu-id="25c04-175">Miután befejeződött a telepítés, a vezérlő, amelyre a karbantartási módú gyorsjavítás telepítve lett, újraindul.</span><span class="sxs-lookup"><span data-stu-id="25c04-175">After the installation is complete, the controller on which the maintenance mode hotfix was installed restarts.</span></span> <span data-ttu-id="25c04-176">Jelentkezzen be az 1. lehetőséggel teljes hozzáféréssel, és ellenőrizze a lemezfirmware verzióját.</span><span class="sxs-lookup"><span data-stu-id="25c04-176">Log in as option 1 with full access and verify the disk firmware version.</span></span> <span data-ttu-id="25c04-177">Típus:</span><span class="sxs-lookup"><span data-stu-id="25c04-177">Type:</span></span>
   
   `Get-HcsFirmwareVersion`
   
   <span data-ttu-id="25c04-178">A várt lemezfirmware-verzió a következő:</span><span class="sxs-lookup"><span data-stu-id="25c04-178">The expected disk firmware versions are:</span></span>
   
   `XMGG, XGEG, KZ50, F6C2, VR08`
   
   <span data-ttu-id="25c04-179">Az alábbiakban egy példa látható a kimenetre.</span><span class="sxs-lookup"><span data-stu-id="25c04-179">A sample output is shown below.</span></span>
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8100
       Name: Update2-8100-SHG0997879L76YD
       Software Version: 6.3.9600.17705
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected to Controller1
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
   
    <span data-ttu-id="25c04-180">Futtassa a `Get-HcsFirmwareVersion` parancsot a második vezérlőn annak ellenőrzéséhez, hogy a szoftververzió frissítve lett-e.</span><span class="sxs-lookup"><span data-stu-id="25c04-180">Run the `Get-HcsFirmwareVersion` command on the second controller to verify that the software version has been updated.</span></span> <span data-ttu-id="25c04-181">Ezután kiléphet a karbantartási módból.</span><span class="sxs-lookup"><span data-stu-id="25c04-181">You can then exit the maintenance mode.</span></span> <span data-ttu-id="25c04-182">Ehhez írja be a következő parancsot mindkét eszközvezérlő esetében:</span><span class="sxs-lookup"><span data-stu-id="25c04-182">To do so, type the following command for each device controller:</span></span>
   
   `Exit-HcsMaintenanceMode`
5. <span data-ttu-id="25c04-183">A tartományvezérlők újraindítása a kilép karbantartási módból.</span><span class="sxs-lookup"><span data-stu-id="25c04-183">The controllers restart when you exit Maintenance mode.</span></span> <span data-ttu-id="25c04-184">A lemezfirmware-frissítések sikeres alkalmazását követően, miután az eszköz kilépett a karbantartási módból, térjen vissza a klasszikus Azure portálhoz.</span><span class="sxs-lookup"><span data-stu-id="25c04-184">After the disk firmware updates are successfully applied and the device has exited maintenance mode, return to the Azure classic portal.</span></span> <span data-ttu-id="25c04-185">Vegye figyelembe, hogy a portál nem lehet, hogy megjelenítése, hogy telepítette-e a karbantartási mód frissítései 24 óra.</span><span class="sxs-lookup"><span data-stu-id="25c04-185">Note that the portal might not show that you installed the Maintenance mode updates for 24 hours.</span></span>

