<!--author=SharS last changed: 03/17/2016-->

#### <a name="toodownload-hotfixes"></a>toodownload gyorsjavítások
Hajtsa végre a következő lépéseket toodownload hello szoftverfrissítés hello.

1. Indítsa el az Internet Explorert, és keresse meg a túl[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).
2. Ha az első alkalommal használja a Microsoft Update katalógus hello ezen a számítógépen, kattintson a **telepítése** amikor felszólító tooinstall hello bővítmény a Microsoft Update katalógusból.
    ![Katalógus telepítése](./media/storsimple-install-update-option-1/HCS_InstallCatalog-include.png)
3. A Microsoft Update katalógus hello hello a keresési mezőbe, írja be a hello Tudásbázis (KB) száma hello gyorsjavítás kívánt toodownload, például **3063418**, és kattintson a **keresési**.
4. Látni fogja a hello **StorSimple frissítés 1.2-es készülékek frissítés** csomagot. Kattintson az **Add** (Hozzáadás) parancsra. hello frissítés toohello kosár lesz hozzáadva.
5. Keressen további gyorsjavítások a fenti hello táblázatban felsorolt (**3043005** és **3063416**), és adja hozzá az egyes hello kosár.
6. Kattintson a **Kosár megtekintése** gombra.
   
    ![Kosár megtekintése](./media/storsimple-install-update-option-1/HCS_InstallBasket-include.png)
7. Kattintson a **Letöltés** gombra. Adja meg vagy **Tallózás** tooa helyi helyének hello tooappear tölti le. hello frissítések letöltése toohello megadott helyre, majd egy almappát azonos nevet hello frissítésként hello helyezi. hello mappába másolt tooa hálózati megosztásra, amely elérhető hello eszköz is lehet.

> [!NOTE]
> hello gyorsjavítások mindkét hello társ vezérlő érkező üzenetek ezt a hibát a tartományvezérlők toodetect elérhetőknek kell lenniük.
> 
> 

