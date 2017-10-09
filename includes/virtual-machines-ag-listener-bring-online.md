1. <span data-ttu-id="81968-101">A Feladatátvevőfürt-kezelőben bontsa ki a **szerepkörök**, és jelölje ki a rendelkezésre állási csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="81968-101">In Failover Cluster Manager, expand **Roles**, and then highlight your availability group.</span></span>  

2. <span data-ttu-id="81968-102">A hello **erőforrások** fülre, kattintson a jobb gombbal a hello figyelő nevét, majd **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="81968-102">On hello **Resources** tab, right-click hello listener name, and then click **Properties**.</span></span>

3. <span data-ttu-id="81968-103">Kattintson a hello **függőségek** fülre. Ha több erőforrások vannak felsorolva, ellenőrizze, hello IP-címek van-e vagy sem és függőségeit.</span><span class="sxs-lookup"><span data-stu-id="81968-103">Click hello **Dependencies** tab. If multiple resources are listed, verify that hello IP addresses have OR, not AND, dependencies.</span></span>  

4. <span data-ttu-id="81968-104">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="81968-104">Click **OK**.</span></span>

5. <span data-ttu-id="81968-105">Kattintson a jobb gombbal a hello figyelőjének nevével, és kattintson **online állapotba hozás**.</span><span class="sxs-lookup"><span data-stu-id="81968-105">Right-click hello listener name, and then click **Bring Online**.</span></span>

6. <span data-ttu-id="81968-106">Hello után figyelő érhető el, a hello **erőforrások** fülre, kattintson a jobb gombbal a hello rendelkezésre állási csoportot, majd **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="81968-106">After hello listener is online, on hello **Resources** tab, right-click hello availability group, and then click **Properties**.</span></span>
   
    ![Hello rendelkezésre állási csoport erőforrása konfigurálása](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678772.gif)

7. <span data-ttu-id="81968-108">Függőség létrehozása a hello figyelő hálózatnév-erőforrás (nem hello IP-erőforrás nevét), és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="81968-108">Create a dependency on hello listener name resource (not hello IP address resources name), and then click **OK**.</span></span>
   
    ![Adja hozzá a függőség hello figyelő neve](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678773.gif)

8. <span data-ttu-id="81968-110">Indítsa el az SQL Server Management Studio eszközt, és csatlakoztassa a toohello elsődleges másodpéldány.</span><span class="sxs-lookup"><span data-stu-id="81968-110">Start SQL Server Management Studio, and then connect toohello primary replica.</span></span>

9. <span data-ttu-id="81968-111">Nyissa meg túl**AlwaysOn magas rendelkezésre állású** > **rendelkezésre állási csoportok** > **\<AvailabilityGroupName\>**   >  **Rendelkezésre állási csoport figyelői**.</span><span class="sxs-lookup"><span data-stu-id="81968-111">Go too**AlwaysOn High Availability** > **Availability Groups** > **\<AvailabilityGroupName\>** > **Availability Group Listeners**.</span></span>  
    <span data-ttu-id="81968-112">hello figyelő nevét, amelyet a Feladatátvevőfürt-kezelőt hozott létre üzenetnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="81968-112">hello listener name that you created in Failover Cluster Manager should be displayed.</span></span>

10. <span data-ttu-id="81968-113">Kattintson a jobb gombbal a hello figyelőjének nevével, és kattintson **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="81968-113">Right-click hello listener name, and then click **Properties**.</span></span>

11. <span data-ttu-id="81968-114">A hello **Port** hello rendelkezésre állási csoport figyelőjének hello portszáma hello segítségével adja meg a korábban használt $EndpointPort (ebben az oktatóanyagban a 1433-as volt hello alapértelmezett), és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="81968-114">In hello **Port** box, specify hello port number for hello availability group listener by using hello $EndpointPort that you used earlier (in this tutorial, 1433 was hello default), and then click **OK**.</span></span>

