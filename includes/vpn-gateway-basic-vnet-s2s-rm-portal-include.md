egy VNet hello Resource Manager üzembe helyezési modellel hello Azure-portál használatával toocreate kövesse hello lépéseket. Használjon hello [példaértékeket](#values) Ha használja ezeket a lépéseket egy oktatóanyag. Ha nem ezeket a lépéseket tennie, mint egy oktatóanyag, lehet, hogy tooreplace hello értékeket a saját. A virtuális hálózatok használatáról további információkért lásd: hello [virtuális hálózat áttekintése](../articles/virtual-network/virtual-networks-overview.md).

1. Egy böngészőből keresse meg a toohello [Azure-portálon](http://portal.azure.com) és jelentkezzen be az Azure-fiókjával.
2. Kattintson az **Új** lehetőségre. A hello **hello a piactéren** mezőbe írja be a "Virtuális hálózat". Keresse meg **virtuális hálózati** hello adott vissza a listán, és kattintson a tooopen hello **virtuális hálózati** panelen.
3. Hello alsó részén hello virtuális hálózat panel hello a közelében **telepítési modell kiválasztása** listára, válassza ki **erőforrás-kezelő**, és kattintson a **létrehozása**. Ekkor megnyílik a hello "Virtuális hálózat létrehozása" tartalmazó panel.

    ![Virtuális hálózat létrehozása panel](./media/vpn-gateway-basic-vnet-s2s-rm-portal-include/createvnet.png "Virtuális hálózat létrehozása panel")
4. A hello **virtuális hálózat létrehozása** panelen hello hálózatok beállításainak konfigurálása. Hello mezők kitöltésekor hello piros felkiáltójel kerül, egy zöld pipa Ha hello mezőben megadott hello karakterek érvényesek.

  - **Név**: Adja meg a virtuális hálózat hello nevét. Ebben a példában a TestVNet1 nevet használjuk.
  - **Címtér**: hello címterület adja meg. Ha több címet szóközöket tooadd, adja hozzá az első címterét. Hozzáadhat további címterületeket később hello VNet létrehozása után. Győződjön meg arról, hogy nem fedi át a helyszíni hely hello címterület megadott hello címterület.
  - **Alhálózati név**: tartomány hozzáadása a hello első alhálózat nevét és az alhálózati cím. Hozzáadhat további alhálózatokat és hello átjáróalhálózatot később, ez a virtuális hálózat létrehozása után. 
  - **Előfizetés**: Győződjön meg arról, hogy a felsorolt hello előfizetés van hello helyes névre. Előfizetések hello legördülő lista használatával módosíthatja.
  - **Erőforráscsoport**: válasszon ki egy meglévő erőforráscsoportot, vagy hozzon létre egy újat az új erőforráscsoport nevének beírásával. Új csoport létrehozásakor nevet hello erőforráscsoport tooyour megfelelően tervezett konfigurációs értékeket. További információ az erőforráscsoportokkal kapcsolatban: [Azure Resource Manager Overview](../articles/azure-resource-manager/resource-group-overview.md#resource-groups) (Az Azure Resource Manager áttekintése).
  - **Hely**: válassza ki a virtuális hálózat hello helyét. hello hely határozza meg, központi telepítését toothis VNet hello erőforrások tároló.

5. Válassza ki **PIN-kód toodashboard** Ha szeretné, hogy a virtuális hálózat egyszerűen a hello irányítópulton toobe képes toofind, és kattintson a **létrehozása**. Miután rákattintott **létrehozása**, megjelenik egy csempe az irányítópulton, amely hello a VNet állapotát mutatja. hello csempe változik hello virtuális hálózat létrehozása folyamatban van.
