<span data-ttu-id="dde61-101">hello rendelkezésre állási csoport figyelőjének az SQL Server rendelkezésre állási csoport figyeli hello egy IP-cím és a hálózati nevet.</span><span class="sxs-lookup"><span data-stu-id="dde61-101">hello availability group listener is an IP address and network name that hello SQL Server availability group listens on.</span></span> <span data-ttu-id="dde61-102">toocreate hello rendelkezésre állási csoport figyelőjének hello a következő:</span><span class="sxs-lookup"><span data-stu-id="dde61-102">toocreate hello availability group listener, do hello following:</span></span>

1. <span data-ttu-id="dde61-103"><a name="getnet"></a>Hello fürt hálózati erőforrás hello nevének beolvasása.</span><span class="sxs-lookup"><span data-stu-id="dde61-103"><a name="getnet"></a>Get hello name of hello cluster network resource.</span></span>

    <span data-ttu-id="dde61-104">a.</span><span class="sxs-lookup"><span data-stu-id="dde61-104">a.</span></span> <span data-ttu-id="dde61-105">RDP tooconnect toohello üzemeltető hello elsődleges replika Azure virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="dde61-105">Use RDP tooconnect toohello Azure virtual machine that hosts hello primary replica.</span></span> 

    <span data-ttu-id="dde61-106">b.</span><span class="sxs-lookup"><span data-stu-id="dde61-106">b.</span></span> <span data-ttu-id="dde61-107">Nyissa meg a Feladatátvevőfürt-kezelőt.</span><span class="sxs-lookup"><span data-stu-id="dde61-107">Open Failover Cluster Manager.</span></span>

    <span data-ttu-id="dde61-108">c.</span><span class="sxs-lookup"><span data-stu-id="dde61-108">c.</span></span> <span data-ttu-id="dde61-109">Jelölje be hello **hálózatok** csomópontját, és a Megjegyzés hello fürthálózat nevének.</span><span class="sxs-lookup"><span data-stu-id="dde61-109">Select hello **Networks** node, and note hello cluster network name.</span></span> <span data-ttu-id="dde61-110">Használja ezt a nevet a hello `$ClusterNetworkName` változóját hello PowerShell-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="dde61-110">Use this name in hello `$ClusterNetworkName` variable in hello PowerShell script.</span></span> <span data-ttu-id="dde61-111">Hello a következő kép hello fürthálózat nevének a **fürt hálózati 1**:</span><span class="sxs-lookup"><span data-stu-id="dde61-111">In hello following image hello cluster network name is **Cluster Network 1**:</span></span>

   ![Fürt hálózati név](./media/virtual-machines-ag-listener-configure/90-clusternetworkname.png)

2. <span data-ttu-id="dde61-113"><a name="addcap"></a>Hello ügyfél-hozzáférési pont hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="dde61-113"><a name="addcap"></a>Add hello client access point.</span></span>  
    <span data-ttu-id="dde61-114">ügyfél-hozzáférési pont hello hello hálózatnév használó alkalmazások tooconnect toohello adatbázis egy rendelkezésre állási csoportban.</span><span class="sxs-lookup"><span data-stu-id="dde61-114">hello client access point is hello network name that applications use tooconnect toohello databases in an availability group.</span></span> <span data-ttu-id="dde61-115">Hello ügyfél-hozzáférési pont létrehozása a Feladatátvevőfürt-kezelőben.</span><span class="sxs-lookup"><span data-stu-id="dde61-115">Create hello client access point in Failover Cluster Manager.</span></span>

    <span data-ttu-id="dde61-116">a.</span><span class="sxs-lookup"><span data-stu-id="dde61-116">a.</span></span> <span data-ttu-id="dde61-117">Bontsa ki a hello fürt nevét, majd **szerepkörök**.</span><span class="sxs-lookup"><span data-stu-id="dde61-117">Expand hello cluster name, and then click **Roles**.</span></span>

    <span data-ttu-id="dde61-118">b.</span><span class="sxs-lookup"><span data-stu-id="dde61-118">b.</span></span> <span data-ttu-id="dde61-119">A hello **szerepkörök** ablaktáblában kattintson a jobb gombbal hello rendelkezésre állási csoport nevét, és jelölje ki **erőforrás hozzáadása** > **ügyfél-hozzáférési pont**.</span><span class="sxs-lookup"><span data-stu-id="dde61-119">In hello **Roles** pane, right-click hello availability group name, and then select **Add Resource** > **Client Access Point**.</span></span>

   ![Ügyfél-hozzáférési pont](./media/virtual-machines-ag-listener-configure/92-addclientaccesspoint.png)

    <span data-ttu-id="dde61-121">c.</span><span class="sxs-lookup"><span data-stu-id="dde61-121">c.</span></span> <span data-ttu-id="dde61-122">A hello **neve** hozzon létre egy nevet az új figyelőt.</span><span class="sxs-lookup"><span data-stu-id="dde61-122">In hello **Name** box, create a name for this new listener.</span></span> 
   <span data-ttu-id="dde61-123">hello új figyelő hello neve: hello hálózatnév használó alkalmazások tooconnect toodatabases hello SQL Server rendelkezésre állási csoportban.</span><span class="sxs-lookup"><span data-stu-id="dde61-123">hello name for hello new listener is hello network name that applications use tooconnect toodatabases in hello SQL Server availability group.</span></span>
   
    <span data-ttu-id="dde61-124">d.</span><span class="sxs-lookup"><span data-stu-id="dde61-124">d.</span></span> <span data-ttu-id="dde61-125">toofinish létrehozása hello figyelőt, kattintson a **következő** kétszer, majd **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="dde61-125">toofinish creating hello listener, click **Next** twice, and then click **Finish**.</span></span> <span data-ttu-id="dde61-126">Nem kapcsolja a hello figyelő vagy erőforrás online ezen a ponton.</span><span class="sxs-lookup"><span data-stu-id="dde61-126">Do not bring hello listener or resource online at this point.</span></span>

