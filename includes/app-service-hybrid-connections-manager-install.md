
1. A hello **hibrid kapcsolatok** panelen hello hibrid kapcsolat imént létrehozott, majd **figyelő telepítő**.
   
    ![Kattintson a figyelő beállítás](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
2. Hello **hibrid kapcsolat tulajdonságai** panel nyílik meg. A **helyszíni Hybrid Connection Manager**, válassza a **manuális letöltés és beállítás**, mentse a letöltött hello HybridConnectionManager.msi csomagot, és másolja a hello átjáró kapcsolati karakterlánca.
   
    ![Kattintson ide tooinstall](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
3. Rendszergazdai jogú parancssort írja be a következő parancs toostart hello telepítő hello:
   
        start HybridConnectionManager.msi
4. Hello után telepítő fut, kattintson a **most nem**, majd keresse meg a toohello %ProgramFiles%\Microsoft\HybridConnectionManager mappa, futtassa a HCMConfigWizard.exe, és kattintson **Igen** a hello **felhasználó Fiókok felügyelete** párbeszédpanel.
5. Illessze be a korábban kimásolt hello hibrid kapcsolati karakterláncot, és kattintson a **OK**. 
   
    ![Telepítése](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
6. Hello telepítés befejeztével kattintson **Bezárás**.
   
    ![Kattintson a Bezárás gombra](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
   
    A hello **hibrid kapcsolatok** panelen, hello **állapot** most az oszlop látható **csatlakoztatva**. 
   
    ![Csatlakoztatott állapot](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)

