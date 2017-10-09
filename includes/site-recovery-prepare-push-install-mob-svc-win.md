### <a name="prepare-for-a-push-installation-on-a-windows-computer"></a>Egy leküldéses telepítését a Windows-számítógép előkészítése

1. Győződjön meg arról, hogy van-e hálózati kapcsolat hello Windows-számítógép és hello a folyamatkiszolgáló között.
2. Hozzon létre egy fiókot, hogy hello folyamatkiszolgáló tooaccess hello számítógép használható. hello fióknak (helyi vagy tartományi) rendszergazdai jogosultságokkal kell rendelkeznie. (Ez a fiók csak hello ügyfélleküldéses telepítés és használjon frissítéseihez.)

   > [!NOTE]
   > Ha nem használ egy olyan tartományi fiók, tiltsa le a távoli felhasználói hozzáférés-vezérlési hello helyi számítógépen. Távoli felhasználó hozzáférés-vezérlés, hello HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System beállításkulcs alatt toodisable hozzáadása egy új DWORD: **LocalAccountTokenFilterPolicy**. Hello érték túl beállítása**1**. toodo ezt a parancsot a parancssorba, futtassa a következő parancs hello:  
   `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`
   >
   >
2. A Windows tűzfal hello számítógépen tooprotect, jelölje be szeretné **lehetővé teszi egy alkalmazás vagy szolgáltatás átengedése a tűzfalon**. Engedélyezése **fájl- és nyomtatómegosztás** és **Windows Management Instrumentation (WMI)**. Tooa tartományhoz tartozó számítógépek esetében egy csoportházirend-objektumot (GPO) segítségével konfigurálhatja a hello tűzfal beállításait.

   ![Tűzfalbeállítások](./media/site-recovery-prepare-push-install-mob-svc-win/mobility1.png)

3. A CSPSConfigtool hello fiók hozzáadása.
    1.  Jelentkezzen be tooyour konfigurációs kiszolgáló.
    2.  Nyissa meg a következőt: **cspsconfigtool.exe**. (Érhető el gyors hello asztalon és hello %ProgramData%\home\svsystems\bin mappában.)
    3.  A hello **fiókok kezelése** lapon jelölje be **fiók hozzáadása**.
    4.  Vegye fel a létrehozott hello fiókot.
    5.  Adja meg a hello hitelesítő adatait használhatja, ha egy számítógép replikáció engedélyezése.