3. <span data-ttu-id="dde61-127"><a name="congroup"></a>Hello IP-erőforrás hello rendelkezésre állási csoport konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="dde61-127"><a name="congroup"></a>Configure hello IP resource for hello availability group.</span></span>

    <span data-ttu-id="dde61-128">a.</span><span class="sxs-lookup"><span data-stu-id="dde61-128">a.</span></span> <span data-ttu-id="dde61-129">Kattintson a hello **erőforrások** lapot, és bontsa ki a létrehozott hello ügyfél-csatlakozási pont.</span><span class="sxs-lookup"><span data-stu-id="dde61-129">Click hello **Resources** tab, and then expand hello client access point you created.</span></span>  
    <span data-ttu-id="dde61-130">hello ügyfél-hozzáférési pont offline állapotban.</span><span class="sxs-lookup"><span data-stu-id="dde61-130">hello client access point is offline.</span></span>

   ![Ügyfél-hozzáférési pont](./media/virtual-machines-ag-listener-configure/94-newclientaccesspoint.png) 

    <span data-ttu-id="dde61-132">b.</span><span class="sxs-lookup"><span data-stu-id="dde61-132">b.</span></span> <span data-ttu-id="dde61-133">Kattintson a jobb gombbal a hello IP-erőforrás, és kattintson a tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="dde61-133">Right-click hello IP resource, and then click properties.</span></span> <span data-ttu-id="dde61-134">Jegyezze fel hello IP-cím hello nevét, és felhasználhatja őket a hello `$IPResourceName` változóját hello PowerShell-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="dde61-134">Note hello name of hello IP address, and use it in hello `$IPResourceName` variable in hello PowerShell script.</span></span>

    <span data-ttu-id="dde61-135">c.</span><span class="sxs-lookup"><span data-stu-id="dde61-135">c.</span></span> <span data-ttu-id="dde61-136">A **IP-cím**, kattintson a **statikus IP-cím**.</span><span class="sxs-lookup"><span data-stu-id="dde61-136">Under **IP Address**, click **Static IP Address**.</span></span> <span data-ttu-id="dde61-137">Hello IP-címet beállítani a hello azonos cím hello Azure-portálon hello terheléselosztói címet beállításakor használt.</span><span class="sxs-lookup"><span data-stu-id="dde61-137">Set hello IP address as hello same address that you used when you set hello load balancer address on hello Azure portal.</span></span>

   ![IP-erőforrás](./media/virtual-machines-ag-listener-configure/96-ipresource.png) 

    <!-----------------------I don't see this option on server 2016
    1. Disable NetBIOS for this address and click **OK**. Repeat this step for each IP resource if your solution spans multiple Azure VNets. 
    ------------------------->

