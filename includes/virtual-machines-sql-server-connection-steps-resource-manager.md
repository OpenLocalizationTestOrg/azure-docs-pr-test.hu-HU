### <a name="configure-a-dns-label-for-hello-public-ip-address"></a>A DNS-címke hello nyilvános IP-cím konfigurálása

tooconnect toohello SQL Server adatbázis-kezelő a hello Internet, fontolja meg egy DNS-címke a nyilvános IP-cím létrehozása. IP-címe is elérheti, de hello DNS-címke létrehoz egy A rekordot, amely könnyebben tooidentify és kivonatok hello alapul szolgáló nyilvános IP-cím.

> [!NOTE]
> DNS-címkére esetén nincs szükség, ha terv, tooonly csatlakozás toohello SQL Server-példány hello belül azonos virtuális hálózaton, vagy csak a helyi számítógépre.

toocreate DNS-címke, először válassza **virtuális gépek** hello portálon. Válassza ki az SQL Server virtuális gép toobring a tulajdonságai megjelenítéséhez.

1. Hello virtuális gépek – áttekintés, válassza ki a **nyilvános IP-cím**.

    ![nyilvános IP-cím](./media/virtual-machines-sql-server-connection-steps/rm-public-ip-address.png)

1. Bontsa ki a nyilvános IP-cím hello tulajdonságait, **konfigurációs**.

1. Adjon meg egy DNS-címkenevet. Ez a név egy A rekordot, amely közvetlenül lehet használt tooconnect tooyour SQL Server virtuális gép neve helyett az IP-cím szerint.

1. Kattintson a hello **mentése** gombra.

    ![dns-címke](./media/virtual-machines-sql-server-connection-steps/rm-dns-label.png)

### <a name="connect-toohello-database-engine-from-another-computer"></a>Adatbázis-kezelő toohello Csatlakozás másik számítógépről

1. Egy számítógép csatlakozik a toohello internet, nyissa meg az SQL Server Management Studio (SSMS). Ha még nem rendelkezik az SQL Server Management Studio alkalmazással, [innen](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) letöltheti.

1. A hello **tooServer csatlakozás** vagy **tooDatabase motor csatlakozás** hello szerkesztése párbeszédpanelen **kiszolgálónév** érték. Adja meg a hello IP-cím vagy hello virtuális gép (hello előző feladatban meghatározott) teljes DNS-nevét. Egy vessző hozzáadásával az SQL Server TCP-portját is megadhatja. Például: `mysqlvmlabel.eastus.cloudapp.azure.com,1433`.

1. A hello **hitelesítési** mezőben válassza **SQL Server-hitelesítés**.

1. A hello **bejelentkezési** mezőben, egy érvényes SQL-bejelentkezési hello típusnév.

1. A hello **jelszó** mezőbe, írja be a jelszót hello hello bejelentkezési azonosító.

1. Kattintson a **Connect** (Csatlakozás) gombra.

    ![ssms connect](./media/virtual-machines-sql-server-connection-steps/rm-ssms-connect.png)