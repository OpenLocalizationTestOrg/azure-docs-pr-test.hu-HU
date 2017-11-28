<span data-ttu-id="e147c-101">A rendelkezésre állási csoport figyelőjének egy IP-cím és a hálózati nevet, amely az SQL Server rendelkezésre állási csoport figyeli.</span><span class="sxs-lookup"><span data-stu-id="e147c-101">The availability group listener is an IP address and network name that the SQL Server availability group listens on.</span></span> <span data-ttu-id="e147c-102">A rendelkezésre állási csoport figyelőjének létrehozásához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="e147c-102">To create the availability group listener, do the following:</span></span>

1. <span data-ttu-id="e147c-103"><a name="getnet"></a>A fürt hálózati erőforrás nevének beolvasása.</span><span class="sxs-lookup"><span data-stu-id="e147c-103"><a name="getnet"></a>Get the name of the cluster network resource.</span></span>

    <span data-ttu-id="e147c-104">a.</span><span class="sxs-lookup"><span data-stu-id="e147c-104">a.</span></span> <span data-ttu-id="e147c-105">RDP segítségével csatlakozzon az Azure virtuális géphez, amelyen az elsődleges replika.</span><span class="sxs-lookup"><span data-stu-id="e147c-105">Use RDP to connect to the Azure virtual machine that hosts the primary replica.</span></span> 

    <span data-ttu-id="e147c-106">b.</span><span class="sxs-lookup"><span data-stu-id="e147c-106">b.</span></span> <span data-ttu-id="e147c-107">Nyissa meg a Feladatátvevőfürt-kezelőt.</span><span class="sxs-lookup"><span data-stu-id="e147c-107">Open Failover Cluster Manager.</span></span>

    <span data-ttu-id="e147c-108">c.</span><span class="sxs-lookup"><span data-stu-id="e147c-108">c.</span></span> <span data-ttu-id="e147c-109">Válassza ki a **hálózatok** csomópont, és figyelje meg a fürt hálózati nevét.</span><span class="sxs-lookup"><span data-stu-id="e147c-109">Select the **Networks** node, and note the cluster network name.</span></span> <span data-ttu-id="e147c-110">Ennek a névnek a `$ClusterNetworkName` változó a PowerShell parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="e147c-110">Use this name in the `$ClusterNetworkName` variable in the PowerShell script.</span></span> <span data-ttu-id="e147c-111">Az alábbi ábrán a hálózati fürtnév **fürt hálózati 1**:</span><span class="sxs-lookup"><span data-stu-id="e147c-111">In the following image the cluster network name is **Cluster Network 1**:</span></span>

   ![Fürt hálózati név](./media/virtual-machines-ag-listener-configure/90-clusternetworkname.png)

2. <span data-ttu-id="e147c-113"><a name="addcap"></a>Az ügyfél-hozzáférési pont hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="e147c-113"><a name="addcap"></a>Add the client access point.</span></span>  
    <span data-ttu-id="e147c-114">Az ügyfél-hozzáférési pont a hálózatnév az adatbázisokat a rendelkezésre állási csoportban való kapcsolódáshoz használt alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="e147c-114">The client access point is the network name that applications use to connect to the databases in an availability group.</span></span> <span data-ttu-id="e147c-115">Az ügyfél hozzáférési pontjának létrehozása a Feladatátvevőfürt-kezelőben.</span><span class="sxs-lookup"><span data-stu-id="e147c-115">Create the client access point in Failover Cluster Manager.</span></span>

    <span data-ttu-id="e147c-116">a.</span><span class="sxs-lookup"><span data-stu-id="e147c-116">a.</span></span> <span data-ttu-id="e147c-117">Bontsa ki a fürt nevét, és kattintson a **szerepkörök**.</span><span class="sxs-lookup"><span data-stu-id="e147c-117">Expand the cluster name, and then click **Roles**.</span></span>

    <span data-ttu-id="e147c-118">b.</span><span class="sxs-lookup"><span data-stu-id="e147c-118">b.</span></span> <span data-ttu-id="e147c-119">Az a **szerepkörök** ablaktáblájában kattintson a jobb gombbal a rendelkezésre állási csoport nevét, majd válassza ki **erőforrás hozzáadása** > **ügyfél-hozzáférési pont**.</span><span class="sxs-lookup"><span data-stu-id="e147c-119">In the **Roles** pane, right-click the availability group name, and then select **Add Resource** > **Client Access Point**.</span></span>

   ![Ügyfél-hozzáférési pont](./media/virtual-machines-ag-listener-configure/92-addclientaccesspoint.png)

    <span data-ttu-id="e147c-121">c.</span><span class="sxs-lookup"><span data-stu-id="e147c-121">c.</span></span> <span data-ttu-id="e147c-122">Az a **neve** hozzon létre egy nevet az új figyelőt.</span><span class="sxs-lookup"><span data-stu-id="e147c-122">In the **Name** box, create a name for this new listener.</span></span> 
   <span data-ttu-id="e147c-123">Az új figyelőt az neve: a hálózat nevét használják az SQL Server rendelkezésre állási csoportban adatbázisaihoz való kapcsolódásra.</span><span class="sxs-lookup"><span data-stu-id="e147c-123">The name for the new listener is the network name that applications use to connect to databases in the SQL Server availability group.</span></span>
   
    <span data-ttu-id="e147c-124">d.</span><span class="sxs-lookup"><span data-stu-id="e147c-124">d.</span></span> <span data-ttu-id="e147c-125">A figyelő létrehozásának befejezéséhez kattintson **következő** kétszer, majd **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="e147c-125">To finish creating the listener, click **Next** twice, and then click **Finish**.</span></span> <span data-ttu-id="e147c-126">Nem kapcsolja a figyelő vagy erőforrás online ezen a ponton.</span><span class="sxs-lookup"><span data-stu-id="e147c-126">Do not bring the listener or resource online at this point.</span></span>

