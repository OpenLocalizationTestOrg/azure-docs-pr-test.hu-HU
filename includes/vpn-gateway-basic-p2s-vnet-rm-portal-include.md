egy VNet hello Resource Manager üzembe helyezési modellel hello Azure-portál használatával toocreate kövesse hello lépéseket. hello képernyőképeket példaként szolgálnak. Lehet, hogy tooreplace hello értékeket a saját. A virtuális hálózatok használatáról további információkért lásd: hello [virtuális hálózat áttekintése](../articles/virtual-network/virtual-networks-overview.md).

1. Egy böngészőből keresse meg a toohello [Azure-portálon](http://portal.azure.com) és szükség esetén jelentkezzen be az Azure-fiókjával.
2. Kattintson az **+** elemre. A hello **hello a piactéren** mezőbe írja be a "Virtuális hálózat". Keresse meg **virtuális hálózati** hello adott vissza a listán, és kattintson a tooopen hello **virtuális hálózati** lap.

  ![Virtuális hálózati erőforrás keresése lap](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/newvnetportal700.png "Virtuális hálózati erőforrás keresése lap")
3. Hello virtuális hálózat lap, a hello hello alján közelében **telepítési modell kiválasztása** listára, válassza ki **erőforrás-kezelő**, és kattintson a **létrehozása**.

  ![A Resource Manager kiválasztása](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/resourcemanager250.png "A Resource Manager kiválasztása")
4. A hello **virtuális hálózat létrehozása** lapján hello hálózatok beállításainak konfigurálása. Hello mezők kitöltésekor hello piros felkiáltójel kerül, egy zöld pipa Ha hello mezőben megadott hello karakterek érvényesek. Előfordulhatnak automatikusan kitöltött értékek is. Ha igen, cserélje le hello értékeket a saját. Hello **virtuális hálózat létrehozása** lap a következő példa hasonló toohello néz ki:

  ![Mező érvényesítése](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/createp2sgvnet.png "Mező érvényesítése")
5. **Név**: Adja meg a virtuális hálózat hello nevét.
6. **Címtér**: hello címterület adja meg. Ha több címet szóközöket tooadd, adja hozzá az első címterét. Hozzáadhat további címterületeket később hello VNet létrehozása után.
7. **Alhálózati név**: hello alhálózati név és alhálózati-címtartomány hozzáadása. Hozzáadhat további alhálózatokat később hello VNet létrehozása után.
8. **Előfizetés**: Győződjön meg arról, hogy előfizetés felsorolt hello van hello helyes. Előfizetések hello legördülő lista használatával módosíthatja.
9. **Erőforráscsoport**: válasszon ki egy meglévő erőforráscsoportot, vagy hozzon létre egy újat az új erőforráscsoport nevének beírásával. Új csoport létrehozásakor nevet hello erőforráscsoport tooyour megfelelően tervezett konfigurációs értékeket. További információ az erőforráscsoportokkal kapcsolatban: [Azure Resource Manager Overview](../articles/azure-resource-manager/resource-group-overview.md#resource-groups) (Az Azure Resource Manager áttekintése).
10. **Hely**: válassza ki a virtuális hálózat hello helyét. hello hely határozza meg, központi telepítését toothis VNet hello erőforrások tároló.
11. Válassza ki **PIN-kód toodashboard** Ha szeretné, hogy a virtuális hálózat egyszerűen a hello irányítópulton toobe képes toofind, és kattintson a **létrehozása**.

 ![PIN-kód toodashboard](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/pintodashboard150.png "PIN-kód toodashboard")
12. Miután rákattintott **létrehozása**, megjelenik egy csempe az irányítópulton, amely hello a VNet állapotát mutatja. hello csempe változik hello virtuális hálózat létrehozása folyamatban van.

  ![Virtuális hálózat létrehozása csempe](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/deploying150.png "Virtuális hálózat létrehozása csempe")