#### <a name="tooinstall-and-verify-regular-mode-hotfixes"></a>tooinstall és normál mód gyorsjavítások ellenőrzése
Hajtsa végre a következő lépéseket tooinstall hello, és ellenőrizze a hello normál módú gyorsjavítások. Ha már telepítette a használatával hello Azure portál, hagyja ki azokat, amelyek túl[telepítse, és ellenőrizze a karbantartási mód gyorsjavítások](#to-install-and-verify-maintenance-mode-hotfixes).

1. tooinstall hello szoftverfrissítési, hozzáférési hello Windows PowerShell felületet a StorSimple eszköz soros konzoljához a. Hajtsa végre a hello részletes utasításait [PuTTy használata tooconnect toohello soros konzol](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console). Hello parancssorban nyomja le az **Enter**.
2. Válassza ki **1. lehetőség** toolog teljes hozzáféréssel rendelkező toohello eszközön.
3. tooinstall hello frissítési csomag, parancssorba hello típusa:
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    DNS helyett IP használata a megosztás elérési útja, a fenti parancs hello. hello credential paraméter csak akkor, ha egy hitelesített megosztás elérésére szolgál.
   
    Azt javasoljuk, hogy használja-e hello credential paraméter tooaccess megosztások. Még akkor is, megosztások, megnyitott túl "mindenki" jellemzően a toounauthenticated felhasználók nem nyitható meg.
   
    Az alábbiakban egy példa látható a kimenetre.
   
    ```
    Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
    \hcsmdssoftwareupdate.exe -Credential contoso\John

    Confirm

    This operation starts hello hotfix installation and could reboot one or
    both of hello controllers. If hello device is serving I/Os, these will not
    be disrupted. Are you sure you want toocontinue?
    [Y] Yes [N] No [?] Help (default is "Y"): Y
    ```

4. Típus **Y** amikor felszólító tooconfirm hello gyorsjavítás telepítése.
5. Figyelje az hello frissítés hello `Get-HcsUpdateStatus` parancsmag.
   
    hello következő minta kimenet látható hello frissítés folyamatban van. Hello `RunInprogress` lesz `True` amikor hello frissítése folyamatban van.
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp : 9/02/2015 10:36:13 PM
    LastUpdateTimestamp : 9/02/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
     a következő minta kimenet hello azt jelzi, hogy adott hello frissítése befejeződött. Hello `RunInProgress` lesz `False` amikor hello frissítése befejeződött.

    ```
    Controller1>Get-HcsUpdateStatus

    RunInprogress       : False
    LastHotfixTimestamp : 9/02/2015 10:56:13 PM
    LastUpdateTimestamp : 9/02/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
   > [!NOTE]
   > Alkalmanként hello parancsmag jelentések `False` amikor hello frissítése még folyamatban van. amely gyorsjavítás hello tooensure befejeződött, várjon néhány percet, futtassa újra a parancsot, és győződjön meg arról, hogy hello `RunInProgress` van `False`. Ha igen, hello gyorsjavítás befejeződött.
   > 
   > 
6. Hello szoftverfrissítés befejeződése után ellenőrizze a hello rendszer szoftververziók. Írja be a következő parancs hello:
   
    `Get-HcsSystem`
   
    A következő verziók hello kell megjelennie:
   
   * HcsSoftwareVersion: 6.3.9600.17584
   * CisAgentVersion: 1.0.9049.0
   * MdsAgentVersion: 26.0.4696.1433
     
     Hello verziószámok hello frissítés telepítését követően ne módosítsa, azt jelzi, hello gyorsjavítás tooapply sikertelen volt. Ha ezt látja, további segítségért forduljon a [Microsoft támogatási szolgálatához](../articles/storsimple/storsimple-contact-microsoft-support.md).
7. Ismételje meg a 3-5 tooinstall hello fennmaradó normál módú gyorsjavítás (KB3043005).

#### <a name="tooinstall-and-verify-maintenance-mode-hotfixes"></a>tooinstall és karbantartási mód gyorsjavítások ellenőrzése
Használja a KB3063416 tooinstall lemez belső vezérlőprogram-frissítésekre. Ezek zavaró frissítések és körül toocomplete 30-45 percig tarthat. Szerint is választhat tooinstall ezeket a tervezett karbantartások az összekötő toohello eszköz soros konzoljához.

tooinstall hello lemez belső vezérlőprogram-frissítésekre, kövesse az alábbi hello utasításokat.

1. Hello eszköz helyez karbantartási módba. Vegye figyelembe, hogy ne használjon Windows PowerShell távoli eljáráshívás tooa eszköz karbantartási módba kapcsolódáskor. Ez a parancsmag hello eszköz tartományvezérlőn hello eszköz soros konzolon keresztül csatlakozáskor kell toorun. Típus:
   
    `Enter-HcsMaintenanceMode`
   
    Az alábbiakban egy példa látható a kimenetre.
   
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
   
    Mindkét hello tartományvezérlők majd indítsa újra a számítógépet karbantartási módba.
2. tooinstall hello lemez belső vezérlőprogram frissítése, típus:
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    Az alábbiakban egy példa látható a kimenetre.
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\DiskFirmwarePackage.exe -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After hello hotfix is installed on this controller, install it on hello peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of hello controllers. Are you sure you want toocontinue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes toocomplete.
3. A figyelő hello telepítési folyamat használatával `Get-HcsUpdateStatus` parancsot. hello frissítés befejeződött amikor hello `RunInProgress` túl változik`False`.
4. Hello telepítés befejezése után, amelyre hello karbantartási mód gyorsjavítás telepítve lett hello vezérlő újra kell indítani. Jelentkezzen be 1. lehetőség teljes hozzáféréssel, és hello lemez belső vezérlőprogram verziójának ellenőrzése. Típus:
   
   `Get-HcsFirmwareVersion`
   
   hello várt, belsővezérlőprogram-verziók a következők:
   
   `XMGG, XGEE, KZ50, F6C2, VR08`
   
   Futtassa a hello `Get-HcsFirmwareVersion` hello második vezérlő tooverify hello szoftverének verziójával, a parancs frissítve lett. Lépjen ki a karbantartási mód hello majd. Írja be a következő parancsot minden eszköz vezérlő hello:
   
   `Exit-HcsMaintenanceMode`
5. Hello, tartományvezérlői újraindítása a kilép karbantartási módból. Után hello lemez belső vezérlőprogram frissítése sikeresen telepítve van, és hello eszköz már kilépett a karbantartási mód visszatérési toohello a klasszikus Azure portálon. Vegye figyelembe, hogy hello portal előfordulhat, hogy ne jelenjen meg, hogy telepítette-e hello karbantartási mód frissítéseit 24 óra.