3. <span data-ttu-id="e147c-127"><a name="congroup"></a>Az IP-erőforrás a rendelkezésre állási csoport konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e147c-127"><a name="congroup"></a>Configure the IP resource for the availability group.</span></span>

    <span data-ttu-id="e147c-128">a.</span><span class="sxs-lookup"><span data-stu-id="e147c-128">a.</span></span> <span data-ttu-id="e147c-129">Kattintson a **erőforrások** lapot, és bontsa ki a létrehozott ügyfél-hozzáférési pontját.</span><span class="sxs-lookup"><span data-stu-id="e147c-129">Click the **Resources** tab, and then expand the client access point you created.</span></span>  
    <span data-ttu-id="e147c-130">Az ügyfél-hozzáférési pont offline állapotban.</span><span class="sxs-lookup"><span data-stu-id="e147c-130">The client access point is offline.</span></span>

   ![Ügyfél-hozzáférési pont](./media/virtual-machines-ag-listener-configure/94-newclientaccesspoint.png) 

    <span data-ttu-id="e147c-132">b.</span><span class="sxs-lookup"><span data-stu-id="e147c-132">b.</span></span> <span data-ttu-id="e147c-133">Kattintson a jobb gombbal az IP-erőforrás, és kattintson a tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="e147c-133">Right-click the IP resource, and then click properties.</span></span> <span data-ttu-id="e147c-134">Jegyezze fel az IP-címet, és ezért a `$IPResourceName` változó a PowerShell parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="e147c-134">Note the name of the IP address, and use it in the `$IPResourceName` variable in the PowerShell script.</span></span>

    <span data-ttu-id="e147c-135">c.</span><span class="sxs-lookup"><span data-stu-id="e147c-135">c.</span></span> <span data-ttu-id="e147c-136">A **IP-cím**, kattintson a **statikus IP-cím**.</span><span class="sxs-lookup"><span data-stu-id="e147c-136">Under **IP Address**, click **Static IP Address**.</span></span> <span data-ttu-id="e147c-137">Állítsa be az IP-cím a címmel, ha úgy állítja be a terheléselosztói címet, az Azure portálon használt.</span><span class="sxs-lookup"><span data-stu-id="e147c-137">Set the IP address as the same address that you used when you set the load balancer address on the Azure portal.</span></span>

   ![IP-erőforrás](./media/virtual-machines-ag-listener-configure/96-ipresource.png) 

    <!-----------------------I don't see this option on server 2016
    1. Disable NetBIOS for this address and click **OK**. Repeat this step for each IP resource if your solution spans multiple Azure VNets. 
    ------------------------->