4. <span data-ttu-id="dde61-139"><a name = "dependencyGroup"></a>Tegyen hello SQL Server rendelkezésre állási csoport erőforrása hello ügyfél-csatlakozási pont.</span><span class="sxs-lookup"><span data-stu-id="dde61-139"><a name = "dependencyGroup"></a>Make hello SQL Server availability group resource dependent on hello client access point.</span></span>

    <span data-ttu-id="dde61-140">a.</span><span class="sxs-lookup"><span data-stu-id="dde61-140">a.</span></span> <span data-ttu-id="dde61-141">A Feladatátvevőfürt-kezelőben kattintson **szerepkörök**, majd kattintson a rendelkezésre állási csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="dde61-141">In Failover Cluster Manager, click **Roles**, and then click your availability group.</span></span>

    <span data-ttu-id="dde61-142">b.</span><span class="sxs-lookup"><span data-stu-id="dde61-142">b.</span></span> <span data-ttu-id="dde61-143">A hello **erőforrások** lap **egyéb erőforrások**, kattintson a jobb gombbal a hello rendelkezésre állási erőforráscsoport, és kattintson a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="dde61-143">On hello **Resources** tab, under **Other Resources**, right-click hello availability resource group, and then click **Properties**.</span></span> 

    <span data-ttu-id="dde61-144">c.</span><span class="sxs-lookup"><span data-stu-id="dde61-144">c.</span></span> <span data-ttu-id="dde61-145">Hello függőségek lapon vegye fel hello ügyfél hozzáférési pont (hello figyelő) erőforrás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="dde61-145">On hello dependencies tab, add hello name of hello client access point (hello listener) resource.</span></span>

   ![IP-erőforrás](./media/virtual-machines-ag-listener-configure/97-propertiesdependencies.png) 

    <span data-ttu-id="dde61-147">d.</span><span class="sxs-lookup"><span data-stu-id="dde61-147">d.</span></span> <span data-ttu-id="dde61-148">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="dde61-148">Click **OK**.</span></span>

5. <span data-ttu-id="dde61-149"><a name="listname"></a>Ellenőrizze a hello ügyfél-hozzáférési pont hello IP-címtől függő erőforrás.</span><span class="sxs-lookup"><span data-stu-id="dde61-149"><a name="listname"></a>Make hello client access point resource dependent on hello IP address.</span></span>

    <span data-ttu-id="dde61-150">a.</span><span class="sxs-lookup"><span data-stu-id="dde61-150">a.</span></span> <span data-ttu-id="dde61-151">A Feladatátvevőfürt-kezelőben kattintson **szerepkörök**, majd kattintson a rendelkezésre állási csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="dde61-151">In Failover Cluster Manager, click **Roles**, and then click your availability group.</span></span> 

    <span data-ttu-id="dde61-152">b.</span><span class="sxs-lookup"><span data-stu-id="dde61-152">b.</span></span> <span data-ttu-id="dde61-153">A hello **erőforrások** lapra, kattintson a jobb gombbal a hello ügyfél hozzáférési pont erőforrásra **kiszolgálónév**, és kattintson a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="dde61-153">On hello **Resources** tab, right-click hello client access point resource under **Server Name**, and then click **Properties**.</span></span> 

   ![IP-erőforrás](./media/virtual-machines-ag-listener-configure/98-dependencies.png) 

    <span data-ttu-id="dde61-155">c.</span><span class="sxs-lookup"><span data-stu-id="dde61-155">c.</span></span> <span data-ttu-id="dde61-156">Kattintson a hello **függőségek** fülre. Győződjön meg arról, hogy a hello IP-cím egy függőséget.</span><span class="sxs-lookup"><span data-stu-id="dde61-156">Click hello **Dependencies** tab. Verify that hello IP address is a dependency.</span></span> <span data-ttu-id="dde61-157">Ha nem, függőség beállítása hello IP-címet.</span><span class="sxs-lookup"><span data-stu-id="dde61-157">If it is not, set a dependency on hello IP address.</span></span> <span data-ttu-id="dde61-158">Ha nincsenek felsorolva több erőforrást, győződjön meg arról, hello IP-címek vagy nem rendelkezik-e és függőségeit.</span><span class="sxs-lookup"><span data-stu-id="dde61-158">If there are multiple resources listed, verify that hello IP addresses have OR, not AND, dependencies.</span></span> <span data-ttu-id="dde61-159">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="dde61-159">Click **OK**.</span></span> 

   ![IP-erőforrás](./media/virtual-machines-ag-listener-configure/98-propertiesdependencies.png) 

    <span data-ttu-id="dde61-161">d.</span><span class="sxs-lookup"><span data-stu-id="dde61-161">d.</span></span> <span data-ttu-id="dde61-162">Kattintson a jobb gombbal a hello figyelőjének nevével, és kattintson **online állapotba hozás**.</span><span class="sxs-lookup"><span data-stu-id="dde61-162">Right-click hello listener name, and then click **Bring Online**.</span></span> 

    >[!TIP]
    ><span data-ttu-id="dde61-163">Ellenőrizheti, hogy hello függőségek megfelelően vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="dde61-163">You can validate that hello dependencies are correctly configured.</span></span> <span data-ttu-id="dde61-164">Nyissa meg a Feladatátvevőfürt-kezelőben, tooRoles, kattintson a jobb gombbal a hello rendelkezésre állási csoporthoz, kattintson **további műveletek**, és kattintson a **függőségi jelentés megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="dde61-164">In Failover Cluster Manager, go tooRoles, right-click hello availability group, click **More Actions**, and then click  **Show Dependency Report**.</span></span> <span data-ttu-id="dde61-165">Hello függőségek megfelelően vannak konfigurálva, amikor hello rendelkezésre állási csoport hello hálózatnév függ, pedig hello hálózatnév hello IP-cím függ.</span><span class="sxs-lookup"><span data-stu-id="dde61-165">When hello dependencies are correctly configured, hello availability group is dependent on hello network name, and hello network name is dependent on hello IP address.</span></span> 


