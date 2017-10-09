<!--author=alkohli last changed: 09/01/16-->

#### <a name="toodownload-hotfixes"></a>toodownload gyorsjavítások
Hajtsa végre a következő lépéseket toodownload hello szoftverfrissítést a Microsoft Update katalógus hello hello.

1. Indítsa el az Internet Explorert, és keresse meg a túl[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).
2. Ha az első alkalommal használja a Microsoft Update katalógus hello ezen a számítógépen, kattintson a **telepítése** amikor felszólító tooinstall hello bővítmény a Microsoft Update katalógusból.
    ![Katalógus telepítése](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)
3. A Microsoft Update katalógus hello hello a keresési mezőbe, írja be a hello Tudásbázis (KB) száma hello gyorsjavítás kívánt toodownload, például **3186843**, és kattintson a **keresési**.
   
    hello gyorsjavítás lista megjelenik, például **összegző szoftverfrissítési csomagot frissítés 3.0 a StorSimple 8000 Series**.
   
    ![Keresés a katalógusban](./media/storsimple-install-update2-hotfix/HCS_SearchCatalog1-include.png)
4. Kattintson az **Add** (Hozzáadás) parancsra. hello frissítés hozzá legyen adva toohello kosár.
5. Keressen további gyorsjavítások a fenti hello táblázatban felsorolt (**3186859**), és adja hozzá az egyes toohello kosár.
6. Kattintson a **Kosár megtekintése** gombra.
7. Kattintson a **Letöltés** gombra. Adja meg vagy **Tallózás** tooa helyi helyének hello tooappear tölti le. hello frissítések letöltése toohello megadott helyre, és az azonos név hello frissítésként hello alárendelt mappába helyezi. hello mappába másolt tooa hálózati megosztásra, amely elérhető hello eszköz is lehet.

> [!NOTE]
> hello gyorsjavítások mindkét hello társ vezérlő érkező üzenetek ezt a hibát a tartományvezérlők toodetect elérhetőknek kell lenniük.
> 
> 

#### <a name="tooinstall-and-verify-regular-mode-hotfixes"></a>tooinstall és normál mód gyorsjavítások ellenőrzése
Hajtsa végre a következő lépéseket tooinstall hello, és ellenőrizze a normál módú gyorsjavítások. Ha már telepítette a használatával hello Azure portál, hagyja ki azokat, amelyek túl[telepítse, és ellenőrizze a karbantartási mód gyorsjavítások](#to-install-and-verify-maintenance-mode-hotfixes).

1. tooinstall hello gyorsjavítások, hozzáférési hello Windows PowerShell felületet a StorSimple eszköz soros konzoljához a. Hajtsa végre a hello részletes utasításait [PuTTy használata tooconnect toohello soros konzol](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console). Hello parancssorban nyomja le az **Enter**.
2. Válassza ki **1. lehetőség** toolog teljes hozzáféréssel rendelkező toohello eszközön. Azt javasoljuk, hogy hello gyorsjavítás hello passzív tartományvezérlőre telepíti először.
3. tooinstall hello gyorsjavítás, parancssorba hello típusa:
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    DNS helyett IP használata a megosztás elérési útja, a fenti parancs hello. hello credential paraméter csak akkor, ha egy hitelesített megosztás elérésére szolgál.
   
    Azt javasoljuk, hogy használja-e hello credential paraméter tooaccess megosztások. Még akkor is, megosztások, megnyitott túl "mindenki" jellemzően a toounauthenticated felhasználók nem nyitható meg.
   
    Adja meg a hello jelszót.
   
    Az alábbiakban egy példa látható a kimenetre.
   
        ````
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \hcsmdssoftwareupdate.exe -Credential contoso\John
   
        Confirm
   
        This operation starts hello hotfix installation and could reboot one or
        both of hello controllers. If hello device is serving I/Os, these will not
        be disrupted. Are you sure you want toocontinue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y
   
        ````
4. Típus **Y** amikor felszólító tooconfirm hello gyorsjavítás telepítése.
5. Figyelje az hello frissítés hello `Get-HcsUpdateStatus` parancsmag. hello frissítés először befejezi az hello passzív történik. Miután hello passzív vezérlő frissül, a feladatátvétel, és hello frissítés majd lesznek érvényben lévő hello más vezérlő. hello frissítés befejeződött, mindkét hello tartományvezérlők frissítésekor.
   
    hello következő minta kimenet látható hello frissítés folyamatban van. Hello `RunInprogress` lesz `True` amikor hello frissítése folyamatban van.

    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 8/29/2016 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     a következő minta kimenet hello azt jelzi, hogy adott hello frissítése befejeződött. Hello `RunInProgress` lesz `False` amikor hello frissítése befejeződött.
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 8/30/2016 9:15:55 AM
    LastUpdateTimestamp : 8/30/2016 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE] 
    > Alkalmanként hello parancsmag jelentések `False` amikor hello frissítése még folyamatban van. amely gyorsjavítás hello tooensure befejeződött, várjon néhány percet, futtassa újra a parancsot, és győződjön meg arról, hogy hello `RunInProgress` van `False`. Ha igen, hello gyorsjavítás befejeződött.

