1. A Feladatátvevőfürt-kezelőben bontsa ki a **szerepkörök**, és jelölje ki a rendelkezésre állási csoporthoz.  

2. A hello **erőforrások** fülre, kattintson a jobb gombbal a hello figyelő nevét, majd **tulajdonságok**.

3. Kattintson a hello **függőségek** fülre. Ha több erőforrások vannak felsorolva, ellenőrizze, hello IP-címek van-e vagy sem és függőségeit.  

4. Kattintson az **OK** gombra.

5. Kattintson a jobb gombbal a hello figyelőjének nevével, és kattintson **online állapotba hozás**.

6. Hello után figyelő érhető el, a hello **erőforrások** fülre, kattintson a jobb gombbal a hello rendelkezésre állási csoportot, majd **tulajdonságok**.
   
    ![Hello rendelkezésre állási csoport erőforrása konfigurálása](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678772.gif)

7. Függőség létrehozása a hello figyelő hálózatnév-erőforrás (nem hello IP-erőforrás nevét), és kattintson a **OK**.
   
    ![Adja hozzá a függőség hello figyelő neve](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678773.gif)

8. Indítsa el az SQL Server Management Studio eszközt, és csatlakoztassa a toohello elsődleges másodpéldány.

9. Nyissa meg túl**AlwaysOn magas rendelkezésre állású** > **rendelkezésre állási csoportok** > **\<AvailabilityGroupName\>**   >  **Rendelkezésre állási csoport figyelői**.  
    hello figyelő nevét, amelyet a Feladatátvevőfürt-kezelőt hozott létre üzenetnek kell megjelennie.

10. Kattintson a jobb gombbal a hello figyelőjének nevével, és kattintson **tulajdonságok**.

11. A hello **Port** hello rendelkezésre állási csoport figyelőjének hello portszáma hello segítségével adja meg a korábban használt $EndpointPort (ebben az oktatóanyagban a 1433-as volt hello alapértelmezett), és kattintson a **OK**.

