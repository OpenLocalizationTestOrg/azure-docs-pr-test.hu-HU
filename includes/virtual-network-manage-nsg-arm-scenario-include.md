## <a name="sample-scenario"></a>Mintaforgatókönyv
toobetter bemutatják, hogyan toomanage NSG-ket, ebben a cikkben az alábbi hello forgatókönyvet.

![VNet-forgatókönyv](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

Ebben a forgatókönyvben létrehoz egy NSG-t az egyes alhálózatokon lévő hello **TestVNet** virtuális hálózatban, folyamata az alábbiakban olvasható: 

* **NSG-előtérbeli**. hello előtér NSG lesz alkalmazott toohello *előtér* alhálózatot, és két szabályt tartalmaz:    
  * **RDP-szabály**. Ez a szabály lehetővé teszi az RDP-forgalmát toohello *előtér* alhálózat.
  * **webalkalmazás-szabály**. Ez a szabály lehetővé teszi a HTTP-forgalom toohello *előtér* alhálózat.
* **NSG-háttérrendszer**. hello háttér NSG lesz alkalmazott toohello *háttér* alhálózatot, és két szabályt tartalmaz:    
  * **SQL-szabály**. Ez a szabály lehetővé teszi, hogy csak a hello forgalmát SQL *előtér* alhálózat.
  * **webalkalmazás-szabály**. Ez a szabály letiltja az összes internetes forgalom kötött a hello *háttér* alhálózat.

Ezek a szabályok hello kombinációja DMZ-szerű eset, ha hello háttér alhálózati hello előtér alhálózati csak fogadhat SQL forgalom bejövő forgalmat, és nincs hozzáférési toohello Internet, amíg hello előtér alhálózati hello képes kommunikálni készítése Internet, és csak a bejövő HTTP-kérelmek fogadásához.

a fent említett toodeploy hello forgatókönyv kövesse [erre a hivatkozásra](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG), kattintson a **tooAzure telepítése**, cserélje le a hello alapértelmezett paraméterértékek szükség és hello portal hello utasításait kövesse. Hello vonatkozó példautasításokat hello sablon az alábbi volt használt toodeploy erőforrás csoportnevek **RG-NSG**. 