1. Hello szoftverfrissítés befejeződése után ellenőrizze a hello rendszer szoftververziók. Típus:
   
    `Get-HcsSystem`
   
    A következő verziók hello kell megjelennie:
   
   * `HcsSoftwareVersion: 6.3.9600.17759`
   * `CisAgentVersion:  1.0.9343.0`
   * `MdsAgentVersion: 30.0.4698.16`
     
     Hello verziószámok hello frissítés telepítését követően ne módosítsa, azt jelzi, hello gyorsjavítás tooapply sikertelen volt. Ha ezt látja, további segítségért forduljon a [Microsoft támogatási szolgálatához](../articles/storsimple/storsimple-contact-microsoft-support.md).
     
     > [!IMPORTANT]
     > Újra kell indítania a hello aktív vezérlő keresztül hello `Restart-HcsController` parancsmag hello fennmaradó frissítések alkalmazása előtt. 
     > 
     > 
2. Ismételje meg a 3-5 tooinstall hello LSI illesztőprogram és a belső vezérlőprogram gyorsjavítást **KB3186859**. Miután hello gyorsjavítás telepítve van, a hello `Get-HcsSystem` parancsmag. hello LSI verziószámának kell lennie:
   
   * `Lsisas2Version: 2.0.78.00`
3. Ismételje meg a 3-5 tooinstall hello Storport és Spaceport frissítés **KB3121261**.
4. 2. frissítés vagy korábbi verzió frissítésekor, toodownload is szüksége lesz:
   
   * az iSCSI-javítás KB3146621 használatával
   * WMI-javítás KB3103616 használatával

#### <a name="tooinstall-and-verify-maintenance-mode-hotfixes"></a>tooinstall és karbantartási mód gyorsjavítások ellenőrzése
Használja a KB3121899 tooinstall lemez belső vezérlőprogram-frissítésekre. Ezek zavaró frissítések, és körülbelül 30 percet toocomplete igénybe vehet. Szerint is választhat tooinstall ezeket a tervezett karbantartások az összekötő toohello eszköz soros konzoljához.

Vegye figyelembe, ha a lemez belső vezérlőprogram már naprakész, akkor nem kell tooinstall ezeket a frissítéseket. Futtassa a hello `Get-HcsUpdateAvailability` hello eszköz soros konzoljához toocheck, ha frissítések érhetők el, és hogy hello parancsmag frissíti biztosan zavaró (karbantartás mód) vagy nem zavaró (normál mód) frissítéseket.

tooinstall hello lemez belső vezérlőprogram-frissítésekre, kövesse az alábbi hello utasításokat.

1. Állítsa hello eszköz hello karbantartási módba. Vegye figyelembe, hogy ne használjon Windows PowerShell távoli eljáráshívás tooa eszköz karbantartási módba kapcsolódáskor. Ehelyett futtassa ezt a parancsmagot hello eszköz tartományvezérlőn hello eszköz soros konzolon keresztül csatlakozáskor. Típus:
   
    `Enter-HcsMaintenanceMode`
   
    Az alábbiakban egy példa látható a kimenetre.
   
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
   
    Mindkét hello tartományvezérlők majd indítsa újra a számítógépet karbantartási módba.
2. tooinstall hello lemez belső vezérlőprogram frissítése, típus:
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    Az alábbiakban egy példa látható a kimenetre.
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\DiskFirmwarePackage.exe -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After hello hotfix is installed on this controller, install it on hello peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of hello controllers. By installing new updates you agree to, and accept any additional terms associated with, hello new functionality listed in hello release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want toocontinue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes toocomplete.
3. A figyelő hello telepítési folyamat használatával `Get-HcsUpdateStatus` parancsot. hello frissítés befejeződött amikor hello `RunInProgress` túl változik`False`.
4. Hello telepítés befejezése után újraindítja a hello vezérlő melyik hello karbantartási mód gyorsjavítás telepítve lett. Jelentkezzen be 1. lehetőség teljes hozzáféréssel, és hello lemez belső vezérlőprogram verziójának ellenőrzése. Típus:
   
   `Get-HcsFirmwareVersion`
   
   hello várt, belsővezérlőprogram-verziók a következők:
   
   `XMGG, XGEG, KZ50, F6C2, VR08`
   
   Az alábbiakban egy példa látható a kimenetre.
   
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
   
    Futtassa a hello `Get-HcsFirmwareVersion` hello második vezérlő tooverify hello szoftverének verziójával, a parancs frissítve lett. Lépjen ki a karbantartási mód hello majd. toodo úgy, írja be a következő parancsot minden eszköz vezérlő hello:
   
   `Exit-HcsMaintenanceMode`
5. Hello, tartományvezérlői újraindítása a kilép karbantartási módból. Után hello lemez belső vezérlőprogram frissítése sikeresen telepítve van, és hello eszköz már kilépett a karbantartási mód visszatérési toohello a klasszikus Azure portálon. Vegye figyelembe, hogy hello portal előfordulhat, hogy ne jelenjen meg, hogy telepítette-e hello karbantartási mód frissítéseit 24 óra.

