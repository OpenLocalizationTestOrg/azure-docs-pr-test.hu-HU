### <a name="determine-hello-dns-name-of-hello-virtual-machine"></a>Határozza meg a virtuális gép hello hello DNS-neve
tooconnect toohello SQL Server adatbázismotorhoz egy másik számítógépről, ismernie kell a tartománynévrendszer (DNS) hello hello virtuális gép nevét. (Ez a hello neve hello internet által használt tooidentify hello virtuális gépet. Hello IP-címet is használhat, de hello IP-cím változhat, amikor Azure redundancia vagy karbantartás erőforrásokat helyez át. hello DNS-neve lesz stabil, mert azok tooa új IP-cím átirányítva.)  

1. Az Azure portál hello (vagy hello előző lépésben), válassza ki a **virtuális gépek (klasszikus)**.
2. Válassza ki az SQL virtuális gépét.
3. A hello **virtuális gép** panelen, a Másolás hello **DNS-név** hello virtuális géphez.
   
    ![DNS-név](./media/virtual-machines-sql-server-connection-steps/sql-vm-dns-name.png)

### <a name="connect-toohello-database-engine-from-another-computer"></a>Adatbázis-kezelő toohello Csatlakozás másik számítógépről
1. Egy számítógép csatlakozik a toohello internet, nyissa meg az SQL Server Management Studio eszközt.
2. A hello **tooServer csatlakozás** vagy **tooDatabase motor csatlakozás** párbeszédpanel hello **kiszolgálónév** hello virtuális számítógép (meghatározott hello hello DNS-nevét adja meg előző tevékenység) és egy nyilvános végpontot portszámot hello formátumban *DNSName, portszám* például **mysqlvm.cloudapp.net,57500**.
   
    ![Csatlakozás az SSMS használatával](./media/virtual-machines-sql-server-connection-steps/33Connect-SSMS.png)
   
    Ha nem emlékszik hello nyilvános végpontot portszám korábban létrehozott, az hello található **végpontok** hello területe **virtuális gép** panelen.
   
    ![Nyilvános port](./media/virtual-machines-sql-server-connection-steps/sql-vm-port-number.png)
3. A hello **hitelesítési** mezőben válassza **SQL Server-hitelesítés**.
4. A hello **bejelentkezési** be, olyan bejelentkezési azonosítót, egy korábbi feladatban létrehozott hello nevét.
5. A hello **jelszó** mezőbe, írja be a jelszót hello az Ön által létrehozott korábbi feladat hello bejelentkezési adatokat.
6. Kattintson a **Connect** (Csatlakozás) gombra.