4. <span data-ttu-id="e147c-139"><a name = "dependencyGroup"></a>Ellenőrizze az SQL Server rendelkezésre állási csoport erőforrása függ, az ügyfél-csatlakozási pont.</span><span class="sxs-lookup"><span data-stu-id="e147c-139"><a name = "dependencyGroup"></a>Make the SQL Server availability group resource dependent on the client access point.</span></span>

    <span data-ttu-id="e147c-140">a.</span><span class="sxs-lookup"><span data-stu-id="e147c-140">a.</span></span> <span data-ttu-id="e147c-141">A Feladatátvevőfürt-kezelőben kattintson **szerepkörök**, majd kattintson a rendelkezésre állási csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="e147c-141">In Failover Cluster Manager, click **Roles**, and then click your availability group.</span></span>

    <span data-ttu-id="e147c-142">b.</span><span class="sxs-lookup"><span data-stu-id="e147c-142">b.</span></span> <span data-ttu-id="e147c-143">Az a **erőforrások** lap **egyéb erőforrások**, kattintson a jobb gombbal a rendelkezésre állási csoport, és kattintson a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="e147c-143">On the **Resources** tab, under **Other Resources**, right-click the availability resource group, and then click **Properties**.</span></span> 

    <span data-ttu-id="e147c-144">c.</span><span class="sxs-lookup"><span data-stu-id="e147c-144">c.</span></span> <span data-ttu-id="e147c-145">A függőségek lapon adja meg az ügyfél hozzáférési pont (figyelő) erőforrás nevét.</span><span class="sxs-lookup"><span data-stu-id="e147c-145">On the dependencies tab, add the name of the client access point (the listener) resource.</span></span>

   ![IP-erőforrás](./media/virtual-machines-ag-listener-configure/97-propertiesdependencies.png) 

    <span data-ttu-id="e147c-147">d.</span><span class="sxs-lookup"><span data-stu-id="e147c-147">d.</span></span> <span data-ttu-id="e147c-148">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="e147c-148">Click **OK**.</span></span>

5. <span data-ttu-id="e147c-149"><a name="listname"></a>Ellenőrizze az ügyfél hozzáférési pont erőforrás függ, az IP-címet.</span><span class="sxs-lookup"><span data-stu-id="e147c-149"><a name="listname"></a>Make the client access point resource dependent on the IP address.</span></span>

    <span data-ttu-id="e147c-150">a.</span><span class="sxs-lookup"><span data-stu-id="e147c-150">a.</span></span> <span data-ttu-id="e147c-151">A Feladatátvevőfürt-kezelőben kattintson **szerepkörök**, majd kattintson a rendelkezésre állási csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="e147c-151">In Failover Cluster Manager, click **Roles**, and then click your availability group.</span></span> 

    <span data-ttu-id="e147c-152">b.</span><span class="sxs-lookup"><span data-stu-id="e147c-152">b.</span></span> <span data-ttu-id="e147c-153">Az a **erőforrások** lapra, kattintson a jobb gombbal az ügyfél hozzáférési pont erőforrásra **kiszolgálónév**, és kattintson a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="e147c-153">On the **Resources** tab, right-click the client access point resource under **Server Name**, and then click **Properties**.</span></span> 

   ![IP-erőforrás](./media/virtual-machines-ag-listener-configure/98-dependencies.png) 

    <span data-ttu-id="e147c-155">c.</span><span class="sxs-lookup"><span data-stu-id="e147c-155">c.</span></span> <span data-ttu-id="e147c-156">Kattintson a **függőségek** fülre.</span><span class="sxs-lookup"><span data-stu-id="e147c-156">Click the **Dependencies** tab.</span></span> <span data-ttu-id="e147c-157">Győződjön meg arról, hogy az IP-cím egy függőséget.</span><span class="sxs-lookup"><span data-stu-id="e147c-157">Verify that the IP address is a dependency.</span></span> <span data-ttu-id="e147c-158">Ha nem, függőség beállítása az IP-címet.</span><span class="sxs-lookup"><span data-stu-id="e147c-158">If it is not, set a dependency on the IP address.</span></span> <span data-ttu-id="e147c-159">Ha nincsenek felsorolva több erőforrást, győződjön meg arról, hogy az IP-címek vagy nem rendelkezik-e és függőségeit.</span><span class="sxs-lookup"><span data-stu-id="e147c-159">If there are multiple resources listed, verify that the IP addresses have OR, not AND, dependencies.</span></span> <span data-ttu-id="e147c-160">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="e147c-160">Click **OK**.</span></span> 

   ![IP-erőforrás](./media/virtual-machines-ag-listener-configure/98-propertiesdependencies.png) 

    <span data-ttu-id="e147c-162">d.</span><span class="sxs-lookup"><span data-stu-id="e147c-162">d.</span></span> <span data-ttu-id="e147c-163">Kattintson a jobb gombbal a figyelő nevét, és kattintson **online állapotba hozás**.</span><span class="sxs-lookup"><span data-stu-id="e147c-163">Right-click the listener name, and then click **Bring Online**.</span></span> 

    >[!TIP]
    ><span data-ttu-id="e147c-164">Ellenőrizheti, hogy a függőségek megfelelően vannak-e konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="e147c-164">You can validate that the dependencies are correctly configured.</span></span> <span data-ttu-id="e147c-165">A Feladatátvevőfürt-kezelőben, nyissa meg a szerepkörökhöz, kattintson a jobb gombbal a rendelkezésre állási csoporthoz, kattintson a **további műveletek**, és kattintson a **függőségi jelentés megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="e147c-165">In Failover Cluster Manager, go to Roles, right-click the availability group, click **More Actions**, and then click  **Show Dependency Report**.</span></span> <span data-ttu-id="e147c-166">A függőségek megfelelően vannak konfigurálva, amikor a rendelkezésre állási csoport függ a hálózat neve, pedig a hálózati név függ az IP-címet.</span><span class="sxs-lookup"><span data-stu-id="e147c-166">When the dependencies are correctly configured, the availability group is dependent on the network name, and the network name is dependent on the IP address.</span></span> 


