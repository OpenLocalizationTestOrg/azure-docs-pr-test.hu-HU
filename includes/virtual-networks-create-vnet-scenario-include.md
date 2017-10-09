## <a name="scenario"></a>Forgatókönyv
toobetter bemutatják, hogyan toocreate egy VNet és alhálózat, ez a dokumentum használja az alábbi hello forgatókönyv.

![VNet-forgatókönyv](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

Ebben az esetben egy **TestVNet** nevű VNetet fog létrehozni a**192.168.0.0./16** egy fenntartott CIDR-blokkjával. A virtuális hálózat hello a következő alhálózatokat fogja tartalmazni: 

* **FrontEnd**, amelynek a CIDR-blokkja **192.168.1.0/24** lesz.
* **BackEnd**, amelynek a CIDR-blokkja **192.168.2.0/24** lesz.

