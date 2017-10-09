Ebben a lépésben kézzel létrehozhat hello rendelkezésre állási csoport figyelőjének a Feladatátvevőfürt-kezelő és az SQL Server Management Studio.

1. Nyissa meg a Feladatátvevőfürt-kezelő hello elsődleges másodpéldányt futtató hello csomópontból.

2. Jelölje be hello **hálózatok** csomópontot, majd a Megjegyzés hello fürthálózat nevének. Ez a név a PowerShell-parancsfájl hello hello $ClusterNetworkName változó használatos.

3. Bontsa ki a hello fürt nevét, majd **szerepkörök**.

4. A hello **szerepkörök** ablaktáblában kattintson a jobb gombbal hello rendelkezésre állási csoport nevét, és jelölje ki **erőforrás hozzáadása** > **ügyfél-hozzáférési pont**.
   
    ![Ügyfél-hozzáférési pont rendelkezésre állási csoport hozzáadása](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678769.gif)

5. A hello **neve** mezőben hozzon létre egy nevet az új figyelőt, kattintson a **tovább** kétszer, majd **Befejezés**.  
    Nem kapcsolja a hello figyelő vagy erőforrás online ezen a ponton.

6. Kattintson a hello **erőforrások** lapon, majd bontsa ki az imént létrehozott hello ügyfél-csatlakozási pont. 
    hello IP-cím erőforrás a fürtön minden fürthálózat jelenik meg. Ha ez egy csak Azure megoldás, csak egy IP-cím erőforrás jelenik meg.

7. Hello alábbi módszerekkel:
   
   * hibrid megoldás tooconfigure:
     
        a. Kattintson a jobb gombbal a hello IP-cím erőforrás tooyour helyszíni alhálózati megfelelő, majd válassza ki **tulajdonságok**. Megjegyzés: hello IP-cím neve és hálózati névtől.
   
        b. Válassza ki **statikus IP-cím**, nem használt IP-címet, és kattintson a **OK**.
 
   * egy Azure-csak megoldás tooconfigure:

        a. Kattintson a jobb gombbal a hello IP-cím erőforrás tooyour Azure alhálózatban megfelelő, majd válassza ki **tulajdonságok**.
       
       > [!NOTE]
       > Ha hello figyelő toocome online később a DHCP által kijelölt ütköző IP-cím miatt nem sikerül, konfigurálhatja a egy érvényes statikus IP-címet a Tulajdonságok ablakban.
       > 
       > 

       b. Az azonos hello **IP-cím** tulajdonságai ablakban, a módosítás hello **IP-cím neve**.  
        Ez a név szerepel hello $IPResourceName változó hello PowerShell-parancsfájlt. Ha a megoldás több Azure virtuális hálózat is, ismételje meg ezt a lépést minden IP-erőforrás.

