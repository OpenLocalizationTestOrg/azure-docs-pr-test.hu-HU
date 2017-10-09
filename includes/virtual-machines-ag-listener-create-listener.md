<span data-ttu-id="3ca0e-101">Ebben a lépésben kézzel létrehozhat hello rendelkezésre állási csoport figyelőjének a Feladatátvevőfürt-kezelő és az SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="3ca0e-101">In this step, you manually create hello availability group listener in Failover Cluster Manager and SQL Server Management Studio.</span></span>

1. <span data-ttu-id="3ca0e-102">Nyissa meg a Feladatátvevőfürt-kezelő hello elsődleges másodpéldányt futtató hello csomópontból.</span><span class="sxs-lookup"><span data-stu-id="3ca0e-102">Open Failover Cluster Manager from hello node that hosts hello primary replica.</span></span>

2. <span data-ttu-id="3ca0e-103">Jelölje be hello **hálózatok** csomópontot, majd a Megjegyzés hello fürthálózat nevének.</span><span class="sxs-lookup"><span data-stu-id="3ca0e-103">Select hello **Networks** node, and then note hello cluster network name.</span></span> <span data-ttu-id="3ca0e-104">Ez a név a PowerShell-parancsfájl hello hello $ClusterNetworkName változó használatos.</span><span class="sxs-lookup"><span data-stu-id="3ca0e-104">This name is used in hello $ClusterNetworkName variable in hello PowerShell script.</span></span>

3. <span data-ttu-id="3ca0e-105">Bontsa ki a hello fürt nevét, majd **szerepkörök**.</span><span class="sxs-lookup"><span data-stu-id="3ca0e-105">Expand hello cluster name, and then click **Roles**.</span></span>

4. <span data-ttu-id="3ca0e-106">A hello **szerepkörök** ablaktáblában kattintson a jobb gombbal hello rendelkezésre állási csoport nevét, és jelölje ki **erőforrás hozzáadása** > **ügyfél-hozzáférési pont**.</span><span class="sxs-lookup"><span data-stu-id="3ca0e-106">In hello **Roles** pane, right-click hello availability group name, and then select **Add Resource** > **Client Access Point**.</span></span>
   
    ![Ügyfél-hozzáférési pont rendelkezésre állási csoport hozzáadása](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678769.gif)

5. <span data-ttu-id="3ca0e-108">A hello **neve** mezőben hozzon létre egy nevet az új figyelőt, kattintson a **tovább** kétszer, majd **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="3ca0e-108">In hello **Name** box, create a name for this new listener, click **Next** twice, and then click **Finish**.</span></span>  
    <span data-ttu-id="3ca0e-109">Nem kapcsolja a hello figyelő vagy erőforrás online ezen a ponton.</span><span class="sxs-lookup"><span data-stu-id="3ca0e-109">Do not bring hello listener or resource online at this point.</span></span>

6. <span data-ttu-id="3ca0e-110">Kattintson a hello **erőforrások** lapon, majd bontsa ki az imént létrehozott hello ügyfél-csatlakozási pont.</span><span class="sxs-lookup"><span data-stu-id="3ca0e-110">Click hello **Resources** tab, and then expand hello client access point you just created.</span></span> 
    <span data-ttu-id="3ca0e-111">hello IP-cím erőforrás a fürtön minden fürthálózat jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3ca0e-111">hello IP address resource for each cluster network in your cluster is displayed.</span></span> <span data-ttu-id="3ca0e-112">Ha ez egy csak Azure megoldás, csak egy IP-cím erőforrás jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3ca0e-112">If this is an Azure-only solution, only one IP address resource is displayed.</span></span>

7. <span data-ttu-id="3ca0e-113">Hello alábbi módszerekkel:</span><span class="sxs-lookup"><span data-stu-id="3ca0e-113">Do either of hello following:</span></span>
   
   * <span data-ttu-id="3ca0e-114">hibrid megoldás tooconfigure:</span><span class="sxs-lookup"><span data-stu-id="3ca0e-114">tooconfigure a hybrid solution:</span></span>
     
        <span data-ttu-id="3ca0e-115">a.</span><span class="sxs-lookup"><span data-stu-id="3ca0e-115">a.</span></span> <span data-ttu-id="3ca0e-116">Kattintson a jobb gombbal a hello IP-cím erőforrás tooyour helyszíni alhálózati megfelelő, majd válassza ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="3ca0e-116">Right-click hello IP address resource that corresponds tooyour on-premises subnet, and then select **Properties**.</span></span> <span data-ttu-id="3ca0e-117">Megjegyzés: hello IP-cím neve és hálózati névtől.</span><span class="sxs-lookup"><span data-stu-id="3ca0e-117">Note hello IP address name and network name.</span></span>
   
        <span data-ttu-id="3ca0e-118">b.</span><span class="sxs-lookup"><span data-stu-id="3ca0e-118">b.</span></span> <span data-ttu-id="3ca0e-119">Válassza ki **statikus IP-cím**, nem használt IP-címet, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="3ca0e-119">Select **Static IP Address**, assign an unused IP address, and then click **OK**.</span></span>
 
   * <span data-ttu-id="3ca0e-120">egy Azure-csak megoldás tooconfigure:</span><span class="sxs-lookup"><span data-stu-id="3ca0e-120">tooconfigure an Azure-only solution:</span></span>

        <span data-ttu-id="3ca0e-121">a.</span><span class="sxs-lookup"><span data-stu-id="3ca0e-121">a.</span></span> <span data-ttu-id="3ca0e-122">Kattintson a jobb gombbal a hello IP-cím erőforrás tooyour Azure alhálózatban megfelelő, majd válassza ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="3ca0e-122">Right-click hello IP address resource that corresponds tooyour Azure subnet, and then select **Properties**.</span></span>
       
       > [!NOTE]
       > <span data-ttu-id="3ca0e-123">Ha hello figyelő toocome online később a DHCP által kijelölt ütköző IP-cím miatt nem sikerül, konfigurálhatja a egy érvényes statikus IP-címet a Tulajdonságok ablakban.</span><span class="sxs-lookup"><span data-stu-id="3ca0e-123">If hello listener later fails toocome online because of a conflicting IP address selected by DHCP, you can configure a valid static IP address in this properties window.</span></span>
       > 
       > 

       <span data-ttu-id="3ca0e-124">b.</span><span class="sxs-lookup"><span data-stu-id="3ca0e-124">b.</span></span> <span data-ttu-id="3ca0e-125">Az azonos hello **IP-cím** tulajdonságai ablakban, a módosítás hello **IP-cím neve**.</span><span class="sxs-lookup"><span data-stu-id="3ca0e-125">In hello same **IP Address** properties window, change hello **IP Address Name**.</span></span>  
        <span data-ttu-id="3ca0e-126">Ez a név szerepel hello $IPResourceName változó hello PowerShell-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="3ca0e-126">This name is used in hello $IPResourceName variable of hello PowerShell script.</span></span> <span data-ttu-id="3ca0e-127">Ha a megoldás több Azure virtuális hálózat is, ismételje meg ezt a lépést minden IP-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="3ca0e-127">If your solution spans multiple Azure virtual networks, repeat this step for each IP resource.</span></span>