6. <span data-ttu-id="e147c-167"><a name="setparam"></a>A fürt paraméterek beállítása a PowerShellben.</span><span class="sxs-lookup"><span data-stu-id="e147c-167"><a name="setparam"></a>Set the cluster parameters in PowerShell.</span></span>
    
    <span data-ttu-id="e147c-168">a.</span><span class="sxs-lookup"><span data-stu-id="e147c-168">a.</span></span> <span data-ttu-id="e147c-169">Másolja a következő PowerShell-parancsfájlt az SQL Server-példányok közül.</span><span class="sxs-lookup"><span data-stu-id="e147c-169">Copy the following PowerShell script to one of your SQL Server instances.</span></span> <span data-ttu-id="e147c-170">A változók a környezet frissítése.</span><span class="sxs-lookup"><span data-stu-id="e147c-170">Update the variables for your environment.</span></span>     
    
    ```PowerShell
    $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
    $IPResourceName = "<IPResourceName>" # the IP Address resource name
    $ILBIP = “<n.n.n.n>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    [int]$ProbePort = <nnnnn>
    
    Import-Module FailoverClusters
    
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```

    <span data-ttu-id="e147c-171">b.</span><span class="sxs-lookup"><span data-stu-id="e147c-171">b.</span></span> <span data-ttu-id="e147c-172">Állítsa be a fürt paramétereket a PowerShell-parancsfájl futtatásával a fürtcsomópontok egyike.</span><span class="sxs-lookup"><span data-stu-id="e147c-172">Set the cluster parameters by running the PowerShell script on one of the cluster nodes.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="e147c-173">Ha az SQL Server-példány külön régióban, kétszer a PowerShell parancsfájl futtatásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="e147c-173">If your SQL Server instances are in separate regions, you need to run the PowerShell script twice.</span></span> <span data-ttu-id="e147c-174">Az első alkalommal használja a `$ILBIP` és `$ProbePort` első régióban.</span><span class="sxs-lookup"><span data-stu-id="e147c-174">The first time, use the `$ILBIP` and `$ProbePort` from the first region.</span></span> <span data-ttu-id="e147c-175">A második alkalommal használja a `$ILBIP` és `$ProbePort` második régióban.</span><span class="sxs-lookup"><span data-stu-id="e147c-175">The second time, use the `$ILBIP` and `$ProbePort` from the second region.</span></span> <span data-ttu-id="e147c-176">A fürthálózat nevének és a fürt IP-erőforrás neve megegyezik.</span><span class="sxs-lookup"><span data-stu-id="e147c-176">The cluster network name and the cluster IP resource name are the same.</span></span> 
