### <a name="open-tcp-ports-in-hello-windows-firewall-for-hello-default-instance-of-hello-database-engine"></a>Nyissa meg a TCP-portok hello adatbázismotor alapértelmezett példánya hello hello a Windows tűzfal
1. Csatlakoztassa a toohello virtuális gépet a távoli asztalról. A virtuális gép toohello csatlakozó részletes utasításokért lásd: [nyisson meg egy SQL virtuális gép távoli asztal](../articles/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md#open-the-vm-with-remote-desktop).
2. Miután hello kezdőképernyőjén naplózza, írja be a **WF.msc**, majd nyomja le az ENTER BILLENTYŰT.
   
    ![Hello tűzfal Program elindítása](./media/virtual-machines-sql-server-connection-steps/12Open-WF.png)
3. A hello **fokozott biztonságú Windows tűzfal**, a hello bal oldali ablaktáblán kattintson a jobb gombbal **bejövő szabályok**, és kattintson a **új szabály** hello művelet ablaktáblában.
   
    ![Új szabály](./media/virtual-machines-sql-server-connection-steps/13New-FW-Rule.png)
4. A hello **új bejövő szabály varázsló** párbeszédpanel **szabálytípus**, jelölje be **Port**, és kattintson a **következő**.
5. A hello **protokoll és portok** párbeszédpanelen alapértelmezés használata hello **TCP**. A hello **adott helyi portok** mezőbe, majd típus hello portszámát hello adatbázismotor példánya hello (**1433** hello alapértelmezett példány vagy a magánhálózati port hello hello végpont lépésben kiválasztott).
   
    ![1433-as TCP-port](./media/virtual-machines-sql-server-connection-steps/14Port-1433.png)
6. Kattintson a **Tovább** gombra.
7. A hello **művelet** párbeszédpanelen jelölje ki **hello csatlakozás engedélyezése**, és kattintson a **következő**.
   
    **Biztonsági Megjegyzés:** Selecting **hello csatlakozás engedélyezése, ha biztonságos** növelheti a biztonságot. Válassza ezt a lehetőséget, ha szeretné tooconfigure további biztonsági beállítások a környezetben.
   
    ![Kapcsolatok engedélyezése](./media/virtual-machines-sql-server-connection-steps/15Allow-Connection.png)
8. A hello **profil** párbeszédpanelen jelölje ki **nyilvános**, **titkos**, és **tartomány**. Ezután kattintson a **Next** (Tovább) gombra.
   
    **Biztonsági Megjegyzés:** Selecting **nyilvános** lehetővé teszi a hozzáférést keresztül hello internet. Amikor csak lehetséges, válasszon egy szigorúbb profilt.
   
    ![Nyilvános profil](./media/virtual-machines-sql-server-connection-steps/16Public-Private-Domain-Profile.png)
9. A hello **neve** párbeszédpanelen írja be egy nevet és leírást a szabályhoz, és kattintson **Befejezés**.
   
    ![Szabály neve](./media/virtual-machines-sql-server-connection-steps/17Rule-Name.png)

Szükség szerint nyisson meg további portokat a többi összetevőhöz. További információkért lásd: [hello Windows tűzfal tooAllow SQL Server-hozzáférés konfigurálása](http://msdn.microsoft.com/library/cc646023.aspx).

### <a name="configure-sql-server-toolisten-on-hello-tcp-protocol"></a>SQL Server toolisten konfigurálja a TCP protokoll hello

[!INCLUDE [Enable TCP](virtual-machines-sql-server-connection-tcp-protocol.md)]

### <a name="configure-sql-server-for-mixed-mode-authentication"></a>Az SQL Server konfigurálása vegyes módú hitelesítéshez
SQL Server adatbázismotor hello nem használható Windows-hitelesítés nélkül tartományi környezetben. tooconnect toohello adatbázismotor egy másik számítógépről, SQL Server konfigurálása a vegyes módú hitelesítést. A vegyes módú hitelesítés az SQL Server-hitelesítést és a Windows-hitelesítést is lehetővé teszi.

> [!NOTE]
> Előfordulhat, hogy a vegyes módú hitelesítés konfigurálása nem szükséges, ha az Azure Virtual Networköt egy már konfigurált tartománykörnyezettel konfigurálta.
> 
> 

1. Miközben csatlakoztatott toohello virtuális gép hello Start lap, írja be a **SQL Server Management Studio** és hello kijelölt ikonra.
   
    hello azt hello felhasználók Management Studio környezet létre kell hoznia a Management Studio első megnyitásakor. Ez eltarthat néhány pillanatig.
2. Management Studio megadja hello **tooServer csatlakozás** párbeszédpanel megnyitásához. A hello **kiszolgálónév** mezőjébe hello virtuális gép tooconnect toohello adatbázismotor hello Object Explorer hello nevét (hello is használhatja a virtuális gép neve helyett **(helyi)** vagy egy egyetlen időszak hello **kiszolgálónév**). Válassza ki **Windows-hitelesítés**, és hagyja  ***virtuális_gép_neve*\your_local_administrator** a hello **felhasználónév** mezőbe. Kattintson a **Connect** (Csatlakozás) gombra.
   
    ![Csatlakozás tooServer](./media/virtual-machines-sql-server-connection-steps/19Connect-to-Server.png)
3. Az SQL Server Management Studio Object Explorer ablaktáblában kattintson a jobb gombbal a hello példány az SQL Server (hello virtuális gép neve) hello nevét, és kattintson **tulajdonságok**.
   
    ![Kiszolgáló tulajdonságai](./media/virtual-machines-sql-server-connection-steps/20Server-Properties.png)
4. A hello **biztonsági** lap **kiszolgálóhitelesítés**, jelölje be **SQL Server és a Windows-hitelesítés üzemmód**, és kattintson a **OK** .
   
    ![A hitelesítési mód kiválasztása](./media/virtual-machines-sql-server-connection-steps/21Mixed-Mode.png)
5. A hello SQL Server Management Studio párbeszédpanelen kattintson a **OK** tooacknowledge hello követelmény toorestart SQL Server.
6. Az Object Explorerben kattintson a jobb gombbal a kiszolgálóra, majd az **Újraindítás** lehetőségre. (Ha az SQL Server Agent fut, azt is újra kell indítani.)
   
    ![Újraindítás](./media/virtual-machines-sql-server-connection-steps/22Restart2.png)
7. A hello SQL Server Management Studio párbeszédpanelen kattintson a **Igen** , amelyet az SQL Server toorestart tooagree.

### <a name="create-sql-server-authentication-logins"></a>SQL Server-hitelesítési bejelentkezés létrehozása
tooconnect toohello adatbázismotor egy másik számítógépről, létre kell hoznia legalább egy SQL Server-hitelesítési bejelentkezési névként.

1. SQL Server Management Studio Object Explorerben bontsa ki a hello mappa kívánt toocreate hello új bejelentkezés hello server-példány.
2. Kattintson a jobb gombbal hello **biztonsági** mappára, mutasson túl**új**, és válassza ki **bejelentkezési...** .
   
    ![Új bejelentkezés](./media/virtual-machines-sql-server-connection-steps/23New-Login.png)
3. A hello **- új bejelentkezés** párbeszédpanel hello **általános** lapján adja meg hello hello új felhasználó hello **bejelentkezési név** mezőbe.
4. Kattintson az **SQL Server-hitelesítés** lehetőségre.
5. A hello **jelszó** mezőbe írja be a jelszót hello új felhasználó. Adjon meg, hogy a jelszó újra hello **jelszó megerősítése** mezőbe.
6. Válassza ki a hello kényszerítési jelszóbeállításokat szükséges (**jelszó házirend**, **kényszerítése a jelszó lejárati**, és **kell változtatni a jelszót a következő bejelentkezésekor**). Ha ehhez a bejelentkezéshez használ a szolgáltatást, nem kell toorequire jelszó módosítása hello következő bejelentkezésekor.
7. A hello **alapértelmezett adatbázis** listára, válassza ki az alapértelmezett adatbázis hello bejelentkezési azonosítóhoz. **fő** hello alapértelmezett ezt a beállítást. Ha még nem hozott létre a felhasználói adatbázis, hagyja üresen ezt a túl beállítása**fő**.
   
    ![Bejelentkezési tulajdonságok](./media/virtual-machines-sql-server-connection-steps/24Test-Login.png)
8. Ha ez hello első bejelentkezés hoz létre, érdemes lehet toodesignate ehhez a bejelentkezéshez egy SQL Server rendszergazdaként. Ha igen, hello **kiszolgálói szerepkörök** ellenőrzése lapon **sysadmin**.
   
   > [!NOTE]
   > Hello SysAdmin (rendszergazda) rögzített kiszolgálói szerepkör tagjai rendelkeznek a teljes körű vezérlést biztosítanak hello adatbázismotor. A szerepkör tagságát így gondosan határozza meg.
   > 
   > 
   
   ![sysadmin](./media/virtual-machines-sql-server-connection-steps/25sysadmin.png)
9. Kattintson az OK gombra.

További információ az SQL Server-bejelentkezésekről: [Bejelentkezés létrehozása](http://msdn.microsoft.com/library/aa337562.aspx).

