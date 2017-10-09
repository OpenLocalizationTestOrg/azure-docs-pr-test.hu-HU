<!--author=alkohli last changed: 08/21/17-->

#### <a name="toodownload-hotfixes"></a>toodownload gyorsjavítások

Hajtsa végre a következő lépéseket toodownload hello szoftverfrissítést a Microsoft Update katalógus hello hello.

1. Indítsa el az Internet Explorert, és keresse meg a túl[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).
2. Ha az első alkalommal használja a Microsoft Update katalógus hello ezen a számítógépen, kattintson a **telepítése** amikor felszólító tooinstall hello bővítmény a Microsoft Update katalógusból.

    ![Katalógus telepítése](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)

3. A Microsoft Update katalógus hello hello a keresési mezőbe, írja be a hello Tudásbázis (KB) száma hello gyorsjavítás kívánt toodownload, például **4037264**, és kattintson a **keresési**.
   
    hello gyorsjavítás lista megjelenik, például **összegző szoftverfrissítési csomagot frissítés 5.0 a StorSimple 8000 Series**.
   
    ![Keresés a katalógusban](./media/storsimple-install-update5-hotfix/update-catalog-search.png)

4. Kattintson a **Letöltés** gombra. Adja meg vagy **Tallózás** tooa helyi helyének hello tooappear tölti le. Kattintson a hello fájlok toodownload toohello megadott helyre és mappára. hello mappába másolt tooa hálózati megosztásra, amely elérhető hello eszköz is lehet.
5. Keressen további gyorsjavítások a fenti hello táblázatban felsorolt (**4037266**), és a letöltési hello megfelelő fájlok toohello meghatározott mappákról, a tábla megelőző hello.

> [!NOTE]
> hello gyorsjavítások mindkét hello társ vezérlő érkező üzenetek ezt a hibát a tartományvezérlők toodetect elérhetőknek kell lenniük.
>
> hello gyorsjavítások 3 külön mappákban kell másolni. Például hello eszköz szoftver/Cis/MDS ügynök frissítése a másolhatók _FirstOrderUpdate_ mappa összes hello más nem zavaró frissítések sikerült átmásolni a hello _SecondOrderUpdate_ mappa, és karbantartási mód frissítések átmásolja a _ThirdOrderUpdate_ mappa.

#### <a name="tooinstall-and-verify-regular-mode-hotfixes"></a>tooinstall és normál mód gyorsjavítások ellenőrzése

Hajtsa végre a következő lépéseket tooinstall hello, és ellenőrizze a normál módú gyorsjavítások. Ha már telepítette az Azure portál használatával hello hagyja ki azokat, amelyek túl[telepítse, és ellenőrizze a karbantartási mód gyorsjavítások](#to-install-and-verify-maintenance-mode-hotfixes).

1. tooinstall hello gyorsjavítások, hozzáférési hello Windows PowerShell felületet a StorSimple eszköz soros konzoljához a. Hajtsa végre a hello részletes utasításait [PuTTy használata tooconnect toohello soros konzol](../articles/storsimple/storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console). Hello parancssorban nyomja le az **Enter**.
2. Válassza ki **1. lehetőség** toolog teljes hozzáféréssel rendelkező toohello eszközön. Azt javasoljuk, hogy hello gyorsjavítás hello passzív tartományvezérlőre telepíti először.
3. tooinstall hello gyorsjavítás, parancssorba hello típusa:
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    DNS helyett IP használata a megosztás elérési útja, a fenti parancs hello. hello credential paraméter csak akkor, ha egy hitelesített megosztás elérésére szolgál.
   
    Azt javasoljuk, hogy használja-e hello credential paraméter tooaccess megosztások. Még akkor is, megosztások, megnyitott túl "mindenki" jellemzően a toounauthenticated felhasználók nem nyitható meg.
   
4. Adja meg a hello jelszót. Az alábbiakban látható egy minta kimenet hello első rendelés frissítések telepítése. Hello első rendelés frissítés toopoint toohello adott fájl szükséges.

    >[!NOTE] 
    > Telepítenie kell a hello _HcsSoftwareUpdate.exe_ első. Miután a telepítés befejeződött, majd telepítse _CisMdsAgentUpdate.exe_.
   
        ````
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \FirstOrderUpdate\HcsSoftwareUpdate.exe -Credential contoso\John
   
        Confirm
   
        This operation starts hello hotfix installation and could reboot one or
        both of hello controllers. If hello device is serving I/Os, these will not
        be disrupted. Are you sure you want toocontinue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y
   
        ````
5. Típus **Y** amikor felszólító tooconfirm hello gyorsjavítás telepítése.
6. Figyelje az hello frissítés hello `Get-HcsUpdateStatus` parancsmag. hello frissítés először befejezi az hello passzív történik. Miután hello passzív vezérlő frissül, a feladatátvétel, és hello frissítés majd lesznek érvényben lévő hello más vezérlő. hello frissítés befejeződött, mindkét hello tartományvezérlők frissítésekor.
   
    hello következő minta kimenet látható hello frissítés folyamatban van. Hello `RunInprogress` van `True` amikor hello frissítése folyamatban van.

    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 07/28/2017 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     a következő minta kimenet hello azt jelzi, hogy adott hello frissítése befejeződött. Hello `RunInProgress` van `False` Ha hello frissítése befejeződött.
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 07/28/2017 9:15:55 AM
    LastUpdateTimestamp : 07/28/2017 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE]
    > Alkalmanként hello parancsmag jelentések `False` amikor hello frissítése még folyamatban van. amely gyorsjavítás hello tooensure befejeződött, várjon néhány percet, futtassa újra a parancsot, és győződjön meg arról, hogy hello `RunInProgress` van `False`. Ha igen, hello gyorsjavítás befejeződött.

