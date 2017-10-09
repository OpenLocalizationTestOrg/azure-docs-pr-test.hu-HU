hello rendelkezésre állási csoport figyelőjének az SQL Server rendelkezésre állási csoport figyeli hello egy IP-cím és a hálózati nevet. toocreate hello rendelkezésre állási csoport figyelőjének hello a következő:

1. <a name="getnet"></a>Hello fürt hálózati erőforrás hello nevének beolvasása.

    a. RDP tooconnect toohello üzemeltető hello elsődleges replika Azure virtuális gép használja. 

    b. Nyissa meg a Feladatátvevőfürt-kezelőt.

    c. Jelölje be hello **hálózatok** csomópontját, és a Megjegyzés hello fürthálózat nevének. Használja ezt a nevet a hello `$ClusterNetworkName` változóját hello PowerShell-parancsfájlt. Hello a következő kép hello fürthálózat nevének a **fürt hálózati 1**:

   ![Fürt hálózati név](./media/virtual-machines-ag-listener-configure/90-clusternetworkname.png)

2. <a name="addcap"></a>Hello ügyfél-hozzáférési pont hozzáadása.  
    ügyfél-hozzáférési pont hello hello hálózatnév használó alkalmazások tooconnect toohello adatbázis egy rendelkezésre állási csoportban. Hello ügyfél-hozzáférési pont létrehozása a Feladatátvevőfürt-kezelőben.

    a. Bontsa ki a hello fürt nevét, majd **szerepkörök**.

    b. A hello **szerepkörök** ablaktáblában kattintson a jobb gombbal hello rendelkezésre állási csoport nevét, és jelölje ki **erőforrás hozzáadása** > **ügyfél-hozzáférési pont**.

   ![Ügyfél-hozzáférési pont](./media/virtual-machines-ag-listener-configure/92-addclientaccesspoint.png)

    c. A hello **neve** hozzon létre egy nevet az új figyelőt. 
   hello új figyelő hello neve: hello hálózatnév használó alkalmazások tooconnect toodatabases hello SQL Server rendelkezésre állási csoportban.
   
    d. toofinish létrehozása hello figyelőt, kattintson a **következő** kétszer, majd **Befejezés**. Nem kapcsolja a hello figyelő vagy erőforrás online ezen a ponton.

3. <a name="congroup"></a>Hello IP-erőforrás hello rendelkezésre állási csoport konfigurálása.

    a. Kattintson a hello **erőforrások** lapot, és bontsa ki a létrehozott hello ügyfél-csatlakozási pont.  
    hello ügyfél-hozzáférési pont offline állapotban.

   ![Ügyfél-hozzáférési pont](./media/virtual-machines-ag-listener-configure/94-newclientaccesspoint.png) 

    b. Kattintson a jobb gombbal a hello IP-erőforrás, és kattintson a tulajdonságok. Jegyezze fel hello IP-cím hello nevét, és felhasználhatja őket a hello `$IPResourceName` változóját hello PowerShell-parancsfájlt.

    c. A **IP-cím**, kattintson a **statikus IP-cím**. Hello IP-címet beállítani a hello azonos cím hello Azure-portálon hello terheléselosztói címet beállításakor használt.

   ![IP-erőforrás](./media/virtual-machines-ag-listener-configure/96-ipresource.png) 

    <!-----------------------I don't see this option on server 2016
    1. Disable NetBIOS for this address and click **OK**. Repeat this step for each IP resource if your solution spans multiple Azure VNets. 
    ------------------------->

4. <a name = "dependencyGroup"></a>Tegyen hello SQL Server rendelkezésre állási csoport erőforrása hello ügyfél-csatlakozási pont.

    a. A Feladatátvevőfürt-kezelőben kattintson **szerepkörök**, majd kattintson a rendelkezésre állási csoporthoz.

    b. A hello **erőforrások** lap **egyéb erőforrások**, kattintson a jobb gombbal a hello rendelkezésre állási erőforráscsoport, és kattintson a **tulajdonságok**. 

    c. Hello függőségek lapon vegye fel hello ügyfél hozzáférési pont (hello figyelő) erőforrás hello nevét.

   ![IP-erőforrás](./media/virtual-machines-ag-listener-configure/97-propertiesdependencies.png) 

    d. Kattintson az **OK** gombra.

5. <a name="listname"></a>Ellenőrizze a hello ügyfél-hozzáférési pont hello IP-címtől függő erőforrás.

    a. A Feladatátvevőfürt-kezelőben kattintson **szerepkörök**, majd kattintson a rendelkezésre állási csoporthoz. 

    b. A hello **erőforrások** lapra, kattintson a jobb gombbal a hello ügyfél hozzáférési pont erőforrásra **kiszolgálónév**, és kattintson a **tulajdonságok**. 

   ![IP-erőforrás](./media/virtual-machines-ag-listener-configure/98-dependencies.png) 

    c. Kattintson a hello **függőségek** fülre. Győződjön meg arról, hogy a hello IP-cím egy függőséget. Ha nem, függőség beállítása hello IP-címet. Ha nincsenek felsorolva több erőforrást, győződjön meg arról, hello IP-címek vagy nem rendelkezik-e és függőségeit. Kattintson az **OK** gombra. 

   ![IP-erőforrás](./media/virtual-machines-ag-listener-configure/98-propertiesdependencies.png) 

    d. Kattintson a jobb gombbal a hello figyelőjének nevével, és kattintson **online állapotba hozás**. 

    >[!TIP]
    >Ellenőrizheti, hogy hello függőségek megfelelően vannak konfigurálva. Nyissa meg a Feladatátvevőfürt-kezelőben, tooRoles, kattintson a jobb gombbal a hello rendelkezésre állási csoporthoz, kattintson **további műveletek**, és kattintson a **függőségi jelentés megjelenítése**. Hello függőségek megfelelően vannak konfigurálva, amikor hello rendelkezésre állási csoport hello hálózatnév függ, pedig hello hálózatnév hello IP-cím függ. 


6. <a name="setparam"></a>Hello fürt paraméterek beállítása a PowerShellben.
    
    a. A következő PowerShell-parancsfájl tooone az SQL Server-példányok hello másolja. A környezet hello változók frissítése.     
    
    ```PowerShell
    $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name)
    $IPResourceName = "<IPResourceName>" # hello IP Address resource name
    $ILBIP = “<n.n.n.n>” # hello IP Address of hello Internal Load Balancer (ILB). This is hello static IP address for hello load balancer you configured in hello Azure portal.
    [int]$ProbePort = <nnnnn>
    
    Import-Module FailoverClusters
    
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```

    b. Állítsa be a hello Fürtparaméterek hello PowerShell-parancsfájl futtatásával hello fürtcsomópontok egyike.  

    > [!NOTE]
    > Ha az SQL Server-példány külön régióban, kétszer kell toorun hello PowerShell-parancsfájlt. első alkalommal hello, használja a hello `$ILBIP` és `$ProbePort` hello első régióban. hello másodszor, használja a hello `$ILBIP` és `$ProbePort` hello második régióban. hello fürt hálózati és hello fürt IP-erőforrás neve hello azonos történik. 
