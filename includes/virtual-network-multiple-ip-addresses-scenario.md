## <a name="scenario"></a>Forgatókönyv
Virtuális gép egyetlen hálózati adapter és a létrehozott és csatlakoztatott tooa virtuális hálózati. virtuális gép hello szükséges három különböző *titkos* IP címeket és a két *nyilvános* IP-címeket. hello IP-címek rendelt IP-konfiguráció a következő toohello:

* **IPConfig-1:** hozzárendel egy *statikus* magánhálózati IP-címet és egy *statikus* nyilvános IP-cím.
* **IPConfig-2:** hozzárendel egy *statikus* magánhálózati IP-címet és egy *statikus* nyilvános IP-cím.
* **IPConfig-3:** hozzárendel egy *statikus* magánhálózati IP-cím és a nyilvános IP-cím.
  
    ![Több IP-cím](./media/virtual-network-multiple-ip-addresses-scenario/multiple-ipconfigs.png)

hello IP-konfigurációk társított toohello hálózati adapter lesz, ha hello NIC jön létre és hello NIC csatolt toohello VM hello virtuális gép létrehozásakor. hello hello forgatókönyvhöz használt IP-címek vannak az ábrán látható. Rendelhet, hogy bármilyen IP cím és a hozzárendelés típusa van szüksége.

> [!NOTE]
> Hello lépéseit, ha ez a cikk rendeli hozzá az összes IP-konfigurációk tooa egyetlen hálózati adapter, több IP-konfigurációk tooany több hálózati Adapterrel virtuális hálózati adapter is hozzárendelhetők. hogyan olvassa el a virtuális gép több hálózati adaptert, és toocreate toolearn hello [több hálózati adapterrel rendelkező virtuális gép létrehozása](../articles/virtual-network/virtual-network-deploy-multinic-arm-ps.md) cikk.
