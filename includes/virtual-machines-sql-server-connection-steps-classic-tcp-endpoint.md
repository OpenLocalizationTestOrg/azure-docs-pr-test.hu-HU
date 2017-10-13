### <a name="create-a-tcp-endpoint-for-the-virtual-machine"></a>TCP-végpont létrehozása a virtuális géphez
Ha az internetről szeretné elérni az SQL Servert, a virtuális gépnek rendelkeznie kell egy, a bejövő TCP-kommunikációt figyelő végponttal. Ez az Azure-konfigurációs lépés a TCP-port bejövő forgalmát egy olyan TCP-portra irányítja, amelyet elér a virtuális gép.

> [!NOTE]
> Ha azonos felhőszolgáltatáson vagy virtuális hálózaton belül csatlakozik, nem kell létrehoznia egy nyilvánosan elérhető végpontot. Ebben az esetben folytassa a következő lépéssel. További információ: [Kapcsolódási esetek](../articles/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-sql-connect.md#connection-scenarios).
> 
> 

1. Az Azure Portalon válassza a **Virtuális gépek (klasszikus)** lehetőséget.
2. Ezután válassza ki az SQL Server virtuális gépét.
3. Válassza a **Végpontok** lehetőséget, majd kattintson a **Hozzáadás** gombra a Végpontok panel tetején.
   
    ![A végpontlétrehozás lépései a Portalon](./media/virtual-machines-sql-server-connection-steps/portal-endpoint-creation.png)
4. A **Végpont hozzáadása** panelen adjon meg egy **Nevet**, például SQLVégpont.
5. A **Protokoll** területen válassza a **TCP** lehetőséget.
6. **Nyilvános port** esetén adjon meg egy portszámot, például **57500**.
7. **Privát port**esetén adja meg az SQL Server figyelőportját, amely alapértelmezés szerint az **1433**.
8. A végpont létrehozásához kattintson az **OK** gombra.