6. <span data-ttu-id="dde61-166"><a name="setparam"></a>Hello fürt paraméterek beállítása a PowerShellben.</span><span class="sxs-lookup"><span data-stu-id="dde61-166"><a name="setparam"></a>Set hello cluster parameters in PowerShell.</span></span>
    
    <span data-ttu-id="dde61-167">a.</span><span class="sxs-lookup"><span data-stu-id="dde61-167">a.</span></span> <span data-ttu-id="dde61-168">A következő PowerShell-parancsfájl tooone az SQL Server-példányok hello másolja.</span><span class="sxs-lookup"><span data-stu-id="dde61-168">Copy hello following PowerShell script tooone of your SQL Server instances.</span></span> <span data-ttu-id="dde61-169">A környezet hello változók frissítése.</span><span class="sxs-lookup"><span data-stu-id="dde61-169">Update hello variables for your environment.</span></span>     
    
    ```PowerShell
    $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name)
    $IPResourceName = "<IPResourceName>" # hello IP Address resource name
    $ILBIP = “<n.n.n.n>” # hello IP Address of hello Internal Load Balancer (ILB). This is hello static IP address for hello load balancer you configured in hello Azure portal.
    [int]$ProbePort = <nnnnn>
    
    Import-Module FailoverClusters
    
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```

    <span data-ttu-id="dde61-170">b.</span><span class="sxs-lookup"><span data-stu-id="dde61-170">b.</span></span> <span data-ttu-id="dde61-171">Állítsa be a hello Fürtparaméterek hello PowerShell-parancsfájl futtatásával hello fürtcsomópontok egyike.</span><span class="sxs-lookup"><span data-stu-id="dde61-171">Set hello cluster parameters by running hello PowerShell script on one of hello cluster nodes.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="dde61-172">Ha az SQL Server-példány külön régióban, kétszer kell toorun hello PowerShell-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="dde61-172">If your SQL Server instances are in separate regions, you need toorun hello PowerShell script twice.</span></span> <span data-ttu-id="dde61-173">első alkalommal hello, használja a hello `$ILBIP` és `$ProbePort` hello első régióban.</span><span class="sxs-lookup"><span data-stu-id="dde61-173">hello first time, use hello `$ILBIP` and `$ProbePort` from hello first region.</span></span> <span data-ttu-id="dde61-174">hello másodszor, használja a hello `$ILBIP` és `$ProbePort` hello második régióban.</span><span class="sxs-lookup"><span data-stu-id="dde61-174">hello second time, use hello `$ILBIP` and `$ProbePort` from hello second region.</span></span> <span data-ttu-id="dde61-175">hello fürt hálózati és hello fürt IP-erőforrás neve hello azonos történik.</span><span class="sxs-lookup"><span data-stu-id="dde61-175">hello cluster network name and hello cluster IP resource name are hello same.</span></span> 
