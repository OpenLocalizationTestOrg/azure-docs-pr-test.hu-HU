1. A távoli asztal csatlakoztatott toohello a virtuális gépet, miközben keresse meg **Configuration Manager**:

    ![Az SSCM megnyitása](./media/virtual-machines-sql-server-connection-tcp-protocol/sql-server-configuration-manager.png)

1. Az SQL Server Configuration Manager, a hello konzol ablaktáblájában bontsa ki a **SQL Server hálózati konfigurációja**.

1. Hello konzol ablaktáblában kattintson **MSSQLSERVER protokolljai** (hello alapértelmezett példány nevét.) Hello részleteket tartalmazó ablaktáblában kattintson a jobb gombbal **TCP** kattintson **engedélyezése** Ha még nincs engedélyezve.

    ![A TCP engedélyezése](./media/virtual-machines-sql-server-connection-tcp-protocol/enable-tcp.png)

1. Hello konzol ablaktáblában kattintson **SQL Server Services**. Hello részleteket tartalmazó ablaktáblában kattintson a jobb gombbal  **SQL Server (*példánynév*) ** (hello alapértelmezett példány **SQL Server (MSSQLSERVER)**), és kattintson a **újraindítása** , SQL Server-példányt toostop, és indítsa újra a hello.

    ![Az Adatbázismotor újraindítása](./media/virtual-machines-sql-server-connection-tcp-protocol/restart-sql-server.png)

1. Zárja be az SQL Server Konfigurációkezelőt.

Az SQL Server adatbázismotor hello protokollok engedélyezésével kapcsolatos további információkért lásd: [engedélyezheti vagy tilthatja le a hálózati protokoll](http://msdn.microsoft.com/library/ms191294.aspx).
