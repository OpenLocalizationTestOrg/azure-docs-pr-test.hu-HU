## <a name="how-toocreate-a-classic-vnet-in-hello-azure-portal"></a>Hogyan toocreate egy klasszikus virtuális hálózatot a hello Azure-portálon
a fenti hello forgatókönyv alapján klasszikus VNet toocreate kövesse hello lépéseket.

1. Egy böngészőből keresse meg a toohttp://portal.azure.com, és ha szükséges, jelentkezzen be az Azure-fiókjával.
2. Kattintson a **új** > **hálózati** > **virtuális hálózati**, figyelje meg, hogy hello **telepítési modell kiválasztása** lista már bemutatja **klasszikus**, és kattintson a **létrehozása**, hello az alábbi ábrán látható módon.
   
    ![VNet létrehozása az Azure Portalon](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)
3. A hello **virtuális hálózati** panelen, a típus hello **neve** hello virtuális hálózaton, és kattintson a **Címtéren**. Konfigurálja a hello VNet és az első alhálózati cím terület beállításait, majd kattintson **OK**. hello alábbi ábrán a mi esetünkben hello CIDR blokk beállításait.
   
    ![Cím terület panel](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)
4. Kattintson a **erőforráscsoport** , és válassza a erőforrás csoport tooadd hello VNet, vagy kattintson a **hozzon létre új erőforráscsoport** tooadd hello VNet tooa új erőforráscsoportot. hello az alábbi ábra hello erőforrás nevű új erőforráscsoport tartozó beállítások **TestRG**. További információ az erőforráscsoportokkal kapcsolatban: [Azure Resource Manager Overview](../articles/azure-resource-manager/resource-group-overview.md#resource-groups) (Az Azure Resource Manager áttekintése).
   
    ![Erőforráscsoport panel létrehozása](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)
5. Szükség esetén módosítsa a hello **előfizetés** és **hely** a VNet beállításait. 
6. Ha nem szeretné, hogy toosee hello VNet csempe megjelenjen hello **Kezdőpulton**, tiltsa le a **PIN-kód tooStartboard**. 
7. Kattintson a **létrehozása** és a hirdetmény hello csempe **virtuális hálózat létrehozása** a hello az alábbi ábrán látható módon.
   
    ![VNet létrehozása a portálon](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)
8. Várjon, amíg hello létrehozott virtuális hálózat toobe, és amikor megjelenik a csempe alatt, kattintson rá a tooadd hello további alhálózatokat.
   
    ![VNet létrehozása a portálon](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)
9. Megtekintheti az hello **konfigurációs** a vnet alább látható módon. 
   
    ![VNet létrehozása a portálon](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)
10. Kattintson a **alhálózatok** > **hozzáadása**, írja be a **neve** , és adja meg egy **-címtartományt (CIDR-blokkja)** az alhálózathoz, majd Kattintson a **OK**. hello az alábbi ábra aktuális esetünkben hello-beállítások.
    
    ![VNet létrehozása az Azure Portalon](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)

