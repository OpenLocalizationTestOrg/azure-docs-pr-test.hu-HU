### <a name="create-a-tcp-endpoint-for-hello-virtual-machine"></a>Hello virtuális gép TCP-végpont létrehozása
A sorrend tooaccess SQL Server hello az internet, hello virtuális gépnek rendelkeznie kell egy végpont toolisten bejövő TCP-kommunikációhoz. Az Azure-alapú konfigurációs lépés irányítja a bejövő TCP port forgalom tooa TCP-portot, amely elérhető toohello virtuális gépet.

> [!NOTE]
> Ha kapcsolódik hello belül ugyanaz a felhőalapú szolgáltatás, vagy a virtuális hálózati, nincs toocreate egy nyilvánosan elérhető végponton. Ebben az esetben lehetett folytatni toohello következő lépésre. További információ: [Kapcsolódási esetek](../articles/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-sql-connect.md#connection-scenarios).
> 
> 

1. Hello Azure portált, válassza ki **virtuális gépek (klasszikus)**.
2. Ezután válassza ki az SQL Server virtuális gépét.
3. Válassza ki **végpontok**, és kattintson a hello **Hozzáadás** hello végpontok panel felső hello gombra.
   
    ![A végpontlétrehozás lépései a Portalon](./media/virtual-machines-sql-server-connection-steps/portal-endpoint-creation.png)
4. A hello **végpont hozzáadása** panelen adjon meg egy **neve** például SQLEndpoint.
5. Válassza ki **TCP** a hello **protokoll**.
6. **Nyilvános port** esetén adjon meg egy portszámot, például **57500**.
7. A **magánhálózati port**, adja meg az SQL Server figyelő portja, alapértelmezett értéke túl**1433**.
8. Kattintson a **Ok** toocreate hello végpont.