7. Hello szoftverfrissítés befejeződése után ellenőrizze a hello rendszer szoftververziók. Típus:
   
    `Get-HcsSystem`
   
    A következő verziók hello kell megjelennie:
   
   * `FriendlySoftwareVersion: StorSimple 8000 Series Update 5.0`
   *  `HcsSoftwareVersion: 6.3.9600.17845`
   
    Hello verziószáma nem változik hello frissítés telepítését követően, azt jelzi, hello gyorsjavítás tooapply sikertelen volt. Ha ezt látja, további segítségért forduljon a [Microsoft támogatási szolgálatához](../articles/storsimple/storsimple-8000-contact-microsoft-support.md).
     
    > [!IMPORTANT]
    > Újra kell indítania a hello aktív vezérlő keresztül hello `Restart-HcsController` parancsmag alkalmazása előtt a következő frissítés hello.
     
8. Ismételje meg a 3-6 tooinstall hello _CisMDSAgentupdate.exe_ ügynök letöltött tooyour _FirstOrderUpdate_ mappa.
8. Ismételje meg a 3-6 lépéseket tooinstall hello második rendelés frissítések. 

    > [!NOTE] 
    > Második rendelés frissítések több frissítést is telepíthető hello csak futtatásával `Start-HcsHotfix cmdlet` és toohello tartalmazó mappát második rendelés frissítések mutat. hello parancsmag hello mappában található a rendelkezésre álló hello frissítések hajtható végre. Ha egy frissítés már telepítve van, hello frissítés logika, amely észleli, és, hogy a frissítés nem alkalmazható.

    Miután minden hello gyorsjavítás telepítve van, a hello `Get-HcsSystem` parancsmag. hello verziók le kell:
    
    * `CisAgentVersion:  1.0.9724.0`
    * `MdsAgentVersion: 35.2.2.0`
    * `Lsisas2Version: 2.0.78.00`


#### <a name="tooinstall-and-verify-maintenance-mode-hotfixes"></a>tooinstall és karbantartási mód gyorsjavítások ellenőrzése

Használja a KB4037263 tooinstall lemez belső vezérlőprogram-frissítésekre. Ezek zavaró frissítések, és körülbelül 30 percet toocomplete igénybe vehet. Szerint is választhat tooinstall ezeket a tervezett karbantartások az összekötő toohello eszköz soros konzoljához.

> [!NOTE] 
> Ha a lemez belső vezérlőprogram már naprakész, ezek a frissítések tooinstall nem szükséges. Futtassa a hello `Get-HcsUpdateAvailability` hello eszköz soros konzoljához toocheck, ha frissítések érhetők el, és hogy hello parancsmag frissíti biztosan zavaró (karbantartás mód) vagy nem zavaró (normál mód) frissítéseket.

tooinstall hello lemez belső vezérlőprogram-frissítésekre, kövesse az alábbi hello utasításokat.

1. Állítsa hello eszköz hello karbantartási módba. 

    > [!NOTE] 
    > Ne használjon Windows PowerShell távoli eljáráshívás tooa eszköz karbantartási módba kapcsolódáskor. Ehelyett futtassa ezt a parancsmagot hello eszköz tartományvezérlőn hello eszköz soros konzolon keresztül csatlakozáskor.

    a karbantartási mód tooplace hello tartományvezérlőre, írja be:
   
    `Enter-HcsMaintenanceMode`
   
    Az alábbiakban egy példa látható a kimenetre.
   
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
   
    Mindkét hello tartományvezérlők majd indítsa újra a számítógépet karbantartási módba.
2. tooinstall hello lemez belső vezérlőprogram frissítése, típus:
   
    `Start-HcsHotfix -Path <path tooupdate file> -Credential <credentials in domain\username format>`
   
    Az alábbiakban egy példa látható a kimenetre.
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\ThirdOrderUpdates\ -Credential contoso\john
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
   
   `XMGJ, XGEG, KZ50, F6C2, VR08, N003, 0107`
   
   Az alábbiakban egy példa látható a kimenetre.
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8600
       Name: Update4-8600-mystorsimple
       Software Version: 6.3.9600.17845
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
   
    Futtassa a hello `Get-HcsFirmwareVersion` hello második vezérlő tooverify hello szoftverének verziójával, a parancs frissítve lett. Lépjen ki a karbantartási mód hello majd. toodo úgy, írja be a következő parancsot minden eszköz vezérlő hello:
   
   `Exit-HcsMaintenanceMode`

5. Hello, tartományvezérlői újraindítása a kilép karbantartási módból. Után hello lemez belső vezérlőprogram frissítése sikeresen telepítve van, és hello eszköz már kilépett a karbantartási mód visszatérési toohello Azure-portálon. Vegye figyelembe, hogy hello portal előfordulhat, hogy ne jelenjen meg, hogy telepítette-e hello karbantartási mód frissítéseit 24 óra.

